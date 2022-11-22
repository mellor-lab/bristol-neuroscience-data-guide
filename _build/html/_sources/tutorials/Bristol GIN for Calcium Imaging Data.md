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
This is part of the real Ca2+ imaging data. The imaging was performed in hippocampal pyramidal cells during Schaffer collateral stimulation. Dendrites at different cell locations were two-photon linescan imaged. More about the project that this data is coming from can be read in the README file. The data is organised into individual imaging/recording sessions named using number IDs. It is further subdivided into slices and cells. Processed data is places in the Analysed folder inside the MAT file. The raw data is located in two separate folders: Ephysdata for electrophysiology data and Imgdata for imaging data. README files located in calcium-imaging-mock-repo\Exp_001_SCstim_DiffLocations\201204 session folder give more detail about what sort of data is stored, variable names and their meaning, and other relevant information.

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
Inside the calcium imaging repository there is a folder with the Matlab file that you can execute in order to convert the data of one particular imaging/recording session into the NWB format. The script is located inside the ```Exp_001_SCstim_DiffLocations/201204/Slice_00/Cell_001/nwb``` folder in the file named ```convert2nwbCaImg.m``` and, if executed, it would convert the data stored in the files below:
```
Exp_001_SCstim_DiffLocations/201204/Slice_002/Cell_001/Analysed/201204__s2d1_004_ED__1 Botden_Analysed.mat
Exp_001_SCstim_DiffLocations/201204/Slice_002/Cell_001/Analysed/201204__s2d1_004_ED__1 Midden_Analysed.mat
Exp_001_SCstim_DiffLocations/201204/Slice_002/Cell_001/Analysed/201204__s2d1_004_ED__1 Topden_Analysed.mat
```

Initially, we start by recording the metadata associated with this experimental session. In this tutorial the metadata is divided into three types: Project, animal, and session metadata. The project metadata is common to all animals and experimental sessions and is defined by the part of the script below:
```
projectName = 'Intracellular Ca2+ dynamics during plateau potentials trigerred by Schaffer collateral stimulation';
experimenter = 'Matt Udakis';
institution = 'University of Bristol';
publications = 'In preparation';
lab = 'Jack Mellor lab';
brainArea = 'Hippocampus CA1-2';
greenIndicator = 'Fluo5f';
redIndicator = 'Alexa594';
```
The names of most of these parameters are self-explanatory. The green and red indicators are calcium indicator types named based on the light wavelength they emit. Next we define animal metadata. The reason to have this type of data separate is that multiple slices can be obtained from the same animal and used in separate imaging/recording sessions. The animal metadata is defined in the code snippet below:
```
animalID = 'm1';
ageInDays = 100;
age = ['P' num2str(ageInDays) 'D']; % Convert to ISO8601 format: https://en.wikipedia.org/wiki/ISO_8601#Durations
strain = 'C57BL/6J';
sex = 'M';
species = 'Mus musculus';
weight = [];
description = '001'; % Animal testing order.
```
Names of these parameters are again self-explanatory. Finally, we define the subject level metadata in the code below:
```
startYear = 2020;
startMonth = 12;
startDay = 4;
startTime = datetime(startYear, startMonth, startDay);
...
sliceNumber = 2;
cellNumber = 1;
imagingRate = 1/21; % A single linescan duration is 1sec with 20sec period in between linescans
lineRate = 1000.; % A number of lines scanned in a second.
sessionID = [animalID '_' year month day '_s' num2str(sliceNumber) ...
  '_c' num2str(cellNumber)]; % mouse-id_time_slice-id_cell-id
sessionDescription = 'Single cell imaging in a slice combined with somatic current clamp recordings and the stimulation of Schaffer collaterals';
sessionNotes = ['201204 - Slice 2 Imaging 3 dendrite regions roughly in the SO SR and SLM regions   ' ...
                'Same line scan with same intensity of stim (2.3V) at different locations along the cell   ' ...
                'Ephys frames to match up with linescans   ' ...
                'Frames   ' ...
                '1-8 Bottom Den   ' ...
                '10-19 Middle Den   ' ...
                '22-28 Top Dendrite   ' ...
                'Missed a few of the imaging with the ephys so more Ephys traces than linescans   ' ...
                'by the end of the experiment the top or neuron started to bleb.'];
```
Again, these variables are mostly self-explanatory or commented in the code. The sessionNotes variable is coming from the ```Exp_001_SCstim_DiffLocations/201204/Slice_002/Cell_001/labbbook.txt``` file.

