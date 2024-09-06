# The Lifespan Brain Chart Consortium
The Lifespan Brain Chart Consortium (LBCC) is a massive dataset of shared and open-access MRI images and derived phenotypes. It was first created by Jakob Seidlitz and Richard Bethlehem for the 2022 paper, [Brain charts for the human lifespan]((https://www.nature.com/articles/s41586-022-04554-y)) and has since been used for many projects across UPenn and the Universty of Cambridge.

## Adding Data
So, you found a cool new dataset in a paper or a data repository and you want to add it to the LBCC. Here's what to do:

### 1. Check if it's there.
First off, double-check whether we already have this dataset in the LBCC. All dataset/derivatives that are processed and ready to be used are listed on the [LBCC Data Release Spreadsheet](https://docs.google.com/spreadsheets/d/1bWK-eXVlHD2t757heWHzW1i-mKiXpzkbqWD0T5JaNIU/edit?gid=83134800#gid=83134800). However, it's also possible that someone else is currently working on adding this data, or that it was going to be added but was abandonded due to data quality, access agreements, etc. All this will be listed on the [Clickup Tracking List](https://app.clickup.com/9011141602/v/li/901102982820).

### 2. Fetch and Organize
All new LBCC data should be added to Respublica. We have instructions on how to add and organize new datasets [here](https://bgdlab.github.io/how-to/organize-imaging-data.html.). **IMPORTANT:** All action items (reaching out to PIs, submitting a data use agreement, downloading or processing data, etc.) should be listed and tracked in the LBCC's [Clickup Tracking List](https://app.clickup.com/9011141602/v/li/901102982820).

*Under development*

### 3. Process
*Under development*

## Using Data
The LBCC dataset is currently stored across two servers - the Cambridge server and Respublica, the CHOP server. The simplest way to access the data (in the form of derived phenotypes) is to use an existing data release. If you're adding new primary datasets, updating existing datasets, and/or adding new processing pipelines/phenotypes, you may want to create your own data release. 

### Using an Existing Release
A data release is a compilation of derived phenotypes, demographic information, etc, from all available studies into a single .csv file. Each data release is stored on Respublica at `/mnt/isilon/bgdlab_processing/Data/LBCC_releases` and is accompanied by a `README.md` file that describes how each release differs from the last one (i.e., adding datasets, correcting an error in an existing dataset, etc.). All data releases, as well as basic information about the primary studies/data they contain, are listed as unique tabs on the [LBCC Data Release Spreadsheet](https://docs.google.com/spreadsheets/d/1bWK-eXVlHD2t757heWHzW1i-mKiXpzkbqWD0T5JaNIU/edit?gid=83134800#gid=83134800). This is useful for seeing what data we have, and you can link to specific releases in your manuscript to easily show what data you used and how it was accessed, processed, etc. Releases are stable, which means that once it's compiled and added to the directory for everyone to use, it SHOULD NOT be altered in any way *(Q - can we enforce this with permissions somehow?)*. This also means that once you've decided on which data release you'd like to use, you can just point your script at the .csv and not have to worry about things changing on you. 

### Creating a New Release
If the existing dataset releases don't have what you need, you should first add the data, phenotypes, etc you want by following the instructions for `Adding Data` above. Then, you can create a new data release as follows.

#### Compiling Your Data Release
*Under development*

#### Naming & Documenting Your Data Release
Now that you have your dataset compiled, you should decide if this is a major or a minor release. For example, a minor release would be adding 2-3 small studies, re-processing existing data with a new pipeline, or adding a new clinical/demographic variable. Minor releases are indexed by a decimal place after the last major release (for example, if the latest release is version 3.0, the next minor release will be version 3.1). Major releases are indexed by the first digit (so for example, a new major release after 3.1 would be 4.0). We've also been naming these after cities, so feel free to name it after the place you're from, where you were when you compiled the data, etc.

Now that you've named your data release, update the .csv filename to reflect the release name. You'll now create 3 forms of documentation to go with your data release: a **Data Dictionary** file, an entry in the **README.md** file, and a new tab in the **[LBCC Data Release Spreadsheet](https://docs.google.com/spreadsheets/d/1bWK-eXVlHD2t757heWHzW1i-mKiXpzkbqWD0T5JaNIU/edit?gid=83134800#gid=83134800)**. Don't be daunted - most of this you can just get by copying the documentation for a prior release and making edits as needed. There's also an R script *(insert path here)* `/LBCC_release_documentation_template.Rmd` that you can use as a template for extracting information you need for the Data Dictionary and Data Release tab from your data release csv.

##### 1. Data Dictionary
First, you'll need to define all the variables/column names that you include in your spreadsheet. Some of them might just be raw regional measures derived from freesurfer, etc, in which case, you can just note this. However, if you transformed variables, ommitted certain diagnoses, etc, be sure to say so. Key things to note are the units you're using for age (and wheter it's age post-conception or regular old age post-birth). Since these releases concatenate across many studies, just an overall definition or explanation of an abbreviation is ok. If people need the gritty details, they can look at the primary study's source - just make sure you note any changes/variables that you combine to bridge the gap from that primary source to the data release.

##### 2. LBCC Data Release Tab
Next, you will need to make a new tab in the [LBCC Data Release Spreadsheet](https://docs.google.com/spreadsheets/d/1bWK-eXVlHD2t757heWHzW1i-mKiXpzkbqWD0T5JaNIU/edit?gid=83134800#gid=83134800) with all study information, pulling from existing releases and adding new variables as necessary. If you do add a variable, be sure to define it in the `DATA DICTIONARY` tab.

##### 3. README Entry
Finally, should also document the new release in `/mnt/isilon/bgdlab_processing/Data/LBCC_releases/README.md`, describing any changes that distinguish this release from the last one. 

Congratulations! You can now drop your data .csv in `/mnt/isilon/bgdlab_processing/Data/LBCC_releases` and move on with your life.

## Sharing Data
The LBCC data is governed by a lot of different agreements. **Always check with Aaron/Jakob/Richard before sharing any data**, both to make sure the data can be shared and for us to keep an overview of who is working on what. If a collaborator whats a big, global dataset's worth of derived phenotypes, the *easiest and safest* thing to do is to share one of the .csvs on the `BGDLabShare` Box drive under `lifespan_derived_phenos/sharable/`. These are data from all controls in open-access/public studies in the original LBCC dataset (with the exception of UKB data). Any further data sharing will have to be explicitly discussed to make sure data sharing agreements are in place.

## BONUS: IRB Approval/Exemption
The original brain charts project was deemed exempt by the CHOP IRB. When starting a new project, you should consider/ask about whether this project is also covered by this exemption or if it requires review. The same is also true for data use agreements! When in doubt, ask.

## Useful Links
+ [LBCC Data Release Spreadsheet](https://docs.google.com/spreadsheets/d/1bWK-eXVlHD2t757heWHzW1i-mKiXpzkbqWD0T5JaNIU/edit?gid=83134800#gid=83134800): Overview of all the currently-available, processed datasets.
+ [Bethlehem, Seidlitz, White et al, 2022](https://www.nature.com/articles/s41586-022-04554-y): The original brain charts paper introducing the LBCC. Has lots of good info about the datasets across various supplementary attachments.
  - [Supplementary Information](https://static-content.springer.com/esm/art%3A10.1038%2Fs41586-022-04554-y/MediaObjects/41586_2022_4554_MOESM1_ESM.pdf): Very long, detailed and hepful supplement. Includes brief descriptions of each primary dataset.
  - **Supplementary Table 1.1**: Downloadable as a set of .csvs, includes demographics, acquisition, and processing parameters for each study broken down by site.
+ [OG Dataset List](https://docs.google.com/spreadsheets/d/15UbnykpXnISXzgEiU9sECyuHXlqirY63V4oTwkuY-58/edit?gid=554339266#gid=554339266): Google sheet listing each dataset in the original LBCC paper and how they were obtained (i.e. OpenNeuro, NDAR, directly from author, ect.)
+ [Member List](https://docs.google.com/spreadsheets/d/1D8YNDcnhwlv2WcUDhreq3fwrkfpfGiFp0OGFVO5d-es/edit?gid=0#gid=0): Full list of consortium members and any relevant conflicts of interest.
+ [BrainChart App](https://brainchart.shinyapps.io/brainchart/): Public-facing app that allows you to interact with the dataset and brain charts from the original paper.
+ [Clickup Tracking List](https://app.clickup.com/9011141602/v/li/901102982820): List for to requesting new datasets and tracking their progress through processing. You will need to be given access to this.
+ [Prior Lifespan Dataset Overview](https://docs.google.com/spreadsheets/d/19KL7GbJLEuYkUS3hhQt7FdqGN9VwupKftvc8Ku1Mcgw/edit): big google spreadsheet created and used by Lena (as well as Jakob and Richard) to track data that was added/reprocessed after the original paper's publication.