(tutorials-pclamp)=
# Bristol GIN for Patch Clamp Data

(tutorials-pclamp-create-account)=
## Create GIN Account
In order to be able to follow this tutorial, you need to have a GIN account either on Bristol GIN or on the public GIN service. GIN web interface documentation has it all explained [here](doc-gin-web-register).

After creating the account, login to it by typing:
```
gin login
```

(tutorials-pclamp-download-data)=
## Download Mock Data
We are going to download intracellular electrophysiology recording data available in [dervinism/icephys_mock_repo](https://gin.g-node.org/dervinism/icephys_mock_repo) repository. To get the data, type in the lines below to your terminal:
```
gin get dervinism/icephys_mock_repo
cd icephys_mock_repo
gin get-content
```
This is part of the real intracellular electrophysiology recording data. The data has been recorded in pyramidal cells in the CA1 region of the hippocampus in vitro. AMPA and NMDA receptors were blocked using NBQX and AP5, respectively. During the recording a stimulating electrode was also placed in the pyramidal layer of the hippocampus roughly 100 um away from the recording site. In addition to the electrical stimulation, optogenetic stimulation was used to excite parvalbumin (PV) expressing interneurons in the pyramidal layer using channelrhodopsin. Both electrical stimulation and optogenetic stimulation were applied interchangeably every 5 seconds using two 5-millisecond light pulses or two action potentials separated by 50 milliseconds while the cell was voltage-clamped (the baseline stimulation regime). Then the cell was switched to the current clamp and the neural plasticity inducing protocal was applied. The plasticity protocol consisted of a burst of 4 action potentials paired with a 5-millisecond light pulse applied at the onset of the first action potential. The plasticity inducing stimulation was applied 100 times every 200 milliseconds. Then the cell was switched back to the voltage clamp and the baseline stimulation regime was resumed. The experiment is described in more detail in the README file.

The data inside the repository is organised based on recording sessions and has the following structure: ```recording-session-date/brain-slice-number/cell-number```. Each recording session has a lab book file containing comments relevant for that particular recording session. Each cell recording has:
- An image of the recorded slice.
- A CSF file containing raw trace data exported from the aquisition CED Signal software.
- A MAT file containing exported version of the CSF file readable in Matlab. Only the traces relevant for the experiment are stored in this file.

(tutorials-pclamp-setup-repo)=
## Set up Your Research Data Repository
You should rename your repository, delete the .git folder located inside the repository root folder, and update your README file if needed. Once this is done, issue the command below:
```
gin init
```
This would intialise the local repository.

(tutorials-pclamp-upload-repo)=
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
```{image} ../assets/images/tutorials/bristol-gin-for-pclamp-data/Fig01-create-repo1.png
:name: fig-gin-pclamp-create-repo1
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Create Repository Using Web Interface**

You should see the New Repository window where you enter the name of the repository and its description as shown below.
```{image} ../assets/images/tutorials/bristol-gin-for-pclamp-data/Fig02-create-repo2.png
:name: fig-gin-pclamp-create-repo2
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Enter Repository Details Using Web Interface**

Finally, you can upload files by clicking the blue Upload file button on the repository page.
```{image} ../assets/images/tutorials/bristol-gin-for-pclamp-data/Fig03-upload-repo.png
:name: fig-gin-pclamp-upload-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 3. Upload Repository Files Using Web Interface**

The limitation of using the web interface is that every time you update your remote repository (upload files), you will be limited to uploading 100 files at a time with each file being no larger than 10 gigabytes. Therefore, it is more effecient/effective to use the command line tools which have none of these limitations.

When you use the web interface, you can specify the commit message title (no more than 50 characters by convention) and the commit message body (no more than 72 characters by convention).

(tutorials-pclamp-remove-content)=
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
gin remove-content '180126/Slice1/Cell1/180126 s1c1.jpg'
gin remove-content 180126/Slice1/Cell1/180126__s1c1_001.cfs
```
To simply restore the file content type in
```
gin get-content
```
If you no longer need to work on your repository and its remote copy is up to date with the local copy, you can simply delete the local repository copy altogether. You should always be able to restore your repository and all of its contents on your local machine by executing these commands in your terminal (replace the repository path as appropriate):
```
gin get dervinism/icepys_mock_repo
cd icepys_mock_repo
gin get-content
```

(tutorials-pclamp-convert2nwb)=
## Convert Your Data to Standardised Format
In order to increase your research data's adherence to the [FAIR principles](https://www.go-fair.org/fair-principles/) of scientific data management and, in particular, to increase the interoperability of your data and chances of it being reused beyond its original purpose of collection, it is highly advisable to convert your data into one of the more popular standard file formats for neuroscience data. One such format is the [Neurodata Without Borders (NWB)](https://www.nwb.org/) which is highly suitable for most of the neurophysiology data. Programming interfaces in both Matlab and Python are available for converting your data. Here we are going to provide explanations of how you can convert your data in both programming languages. While showing you examples of that, we will continue our focus on the intracellular electrophysiology data.

(tutorials-pclamp-convert2nwb-matlab)=
### Convert to NWB Using Matlab
(tutorials-pclamp-convert2nwb-matlab-install-matnwb)=
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

(tutorials-pclamp-convert2nwb-matlab-record-metadata)=
#### Record Metadata
Inside the intracellular electrophysiology repository there is a folder with the Matlab file that you can execute in order to convert the data of one particular imaging/recording session into the NWB format. The script is located inside the ```180126/Slice1/Cell1/180126/nwb``` folder in the file named ```convert2nwbpClamp.m``` and, if executed, it would convert the data stored in the file ```180126/Slice1/Cell1/180126__s1c1_001_ED.mat```.

Initially, we start by recording the metadata associated with this experimental session. In this tutorial the metadata is divided into three types: Project, animal, and session metadata. The project metadata is common to all animals and experimental sessions and is defined by the part of the script below:
```

```

```{note}
This section is work in progress. Current priority!
```