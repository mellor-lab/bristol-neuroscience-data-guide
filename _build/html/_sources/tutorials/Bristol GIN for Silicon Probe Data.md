(tutorials-silicon-probe)=
# Bristol GIN for Silicon Probe Data

(tutorials-silicon-probe-create-repo)=
## Create Repository to Store Your Research Data
The example repository is organised according to the [Tonic Research Project Template](https://github.com/tonic-team/Tonic-Research-Project-Template). You can organise your data according to this template by following a few simple steps. First, go to your GIN web page (GIN-domain-name/your-username) and click on the Import Repository button.
```{image} ../assets/images/tutorials/bristol-gin-for-silicon-probe-data/import-repo.png
:name: fig-gin-silicon-import-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Import Repository**

You should be brought to the Import Repository page. As a Clone Address specify the Tonic template github page: https://github.com/tonic-team/Tonic-Research-Project-Template. Give the name to your repository and a concise description of the kind of data stored in the repository. In my case I am creating a repository containing dual silicon probe recordings of spontaneous neural activity in various brain areas of the mouse. Then click the green Migrate Repository button.
```{image} ../assets/images/tutorials/bristol-gin-for-silicon-probe-data/import-repo2.png
:name: fig-gin-silicon-import-repo2
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Describe Your New Repository**

You should see the template contents cloned under your new repository name.

Now open the terminal and navigate to the folder on your local file system where you would like to keep your research data. Download the remote repository to that location by typing this command (replace the repository path with your own):
```
gin get dervinism/infraslow-dynamics
```
You can now use this empty repository to store all of your newly acquired research project data and documents. If you already have data that was generated in the past, you can copy it here.

(tutorials-silicon-probe-setup-repo)=
## Set up Your Research Data Repository
I am going to copy old research data to this newly created repository and make certain rearrangements within the existing folder structure to make it more suitable for already existing data structures created while carrying out my research project. I am also going to use mock research data to reduce the size of the repository so that repository management actions can be performed fast for this tutorial. The [mock data repository](https://gin.g-node.org/dervinism/mock-ecephys-project) is available to download on GIN. You can download it by typing the line below in your terminal:
```
gin get dervinism/mock-ecephys-project
cd mock-ecephys-project
gin get-content
```
Once downloaded, open the repository and copy the contents of the repository except for the .git folder. Delete the contents of the infraslow-dynamics repository and paste the copied contents from the mock-ecephys-project repository. Edit the README file accordingly to reflect the new name of the repository and other info.

(tutorials-silicon-probe-commit)=
## Record Your Local Research Data Repository Changes
Once your repository is set up (data folders organised, data files placed in right locations, etc.), you should register the state of your repository with the local version control system. By doing so you create the image of your repository that can always be reverted to in the future in case the need to do so arises. When commiting local repository changes to your version control system you typically provide a concise message describing the changes. By convention, the message length should not exceed 50 characters. As for the first record, we type:
```
gin commit . -m "Initial commit"
```
This action would commit all changes locally. Dot means that the command is executed on the contents of the current working directory; therefore, make sure that you are inside the root folder of your repository when carrying out this action. The flag -m is used to pass the commit message. When you make new changes to the repository, whether editing text files or manipulating your data files, you should commit these changes periodically to your local versioning system by executing in the terminal a similar command:
```
gin commit . -m "A message describing new changes"
```

(tutorials-silicon-probe-upload)=
## Update Your Remote Research Data Repository
All of the changes that were commited previously, were done so locally. We were working on a local copy of our research data repository. In order to update our remote research data respository, whether residing on Bristol GIN or on the public GIN server, we need to push our local changes to the remote copy of our repository. We do so by simply executing the line below in the terminal:
```
gin upload .
```
Any new files and any new changes to exisiting files should now be uploaded onto the remote repository and we should be able to see them if we navigated to the repository web page. Alternatively, we can also update the remote repository using the web interface. We do so by navigating to the repository webpage and clicking the blue Upload file button.
```{image} ../assets/images/tutorials/bristol-gin-for-silicon-probe-data/upload-repo.png
:name: fig-gin-silicon-upload-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 3. Update Remote Repository via Web**

The limitation of using the web interface is that every time you update your remote repository, you will be limited to uploading 100 files at a time with each file being no larger than 10 gigabytes. Therefore, it is more effecient/effective to use the command line tools which have none of these limitations.

When you use the web interface, you can specify the commit message title (no more than 50 characters by convention) and the commit message body (no more than 72 characters by convention).

(tutorials-silicon-probe-remove-content)=
## Remove Content of Your Local Research Data Repository
One advantage of using GIN for your data repository mangement is that you do not need to keep duplicate repositories in order to prevent accidental detrimental changes to your main repository. One reason for that is having version control system. The other reason is that you can safely remove the content of your local repository and replace it with pointers to original files. As a result you can save space on your local hard-drive. To remove the local content type the following line in your commend terminal:
```
gin remove-content
```
Local files larger than 10 megabytes should be replaced by symbolic links. In case you want to remove the content of specific files only, you can type:
```
gin remove-content <absolute or relative path to file or folder>
```
For example, to remove the raw research data from our silicon probe recording repository, we type:
```
gin remove-content 03_data/001_uol_neuronexus_exp_raw_derived
gin remove-content 03_data/002_uol_neuropixels_exp_raw_derived
```
To simply restore the file content type in
```
gin get-content
```
If you no longer need to work on your repository and its remote copy is up to date with the local copy, you can simply delete the local repository copy altogether. You should always be able to restore your repository and all of its contents on your local machine by executing these commands in your terminal (replace the repository path as appropriate):
```
gin get dervinism/mock-ecephys-project
cd mock-ecephys-project
gin get-content
```

(tutorials-silicon-probe-compress)=
## Compress Raw Data
Silicon probe recording raw data files take a lot of space, especially neuropixels recordings. Often these files consume most of the hard-disk space compared to any other files produced during the data processing. It is, therefore, advisable to compress them. The International Brain Laboratory has provided compression software for that purpose which is easy to use with instructions on how to install and compress your files provided [here](https://github.com/int-brain-lab/mtscomp). Briefly, you can install it by typing in your terminal:
```
pip install mtscomp
```
Make sure you also have the required dependencies installed.

Once installed, the compression is straight forward. All you need is to specify the name of your binary file, number of recording channels, the sampling rate, and the binary data type as in the example below:
```
mtscomp <data-filename>.bin -n 385 -s 30000 -d int16
```
The compression software will produce ```<data-filename>.cbin``` compressed file and ```<data-filename>.ch``` json metadata file describing the compression parameters. These two files should be preserved while the original data binary file can be deleted. To restore the original file, type
```
mtsdecomp <data-filename>.cbin -o <data-filename>.bin
```
You can expect a three-fold reduction in the file size after the compression. So it is a good way to greatly reduce the size of your repositories.

(tutorials-silicon-probe-convert2nwb)=
## Convert Your Data to Standardised Format
In order to increase your research data's adherence to the [FAIR principles](https://www.go-fair.org/fair-principles/) of scientific data management and, in particular, to increase the interoperability of your data and chances of it being reused beyond its original purpose of collection, it is highly advisable to convert your data into one of the more popular standard file formats for neuroscience data. One such format is the [Neurodata Without Borders (NWB)](https://www.nwb.org/) which is highly suitable for most of the neurophysiology data. Programming interfaces in both Matlab and Python are available for converting your data. Here we are going to provide explanations of how you can convert your data in both programming languages. While showing you examples of that, we will continue our focus on the extracellular electrophysiology data.

(tutorials-silicon-probe-convert2nwb-matlab)=
### Convert to NWB Using Matlab
(tutorials-silicon-probe-convert2nwb-matlab-install-matnwb)=
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

(tutorials-silicon-probe-convert2nwb-matlab-record-metadata)=
#### Record Metadata
We have prepared example repositories containing single session extracellular physiology recording data collected with [Neuronexus](https://gin.g-node.org/dervinism/convert2nwbMatNnx) and [Neuropixels](https://gin.g-node.org/dervinism/convert2nwbMatNpx) probes and Matlab scripts that would convert that data to NWB format. They can be used to familiarise with the Matlab NWB conversion scheme for spiking data combined with behavioural measurements. Both conversion scripts are very similar and, thus, we will focus on the Neuropixels use case.

To donwload the Neuropixels repository, type in your terminal:
```
gin get dervinism/convert2nwbMatNpx
cd convert2nwbMatNpx
gin get-content
```

It will take some time to download the full repository. Once the download is complete you can open the ```convert2nwb.m``` file and and execute it right away. The script would load derived spiking and behavioural data from ```convert2nwbMatNpx/npx_derived_data/M200324_MD/M200324_MD.mat``` file, convert it to the NWB format, and save it inside ```convert2nwbMatNpx/npx_derived_data_nwb``` folder as ```ecephys_session_01.nwb``` file.

We will now analyse the conversion script in more detail. The script starts by executing three parameter files to initiate the conversion environment. The first parameter file ```nwbParams.m``` contains the most general type of parameters that apply to all animals and recording sessions of the experiment, like:
```
projectName = 'Brainwide Infraslow Activity Dynamics';
experimenter = 'Martynas Dervinis';
institution = 'University of Leicester';
publications = 'In preparation';
lab = 'Michael Okun lab';
dataset = 'neuropixels';
videoFrameRate = 25; % Hz
```
The names of most of these parameters are self-explanatory. The videoFrameRate variable contains the camera's frame rate recording the animal's pupil. There are also input and output folders defined at the bottom of the parameter file. They include:
- ```rawDataFolder``` which contains probe recording channel maps and unit waveform files;
- ```derivedDataFolder``` which contains processed spiking data;
- ```derivedDataFolderNWB``` which is the output folder where converted NWB files are saved.

As the name implies, ```nwbAnimalParams.m``` file contains parameters specific to a particular animal and common to all recording sessions for that animal. They include:
```
animalID = 'M200324_MD';
dob = '20200206'; % yyyymmdd
...
strain = 'C57BL/6J';
sex = 'M';
species = 'Mus musculus';
weight = [];
description = '025'; % Animal testing order.
```
Names of these parameters are self-explanatory. The script also defines input and output folders specifically for the animal of interest.

The ```nwbSessionParams.m``` file stores parameters about recording sessions. Some of these parameters are defined at the top of the file explicitly for each individual session and some even explicitly for each recording probe, like:
```
sessionID = {'20200324161349'};
sessionDescription = {'anaesthesia'};
sessionNotes = {'...'};
endCh{1}{1} = [88 117 149 342 384]; % Corresponding probe end channels starting from the tip of the probe. Corresponding and previous end channels are used to work out probe channels that reside in the corresponding brain area.
endCh{1}{2} = [41 091 138 217 304 384];
```
The ```for``` loop that follows next sets the remaining parameters that are common across recording sessions, like ```electrodeName```, ```electrodeDescription```, ```electrodeManufacturer```, ```nShanks``` (number of probe shanks), ```nChannelsPerShank``` (number of recording channels per shank), ```nCh``` (total number of probe channels), ```areas``` (brain areas that probes span), ```electrodeCoordinates``` (electrode insertion coordinates), ```electrodeLabel```, and ```electrodeImplantationType``` (i.e., acute or chronic).

Most of the parameters defined in the three parameter files comprise metadata. The way you define your metadata may be different. For example, you may have your own custom scripts that contain the metadata or you may store your metadata in files organised according to one of standard neuroscientific metadata formats like [odML](http://g-node.github.io/python-odml/). Whichever your preference is, this part of the NWB conversion procedure will vary depending on the individual researcher.

The initialisation process is completed by intialising the Matlab NWB classes by calling
```
generateCore;
```
within the ```convert2nwb.m``` script file.

The conversion process then goes through every recording session and generates NWB files individually for each session inside the ```for``` loop. We start by creating an [```NWBFile```](https://neurodatawithoutborders.github.io/matnwb/doc/NwbFile.html) object and defining session metadata:
```
% Assign NWB file fields
nwb = NwbFile( ...
  'session_description', sessionDescription{iSess},...
  'identifier', [animalID '_' sessionID{iSess}], ...
  'session_start_time', sessionStartTime{iSess}, ...
  'general_experimenter', experimenter, ... % optional
  'general_session_id', sessionID{iSess}, ... % optional
  'general_institution', institution, ... % optional
  'general_related_publications', publications, ... % optional
  'general_notes', sessionNotes{iSess}, ... % optional
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

(tutorials-silicon-probe-convert2nwb-matlab-electrodes-table)=
#### Construct Electrodes Table
Storing extracellular electrophysiology data is not possible without defining the ```electrodes``` table which is a [```DynamicTable```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+hdmf_common/DynamicTable.html) object. We do it first by creating a [Matlab table array](https://uk.mathworks.com/help/matlab/tables.html) using a code wrapped inside the ```createElectrodeTable``` function. We put the table generation code inside the function, because it is going to be reused for each probe. The function call for probe 1 is executed by the code below:
```
% Create electrode tables: Info about each recording channel
input.iElectrode = 1;
input.electrodeDescription = electrodeDescription{iSess};
input.electrodeManufacturer = electrodeManufacturer{iSess};
input.nShanks = nShanks{iSess};
input.nChannelsPerShank = nChannelsPerShank{iSess};
input.electrodeLocation = electrodeLocation{iSess};
input.electrodeCoordinates = electrodeCoordinates{iSess};
input.sessionID = sessionID{iSess};
input.electrodeLabel = electrodeLabel{iSess};
if probeInserted{iSess}{input.iElectrode} && ~isempty(endCh{iSess}{1})
  tbl1 = createElectrodeTable(nwb, input);
else
  tbl1 = [];
end
```

The table array for the probe 1 is then created within the function:
```
% Create a table with given column labels
variables = {'channel_id', 'channel_local_index', 'x', 'y', 'z', 'imp', 'location', 'filtering', 'group', 'channel_label', 'probe_label'};
tbl = cell2table(cell(0, length(variables)), 'VariableNames', variables);

% Register the probe device
device = types.core.Device(...
  'description', input.electrodeDescription{iEl}, ...
  'manufacturer', input.electrodeManufacturer{iEl} ...
  );
nwb.general_devices.set(['probe' num2str(iEl)], device);

for iShank = 1:nSh{iEl}
  
  % Register a shank electrode group (only one because this is a single shank probe)
  electrode_group = types.core.ElectrodeGroup( ...
    'description', ['electrode group for probe' num2str(iEl)], ...
    'location', input.electrodeLocation{iEl}{end}, ...
    'device', types.untyped.SoftLink(device), ...
    'position', table(input.electrodeCoordinates{iEl}(1,1), ...
    input.electrodeCoordinates{iEl}(1,2), ...
    input.electrodeCoordinates{iEl}(1,3), ...
    'VariableNames',{'x','y','z'}) ...
    );
  nwb.general_extracellular_ephys.set(['probe' num2str(iEl)], electrode_group);
  group_object_view = types.untyped.ObjectView(electrode_group);
  
  % Populate the electrode table
  for iCh = 1:nCh{iEl}
    if iCh < 10
      channelID = str2double([input.sessionID num2str(iEl) '00' num2str(iCh)]);
    elseif iCh < 99
      channelID = str2double([input.sessionID num2str(iEl) '0' num2str(iCh)]);
    else
      channelID = str2double([input.sessionID num2str(iEl) num2str(iCh)]);
    end
    channel_label = ['probe' num2str(iEl) 'shank' num2str(iShank) 'elec' num2str(iCh)];
    tbl = [tbl; ...
      {channelID, iCh, input.electrodeCoordinates{iEl}(iCh, 1), input.electrodeCoordinates{iEl}(iCh, 2), input.electrodeCoordinates{iEl}(iCh, 3),...
      NaN, input.electrodeLocation{iEl}{iCh}, 'unknown', group_object_view, channel_label, input.electrodeLabel{iEl}}]; %#ok<*AGROW>
  end
end
```
The code initialises an empty table with given column labels. It then records the probe [```Device```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/Device.html) (which is an object itself) inside the ```NWBFile``` object. Next, we create an [```ElectrodeGroup```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/ElectrodeGroup.html) object which is used to define electrode groupings within the probe. In our case we have a single shank probe and, therefore, we define all recording channels to be part of a single group. Though, grouping on some other basis is possible. You may have noticed that the ```device``` property is set as a Softlink object which is used to link to an already existing NWB object (in our case) or to an object within an NWB file using a path. Similarly, ```ElectrodeGroup``` is used to create an ObjectView object which works in a very similar way to the SoftLink object, and which is later used in constructing the ```electrodes``` table. The ```position``` property is a Matlab table array with columns being stereotaxic coordinates. Finally, the ```electrodes``` table is filled in channel by channel (rows) with channel subtables.

The same procedure is then repeated for the probe 2. Finally, the combined table array is then converted into a ```DynamicTable``` object using ```util.table2nwb``` function which also takes in the table description as the second argument:
```
tbl = [tbl1; tbl2];
electrode_table = util.table2nwb(tbl, 'all electrodes');
nwb.general_extracellular_ephys_electrodes = electrode_table;
```
```{note}
You can add any number of custom columns to a ```DynamicTable``` object and, therefore, you can expand an ```electrodes``` table object with any additional metadata you deem necessary.
```

(tutorials-silicon-probe-convert2nwb-matlab-init-spiking-data)=
#### Load and Initialise Spiking Data
The next line of code loads processed spiking data from the Matlab MAT file by calling the ```getSpikes``` function:
```
[spikes, metadata, derivedData] = getSpikes(derivedData, animalID, sessionID{iSess}, tbl);
```
This is a custom function containing the loading algorithm that very much depends on the processed data structure stored inside the MAT file. I will not go into the detail of how the function runs as your own data is very likely to be structured differently. However, you are welcome to explore the code yourself as it is commented generously. It will suffice to say that the function outputs the ```spikes``` variable which is a 1-by-n cell array with unit spike times in seconds, where n is the number of units. Moreover, the function also outputs the ```metadataTbl``` variable which is a Matlab table array with rows corresponding to individual clusters (units) and columns to various metadata types describing unit properties, like ```cluster_id```, ```local_cluster_id```, ```type```, ```channel_index```, ```channel_id```, ```local_channel_id```, ```rel_horz_position```, ```rel_vert_position```, ```isi_violations```, ```isolation_distance```, ```area```, ```probe_id```, and ```electrode_group```. You can find the description of each of these properties in the ```getSpikes``` function definition.

Once the spike times are extracted, we convert them into [VectorData](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+hdmf_common/VectorData.html) and [VectorIndex](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+hdmf_common/VectorIndex.html) objects by executing the line below:
```
[spike_times_vector, spike_times_index] = util.create_indexed_column(spikes);
```
```spike_times_vector.data``` property is simply a vector of spike times where unit spike times are consequtively arranged into this single vector grouped on a unit basis. Meanwhile, ```spike_times_index.data``` property is a vector of indices which are then used to index the end of corresponding unit spike times data (i.e., row breaks). In conjunction, VectorData and VectorIndex objects can be used to encode ragged arrays. Ragged arrays have rows with different number of columns. All units taken together and their spike times form such a ragged array which is illustrated in the figure below.
```{image} ../assets/images/tutorials/bristol-gin-for-silicon-probe-data/ragged-array.png
:name: fig-gin-silicon-ragged-array
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 4. Ragged Array** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Original image taken from [here](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/ecephys.html).

(tutorials-silicon-probe-convert2nwb-matlab-load-waveforms)=
#### Load Waveforms
Before converting spiking data we load unit waveforms. The waveforms data can also be stored as a ragged array, even as a double-indexed one. You can find more information [here](https://nwb-schema.readthedocs.io/en/latest/format_description.html) on how to construct such arrays. In our case, the waveforms arrays are rather simple: we are only interested in average waveforms on the probe recording channel with the largest waveform amplitude. This data is stored in the ```waveformMeans``` variable which is a cell array of average waveforms with cells corresponding to individual units. The variable is constructed after loading and reshaping the waveforms located inside the ```convert2nwbMatNpx/npx_raw_derived_data/M200324_MD/<session-ID>/waveforms.mat``` files:
```
% Load and reshape unit waveforms
...
for iWave = 1:numel(waveformMeans)
  if isempty(waveformMeans{iWave})
    waveformMeans{iWave} = nan(1,nWaveformSamples);
  end
end
```

(tutorials-silicon-probe-convert2nwb-matlab-units-table)=
#### Construct Units Table
In order to store spiking data and other related data, we construct the [```units``` table](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/Units.html) which is, like the ```electrodes``` table, a ```DynamicTable``` object. As such it supports addition of additional metadata columns. The code that constructs the ```units``` table is shown below:
```
nwb.units = types.core.Units( ...
  'colnames', {'cluster_id','local_cluster_id','type',...
  'peak_channel_index','peak_channel_id',... % Provide the column order. All column names have to be defined below
  'local_peak_channel_id','rel_horz_pos','rel_vert_pos',...
  'isi_violations','isolation_distance','area','probe_id',...
  'electrode_group','spike_times','spike_times_index'}, ...
  'description', 'Units table', ...
  'id', types.hdmf_common.ElementIdentifiers( ...
  'data', int64(0:length(spikes) - 1)), ...
  'cluster_id', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,1}), ...
  'description', 'Unique cluster id'), ...
  'local_cluster_id', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,2}), ...
  'description', 'Local cluster id on the probe'), ...
  'type', types.hdmf_common.VectorData( ...
  'data', metadata{:,3}, ...
  'description', 'Cluster type: unit vs mua'), ...
  'peak_channel_index', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,4}), ...
  'description', 'Peak channel row index in the electrode table'), ...
  'peak_channel_id', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,5}), ...
  'description', 'Unique ID of the channel with the largest cluster waveform amplitude'), ...
  'local_peak_channel_id', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,6}), ...
  'description', 'Local probe channel with the largest cluster waveform amplitude'), ...
  'rel_horz_pos', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,7})./1000, ...
  'description', 'Probe-relative horizontal position in mm'), ...
  'rel_vert_pos', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,8})./1000, ...
  'description', 'Probe tip-relative vertical position in mm'), ...
  'isi_violations', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,9}), ...
  'description', 'Interstimulus interval violations (unit quality measure)'), ...
  'isolation_distance', types.hdmf_common.VectorData( ...
  'data', cell2mat(metadata{:,10}), ...
  'description', 'Cluster isolation distance (unit quality measure)'), ...
  'area', types.hdmf_common.VectorData( ...
  'data', metadata{:,11}, ...
  'description', ['Brain area where the unit is located. Internal thalamic ' ...
  'nuclei divisions are not precise, because they are derived from unit locations on the probe.']), ...
  'probe_id', types.hdmf_common.VectorData( ...
  'data', metadata{:,12}, ...
  'description', 'Probe id where the unit is located'), ...
  'spike_times', spike_times_vector, ...
  'spike_times_index', spike_times_index, ...
  'electrode_group', types.hdmf_common.VectorData( ...
  'data', metadata{:,13}, ...
  'description', 'Recording channel groups'), ...
  'waveform_mean', types.hdmf_common.VectorData( ...
  'data', cell2mat(waveformMeans), ...
  'description', ['Mean waveforms on the probe channel with the largest waveform amplitude. ' ...
  'MUA waveforms are excluded. The order that waveforms are stored match the order of units in the unit table.']) ...
);
```

The property ```colnames``` specifies ```units``` table column names and should be used to indicate column order. The ```description``` property is simply used to describe what kind of information is stored in the table. When constructing the table you have to provide the ```id``` property which is the [ElementIdentifiers](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+hdmf_common/ElementIdentifiers.html) list object containing indices starting with 0 and used to identify ```DynamicTable``` rows. Custom properties are passed in as VectorData objects with their own ```data``` and ```description``` properties. In the above code snippet these include ```cluster_id```, ```local_cluster_id```, ```type```, ```peak_channel_index```, ```peak_channel_id```, ```local_peak_channel_id```, ```rel_horz_pos```, ```rel_vert_pos```, ```isi_violations```, ```isolation_distance```, ```area```, and ```probe_id```. You can also pass in a few optional properties like ```electrode_group``` VectorData object which specifies an electrode group that each spike unit came from and ```electrodes``` [```DynamicTableRegion```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+hdmf_common/DynamicTableRegion.html) object used to reference the ```electrodes``` table containing the electrodes that each spike unit came from, as well as other properties. Finally, we also pass in the ```waveform_mean``` property containing a matrix of mean unit waveforms as a VectorData object. The latter property will not be one of the table columns as it is not listed in the ```colnames``` property but it will be accessible via dot indexing.
```{note}
VectorData objects should not have cell arrays of integers (cell arrays of strings are fine) as their data property. During the NWB conversion they are not going to be encoded properly and will generate an error. You can use Matlab's ```cell2mat``` function to convert them into regular number arrays.
```

(tutorials-silicon-probe-convert2nwb-matlab-pupil-area)=
#### Add Behavioural Module: Pupil Area Size
We load pupil area size data from the same file containing processed spiking data. We then convert this data into a [```TimeSeries```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/TimeSeries.html) object which is designed to store any general purpose time series data. This object has a few mandatory properties, like ```data```, ```data_unit```, and ```starting_time_rate```, and a few optional ones, like ```timestamps```, ```control```, ```control_description```, and ```description```. The ```data``` property can be stored as any 1-D, 2-D, 3-D, or 4-D array where the first dimension is time. We use it to store our ```pupilAreaSize``` data vector. The ```data_unit``` property has to be a string, while the ```starting_time_rate``` property is a scalar of ```float32``` type. The ```timestamps``` property should give data sampling times and should be stored as a unidimensional number array of ```float64``` type. The ```control``` property is used for labeling data samples with integers. The length of this array should be the same as the length of the first data dimension representing time. In our case I am using integers 0 and 1 to mark acceptable quality data. The meaning of these labels is provided by the ```control_description``` property which is a cell array of strings with cells describing labels in the increasing order. The full code that converts the pupil area size data into the appropriate form is shown below:
```
pupilAreaSize = types.core.TimeSeries( ...
  'data', pupilAreaSize, ...
  'timestamps', videoFrameTimes, ...
  'data_unit', 'pixels^2', ...
  'starting_time_rate', videoFrameRate,...
  'control', uint8(acceptableSamples),...
  'control_description', {'low quality samples that should be excluded from analyses';...
  'acceptable quality samples'},...
  'description', ['Pupil area size over the recording session measured in pixels^2. ' ...
  'Acceptable quality period starting and ending times are given by data_continuity parameter. ' ...
  'The full data range can be divided into multiple acceptable periods.'] ...
);

