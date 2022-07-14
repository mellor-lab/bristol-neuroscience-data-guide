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

(doc-gin-client-logout)=
## Logout from your Account
To logout from your GIN server account, type in the terminal
```
gin logout
```

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
## Create a Remote Bare Repository
Throughout this chapter the repository residing on your local or public GIN server is refered to as a remote repository. It is the repository where you push changes from your local repository and where your collaborators do the same. To create a remote repository that does not have a local corresponding repository (a clone), like how you create a repository using the web interface, you call the ```create``` command with a special flag like in the line below
```
gin create --no-clone
```

(doc-gin-client-create-local-repo)=
## Initialise a Local Repository
In order to initialise a local repository that does not have a remote equivalent, in the terminal we type
```
gin init
```
This command initialises the current working directory as the root folder of the new repository. All files within the root folder and and all subfolders are also initialised as part of the repository, unless you initialise the repository with a .gitignore file that explicitly states not to ignore certain files. As a test, check if a hidden folder .git has been created inside your working directory.

(doc-gin-client-list-repos)=
## List repository (and find one)
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
gin commit . -m <"message string">
```
Give an apt and succinct message describing the changes you made since the previous commit. If it is the initial commit, it would suffice to state so. Mind though that the commit action does not update the remote repository, your changes are still local. There may not even be a remote repository yet. This is different to using the web interface which only lets you to upload files simultaneously with the commit message.

You may be specific and commit changes relating only to a few files by specifying file names with relative paths to them
```
gin commit . -m <"message string"> [<absolute or relative path to your files>]...
```

(doc-gin-client-update-repo)=
## Update a Remote Repository (Upload Files)
Once you updated your local repository with a commit message, you can update your remote repository by pushing changes. The gin client command for doing that is
```
gin upload .
```
This command has to be called from within the root folder of your local repository and it will push all changes to the associated remote repository. To be more specific, you can specify files that you want to upload.
```
gin upload [<absolute or relative path to your files>]...
```
You can use wildcards when specifying filenames. For example,
```
gin upload *.npy
```
Finally, you can be specific about which remote repositories you want to push your changes to
```
gin upload . --to origin, repository-to-share
```

(doc-gin-client-dowload-repo)=
## Download (Clone) a Remote Repository
In order to download a copy (clone) of a full remote reposiotry onto your local machine, in your terminal type
```
gin get <repopath>
cd <repo-name>
gin get-content
```
where repopath is the path to a remote repository in the form of repo-owner/repo-name. The cloning action will create a new folder called repository-name and initialise with default options. All files within the remote repository will be downloaded locally. If you do not wish to download large files, you can skip the command ```get-content``` which will download placeholders only instead of full files. Alternatively, instead of ```get-content``` command one can use ```download --content``` command.

(doc-gin-client-update-local-repo-remote-content)=
## Update a Local Repository with Remote Content
If you are working collaboratively on a repository, it is a good idea to bring your local repository in sync with the remote repository prior starting working on your local instance. One way to do it is to change your working directory to the root folder of your local repository and call the ```download``` command
```
gin download --content
```
This command will download new files, update old ones, and delete any local files that were removed from the remote repository. Note that if you modified files in the local repository, the download command will fail and issue an error noting you that local files cannot be overwritten or merged. Local files can only be overwritten if these changes are tracked by the remote repository and, thus, are historically preceding the more recent remote changes. In order to download remote changes that clash with the local changes, you have to [upload your local changes to the remote repository](doc-gin-client-update-repo) first. In this way the remote reposiotry will be able to track the clashing changes.

(doc-gin-client-sync-repos)=
## Synchronise Local and Remote Repositories
Repository synchronisation performs download and upload actions simultaneously. Any local changes are uploaded onto the remote repository if they are not yet tracked by the remote repository. The reverse is also true: remote changes are downloaded onto the local repository if the local repository is not up to date with the remote repository. However, if the same files are being edited on both local and remote instances of the repository, you would have to issue the ```upload``` command in order to register these changes with the remote repository before you can synchronise the two repositoriesn (see [upload files section](doc-gin-client-update-repo)). In order to synchronise repositories, in the terminal type
```
gin sync --content
```

(doc-gin-client-list-repo-vers)=
## List Repository Versions
One of the main functions of GIN is research data repository version control. In order to list repository versions in reverse order, type
```
gin version
```
This will list the last 10 repository versions and you will be prompted to enter the version number that you want to roll back to. The files will be restored to the current working directory. If you do not want to roll back to any particular version, press Ctrl+Z. To list versions going back further into the past, type
```
gin version --max-count <number>
```
or
```
gin version -n <number>
```
Setting number to equal to 0 will list all previous repository versions. Below is an example output
```
...
[ 3]  b05338f * Thu Jul 14 11:26:22 2022 (+0100)

    Modified the log file

  Modified
    mock_project/animal_ID/session1_2022-05-16/log.txt
...
```
The output list displays the commit ID (hash), date and time of the commit, the commit message, and names of files that were modified. You can use the commit ID to [roll back repository version](doc-gin-client-rollback-repo). 

(doc-gin-client-rollback-repo)=
## Roll back a Repository to an Earlier Version
One of the main functions of GIN is research data repository version control. In order to roll back repository to an earlier version, type
```
gin version
```
This will list the last 10 repository versions and prompt you to enter the version number you would like to roll back to. The files would be restored to the current working directory. Another way to roll back would be to explicitly specify commit id (hash; see how to find one [here](doc-gin-client-list-repo-vers)). Simply type
```
gin version --id <hash>
```
In order to restore a previous version of the repository to a different folder than the root repository folder, use the ```--copy-to``` flag by typing
```
gin version --copy-to <absolute or relative path to a system folder>
```
or
```
gin version --id <hash> --copy-to <absolute or relative path to a system folder>
```

(doc-gin-client-rollback-part-repo)=
## Roll back a Part of a Repository to an Earlier Version
If you need to restore a part of a repository, like a file or a folder, to an earlier version, you can specify the path to the file or the folder relative to the repository root folder.
```
gin version <root-relative path to a file or a folder>
```
or
```
gin version --id <hash> <root-relative path to a file or a folder>
```
In case when you want to restore a file or a folder to a different location other than the root folder of the local repository, specify accordingly
```
gin version --copy-to <absolute or relative path to a system folder> <root-relative path to a file or a folder>
```
or
```
gin version --id <hash> --copy-to <absolute or relative path to a system folder> <root-relative path to a file or a folder>
```