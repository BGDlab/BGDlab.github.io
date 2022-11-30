CLIP Radiology Report Annotating Guidelines and Process

Overall Purpose

* Create separate datasets for Jenna’s study by sorting notes into three categories
    * Not CLIP (0) - images with significant issues that have to be excluded
    * Maybe CLIP (1) - images that would likely need to be excluded from a typical research study but where quantitative analyses such as Freesurer could still be interesting, potentially require a closer look depending on the specific research context
    * CLIP (2) - what could arguably be included in a research study on brain morphometry

Directions

1. Open Terminal app
2. Run code:
        cd ~/Desktop/CLIP
        jupyter notebook
3. Run the two gray boxes on Jupyter

FYI
* Read the histories and findings in each note
* In general, be pretty liberal when rating notes
* These guidelines are not set in stone - some of the distinctions are blurry, so use the context and your best judgment
* Suggested WordsToHighlight = ['cyst', 'lesion', 'mass', 'Chiari', 'outside', 'unremarkable', 'chemo', 'shunt', 'post', 'surgical', 'resection', 'oma', 'tomy', 'orthodontic', 'motion', 'movement', 'degrad', 'artifact', 'virchow-robin', 'enlarg', 'prominen', 'mild', 'aneurysm', 'hemorrhage', 'asymmetr', 'ectopia', 'minor', 'incidental', 'anomaly', 'hypoplasia', 'abnormal', 'protrusion', 'signal', 'gliosis', 'telangiectasia', 'hyperintensity', ‘cavum’, ‘vari’, ‘cephaly’]

Key Words for Ratings (0, 1, or 2)

* 0: Not CLIP
    * History of aneurysm
    * Hemorrhage or possible hemorrhage
    * Orthodontic hardware degrades the image (ex: results in significant signal dropout and geometric distortion)
    * Multiple issues in addition to a Chiari malformation
    * Outside scan
    * Chemo
    * Most -omas, -otomy, and -ectomy (ex: craniectomy, craniotomy, glioblastoma, glioma, astrocytoma, neurofibromatosis)
    * Shunt
    * Surgery (ex: postop, post surg, surgical cavity, surgical site, resection)
* 1: Maybe CLIP 
    * Mildly enlarged
    * Motion (ex: motion artifact, motion degradation)
    * Cerebellar ectopia
    * Choroid plexus cyst
    * Pineal cyst
    * Pars intermedia cyst
    * Small developmental anomaly 
    * Hypoplasia
    * Measurement associated with Chiari malformation (four to five mm of cerebellar tonsillar ectopia), minimal cerebellar tonsillar ectopia
    * Relevant family history (ex: sibling died from medulloblastoma)
    * Fewer than three incidental notes
    * Likely artifactual signal abnormality
    * Signal abnormality on a single slice 
    * Lesion 
    * Gliosis
    * Telangiectasia
    * Large cyst (ex: 13 mm x 7 mm)
    * T2 or FLAIR hyperintensity if
        * It is not the only issue
        * It is not small
        * The radiologist does not mention it is likely due to an anatomic variant 
        * It does not indicate a tumor, lesion, mass, or hemorrhage
    * Virchow-robin or perivascular space
    * Prominent CSF
    * (Slight or mild) asymmetry 
    * (Slight) enlargement (ex: of ventricles)
    * Within the range of a developmental variant
    * Mild
    * Septum cavum
    * Heterotopia
    * Polymicrogyria
    * Macro or microcephaly
    * Prominent pituitary gland with a convex superior border
    * Glioma
* 2: Okay for CLIP (these terms are irrelevant to CLIP or do not affect it significantly)
    * Mucosal thickening
    * Mucous retention cyst
    * Enlarged tonsils
    * Choroid-related cyst with no other issues
    * Hypertrophy of the tonsils (not the brain)
    * Trace mastoid air cell fluid bilaterally
    * Braces with no other issues
    * Small cyst
    * Sinus and nose issues (ex: opacification of the sinus cavities, sinus disease, ethmoid disease)
    * Early calcification
    * Minimal eye-related problems (ex: occipital plagiocephaly, hypoplasia of the optic nerves, asymmetry of the optic nerves)
    * Diffuse fatty marrow expansion
    * Mild hypoplasia
    * Mild brachycephaly
    * Cerebral palsy but the image is fine
    * T2 or FLAIR hyperintensity if it
        * Is the only issue
        * Is small
        * Is likely due to an anatomic variant
        * Does not indicate a tumor, lesion, mass, or hemorrhage
    * Chiari malformation if it is small enough with no measurement or borderline with an otherwise unremarkable brain
    * Unremarkable 
    * Cerebellar tonsils are above the level of the foramen magnum
    * Artery-related issues if not a hemorrhage or aneurysm
* 1 or 2 (really depends on the rest of the report and the context)
    * Artifactual 
    * Anatomical variant (could mark as 1 to be safe)
    * Prominence
    * The patient’s only problem is orthodontic hardware and the clinicians were able to see enough of the image for a comprehensive report

Understanding Terms

* Orthodontic hardware - throws off images due to the metal
* Artifactual - fuzzy but probably not physiological, likely appeared during the imaging process due to human or systemic error,  probably not anything wrong with the brain
* Cerebellar ectopia - if the brain extended more, it would be a Chiari malformation
* In a range of/within anatomic or developmental variant - still in the range of normal, no definitive problem, can sometimes appear in “healthy kids” 
* Hypoplasia - smaller than normal
* ? tumor - possible tumor
* One cm nodule of high signal material - likely represents a relatively large cyst
* Calcification - hardening
* Plagiocephaly - developmental flattening of the head
* Diffuse fatty marrow expansion as evidenced by a thick calvarium with a prominent diploic space - the base of the skull is a little thicker than it should be
* Incidental - unexpected extra and noteworthy finding
* Stable nonenhancing focus of T2 hyperintensity - the radiologist is not sure what it means yet, need to follow up on 
* Outside scan - unlikely to acquire these scans, since they did not happen at CHOP, so there is no way to confirm what is going on in them

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
- Run these cells
- The first cell does XYZ
- The second cell prints the next report. Use the Guidelines above to determine which category this report falls into. Type your rating in the text box and hit enter
- The notebook saves the ratings every X times
- When you're finished grading reports, click the "Stop" button to stop the kernel running the notebook



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



