How to organize imaging data

## Start here

There are a few different types of imaging data we can get. The first distinction is the data source: clinical data from one of the hospital systems, research data from one of the hospital systems or research collaborators, and research data from trusted neuroimaging sharing platforms. The second distinction is the tier of data:

- Tier A: NIFTI (.nii.gz or .nii) images with JSON (.json) sidecars, or raw DICOM (.dcm) files that can be converted to NIFTIs with JSON sidecars
- Tier B: NIFTI images without JSON sidecars - we can still do some image analyses even if we don't have the metadata in the JSON files
- Tier C: a file containing imaging phenotypes measured via a specific image processing pipeline

All data should come with a metadata file specifying the patient/subject id, age at scan, sex, and diagnosis.

## Conventions

- No underscore characters in project directory names (helps with file path parsing further down the line)
- Data sets from certain websites are given site prefixes: OpenNeuro-, NDAR-, etc.
- Age is in days post-birth unless otherwise specified


## Adding a new data set

If the data is not on Respublica, work with the Data Team to get it there. The main data directory on our fileshare is located at `/mnt/isilon/bgdlab_processing/Data`. Within this directory are 4 category based directories:

- `LBCC`
- `misc_clinical`
- `misc_research`
- `SLIP`

Data sets downloaded for inclusion in the lifespan data set belong in LBCC, clinical controls from CHOP belong in SLIP, and other data sets (eg, clinical case cohorts or larger independent research studies like ABCD) belong in the respective `misc` folder.

Each data set gets its own data set folder within its category directory. For example, a data set called Miniature Spork downloaded for the lifespan project would be located at `/mnt/isilon/bgdlab_processing/Data/LBCC/MiniatureSpork/`. A data dump from Arcus for the SLIP project would be located at `/mnt/isilon/bgdlab_processing/Data/SLIP/YYYY-MM-DD_request` where `YYYY-MM-DD` is the date the request was submitted to Arcus.

Within a data set folder, the downloader should create 2 subdirectories and 1 README file. The directories are `code` and `orig`. Any code used to download or organize this data set belongs in the `code` folder. The original download belongs in the `orig` folder. In the README.md file, the downloader must add a line `Source:` specifying the source of the data and a dated note detailing what has been done to the data set on the current date. For example, the README.md for the hypothetical Miniature Spork data set would read as follows

> Project: Miniature Spork
> Source: theminiaturesporkproject.org
>
> 2023-09-19
> Downloaded the dataset from the source website.

and its directory structure would be

```
LBCC
|-- MiniatureSpork
|   |-- README.md
|   |-- code
|   |   |-- download_sporks.sh
|   |-- orig
|   |   |-- ...
```

**LBCC**: *PROCESS UNDER REVIEW* Each data set is also given an entry in the LBCC Dataset Status file. The individual who downloaded the data set is responsible for adding the information about the data set and its participants to the Dataset Overview page. The primary info is automatically populated in the Respublica Processing Status page. The downloader must also add a link to the source webpage, the date of the download, and their name as the downloader.

**NDAR Downloads**: For instructions on how to download data directly from the NDAR to Respublica, refer [here](https://bgdlab.github.io/informatics/nda-tools.html). 

## Examining a new data set

Before doing anything to a data set, we need to understand what data it contains and how it is currently structured. Every data set will be different in this regard (with the exception of most SLIP data).

The `tree` and `ls` commands are helpful for viewing the structure of a data set's directories. After determining the number of directory levels and the meaning of each level (subject, session, modality, derivatives, etc.), it is then important to view any available images for several subjects using FSLEyes (on Respublica run `module load fsl` before running `fsleyes` on the command line). Visual inspection of the images is important to confirm the presence of high-resolution isotropic structural scans, which are needed for many neuroimage processing pipelines.

At this stage, data sets can be assigned one of the following statuses:
- 1 Inspected Fail: the directory structures have been inspected and NO high-resolution isotropic structural scan was consistently identified between subjects
- 2 Inspected Pass: the directory structures have been inspected and high-resolution isotropic structural scans were found

Data sets with the second of these statuses then can be organized according to BIDs/BIDs-like structure.

*SLIP*: We have worked out a directory structure convention with Arcus. Every SLIP data set (and any clinical data set we get through Arcus) as of 2023-09 will have the same top 3 levels:

```
2023-09-01_slip_request
|-- HM100001_73456768156586_486
|   |-- GCPDicom_...
|   |   |-- 156.5746.018465.048645...
|   |   |   |-- ...
|-- HM185DJK_64769582851461_784
|   |-- GCPDicom_...
|   |   |-- 156.5746.018465.048645...
|   |   |   |-- ...
|-- HM185DJK_30456718645759_1247
|   |-- GCPDicom_...
|   |   |-- 156.5746.018465.048645...
|   |   |   |-- ...
```

where the `HM185DJK_30456718645759_1247` string consists of the anonymized subject identifier, the procedure order identifier, and the patient's age in days **post birth**. The first level down contains a dump of anonymized files associated with that set of information. For more details on how to organize SLIP and similar clinical data, see [the corresponding Github repo](https://github.com/BGDlab/dataorg-arcus).

