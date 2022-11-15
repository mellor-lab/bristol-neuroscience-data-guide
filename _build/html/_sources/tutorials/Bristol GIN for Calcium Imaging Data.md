(tutorials-caimage)=
# Bristol GIN for Calcium Imaging Data

(tutorials-caimage-create-account)=
## Create GIN Account
In order to be able to follow this tutorial, you need to have a GIN account either on Bristol GIN or on the public GIN service. GIN web interface documentation has it all explained [here](doc-gin-web-register).

After creating the account, login to it by typing:
```
gin login
```

(tutorials-caimage-download-data)=
## Download Mock Data
We are going to download Ca2+ imaging data available in dervinism/calcium-imaging-mock-repo repository. To get the data, type in the lines below to your terminal:
```
gin get dervinism/calcium-imaging-mock-repo
cd calcium-imaging-mock-repo
gin get-content
```
This is part of the real Ca2+ imaging data. The imaging was performed in hippocampal pyramidal cells during Schaffer collateral stimulation. Dendrites at different cell locations were two-photon linescan imaged. More about the project that this data is coming from can be read in the README file. The data is organised into individual recording sessions named using number IDs. It is further subdivided into slices and cells. Processed data is places in the Analysed folder inside the MAT file. The raw data is located in two separate folders: Ephysdata for electrophysiology data and Imgdata for imaging data. README files located in calcium-imaging-mock-repo\Exp_001_SCstim_DiffLocations\201204 session folder give more detail about what sort of data is stored, variable names and their meaning, and other relevant information.

(tutorials-caimage-setup-repo)=
## Set up Your Research Data Repository
You should rename your repository, delete the .git folder located inside the repository root folder, and update your README file if needed. Once this is done, issue the command below:
```
gin init
```
This would intialise the local repository.

(tutorials-caimage-upload-repo)=
## Upload Repository
The next step is to create a remote repository with the same name and associate it with the local repository. You can simply do it by typing in your terminal:
```
gin add-remote your-repo-name gin:your-username/your-repo-name --default
```
In this case, the remote repository is going to be created since it does not exist. You can now type:
```
gin commit . -m "Initial commit"
gin upload .
```
The first command would record local changes with the local version control system. By doing so you create the image of your repository that can always be reverted to in the future in case the need to do so arises. When commiting local repository changes to your version control system you typically provide a concise message describing the changes. By convention, the message length should not exceed 50 characters. Dot means that the command is executed on the contents of the current working directory; therefore, make sure that you are inside the root folder of your repository when carrying out this action. The flag -m is used to pass the commit message. When you make new changes to the repository, whether editing text files or manipulating your data files, you should commit these changes periodically to your local versioning system. Finally, the ```upload``` command updates the remote instance of the repository with the changes that were commited locally up to this point.

Alternatively, you can create a repository using the GIN web interface. You do it by clicking the plus sign in the top menu and then clicking and chosing the New Repository option.
```{image} ../assets/images/tutorials/bristol-gin-for-calcium-imaging-data/Fig01-create-repo1.png
:name: fig-gin-caimage-create-repo1
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Create Repository Using Web Interface**

You should see the New Repository window where you enter the name of the repository and its description as shown below.
```{image} ../assets/images/tutorials/bristol-gin-for-calcium-imaging-data/Fig02-create-repo2.png
:name: fig-gin-caimage-create-repo2
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Enter Repository Details Using Web Interface**

Finally, you can upload files by clicking the blue Upload file button on the repository page.
```{image} ../assets/images/tutorials/bristol-gin-for-calcium-imaging-data/Fig03-upload-repo.png
:name: fig-gin-caimage-upload-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 3. Upload Reposiotry Files Using Web Interface**

The limitation of using the web interface is that every time you update your remote repository (upload files), you will be limited to uploading 100 files at a time with each file being no larger than 10 gigabytes. Therefore, it is more effecient/effective to use the command line tools which have none of these limitations.

When you use the web interface, you can specify the commit message title (no more than 50 characters by convention) and the commit message body (no more than 72 characters by convention).

(tutorials-caimage-remove-content)=
## Remove Content of Your Local Research Data Repository
One advantage of using GIN for your data repository mangement is that you do not need to keep duplicate repositories in order to prevent accidental detrimental changes to your main repository. One reason for that is having version control system. The other reason is that you can safely remove the content of your local repository and replace it with pointers to original files. As a result you can save space on your local hard-drive. To remove the local content type the following line in your terminal:
```
gin remove-content
```
Local files larger than 10 megabytes should be replaced by symbolic links. In case you want to remove the content of specific files only, you can type:
```
gin remove-content <absolute or relative path to file or folder>
```
For example, to remove particular raw research data from our calcium imaging data repository, we type:
```
gin remove-content Exp_001_SCstim_DiffLocations/201204/Slice_002/Cell_001/Ephysdata
gin remove-content Exp_001_SCstim_DiffLocations/201204/Slice_002/Cell_001/Imgdata
```
To simply restore the file content type in
```
gin get-content
```
If you no longer need to work on your repository and its remote copy is up to date with the local copy, you can simply delete the local repository copy altogether. You should always be able to restore your repository and all of its contents on your local machine by executing these commands in your terminal (replace the repository path as appropriate):
```
gin get dervinism/calcium-imaging-mock-repo
cd calcium-imaging-mock-repo
gin get-content
```

(tutorials-caimage-convert2nwb)=
## Convert Your Data to Standardised Format
In order to increase your research data's adherence to the [FAIR principles](https://www.go-fair.org/fair-principles/) of scientific data management and, in particular, to increase the interoperability of your data and chances of it being reused beyond its original purpose of collection, it is highly advisable to convert your data into one of the more popular standard file formats for neuroscience data. One such format is the [Neurodata Without Borders (NWB)](https://www.nwb.org/) which is highly suitable for most of the neurophysiology data. Programming interfaces in both Matlab and Python are available for converting your data. Here we are going to provide explanations of how you can convert your data in both programming languages. While showing you examples of that, we will continue our focus on the calcium imaging data.

(tutorials-caimage-convert2nwb-matlab)=
### Convert to NWB Using Matlab
(tutorials-caimage-convert2nwb-matlab-install-matnwb)=
#### Install MatNWB Toolbox
To begin with the Matlab demonstration, you would need to install [MatNWB toolbox](https://neurodatawithoutborders.github.io/matnwb/). To download the toolbox, type in your terminal:
```
git clone https://github.com/NeurodataWithoutBorders/matnwb.git
```
Move the downloaded repository to the folder where you keep your code libraries. Then type the following in the Matlab command line:
```
cd matnwb
addpath(genpath(pwd));
generateCore();
```
You can now start using MatNWB. MatNWB interface documentation can be accessed [here](https://neurodatawithoutborders.github.io/matnwb/doc/index.html).

(tutorials-caimage-convert2nwb-matlab-record-metadata)=
#### Record Metadata

```{note}
This section is work in progress. Current priority!
```