pupilTracking = types.core.PupilTracking('TimeSeries', pupilAreaSize);
behaviorModule = types.core.ProcessingModule('description', 'contains behavioral data');
behaviorModule.nwbdatainterface.set('PupilTracking', pupilTracking);
```
As you will notice, our ```TimeSeries``` object is assigned to a [```PupilTracking```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/PupilTracking.html) object which can hold one or more of the ```TimeSeries``` objects. We then create a [```ProcessingModule```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/ProcessingModule.html) container object to store the bahavioural data. The final line creates a ```nwbdatainterface.PupilTracking``` property with our ```PupilTracking``` object within the ```ProcessingModule``` object.

(tutorials-silicon-probe-convert2nwb-matlab-movement)=
#### Add Behavioural Module: Total Facial Movement
The facial movement data conversion process into appropriate form for NWB storage is almost identical to the conversion process applied to the pupil area size data. First, we load the data and store it inside the ```TimeSeries``` object. The steps are identical. Next, we store our ```TimeSeries``` object inside the [```BehavioralTimeSeries```](https://neurodatawithoutborders.github.io/matnwb/doc/+types/+core/BehavioralTimeSeries.html) object designed for holding one or more ```TimeSeries``` objects:
```
behavioralTimeSeries = types.core.BehavioralTimeSeries('TimeSeries', totalFaceMovement);
```
We then create a ```nwbdatainterface.BehavioralTimeSeries``` property with our ```BehavioralTimeSeries``` object within the ```ProcessingModule``` container object:
```
behaviorModule.nwbdatainterface.set('BehavioralTimeSeries', behavioralTimeSeries);
```
Finally, we create the behaviour module within the ```NWBFile``` object:
```
nwb.processing.set('behavior', behaviorModule);
```

(tutorials-silicon-probe-convert2nwb-matlab-save)=
#### Save NWB File
We call the ```nwbExport``` function to save our data in the NWB format:
```
if iSess < 10
  nwbExport(nwb, [animalDerivedDataFolderNWB filesep 'ecephys_session_0' num2str(iSess) '.nwb']);
