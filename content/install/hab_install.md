+++
title = "Install Chef InSpec from a Habitat package"
draft = false

[menu.install]
    title = "Install"
    identifier = "install/install_hab"
    parent = "install"
    weight = 20
+++


To install Chef InSpec from a Habitat package, follow these steps:

## 1. Install Habitat CLI

### 1.1 Install Habitat CLI on Linux

### Using the Official Installation Script

```sh
curl https://raw.githubusercontent.com/habitat-sh/habitat/main/components/hab/install.sh | sudo bash
```

Alternatively, follow the official [Habitat installation guide](https://docs.chef.io/habitat/install_habitat/).

### 1.2 Install Habitat CLI on Windows

### Using PowerShell (Run as Administrator)

Open a **PowerShell** window and execute:

```ps1
Set-ExecutionPolicy Bypass -Scope Process -Force iwr https://raw.githubusercontent.com/habitat-sh/habitat/main/components/hab/install.ps1 -UseBasicParsing | iex
```

Alternatively, follow the official [Windows installation guide](https://docs.chef.io/habitat/install_habitat/#install-chef-habitat-using-a-powershell-install-script).

### Verify Installation

```sh
hab --version
```

Example output:

```sh
hab 1.x.x/2025xxxx
```

- - -

### **Set the Habitat Auth Token**

Habitat requires the `HAB_AUTH_TOKEN` environment variable to authenticate with private Builder channels like **base-2025**.

```sh
export HAB_AUTH_TOKEN=<your_habitat_auth_token>
```

Replace `<your_habitat_auth_token>` with the token from your Habitat Builder account.

Alternatively follow [Habitat CLI Setup Documentation](https://docs.chef.io/habitat/hab_setup/ "https://docs.chef.io/habitat/hab_setup/").

## 2. Install the Desired hab Package

Once Habitat is installed, you can install any Habitat package using:

```sh
sudo hab pkg install <origin>/<package> --channel <channel> --binlink --force
```

### Parameters:

|     |     |     |
| --- | --- | --- |
| Parameter | Description | Example |
| `<origin>` | The package origin (publisher). | `chef` |
| `<package>` | The package name. | `inspec` |
| `<channel>` | The release channel. | `base-2025` |
| `--binlink`or `-b` | (Optional) Links the package binary into `/bin` for system-wide access. |     |

**Example for InSpec 7:**

```sh
sudo hab pkg install chef/inspec --channel base-2025 --binlink
```

During installation, you will see output similar to:

```sh
» Installing chef/inspec/7.0.84/20251008090638
↓ Downloading chef/inspec/7.0.84/20251008090638 for x86_64-linux 38.40 MB / 38.40 MB \ [====================================================================================================================================================================================================] 100.00 % 91.30 MB/s
☛ Verifying chef/inspec/7.0.84/20251008090638
☛ Verifying core/git/2.49.0/20250627165352
☛ Verifying core/ruby3_4/3.4.2/20250627165939
☛ Verifying core/curl/8.14.1/20250627154406
✓ Installed core/curl/8.14.1/20250627154406
✓ Installed core/ruby3_4/3.4.2/20250627165939
✓ Installed core/git/2.49.0/20250627165352
✓ Installed chef/inspec/7.0.84/20251008090638
★ Install of chef/inspec/7.0.84/20251008090638 complete with 40 new packages installed.
» Binlinking inspec from chef/inspec/7.0.84/20251008090638 into /bin
★ Binlinked inspec from chef/inspec/7.0.84/20251008090638 to /bin/inspec
```


## 3. Verify the Installation

## List Installed InSpec Packages

```sh
hab pkg list | grep inspec
```

## Check Version via Habitat CLI

```sh
hab pkg exec chef/inspec inspec version
```

## Check Version via Binlinked Command

If you used `--binlink` during installation, you can simply run:

`inspec version`

## 4. Additional Notes

*   The `--binlink` flag makes InSpec accessible globally (e.g., `/bin/inspec`).
    
*   To upgrade to a newer version in the same channel, re-run the install command with `--force`.
    

## Uninstall Hab Package

To Uninstall the hab package use the command

```sh
sudo hab pkg uninstall chef/inspec
```


## Troubleshooting

If you encounter an error similar to the following:

```sh
✗✗✗
✗✗✗ When applicable, we try once, then re-attempt 5 times to
✗✗✗ download a package. Unfortunately, we failed to download
✗✗✗ chef/inspec/7.*.**/2025****** for x86_64-linux.
✗✗✗ Last error: [401 Unauthorized] Please check that you have specified a valid Personal Access Token.
```

This indicates that the **Habitat CLI** was unable to authenticate with **Builder**, usually due to a missing or invalid **Personal Access Token (HAB\_AUTH\_TOKEN)**.

Pass the authentication token explicitly during the command:

```sh
sudo hab pkg install chef/inspec --channel base-2025 --binlink --auth $HAB_AUTH_TOKEN
```