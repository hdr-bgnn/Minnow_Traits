[![DOI](https://zenodo.org/badge/470771551.svg)](https://zenodo.org/badge/latestdoi/470771551)

# Minnow Segmented Traits

We use a segmentation model to extract traits from minnows (Family: Cyprinidae).

This repository serves as a case study of an automated workflow and extraction of morphological traits using machine learning on image data. 

We expand upon work already done by BGNN, including metadata collection by the [Tulane Team](https://bgnn.tulane.edu/) and the [Drexel Team](https://github.com/hdr-bgnn/drexel_metadata) (see [Leipzig et al. 2021](https://link.springer.com/chapter/10.1007/978-3-030-71903-6_1), [Pepper et al. 2021](https://ieeexplore.ieee.org/abstract/document/9651834?casa_token=gzgYa9cfbZAAAAAA:mFhU1Wc4bkBbL066-2Iwsec-eY2u_1h4FfgoDgGMnNqS5NLOTsJ0Jn78GOzU7tbbz4J-sw), and [Narnani et al. 2022](https://www.researchsquare.com/article/rs-1506561/latest.pdf)), and a segmentation model developed by the [Virginia Tech Team](https://github.com/hdr-bgnn/BGNN-trait-segmentation). We developed morphology extraction tools ([Morphology-analysis](https://github.com/hdr-bgnn/Morphology-analysis)) with the help of the Tulane Team. We incorporate these tools into [BGNN_Core_Workflow](https://github.com/hdr-bgnn/BGNN_Core_Workflow).

Finally, with the help of the Duke Team, we create an automated workflow.


![workflow](https://github.com/hdr-bgnn/Minnow_Segmented_Traits/blob/readme-edits/workflow%20use%20case.png)


## Goals

* Create a use case for using an automated workflow
* Show best practices for interacting with other repositories
* Show utility of using a machine learning segmentation model to accelerate trait extraction from images of specimens


## Organization

*Scripts*
- [Data_Manipulation.R](https://github.com/hdr-bgnn/Minnow_Traits/blob/streamline/Scripts/Data_Manipulation.R): code for manipulating and merging data files
- [Minnow_Selection_Image_Quality_Metadata.R](https://github.com/hdr-bgnn/Minnow_Traits/blob/streamline/Scripts/Minnow_Selection_Image_Quality_Metadata.R): code for image selection
- [Presence_Absence_Analysis.R](https://github.com/hdr-bgnn/Minnow_Traits/blob/streamline/Scripts/Presence_Absence_Analysis.R): code for analyzing machine learning outputs
- [init.R](https://github.com/hdr-bgnn/Minnow_Traits/blob/streamline/Scripts/init.R): code to load functions in [Functions](https://github.com/hdr-bgnn/Minnow_Traits/tree/streamline/Scripts/Functions)

*Files*
- [Previous_Measurements](https://github.com/hdr-bgnn/Minnow_Segmented_Traits/blob/streamline/Files/Previoius_Measurements.xlsx): a file of measurements of minnow traits by  found in the supplemental information

*Results*
- a folder for the outputs from the workflow
  1. tables of results from analyses
  2. /Figures contains all figures created from analyses

*Config*
- contains the config.yml file
  - the user can change the file inputs or number of images under ```limit_images```

## Inputs

### Data Files

All input files are stored in the [Fish Traits](https://covid-commons.osu.edu/dataverse/fish-traits) dataverse hosted by OSU.


### Components

The total size of the components are 5.6G (as of 5 May 2023). 

All weights and dependencies for all components of the workflow are stored in the [Fish Traits](https://covid-commons.osu.edu/dataverse/fish-traits) dataverse hosted by OSU.

* Metadata by Drexel Team
  - Object detection of fish and rule from fish images
  - [Repository](https://github.com/hdr-bgnn/drexel_metadata)
  - [Model Weights](https://covid-commons.osu.edu/dataset.xhtml?persistentId=doi:10.5072/FK2/MMX6FY&version=DRAFT)
  
* Reformatting of metadata
  - Trim metadata output from Metadata step to only the values necessary for this project
  - [Repository](https://github.com/hdr-bgnn/drexel_metadata_formatter)

* Crop Image
  - Extract bounding box information from metadata file
  - Resizes and crops fish from image
  - [Repository](https://github.com/hdr-bgnn/Crop_image)

* Segmentation Model by Virginia Tech Team
  - Segments fish traits from fish images
  - [Repository](https://github.com/hdr-bgnn/BGNN-trait-segmentation)
  - [Pretrained Model Weights](https://covid-commons.osu.edu/dataset.xhtml?persistentId=doi:10.5072/FK2/CGWDW4)
  - [Trained Model Weights](BGNN-trait-segmentation)

* Morphology analysis by Tulane Team and Battelle Team
  - Tool to calculate presence of traits
  - [Repository](https://github.com/hdr-bgnn/Morphology-analysis)

* Machine Learning Workflow by Battelle Team and Duke Team
  - Calls all the above containers
  - [Repository](https://github.com/hdr-bgnn/BGNN_Core_Workflow)


### Images

The fish images are from the Great Lakes Invasives Network [(GLIN)](https://glin.com/) and stored on [Fish-AIR](https://fishair.org/). 
We are using images specifically from the [Illinois Natural History Survey](https://inhs.illinois.edu/) [(INHS images)](http://www.tubri.org/HDR/INHS/).


#### Image Selection
    
R code (Minnow_Selection_Image_Quality_Metadata.R) was used to filter out high quality, minnow images using the IQM and IM metadata files.

All image metadata files are downloaded from [Fish-AIR](https://fishair.org/) and the version used is stored on the OSC data commons under the Fish Traits dataverse. The metadata files have been generated using the [Tulane worflow](https://bgnn.tulane.edu/).

Criteria for selection of an image was based on findings from [Pepper et al. 2021](https://ieeexplore.ieee.org/abstract/document/9651834?casa_token=gzgYa9cfbZAAAAAA:mFhU1Wc4bkBbL066-2Iwsec-eY2u_1h4FfgoDgGMnNqS5NLOTsJ0Jn78GOzU7tbbz4J-sw).

Criteria chosen:

* family == "Cyprinidae"
* specimenView == "left"
* specimenCurved == "straight"
* allPartsVisible == "True"
* partsOverlapping == "True"
* partsFolded == "False"
* uniformBackground == "True"
* partsMissing == "False"
* brightness == "normal"
* onFocus == "True"
* colorIssues == "none"
* containsScaleBar == "True"
* from either INHS or UWZM institutions

**The resulting dataset of 18 species and 1021 images.**


### Analysis

See more details in [Morphology-analysis](https://github.com/hdr-bgnn/Morphology-analysis).

Each segmented image has the following traits: trunk, head, eye, dorsal fin, caudal fin, anal fin, pelvic fin, and pectoral fin. For each segmented trait, there may be more than one "blob", or group of pixels identifying a trait. We created a matrix of <a href="https://github.com/hdr-bgnn/minnowTraits/blob/main/Files/presence.absence.matrix.csv"> presence.absence.matrix.csv</a>.

For each trait, we counted the number of "blobs" and the percentage of the largest blob as a proportion of all blobs for a trait.

All intermediate tables will be saved in the folder "Results".


#### Figures

We created a heat map to show the success of the segmentation to detect traits from the images.

Figures are in the folder "Results".


## Running the Workflow

This workflow requires R, conda, and docker to run.

### Installing snakemake

To run the workflow we use snakemake.
See the [official instructions for installing snakemake](https://snakemake.readthedocs.io/en/stable/getting_started/installation.html).

To install snakemake on the OSC cluster run:
```
module load miniconda3
conda create -n snakemake -c bioconda -c conda-forge -c r snakemake r-essentials r-xml -y
```

where `-n` designates the name, "snakemake", and `-c` designates the channel(s): "bioconda", "conda-forge", and "r".

To check that the environment was made:
```
conda info -e
```

### Installing R packages

The R packages required by the pipeline must be installed into the `Library` directory that is created.
This can be accomplished by running `Rscript dependencies.R`.

On the OSC cluster this can be done like so:

```
mkdir Library
module load cmake #defaukts to version on node
module load R/4.2.1-gnu11.2
Rscript dependencies.R
```

### Dataverse configuration

To download the unpublished input files from our Dataverse instance requires
supplying the URL to the instance and a Dataverse API Token.
The URL and API Token need to be placed into a config file stored in your home
directory name ".dataverse".

To find your token visit [datacommons.tdai.osu.edu]( https://datacommons.tdai.osu.edu/),
after logging in click your name in the top right corner, click API Token.
You should see a screen for managing your API Token.

Then run the following command to create your config file:

```
singularity run docker://ghcr.io/imageomics/dataverse-access:0.0.3 dva setup
```

This command will download the dva container and create your `~/.dataverse` config file.
You will see two prompts. For the `URL` prompt enter `https://datacommons.tdai.osu.edu/`.
For `API Token` prompt enter your API token.

```
Enter Dataverse URL: https://datacommons.tdai.osu.edu/
Enter your Dataverse API Token: <yourtoken>
```

### Limit images

In the config.yml file, the user can limit the number of images for a test run by change the integer under ```limit_images```, or run them all by entering ```""```.

### Running snakemake

Activate snakemake:

```
source activate snakemake
```

To run on a local computer:

After activating R and snakemake the pipeline can be run using `snakemake --cores 1`.

To run on the OSC cluster:

```
sbatch run-workflow.sh
```

To check the status of the job:

```
squeue -u $USER
```