else
  nwbExport(nwb, [animalDerivedDataFolderNWB filesep 'ecephys_session_' num2str(iSess) '.nwb']);
end
```

(tutorials-silicon-probe-convert2nwb-matlab-read)=
#### Read NWB File
Now if you want to open the NWB file that you just saved in Matlab, you can issue a command
```
nwb2 = nwbRead('ecephys_session_01.nwb');
```
which will read the file passively. The action is fast and it does not load all of the data at once but rather make it readily accessible. This is useful as it allows you to read data selectively without loading the entire file content into the computer memory.

If you want to read the entire unit table which has all unit spiking data and associated metadata, the easiest way is to issue a command
```
unitsTable = nwb2.units.loadAll.toTable();
```
which will convert the ```DynamicTable``` object into a Matlab table array. The latter is a common Matlab data type and can be manipulated easily using regular Matlab syntax and indexing.

In contrast, if you are interested in a single unit data, you can load it using the following line of code:
```
unitRow = nwb2.units.getRow(1);
```

To load only spiking data for the same unit, the following line of code will do:
```
unitRowSpikeTimes = nwb2.units.getRow(1).spike_times{1};
```

If you want to load mean unit waveforms, the following line of code will load them as a floating-point number array:
```
unitsWaveforms = nwb2.units.waveform_mean.loadAll.data;
```

Meanwhile, behavioural data can be accessed by running the code below:
```
pupilAreaSize = nwb2.processing.get('behavior'). ...
    nwbdatainterface.get('PupilTracking'). ...
    timeseries.get('TimeSeries').data(:);

