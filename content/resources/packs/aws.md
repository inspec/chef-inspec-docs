+++
title = "Chef InSpec and AWS"
draft = false
linkTitle = "AWS"
summary = "Resources for auditing AWS"

[menu.resources]
    title = "AWS"
    identifier = "resources/packs/aws"
    parent = "resources/packs"
+++

Chef InSpec has resources for auditing AWS.

## Initialize an InSpec profile for auditing AWS

With Chef InSpec 4 or greater, you can create a profile for auditing AWS resources with `inspec init profile`:

```bash
$ inspec init profile --platform aws <PROFILE_NAME>
Create new profile at /Users/me/<PROFILE_NAME>
 * Creating directory libraries
 * Creating file README.md
 * Creating directory controls
 * Creating file controls/example.rb
 * Creating file inspec.yml
 * Creating file inputs.yml
 * Creating file libraries/.gitkeep
```

Assuming the `inputs.yml` file contains your AWS project ID, you can execute this sample profile using the following command:

```bash
inspec exec <PROFILE_NAME> --input-file=<PROFILE_NAME>/inputs.yml -t gcp://
```

## Set AWS credentials

Chef InSpec uses the standard AWS authentication mechanisms. Typically, you will create an IAM user specifically for auditing activities.

1. Create an IAM user in the AWS console, with your choice of username. Check the box marked "Programmatic Access."

1. On the Permissions screen, choose Direct Attach. Select the AWS-managed IAM profile named "ReadOnlyAccess." If you wish to restrict the user further, you may do so; see individual Chef InSpec resources to identify which permissions are required.

1. After generating the key, record the access key ID and secret key.

### Provide credentials with environment variables

