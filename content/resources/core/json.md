+++
title = "json resource"
draft = false

platform = "os"

[menu.resources]
    title = "json"
    identifier = "resources/core/json.md json resource"
    parent = "resources/core"
+++

Use the `json` Chef InSpec audit resource to test data in a JSON file.

## Availability

### Install

{{< readfile file="content/reusable/md/inspec_installation.md" >}}

### Version

This resource first became available in v1.0.0 of InSpec.

## Syntax

A `json` resource block declares the data to be tested. Assume the following JSON file:

```json
{
  "name" : "hello",
  "meta" : {
    "creator" : "John Doe"
  },
  "array": [
    "zero",
    "one"
  ]
}
```

This file can be queried using:

```ruby
describe json('/path/to/name.json') do
  its('name') { should eq 'hello' }
  its(['meta','creator']) { should eq 'John Doe' }
  its(['array', 1]) { should eq 'one' }
end
```

where:

- `name` is a configuration setting in a JSON file
- `should eq 'foo'` tests a value of `name` as read from a JSON file versus the value declared in the test

The `json` resource can also be used with JSON formatted output from a command.
Using the same JSON as the previous example, it can be queried using:

```ruby
describe json({ command: 'retrieve_data.py --json'}) do
  its('name') { should eq 'hello' }
  its(['meta','creator']) { should eq 'John Doe' }
  its(['array', 1]) { should eq 'one' }
end
```

Finally, content can be passed directly to the resource:

```ruby
describe json({ content: '{\"item1\": { \"status\": \"available\" } }' }) do
  its(['item1', 'status']) { should cmp 'available' }
end
```

## Examples

The following examples show how to use this Chef InSpec audit resource.

### name

The `name` matcher tests the value of the filename as read from a JSON file versus the value declared in the test:

```ruby
its('name') { should eq '/tmp/example.json' }
```

### Test a cookbook version in a `policyfile.lock.json` file

```ruby
describe json('policyfile.lock.json') do
  its(['cookbook_locks', 'omnibus', 'version']) { should eq('2.2.0') }
end
```

### Test JSON output from an HTTP API

Our example API has a `/health` endpoint, which looks like this:

```json
{
  "service": {
    "port": 3000,
    "status": "ok"
  }
}
```

Example test:

```ruby
describe json(content: http('http://localhost:3000/health').body) do
  its(['service', 'port']) { should eq 3000 }
  its(['service', 'status']) { should eq 'ok' }
end
```

## Matchers

{{< readfile file="content/reusable/md/inspec_matchers_link.md" >}}
