How to organize imaging data

## Start here

There are a few different types of imaging data we can get. The first distinction is the data source: clinical data from one of the hospital systems, research data from one of the hospital systems or research collaborators, and research data from trusted neuroimaging sharing platforms. The second distinction is the tier of data:

- Tier A: NIFTI (.nii.gz or .nii) images with JSON (.json) sidecars, or raw DICOM (.dcm) files that can be converted to NIFTIs with JSON sidecars
- Tier B: NIFTI images without JSON sidecars - we can still do some image analyses even if we don't have the metadata in the JSON files
- Tier C: a file containing imaging phenotypes measured via a specific image processing pipeline

All data should come with a metadata file specifying the patient/subject id, age at scan, sex, and diagnosis.

## Conventions

- No underscore characters in project directory names (helps with file path parsing further down the line)
- Age is in days post-birth unless other


## Adding a new data set

If the data is not on Respublica, work with the Data Team to get it there. The main data directory on our fileshare is located at `/mnt/isilon/bgdlab_resnas03/Data`. Within this directory are 4 category based directories:

- LBCC
- misc_clinical
- misc_research
- SLIP

Data sets downloaded for inclusion in the lifespan data set belong in LBCC, clinical controls from CHOP belong in SLIP, and other data sets (eg, clinical case cohorts or larger independent research studies like ABCD) belong in the respective `misc` folder.

Each data set gets its own data set folder within its category directory. For example, a data set called MiniatureSpork downloaded from the OpenNeuro platform for the lifespan project would be located at `/mnt/isilon/bgdlab_resnas03/Data/LBCC/OpenNeuro-MiniatureSpork/`. This data set also would be given an entry in the LBCC Dataset Status file. The individual who downloaded the data set would be responsible for adding the information about the data set and its participants to the Dataset Overview page.

