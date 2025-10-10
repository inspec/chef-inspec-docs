+++
title = "License Chef InSpec"
draft = false

[menu.install]
    title = "License"
    identifier = "install/license"
    parent = "install"
    weight = 30
+++

To use Chef InSpec 7, you need to [accept the Chef EULA](#accept-the-chef-eula). Depending on the InSpec distribution you install, you may also need a [license](#license-key):

- Official distributions, such as the Chef Habitat package for InSpec 7, don't require a license.
- If you install InSpec through an undistributed package, such as the inspec gem, you must add a license.

You can't manually enable or disable licensing. The licensing requirements are determined by the distribution.

If you'd like to try out Chef InSpec, you can [request a trial license](https://www.chef.io/licensing/inspec/license-generation-free-trial).

For more information, see [Chef's licensing documentation](https://docs.chef.io/licensing/).

## Accept the Chef EULA

You must accept the [Chef End User License Agreement (EULA)](https://www.chef.io/end-user-license-agreement) before you run Chef InSpec. You can accept the EULA in two ways:

- [Use a command line option](#use-a-command-line-option)
- [Set an environment variable](#set-an-environment-variable)

If you don't set either, Chef InSpec prompts you to accept the EULA interactively. If the prompt can't be displayed, Chef InSpec exits with code 172.

If Chef InSpec can't persist the accepted license, it sends a message to STDOUT and continues to run. You'll need to accept the license again the next time you run Chef InSpec.

### Use a command line option

Add the `--chef-license <value>` argument to accept the Chef EULA.

```sh
inspec exec <PROFILE_NAME> --chef-license <value>
```

Replace `<value>` with one of these options:

`accept`
: Accepts the license and tries to persist a marker file locally. Future runs won't require accepting the license again.

`accept-silent`
: Similar to `accept`, but it doesn't send messaging to STDOUT.

`accept-no-persist`
: Similar to `accept-silent`, but doesn't persist a marker file. You'll need to accept the license again for future runs.

### Set an environment variable

Set the `CHEF_LICENSE="<value>"` environment variable to accept the Chef EULA.

```sh
export CHEF_LICENSE="<value>"
inspec exec <PROFILE_NAME>
```

Replace `<value>` with one of these options:

`accept`
: Accepts the license and tries to persist a marker file locally. Future runs won't require accepting the license again.

`accept-silent`
: Similar to `accept`, but it doesn't send messaging to STDOUT.

`accept-no-persist`
: Similar to `accept-silent`, but doesn't persist a marker file. You'll need to accept the license again for future runs.

## License key

Chef InSpec accepts a license key in three ways:

- [Use the interactive license dialog](#use-the-interactive-license-dialog)
- [command line option](#use-a-command-line-option-1)
- [environment variable](#set-an-environment-variable-1)

{{< note >}}
If you're a commercial customer, you can use an asset serial number from the [Progress support portal](https://community.progress.com/s/products/chef) as your license key.
{{< /note >}}

### Use the interactive license dialog

The easiest way to provide a license key is to run Chef InSpec.
If no license key is set and it doesn't detect an automated method of setting a license key, Chef InSpec starts an interactive licensing dialog.

To add a license with the interactive license dialog, follow these steps:

1. Run a top-level command, such as `inspec shell`.
1. At the prompt, select **I already have a license ID**.

1. Enter your license key at the next prompt.

Chef InSpec validates your license key, displays license details, runs `inspec shell`, and stores the key for future use.

### Use a command line option

Set the license key with the `--chef-license-key` option:

```sh
inspec exec <PROFILE_NAME> --chef-license-key <LICENSE_KEY>
```

Most of the main Chef InSpec CLI commands accept this argument, however some plugins don't support the flag.

### Set an environment variable

Set the license key with the `CHEF_LICENSE_KEY` environment variable:

```sh
export CHEF_LICENSE_KEY=<LICENSE_KEY>
inspec exec <PROFILE_NAME>
```

## Use Chef Local License Service

For large or air-gapped fleets, you can retrieve a license key from a [Chef Local License Service](https://docs.chef.io/licensing/local_license_service/). With this service, you only need the service URLs.

Chef InSpec requests license keys from the Local License Service and uses them during execution. Chef InSpec doesn't store these keys long-term.

You can set a Local License Service URL in two ways:

- [Use a command line option](#use-a-command-line-option-2)
- [Set an environment variable](#set-an-environment-variable-2)

### Use a command line option

Set the Chef Local License Service URL with the `--chef-license-server` option:

```sh
inspec exec <PROFILE_NAME> --chef-license-server https://license-server.example.com
```

### Set an environment variable

Set the Chef Local License Service URL with the `CHEF_LICENSE_SERVER` environment variable:

```sh
export CHEF_LICENSE_SERVER=https://license-server.example.com
inspec exec <PROFILE_NAME>
```

#### Use multiple license servers

You can set up to five Chef Local License Service URLs as a comma-separated list. Chef InSpec tries each URL and uses the first one that works.

```sh
export CHEF_LICENSE_SERVER=https://license-server-01.example.com,https://license-server-02.example.com
inspec exec <PROFILE_NAME>
```

Make sure you synchronize your license servers to avoid inconsistent results.

## Licensing Telemetry service

The Chef Licensing Telemetry service gathers product activation, usage statistics, environment information, bugs, and other data related to Chef InSpec.

This feature is enabled for free and trial tiers only. It's not enabled for commercial users.

For more information, see the [Progress Privacy Policy](https://www.progress.com/legal/privacy-policy).
