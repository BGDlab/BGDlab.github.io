*Respublica*

A CHOP HPC that we also use as a fileshare. Uses `slurm` and has both VM and web interface options.

## Getting Access to Respublica

Log in to [CIRRUS](https://www.research.chop.edu/applications/cirrus). First, request an account for the user (Respublica Access Request), then request the user be added to the fileshare (Fileshare Access Request).

## Running the Image QC Tool on Respublica

1. Send Jenna your CHOP username (aka the email you use to log into CHOP resources). Jenna will submit a request for you to get access to the fileshare.
2. When you have access, go to [https://beyond.chop.edu](https://beyond.chop.edu) and connect to VM Horizon in the browser. You will need to log in using your CHOP credentials.
3. Open RES-RHEL-HPC. It will initially load as a black screen, wait until the background with a large dark 8 and a "Red Hat Enterprise" logo appear. This is the virtual machine you have just connected to. At the top left corner of the VM, there is a menu titled "Activities". Click "Activities" to pop out a dock on the left side of the window and click the terminal.
4. Enter the following command in the terminal: `git clone https://github.com/jmschabdach/manual_smri_qc.git`. Note: you will use your computer's regular copy-paste keyboard shortcut to copy this command, but you will need to use Control Shift V to paste the command in the VM's terminal. Hit Enter to run the command.
5. You can now log out from the VM by going to the power menu in the top right corner of the window and selected Log Out.
6. In your browser, go to [https://respublica-web.research.chop.edu](https://respublica-web.research.chop.edu) and log in.
7. Navigate to the "My Interactive Sessions" tab using the menu at the top of the page. From the bar on the left side of this tab, select "Jupyter Notebook" under the Servers option.
8. Enter the following specifications into the corresponding fields:
    - Number of hours: 1
    - Number of CPUs: 1
    - Amount of memory (GB): 4
    - GPU: 0
9. Click the "Launch" button. The browser page will inform you that your request has been submitted and as you to wait for a few minutes for your allocation to be ready. When it is ready, click the "Connect to Jupyter" button.
10. A new tab will open in your browser showing your home directory on the HPC. Double click on the "manual_smri_qc" folder - this directory contains the git repository you previously cloned in step 4. 
11. Open the file "Image QC Tool.ipynb" with a notebook icon to its left by double clicking on the file name.
12. Follow the instructions (link) to grade all of the images in a single batch.
13. When you finish rating the images in a batch, save and close the notebook tab. Close the tab containing the directory contents where you had opened the notebook originally. Finally, in the respublica-web.research.chop.edu tab, delete the jupyter notebook even if you have time remaining. 

Note: you can adjust the time requested for the Jupyter notebook if you are going to grade multiple batches in one sitting. Each batch is designed to take 30 minutes to grade. Close the notebook on the server when you're done.
