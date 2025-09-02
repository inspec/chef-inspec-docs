+++
title = "Chef InSpec and Alibaba Cloud"
draft = false
linkTitle = "Alibaba Cloud"


[menu.cloud]
    title = "AliCloud"
    identifier = "cloud/alibaba"
    parent = "cloud"
+++

Chef InSpec has resources for auditing Alibaba.

You will need to install AliCloud SDK version 0.8.0 and require AliCloud credentials to use the Chef InSpec AliCloud resources.

## Set AliCloud credentials

You can configure AliCloud credentials in an [.envrc file](https://github.com/inspec/inspec-alicloud/blob/main/.envrc_example) or export them in your shell.

```bash
# Example configuration
export ALICLOUD_ACCESS_KEY="anaccesskey"
export ALICLOUD_SECRET_KEY="asecretkey"
export ALICLOUD_REGION="eu-west-1"
```

## Alibaba resources

{{< inspec_resources platform="alicloud" >}}
