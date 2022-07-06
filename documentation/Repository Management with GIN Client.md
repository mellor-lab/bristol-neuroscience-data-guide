(doc-gin-client)=
# Repository Management with GIN Client
This is the documentation on how to use GIN command line tools for research data management. Below is the description of some notation that is used here and that you may not be familiar with:
[] - square brackets mean an optional argument
<> - angle brackets mean that the text within should be replaced with an appropriate argument

(doc-gin-client-login)=
## Login to your Account
In order to login to GIN, you have to have a registered account on the [local GIN server](https://www.bristol.ac.uk/bristolgin/) or on the [public GIN server](https://gin.g-node.org/). Instructions on how to do it are available [here](doc-gin-web-register). You login by typing in the terminal
```
gin login
```
or
```
gin login [<username>]
```
You should be prompted to enter your credentials. Once you are loged in, you can start managing your GIN repositories using command line tools.

(doc-gin-client-create-repo)=
## Create a Repository
The difference of working with the GIN web interface and working with the GIN client is that instead of working directly with the remote repository residing on the server you are going to be working with the local repository residing on your computer first while periodically uploading changes to the remote repository second. Hence, you are going to have a local and remote repositories both tracking changes you make. Typically you would be synchronising your working files with the local repository frequently while synchronising with the remote repository occasionally. Having this in mind, you create a local repository on your computer by running a command
```
gin create
```
or
```
gin create [<repository-name>] [<'repository description'>]
```
The repository will be created locally on your computer.

(doc-gin-client-find-repo)=
## Find a Repository

(doc-gin-client-update-repo)=
## Update a Repository (Upload Files)

(doc-gin-client-dowload-repo)=
## Download (Clone) a Repository

(doc-gin-client-dowload-file)=
## Download a File

(doc-gin-client-delete-file)=
## Delete a File

(doc-gin-client-create-file)=
## Create a Text File

(doc-gin-client-rename-repo)=
## Rename a Repository

(doc-gin-client-repo-visibility)=
## Change the Visibility of a Repository

(doc-gin-client-transfer-repo)=
## Transfer the Ownership of a Repository

(doc-gin-client-delete-repo)=
## Delete a Repository

(doc-gin-client-add-collaborator)=
## Add Collaborators to a Repository

(doc-gin-client-create-org)=
## Create an Organisation

(doc-gin-client-delete-org)=
## Delete an Organisation

(doc-gin-client-add-org-member)=
## Add an Organisation Member

(doc-gin-client-grant-org-ownership)=
## Grant Organisation Ownership to a User

(doc-gin-client-create-org-repo)=
## Create a Repository under Organisation's Ownership

(doc-gin-client-create-team)=
## Create a Team

(doc-gin-client-add-team-memebers)=
## Add Team Members

(doc-gin-client-add-team-repo)=
## Add a Repository to a Team

(doc-gin-client-delete-team)=
## Delete a Team

(doc-gin-client-delete-account)=
## Delete an Account

(doc-gin-client-report-issue)=
## Report an Issue