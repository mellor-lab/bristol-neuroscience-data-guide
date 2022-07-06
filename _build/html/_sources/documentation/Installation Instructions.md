(doc-install)=
# Installation Instructions

(doc-install-wingin)=
## WinGin Installation

(doc-install-gin-client)=
## GIN Client Installation
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
### On Linux
To install git-annex on a Linux machine, simply type in the terminal
```
sudo apt-get install git-annex
```
Test your installation by typing
```
git-annex version
```