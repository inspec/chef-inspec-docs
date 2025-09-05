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

The InSpec Kubernetes resource pack lets you validate objects and resources inside Kubernetes clusters.

## Requirements

- Chef InSpec version 3.7 or later
- The InSpec [train-kubernetes](https://github.com/inspec/train-kubernetes) plugin

## Add to an InSpec profile

To use the `inspec-k8s` resources in a profile, follow these steps:

1. In your `inspec.yml` file, add the resource pack as a dependency:

    ```yaml
    supports:
      platform: k8s
    depends:
      - name: inspec-k8s
        url: https://github.com/inspec/inspec-k8s/archive/main.tar.gz
    ```

1. Add your controls to the profile. See the examples in the next section.

1. Set your `KUBECONFIG` environment variable or `~/.kube/config` to a valid Kubernetes config that points to your target cluster and includes valid credentials.

1. In the profile root directory, run:

    ```shell
    inspec exec . -t k8s://
    ```

For a complete example, see the [sample inspec-k8s profile](https://github.com/inspec/inspec-k8s-sample).

## Example controls

The following InSpec base resources are available:

- `k8sobjects`: Lists and filters Kubernetes objects.
- `k8sobject`: Assesses a specific Kubernetes object.

To list and filter pods in the default namespace with a specific label:

```ruby
describe k8sobjects(api: 'v1', type: 'pods', namespace: 'default', labelSelector: 'run=nginx') do
  it { should exist }
  ...
end
```

To list and filter namespaces with a label:

```ruby
describe k8sobjects(api: 'v1', type: 'namespaces', labelSelector: 'myns=prod') do
  it { should exist }
  ...
end
```

To assess the spec of a specific pod:

```ruby
describe k8sobject(api: 'v1', type: 'pod', namespace: 'default', name: 'my-pod') do
  it { should exist }
  its('name') { should eq 'my-pod' }
  ...
end
```

To test the properties of a file inside a Linux-based pod or container:

```ruby
describe k8s_exec_file(path: '/etc/config/settings.yaml', pod: 'nginx-pod', namespace: 'default') do
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

If you have issues installing the `train-kubernetes` plugin with `inspec plugin install train-kubernetes`, try these steps:

- Run `gem install train-kubernetes` before running `inspec plugin install train-kubernetes`.
- In `~/.inspec/plugins.json`, make sure the `version` value is `"0.1.3"` (not `"= 0.1.3"`). Update it if needed.
- Install the `k8s-client` gem version `0.10.4` or later: `gem install k8s-client -v 0.10.4`.
- Make sure only one version of the `excon` gem is installed. Run `gem list | grep excon` and remove older versions with `gem uninstall excon`.
