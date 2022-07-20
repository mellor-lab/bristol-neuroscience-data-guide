(doc-gin-client)=
# Repository Management with GIN Client
This is the documentation on how to use GIN command line tools for research data management. Below is the description of some notation that is used here and that you may not be familiar with:
- []: square brackets mean an optional argument (inclusive of brackets)
- <>: angle brackets mean that the text within should be replaced with an appropriate argument (inclusive of brackets).
- Both types of brackets can be combined.
- The use of dashes in command argument names indicates that the argument name does not allow for blank spaces.
- Path arguments most often do not contain spaces. If your path does contain spaces, enclose your path argument within single or double quotations. For example, use either `gin some-command path/to/my/repository` or `gin some-command "path/to/my repository"`.

(doc-gin-client-list-commands)=
## List All GIN Client Commands
To get the names of all commands available to use with the GIN client and their brief descriptions, type
```
gin help
```

(doc-gin-client-doc-help)=
## Display Command Documentation
To print the documentation for a particular GIN client command, type
```
gin help command
```
where 'command' is the name of a specific command.

(doc-gin-client-configure-server)=
## Configure a GIN Server
After installing the GIN client but before starting to use it, you have to configure the GIN client to work with a particular GIN server. In the most likely case where you want to use Bristol GIN, you have to issue the following commands in your terminal
```
gin logout
gin add-server --web https://www.bristol.ac.uk/bristolgin:2121 --git git@bristol.ac.uk/bristolgin:22 bristol-gin
gin use-server bristol-gin
gin login
```
In another likely case where you want to use the public GIN server, type in the following commands into your terminal
```
gin logout
gin add-server --web https://gin.g-node.org:443 --git git@gin.g-node.org:22 gin
gin use-server gin
gin login
```
You are all set to use GIN. The use of the `use-server` command sets the configured server as default. All gin commands will work with the default server only. If you do not want to change your default server, skip the `use-server` command. Mind though that the above server configurations might change in the future.

(doc-gin-client-list-servers)=
## List Configured Servers
If you want to see the list of servers that your GIN client is configured to work with, in your terminal type
```
gin servers
```

(doc-gin-client-change-server)=
## Change the Default Server
If you want to change the default server that GIN client works with, in your terminal type
```
gin use-server alias
```
where 'alias' is the server name.

(doc-gin-client-remove-server)=
## Remove a Configured Server
If you want to remove a server from the list of servers that your GIN client is configured to work with, in your terminal type
```
gin remove-server alias
```
or
```
gin rm-server alias
```
where 'alias' is the server name.

