+++
title = "Chef InSpec and GCP"
draft = false
linkTitle = "GCP"
summary = "Resources for auditing GCP"

[menu.resources]
    title = "GCP"
    identifier = "resources/packs/gcp"
    parent = "resources/packs"
+++

Chef InSpec has resources for auditing GCP.

## Initialize an InSpec profile for auditing GCP

With Chef InSpec 4 or greater, you can create a profile for testing GCP resources with `inspec init profile`:

```bash
$ inspec init profile --platform gcp my-profile
Create new profile at /Users/me/my-profile
 * Creating directory libraries
 * Creating file README.md
 * Creating directory controls
 * Creating file controls/example.rb
 * Creating file inspec.yml
 * Creating file inputs.yml
 * Creating file libraries/.gitkeep
```

Assuming the `inputs.yml` file contains your GCP project ID, this sample profile can then be executed using the following command:

```bash
inspec exec my-profile --input-file=my-profile/inputs.yml -t gcp://
```

## Set GCP credentials

To use Chef InSpec GCP resources, you will need to install and configure the Google Cloud SDK.
Instructions for this pre-requisite can be found in the [Google CLoud SDK documentation](https://cloud.google.com/sdk/docs/).

### Set the GCP credentials file

While InSpec can use user accounts for authentication, [Google Cloud documentation](https://cloud.google.com/docs/authentication/) recommends using service accounts.

1. Create a [service account](https://cloud.google.com/docs/authentication/getting-started) with the scopes appropriate for your needs.

1. Download the credential JSON file, for example `project-credentials.json`, to your workspace and activate your service account with `gcloud auth activate-service-account`.

    ```bash
    gcloud auth activate-service-account --key-file project-credentials.json
    ```

### Provide credentials using environment variables

You may also set the GCP credentials json file using the `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

```bash
export GOOGLE_APPLICATION_CREDENTIALS='/Users/me/.config/gcloud/project-name-1-feb7993e8660.json'
```

Once you have your environment variables set, you can verify your credentials by running:

```bash
$ inspec detect -t gcp://

== Platform Details

Name:      gcp
Families:  cloud, api
Release:   google-cloud-v
```