totalFacialMovement = nwb2.processing.get('behavior'). ...
    nwbdatainterface.get('BehavioralTimeSeries'). ...
    timeseries.get('TimeSeries').data(:);
```
Other associated behavioural properties can be accessed by replacing the ```data``` property by ```timestamps``` or ```control``` and so on.

Some metadata is often directly available as properties of the ```NWBFile``` object, like:
```
sessionDescription = nwb2.session_description;
```
Subject metadata is available via the command, for example:
```
animalID = nwb2.general_subject.subject_id;
```
While the electrode metadata can be accessed in the following way:
```
electrodesTable = nwb2.general_extracellular_ephys_electrodes.toTable();
```
```electrodes``` table is a ```DynamicTable``` object and, therefore, methods of accessing it are the same as those discussed for accessing the ```units``` table. For more detailed account of how you can access and manipulate ```DynamicTable``` objects refer to an external [MatNWB DynamicTables Tutorial](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/dynamic_tables.html).

(tutorials-silicon-probe-convert2nwb-matlab-validate)=
#### Validate NWB File
MatNWB does not provide its own NWB file validator. However, you can validate NWB files generated using Matlab in python. For instructions on how to do it, refer to the corresponding [PyNWB validation section](tutorials-silicon-probe-convert2nwb-py-validate) of the tutorial.

(tutorials-silicon-probe-convert2nwb-matlab-resources)=
#### Resources
This section explained how you can use Matlab to convert your processed spiking and behavioural data into NWB files. It is not an exhaustive overview of Matnwb toolbox extracellular electrophysiology functionality. There are other potential use cases involving local field potentials (LFPs) and different types of behavioural data. There is a number of externally available tutorials covering some aspects of converting data obtained in extracellular electrophysiology experiments to the NWB file format using Matlab:
- [Intro to NWB Tutorial](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/intro.html)
- [NWB Extracellular Electrophysiology Tutorial](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/ecephys.html)
- [Behavior Data Tutorial](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/behavior.html)
- [MatNWB DynamicTables Tutorial](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/dynamic_tables.html)
- [MatNWB API Documentation](https://neurodatawithoutborders.github.io/matnwb/doc/index.html)
- [NWB Schema Overview](https://nwb-schema.readthedocs.io/en/latest/index.html)

(tutorials-silicon-probe-convert2nwb-py)=
### Convert to NWB Using Python
(tutorials-silicon-probe-convert2nwb-py-install)=
#### Install PyNWB
To convert your extracellular electrophysiology data, you will need to install PyNWB first. To do so, type in your terminal:
```
pip install -U pynwb
```
Now you're ready to start working with PyNWB. The full installation instructions are available [here](https://pynwb.readthedocs.io/en/stable/install_users.html).

(tutorials-silicon-probe-convert2nwb-py-record-metadata)=
#### Record Metadata
Just like in the Matlab tutorial, we have prepared example repositories containing single session extracellular physiology recording data collected with [Neuronexus](https://gin.g-node.org/dervinism/convert2nwbPyNnx) and [Neuropixels](https://gin.g-node.org/dervinism/convert2nwbPyNpx) probes and Python scripts that would convert that data to NWB format. They can be used to familiarise with the Python NWB conversion scheme for spiking data combined with behavioural measurements. Both conversion scripts are very similar and, thus, we will focus on the Neuropixels use case.

To donwload the Neuropixels repository, type in your terminal:
```
gin get dervinism/convert2nwbPyNpx
cd convert2nwbPyNpx
gin get-content
```

It will take some time to download the full repository. Once the download is complete you can open the ```convert2nwb.py``` file and and execute it right away. The script would load derived spiking and behavioural data from ```convert2nwbPyNpx/npx_derived_data/M200324_MD/M200324_MD.mat``` file, convert it to the NWB format, and save it inside ```convert2nwbPyNpx/npx_derived_data_nwb``` folder as ```ecephys_session_01.nwb``` file.

We will now analyse the conversion script in more detail. The script starts by importing general and ```pynwb``` module dependencies, as well as custom functions located inside the ```localFunctions.py``` module file.
```python
from datetime import datetime
from dateutil.tz import tzlocal
import numpy as np

