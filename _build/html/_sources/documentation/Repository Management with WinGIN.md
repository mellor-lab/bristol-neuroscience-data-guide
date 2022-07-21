(doc-wingin)=
# Repository Management with WinGIN
This is the documentation on how to use WinGIN which is the graphical user interface for managing files on a GIN server for Windows operating system users. You can use it to manage files on Bristol GIN and on the public GIN server. It is a frontend to the GIN Client. For installation instructions refer to [WinGIN Installation](doc-install-wingin).

(doc-wingin-login)=
## Login to your Account
In order to login to GIN, you have to have a registered account on the [local GIN server](https://www.bristol.ac.uk/bristolgin/) or on the [public GIN server](https://gin.g-node.org/). Instructions on how to do it are available [here](doc-gin-web-register). You login by launching the GinClientApp and entering your account credentials.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig01-login.png
:name: fig-wingin-login
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Login to your Account**

When loging in for the first time, Gin Server Alias has only a single option pointing to the public GIN server. You can [add another GIN server](doc-wingin-configure-server) later.

(doc-wingin-global-options)=
## Set Global Options
Once you are logged into your GIN account, you can view global options by clicking on the Global Options pane.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig02-global-options.png
:name: fig-wingin-global-options
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Global Options**

Global options allows you to set the frequency of checking for **repository updates**. You can chose between 5, 15 (default), 30, and 60 minutes or disable repository update checkup by choosing the 'never' option.
```{warning}
If you choose any of the update options other than 'never', your local repositories will be updated automatically every now and then if WinGIN is running in the background. This may potentially be unwanted. It is, therefore, safer to disable automatic update checkup.
```

You also have an option to set your **checkout directory**. The checkout directory is where the downloaded repository files are kept. You are not supposed to edit files in this location. It is advised to keep the default `C:\Users\<username>\AppData\Roaming\g-node\GinWindowsClient\Repositories` location as a checkout directory, unless you are in risk of running out of space on the C: drive. The checkout directory should not be easily accessible.

The **mountpoint directory** is your repository working space. This is the location where you work with your repository files: Add them, remove, or edit. Any changes you make in the mountpoint directory will be reflected in the checkout directory. Contrary to the checkout directory, the mountpoint directory should be easily accessible.

Turning on **Download all Annex'ed Data** will download all files upon checkout from the repository. If this option is turned off, only the repository folder structure containing file pointers will be downloaded.

Clicking **Help** will open the GIN repository page containing all GIN info files.

(doc-wingin-main-window)=
## Bring up the Main Window
You can close the WinGIN window at any time without closing the app. WinGIN will still be running in the background and can be located in the taskbar. You can relaunch the main window by double-clicking the blue GIN icon or right-clicking the icon and chosing one of the three window panes to open. You can close the WinGIN app by right-clicking the icon in the taskbar and chosing the Exit option.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig03-locate-wingin.png
:name: fig-wingin-locate
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 3. Locating WinGIN in the Taskbar**

(doc-wingin-configure-server)=
## Configure a GIN Server
You can add a new server by opening the Login pane in the main window and clicking the Add Server button located on the right side of the window.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig04-add-server.png
:name: fig-wingin-add-server
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 4. Add a New Server**

You should be able to see the Add Gin Server window. In order to add the Bristol GIN server specify Server Alias as bristol-gin. In the web part of configuration set the protocol to https. Hostname should be set to https://www.bristol.ac.uk/bristolgin, while for the port type in 2121. In the Git part of configuration the user name should be set to git. Hostname should be set to git@bristol.ac.uk/bristolgin bristol-gin, while port should be chosen as 22. Once you enter all required configuration information, press the Save button to add a new server.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig05-configure-server.png
:name: fig-wingin-configure-server
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 5. Configure a Server**

(doc-wingin-show-servers)=
## Show Configured Servers
If you want to see the list of servers that your GinClientApp is configured to work with, press the dropdown arrow to the right of the current server in the Login pane
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig06-show-servers.png
:name: fig-wingin-show-servers
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 6. Show Configured Servers**

(doc-wingin-change-server)=
## Change the Default Server
If you want to change the default server that your GinClientApp works with, press the Set Default button in the Login pane
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig07-change-server.png
:name: fig-wingin-change-server
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 7. Set your Default Server**

(doc-wingin-create-repo)=
## Create a Bare Repository
In order to create an empty repository, open the repositories pane in the main WinGIN window and click the Create New button located at the bottom of the window.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig08-create-repo.png
:name: fig-wingin-create-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 8. Request to Create a New Repository**

You should then be prompted to enter the new repository details as below.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig09-create-repo2.png
:name: fig-wingin-enter-repo-details
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 9. Enter Repository Details**

After clicking OK button the new repository should appear in your repositories workspace as indicated by an arrow below.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig10-create-repo3.png
:name: fig-wingin-repo-created
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 10. New Repository**

The new repository should appear on the GIN server, as well as a new folder should be created in your checkout and mountpoint directories.

(doc-wingin-delete-repo)=
## Delete a Local Repository
In order to delete the local instance of a repository, go to the Repositories pane in the main WinGIN window and click the Remove button located at the bottom of the window.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig11-delete-repo.png
:name: fig-wingin-delete-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 11. Delete a Repository**

(doc-wingin-update-repo)=
## Update a Remote Repository (Upload Files)
In order to update the remote repository with changes that were made in the local repository or to upload files to a remote repository, go to the Windows taskbar, right-click on the blue GIN icon, choose the repository you want to upload files to, and click on the Upload changes option.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig12-upload-repo.png
:name: fig-wingin-upload-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 12. Update a Remote Repository**

You should then see the Files to upload window showing all files to be updated/uploaded to the repository. Write down a succinct commit message at the bottom of the window. If it is an initial commit, it would suffice to say so. Once you click OK, the remote repository should be synchronised with the local one.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig13-upload-repo2.png
:name: fig-wingin-upload-repo-commit
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 13. Commit Changes to a Remote Repository**

Alternatively, you can right-click a file, go to the Gin Repository option and click on Upload file like it is shown below.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig14-upload-file.png
:name: fig-wingin-upload-file
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 14. Upload a File to a Remote Repository**

(doc-wingin-remove-content)=
## Remove the Content of Local Files
If your remote and local repositories are synchronised, you may want to remove the local content to free storage space. In order to do so, right-click the file of interest, then go to Gin Repository option and further choose the Remove local content option.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig15-remove-content.png
:name: fig-wingin-remove-content
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 15. Remove Local Content**

(doc-wingin-dowload-repo)=
## Download (Clone) a Remote Repository
In order to download a copy (clone) of a full remote repository onto your local machine, go to the Repositories pane in the main WinGIN window and click the Checkout button in the bottom left corner of the window.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig16-download-repo.png
:name: fig-wingin-download-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 16. Request to Download a Repository**

Then specify details of an existing remote repository as in the example below.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig17-download-repo2.png
:name: fig-wingin-download-repo-details
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 17. Specify Details of a Repository to Download**

The requested repository is then downloaded to your checkout directory and also should appear among your managed repositories as indicated by an arrow in the image below.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig18-download-repo3.png
:name: fig-wingin-download-repo-complete
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 18. Remote Repository Successfully Downloaded**
```{note}
Repository checkout does not download full files but only file pointers, unless you turned on the Download all Annex'ed Data option in the Global Options pane. To get full files, double click the file of interest.
```

(doc-wingin-update-local-repo-remote-content)=
## Update a Local Repository with Remote Changes
In order to update the local repository, go to the Windows taskbar, right-click the blue GIN icon, go to the repository of interest and select the Update option.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig19-update-local-repo.png
:name: fig-wingin-update-local-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 19. Update your Local Repository**

You should be notified that your repository is up to date after updating completes.
```{note}
Repository updating does not download full files but only file pointers, unless you turned on the Download all Annex'ed Data option in the Global Options pane. To get full files, double click the file of interest.
```

(doc-wingin-update-local-files-remote-content)=
## Update Local Files with Remote Content
If you want to update a specific file with remote content or if you want to restore the content to a local file with removed content, double-click the file of interest. Alternatively, you can right-click the file of interest, then select the Gin Repository option and further choose the Download File option.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig20-download-content.png
:name: fig-wingin-download-content
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 20. Download File Content**

(doc-gin-client-rollback-file)=
## Roll back a File to an Earlier Version
In order to restore a file to an earlier version, right-click the file of interest, go to the Gin Repository option and further choose Get older version option.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig21-roll-back-version.png
:name: fig-wingin-roll-back-version
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 21. Roll back a File to its Previous Version**

This should open the Select previous version window. Choose the version you would like to go back to and press the Restore button.
```{image} ../assets/images/documentation/repository-management-with-wingin/Fig22-select-version.png
:name: fig-wingin-select-version
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 22. Select the Version you Want to Roll Back to**

The older file is restored in the same folder as the new one, but with the date in the file name: "file name"-"date"
```{warning}
If there is another file with the same name in the form "file name"-"date" in the same folder, the file will be overwritten.
```