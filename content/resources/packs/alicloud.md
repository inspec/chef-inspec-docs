+++
title = "Chef InSpec and Alibaba Cloud"
draft = false
linkTitle = "Alibaba Cloud"
summary = "Resources for auditing Alibaba Cloud"


[menu.resources]
    title = "AliCloud"
    identifier = "resources/packs/alibaba"
    parent = "resources/packs"
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
