# BABS - Making a container


One of the inputs to BABS is a containerized BIDS App. This document explains what a BIDS App is, how to create a Docker container for it on an Intel chip laptop, push the container to DockerHub, and run it on Respublica using Singularity. For basics on containers and images, see [Docker's guide](https://www.docker.com/resources/what-container/).

  

### What is a BIDS App?

A BIDS App is a containerized neuroimaging analysis tool that adheres to the Brain Imaging Data Structure (BIDS) standards. BIDS Apps simplify the process of running neuroimaging analyses by ensuring consistent data formats and containerized environments, which help in reproducibility and portability across different computing environments.

  

### Creating a Docker Container for the BIDS App

To create a Docker container for your BIDS App on an Intel chip laptop, follow these steps:

#### **➝** Step 1: Install Docker

1. **Download Docker Desktop:**
    *   Go to the [Docker website](https://docs.docker.com/guides/getting-started/) and download Docker Desktop for your operating system.
2. **Install Docker Desktop:**
    *   Run the installer and follow the installation instructions.
3. **Verify Installation:**
    *   Open a terminal and run the command:

```bash
docker --version
```

*       *   You should see the Docker version information if the installation was successful.
    *   

#### **➝** Step 2: Create a Folder for Docker Files

1. Create a folder to store all your Docker container-related files:

```powershell
$ mkdir my-bids-app
$ cd my-bids-app
```

  

This folder will have three necessary files:

*   `Dockerfile`: Defines the Docker image.
*   `run_bids_app.py` (or any other name) : A Python script to take inputs and instruct the container how to run.
*   `prep_bids_app.sh`: A bash script used during development to test if the container is working as needed.

  

#### **➝ Step 3: Create a Dockerfile**

1. In your project directory, create a file named `Dockerfile`.
2. Example Dockerfile:

```bash
# Base Image
FROM freesurfer/freesurfer:7.4.1


COPY run_synthseg.py /run_synthseg.py
RUN chmod +x /run_synthseg.py
ENTRYPOINT ["/usr/local/freesurfer/python/bin/python3", "/run_synthseg.py"]
```

This Dockerfile starts with the base image `freesurfer/freesurfer:7.4.1` . It copies a Python script named `run_synthseg.py` into the container, makes it executable, and configures it as the entry point to run using Python 3 from the FreeSurfer environment.

  

Modify as needed to include all dependencies and code for your BIDS App.

  

#### **➝ Step 4: Create** **`run_bids_app.py`**

1. Create a file named `run_bids_app.py` (or any other name) in the same directory.
2. Add your Python code that defines how to run the BIDS App.

Example `run_synthseg.py` :

This file runs SynthSeg on the input dataset provided, on the scans chosen by the user using the t1w, t2w, mprage, flair flags.

```python
import argparse
import os
import os.path as op
import subprocess

def cli():
    parser = argparse.ArgumentParser(
        description='SynthSeg BIDS App')
    parser.add_argument(
        'input_dir',
        help='The directory of the (unzipped) input dataset, '
        'or the path to the zipped dataset; '
        'unzipped dataset should be formatted according to the BIDS standard.')

    parser.add_argument(
        'output_dir',
        help='The directory where the output files should be stored')

    parser.add_argument(
        'analysis_level',
        choices=['participant'],
        help='Level of the analysis that will be performed.'
        " Currenlty only 'participant' is allowed.")

    parser.add_argument(
        '--participant_label', '--participant-label',
        help='The label of the participant that should be analyzed. The label'
        ' can either be <sub-xx>, or just <xx> which does not include "sub-".'
        " Currently only one participant label is allowed.",
        required=True)

    parser.add_argument(
        '--t1w',
        help='If present, T1w scans will be processed. If not present, T1w scans will not be processed.', action='store_true')

    parser.add_argument(
        '--t2w',
        help='If present, T2w scans will be processed. If not present, T2w scans will not be processed.', action='store_true')

    parser.add_argument(
        '--flair',
        help='If present, FLAIR scans will be processed. If not present, FLAIR scans will not be processed.', action='store_true')

    parser.add_argument(
        '--mprage',
        help='If present, MPRAGE scans will be processed. If not present, MPRAGE scans will not be processed.', action='store_true')

    return parser

def run_robust_synthseg(scan, inFn, out_robust, volscsv, qccsv, postfn):
    print()
    print()
    print("===== Running SynthSeg (Robust) on : ", scan, " =====")

    out_dir = op.join(out_robust, scan.split(".nii")[0])
    volscsv = op.join(out_dir, volscsv)
    qccsv = op.join(out_dir, qccsv)
    postfn = op.join(out_dir, postfn)
    result = subprocess.run(["mri_synthseg", "--i", inFn, "--o", out_dir, "--vol", volscsv, "--qc", qccsv, "--post", postfn, "--robust", "--parc"])

    return result

def run_notrobust_synthseg(scan, inFn, out_notrobust, volscsv, qccsv, postfn):

    print()
    print()
    print("===== Running SynthSeg (Not Robust) on : ", scan, " =====")

    out_dir = op.join(out_notrobust, scan.split(".nii")[0])
    volscsv = op.join(out_dir, volscsv)
    qccsv = op.join(out_dir, qccsv)
    postfn = op.join(out_dir, postfn)
    result = subprocess.run(["mri_synthseg", "--i", inFn, "--o", out_dir, "--vol", volscsv, "--qc", qccsv, "--post", postfn, "--parc"])

    return result

def get_fs_version():

    output = subprocess.check_output(['recon-all', '-version']).decode('utf-8')
    version = output.split("-")[-3]
    return version

def main():

    args = cli().parse_args()

    # Sanity checks and preparations: --------------------------------------------
    if args.participant_label:
        if "sub-" == args.participant_label[0:4]:
            participant_label = args.participant_label
        else:
            participant_label = "sub-" + args.participant_label

    print("participant: " + participant_label)

    # check and make output dir:
    if op.exists(args.output_dir) is False:
        os.makedirs(args.output_dir)

    dir_4analysis = os.path.join(args.input_dir, participant_label)
    #print(dir_4analysis)
    sessions = [ses for ses in os.listdir(dir_4analysis) if op.isdir(op.join(dir_4analysis,ses))] # Gets sessions
    #print(sessions)

    # Check scans to be processed :
    types_to_process = []
    if args.t1w is True:
        types_to_process.append("T1w")
    if args.t2w is True:
        types_to_process.append("T2w")
    if args.flair is True:
        types_to_process.append("FLAIR")
    if args.mprage is True:
        types_to_process.append("MPRAGE")

    if len(types_to_process)==0:
        print("Please specify the type of scans you would like to process (--t1w, --t2w, --flair or --mprage) and try again.")
        exit(1)

    fs_version = get_fs_version()
    # Synthseg robust directory
    robust_dir = "synthseg_fs"+get_fs_version()+"_parc_robust"
    out_robust = op.join(args.output_dir, robust_dir)
    if op.exists(out_robust) is False:
        os.makedirs(out_robust)

    # Synthseg not robust directory
    notrobust_dir = "synthseg_fs"+get_fs_version()+"_parc_notrobust"
    out_notrobust = op.join(args.output_dir, notrobust_dir)
    if op.exists(out_notrobust) is False:
        os.makedirs(out_notrobust)

    for ses in sessions :
        sesPath = op.join(dir_4analysis, ses)
        if "anat" in os.listdir(sesPath):
            anatPath = op.join(sesPath, "anat")
            # Get only the scans we wish to process
            scans = [fn for fn in os.listdir(anatPath) if (any(x.lower() in fn.lower() for x in types_to_process) and (".nii" in fn or ".nii.gz" in fn))]
        else :
            continue
        for scan in scans:
            scanPath = op.join(anatPath, scan)
            volscsv = "volumes.csv"
            qccsv = "qc_scores.csv"
            postfn = "posterior_probability_maps.nii.gz"

            # Robust synthseg
            result_robust = run_robust_synthseg(scan, scanPath, out_robust, volscsv, qccsv, postfn)
            if result_robust.returncode!=0 :
                print("SynthSeg Robust did not run for scan : ", scan)
            print()
            print()

            # Not robust synthseg
            result_notrobust = run_notrobust_synthseg(scan, scanPath, out_notrobust, volscsv, qccsv, postfn)
            if result_notrobust.returncode!=0 :
                print("SynthSeg Non-Robust did not run for scan : ", scan)
            print()
            print()


if __name__ == "__main__":
    main()
```

  

**Note** : The first four arguments (namely `input_dir`, `output_dir`, `analysis_level`, and `participant_label`) highlighted in yellow above are required for BABS. They should be copied as is. Other arguments that might be needed to run the BIDS App can be added (in this case, `t1w`, `t2w`, `mprage`, and `flair` were added). When the container is run on Respublica, the four required arguments will be provided by BABS. Any additional arguments and inputs needed for them will be mentioned in the BABS YAML file.

  

#### **➝ Step 5: Create** `prep_bids_app.sh`

  

1. Create a file named `prep_bids_app.sh` in the same directory.
2. This script tests your Docker container and can be run whenever changes are made to your BIDS App during development, eliminating the need to run individual commands each time.

Example `prep_synthsegBIDSApp.sh` :

```bash
#!/bin/bash

# first, cd to the folder containing this bash script (and Dockerfile + run_synthseg.py)

version_tag="0.0.1"
version_tag_dash="0-0-1"

# Build:
docker build -t yourdockerhubusername/synthseg_bids_app:${version_tag} -f Dockerfile .

# Test:
dir_to_mount="/path/to/input_dataset"
mounted_dir="/root/input_dataset"
mounted_input_dir="/root/input_dataset/BIDS"
mounted_output_dir="/root/input_dataset/derivatives"


participant_label="sub-1234"

# unzipped input ds:
docker run --rm -it -v ${dir_to_mount}:${mounted_dir} \
    yourdockerhubusername/synthseg_bids_app:${version_tag} \
    ${mounted_input_dir} ${mounted_output_dir} participant \
    --participant_label ${participant_label} \
    --t1w \
    --t2w
    # Output : Should have created a derivatives directory inside input_dataset and 2 sub-directories
    # within it (one each for synthseg-robust and synthseg-notrobust) with each having one output directory per scan
```

This script :

*   Builds a Docker image tagged with `yourdockerhubusername/synthseg_bids_app:${version_tag}` using the `Dockerfile` in the current directory.
*   It then tests the Docker image by running a container with an example input dataset (`dir_to_mount`) mounted inside the container (`mounted_dir`).
*   The BIDS App inside the container processes the input dataset (`${mounted_input_dir}`) and outputs results to `${mounted_output_dir}`.
*   Adjust all the variables as per your specific setup.

  

1. Make the script executable:

```powershell
$ chmod +x prep_bids_app.sh
```

Once everything works as expected, proceed to the next step.

  

#### **➝ Step 6: Build and push the Docker Image to DockerHub**

1. Cd to the directory that contains all the container related files

```powershell
$ my-bids-app
```

  

1. Build the image

```powershell
$ docker build -t my_bids_app:{version_tag} .
```

Replace my\_bids\_app with the name of your container and {version\_tag} with the tag you wish to give it. Example : synthseg\_bids\_app:0.0.1

  

1. Login to DockerHub

```powershell
$ docker login
```

This will prompt you to enter your username and password.

  

1. Tag the image

```powershell
$ docker tag my_bids_app:{version_tag} yourdockerusername/my_bids_app:{version_tag}
```

  

1. Push the image

```powershell
$ docker push yourdockerusername/my_bids_app:{version_tag}
```

This will push the image to your DockerHub repository.

  

#### **➝ Step 7: Using the image on Respublica**

```powershell
$ singularity build my_bids_app-{version_tag}.sif docker://yourdockerusername/my_bids_app:{version_tag}
```

## References
Last updated 2024-09-05 by dabrielz

**Contributors**

@shreyagudapati9

@dabrielz