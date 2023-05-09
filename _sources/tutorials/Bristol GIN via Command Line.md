(tutorials-cli)=
# Bristol GIN via Command Line

(tutorials-cli-get-started)=
## Get Started
In this tutorial I give an example of how you can manage your GIN repository using command line tools. First step is to download and [install the GIN client](https://gin.g-node.org/G-Node/Info/wiki/GIN+CLI+Setup) on your computer. GIN client can be installed on Windows, macOS, and Linux machines.

Once the GIN client is installed, [register your account](doc-gin-web-register) with [Bristol GIN](welcome-bristol-gin) or the [public GIN service](https://gin.g-node.org/user/sign_up). Registration is easy and when it is done, you can login to your GIN account via the command line (just specify your own username; if on Windows make sure you are using the cmd terminal and not the powerShell):
```
gin login <username>
```

(tutorials-cli-create-repo)=
## Set-up Repository
Now we intend to create a repository. You can go directly to the folder where your local repository is located. Alternatively, you can download a [mock repository](https://gin.g-node.org/dervinism/mockRepository) from GIN prepared for this tutorial:
```
gin get dervinism/mockRepository
cd mockRepository
gin get-content
```
Make sure you delete the hidden .git folder inside the repository root folder once the repository downloading is finished. You can also rename the root folder as you like. I am going to rename the folder to mockRepositoryCLI to reflect the fact that the repository is managed using the command line interface.

This mock repository contains mock Neuropixels data acquired during a single recording session with two silicon probes. It has the following folder structure:
```mockRepositoryCLI/mockExperiment/animal_ID/session1_2022-05-16/probe1``` (all data pertaining to the recording probe 1) \
```mockRepositoryCLI/mockExperiment/animal_ID/session1_2022-05-16/probe2``` (all data pertaining to the recording probe 2) \
```mockRepositoryCLI/mockExperiment/animal_ID/session1_2022-05-16``` (data that is related to both recording probes like pupil video, for example).

We initialise the mockRepositoryCLI folder as a local GIN repository by typing in the terminal while at the repository root folder:
```
gin init
```
What this does is creating .git folder which keeps track of changes we make to our local repository. There is no gitignore file, so all files in the local repository are version controlled. If you want to make exceptions to that, please add a [gitignore file](https://git-scm.com/docs/gitignore) specifying which files should be exempt from the local GIN repository.

We can now create the remote repository on the remote GIN server by typing:
```
gin create mockRepositoryCLI "mock Neuropixels data pertaining to a single recording session acquired using two extracellular probes" --no-clone
```
This command creates an empty remote repository with the description enclosed in the double quotes. If you login to your account using the web interface, you will be able to see that a new empty repository with this name has been created. The flag ```--no-clone``` indicates that the creation of the local repository copy is ommited because we already have a local copy. For more different options on creating repositories have a look at [our GIN client documentation](doc-gin-client) or type ```gin help create``` in the terminal.

As a final step of setting up our repository, we associate local and remote copies of our repositories by typing (replace the angle brackets and their content with your own user name):
```
gin add-remote mockRepositoryCLI gin:<username>/mockRepositoryCLI
```
```gin:``` indicates that the server resides on the remote GIN server. If the remote server was residing localy, you could specify it in the form ```dir:path```.

If we make any changes to the local repository, we should commit those changes to the local version control system regularly. This is a new repository and we have not made any changes to it yet. Therefore, we could just indicate that by typing in the terminal:
```
gin commit . -m "Initial commit"
```

(tutorials-cli-upload-files)=
## Upload Repository
Next we upload the contents of our local GIN repository to the remote GIN repository by simply typing
```
gin upload .
```
Now If you want to [transfer the ownership](tutorials-gui-transfer-ownership) of mockRepositoryCLI to the University of Bristol this could be done using the web interface. We [create a team](tutorials-gui-create-team) and associate it with a particular repository using the web interface too, as gin command line tools are very limited. Alternatively, one can use a [DataLad service](http://handbook.datalad.org/en/latest/basics/101-139-gin.html) or Git command line tools as GIN is integrated with these much more powerful tools. However, there is a steep learning curve associated with mastering the use of DataLad.

(tutorials-cli-remove-content)=
## Remove Local Repository Content
One advantage of using GIN for your data repository mangement is that you do not need to keep duplicate repositories in order to prevent accidentally corrupting your main repository. One reason for that is having version control system. The other reason is that you can safely remove the content of your local repository and replace it with pointers to original files. As a result you can save space on your local hard-drive. To remove the local content, type the following line in your command terminal:
```
gin remove-content
```
Local files larger than 10 megabytes should be replaced by symbolic links. To restore the file content, simply type in
```
gin get-content
```

(tutorials-cli-commit)=
## Record Repository Changes
Once you have your repository set up, and both its local and remote copies are in sync, you would be working on your local copy only. Changes made to your local copy will need to be periodically recorded. For example, you may have updated your repository README file (e.g., changed the repository title from mockRepo to mockRepoCLI). In order record those changes type:
```
gin commit . -m "Updated README file"
```
This change will stay local only. In order to propagate it to the remote copy, do not forget to execute
```
gin upload .
```

(tutorials-cli-download)=
## Download Repository
If you are working with a collaborator, and he or she has been given access to your repository, they can download it by typing:
```
gin get <username>/mockRepositoryCLI
```
The remote repository will be downloaded with its .git folder which has the info associating it with its remote instance. However, files that are bigger than 10 megabytes would not be downloaded fully, only pointers would. To download these bigger files, type:
```
gin get-content
```

(tutorials-cli-sync)=
## Update Local Repository with Remote Content
Your collaborators were working with the same repository for a while and you have not been up to date with the latest changes. The latest changes are on the remote repository and to make sure that your local repository is up to date with its remote instance, you can just simply run:
```
gin sync
```
This command updates the local repository. However, any new files on the remote server are downloaded only as pointers. If you want to download full files, you should specify that file content is required:
```
gin sync --content
```
or
```
gin sync
gin get-content
```
Often there are multiple ways of achieving the same result.

(tutorials-cli-revert)=
## Revert Repository to Previous Version
A useful action that you can perform on a remote repository is to revert certain unwanted recent changes and commit new changes. Before you can do that, you should be able to list the available reposiotry versions. To do so, type
```
gin version
```
This will list 10 most recent repository versions (to get more information on version display check [here](doc-gin-client-list-repo-vers)). In my case I get the following output:
```
[1]  6815212 * Tue Sep 20 16:59:19 2022 (+0100)

    Added a few larger files

  Modified
    README.md, mockExperiment/animal_ID/session1_2022-05-16/probe1/continuous.ap_CAR.cbin,
    mockExperiment/animal_ID/session1_2022-05-16/probe1/continuous.lf.cbin, mockExperiment/animal_ID/session1_2022-05
    16/probe2/continuous.ap_CAR.cbin, mockExperiment/animal_ID/session1_2022-05-16/probe2/continuous.lf.cbin

[2]  e19c06d * Tue Sep 20 13:05:47 2022 (+0100)

    Initial commit

  Added
    LICENSE.txt, README.md, mockExperiment/animal_ID/session1_2022-05-16/downsampledMat.mat,
    ...
```
You can see a number of useful attributes displayed associated with each repository version, like files added or modified, the modification date, and, importantly, the ID or the hash associated with the version. You will need the latter number in order to restore the repository to that particular state (to read more on version rollback check [here](doc-gin-client-rollback-repo)). Let's say we want to restore the repository to its initial state. We type
```
gin version --id e19c06d
```
Now if you look at your local repository files, you would see that they have changed to their initial version. We can now do some work on our local repository, record the local repository changes, and push those changes to the remote repository by typing:
```
gin commit . -m "Fixed that annoying bug introduced earlier"
gin upload .
```
or we can revert back to the version before by typing
```
gin version --id 6815212
```

(tutorials-cli-resources)=
## Further Resources
Hopefully this tutorial provides you with the basic understanding of how to use the GIN client and familiarises you with the most common commands that you are going to use when managing your repositories. For a more extensive overview see our [GIN client documentation](doc-gin-client). If you are keen on becoming trully profficient with GIN, try out these external resources:
- [Local GIN client - Usage Guide](https://gin.g-node.org/G-Node/Info/wiki/GIN+CLI+Usage+Tutorial)
- [Local GIN client - Useful recipes and short workflows](https://gin.g-node.org/G-Node/Info/wiki/GIN+CLI+Recipes)
- [GIN CLI configuration](https://gin.g-node.org/G-Node/Info/wiki/GIN+CLI+Config)
- [GIN detailed command overview](https://gin.g-node.org/G-Node/Info/wiki/GIN+CLI+Help)

And if you need to refresh your memory about some GIN client command, just type in your terminal:
```
gin help <command-name>
```