# Red Hat Package Management
## RPM
`rpm` is the low-level package management tool for RPM-based distributions.
### Install an RPM Package
```bash
rpm -i package.rpm
```
Example:
```bash
rpm -i webmin-2.021-1.noarch.rpm
```
### Ignore Dependencies
```bash
rpm -i --nodeps package.rpm
```
> `--nodeps` should be used carefully because it can install a package with missing dependencies.
### Query Package Information
```bash
rpm -qi webmin
```
### Remove a Package
```bash
rpm -e webmin
```
### List Installed Packages
```bash
rpm -qa
```
Search installed packages:
```bash
rpm -qa | grep firefox
```
### Verify Package
```bash
rpm -k package.rpm
```

## YUM
YUM provides higher-level package management on older Red Hat-based systems.
```bash
yum update
yum upgrade
yum install firefox
yum remove firefox
yum search gedit
```
Repository configuration is commonly stored under:
```bash
cd /etc/yum.repos.d/
ls
```

## Download RPM Packages
`yumdownloader` can download RPM packages without installing them.
```bash
yumdownloader firefox
```
For downloading packages together with dependencies:
```bash
dnf download --resolve PACKAGE
```
The downloaded RPM files can then be transferred to an offline system and installed with:
```bash
sudo dnf install ./*.rpm
```
> `dnf download` is provided through the DNF tooling on systems where the command is available.

## RPM vs YUM/DNF
|Tool|	Role|
|:---|:---:|
|`rpm`|	Low-level RPM package management|
|`yum`|	High-level package management|
|`dnf`|	Modern package manager replacing YUM on many systems|
|`yumdownloader`|	Download RPM packages without installing|
|`dnf download`|	Download RPM packages|