from pynwb import NWBFile, NWBHDF5IO, TimeSeries
from pynwb.behavior import PupilTracking, BehavioralTimeSeries
from pynwb.core import DynamicTable
from pynwb.ecephys import ElectricalSeries, LFP
from pynwb.file import Subject

from localFunctions import createElectrodeTable, array2list, getSpikes, reshapeWaveforms, concatenateMat, parsePeriod, markQualitySamples
```
These are not all modules that we need. Some of them are loaded inside parameter files. These other modules include: ```pathlib```, ```os```, ```re```, ```datetime```, ```numpy```, ```h5py```, and ```scipy``` among others.

We then execute three parameter files to initiate the conversion environment. The first parameter file ```nwbParams.py``` contains the most general type of parameters that apply to all animals and recording sessions of the experiment, like:
```python
projectName = 'Brainwide Infraslow Activity Dynamics'
experimenter = 'Martynas Dervinis'
institution = 'University of Leicester'
publications = 'In preparation'
lab = 'Michael Okun lab'
dataset = 'neuropixels'
videoFrameRate = 25.0 # Hz
```
The names of most of these parameters are self-explanatory. The ```videoFrameRate``` variable contains the camera's frame rate recording the animal's pupil. There are also input and output folders which are initialised as global variables and defined at the bottom of the parameter file. They include:
- ```rawDataFolder``` which contains probe recording channel maps and unit waveform files;
- ```derivedDataFolder``` which contains processed spiking data;
- ```derivedDataFolderNWB``` which is the output folder where converted NWB files are saved.

As the name implies, ```nwbAnimalParams.py``` file contains parameters specific to a particular animal and common to all recording sessions for that animal. They include:
```python
animalID = 'M200324_MD'
dob = '20200206'
...
strain = 'C57BL/6J'
sex = 'M'
species = 'Mus musculus'
weight = []
description = '025' # Animal testing order.
```
Names of these parameters are self-explanatory. The script also defines input and output folders specifically for the animal of interest.

The ```nwbSessionParams.py``` file stores parameters about recording sessions. Some of these parameters are defined at the top of the file explicitly for each individual session and some even explicitly for each recording probe, like:
```python
sessionID = ['20200324161349']
sessionDescription = ['anaesthesia']
sessionNotes = ['...']
endCh = [[None] * 2 for i in range(len(sessionID))]
endCh[0][0] =  np.array([88, 117, 149, 342, 384]) # Corresponding probe end channels starting from the tip of the probe. Corresponding and previous end channels are used to work out probe channels that reside in the corresponding brain area.
endCh[0][1] = np.array([41, 91, 138, 217, 304, 384])
```
The ```for``` loop that follows next sets the remaining parameters that are common across recording sessions, like ```electrodeName```, ```electrodeDescription```, ```electrodeManufacturer```, ```nShanks``` (number of probe shanks), ```nChannelsPerShank``` (number of recording channels per shank), ```nCh``` (total number of probe channels), ```areas``` (brain areas that probes span), ```electrodeCoordinates``` (electrode insertion coordinates), ```electrodeLabel```, and ```electrodeImplantationType``` (i.e., acute or chronic).

Most of the parameters defined in the three parameter files comprise metadata. The way you define your metadata may be different. For example, you may have your own custom scripts that contain the metadata or you may store your metadata in files organised according to one of standard neuroscientific metadata formats like [odML](http://g-node.github.io/python-odml/). Whichever your preference is, this part of the NWB conversion procedure will vary depending on the individual researcher.

The conversion process then goes through every recording session and generates NWB files individually for each session inside the ```for``` loop of the ```convert2nwb.py``` file. We start by creating an [```NWBFile```](https://pynwb.readthedocs.io/en/stable/pynwb.file.html#pynwb.file.NWBFile) object and defining session metadata:
```python
# Assign NWB file fields
nwb = NWBFile(
  session_description = sessionDescription[iSess],
  identifier = animalID + '_' + sessionID[iSess], 
  session_start_time = sessionStartTime[iSess], 
  experimenter = experimenter,  # optional
  session_id = sessionID[iSess],  # optional
  institution = institution,  # optional
  related_publications = publications,  # optional
  notes = sessionNotes[iSess],  # optional
  lab = lab) # optional
