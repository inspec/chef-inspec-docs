+++
title = "Uninstall Chef InSpec"
draft = false

[menu.uninstall]
    title = "Uninstall"
    identifier = "uninstall"
+++

## Uninstall Habitat-based Chef InSpec

If you installed Chef InSpec as a Habitat package, use the [`hab pkg uninstall`](https://docs.chef.io/habitat/latest/reference/habitat_cli/#hab-pkg-uninstall) command:

```sh
sudo hab pkg uninstall chef/inspec
```

## Uninstall Chef InSpec on Debian-based distributions

To uninstall Chef InSpec, follow these steps:

1. Remove the package:

   ```sh
   sudo apt-get remove inspec
   ```

1. Verify that the package has been removed:

   ```sh
   dpkg -l inspec
   ```

   The command returns no output if the package is removed successfully.

## Uninstall Chef InSpec on RPM-based distributions

To uninstall Chef InSpec, follow these steps:

1. Remove the package.

    - Using `dnf`:

      ```sh
      sudo dnf remove inspec-enterprise
      ```

    - For Amazon Linux 2 or systems using `yum`:

      ```sh
      sudo yum remove inspec
      ```

1. Verify that the package has been removed:

   ```sh
   rpm -qa | grep inspec
   ```

   The command returns no output if the package is removed successfully.

## Uninstall Chef InSpec on macOS

To uninstall Chef InSpec on macOS, follow these steps:

1. Remove the `inspec` symlink:

   ```sh
   sudo rm -f /usr/local/bin/inspec
   ```

1. Remove the installed application files:

   ```sh
   sudo rm -rf "/Library/Application Support/InSpec"
   ```

1. Remove the underlying Habitat package files:

   ```sh
   sudo rm -rf /opt/hab
   ```

   Skip this step if you use Habitat for other packages on this system.

1. Remove the package receipt:

   ```sh
   sudo pkgutil --forget io.chef.inspec-enterprise
   ```

1. Verify that the package has been removed:

   ```sh
   pkgutil --pkgs | grep io.chef.inspec-enterprise
   ```

   The command returns no output if the package is removed successfully.

## Uninstall Chef InSpec on Windows

To uninstall using the Windows UI:

1. Open **Settings > Apps > Installed apps**.
1. Search for **Chef InSpec**.
1. Select **Uninstall**, then follow the on-screen prompts.

To uninstall from the command line, run the following command in an elevated PowerShell or Command Prompt session:

```powershell
msiexec /x inspec-enterprise-<VERSION>-x64.msi /qn
```

Replace `<VERSION>` with the version number of the currently installed package.
