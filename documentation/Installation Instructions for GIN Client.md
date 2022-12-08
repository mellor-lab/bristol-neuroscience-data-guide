(doc-install)=
# Installation Instructions for GIN Client

(doc-install-wingin)=
## WinGIN Installation
Download [WinGIN-install.exe](https://gin.g-node.org/G-Node/wingin-installers/raw/master/WinGIN-install.exe) and [Setup.msi](https://gin.g-node.org/G-Node/wingin-installers/raw/master/Setup.msi) files to a local directory. Run WinGIN-install.exe and follow the steps. At the end of the installation, you will be asked to install Dokan. If Dokan is already installed, you may skip this step by either unchecking the checkbox or cancelling the Dokan (un)install window that will appear. Launch WinGIN and wait for it to download and extract GIN CLI. In your start menu WinGIN will appear as GinClientApp.

(doc-install-gin-client)=
## GIN Client Installation
(doc-install-gin-client-linux)=
### On Linux
[Download the bundle file](https://gin.g-node.org/G-Node/gin-cli-releases/raw/master/gin-cli-latest.deb) containig GIN client, git, and git-annex. To install it, either double-click the file (if you are using graphical interface) or change your current working directory to the file location and execute the following line in the terminal
```
sudo dpkg -i gin-cli-latest.deb
```
You should be all set. To test the installation type in
```
gin --version
```

If you already have git and git-annex installed on your system, you can download gin client only without its dependencies.

(doc-install-git)=
## Git Installation
(doc-install-git-linux)=
### On Linux
To install git on a Linux machine, simply type in the terminal
```
sudo apt-get install git
```
Test your installation by typing
```
git --version
```

(doc-install-git-annex)=
## Git-Annex Installation
(doc-install-git-annex-linux)=
### On Linux
To install git-annex on a Linux machine, simply type in the terminal
```
sudo apt-get install git-annex
```
Test your installation by typing
```
git-annex version
```