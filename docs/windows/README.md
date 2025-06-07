```
(ZIP File)
install.ahqdb
├── dist/
│ └── (files to extract)
├── isInstalled.ps1
├── install.ps1
├── uninstall.ps1
├── update.ps1
├── link.toml
└── .build
```

## Table of contents

- [dist directory](#dist-directory)
- [isInstalled.ps1](#isinstalledps1)
- [install.ps1](#installps1)
- [uninstall.ps1](#uninstallps1)
- [update.ps1](#updateps1)
- [link.toml](#linktoml)
- [.build](#build)

## `dist` directory

The `dist` directory contains the files to be extracted to the installation directory.

These files are extracted before the `install.ps1` script is copied and ran.

## `isInstalled.ps1`

The `isInstalled.ps1` script is ran to check if the application is already installed.

The ps1 script should exit with the following exit codes:

- `0`: The application is installed
- `1`: The application is not installed

##### The `isInstalled.ps1` script is ran with the following **environment variables**:

- `EXECUTION_MODE`

  > Either `UserMode` or `AdminMode`

  - **UserMode**: The `install.ps1` script is ran as the current user.
  - **AdminMode**: The `install.ps1` script is ran as the admin user.

  > ### Note
  >
  > a. `UserMode` also means that the application is being installed for the current user
  >
  > b. `AdminMode` also means that the application is being installed for the whole machine

## `install.ps1`

The `install.ps1` script is ran after the `dist` directory is extracted to the installation directory.

##### The `install.ps1` script is ran with the following **environment variables**:

- `AHQDB_INSTALL_DIR`

  > The installation directory.

  > This directory **need not** be the **same directory** where the `ps1` script is copied over to.

- `EXECUTION_MODE`

  > Either `UserMode` or `AdminMode`

  - **UserMode**: The `install.ps1` script is ran as the current user.
  - **AdminMode**: The `install.ps1` script is ran as the admin user.

  > ### Note
  >
  > a. `UserMode` also means that the application is being installed for the current user
  >
  > b. `AdminMode` also means that the application is being installed for the whole machine

## `uninstall.ps1`

The `uninstall.ps1` script is ran **before** the `dist` directory is removed in the uninstall process.

This is intended to revert the changes made by the `install.ps1` script.

[The same **environment variables** as `install.ps1` are also available in `uninstall.ps1`](#the-installps1-script-is-ran-with-the-following-environment-variables)

## `update.ps1`

The `update.ps1` script is ran **after** the **new** `dist` directory from the update is extracted to the installation directory.

**Note that the `update.ps1` script is ran when both the **old** and the **new** `dist` directory are available.**

### Expected Directory Map during this time

```
<app-id>/
├── dist-v{old-version}/
│ └── (older installation files)
├── dist-v{new-version}/
│ └── (newer installation files)
└── update.ps1
```

##### The `update.ps1` script is ran with the following **environment variables**:

- `AHQDB_OLD_INSTALL_DIR`

  > The (old) installation directory.

  > This directory **need not** be the **same directory** where the `ps1` script is copied over to.

- `AHQDB_NEW_INSTALL_DIR`

  > The (new) installation directory.

  > This directory **need not** be the **same directory** where the `ps1` script is copied over to.

- `OLD_BUILD`

  > The old build number from the `.build` file.

- `EXECUTION_MODE`

  > Either `UserMode` or `AdminMode`

  - **UserMode**: The `install.ps1` script is ran as the current user.
  - **AdminMode**: The `install.ps1` script is ran as the admin user.

  > ### Note
  >
  > a. `UserMode` also means that the application is being installed for the current user
  >
  > b. `AdminMode` also means that the application is being installed for the whole machine

## `link.toml`

The `link.toml` file is a TOML file that contains the link (windows application shortcut) specification for the application.

`link.toml` is used after the `install.ps1` script is ran.

### Sample:

```toml
[link]
name = "My Cool Application"
# The first argument is the path to the icon relative to the dist/ dir
# DO NOT ADD `./dist/` at the beginning
exe = "myapp.exe"
# Optional
args = "--this --is --a --shortcut"
# Optional
description = "My Cool Application is here! Loream ipsum dolor sit amet"
# Optional
# The first argument is the path to the icon relative to the dist/ dir
# DO NOT ADD `./dist/` at the beginning
# The second argument is the index of the icon, for .ico files, it is generally 0
icon = ["icon1.ico", 0]
```

## `.build`

The `.build` file is a file that contains the build number of the application.

This build number is the `OLD_BUILD` environment variable in [update.ps1](#the-updateps1-script-is-ran-with-the-following-environment-variables)

> This is not the same as Application `VERSION`
