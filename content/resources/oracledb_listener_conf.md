+++
title = "oracledb_listener_conf resource"
draft = false

platform = "os"

[menu.resources]
    title = "oracledb_listener_conf"
    identifier = "resources/os/oracledb_listener_conf.md oracledb_listener_conf resource"
    parent = "resources/os"
+++

Use the `oracledb_listener_conf` Chef InSpec audit resource to test the listeners settings of Oracle DB, typically located at `$ORACLE_HOME/network/admin/listener.ora` or `$ORACLE_HOME\network\admin\listener.ora` depending upon the platform.

## Installation

{{< readfile file="content/reusable/md/inspec_installation.md" >}}

## Requirements

- You must have sufficient permission to access listener settings defined in `listener.ora` file.
- Value for environment variable `ORACLE_HOME` should be set in the system.

## Syntax

A `oracledb_listener_conf` resource block fetches listeners settings in the `listener.ora` file, and then compares them with the value stated in the test:

    describe oracledb_listener_conf do
      its('config item') { should eq 'value' }
    end

## Examples

The following examples show how to use this Chef InSpec audit resource.

### Test parameters set within the listener file

    describe oracledb_listener_conf do
      its('DEFAULT_SERVICE_LISTENER') { should eq 'XE' }
      its('EM_EXPRESS_PORT') { should eq '5500' }
    end

## Matchers

{{< readfile file="content/reusable/md/inspec_matchers_link.md" >}}
