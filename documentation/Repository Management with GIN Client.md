(doc-gin-client)=
# Repository Management with GIN Client
This is the documentation on how to use GIN command line tools for research data management. Below is the description of some notation that is used here and that you may not be familiar with: \
[] - square brackets mean an optional argument (inclusive of brackets) \
<> - angle brackets mean that the text within should be replaced with an appropriate argument (inclusive of brackets).

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
## Create a Bare Repository
You create a local repository on your computer and a remote repository on the GIN server by running a command
```
gin create
```
or
```
gin create [<repository-name>] [<'repository description'>]
```
The repository will be created on the remote repository and downloaded locally onto your computer inside a folder with the same name as the repository. The new local repository should contain a hidden folder .git that tracks local changes when you explicitly record them (see [the section on commiting changes](doc-gin-client-update-local-repo)). If you want to use your current working directory as the root folder of the local repository, add a flag to the ```create``` command
```
gin create --here
```

(doc-gin-client-create-remote-repo)=
## Create a Remote-only Bare Repository
Throughout this chapter the repository residing on your local or public GIN server is refered to as a remote repository. It is the repository where you push changes from your local repository and where your collaborators do the same. To create a remote repository that does not have a local corresponding repository (a clone), like how you create a repository using the web interface, you call the ```create``` with a special flag like in the line below
```
gin create --no-clone
```

(doc-gin-client-create-local-repo)=
## Initialise a Repository
In order to initialise a local repository that does not have a remote equivalent, in the terminal we type
```
gin init
```
This command initialises the current working directory as the root folder of the new repository. All files within the root folder and and all subfolders are also initialised as part of the repository, unless you initialise the repository with a .gitignore file that explicitly states not to ignore certain files. As a test, check if a hidden folder .git has been created inside your working directory.

(doc-gin-client-list-repos)=
## Find a Repository
If you want to list all remote repositories that you own, in the terminal type
```
gin repos
```
You may see a smilar looking output
```
* dervinism/vr-setup-project
        Location: https://gin.g-node.org/dervinism/vr-setup-project
        Description: A mock repository containing all files used in building and running the virtual reality experimental set-up.

* dervinism/intracellular-rec-project
        Location: https://gin.g-node.org/dervinism/intracellular-rec-project
        Description: Mock intracellular recordings and analysis code

* dervinism/GIN-tutorial-Mellor-lab-retreat-22-06-2022
        Location: https://gin.g-node.org/dervinism/GIN-tutorial-Mellor-lab-retreat-22-06-2022
        Description: An introductory tutorial to GIN (G-Node Infrastructure) neuroscience data sharing platform at UoB
        This repository is public
```
To see all repositories that you are a member of, type
```
gin repos --all
```
To see only repositories that you share with others (excluding own repositories), type
```
gin repos --shared
```

(doc-gin-client-update-local-repo)=
## Record Changes to a Local Repository (commit)
The action of updating the change tracking system of the local instance of your repository (located inside the .git folder) with changes that you made to your local files is called commiting. You commit changes by calling the commit command from the root folder of your repository
```
gin commit -m <"message string">
```
Give an apt and succinct message describing what changes you made in comparison to the previous commit. If it is the initial commit, it would suffice to state so. Mind though that the commit action does not update the remote repository, your changes are still local. There may not even be a remote repository yet. This is different to using the web interface which only lets you to upload files simultaneously with the commit message.

You may be specific and commit changes relating only to a few files by specifying file names with relative paths to them
```
gin commit -m <"message string"> [<filenames>]...
```

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