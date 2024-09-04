# How to organize imaging data
##### Table of Contents  
[Contents of the Data Directory](#contents-of-the-data-directory)
[Conventions](#conventions)

[Curation](#data-curation-process)
* [Download](#downloading)
* [Inspection](#data-inspection)
* [Organization](#data-organization)
* [Peer Review](#peer-review)
* [Pre-processing](#pre-processing)
* [Stability/Post-processing](#stablepost-processing)


---

## Contents of the Data Directory

The main data directory on our fileshare is located at `/mnt/isilon/bgdlab_processing/Data`. Within this directory are our core 4 directories:

```
LBCC
misc_clinical
misc_research
SLIP
```

Data sets downloaded for inclusion in the lifespan data set belong in `LBCC`, clinical controls from CHOP belong in `SLIP`, and other data sets (eg, clinical case cohorts or larger independent research studies like ABCD) belong in the respective `misc` folder. Other directories contain data from various sources and are for various projects specific to the individual(s) who created them.

Each dataset gets its own folder within its category directory. For example, a data set called `Miniature Spork` downloaded for the lifespan project would be located at `/mnt/isilon/bgdlab_processing/Data/LBCC/MiniatureSpork/`. A release of data from Arcus for the SLIP project for a request from March of 2024 would be located at `/mnt/isilon/bgdlab_processing/Data/SLIP/slip_2024_03`.

An overview of both LBCC and SLIP datasets can be found here: [datasets_overview](datasets_overview.md)



## Conventions
* No underscore characters in project directory names (helps with file path parsing further down the line)
* Data sets from certain websites are given site prefixes: `OpenNeuro-`, `NDAR-`, etc.
* Data sets from Arcus are named based on their request: 
    * `slip_YYYY-MM`
    * `YYYY_MM_genetics_patients`
* Age is in days post-birth unless otherwise specified
* Filename structure (of scans and scan sidecars) must be BIDS compliant. For more info see [BIDS](https://bids-standard.github.io/bids-starter-kit/folders_and_files/files.html).
    * Must contain:
        * ses-**identifier**
        * run-**index**
    * Only alpha-numeric characters

---

## Data Curation Process
The general process for organizing any data for the lab is as follows:
1. [Download](#downloading)
    * [NDAR](#ndar-source)
    * [OpenNeuro](#from-openneuro)
2. [Inspection](#data-inspection)
    * [Classification](#data-classifications)
3. [Organization](#data-organization)
    * [NDAR](#ndar-data---lbcc)
    * [SLIP/CIG](#chop-imaging---slipcignf1)
    * [OpenNeuro](#openneuro---lbcc)
4. [Peer review](#peer-review)
    * [Checklist](#checklist)
5. [Pre-processing](#pre-processing)
6. [Stable/post-processing](#stablepost-processing)

---
### Downloading

#### NDAR Source
Downloading data from the NIMH Data Archive (NDA) to Respublica requires the use of *nda-tools*.
1. To set up the nda-tools  environment, see [instructions](https://bgdlab.github.io/informatics/nda-tools.html)
2. After the nda-tools  environment is set up, create the download package on NDAR with all the data you want to download:
    * Datasets **must** contain a table detailing information about the data provided for each subject. Usually this is `image03.txt` and the template scripts are based on the assumption that this table exists. Typically this will always be included in the package, but on the off chance that it isn't present, there is probably another table detailing the imaging included (`imagingcollection.txt` has come up before), or something went wrong with the download (i.e. none of the .txt files downloaded). 
    * Datasets should also contain a table (or tables) detailing demographic information collected (this is usually `ndarsubject01.txt`) but such information isn't always collected so won't always be available
3. Create a directory on Respublica for the dataset with a `data_dump` and `code` subdirectories.
```
LBCC
    -- dataset
        -- data_dump
        -- code
```
4. Once package is created, use the package id in the following commands:
```
cp /mnt/isilon/bgdlab_processing/Data/LBCC/template_not_git/ndaDownloadSubmit.sh $path/to/dataset/code
cd $path/to/dataset/code
bash ndaDownloadSubmit.sh $package_id $ndarUsername $path/to/dataset/data_dump
```
If this is your first time downloading with **ndatools** it may ask for your Respublica keyring (should have set that up when doing the environment setup) and/or your password to your NDA account. This password is **not** the password you use to login to NDA through login.gov, it is the password set on the NDA account itself (can change the password by going to your account and navigating to your profile page). 

#### From OpenNeuro
Once a dataset has been identified:
1. Go to the "Download" tab on the datasets' dashboard. 
2. Choose the "Download with a shell script" method to get a *.sh*  script. 
3. Create a directory on Respublica for the dataset with a `data_dump` and `code` subdirectories. 
```
LBCC
    -- dataset
        -- data_dump
        -- code
```
4. Either:
    * Upload the script to Respublica with `rsync -avz $source $path/to/dataset/code` 
    OR
    * Download the shell script from a browser on Respublica to the `code` subdirectory **(recommended for non-CHOP computers)**
5. Execute the script with the destination of the data being the `data_dump`  subdirectory


### Data Inspection
Before doing anything to a data set, we need to understand what data it contains and how it is currently structured. Every data set will be different in this regard (with the exception of most SLIP data).

The `tree` and `ls` commands are helpful for viewing the structure of a dataset's `data_dump`. After determining the number of directory levels and the meaning of each level (subject, session, modality, derivatives, etc.), it is then important to view any available images for several subjects using FSLeyes:
```
module load fsl
fsleyes sub_01/nifti/*.nii.gz
```
Visual inspection of the images is important to confirm the presence of high-resolution isotropic structural scans, which are needed for many neuroimage processing pipelines, check for any oddities in image orientation, etc. 

#### Data Classifications
There are a few different types of imaging data we can get. The first distinction is the data source: 

1. clinical data from one of the hospital systems
2. research data from one of the hospital systems or research collaborators
3. research data from trusted neuroimaging sharing platforms. 

The second distinction is the tier of data:

* Tier A: NIFTI (.nii.gz or .nii) images with JSON (.json) sidecars, or raw DICOM (.dcm) files that can be converted to NIFTIs with JSON sidecars
* Tier B: NIFTI images without JSON sidecars - we can still do some image analyses even if we don't have the metadata in the JSON files
* Tier C: a file containing imaging phenotypes measured via a specific image processing pipeline
* Tier F: data is not up to standard to use, indicative to missing metadata files, corrupt files, undesired imaging acquisitions/types, etc. 
* Tier Unknown: Automatic tier given to an unexplored dataset 

All data should come with a metadata file including the following information:
| Mandatory | Only if Applicable |
|------:|:------:|
|patient/subject id|session id|
|age at scan|diagnosis|
|sex||

Datasets should be classified based on the above criteria before moving to the next step. Any notes about the inspection should be added in the datasets `README`. Any Tier F dataset should be removed from Respublica and 
1. Dataset listed as Tier F with "dropped" status in the ClickUp tracker list
2. If LBCC: 
    * Dataset listed in the [spreadsheet](https://docs.google.com/spreadsheets/d/19KL7GbJLEuYkUS3hhQt7FdqGN9VwupKftvc8Ku1Mcgw/edit?gid=1454534495#gid=1454534495) as "0 Abandoned" for its `Lifespan Version`

### Data Organization
All data has to be organized into a structure that is BIDS compliant. See [Conventions](#conventions) for more information. The remainder of this section will provide examples of directory structures for Tier A and Tier B datasets and explain how each dataset went from original download to final organized structure.


#### Example BIDS from  Tier A dataset:
```
BIDS
    -- sub_01
        -- ses_01
            -- anat
                -- sub_01-ses_01-run-001_T1w.nii.gz
                -- sub_01-ses_01-run_001_T1w.json
```
There are currently 2 main pipeline for data organization based on the source of data
* [NDAR](#ndar-data---lbcc)
* [SLIP/CIG](#chop-imaging---slipcignf1)
    * [CHANGES](#changes)

#### NDAR Data - LBCC
A [GitHub Repo](https://github.com/BGDlab/data-org) is maintained containing template scripts to use to organize data from NDAR. Due to uniqueness of each NDAR dataset, the templates are not a guarantee to work beyond the first step `buildBidsDir.py` script. See the repo README for instructions for running the scripts. It may be desired to use tools like *heudiconv* and *CuBIDS* for organization, but the scripts don't use any tools other than *dcm2niix*. 

For all NDAR datasets, subject identifiers are generated from the **subjectkey** column containing a string beginning with NDAR found in the `image03.txt` table. Most NDAR datasets contain this exact table, but some datasets may contain an alternative file called `imagingcollection.txt`. 

Data downloaded from NDAR may be compressed into various filetypes. Data team has come across several types and have adapted the template scripts to handle these filetypes:
* .nhdr
* .nrrd
* . tgz
* .zip

###### Example: NDAR-LBCC Dataset (Tier A)
```
dataset
    -- BIDS
        -- sub-NDAR1
            -- ses-01
                -- anat
                    -- sub-NDAR1_ses-01_run-001_T1w.nii.gz
                    -- sub-NDAR1_ses-01_run-001_T1w.json
        -- sub-NDAR2
            -- ses-01
                -- anat
                    -- sub-NDAR2_ses-01_run-001_T1w.nii.gz
                    -- sub-NDAR2_ses-01_run-001_T1w.json
    -- code
        -- README.md
        -- data-org
            -- ...
    -- data_dump
        -- image03.txt
        -- NDAR1.tgz
        -- NDAR2.tgz
    -- rawdata
        -- untarred_NDAR1
            -- NDAR1_T1w.nii.gz
            -- NDAR1_T1w.json
        -- untarred_NDAR2
            -- ...dcm
            -- ...dcm
    -- README.md
```
In the example, data arrived in tarballs (compressed file archives) containing nfiti/json pairs and some dicoms. When data comes in compressed, contents are extracted to a new `rawdata` directory. If dicoms are contained in the archive, the dicoms stay in the `rawdata` and the converted niftis go into their appropriate `BIDS` directory. If nifti/json pairs in the archive, they get copied to their appropriate `BIDS` directory. There is a main dataset **README** and a **README** for the `code` directory where all scripts and commands are documented. The main dataset **README** must note how the data arrived (nifti and/or dicom). 

###### Example: NDAR-LBCC Dataset (Tier B)
```
dataset
    -- BIDS
        -- sub-NDAR1
            -- ses-01
                -- anat
                    -- sub-NDAR1_ses-01_run-001_T1w.nii.gz
                    -- sub-NDAR1_ses-01_run-001_T1w.json
        -- sub-NDAR2
            -- ses-01
                -- anat
                    -- sub-NDAR2_ses-01_run-001_T1w.nii.gz
                    -- sub-NDAR2_ses-01_run-001_T1w.json
    -- code
        -- README.md
        -- data-org
            -- ...
    -- data_dump
        -- image03.txt
        -- NDAR1*_T1w.nii.gz
        -- NDAR2*_T1w.nii.gz
    -- README.md
```
In the example, data arrived uncompressed with just niftis, no json sidecars. In these instances, some metadata can be gleaned from the `image03.txt` table and json sidecars created with the `createJson.py` script in the GitHub repo. The main dataset **README** must note that the data arrived as nifti only (Tier B) and that json sidecars were created with a script pulling in data from `image03.txt` table. The `code` **README** would list the scripts/commands run. 



#### CHOP Imaging - SLIP/CIG/NF1
A [GitHub Repo](https://github.com/BGDlab/dataorg-arcus) is maintained containing scripts used in organizing CHOP imaging data. This pipeline employs *heudiconv* and *CuBIDS*. 
##### Before 
##### After

###### Example: SLIP Release post September 2024
```
slip_YYYY_MM
    -- BIDS
        -- CHANGES
        -- code
            -- CuBIDS
                -- ..
        -- dataset_description.json
        -- participants.json
        -- participants.tsv
        -- README
        -- scans.json
        -- sub-HM3WPPESR
            -- ses-439563557339procId006437ageDays
                -- anat
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-MPRAGEStandardized3TScannerId45886Slices192Voxel0p898mm_run-001_T1w.nii.gz
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-MPRAGEStandardized3TScannerId45886Slices192Voxel0p898mm_rec-NORM_run-001_T1w.json
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSEStandardized3TScannerId45886Slices98Voxel0p898mm_rec-NORM_run-001_T2w.nii.gz
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSEStandardized3TScannerId45886Slices98Voxel0p898mm_rec-NORM_run-001_T2w.json
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSE3TScannerId45886Slices100Voxel0p898mm_rec-NORM_run-001_T2w.nii.gz
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSE3TScannerId45886Slices100Voxel0p898mm_rec-NORM_run-001_T2w.json
        -- dicoms
            -- HM3WPPESR_439563557339_6437
                -- GCP*
                    -- *
                        -- *
                            -- *dcm
    -- sourcedata
        -- HM3WPPESR
            -- 439563557339procId006437ageDays
                -- *
                    -- *
                        -- *dcm
    -- rawdata-tmp
    -- heudiconv-fails
    -- dataorg-arcus
        -- ...
    -- YYYY_MM_requested_sessions_with_metadata.csv

```
In the example, CuBIDS processing outputs the new files: `participants.json`, `participants.tsv`,`dataset_description`,`CHANGES`, and the `code` subdirectory. See [CuBIDS](https://cubids.readthedocs.io/en/latest/about.html) for more information on CuBIDS output. 
**Note** In new releases of SLIP, the above example will be how files are named. Previous versions of SLIP (as of 8/12/24) are in the following format. 
###### Example: SLIP Release prior to September 2024
```
slip_YYYY_MM
    -- BIDS
        -- CHANGES
        -- code
            -- CuBIDS
                -- ..
        -- dataset_description.json
        -- participants.json
        -- participants.tsv
        -- README
        -- scans.json
        -- sub-HM3WPPESR
            -- ses-439563557339procId006437ageDays
                -- anat
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-MPRStandardized3.0TScannerId45886Slices192Voxel0.898mm_run-0001_T1w.nii.gz
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-MPRStandardized3.0TScannerId45886Slices192Voxel0.898mm_run-0001_T1w.json
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSESVariant3.0TScannerId45886Slices98Voxel0.898mm_run-0001_T2w.nii.gz
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSESVariant3.0TScannerId45886Slices98Voxel0.898mm_run-0001_T2w.json
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSE_run-0001_T2w.nii.gz
                    -- sub-HM3WPPESR_ses-439563557339procId006437ageDays_acq-TSE_run-0001_T2w.json
        -- dicoms
            -- HM3WPPESR_439563557339_6437
                -- GCP*
                    -- *
                        -- *
                            -- *dcm
    -- sourcedata
        -- HM3WPPESR
            -- 439563557339procId006437ageDays
                -- *
                    -- *
                        -- *dcm
    -- rawdata-tmp
    -- heudiconv-fails
    -- dataorg-arcus
        -- ...
    -- YYYY_MM_requested_sessions_with_metadata.csv

```
###### Changes in SLIP Organization September 2024
Make note of the updates:
* MPR -> MPRAGE
* Decimal point changed to 'p':
    * 3.0T -> 3T
    * 1.5T -> 1p5T
    * Voxel0.898mm -> Voxel0p898mm
* Addtional BIDS key/value pairs added to filename
    * rec-**NORM/DIS2D/DIS3D/PROPELLER/FIL/MFSPLIT** 
        * The reconstruction method taken from the `ImageType` metadata field 
    * ce-**gad** 
        * Indicative of a scan as post contrast ("gad" in reference to gadolonium, the standard contrast agent from `ContrastBolus/Agent` metadata field)
    * mt-**on**
        * Denotes scans were magentization transfer was used (as defined with "MTS" in `ScanOptions` metadata field)
* Scans outside of the CHOP Standard MPRAGE are labeled as *Standardized* or *StandardizedVariant* based on if their parameters closely match the parameters of common protocols listed in the `parameter_ranges.tsv` table. 
* "FatSat" added to end of the *acq* key/value pair
    * Denotes scans where fat saturation was used (as defined with "FS" in `ScanOptions` metadata field)
* "MTS" in *acq* key/value pair
    * Denotes scans were magentization transfer was used (as defined with "MTS" in `ScanOptions` metadata field)
* Run number 3 total digits instead of 4


#### OpenNeuro - LBCC
In general, datasets on OpenNeuro are already BIDS compliant. However, sometimes they need some tweaking to match the format that we desire for the lab. Scripts from either repos for NDAR or Arcus data org can be used as templates/starting off points. 

###### Example: OpenNeuro-LBCC Dataset (Tier A)
```
dataset
    -- BIDS
        -- sub-001
            -- ses-01
                -- anat
                    -- sub-001_ses-01_run-001_T1w.nii.gz
                    -- sub-001_ses-01_run-001_T1w.json
        -- sub-002
            -- ses-01
                -- anat
                    -- sub-002_ses-01_run-001_T1w.nii.gz
                    -- sub-002_ses-01_run-001_T1w.json
    -- code
        -- README.md
        -- sort.sh
    -- data_dump
        -- participants.tsv
        -- sub-001
                -- anat
                    -- sub-001_T1w.nii.gz
                    -- sub-001_T1w.json
        -- sub-002
                -- anat
                    -- sub-002_T1w.nii.gz
                    -- sub-002_T1w.json
    -- README.md
```
In the example, the data has already been BIDSifed. However, our conventions require `run-00X` and `ses-0X` in the filename so a script `sort.sh` was custom built to bring the data up to standard. In the main README it is noted how the data arrived (nifti/json pair).
###### Example: OpenNeuro-LBCC Dataset (Tier C)
```
dataset
    -- derivatives
        -- mri_segs.csv
    -- code
        -- README.md
    -- data_dump
        -- participants.tsv
        -- mri_segs.csv
        -- README
    -- README.md
```
In the example, data featured a csv consisting of derivative volume segmentation values and no raw or converted data. The table with phenotypes is be stored in a new `derivatives` directory. Even though no code was needed for organization, the `code` directory should still have a README. The main dataset README would note the tier of the data (Tier C). 

### Peer Review
The peer review process involves reviewing the dataset directory to ensure that conventions are met, documentation is updated, and that data is ready for pre-processing.

#### Checklist
- [ ] *run-00X* in filename
- [ ] *ses-0X* as subdirectory of *sub-X* and present in filename
- [ ] README descriptive of dataset, organization process and source data content
- [ ] filenames completely alpha-numeric
    * exception to this for older releases of SLIP where decimal points are currently in the filename
- [ ] Static copy of all code used in a `code` subdirectory or standard repo directory (i.e. `dataorg-arcus` or `data-org`)
- [ ] External documentation/ClickUp tracker updated 

### Pre-processing
Data is generally pre-processed with BABS+Synthseg. See dynamic [Data Processing](https://doc.clickup.com/9011141602/p/h/8chp6z2-9011/da15d609f547742) for most up to date instructions. 

### Stable/post-processing
Datasets are moved to this status once all pre-processing has occurred. Meaning that there will be no changes to the `BIDS` directories and that Synthseg phenotypes have been extracted into the `derivatives` directory.











