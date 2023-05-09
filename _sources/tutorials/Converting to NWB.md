(tutorials-nwb)=
# Converting to NWB
This is the index page gathering links to various aspects of [NWB](https://www.nwb.org/) file conversion for [silicon probe data](tutorials-silicon-probe), [intracellular electrophysiological recording data](tutorials-pclamp), and [data obtained using calcium imaging](tutorials-caimage).

(tutorials-nwb-install-matnwb)=
## Install MatNWB Toolbox
To begin using NWB format in Matlab, you will need to install [MatNWB toolbox](https://neurodatawithoutborders.github.io/matnwb/). To download the toolbox, type in your terminal:
```
git clone https://github.com/NeurodataWithoutBorders/matnwb.git
```
Move the downloaded repository to the folder where you keep your code libraries. Then type the following in the Matlab command line:
```matlab
cd matnwb
addpath(genpath(pwd));
generateCore();
```
You can now start using MatNWB. MatNWB interface documentation can be accessed [here](https://neurodatawithoutborders.github.io/matnwb/doc/index.html).

(tutorials-nwb-install-pynwb)=
## Install PyNWB
To begin using NWB format in Python, you will need to install PyNWB first. To do so, type in your terminal:
```
pip install -U pynwb
```
Now you're ready to start working with PyNWB. The full installation instructions are available [here](https://pynwb.readthedocs.io/en/stable/install_users.html).

(tutorials-nwb-record-metadata)=
## Record Metadata
[Record extracellular electrophysiology metadata in Matlab](tutorials-silicon-probe-convert2nwb-matlab-record-metadata) \
[Record intracellular electrophysiology metadata in Matlab](tutorials-pclamp-convert2nwb-matlab-record-metadata) \
[Record calcium imaging metadata in Matlab](tutorials-caimage-convert2nwb-matlab-record-metadata) \
[Record extracellular electrophysiology metadata in Python](tutorials-silicon-probe-convert2nwb-py-record-metadata) \
[Record intracellular electrophysiology metadata in Python](tutorials-pclamp-convert2nwb-py-record-metadata) \
[Record calcium imaging metadata in Python](tutorials-caimage-convert2nwb-py-record-metadata)

(tutorials-nwb-electrodes-table)=
## Create Electrodes Table
[Create Electrodes table in Matlab](tutorials-silicon-probe-convert2nwb-matlab-electrodes-table) \
[Create Electrodes table in Python](tutorials-silicon-probe-convert2nwb-py-electrodes-table)

(tutorials-nwb-units-table)=
## Create Units Table
[Create Units table in Matlab](tutorials-silicon-probe-convert2nwb-matlab-units-table) \
[Create Units table in Python](tutorials-silicon-probe-convert2nwb-py-units-table)

(tutorials-nwb-matlab-pupil-area)=
## Add Behavioural Module: Pupil Area Size
[Add pupil area module in Matlab](tutorials-silicon-probe-convert2nwb-matlab-pupil-area) \
[Add pupil area module in Python](tutorials-silicon-probe-convert2nwb-py-pupil-area)

(tutorials-nwb-matlab-movement)=
## Add Behavioural Module: Total Facial Movement
[Add movement module in Matlab](tutorials-silicon-probe-convert2nwb-matlab-movement) \
[Add movement module in Python](tutorials-silicon-probe-convert2nwb-py-movement)

(tutorials-nwb-convert-icephys)=
## Convert Intracellular Electrophysiology Recording Data
[Convert current and voltage clamp data in Matlab](tutorials-pclamp-convert2nwb-matlab-convert-icephys) \
[Convert current and voltage clamp data in Python](tutorials-pclamp-convert2nwb-py-convert-icephys) \
[Convert current clamp data recorded in parallel to calcium imaging in Matlab](tutorials-caimage-convert2nwb-matlab-convert-icephys) \
[Convert current clamp data recorded in parallel to calcium imaging in Python](tutorials-caimage-convert2nwb-py-convert-icephys)

(tutorials-nwb-recordings-table)=
## Create Intracellular Recordings Table
[Create intracellular recordings table in Matlab](tutorials-pclamp-convert2nwb-matlab-recordings-table) \
[Create intracellular recordings table in Python](tutorials-pclamp-convert2nwb-py-recordings-table)

(tutorials-nwb-group-sweeps-simultaneous)=
## Group Simultaneously Recorded Sweeps
[Group simultaneously recorded sweeps in Matlab](tutorials-pclamp-convert2nwb-matlab-group-sweeps-simultaneous) \
[Group simultaneously recorded sweeps in Python](tutorials-pclamp-convert2nwb-py-group-sweeps-simultaneous)

(tutorials-nwb-group-sweeps-sequential)=
## Group Sequentially Recorded Sweeps
[Group sequentially recorded sweeps in Matlab](tutorials-pclamp-convert2nwb-matlab-group-sweeps-sequential) \
[Group sequentially recorded sweeps in Python](tutorials-pclamp-convert2nwb-py-group-sweeps-sequential)

(tutorials-nwb-group-sweeps-repetitions)=
## Group Recordings into Runs
[Group recordings into runs in Matlab](tutorials-pclamp-convert2nwb-matlab-group-sweeps-repetitions) \
[Group recordings into runs in Python](tutorials-pclamp-convert2nwb-py-group-sweeps-repetitions)

(tutorials-nwb-group-sweeps-conditions)=
## Group Runs into Experimental Conditions
[Group runs into experimental conditions in Matlab](tutorials-pclamp-convert2nwb-matlab-group-sweeps-conditions) \
[Group runs into experimental conditions in Python](tutorials-pclamp-convert2nwb-py-group-sweeps-conditions)

(tutorials-nwb-add-image)=
## Add Grayscale and RGB Images
[Add recording slice image in Matlab](tutorials-pclamp-convert2nwb-matlab-add-image) \
[Add recording slice image in Python](tutorials-pclamp-convert2nwb-py-add-image) \
[Add static fluorescence images in Matlab](tutorials-caimage-convert2nwb-matlab-convert-images) \
[Add static fluorescence images in Python](tutorials-caimage-convert2nwb-py-convert-images)

(tutorials-nwb-create-img-planes)=
## Create Imaging Planes
[Create imaging planes in Matlab](tutorials-caimage-convert2nwb-matlab-create-img-planes) \
[Create imaging planes in Python](tutorials-caimage-convert2nwb-py-create-img-planes)

(tutorials-nwb-convert-fluorescence)=
## Convert Fluorescence Data
[Convert fluorescence data in Matlab](tutorials-caimage-convert2nwb-matlab-convert-fluorescence) \
[Convert fluorescence data in Python](tutorials-caimage-convert2nwb-py-convert-fluorescence)

(tutorials-nwb-convert-fluorescence-intensity-change)=
## Convert Change in Fluorescence Intensity Data
[Convert fluorescence intensity change data in Matlab](tutorials-caimage-convert2nwb-matlab-convert-fluorescence-intensity-change) \
[Convert fluorescence intensity change data in Python](tutorials-caimage-convert2nwb-py-convert-fluorescence-intensity-change)

(tutorials-nwb-save-matlab)=
## Save NWB File in Matlab
```matlab
nwbExport(nwbFileObj, <filename path string>);
```

(tutorials-nwb-save-py)=
## Save NWB File in Python
```python
from pynwb import NWBHDF5IO
with NWBHDF5IO(<filename path string>, "w") as io:
  io.write(nwbFileObj)
```

(tutorials-nwb-read)=
## Read NWB File
[Read a file with extracellular electrophysiology data in Matlab](tutorials-silicon-probe-convert2nwb-matlab-read) \
[Read a file with intracellular electrophysiology data in Matlab](tutorials-pclamp-convert2nwb-matlab-read) \
[Read a file with calcium imaging data in Matlab](tutorials-caimage-convert2nwb-matlab-read) \
[Read a file with extracellular electrophysiology data in Python](tutorials-silicon-probe-convert2nwb-py-read) \
[Read a file with intracellular electrophysiology data in Python](tutorials-pclamp-convert2nwb-py-read) \
[Read a file with calcium imaging data in Python](tutorials-caimage-convert2nwb-py-read)

(tutorials-nwb-validate)=
## Validate NWB File
For validating NWB files use command line and type in
```
python -m pynwb.validate <filename path>
```
The output for a valid NWB file should be:
```
Validating <filename path> against cached namespace information using namespace 'core'.
 - no errors found.
```
The program exit code should be 0. On error, the program exit code is 1 and the list of errors is outputted.

For more details on the validation process, refer to an external [resource](https://pynwb.readthedocs.io/en/stable/validation.html).