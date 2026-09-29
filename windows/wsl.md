# Windows Subsystem for Linux (WSL)
## Overview
WSL allows Linux distributions to run directly on Windows without using a traditional virtual machine interface.
This document covers the installation and basic management of WSL.
---
## Prerequisites and Environment
Some Windows features may be required depending on the WSL configuration.
### Virtual Machine Platform
Enable the **Virtual Machine Platform** Windows feature for WSL 2.
### Windows Subsystem for Linux
Enable the **Windows Subsystem for Linux** Windows feature.
### Network Access
WSL installation and distribution downloads require network access to Microsoft services.
If access to required services is restricted in the current network environment, a VPN may be necessary.
> Regional settings and VPN are environment-dependent and are not WSL requirements.
---
## Installing WSL
Open **Command Prompt or PowerShell as Administrator**.
### Install WSL
```powershell
wsl --install
```
This installs WSL and the default Linux distribution.
### Install a Specific Distribution
```powershell
wsl --install -d <Distro>
```
Example:
```powershell
wsl --install -d Ubuntu
```
## Working with Linux Distributions
### List Available Distributions
Show distributions available for installation:
```powershell
wsl --list --online
```
Short form:
```powershell
wsl -l -o
```
### List Installed Distributions
```powershell
wsl --list --verbose
```
Short form:
```powershell
wsl -l -v
```
This displays information such as the distribution state and WSL version.
## Running WSL
Start the default Linux distribution:
```powershell
wsl
```
Run a specific distribution:
```powershell
wsl -d <Distro>
```
Example:
```powershell
wsl -d Ubuntu
```
## WSL Information and Status
### Check WSL Status
```powershell
wsl --status
```
### Check WSL Version
```powershell
wsl --version
```
### Update WSL
```powershell
wsl --update
```
## Managing Distributions
### Set the Default Distribution
```powershell
wsl --set-default <Distro>
```
Example:
```powershell
wsl --set-default Ubuntu
```
### Set the Default WSL Version
```powershell
wsl --set-default-version 2
```
## Stopping WSL
### Shut Down All WSL Instances
```powershell
wsl --shutdown
```
### Terminate a Specific Distribution
```powershell
wsl --terminate <Distro>
```
Example:
```powershell
wsl --terminate Ubuntu
```
## Removing a Distribution
To completely remove a distribution:
```powershell
wsl --unregister <Distro>
```
Example:
```powershell
wsl --unregister Ubuntu
```
> `wsl --unregister` permanently removes the selected distribution and its data.
## Useful Notes
- WSL 2 uses a virtualized Linux kernel and provides better Linux compatibility than WSL 1.
- `wsl -l -v` is useful for checking installed distributions and whether they are using WSL 1 or WSL 2.
- `wsl --shutdown` stops all running WSL instances.
- `wsl --unregister` should be used carefully because the distribution's data is deleted.
- `wsl` is the standard command for starting the default distribution.
