(tutorials-gui)=
# Bristol GIN via Graphical Interfaces

(tutorials-gui-aims)=
## Aims
The aim of this tutorial is to give you an introduction to using the GIN web interface. For this tutorial you will need to download a [mock repository](https://gin.g-node.org/dervinism/mockRepository).

In this tutorial you will learn how to:
1. Create a repository
2. Upload files to a repository
3. Download files from a repository onto your computer
4. Transfer repository ownership to your organisation
5. Create a collaborating team
6. Assign the repository to the team
7. Add members to the team

(tutorials-gui-create-account)=
## Create GIN Account
For this tutorial you can either use our local [Bristol GIN](https://bristol.ac.uk/bristolgin/) or the [public GIN](https://gin.g-node.org/): the two work the same. We start by registering an account with the GIN. You just need to follow the instructions on the screen, confirm your email, and your account is set up ([Figure 1](fig-gin-gui-register-account)).
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/register-account.png
:name: fig-gin-gui-register-account
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Register your Account**

Once the registration is done, you should have an account open as shown in the figure below.
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/fresh-account.png
:name: fig-gin-gui-fresh-account
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Fresh Account**

(tutorials-gui-create-repo)=
## Create Repository
The next step is to create a repository.
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/create-repo.png
:name: fig-gin-gui-create-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 3. Create Repository**

I am going to use mock Neuropixels data acquired during a single recording session using two silicon probes. Below is an example ([Figure 3](fig-gin-gui-create-repo)) showing how to enter basic data describing a repository. I am not going to enter anything in the gitignore entry but one can indicate which files and folders of the local instance of the repository should not be synchronised with the remote repository by providing a [gitignore file](https://git-scm.com/docs/gitignore).

By default the repository is private but it can be changed to public at any time (not yet true for Bristol GIN). You can also add collaborators at a later stage. Once the repository is created you should see the page below.
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/fresh-repo.png
:name: fig-gin-gui-fresh-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 4. Fresh Repository**

(tutorials-gui-repo-settings)=
## Repository Settings
Repository properties can be modified via the Settings pane ([Figure 5](fig-gin-gui-settings)). Figure below shows part of available settings. More advanced settings can be set up (revealed if you scroll down), as well as collaborators can be added to the repository who can be granted full or restricted access rights ([Figure 6](fig-gin-gui-collaborations)).
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/settings.png
:name: fig-gin-gui-settings
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 5. Repository Settings**
<br/><br/>

```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/collaborations.png
:name: fig-gin-gui-collaborations
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 6. Setting up Collaborations**

(tutorials-gui-upload-files)=
## Uplpoad Research Data Files
When we have a repository created and settings adjusted, the next step is to upload research data files. I have created a simple folder structure containing mock data that I would typically generate during a single recording session using two Neuropixels probes and a pupil camera but which also include pre-processing data generated during spike sorting with [Kilosort](https://github.com/MouseLand/Kilosort) and [Phy](https://github.com/cortex-lab/phy). The files can be uploaded using the blue ‘upload file’ button ([Figure 4](fig-gin-gui-fresh-repo)) which brings the ‘Commit changes’ page ([Figure 7](fig-gin-gui-upload-files)). The files can then be uploaded by clicking the area marked by the blue dashed line or by dragging files or folders to that area. Note that the web interface limits the ammount and the size of files that can be uploaded. The proper more efficient way to manage multiple and large files is to use the [GIN command line tools](doc-gin-client) which are covered in [the next tutorial](tutorials-cli).
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/upload-files.png
:name: fig-gin-gui-upload-files
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 7. File Upload Page**

In my case I simply drag-and-drop the contents of the repository root folder. This mock research data folder contains the following structure: \
```mock_repository/mock_experiment/animal_ID/session1_2022-05-16/probe1``` (all data pertaining to the recording probe 1) \
```mock_repository/mock_experiment/animal_ID/session1_2022-05-16/probe2``` (all data pertaining to the recording probe 2) \
```mock_repository/mock_experiment/animal_ID/session1_2022-05-16``` (data that is related to both recording probes like pupil video, for example). \
I also add a commit message that succinctly describes the change that I am introducing to the repository.
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/commit.png
:name: fig-gin-gui-commit
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 8. Commit Files to Repository**

There are more than 100 files in this repository and so not all files could be uploaded at once. As we can see in the figure above, the very last files are crossed out to indicate that they cannot be uploaded. Therefore, we need to repeat this procedure with these remaining files making sure that the entire repository is fully uploaded.

(tutorials-gui-download-files)=
## Downlpoad Research Data Repositories and Files
The repository now contains files which can be downloaded by your collaborators or by yourself, if you lose access to your local instance of the repository. Just press the download button indicated in [Figure 9](fig-gin-gui-download-repo) to download the entire repository. Files larger than 10 megabytes will not be downloaded and only pointers will. Big files have to be downloaded individually. Alternatively, large files can be downloaded using [GIN client](doc-gin-client) or [git-annex](https://git-annex.branchable.com/) (command line tools). To download files individually, simply locate the file within the repository and click the Download button to the right of the file of interest ([Figure 10](fig-gin-gui-download-files)). Similarly, you can delete a file by clicking the button with the bin symbol to the right of the Download button. If the file is a text file it can also be edited by pressing the button with the pen symbol. GIN allows creating new text files by pressing the blue New file button ([Figure 9](fig-gin-gui-download-repo)). If you give an .md (Markdown) extension to the text file, it will allow you to use rich text editing tools just like in Github. These text editing tools are particularly useful when creating README files. Markdown text editing tools were used to create this tutorial.
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/download-repo.png
:name: fig-gin-gui-download-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 9. Download Repository**
<br/><br/>

```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/download-files.png
:name: fig-gin-gui-download-files
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 10. Download Individual Files**

(tutorials-gui-transfer-ownership)=
## Transfer Repository Ownership
Non-text files can be modified by uploading a new version of the repository files. In this way the old files are not only being replaced by the new ones, but changes are being tracked and the entire history of repository modifications is being preserved. Furthermore, you can create new repositories, add another organisation and associate the repository with a particular organisation. When I created my account, I indicated my affiliation with the University of Bristol (UoB). Now I am going to transfer the ownership of this repository to UoB. In the repository settings ([Figure 5](fig-gin-gui-settings)) I scroll down to the very bottom of the page passing the Advanced Settings all the way down to the Danger Zone where I carry out the transfer of ownership ([Figure 11](fig-gin-gui-danger-zone)).
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/danger-zone.png
:name: fig-gin-gui-danger-zone
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 11. Danger Zone**

Once the ownership transfer has been carried out, the repository name should become associated with the new owner: UoB ([Figure 12](fig-gin-gui-repo-name)).
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/repo-name.png
:name: fig-gin-gui-repo-name
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 12. Transferring Repository Ownership**

```{warning}
If you are transfering the ownership of your repository to an organisation, make sure that you have admin rights in that organisation. Even if you think this is the case, it is worth double-checking this. You may have given a name to your organisation that has already been taken and you may end up not having such rights. In such a scenario you may end up inadvertently losing access to your own repository. Be extremely careful in the Danger Zone!
```

(tutorials-gui-create-team)=
## Create Teams
If you go to the Dashboard of your account (see the top left corner of the [Figure 12](fig-gin-gui-repo-name)), you can view activity that is associated with your account. There you can also see teams and organisations associated with the repositories you work with. If you click on the organisation that is associated with one of your repositories ([Figure 13](fig-gin-gui-dashboard)), you will be taken to the organisation’s page ([Figure 14](fig-gin-gui-org-page)) where you can create new teams of users. [Figure 15](fig-gin-gui-create-team) shows an example of creating a team that works with Neuropixels recordings: carrying out experiments using Neuropixels probes and writing code to analyse data generated using these probes. As one of the final steps, you associate the team with a repository which would give anyone within that team elevated rights to manipulate the repository as indicated in the [Figure 16](fig-gin-gui-associate-repo2team). If you press the blue Join button, you become a member of this team.
```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/dashboard.png
:name: fig-gin-gui-dashboard
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 13. Finding your Organisation in Dashboard**
<br/><br/>

```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/org-page.png
:name: fig-gin-gui-org-page
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 14. Organisation Page**
<br/><br/>

```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/create-team.png
:name: fig-gin-gui-create-team
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 15. Create Team**
<br/><br/>

```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/associate-repo2team.png
:name: fig-gin-gui-associate-repo2team
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 16. Assign Repository to Team**

(tutorials-gui-add-team-member)=
## Add Team Members
The team that you have created currently has no members. By pressing the blue Join button [Figure 16](fig-gin-gui-associate-repo2team), you can be the first member to join the team. You can also invite other members of your lab or external collaborators to join the team. If you press on the blue members text, you will be prompted to add new team members ([Figure 17](fig-gin-gui-add-team-member)). Choose another member of your lab to add as a collaborating team member. The new team member will have access and elevated rights to control the repository.

```{image} ../assets/images/tutorials/bristol-gin-via-graphical-interfaces/add-team-member.png
:name: fig-gin-gui-add-team-member
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 17. Add New Team Meambers**

Any issues relating to a particular repository can be reported under the Issues tab in the repository page (Figures [4](fig-gin-gui-fresh-repo), [9](fig-gin-gui-download-repo), and [12](fig-gin-gui-repo-name)). Any requests to make changes to the repository can be approved under the Pull Requests tab (Figures [4](fig-gin-gui-fresh-repo), [9](fig-gin-gui-download-repo), and [12](fig-gin-gui-repo-name)). A more extended description of how to use the GIN server web interface is available at the [GIN web interface guide](https://gin.g-node.org/G-Node/Info/wiki/Web+Interface). There is also [a Windows GIN client](doc-wingin) available that functions as a GIN GUI for Windows systems. An external [WinGIN tutorial](https://gin.g-node.org/G-Node/Info/wiki/WinGIN+Tutorial) provides additional useful guidance if you are interested in using it.