You may provide the credentials to Chef InSpec by setting the following environment variables: `AWS_REGION`, `AWS_ACCESS_KEY_ID`, and `AWS_SECRET_ACCESS_KEY`. You may also use `AWS_PROFILE`, or if you are using MFA, `AWS_SESSION_TOKEN`. See the [AWS Command Line Interface Docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for details.

Once you have your environment variables set, you can verify your credentials by running:

```bash
$ inspec detect -t aws://

== Platform Details
Name:      aws
Families:  cloud, api
Release:   aws-sdk-v2.10.125
```

### Provide credentials using Chef InSpec target option

Look for a file in your home directory named `~/.aws/credentials`. If it doesn't exist, create it. Choose a name for your profile; here, we're using the name 'auditing'. Add your credentials as a new profile, in INI format:

```bash
[auditing]
aws_access_key_id = AKIA....
aws_secret_access_key = 1234....abcd
```

You may now run Chef InSpec using the `--target` / `-t` option, using the format `-t aws://region/profile`. For example, to connect to the Ohio region using a profile named 'auditing', use `-t aws://us-east-2/auditing`.

To verify your credentials, run:

```bash
$ inspec detect -t aws://

== Platform Details
Name:      aws
Families:  cloud, api
Release:   aws-sdk-v2.10.125
```

-----------------------------------

This InSpec resource pack uses the native Google Cloud Platform (GCP) support in InSpec and provides the required resources to write tests for GCP.

## Prerequisites

### Install and configure the Google Cloud SDK

1. Download and install the [Google Cloud SDK](https://cloud.google.com/sdk/docs/):

    ```sh
    ./google-cloud-sdk/install.sh
    ```

1. Create credentials a file:

    ```bash
    gcloud auth application-default login
    ```

    If successful, this should be similar to:

    ```bash
    $ cat ~/.config/gcloud/application_default_credentials.json

    {
      "client_id": "764086051850-6qr4p6gpi6hn50asdr.apps.googleusercontent.com",
      "client_secret": "d-fasdfasdfasdfaweroi23jknrmfs;f8sh",
      "refresh_token": "1/asdfjlklwna;ldkna'dfmk-lCkju3-yQmjr20xVZonrfkE48L",
      "type": "authorized_user"
    }
    ```

While InSpec can use user accounts for authentication, [Google recommends using service accounts](https://cloud.google.com/docs/authentication/).

### Create a service account

1. In Google Cloud, go to **Console**.
1. Select **Service Accounts**.
1. Select **Create Service Accounts** and give the required details.
1. Select **Done**.
1. Now Select **Keys** Tab.
1. Select **Add Key**.
1. Select **Create New Key**.
1. Set the key type to **JSON**.
1. Select **Create**.
1. The Service Account Key will be downloaded. For example: `project-name-1-feb7993e8660.json-project-name-1-feb7993e8660.json`
1. Move the key to the `~/.config/gcloud` folder. If the file gets downloaded in the `Downloads` folder, use the below command.

    ```bash
    mv ~/Downloads/project-name-1-feb7993e8660.json ~/.config/gcloud/
    ```

1. The JSON credentials file for a service account looks like this:

    ```bash
    $ cat ~/.config/gcloud/project-name-1-feb7993e8660.json
    {
      "type": "service_account",
      "project_id": "project-name-1",
      "private_key_id": "eb45b2fc0c33ea9b6fa212aaa08b1ed814bf8660",
      "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvwIBADAN3662...fke9n6LAf268E/4EWhIzg==\n-----END PRIVATE KEY-----\n",
      "client_email": "auto-testing@project-name-1.iam.gserviceaccount.com",
      "client_id": "112144174133171863632",
      "auth_uri": "https://accounts.google.com/o/oauth2/auth",
      "token_uri": "https://oauth2.googleapis.com/token",
      "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
      "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/auto-testing%40project-name-1.iam.gserviceaccount.com"
    }
    ```

1. Now set up the environmental variable for the 'GOOGLE_APPLICATION_CREDENTIALS'. And InSpec can be instructed to use it by setting this ENV variable prior to running `inspec exec`:

    ```bash
    export GOOGLE_APPLICATION_CREDENTIALS='~/.config/gcloud/project-name-1-feb7993e8660.json'
    ```

If you get an error, check your IAM roles and permissions.

### Enable AWS APIs

Enable the appropriate APIs that you want to use:

- [Enable Compute Engine API](https://console.cloud.google.com/apis/library/compute.googleapis.com/)
- [Enable Kubernetes Engine API](https://console.cloud.google.com/apis/library/container.googleapis.com)

## Use the resources

Since this is an InSpec resource pack, it only defines InSpec resources. It includes example tests only. To easily use the GCP resources in your tests do the following:

### Create an InSpec profile for GCP

1. Create a new InSpec profile:

    ```bash
    $ inspec init profile --platform gcp gcp-profile-name

    Create new profile at /Users/spaterson/gcp-profile-name
    * Create directory libraries
    * Create file README.md
    * Create directory controls
    * Create file controls/example.rb
    * Create file inspec.yml
    * Create file attributes.yml
    * Create file libraries/.gitkeep
    ```

    This generates a new directory with an InSpec GCP profile.

1. Update the `inputs.yml` to point to your project

   ```yml
   gcp_project_id: 'your-gcp-project'
   ```

   Replace `your-gcp-project` with the Project ID from your GCP JSON credentials file.

1. The generated `inspec.yml` file automatically points to the InSpec GCP resource pack:

    ```yaml
    name: gcp-profile-name
    title: GCP InSpec Profile
    maintainer: The Authors
    copyright: The Authors
    copyright_email: you@example.com
    license: Apache-2.0
    summary: An InSpec Compliance Profile For GCP
    version: 0.1.0
    inspec_version: '>= 4'
    inputs:
    - name: gcp_project_id
      required: true
      description: 'The GCP project identifier.'
    depends:
    - name: inspec-gcp
      url: https://github.com/inspec/inspec-gcp/archive/main.tar.gz
    supports:
    - platform: gcp
    ```

    You can set a particular version of the InSpec GCP resources. For available inspec-gcp versions, see this list of [inspec-gcp tagged versions](https://github.com/inspec/inspec-gcp/tags) or [inspec-gcp release versions](https://github.com/inspec/inspec-gcp/releases).

## Run the tests

```bash
$ cd my-gcp-profile/
$ inspec exec . -t gcp:// --input-file inputs.yml

Profile: GCP InSpec Profile (my-profile)
Version: 0.1.0
Target:  gcp://local-service-account@my-gcp-project.iam.gserviceaccount.com
Target ID: 8123456-12a3-1234-123a-a12s5c5abcx1

  ✔  gcp-single-region-1.0: Ensure single region has the correct properties.
     ✔  Region europe-west2 zone_names should include "europe-west2-a"
  ✔  gcp-regions-loop-1.0: Ensure regions have the correct properties in bulk.
     ✔  Region asia-east1 is expected to be up
     ✔  Region asia-east2 is expected to be up
     ✔  Region asia-northeast1 is expected to be up
     ✔  Region asia-northeast2 is expected to be up
     ✔  Region asia-northeast3 is expected to be up
     ✔  Region asia-south1 is expected to be up
     ✔  Region asia-south2 is expected to be up
     ✔  Region asia-southeast1 is expected to be up
     ✔  Region asia-southeast2 is expected to be up
     ✔  Region australia-southeast1 is expected to be up
     ✔  Region australia-southeast2 is expected to be up
     ✔  Region europe-central2 is expected to be up
     ✔  Region europe-north1 is expected to be up
     ✔  Region europe-southwest1 is expected to be up
     ✔  Region europe-west1 is expected to be up
     ✔  Region europe-west2 is expected to be up
     ✔  Region europe-west3 is expected to be up
     ✔  Region europe-west4 is expected to be up
     ✔  Region europe-west6 is expected to be up
     ✔  Region europe-west8 is expected to be up
     ✔  Region europe-west9 is expected to be up
     ✔  Region me-west1 is expected to be up
     ✔  Region northamerica-northeast1 is expected to be up
     ✔  Region northamerica-northeast2 is expected to be up
     ✔  Region southamerica-east1 is expected to be up
     ✔  Region southamerica-west1 is expected to be up
     ✔  Region us-central1 is expected to be up
     ✔  Region us-east1 is expected to be up
     ✔  Region us-east4 is expected to be up
     ✔  Region us-east5 is expected to be up
     ✔  Region us-south1 is expected to be up
     ✔  Region us-west1 is expected to be up
     ✔  Region us-west2 is expected to be up
     ✔  Region us-west3 is expected to be up
     ✔  Region us-west4 is expected to be up


Profile:   Google Cloud Platform Resource Pack (inspec-gcp)
Version:   1.10.37
Target:    gcp://local-service-account@my-gcp-project.iam.gserviceaccount.com
Target ID: 8123456-12a3-1234-123a-a12s5c5abcx1

     No tests executed.

Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary:  36 successful, 0 failures, 0 skipped
```