```
Each file must have a ```session_description``` identifier and a ```session_start_time``` parameter. We then initialise the [```Subject```](https://pynwb.readthedocs.io/en/stable/pynwb.file.html#pynwb.file.Subject) object and the metadata it contains:
```python
# Create subject object
nwb.subject = Subject(
  subject_id = animalID,
  age = age,
  description = description,
  species = species,
  sex = sex)
```

(tutorials-silicon-probe-convert2nwb-py-electrodes-table)=
#### Construct Electrodes Table
Storing extracellular electrophysiology data is not possible without defining the ```electrodes``` table which is a [```DynamicTable```](https://hdmf.readthedocs.io/en/stable/hdmf.common.table.html#hdmf.common.table.DynamicTable) object. We do it first by creating a numpy array object using a code wrapped inside the ```createElectrodeTable``` function. We put the table generation code inside the function, because it is going to be reused for each probe. The function call for probe 1 is executed by the code below:
```python
input = {
  "iElectrode": 0,
  "electrodeDescription": electrodeDescription[iSess],
  "electrodeManufacturer": electrodeManufacturer[iSess],
  "nShanks": nShanks[iSess],
  "nChannelsPerShank": nChannelsPerShank[iSess],
  "electrodeLocation": electrodeLocation[iSess],
  "electrodeCoordinates": electrodeCoordinates[iSess],
  "sessionID": sessionID[iSess],
  "electrodeLabel": electrodeLabel[iSess]}
if probeInserted[iSess][input["iElectrode"]] and endCh[iSess][input["iElectrode"]].any():
  (tbl1, columnLabels, columnDescription) = createElectrodeTable(nwb, input)
else:
  tbl1 = []; columnLabels = []; columnDescription = []
```

The table numpy array for the probe 1 is then created within the function:
```python
# Parse input
iEl = input["iElectrode"]
nSh = input["nShanks"]
nCh = input["nChannelsPerShank"]

# Create a table with given column labels
columnLabels = ['channel_id', 'channel_local_index', 'x', 'y', 'z', 'imp', 'location', 'filtering', 'group', 'channel_label', 'probe_label']
columnDescription = [
  'A unnique probe channel ID formed by combining session ID, probe reference number, and channel number relative to the tip of the probe.',
  'Channel index relative to the tip of the probe. Channel indices are only unique within a probe.',
  'Channel AP brain surface coordinate (probe inisertion location mm).',
  'Channel ML brain surface coordinate (probe inisertion location mm).',
  'Channel location relative to the tip of the probe in mm.',
  'Channel impedance.',
  'Channel brain area location.',
  'Type of LFP filtering applied.',
  'Channel electrode group (e.g., shank 1).',
  'Channel_label.',
  'Probe_label.'
]
tbl = np.empty((0, len(columnLabels)))

# Register the probe device
device = nwb.create_device(
  name = 'probe' + str(iEl+1),
  description = input["electrodeDescription"][iEl],
  manufacturer = input["electrodeManufacturer"][iEl])

for iShank in range(nSh[iEl]):
  
  # Register a shank electrode group (only one because this is a single shank probe)
  electrode_group = nwb.create_electrode_group(
    name = 'probe' + str(iEl+1),
    description = 'electrode group for probe' + str(iEl+1),
    location = input["electrodeLocation"][iEl][-1],
    device = device,
    position = input["electrodeCoordinates"][iEl][-1])
  
  # Populate the electrode table
  for iCh in range(nCh[iEl]):
    if iCh < 10-1:
      channelID = str(input["sessionID"] + str(iEl+1) + '00' + str(iCh+1))
    elif iCh < 99-1:
      channelID = str(input["sessionID"] + str(iEl+1) + '0' + str(iCh+1))
    else:
      channelID = str(input["sessionID"] + str(iEl+1) + str(iCh+1))
    channel_label = 'probe' + str(iEl+1) + 'shank' + str(iShank+1) + 'elec' + str(iCh+1)
    tbl = np.append(tbl, np.matrix([
      channelID, iCh+1, input["electrodeCoordinates"][iEl][iCh][0], input["electrodeCoordinates"][iEl][iCh][1], input["electrodeCoordinates"][iEl][iCh][2],
      np.nan, input["electrodeLocation"][iEl][iCh], 'unknown', electrode_group, channel_label, input["electrodeLabel"][iEl]]), axis=0)
  
return np.array(tbl), columnLabels, columnDescription
```
The code initialises an empty table with given column labels. It then records the probe [```Device```](https://pynwb.readthedocs.io/en/stable/pynwb.device.html?highlight=create_device) inside the ```NWBFile``` object (see the method documentation [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=create_device#pynwb.file.NWBFile.create_device)). Next, we create an [```ElectrodeGroup```](https://pynwb.readthedocs.io/en/stable/pynwb.ecephys.html#pynwb.ecephys.ElectrodeGroup) object which is used to define electrode groupings within the probe (see the method documentation [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=create_electrode_group#pynwb.file.NWBFile.create_electrode_group)). In our case we have a single shank probe and, therefore, we define all recording channels to be part of a single group. Though, grouping on some other basis is possible. We set the ```device``` property to the previously created ```Device``` object. The ```location``` property defines the location on the cortical surface where the probe was inserted, while the ```position``` property is a numpy array with columns being stereotaxic coordinates. Finally, the ```electrodes``` table is filled in channel by channel (rows) with channel subtables.

The same procedure is then repeated for the probe 2. The numpy arrays for the two probes are then combined and converted into a column list:
```python
if len(tbl1) and len(tbl2):
  tbl = array2list(np.append(tbl1, tbl2, axis=0), columnLabels, columnDescription)
elif len(tbl1) and not len(tbl2):
  tbl = array2list(tbl1, columnLabels, columnDescription)
elif not len(tbl1) and len(tbl2):
  tbl = array2list(tbl2, columnLabels, columnDescription)
else:
  tbl = []
```
Finally, this list is converted into an ```electrodes``` table ```DynamicTable``` object. We start by defining ```electrodes``` table columns using ```add_electrode_column``` ```NWBFile``` method (documented [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=add_electrode_column#pynwb.file.NWBFile.add_electrode_column)). Next we add table entries electrode by electrode using the ```add_electrode``` method (documented [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=add_electrode_column#pynwb.file.NWBFile.add_electrode)). We finish by converting the ```electrodes``` table into a pandas [```DataFrame``` object](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html) by calling the ```to_dataframe``` ```DynamicTable``` method (documented [here](https://hdmf.readthedocs.io/en/stable/hdmf.common.table.html#hdmf.common.table.DynamicTable.to_dataframe)).
```python
if len(tbl):
  for iColumn in range(len(columnLabels)):
    if columnLabels[iColumn] is not 'location' and columnLabels[iColumn] is not 'group':
      nwb.add_electrode_column(name=columnLabels[iColumn], description=columnDescription[iColumn])
  
  for iElec in range(len(tbl[0].data)):
    nwb.add_electrode(
      channel_id=tbl[0].data[iElec],
      channel_local_index=tbl[1].data[iElec],
      x=tbl[2].data[iElec],
      y=tbl[3].data[iElec],
      z=float(tbl[4].data[iElec]),
      imp=tbl[5].data[iElec],
      location=tbl[6].data[iElec],
      filtering=tbl[7].data[iElec],
      group=tbl[8].data[iElec],
      channel_label=tbl[9].data[iElec],
      probe_label=tbl[10].data[iElec]
    )

  nwb.electrodes.to_dataframe()
