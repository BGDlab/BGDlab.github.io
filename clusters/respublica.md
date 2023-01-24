*Respublica*

A CHOP HPC that we also use as a fileshare. Uses `slurm` and has both VM and web interface options.

# Getting Access to Respublica

If you do not (or do not know if you) have an eResearch account:

1. Contact Nina Laney to get your eResearch account set up. Let her know that you need access to the lab fileshare. 
2. Do the required CITI training and make sure your CITI training account is affiliated with CHOP. Make sure you have done all of the trainings required by CHOP.
3. Set up 2 factor authentication according to the instructions Nina will send you (if she doesn't, ask for it).

If you already have an eResearch account: 

1. Email Nina to let her know you are requesting Respublica accounts and ask her to approve access for you when she gets the chance. CC Aaron or Jenna if needed.
2. Log in to [CIRRUS](https://www.research.chop.edu/applications/cirrus). 
3. Submit a Respublica Access Request for yourself.
4. Submit a Fileshare Access Request to be added to the BGD Lab Respublica Fileshare (/mnt/isilon/bgdlab_resnas03). 

# Getting Code onto Respublica

1. Make sure your Github repository is up to date with the code you are currently using.
2. When your eResearch account has Respublica access, go to [https://beyond.chop.edu](https://beyond.chop.edu) and connect to VM Horizon in the browser. You will need to log in using your CHOP credentials.
3. Open RES-RHEL-HPC. It will initially load as a black screen, wait until the background with a large dark 8 and a "Red Hat Enterprise" logo appear. This is the virtual machine you have just connected to. At the top left corner of the VM, there is a menu titled "Activities". Click "Activities" to pop out a dock on the left side of the window and click the terminal.
4. If this is the first time you're putting the code on Respublica, you will need to `git clone` yourrepo first. In the terminal, type `git clone` followed by your repository name. The full command will look something like `git clone https://github.com/(the username of the repo owner)/(repo name).git`. 
5. If you have already cloned the repo, navigate into the repo directory on Respublica (`cd (diretory name`) followed by `git pull` to get the most up to date version of your repo from Github.
6. You can now log out from the VM by going to the power menu in the top right corner of the window and selected Log Out.

Note: you will need to use Control Shift C to copy from and Control Shift V to paste into the VM's terminal. Hit Enter to run the command.

# Getting a .CSV File onto Respublica - SECTION UNDER DEVELOPMENT

*This bit has only been tested on one computer that was owned by CHOP and therefore did not have to connect to the virtual Desktop first.*
1. When your eResearch account has Respublica access: 
     - If you're on a CHOP-owned computer, go to [https://beyond.chop.edu](https://beyond.chop.edu) and connect to VM Horizon in the browser. You will need to log in using your CHOP credentials. Open RES-RHEL-HPC. 
     - If you're on a non-CHOP computer, connect to the VM Horizon desktop. If you don't have it on your computer already, go to [https://beyond.chop.edu](https://beyond.chop.edu) to download. Once you've downloaded, enter the server address: `https://beyond.chop.edu`. You should get a pop-up asking permission to access local files - accept this. 
2. You've now connected to the virtual machine. It will initially load as a black screen, wait until the background with a large dark 8 and a "Red Hat Enterprise" logo appear. At the top left corner of the VM, there is a menu titled "Activities". Click "Activities" to pop out a dock on the left side of the window and click the terminal.
3. In the terminal, type `ls` to see the contents of your home directory. The directory `tsclient` connects from the virtual machine to your computer. Type `ls tsclient/local/path/to/file` to confirm the file is there, then transfer with `then rsync -avz ~/tsclient/my/desired/path/to/file /desired/respublica/location`

# Using Respublica to Run Code in a Browser

1. This step varies depending on whether or not you own your computer. 
    - If your computer is not owned by CHOP: Log in to [https://connect.chop.edu](https://connect.chop.edu). Connect to the Virtual Desktop and open a browser window. 
    - If your computer is owned by CHOP: Connect to the VPN. 
2. In your browser, go to [https://respublica-web.research.chop.edu](https://respublica-web.research.chop.edu) and log in.
3. Navigate to the "My Interactive Sessions" tab using the menu at the top of the page. The bar on the left side of this tab contains a list of software servers/interfaces you can choose from. Select the software you wish to use.
4. Before the server will allocate computational power for you, you will need to tell it how much computational power and memory you want to use and for how long. These specifications will vary depending on what you are doing. For example, the manual image qc process uses the following parameters:
    - Number of hours\*: 1
    - Number of CPUs: 1
    - Amount of memory (GB): 4
    - GPU: 0
5. Click the "Launch" button. The browser page will inform you that your request has been submitted and as you to wait for a few minutes for your allocation to be ready. When it is ready, click the "Connect" button.
6. The software you chose will open in this tab or a new tab. The path the software defaults to is `/home/(your username)/`.

# Using Respublica to Run Code via `sbatch` Jobs

- All job code lives in `/mnt/isilon/bgdlab_resnas03/code` under a subdirectory named after the processing pipeline.
- Example: SynthSeg+
    - In the code directory, there’s a `synthseg` subdirectory. 
    - This subdir has a file called `synthsegJobSubmitter.sh` that iterates over a rawdata BIDS directory (command line argument) and submits a new job for each T1w.nii.gz file contained in the subject/session/anat directories.
    - This script calls `jobSynthSeg.sh`, which contains the sbatch arguments etc. and commands to actually run the SynthSeg+ job.
    - To run SynthSeg+ on a BIDs directory, `bash /mnt/isilon/bgdlab_resnas03/code/synthseg/synthsegJobSubmitter.sh /mnt/isilon/bgdlab_resnas03/Data/datasetname/rawdata`
