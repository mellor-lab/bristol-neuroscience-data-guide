(tutorials-silicon-probe)=
# Bristol GIN for Silicon Probe Data

(tutorials-silicon-probe-create-repo)=
## Create a Repository to Store Your Research Data
The example repository is organised according to the [Tonic Research Project Template](https://github.com/tonic-team/Tonic-Research-Project-Template). You can organise your data according to this template by following a few simple steps. First, go to your GIN web page (GIN-domain-name/your-username) and click on the Import Repository button.
```{image} ../assets/images/tutorials/bristol-gin-for-silicon-probe-data/import-repo.png
:name: fig-gin-silicon-import-repo
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Import a Repository**

You should be brought to the Import Repository page. As a Clone Address specify the Tonic template github page: https://github.com/tonic-team/Tonic-Research-Project-Template. Give the name to your repository and a concise description of the kind of data stored in the repository. In my case I am creating a repository containing dual silicon probe recordings of spontaneous neural activity in various brain areas of the mouse. Then click the green Migrate Repository button.
```{image} ../assets/images/tutorials/bristol-gin-for-silicon-probe-data/import-repo2.png
:name: fig-gin-silicon-import-repo2
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Describe Your New Repository**

You should see the template contents cloned under your new repository name.

Now open the terminal and navigate to the folder on your local file system where you would like to keep your research data. Download the remote repository to that location by typing this command:
```
gin get dervinism/infraslow-dynamics
```
You can now use this empty repository to store all of your newly acquired research project data and documents. If you already have data that was generated in the past, you can copy it here.

## Set up Your Research Data Repository
I am going to copy old research data to this newly created repository and make certain rearrangements within the existing folder structure to make it more suitable for already existing data structures created while carrying out my research project. I am also going to use mock research data to reduce the size of the repository so that repository management actions can be performed fast for this tutorial. The [mock data repository](https://gin.g-node.org/dervinism/mock-ecephys-project) is available to download on GIN. You can download it by typing the line below in your terminal:
```
gin get dervinism/mock-ecephys-project
```
Once downloaded, open the repository and copy the contents of the repository except for the .git folder. Delete the contents of the infraslow-dynamics repository and paste the copied contents from the mock-ecephys-project repository. Edit the README file accordingly to reflect the new name of the repository and other info.