(doc-gin-client-login)=
## Login to your Account
In order to login to GIN, you have to have a registered account on the [local GIN server](https://www.bristol.ac.uk/bristolgin/) or on the [public GIN server](https://gin.g-node.org/). Instructions on how to do it are available [here](doc-gin-web-register). You login by typing in the terminal
```
gin login
```
or
```
gin login <username>
```
You should be prompted to enter your credentials. If you want to login to an account on another server, include the `--server` flag, like
```
gin login [<username>] --server alias
```
where 'alias' is the name of a configured server. Once you are logged in, you can start managing your GIN repositories using command line tools.

(doc-gin-client-logout)=
## Logout from your Account
To logout from your GIN server account, type in the terminal
```
gin logout
```

(doc-gin-client-user-info)=
## Get GIN User Info
To display the currently logged in GIN user, type
```
gin info
```
To display information about another GIN user, type
```
gin info <username>
```
To display information about another user located on another server, type
```
gin info <username> --server alias
```
where 'alias' is the name of a configured server.

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
The repository will be created on the remote repository and downloaded locally onto your computer inside a folder with the same name as the repository. The new local repository should contain a hidden folder .git that tracks local changes when you explicitly record them (see [the section on commiting changes](doc-gin-client-update-local-repo)). If you want to use your current working directory as the root folder of the local repository, add a flag to the `create` command
```
gin create --here
```
If you want the remote instance of your repository to be located on a different server other than the default one, include the `--server` flag, like
```
gin create --server alias
```
where 'alias' is the name of a configured server.

(doc-gin-client-create-remote-repo)=
## Create a Remote Bare Repository
Throughout this chapter the repository residing on your local or public GIN server is refered to as a remote repository. It is the repository where you push changes from your local repository and where your collaborators do the same. To create a remote repository that does not have a local corresponding repository (a clone), like how you create a repository using the web interface, you call the `create` command with a special flag like in the line below
```
gin create --no-clone
```

(doc-gin-client-create-local-repo)=
## Initialise a Local Repository
In order to initialise a local repository that does not have a remote equivalent, in the terminal we type
```
gin init
```
This command initialises the current working directory as the root folder of the new repository. All files within the root folder and and all subfolders are also initialised as part of the repository, unless you initialise the repository with a .gitignore file that explicitly states not to track certain files. As a test, check if a hidden folder .git has been created inside your working directory.

(doc-gin-client-list-repos)=
## List Repository (and Find one)
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
To list repositories located on another server, include the `--server` flag, like
```
gin repos --server alias
```
where 'alias' is the name of a configured server.

(doc-gin-client-repo-info)=
## Display Repository Description
If you want to obtain information about a specific repository, in the terminal type
```
gin repoinfo <repopath>
```
where repopath describes the repository path in the form of repo-owner/repo-name.
To display information about a repository located on another server, type
```
gin repoinfo <repopath> --server alias
```
where 'alias' is the name of a configured server.

(doc-gin-client-update-local-repo)=
## Record Changes to a Local Repository (Commit)
The action of updating the change tracking system of the local instance of your repository (located inside the .git folder) with changes that you made to your local files is called commiting. You commit changes by calling the commit command from the root folder of your repository
```
gin commit . -m <"message string">
```
Give an apt and succinct message describing the changes you made since the previous commit. If it is the initial commit, it would suffice to state so. Mind though that the commit action does not update the remote repository, your changes are still local. There may not even be a remote repository yet. This is different to using the web interface which only lets you to upload files simultaneously with the commit message.

You may want to be specific and commit changes made only to a few files by specifying file names (or folder names) including paths
```
gin commit . -m <"message string"> <absolute or relative path to your files or folders>...
```

(doc-gin-client-file-status)=
## List the Status of Local Repository Files
You can request GIN to display the status of all files in the local repository by typing
```
gin ls
```
or
```
gin status
```
or display the status of a specific file or files within a sub-folder by typing
```
gin ls <absolute or relative path to your file or folder>
```
or
```
gin status <absolute or relative path to your file or folder>
```
Multiple files will be displayed recursively. According to the GIN documentation, the meaning of the status abbreviations are as follows:
- OK: The file is part of the GIN repository and its contents are synchronised with the server.
- TC: The file has been locked or unlocked and the change has not been recorded yet (and it is unmodified).
- NC: The local file is a placeholder and its contents have not been downloaded.
- MD: The file has been modified locally and the changes have not been recorded yet.
- LC: The file has been modified locally, the changes have been recorded but they haven't been uploaded.
- RM: The file has been removed from the repository.
- ??: The file is not under repository control.

(doc-gin-client-list-associated-remotes)=
## List Remote Repositories Associated with the Local Repository
A local repository can be associated with one or more remote repositories or with none at all. To list the available associated repositories where you can push local changes, in the terminal type
```
gin remotes
```
You may see a similar output
```
:: Configured remotes
 origin: ssh://git@gin.g-node.org:22/dervinism/dual-npx-project [default]
 repo-to-share: ssh://git@gin.g-node.org:22/dervinism/dual-npx-project-colab
```
The name 'origin' typically refers to a remote repository that is the origin of the local repository and where local changes are pushed by default. If there are more associated repositories, they all would also be listed.

(doc-gin-client-associate-remote)=
## Associate a Remote Repository with the Local Repository
In order to associate a remote repository with the local repository, while located in the local repository root directory type the following command in the terminal
```
gin add-remote <repo-name> <alias:repo-path>
```
You can specify any 'repo-name' for your newly added repository except for the reserved keyword 'all' which, if specified, performs updates to all associated remotes. Alias can only be either 'gin' for a remote GIN repository or 'dir' for a remote repository residing on the local file system. If neither is specified, it is assumed to be the address of a git server. For remotes residing on a configured GIN server, the path is the location of the repository on the server, in the form of repository-owner/repository-name. For local remotes, it is the path to the storage directory.

When a remote is added, but the specified name does not exist, the client will offer to create it.

A new remote is set as the default for uploading if no other remotes are associated. To set any new remote as the
default, use the flag `--default`.

For example, you could type something like
```
gin add-remote repo-to-share gin:dervinism/dual-npx-project-colab --default
```
This would prompt you to create a new default remote repository for uploading local changes
```
:: Checking remote: not found
Remote ssh://git@gin.g-node.org:22/dervinism/dual-npx-project-colab does not exist. Would you like to create it?
[c]reate / [a]dd anyway / a[b]ort: c
:: Creating repository 'dervinism/dual-npx-project-colab' OK
:: Added new remote: repo-to-share [ssh://git@gin.g-node.org:22/dervinism/dual-npx-project-colab]
:: Default remote: repo-to-share
```

(doc-gin-client-display-default-remote)=
## Display the Default Associated Remote Repository
In order to display the default remote, while located in the local repository root directory type the following command in the terminal
```
gin use-remote
```

(doc-gin-client-change-default-remote)=
## Change the Default Associated Remote Repository
In order to change the default remote, while located in the local repository root directory type the following command in the terminal
```
gin use-remote <repo-name>
```

(doc-gin-client-remove-remote)=
## Remove an Associated Remote Repository from the Local Repository
In order to remove an associated remote repository from the local repository, within the root directory of the local repository type the following command
```
gin remove-remote <repo-name>
```
or
```
gin rm-remote <repo-name>
```
The remote repository would still be available at its remote location but would no longer be associated with the local repository.

(doc-gin-client-update-repo)=
## Update a Remote Repository (Upload Files)
Once you updated your local repository with a commit message, you can update your remote repository by pushing changes. The gin client command for doing that is
```
gin upload .
```
This command has to be called from within the root folder of your local repository and it will push all changes to the associated remote repository. To be more specific, you can specify files that you want to upload.
```
gin upload <absolute or relative path to your files>...
```
You can use wildcards when specifying filenames. For example,
```
gin upload *.npy
```
Finally, you can be specific about which remote repositories you want to push your changes to. For example,
```
gin upload . --to origin, repo-to-share
```
The flag `--to` is used to indicate repository names to push changes to. The names 'origin' and 'repo-to-share' are just examples, but 'origin' typically referers to the remote repository that was downloaded (cloned) on the local file system.

(doc-gin-client-remove-content)=
## Remove the Content of Local Files
When you [update a remote repository](doc-gin-client-update-repo), you may wish to remove the content of files residing in your local repository as they consume storage space. In order to do so, while inside the root folder of your local repository type
```
gin remove-content
```
or simply
```
gin rmc
```
All your local files should then be replaced with file pointers or symbolic links. Files or changes that were not uploaded onto the remote repository are kept. If you wish to remove the content of a specific sub-folder, change your working directory to this sub-folder and issue the same command. Another way to do that is to include the path of that folder or of a specific file, like
```
gin remove-content <absolute or relative path to file or folder>
```
Again, if the file has not been uploaded onto the remote repository (even if specified explicitly), the contents of that file will not be removed. Files with removed content appear as 'No Content' or 'NC' when running the `gin ls` command.

(doc-gin-client-dowload-repo)=
## Download (Clone) a Remote Repository
In order to download a copy (clone) of a full remote repository onto your local machine, in your terminal type
```
gin get <repopath>
cd <repo-name>
gin get-content
```
where 'repopath' is the path to a remote repository in the form of repo-owner/repo-name. The cloning action will create a new folder called 'repo-name' and initialise with default options. All files within the remote repository will be downloaded locally. If you do not wish to download large files, you can skip the command `get-content`. In that case, placeholders will be downloaded instead of full files only. Alternatively, instead of `get-content` command one can use `download --content` command.

(doc-gin-client-update-local-repo-remote-content)=
## Update a Local Repository with Remote Content
If you are working collaboratively on a repository, it is a good idea to bring your local repository in sync with the remote repository prior to starting working on your local instance. One way to do this is to change your working directory to the root folder of your local repository and call the `download` command
```
gin download --content
```
or
```
gin get-content
```
or
```
gin getc
```
These commands will download new files, update old ones, and delete any local files that were removed from the remote repository. Note that if you modified files in the local repository, the download command will fail and issue an error noting you that local files cannot be overwritten or merged. Local files can only be overwritten if these changes are tracked by the remote repository and, thus, are historically preceding the more recent remote changes. In order to download remote changes that clash with the local changes, you have to [upload your local changes to the remote repository](doc-gin-client-update-repo) first. In this way the remote reposiotry will be able to track the clashing changes.

(doc-gin-client-update-local-files-remote-content)=
## Update Local Files with Remote Content
If you want to update specific files or folders with remote content or if you want to restore the content to local files and folders with removed content, type
```
gin get-content <absolute or relative path to file or folder>
```
This action will fail in instances when you make changes to your local files that were never uploaded onto a remote repository. [Upload local changes](doc-gin-client-update-repo) first before pulling any content from remote repositories.

(doc-gin-client-sync-repos)=
## Synchronise Local and Remote Repositories
Repository synchronisation performs download and upload actions simultaneously. Any local changes are uploaded onto the remote repository if they are not yet tracked by the remote repository. The reverse is also true: remote changes are downloaded onto the local repository if the local repository is not up to date with the remote repository. However, if the same files are being edited on both local and remote instances of the repository, you would have to issue the `upload` command in order to register these changes with the remote repository before you can synchronise the two repositories (see [upload files section](doc-gin-client-update-repo)). In order to synchronise repositories, in the terminal type
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
Setting 'number' to equal to 0 will list all previous repository versions. Below is an example output
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
In order to restore a previous version of the repository to a different folder than the root repository folder, use the `--copy-to` flag by typing
```
gin version --copy-to <absolute or relative path to local file system folder>
```
or
```
gin version --id <hash> --copy-to <absolute or relative path to local file system folder>
```

(doc-gin-client-rollback-part-repo)=
## Roll back a Part of a Repository to an Earlier Version
If you need to restore a part of a repository, like a file or a folder, to an earlier version, you can specify the path to the file or the folder relative to the repository root folder.
```
gin version <root-relative path to file or folder>
```
or
```
gin version --id <hash> <root-relative path to file or folder>
```
In case when you want to restore a file or a folder to a different location other than the root folder of the local repository, specify accordingly
```
gin version --copy-to <absolute or relative path to local file system folder> <remote repository root-relative path to file or folder>
```
or
```
gin version --id <hash> --copy-to <absolute or relative path to local file system folder> <remote repository root-relative path to file or folder>
```

(doc-gin-client-lock-file)=
## Prevent Local File or a Folder from Being Edited
While working with your local repository you may want to prevent certain files or folders from being edited. In order to do that, type
```
gin lock <filenames>...
gin commit . -m <"commit message">
```
Here 'filenames' could refer to one or multiple files or folders, including their absolute or relative paths. When you lock a file or a folder, you also have to use the `commit` command in order to update the local tracking system. Issuing the `lock` command replaces the local locked files with pointers or symbolic links if supported by the file system. Locked files that have not yet been committed are marked as 'Lock status changed' or 'TC' in the output of the `ls` command.

(doc-gin-client-unlock-file)=
## Unlock a Local File or a Folder for Editing
If at some point you locked local files or folders to prevent them from being edited, you can unlock them by typing in the terminal
```
gin unlock <filenames>...
gin commit . -m <"commit message">
```
Here 'filenames' could refer to one or multiple files or folders, including their absolute or relative paths. When you unlock a file or a folder, you also have to use the `commit` command in order to update the local tracking system. Issuing the `unlock` command should download full files locally in places where only file pointers exist. Unlocked files that have not yet been committed are marked as 'Lock status changed' or 'TC' in the output of the `ls` command.