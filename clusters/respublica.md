*Respublica*

A CHOP HPC that we also use as a fileshare. Uses `slurm` and has both VM and web interface options.

## Getting Access to Respublica

If you do not (or do not know if you) have an eResearch account:

1. Contact Nina Laney to get your eResearch account set up. Let her know that you need access to the lab fileshare. 
2. Do the required CITI training and make sure your CITI training account is affiliated with CHOP. Make sure you have done all of the trainings required by CHOP.
3. Set up 2 factor authentication according to the instructions Nina will send you (if she doesn't, ask for it).

If you already have an eResearch account: 

1. Email Nina to let her know you are requesting Respublica accounts and ask her to approve access for you when she gets the chance. CC Aaron or Jenna if needed.
2. Log in to [CIRRUS](https://www.research.chop.edu/applications/cirrus). 
3. Submit a Respublica Access Request for yourself.
4. Submit a Fileshare Access Request to be added to the BGD Lab Respublica Fileshare (/mnt/isilon/bgdlab_resnas03). 


## Running the Image QC Tool on Respublica

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
