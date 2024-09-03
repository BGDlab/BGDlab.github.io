
# Datasets Overview

[LBCC](#lbcc)
* [Directory Structure](#directory-structure)
* [Documentation](#documentation)

[CHOP Imaging Datasets](#chop-imaging-datasets)
* [Directory Structure](#directory-structure-1)
* [Documentation](#documentation-1)

## LBCC

### Directory Structure 
Within a dataset, at the final stable/post-processing stage, the structure should be
```
LBCC
    -- dataset
        -- README.md
        -- data_dump
        -- code
            -- README
        -- BIDS
        -- rawdata
        -- derivatives
        -- README
```
#### Subdirectories

###### code
Any code used to download or organize this data set belongs in the code folder. 

###### datadump
All files downloaded from a dataset

###### rawdata
Not always present, used for when file conversion is necessary when the contents of a data dump are compressed file archives, dicoms, or some other file type that requires conversion to nifti

###### BIDS
BIDSifed data and participants file + dictionary

###### derivatives
Output of pre-processing (Synthseg) any post-processing pipelines


### Documentation
#### Respublica
In the `README` file in the dataset directory on Respublica, the downloader must include:
* Information on the download (source, download date, who downloaded the data)
* Data inspection (tier, inspection date)
* Notes descriptive of the contents of the data and any other important information
* Summary demographics including diagnoses (if applicable)
* Organization process (who organized it, organized date)

The template `README` can be found here: `/mnt/isilon/bgdlab_processing/LBCC/template_not_git/README_dataset.md`


###### Example
For example, the README.md for the hypothetical `Miniature Spork` data set would read as follows:

---

```Dataset: Miniature Spork 
**Data Source**
Data Source: theminiaturesporkproject.org
Downloaded: Dabriel Zimmerman
Download Date: 08/12/2024
**Inspection**
Data Tier: Tier A
Inspector: Dabriel Zimmerman
Inspection Date: 8/12/2024
Notes: Dataset downloaded for test purposes. Contains dummy imaging data. 
**Summary Demographics**
N Total = 5       | Mean Age (adjusted age in days) = 9131
N Male = 3        | Min Age (djusted age in days) = 8766
N Female = 2      | Max Age (djusted age in days) = 9496
**Organization Process**
Organizer: Dabriel Zimmerman
Organization Date: 8/12/2024```

---

A table summarizing the data present is kept in the `BIDS` directory along with a dictionary: 
* `participants.tsv`
* `participants.json`

#### Google Docs
In the [Lifespan](https://docs.google.com/spreadsheets/d/19KL7GbJLEuYkUS3hhQt7FdqGN9VwupKftvc8Ku1Mcgw/edit?gid=1454534495#gid=1454534495) spreadsheet, each dataset is given its own entry with summary demograhics and other relevant information **after** dataset has been peer reviewed. The individual who downloaded the data set is responsible for adding the information. 

#### ClickUp
This [list](https://app.clickup.com/9011141602/v/li/901102982820) tracks the status of LBCC datasets:
* **Requested**: Dataset requested and added to queue
* **Download**: Requested data downloaded from source onto Respublica
* **Organize**: Downloaded dataset is organized and BIDS complaint
* **Peer Review**: Organized dataset is peer reviewed to ensure dataset is up to code and documentation is complete
* **BABS Synthseg**: Reviewed datasets are processed using BABS Synthseg and phenotypes have been extracted 
* **Supplemental Processing**: Reviewed datasets have been processed with Synthseg and now are undergoind additional processing with custom pipelines (i.e. AFNI CT, eaCSF)
* **Dropped**: Dataset is dropped during any point of the organizing/pre-processing process

Additionally, data tier, assignee, and requestor are tracked in the list. 


## CHOP Imaging Datasets
There are currently 3 main sources of data from CHOP:
1. SLIP - scans requested from radiology with limited imaging pathology 
2. Clinical Imaging Genetics - ?? 
3. Nf1 - ??

SLIP data is contained in the `SLIP` directory. Clinical imaging genetics data is contained in `misc_clinical` as `YYYY_MM_genetics_patient_imaging`. Nf1 data is contained ??? (seems to be both in `misc_clinical` and `nf1`). **Non-SLIP CHOP data follows the same conventions/strcuture except where specified**.

#### Directory Structure
SLIP requests are delivered into the `dicoms` subdirectory with a manifest `YYYY_MM_requested_sessions_with_metadata.csv` listing all relevant metadata needed for organization. The [dataorg-arcus](https://github.com/BGDlab/dataorg-arcus) GitHup repo should be cloned here.

##### Subdirectories
```
SLIP
    -- dataset
        -- BIDS
        -- dataorg-arcus
        -- derivatives
        -- dicoms
        -- sourcedata
        -- rawdata-tmp
        -- YYYY_MM_requested_sessions_with_metadata.csv
```

###### dataorg-arcus
Cloned **dataorg-arcus** repo used for organization of the data.  

###### dicoms
Delivered dicoms from Arcus

###### sourcedata
Sorted dicoms 

###### rawdata-tmp
Intermediary directory for *heudiconv* processing

###### BIDS
BIDSifed data, participants file + dictionary, CuBIDS output

###### derivatives
Output of Synthseg and any other post-processing pipelines

###### heudiconv-fails
Not always present, appears if there are issues with heudiconv and it fails for specific sessions. 


#### SLIP deliveries contained in `dicoms`
###### W/ GCP garbage string
```
SLIP
    -- dataset
        -- dicoms
            -- HM10RYBAA_394518280517_6061
                -- GCP*
                    -- *
                        -- *
                            -- *dcm
        -- YYYY_MM_requested_sessions_with_metadata.csv
```
###### W/O GCP garbage string
```
SLIP
    -- dataset
        -- dicoms
            -- HM10RYBAA_394518280517_6061
                -- *
                    -- *
                        -- *dcm
         -- YYYY_MM_requested_sessions_with_metadata.csv
```
In the example `HM10RYBAA_394518280517_6061` string consists of the anonymized subject identifier, the procedure order identifier, and the patient's age in days post birth. The first level down contains a dump of anonymized files associated with that set of information.

#### Documentation

##### Respublica
A table summarizing the data present is kept in the `BIDS` directory along with a dictionary: 
* `participants.tsv`
* `participants.json`
##### ClickUp
This [list](https://app.clickup.com/9011141602/v/li/901103378470) tracks the status of CHOP imaging datasets (SLIP, CIG, NF1):
* **Requested**: Request initiated for data to be delivered from radiology
* **Download**: Requested data downloaded onto Respublica
* **Organize**: Downloaded data organized and BIDS complaint
* P**eer Review**: Organized datais peer reviewed to ensure dataset is up to code and documentation is complete
* **Stable**: Reviewed dataset is ready for post-processing and will not undergo changes at the `BIDS` level. 

Additionally, data tier, assignee, and requestor are tracked in the list. 