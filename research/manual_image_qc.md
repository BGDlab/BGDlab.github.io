*Structural Image QC*

These guidelines were last updated 2022-11-07.

## Overview

The purpose of this QC step is to have as many people as possible look at structural brain MRIs and provide an informed but quickly formed opinion about the quality of a scan. To prevent overwhelm as well as combat any sort of grading order bias, the images have been randomly divided into batches stratified by age group (neonatal/infant brains look different from older kid brains). Each batch is designed to take about 30 minutes to fully grade.

Details:
- For each scan, 3 slices (pseudorandomly chosen at approximately the 25\%, 50\%, and 75\% slices) will be displayed for a single view (saggittal, axial, or coronal). 
- All 9 slices for a single scan are contained in the same batch.
- All images in a batch MUST be graded in the same sitting.
- Every grader will grade every batch, but the batch order and the order in which the images are displayed are both randomized.
- All of the grades will be averaged to get a final QC score.

## Grading criteria 

The grading follows a 0/1/2/-1 system with each rating having the following meaning:
- 0: poor quality, ringing, high degree of motion
- 1: not sure/maybe usable
- 2: good quality
- -1: a flag indicating “heck don't include this scan but not for quality reasons” images. Think scans that aren’t of the brain, are post contrast (bright CSF), or are magically not MRI.

Things to consider when looking at an image:
- Tissue contrast: is the "edge" between gray matter and white matter clear? Are the cortical folds distinct?
- Movement artifacts: is the image blurry? Does it look like two (or more) images with the brain in slightly different positions are being displayed on top of each other?
- Ringing: does it look like someone dropped a pebble into a pool of water? 

## Important note: age based lack of tissue differentiation

There is a phenomenon that occurs in T1 weighted images of infants. In particularly young patients, the brightness of white and grey matter is flipped. Starting around 6 months of age (~180 days), the tissue properties begin to change with the end result that gray matter shows up gray in T1w images and white matter shows up white in T1 weighted images. In the manual QC, difficulties identifying gray and white tissue due to this phenomenon are referred to as “age based lack of tissue differentiation” and are labeled as 1.


Additional details about these guidelines are available with rating examples in a document, but Jenna didn't feel comfortable posting it directly on this page so message her to get access to it.
CLIP Manual Image QC Guidelines and Process

# Overall Purpose

- Manually examine snapshots of CLIP images to confirm the quality of the data.
- Ratings are aggregated across raters

# Image QC Grading Guidelines

Jenna put in stuff from the powerpoint in here

# Image QC Grading Process

9 PNGs per image, stratified by age and divided into batches that should take about 30 minutes to rate

# Using the Jupyter Notebook on Respublica

*Set Up*

Get your eResearch account set up first (see [Respublica](clusters/respublica.md))

*Running the Notebook*

- Clone the code
- Connect to Respublica in the browser
- Start an interactive Jupyter notebook session
- A new tab will open in your browser showing your home directory on Respublica. Click on the directory named "repo", then double click the file "filename.ipynb" to open the notebook.
- The notebook will look like this...
- It is composed of cells. Gray cells are called code cells. Code cells can be executed by clicking the "play" button, selecting Run Cell from the Cell menu, or using the keyboard shortcut Shift Enter.
- This notebook is set up with ...
- Edit this parameter in the first cell
- Run these cells
- The first cell does XYZ
- The second cell prints the next report. Use the Guidelines above to determine which category this report falls into. Type your rating in the text box and hit enter
- When you have finished the batch, a message will appear at the bottom of the notebook. If it's "error message", stop rating and contact Jenna


*Troubleshooting*

Problem: The notes disappeared

Potential Solution 1:
- Click save
- Restart kernel
- Run the two code blocks

Potential Solution 2:
- Double click on the bit from annotationHelperLib import * (it should change the font and switch to a gray background again)
- In the menu bar at the top should be a dropdown menu that says “Markdown” - change it to “Python” or “Code”
- Hit the Run button
- Delete the two ## in front of the line beginning with `from`


# Getting Access to Respublica

# Running the Image QC Tool on Respublica

Before getting started, read and familiarize yourself with the manual image grading guidelines [here](https://bgdlab.github.io/research/image_qc.html).

1. When your eResearch account has Respublica access, go to [https://beyond.chop.edu](https://beyond.chop.edu) and connect to VM Horizon in the browser. You will need to log in using your CHOP credentials.
2. Open RES-RHEL-HPC. It will initially load as a black screen, wait until the background with a large dark 8 and a "Red Hat Enterprise" logo appear. This is the virtual machine you have just connected to. At the top left corner of the VM, there is a menu titled "Activities". Click "Activities" to pop out a dock on the left side of the window and click the terminal.
3. Enter the following command in the terminal: `git clone https://github.com/jmschabdach/manual_smri_qc.git`. If you have already run this command in the past, run the commands `cd manual_smri_qc` followed by `git pull` to get the most up to date version of the grading code. Note: you will use your computer's regular copy-paste keyboard shortcut to copy this command, but you will need to use Control Shift V to paste the command in the VM's terminal. Hit Enter to run the command.
4. You can now log out from the VM by going to the power menu in the top right corner of the window and selected Log Out.
5. This step varies depending on whether or not you own your computer. 
    - If your computer is not owned by CHOP: Log in to [https://connect.chop.edu](https://connect.chop.edu). Connect to the Virtual Desktop and open a browser window. 
    - If your computer is owned by CHOP: Connect to the VPN. 
6. In your browser, go to [https://respublica-web.research.chop.edu](https://respublica-web.research.chop.edu) and log in.
7. Navigate to the "My Interactive Sessions" tab using the menu at the top of the page. From the bar on the left side of this tab, select "Jupyter Notebook" under the Servers option.
8. To make sure you have the resources to rate a single batch, enter the following specifications into the corresponding fields:
    - Number of hours\*: 1
    - Number of CPUs: 1
    - Amount of memory (GB): 4
    - GPU: 0
9. Click the "Launch" button. The browser page will inform you that your request has been submitted and as you to wait for a few minutes for your allocation to be ready. When it is ready, click the "Connect to Jupyter" button.
10. A new tab will open in your browser showing your home directory on the HPC. Double click on the "manual_smri_qc" folder - this directory contains the git repository you previously cloned in step 3. 
11. Open the file "Image QC Tool.ipynb" with a notebook icon to its left by double clicking on the file name.
12. When you finish rating the images in a batch, save and close the notebook tab. Close the tab containing the directory contents where you had opened the notebook originally. Finally, in the respublica-web.research.chop.edu tab, delete the jupyter notebook even if you have time remaining - this is good ettiquitte, good practice, and it frees up the resources you were using for someone else. 

\*Note: you can adjust the time requested for the Jupyter notebook if you are going to grade multiple batches in one sitting. Each batch is designed to take 30 minutes to grade. Close the notebook on the server when you're done.