```
```{note}
You can add any number of custom columns to a ```DynamicTable``` object and, therefore, you can expand an ```electrodes``` table object with any additional metadata you deem necessary.
```

(tutorials-silicon-probe-convert2nwb-py-init-spiking-data)=
#### Load and Initialise Spiking Data
The next line of code loads processed spiking data from the Matlab MAT file by calling the ```getSpikes``` function:
```python
(spikes, metadata, derivedData, columnLabels, columnDescription) = getSpikes(derivedData, animalID, sessionID[iSess], tbl)
```
This is a custom function containing the loading algorithm that very much depends on the processed data structure stored inside the MAT file. I will not go into the detail of how the function runs as your own data is very likely to be structured differently. However, you are welcome to explore the code yourself as it is commented generously. It will suffice to say that the function outputs the ```spikes``` variable which is a a 1-by-n list of numpy arrays of unit spike times in seconds, where n is the number of units. Moreover, the function also outputs the ```metadata``` variable which is a numpy array with rows corresponding to individual clusters (units) and columns to various metadata types describing unit properties, like ```cluster_id```, ```local_cluster_id```, ```type```, ```peak_channel_index```, ```peak_channel_id```, ```local_peak_channel_id```, ```rel_horz_position```, ```rel_vert_position```, ```isi_violations```, ```isolation_distance```, ```area```, ```probe_id```, and ```electrode_group```. You can find the description of each of these properties inside the ```getSpikes``` function definition.

For convenience and computational efficiency, the function also outputs the already loaded MAT file which can be reused later (the ```derivedData``` variable). The column labels of the metadata array and their descriptions are also provided as ```columnLabels``` and ```columnDescription``` variables, respectively.

```{note}
Spike times are stored as a ragged array inside the NWB file. Because each unit has a different number of spikes, spiking data from all units are pooled together into a single vector but spikes are grouped on the unit basis. An index vector is constructed and stored together inside the NWB file indexing final spikes of each unit (i.e., row breaks of the initial numpy array). You can read more about ragged arrays [here](https://neurodatawithoutborders.github.io/matnwb/tutorials/html/ecephys.html).
```

(tutorials-silicon-probe-convert2nwb-py-load-waveforms)=
#### Load Waveforms
Before converting spiking data we load unit waveforms. The waveforms data can also be stored as a ragged array, even as a double-indexed one. You can find more information [here](https://nwb-schema.readthedocs.io/en/latest/format_description.html) on how to construct such arrays. In our case, the waveforms arrays are rather simple: we are only interested in average waveforms on the probe recording channel with the largest waveform amplitude. This data is stored in the ```waveformMeans``` variable which is a numpy array of average waveforms with rows corresponding to individual units (MUAs are stored as NANs). The variable is constructed after loading and reshaping the waveforms located inside the ```convert2nwbPyNpx/npx_raw_derived_data/M200324_MD/<session-ID>/waveforms.mat``` files by calling the ```reshapeWaveforms``` function:
```python
# Load data fields
if len(waveforms):
  maxWaveforms = np.array(waveforms.get('maxWaveforms')).transpose()
  cluIDs = np.array(waveforms.get('cluIDs')).transpose()
else:
  maxWaveforms = []
  cluIDs = []

# Load waveforms
metadataInds = np.where(np.array(np.isin(metadata[:,11], 'probe' + str(iEl))))[0]
metadata = metadata[metadataInds]
waveformsMean = []
if len(maxWaveforms) and len(maxWaveforms.shape):
  nWaveformSamples = maxWaveforms.shape[1]
else:
  nWaveformSamples = 200

for iUnit in range(metadata.shape[0]):
  row = np.where(np.isin(cluIDs, metadata[iUnit,1]))[0]
  if row.size:
    waveformsMean.append(maxWaveforms[row])
  else:
    waveformsMean.append(np.full([1,nWaveformSamples], np.nan))

return waveformsMean
```

(tutorials-silicon-probe-convert2nwb-py-units-table)=
#### Construct Units Table
In order to store spiking data and other related data, we construct the [```units``` table](https://pynwb.readthedocs.io/en/stable/pynwb.misc.html#pynwb.misc.Units) which is, like the ```electrodes``` table, a ```DynamicTable``` object. As such it supports addition of extra metadata columns. The code that constructs the ```units``` table is shown below:
```python
for iColumn in range(len(columnLabels)-1):
  nwb.add_unit_column(name=columnLabels[iColumn], description=columnDescription[iColumn])
for iUnit in range(len(spikes)):
  nwb.add_unit(
    cluster_id=metadata[iUnit,0],
    local_cluster_id=metadata[iUnit,1],
    type=metadata[iUnit,2],
    peak_channel_index=metadata[iUnit,3],
    peak_channel_id=metadata[iUnit,4],
    local_peak_channel_id=metadata[iUnit,5],
    rel_horz_pos=metadata[iUnit,6],
    rel_vert_pos=metadata[iUnit,7],
    isi_violations=metadata[iUnit,8],
    isolation_distance=metadata[iUnit,9],
    area=metadata[iUnit,10],
    probe_id=metadata[iUnit,11],
    electrode_group=metadata[iUnit,12],
    spike_times=spikes[iUnit],
    waveform_mean=waveformMeans[iUnit]
  )
```
Like when constructing the ```electrodes``` table, we first define the columns of the ```units``` table by invoking the ```NWBFile``` ```add_unit_column``` method (you can find its documentation [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=add_unit_column#pynwb.file.NWBFile.add_unit_column)). The column names are derived from the ```columnLabels``` variable which is defined inside the ```getSpikes``` function discussed in [one of the previous subsections](tutorials-silicon-probe-convert2nwb-py-init-spiking-data). Column descriptions are derived from the ```columnDescription``` variable which is also defined inside the same function. Once columns are defined, we add metadata, spike times, and waveforms for each unit individually using the ```add_unit``` method (documented [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=add_unit_column#pynwb.file.NWBFile.add_unit)).

(tutorials-silicon-probe-convert2nwb-py-pupil-area)=
#### Add Behavioural Module: Pupil Area Size
The code inside the ```convert2nwb.py``` file up to this point described how to convert spiking data into the NWB format. If there was no behavioural or other data recorded, the NWB file can now be saved. That is rarely the case, however, and this and the next subsections describe how to convert certain type of behavioural data into this format. We start with the data extracted after analysing recordings of the pupil area size.

We load pupil area size data from the same file containing processed spiking data. We then convert this data into a [```TimeSeries```](https://pynwb.readthedocs.io/en/stable/pynwb.base.html#pynwb.base.TimeSeries) object which is designed to store any general purpose time series data. This object has a few mandatory properties, like ```name```, ```data```, and ```unit``` and a few optional ones, like ```timestamps```, ```control```, ```control_description```, and ```description```. The ```data``` property can be stored as any 1-D, 2-D, 3-D, or 4-D array where the first dimension is time. We use it to store our ```pupilAreaSize``` numpy vector. The ```data_unit``` property has to be a string, while the ```timestamps``` property should give data sampling times and should be stored as a unidimensional numpy array. The ```control``` property is used for labeling data samples with integers. The length of this array should be the same as the length of the first data dimension representing time. In our case I am using integers 0 and 1 to mark acceptable quality data. The meaning of these labels is provided by the ```control_description``` property which is a string describing labels in the increasing order. The full code that converts the pupil area size data into the appropriate form is shown below:
```python
pupilAreaSize = TimeSeries(
  name='TimeSeries',
  data=pupilAreaSize.squeeze(),
  timestamps=videoFrameTimes.squeeze(),
  unit='pixels^2',
  control=acceptableSamples.squeeze(),
  control_description='low quality samples that should be excluded from analyses; acceptable quality samples',
  description='Pupil area size over the recording session measured in pixels^2. ' +
    'Acceptable quality period starting and ending times are given by data_continuity parameter. ' +
    'The full data range can be divided into multiple acceptable periods.'
)
behaviour_module = nwb.create_processing_module(name="behavior", description="contains behavioral data")
pupilTracking = PupilTracking(pupilAreaSize)
behaviour_module.add(pupilTracking)
```
As you will notice, our ```TimeSeries``` object ```pupilAreaSize``` is assigned to a [```PupilTracking```](https://pynwb.readthedocs.io/en/stable/pynwb.behavior.html#pynwb.behavior.PupilTracking) object which can hold one or more of the ```TimeSeries``` objects. We also create a [```ProcessingModule```](https://pynwb.readthedocs.io/en/stable/pynwb.base.html#pynwb.base.ProcessingModule) container object to store the bahavioural data using the ```NWBFile``` ```create_processing_module``` method (documented [here](https://pynwb.readthedocs.io/en/stable/pynwb.file.html?highlight=ProcessingModule#pynwb.file.NWBFile.create_processing_module)). The final line adds the ```PupilTracking``` object to the ```ProcessingModule``` object by invoking the ```add``` method (documented [here](https://pynwb.readthedocs.io/en/stable/pynwb.base.html#pynwb.base.ProcessingModule.add)).

(tutorials-silicon-probe-convert2nwb-py-movement)=
#### Add Behavioural Module: Total Facial Movement
The facial movement data conversion process into appropriate form for NWB storage is almost identical to the conversion process applied to the pupil area size data. First, we load the data and store it inside the ```TimeSeries``` object. The steps are identical. Next, we store our ```TimeSeries``` object inside the [```BehavioralTimeSeries```](https://pynwb.readthedocs.io/en/stable/pynwb.behavior.html?highlight=BehavioralTimeSeries#pynwb.behavior.BehavioralTimeSeries) object designed for holding one or more ```TimeSeries``` objects:
```python
behavioralTimeSeries = BehavioralTimeSeries(totalFaceMovement)
```
We then then add the ```BehavioralTimeSeries``` object to the ```ProcessingModule``` container object:
```python
behaviour_module.add(behavioralTimeSeries)
```

(tutorials-silicon-probe-convert2nwb-py-save)=
#### Save NWB File
We use the ```NWBFile``` [```NWBHDF5IO``` object](https://pynwb.readthedocs.io/en/stable/pynwb.html?highlight=NWBHDF5IO#pynwb.NWBHDF5IO) to save our data in the NWB format:
```python
if iSess < 9:
  nwbFilename = os.path.join(animalDerivedDataFolderNWB, 'ecephys_session_0' + str(iSess+1) + '.nwb')
else:
  nwbFilename = os.path.join(animalDerivedDataFolderNWB, 'ecephys_session_' + str(iSess+1) + '.nwb')
with NWBHDF5IO(nwbFilename, "w") as io:
  io.write(nwb)
```

(tutorials-silicon-probe-convert2nwb-py-read)=
#### Read NWB File
Reading NWB files in Python is done via the ```NWBHDF5IO``` class. Hence, import it by calling:
```python
from pynwb import NWBHDF5IO
```
Now if you want to open the NWB file that you just saved in Python, you can issue a command
```python
file_path = './npx_derived_data_nwb/M200324_MD/ecephys_session_01.nwb'
io = NWBHDF5IO(file_path, mode="r")
nwb2 = io.read()
```
which will read the file passively. The action is fast and it does not load all of the data at once but rather make it readily accessible. This is useful as it allows you to read data selectively without loading the entire file content into the computer memory.

If you want to read the entire unit table which has all unit spiking data and associated metadata, the easiest way is to issue a command
```python
units = nwb2.units.to_dataframe()
```
which will specifically load and convert the HDF5 ```DynamicTable``` object into a pandas ```DataFrame``` object. The latter is a common Python data type.

In contrast, if you are interested in a single unit data, you can load it using the following line of code:
```python
unitRowColLabels = units.columns.values
unitRowValues = units.iloc[0].values
```

To load only spiking data for the same unit, the following line of code will do:
```python
unitRowSpikeTimes = units["spike_times"][0]
```

If you want to load mean unit waveforms, the following line of code will load them as a numpy array:
```python
unitWaveforms = units["waveform_mean"].values
```

Meanwhile, behavioural data can be accessed by running the code below:
```python
pupilAreaSize = nwb2.processing['behavior']['PupilTracking']['TimeSeries'].data[:]
totalFacialMovement = nwb2.processing['behavior']['BehavioralTimeSeries']['TimeSeries'].data[:]
```
Other associated behavioural properties can be accessed by replacing the ```data``` property by ```timestamps``` or ```control``` and so on.

Some metadata is often directly available as properties of the ```NWBFile``` object, like:
```python
sessionDescription = nwb2.session_description
```
Subject metadata is available via the command, for example:
```python
animalID = nwb2.subject.subject_id
```
While the electrode metadata can be accessed in the following way:
```python
electrodesTable = nwb2.ec_electrodes.to_dataframe()
```
```electrodes``` table is a pandas ```DataFrame``` object and, therefore, methods of accessing it are the same as those discussed for accessing the ```units``` table.

(tutorials-silicon-probe-convert2nwb-py-validate)=
#### Validate NWB File
For validating NWB files use command line and type in
```
python -m pynwb.validate ./npx_derived_data_nwb/M200324_MD/ecephys_session_01.nwb
```
in order to validate the file that was recently created. The output should be:
```
Validating ./nnx_derived_data_nwb/M190114_A_MD/ecephys_session_01.nwb against cached namespace information using namespace 'core'.
 - no errors found.
```
The program exit code should be 0. On error, the program exit code is 1 and the list of errors is outputted.

For more details on the validation process, refer to an external [resource](https://pynwb.readthedocs.io/en/stable/validation.html).

(tutorials-silicon-probe-convert2nwb-py-resources)=
#### Resources
This section explained how you can use Python to convert your processed spiking and behavioural data into NWB files. It is not an exhaustive overview of PyNWB package extracellular electrophysiology functionality. There are other potential use cases involving local field potentials (LFPs) and different types of behavioural data. There is a number of externally available tutorials covering some aspects of converting data obtained in extracellular electrophysiology experiments to the NWB file format using Python:
- [NWB File Basics Tutorial](https://pynwb.readthedocs.io/en/stable/tutorials/general/file.html#sphx-glr-tutorials-general-file-py)
- [Extracellular Electrophysiology Data Tutorial](https://pynwb.readthedocs.io/en/stable/tutorials/domain/ecephys.html#sphx-glr-tutorials-domain-ecephys-py)
- [Reading and Exploring an NWB File Tutorial](https://pynwb.readthedocs.io/en/stable/tutorials/general/read_basics.html#sphx-glr-tutorials-general-read-basics-py)
- [Validating NWB files](https://pynwb.readthedocs.io/en/stable/validation.html)
- [PyNWB API Documentation](https://pynwb.readthedocs.io/en/stable/api_docs.html)
- [NWB Schema Overview](https://nwb-schema.readthedocs.io/en/latest/index.html)

(tutorials-silicon-probe-convert2nwb-hdfview)=
### Examine NWB File Structure Using HDFView
You can use [HDF View](https://www.hdfgroup.org/downloads/hdfview) software to visualise the structure of your NWB files using a simple graphical interface.