+++
title = "Chef InSpec and Kubernetes"
draft = false
linkTitle = "Kubernetes"
summary = "Resources for auditing Kubernetes"

[menu.resources]
    title = "Kubernetes"
    identifier = "resources/packs/k8s"
    parent = "resources/packs"
+++

The InSpec Kubernetes resource pack provides InSpec helpers to validate an object or resource inside Kubernetes.

## Requirements

- Chef Inspec 3.7+ or 4.x+
- The InSpec [train-kubernetes](https://github.com/inspec/train-kubernetes) plugin is installed.

## Add to an Inspec profile

To use the `inspec-k8s` resources in a profile, follow these steps:

1. Define the `inspec-k8s` resource pack as a dependency in the `inspec.yml` file:

    ```yaml
    supports:
      platform: k8s
    depends:
      - name: inspec-k8s
        url: https://github.com/inspec/inspec-k8s/archive/main.tar.gz
    ```

1. Define your controls. See the examples below.

1. Set your `KUBECONFIG` environment variable or `~/.kube/config` to a valid Kubernetes config that points to the target cluster with valid credentials.

1. Execute the profile from the profile root:

    ```shell
    inspec exec . -t k8s://
    ```

See the [sample inspec-k8s profile](https://github.com/inspec/inspec-k8s-sample).

## Example controls

The following InSpec base resources are implemented:

- k8sobjects
- k8sobject

You can list and filter objects:

```ruby
describe k8sobjects(api: 'v1', type: 'pods', namespace: 'default', labelSelector: 'run=nginx') do
  it { should exist }
  ...
end
```

```ruby
describe k8sobjects(api: 'v1', type: 'namespaces', labelSelector: 'myns=prod') do
  it { should exist }
  ...
end
```

And then assess the spec of a specific object:

```ruby
describe k8sobject(api: 'v1', type: 'pod', namespace: 'default', name: 'my-pod') do
  it { should exist }
  its('name') { should eq 'my-pod' }
  ...
end
```

You can test the properties of files within in a pod or container.
This is only supported Linux-based containers.

```ruby
describe k8s_exec_file(path: 'FULLY_QUALIFIED_PATH', pod: 'POD_NAME', namespace: 'NAMESPACE_NAME') do
  it { should exist }
  it { should be_file }
  it { should be_readable }
  it { should be_writable }
  it { should be_executable.by_user('root') }
  it { should be_owned_by 'root' }
  its('mode') { should cmp '0644' }
end
```

## Troubleshooting

If you run into issues installing the `train-kubernetes` plugin with `inspec plugin install train-kubernetes`, try the following:

- Run `gem install train-kubernetes` before `inspec plugin install train-kubernetes`.
- Verify the `~/.inspec/plugins.json` has `"0.1.3"` and not `"= 0.1.3"` for the `version` value. Update it if needed.
- Verify you can cleanly install the `k8s-client` gem version `0.10.4` or greater.  e.g. `gem install k8s-client -v 0.10.4`
- Verify that only one version of the `excon` gem is installed. For example, `gem list | grep excon`. If you see two versions, remove the older version with `gem uninstall excon`.