The way you define your metadata may be different. For example, you may have your own custom scripts that contain the metadata or you may store your metadata in files organised according to one of standard neuroscientific metadata formats like [odML](http://g-node.github.io/python-odml/). Whichever your preference is, this part of the NWB conversion procedure will vary depending on the individual researcher.

The initialisation process is completed by intialising the Matlab NWB classes by calling
```
generateCore;
```
We start the conversion process by creating an [```NWBFile```](https://neurodatawithoutborders.github.io/matnwb/doc/NwbFile.html) object and defining session metadata:
```
% Assign NWB file fields
nwb = NwbFile( ...
  'session_description', sessionDescription, ...
  'identifier', sessionID, ...
  'session_start_time', startTime, ...
  'general_experimenter', experimenter, ... % optional
  'general_session_id', sessionID, ... % optional
  'general_institution', institution, ... % optional
  'general_related_publications', publications, ... % optional
  'general_notes', sessionNotes, ... % optional
  'general_lab', lab); % optional
```
Each file must have a ```session_description``` identifier and a ```session_start_time``` parameter. We then initialise the [```Subject```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/Subject.html) object and the metadata it contains:
```
% Create subject object
subject = types.core.Subject( ...
  'subject_id', animalID, ...
  'age', age, ...
  'description', description, ...
  'species', species, ...
  'sex', sex);
nwb.general_subject = subject;
```

(tutorials-caimage-convert2nwb-matlab-create_img_planes)=
#### Create Imaging Planes
In order to store optical imaging data, we need to create imaging planes. First, we create [```OpticalChannel```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/OpticalChannel.html) objects for each calcium indicator we use. The objects contain ```description``` and ```emission_lambda``` properties. In our case we use indicators Fluo5f and Alexa594 which have their maximum emission wavelengths at approximately 516 and 616 nanometers, respectively. These wavelength roughly correspond to green and red colours. Therefore, we name the channels based on the colours their corresponding calcium indicators emit.
```
green_optical_channel = types.core.OpticalChannel( ...
  'description', ['green channel corresponding to ' greenIndicator], ...
  'emission_lambda', 516.);

red_optical_channel = types.core.OpticalChannel( ...
  'description', ['red channel corresponding to ' redIndicator], ...
  'emission_lambda', 616.);
```
Another needed prerequisite is the [```Device```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/Device.html) object which is stored within the ```NWBFile``` object. Idealy, the ```Device``` object should have a ```description``` and ```manufacturer``` properties as defined below:
```
% Create the imaging device object
device = types.core.Device(...
  'description', 'Two-photon microscope', ...
  'manufacturer', 'Scientifica');
nwb.general_devices.set('2P_microscope', device);
```
We name the device as 2P_microscope but you are free to name it differently. Having created ```OpticalChannel``` and ```Device``` objects we can now create optical imaging planes corresponding to the green and red optical channels. We do this by executing the code below:
```
% Create imaging plane objects
imaging_plane_name = 'green_imaging_plane';
green_imaging_plane = types.core.ImagingPlane( ...
  'optical_channel', green_optical_channel, ...
  'description', 'The plane for imaging calcium indicator Fluo5f.', ...
  'device', types.untyped.SoftLink(device), ...
  'excitation_lambda', 810., ...
  'imaging_rate', imagingRate, ...
  'indicator', 'Fluo5f', ...
  'location', brainArea);
nwb.general_optophysiology.set(imaging_plane_name, green_imaging_plane);

imaging_plane_name = 'red_imaging_plane';
red_imaging_plane = types.core.ImagingPlane( ...
  'optical_channel', red_optical_channel, ...
  'description', 'The plane for imaging calcium indicator Alexa594.', ...
  'device', types.untyped.SoftLink(device), ...
  'excitation_lambda', 810., ...
  'imaging_rate', imagingRate, ...
  'indicator', 'Alexa594', ...
  'location', brainArea);
nwb.general_optophysiology.set(imaging_plane_name, red_imaging_plane);
```
The [```ImagingPlane```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/ImagingPlane.html) object defines various properties of imaging planes used to generate the optical physiology data. Required properties include ```excitation_lambda``` (the wavelength used to excite calcium indicators in nm), ```indicator```, ```location``` (brain area where the imaging plane is located), ```optical_channel``` (previously created ```OpticalChannel``` objects), and ```device``` (a link to the ```Device``` object defining the two-photon microscope). You may have noticed that the ```device``` property is set as a ```Softlink``` object which is used to link to an already existing NWB object (in our case) or to an object within an NWB file using a path. It is also useful to provide ```description```, ```imaging_rate``` (the rate at which individual linescans are obtained), and any other properties you deem necessary. Such properties may include ```origin_coords``` defining physical location of the first element of the imaging plane relative to some reference point (e.g., bregma; defined by the ```reference_frame``` property) and ```grid_spacing``` to define the distance between individual pixels within the imaging plane.

(tutorials-caimage-convert2nwb-matlab-load-data)=
#### Load Imaging/Recording Data


```{note}
This section is work in progress. Current priority!
```