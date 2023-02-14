SLIP Radiology Report Annotating Guidelines and Process

Overall Purpose

- Create separate datasets for the SLIP study by sorting notes into three categories
    - Not SLIP (0) - images with significant issues that have to be excluded
    - Maybe SLIP (1) - images that would likely need to be excluded from a typical research study but where quantitative analyses such as Freesurer could still be interesting, potentially require a closer look depending on the specific research context
    - SLIP (2) - what could arguably be included in a research study on brain morphometry

Directions

1. Request access to Arcus Lab 605 and access it from [arcus.chop.edu](arcus.chop.edu). 
2. When access has been granted, turn the lab "ON" and open the Terminal application.
3. Manually type the following command and hit enter: `rsync -avz /mnt/arcus/lab/shared/annotation-helper-tool .`.
4. Type `ls` and hit enter. You should see a list of directories, one of which will be `annotation-helper-tool`.
5. Now use the menu bar (icon with three horizontal lines located in the top left corner of the webpage) to switch from Terminal to Jupyter.
6. In the Jupyter interface, there is a column on the left side of the page indicating the current directory. Double click on the `annotation-helper-tool` to enter the directory. Double click the file `Arcus_Radiology_Report_Grading.ipynb` file to open it in the Jupyter file editor.
7. Follow the instructions in the notebook.


FYI
- Read the histories and findings in each note
- In general, be pretty liberal when rating notes
- These guidelines are not set in stone - some of the distinctions are blurry, so use the context and your best judgment
- Suggested WordsToHighlight = ['cyst', 'lesion', 'mass', 'Chiari', 'outside', 'unremarkable', 'chemo', 'shunt', 'post', 'surgical', 'resection', 'oma', 'tomy', 'orthodontic', 'motion', 'movement', 'degrad', 'artifact', 'virchow-robin', 'enlarg', 'prominen', 'mild', 'aneurysm', 'hemorrhage', 'asymmetr', 'ectopia', 'minor', 'incidental', 'anomaly', 'hypoplasia', 'abnormal', 'protrusion', 'signal', 'gliosis', 'telangiectasia', 'hyperintensity', ‘cavum’, ‘vari’, ‘cephaly’]

Key Words for Ratings (0, 1, or 2)

- 0: Not 
    - History of aneurysm
    - Hemorrhage or possible hemorrhage
    - Orthodontic hardware degrades the image (ex: results in significant signal dropout and geometric distortion)
    - Multiple issues in addition to a Chiari malformation
    - Outside scan
    - Chemo
    - Most -omas, -otomy, and -ectomy (ex: craniectomy, craniotomy, glioblastoma, glioma, astrocytoma, neurofibromatosis)
    - Shunt
    - Surgery (ex: postop, post surg, surgical cavity, surgical site, resection)
- 1: Maybe SLIP 
    - Mildly enlarged
    - Motion (ex: motion artifact, motion degradation)
    - Cerebellar ectopia
    - Choroid plexus cyst
    - Pineal cyst
    - Pars intermedia cyst
    - Small developmental anomaly 
    - Hypoplasia
    - Measurement associated with Chiari malformation (four to five mm of cerebellar tonsillar ectopia), minimal cerebellar tonsillar ectopia
    - Relevant family history (ex: sibling died from medulloblastoma)
    - Fewer than three incidental notes
    - Likely artifactual signal abnormality
    - Signal abnormality on a single slice 
    - Lesion 
    - Gliosis
    - Telangiectasia
    - Large cyst (ex: 13 mm x 7 mm)
    - T2 or FLAIR hyperintensity if
        - It is not the only issue
        - It is not small
        - The radiologist does not mention it is likely due to an anatomic variant 
        - It does not indicate a tumor, lesion, mass, or hemorrhage
    - Virchow-robin or perivascular space
    - Prominent CSF
    - (Slight or mild) asymmetry 
    - (Slight) enlargement (ex: of ventricles)
    - Within the range of a developmental variant
    - Mild
    - Septum cavum
    - Heterotopia
    - Polymicrogyria
    - Macro or microcephaly
    - Prominent pituitary gland with a convex superior border
    - Glioma
- 2: Okay for SLIP (these terms are irrelevant to SLIP or do not affect it significantly)
    - Mucosal thickening
    - Mucous retention cyst
    - Enlarged tonsils
    - Choroid-related cyst with no other issues
    - Hypertrophy of the tonsils (not the brain)
    - Trace mastoid air cell fluid bilaterally
    - Braces with no other issues
    - Small cyst
    - Sinus and nose issues (ex: opacification of the sinus cavities, sinus disease, ethmoid disease)
    - Early calcification
    - Minimal eye-related problems (ex: occipital plagiocephaly, hypoplasia of the optic nerves, asymmetry of the optic nerves)
    - Diffuse fatty marrow expansion
    - Mild hypoplasia
    - Mild brachycephaly
    - Cerebral palsy but the image is fine
    - T2 or FLAIR hyperintensity if it
        - Is the only issue
        - Is small
        - Is likely due to an anatomic variant
        - Does not indicate a tumor, lesion, mass, or hemorrhage
    - Chiari malformation if it is small enough with no measurement or borderline with an otherwise unremarkable brain
    - Unremarkable 
    - Cerebellar tonsils are above the level of the foramen magnum
    - Artery-related issues if not a hemorrhage or aneurysm
- 1 or 2 (really depends on the rest of the report and the context)
    - Artifactual 
    - Anatomical variant (could mark as 1 to be safe)
    - Prominence
    - The patient’s only problem is orthodontic hardware and the clinicians were able to see enough of the image for a comprehensive report

Understanding Terms

- Orthodontic hardware - throws off images due to the metal
- Artifactual - fuzzy but probably not physiological, likely appeared during the imaging process due to human or systemic error,  probably not anything wrong with the brain
- Cerebellar ectopia - if the brain extended more, it would be a Chiari malformation
- In a range of/within anatomic or developmental variant - still in the range of normal, no definitive problem, can sometimes appear in “healthy kids” 
- Hypoplasia - smaller than normal
- ? tumor - possible tumor
- One cm nodule of high signal material - likely represents a relatively large cyst
- Calcification - hardening
- Plagiocephaly - developmental flattening of the head
- Diffuse fatty marrow expansion as evidenced by a thick calvarium with a prominent diploic space - the base of the skull is a little thicker than it should be
- Incidental - unexpected extra and noteworthy finding
- Stable nonenhancing focus of T2 hyperintensity - the radiologist is not sure what it means yet, need to follow up on 
- Outside scan - unlikely to acquire these scans, since they did not happen at CHOP, so there is no way to confirm what is going on in them

# Using the Jupyter Notebook on Arcus

*Set Up*

Get your eResearch account set up first, then get access to the Arcus lab.

*Running the Notebook* - LOH

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

