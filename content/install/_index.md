+++
title = "Install Chef InSpec"
draft = false

[menu.install]
    title = "Install Chef InSpec"
    identifier = "install/install"
    parent = "install"
    weight = 10
+++

<!-- cSpell:ignore -->

Chef InSpec 7 installers are available for Windows, Debian, and RPM-based Linux distributions.
You can download and install pre-built `.msi`, `.deb`, or `.rpm` packages using your existing package management tools.

## Supported platforms

Chef InSpec is supported on the following platforms:

- Windows x86-64
- Linux x86-64

## Prerequisites

This installation process has the following prerequisites:

- Chef InSpec, Chef Automate, and Chef Workstation aren't installed on the target system.
- On Windows systems, `tar` is installed.
- On Debian-based systems, the `dpkg` package manager is installed.
- On RPM-based systems, `rpm` and either `dnf` or `yum` are installed. For Amazon Linux 2, use `rpm` and `yum`.
- You have a valid Progress Chef license key.
- The target system is connected to the internet.

## Uninstall an older version of Chef InSpec

If an older version of Chef InSpec is already installed, uninstall it before installing a newer version to avoid package conflicts.

### Uninstall on Debian-based distributions

1. Remove the package:

   ```sh
   sudo apt-get remove inspec
   ```

1. Verify that the package has been removed:

   ```sh
   dpkg -l inspec
   ```

   The command returns no output if the package is removed successfully.

### Uninstall on RPM-based distributions

1. Remove the package.

   Using `dnf`:

   ```sh
   sudo dnf remove inspec
   ```

   For Amazon Linux 2 or systems using `yum`:

   ```sh
   sudo yum remove inspec
   ```

1. Verify that the package has been removed:

   ```sh
   rpm -qa inspec
   ```

   The command returns no output if the package is removed successfully.

### Uninstall on Windows

To uninstall using the Windows UI:

1. Open **Settings > Apps > Installed apps**.
1. Search for **Chef InSpec**.
1. Select **Uninstall**, then follow the on-screen prompts.

To uninstall from the command line, run the following command in an elevated PowerShell or Command Prompt session:

```powershell
msiexec /x inspec-enterprise-<VERSION>-x64.msi /qn
```

Replace `<VERSION>` with the version number of the currently installed package.

## Install Chef InSpec

To install Chef InSpec, follow these steps:

### Download the Chef InSpec installer

Download the installer package from the [Chef downloads page](https://www.chef.io/downloads).
Select **Chef InSpec**, then choose the target platform and version.

To download programmatically, use the following commands.
Replace `<VERSION>` with the version number to install and `<LICENSE_ID>` with your Chef license ID.

#### Download the Debian-based installer

Using `wget`:

```sh
wget -O "inspec-enterprise-<VERSION>-linux.deb" "https://chefdownload-commercial.chef.io/stable/inspec/download?eol=false&license_id=<LICENSE_ID>&m=x86_64&p=linux&pm=deb&v=<VERSION>"
```

Using `curl`:

```sh
curl -o "inspec-enterprise-<VERSION>-linux.deb" "https://chefdownload-commercial.chef.io/stable/inspec/download?eol=false&license_id=<LICENSE_ID>&m=x86_64&p=linux&pm=deb&v=<VERSION>"
```

#### Download the RPM-based installer

Using `wget`:

```sh
wget -O "inspec-enterprise-<VERSION>-linux.rpm" "https://chefdownload-commercial.chef.io/stable/inspec/download?eol=false&license_id=<LICENSE_ID>&m=x86_64&p=linux&pm=rpm&v=<VERSION>"
```

Using `curl`:

```sh
curl -o "inspec-enterprise-<VERSION>-linux.rpm" "https://chefdownload-commercial.chef.io/stable/inspec/download?eol=false&license_id=<LICENSE_ID>&m=x86_64&p=linux&pm=rpm&v=<VERSION>"
```

#### Download the Windows installer

Run the following command in an elevated PowerShell session:

```powershell
Invoke-WebRequest -Uri "https://chefdownload-commercial.chef.io/stable/inspec/download?eol=false&license_id=<LICENSE_ID>&m=x86_64&p=windows&pm=msi&v=<VERSION>" -OutFile "inspec-enterprise-<VERSION>-windows.msi"
```

### Install the package

Go to the directory containing the downloaded installer and run the appropriate install command for your platform.

#### Install on Debian-based distributions

```sh
sudo dpkg -i inspec-enterprise-<VERSION>_amd64.deb
```

Replace `<VERSION>` with the version number of the downloaded package, for example:

```sh
sudo dpkg -i inspec-enterprise-7.6.0-1_amd64.deb
```

#### Install on RPM-based distributions

Using `rpm`:

```sh
sudo rpm -Uvh inspec-enterprise-<VERSION>.x86_64.rpm
```

Using `dnf`:

```sh
sudo dnf install ./inspec-enterprise-<VERSION>.x86_64.rpm
```

For Amazon Linux 2 or systems using `yum`:

```sh
sudo yum install ./inspec-enterprise-<VERSION>.x86_64.rpm
```

Replace `<VERSION>` with the version number of the downloaded package, for example `inspec-enterprise-7.6.0-1.el8.x86_64.rpm`.

#### Install on Windows

Run the following command in an elevated PowerShell or Command Prompt session:

```powershell
msiexec /i inspec-enterprise-<VERSION>-x64.msi /qn
```

Replace `<VERSION>` with the version number of the downloaded package, for example `inspec-enterprise-7.6.0-x64.msi`.

Alternatively, double-click the `.msi` file and follow the on-screen installation wizard.

### Verify the installation

```sh
inspec version
```

The output displays the installed version of Chef InSpec.

## Upgrade Chef InSpec

To upgrade Chef InSpec to a newer version:

1. [Uninstall the current version](#uninstall-an-older-version-of-chef-inspec) using the steps for your platform.
1. [Download and install the new version](#install-chef-inspec) using the steps for your platform.

## Uninstall Chef InSpec

To remove Chef InSpec from your system, follow the steps in [Uninstall an older version of Chef InSpec](#uninstall-an-older-version-of-chef-inspec).

## More information

- [Chef Download API documentation](https://docs.chef.io/download/)
- [Chef InSpec documentation](https://docs.chef.io/inspec/7.0/)
