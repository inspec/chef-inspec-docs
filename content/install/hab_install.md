+++
title = "Install Chef InSpec from a Habitat package"
draft = false

[menu.install]
    title = "Install"
    identifier = "install/install_hab"
    parent = "install"
    weight = 20
+++

To install Chef InSpec from a Habitat package, you first install Habitat CLI, then install the Chef InSpec package.

## Install Habitat CLI

### On Linux

Use the official installation script:

```sh
curl https://raw.githubusercontent.com/habitat-sh/habitat/main/components/hab/install.sh | sudo bash
```

For more installation options, see the [Habitat installation guide](https://docs.chef.io/habitat/install_habitat/).

### On Windows

Open **PowerShell** as an administrator and run:

```ps1
Set-ExecutionPolicy Bypass -Scope Process -Force
iwr https://raw.githubusercontent.com/habitat-sh/habitat/main/components/hab/install.ps1 -UseBasicParsing | iex
```

For more installation options, see the [Windows installation guide](https://docs.chef.io/habitat/install_habitat/#install-chef-habitat-using-a-powershell-install-script).

### Verify Habitat installation

```sh
hab --version
```

Example output:

```sh
hab 1.x.x/2025xxxx
```

## Set authentication token

Habitat requires the `HAB_AUTH_TOKEN` environment variable to authenticate with private Builder channels like **base-2025**.

Set your authentication token:

```sh
export HAB_AUTH_TOKEN=<HABITAT_AUTH_TOKEN>
```

Replace `<HABITAT_AUTH_TOKEN>` with your token from your Chef Habitat Builder account.

For more information, see the [Habitat CLI Setup Documentation](https://docs.chef.io/habitat/hab_setup/).

## Install Chef InSpec package

Install the Chef InSpec Habitat package:

```sh
sudo hab pkg install chef/inspec --channel base-2025 --binlink
```

### Command parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `--channel` | The release channel | `base-2025` |
| `--binlink` or `-b` | Links the package binary to `/bin` for system-wide access | |

During installation, you'll see output similar to:

```sh
» Installing chef/inspec/7.0.84/20251008090638
↓ Downloading chef/inspec/7.0.84/20251008090638 for x86_64-linux 38.40 MB / 38.40 MB
☛ Verifying chef/inspec/7.0.84/20251008090638
✓ Installed chef/inspec/7.0.84/20251008090638
★ Install of chef/inspec/7.0.84/20251008090638 complete with 40 new packages installed.
» Binlinking inspec from chef/inspec/7.0.84/20251008090638 into /bin
★ Binlinked inspec from chef/inspec/7.0.84/20251008090638 to /bin/inspec
```

## Verify installation

### List installed packages

```sh
hab pkg list | grep inspec
```

### Check version with Habitat CLI

```sh
hab pkg exec chef/inspec inspec version
```

### Check version with binlinked command

If you used `--binlink` during installation, run:

```sh
inspec version
```

## Manage packages

### Upgrade to newer version

To upgrade to a newer version in the same channel, re-run the install command with `--force`:

```sh
sudo hab pkg install chef/inspec --channel base-2025 --binlink --force
```

### Uninstall package

```sh
sudo hab pkg uninstall chef/inspec
```

## Troubleshooting

### Authentication errors

While installing Chef InSpec, if you see an error like this:

```sh
✗✗✗ When applicable, we try once, then re-attempt 5 times to
✗✗✗ download a package. Unfortunately, we failed to download
✗✗✗ chef/inspec/7.*.**/2025****** for x86_64-linux.
✗✗✗ Last error: [401 Unauthorized] Please check that you have specified a valid Personal Access Token.
```

This indicates that Habitat CLI can't authenticate with Builder due to a missing or invalid personal access token.

To resolve this issue, pass the authentication token explicitly:

```sh
sudo hab pkg install chef/inspec --channel base-2025 --binlink --auth $HAB_AUTH_TOKEN
```