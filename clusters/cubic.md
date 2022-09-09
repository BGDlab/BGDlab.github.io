*CUBIC*

A Penn HPC. Uses SGE for job launching. 

## Getting up and running on CUBIC

## 1. Get a project

Project request as a ticket. Requires form 19.01 CUBIC Project Application, confirmation email from PI that project can be created. The project can be created by submitting a ticket to the [CBICA helpdesk](https://cbica-infr-vweb/rt/SelfService/).

Note: if the user is trying to access CUBIC from a personal laptop, they will need to submit a ticket to get approval to use the BIG-IP vpn by IS. 

> Yeah so first I called the IT support desk and they filed a ticket to get my account approved to use BIG-IP by security. I got an email from someone named Kristy Peters in the end who said she set it up -- a user in the lab.

## 2. Setting up the project

Highly recommend installing your own conda for the project. Makes managing packages so much easier. Double check with your friendly lab tech support to see if this step has already been taken care of. Instructions can be found over in PennLINC's [wiki](https://pennlinc.github.io/docs/cubic#installing-miniconda-in-your-project-the-hard-way).

## 3. Add users to the project

Before adding a user to a CUBIC project, the user must have a UPHS or Pennmedicine account as well as VPN access. The data manager or PI of a project can request a new user account via a [ticket](https://cbica-portal.uphs.upenn.edu/rt/SelfService/). The creation of a user account should be straightforward and quick. The only information that is needed is the UPHS username and full name of the user. (Note: make sure the spelling is correct and consistent, this has caused problems in the past.)

Once the user has a CUBIC account, they can be added to the project. This is again done via the ticket portal. The information required to complete the request includes: 
- the user's username, 
- the full path to the project, 
- the level of access the user should have (read only or read and write), and 
- written permission from the PI of the project needs to be attached to the ticket when it is submitted (a pdf of an email confirmation is sufficient).


## 4. Changing CUBIC/PennMedicine VPN Password

If you want to reset your password before it expires:
- Completely log out of CUBIC and the VPN.
- Call the help desk (215-662-7474) to get the password manually expired.
- Restart the VPN. You will be prompted to enter a new password.

You can also wait until the password expires at which point you will be prompted to enter a new password when you next start the VPN.

The new password must contain upper and lowercase characters, a number, and a special character, AND must not overlap 4 or more characters of the previous 5 passwords.

## 5. CUBIC Orientation

CUBIC has a slightly odd set up. Each person is given an individual login as a user, but the bulk of the work is done as a project “pseudo-user”. The project pseudo-user is both a user and a group (if you’re familiar with permissions). Users can be assigned access to multiple project pseudo-users, and you will need to connect to each project individually after connecting to CUBIC. For clarity, the individual accounts assigned to individual people will be referred to as `<username>` and the project pseudo-users will be referred to as `<projectname>`.

### **Connecting to CUBIC**

- Prerequisites: UPHS id/password and CUBIC account created
- Need to connect to CUBIC first as your individual user. Start by running the BIG-IP Edge Client (icon is a red circle with f5 in it). 
- Connect to CUBIC: `ssh -X -Y <username>@cubic-login.uphs.upenn.edu`


### **Spaces on CUBIC**

- User Home: The current working directory when you log in to CUBIC will be: `/cbica/home/<username>`. The individual accounts have very little storage, so most of your work will be done as a project pseudo-user. 

- Project Home: You will want to change your user from your individual account to your project: `sudo -u <projectname> bash`. This command changes your active user from your individual account to the project pseudo-user account, but you will still be in the same `/cbica/home/<username>` directory. Using the command `cd ~` will change your directory to `/cbica/projects/<projectname>`. This is where you'll store data and do stuff.

- Scratch/Tmp Dir: `$SBIA_TMPDIR` is usually `/scratch/<projectname>`. This directory is where temporary files generated during a job may be stored. Unclear if there is any trash management, we know people who had to write their own scratch trashman because otherwise their jobs consumed all of the power of the cluster.

## 6. Getting Data on CUBIC

You can transfer files from your local computer to [[CUBIC]] using the `rsync` command.

```rsync -avz <local directory> <username>@cubic-loging.uphs.upenn.edu:/cbica/projects/<projectname>/dropbox/```

You will want the destination directory to be `/cbica/projects/<projectname>/dropbox`. Since you have to transfer the data as your individual user, your individual user is the owner of the data when it arrives on CUBIC. You can verify that you are the owner of the data using `ls -lt` on `dropbox`. You will see something like

```drwxrwx---. 3 schabdaj bgdimagecentral 4096 Feb 1 15:22 22q\_0002/```

The first column of data in this line `drwxrwx---` contains 10 pieces of data. The first element, `d`, tells us that this file is a directory. The next three elements, `rwx`, show that the user who owns the file has permission to read (r), write (w), and execute (x) the file. The next three elements show the read/write/execute permissions for the group who owns the data. The final three elements `---` show that anyone else in the system does not have read, write, or execute permissions.

The third column displays the username of the user who owns the file (`schabdaj`). If the name here is not the project username (`bgdimagecentral`), don't move the data yet. The fourth column displays the name of the group that owns the files. This should be the project username. (Note: a user can belong to multiple groups and can see files for the distinct groups as their individual username, but the `projectname` users cannot access files from other projects.)

Unfortunately, the project user cannot manipulate files owned by another user, even if that user belongs to the project group. Leave your data in the dropbox directory for now. CUBIC has a process that runs periodically that changes the ownership of files in the dropbox directory of each project to the project username.


## 7. Running jobs on CUBIC

CUBIC uses the Sungrid Engine (SGE)’s job manager. Here’s the general syntax for a few things.

- `qsub [command]`: Submit a job to run `[command]`
- `qstat`: See the queue of jobs
- `qdel [job id number]`: Remove a single job with the specified ID number from the queue. The ID number can be found using `qstat`.

There are two types of jobs: interactive jobs and jobs that can run on a script. It is recommended that if you are doing anything computationally expensive, you should request an interactive job (syntax needed) rather than start working on the login node. All other jobs (most of the jobs we want to use the HPC cluster for) should be submitted to the job queue using qsub.

Here’s an example of a script that can be submitted to qsub. We’ll call it `test_job.sh`. It is designed to run a version of FreeSurfer’s reconall where the version number is specified as an input argument when the job is submitted.

```
#!/bin/sh
## The name of the job										
#$ -N test-job-woo										
## Join stdout and stderr:									
#$ -j y												
#
# Set the amount of memory being requested.						
#$ -l h_vmem=8G	

# Source the project-user’s miniconda setup						
source /cbica/projects/bgdimagecentral/.software/miniconda3/etc/profile.d/conda.sh

# Input arguments										
FSVERSION=$1											
OUTDIR=$2
SUBJ=$3												
INTERIM=$4

# Load and unload modules needed to run your code					
if [[ $FSLVERSION == "6.0.0" ]] ; then							
    module unload freesurfer/5.3.0							
    module load freesurfer/6.0.0								
elif [[ $FSLVERSION == "5.3.0" ]] ; then						
    module unload freesurfer/6.0.0							
    module load freesurfer/5.3.0								
elif [[ $FSLVERSION == "7.1.1" ]] ; then						
    module load freesurfer/7.1.0	
else
    echo "Something wrong with FSVERSION $FSVERSION"				
fi

# Run recon-all											 
recon-all -sd $OUTDIR -subject $SUBJ -i $INTERIM -all -target /cbica/projects/bgdimagecentral/.fsaverage	
```

The lines at the top of the script marked by `#$` specify arguments that could also be passed to qsub on the command line. If you’re running a large number of jobs with the same arguments, it is often easier to set them this way. This is not a comprehensive list of all possible arguments.

The next comment precedes the first command that will be executed when this job is run. At the start of the job, you want to set up your environment. This means sourcing any configuration files that are important to what you’re doing (i.e., sourcing this conda setup here is required for using libraries installed via conda for the `projectuser`).

The second section parses arguments that were passed to the bash script (because this is a bash script that is being executed as a job when it is assigned resources by the queue).

The third section is a bit of logic performed for this job to make sure the correct modules are loaded - aka, a continuation of setting up the environment. The admins of CUBIC maintain a vast number of software tools for different biomedical informatics and bioinformatics computational needs. There is a good chance something you want to run can be loaded with a `module load [software name]/[version number]` command. The module tool is helpful especially because it automatically handles version conflicts between different programs. For example, the example script has a line to unload a module followed by a line to load a different version of the module, but running the command to load the different version of the module would produce the same results: modules are unloaded when the user manually loads a new module.

The final section is the code that needs to be run. This section can vary in length, it happens to be a single line for this use case (running FreeSurfer’s reconall on a single subject).

To submit a single job using this script to the queue, run: `qsub test_job.sh “6.0.0” /full/path/to/output/directory sub-0001 /full/path/to/interim/file`

Side note: in the directory where you run the qsub command above, there will be placed an error log and an output log for the job when it runs. In the above example, the line `#$ -j y` joins the standard output and error to the same stream and results in only an output log. To avoid cluttering up our main directory, we have implemented a recurring job that removes certain specific files from the home directory once an hour. If you wish to keep your logs, cd into the ~/logs/ directory and run your jobs from there.

## 8. Software set up in `bgdimagecentral` on CUBIC

### `@bb_thickness`

The ball and box method from here https://afni.nimh.nih.gov/pub/dist/HBM2018/OHBM_2018_Thickness.pdf

**What it does**: This method defines depth as the size of the largest sphere or cube that will fit around a voxel, while staying entirely within the mask. Using cubes extends depth measures into corners more readily. An additional dataset is produced by mapping spheres of the depth of each voxel from smallest to largest, each at least partially encompassing its predecessor. The thickness is then defined volumetrically as the largest depth sphere, often from a neighboring voxel, that contains each given voxel.

**Running on CUBIC**
```
> module load afni/2022_05_03                                                       .
> @thickness_master                                                                 .
    -maskset sas_north.nii.gz                                                       .
    -outdir thick_base
```

Produces 3 output folders: thick_bb, thick_erosion, and thick_in2out

### conda

Python package and virtual environment manager

What it does: alternative method to pip for easily installing packages

Install location:

**Running on CUBIC**: Include this line in any job that needs to use conda or a library installed via conda: `source /cbica/projects/bgdimagecentral/.software/miniconda3/etc/profile.d/conda.sh .`

Currently installed packages: pandas, numpy, ...

### Infant FreeSurfer 6 

Two versions based on different FreeSurfer versions have been set up for the bgdimagecentral project.

What it does: Used for running FreeSurfer 6.0.0-based infant_recon_all on pediatric patients in the age range [0-3] (following Lifespan guidelines) using infant brain atlases.

Install location: `/cbica/projects/bgdimagecentral/.software/freesurfer-infant-6`

Running on CUBIC (in a job submitted to qsub):
```
# Remove the preloaded version of freesurfer                                         
module unload freesurfer/5.3.0 # current default                                     
                                                                                     
# Set up the environment variables needed                                            
export FREESURFER_HOME=/cbica/projects/bgdimagecentral/.software/freesurfer-infant-6/
export PATH="$PATH:$FREESURFER_HOME"                                                 
INFANT_FREESURFER=/cbica/projects/bgdimagecentral/.software/freesurfer-infant-6      
                                                                                     
# Run and source the scripts to set up the Infant FreeSurfer 6 environment           
bash $INFANT_FREESURFER/SetUpFreeSurfer.sh                                           
source $INFANT_FREESURFER/FreeSurferEnv.sh
```

### Infant FreeSurfer 7

Two versions based on different FreeSurfer versions have been set up for the bgdimagecentral project. 

What it does: Used for running FreeSurfer 7.1.1-based infant_recon_all on pediatric patients in the age range [0-3] (following Lifespan guidelines) using infant brain atlases.

Install location: `/cbica/projects/bgdimagecentral/.software/freesurfer-infant-7` 

With a dependency on `/cbica/projects/bgdimagecentral/.software/freesurfer-7.1.1`

Running on CUBIC (in a job submitted to qsub):
```
# Remove the preloaded version of freesurfer                                         
module unload freesurfer/5.3.0 # current default                                     
                                                                                     
# Set up the environment variables needed                                            
export FREESURFER_HOME=/cbica/projects/bgdimagecentral/.software/freesurfer-7.1.1/   
export PATH="$PATH:$FREESURFER_HOME"                                                 
INFANT_FREESURFER=/cbica/projects/bgdimagecentral/.software/freesurfer-infant-7      
                                                                                     
# Run and source the scripts to set up the Infant FreeSurfer 7 environment           
bash $INFANT_FREESURFER/SetUpFreeSurfer.sh                                           
source $INFANT_FREESURFER/FreeSurferEnv.sh
```

### SynthSeg 2.0

What it does: a machine learning based method for segmenting brain tissue in images that FreeSurfer might struggle with

Install Location: `/cbica/projects/bgdimagecentral/.software/SynthSeg`

Running on CUBIC (in a job submitted to qsub):
```
# Activate the conda environment                                                     
conda activate synthseg                                                              
                                                                                     
# Run the predict command                                                            
python $SYNTHSEG_HOME/scripts/commands/SynthSeg_predict.py –-help
```
