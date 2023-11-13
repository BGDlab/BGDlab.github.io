# Instructions on how to use nda-tools to download NDAR datasets onto Respublica

### Source: [https://github.com/NDAR/nda-tools](https://github.com/NDAR/nda-tools)

### Install:

1. Set up the `conda` environment

- `conda create -n nda-tools`
- `conda activate nda-tools`
- If pip is not installed in the conda env, `conda install -c conda-forge pip`
- `pip install numpy==1.22.4` -- there's a dependency on numpy >= 1.16 and numpy < 1.23
- If you get an error about `AttributeError: module 'pkgutil' has no attribute 'ImpImporter'. Did you mean: 'zipimporter'?`, check your python version `python --version`. 
    - So far, have confirmed this error occurs with Python `3.12` but not Python `3.9.14`. 
    - Change your Python version using `conda install python=3.9` then rerun the previous command to install numpy
- `pip install nda-tools`

2. Check that the `keyring` works

- `python`
- `import keyring`: if you get an import error here, exit python and run `pip install keyring` to install keyring from https://pypi.org/project/keyring/.
- `keyring.set_password("thisisasillytest", 'iam', 'groot')`  -- This is where errors happen on Respublica. Those errors likely have to do with the keyring automatically setting a password for the keyring:
    - In the terminal, run the command `rm ~/.local/share/keyring/*`
    - Then run `python`, `import keyring`, and `keyring.set_password("thisisasillytest", "iam", "groot")`
    - The system (at least on Respublica) will prompt you to create a new password for the keyring
- `keyring.get_password("thisisasillytest", 'iam')` will return `'groot'`
- `exit()`

3. Set up the `keyring`

- Make sure you know your NDA username and password (https://nda.nih.gov/nda/creating-an-nda-account.html). You may need to reset your password from your NDA account page.
- Checking the source name:
    - Exit python
    - Run a `downloadcmd` command without the correct password and see which file in the NDATools site package is the source of the error:  `File "/Users/youngjm/opt/miniconda3/envs/ndatools/lib/python3.9/site-packages/NDATools/clientscripts/downloadcmd.py", line 177, in configure`
    - I traced the relevant file back to `./miniconda3/envs/ndatools/lib/python3.9/site-packages/NDATools/Configuration.py`. SERVICE_NAME is defined on line 58 as `'nda-tools'`
- `python`
- `import keyring`
- `keyring.set_password("nda-tools", "NDAR_USERNAME", "pass")`
- `keyring.get_password("nda-tools", "NDAR_USERNAME")`

4. Prep a data package on [https://nda.nih.gov](https://nda.nih.gov)

- (Full instructions not included)
- Data Packages you create will be stored in a table under Dashboard > Data Package for a number of weeks.

5. Download the data package

- For the data package you wish to download, copy the ID number in the first column. Make sure there is enough space in the destination to store the downloaded files.
- On Respublica, run the following script with your arguments via bash (NOT slurm): `bash /mnt/isilon/bgdlab_resnas03/Data/LBCC/template_code/ndaDownloadSubmit.sh 12345678 ndarUsername /path/to/destination/` where
    - `12345678` is the desired Data Package's ID
    - `ndarUsername` is your NDA username
    - `/path/to/destination/` is the full path to the location where you want to store the data download

### Contents of `ndaDownloadSubmit.sh`:

```
#!/bin/bash

#SBATCH -N 1
#SBATCH -n 16
#SBATCH --mem=100G
#SBATCH --time=7-00
#SBATCH --job-name=ndar-download

PACKID=$1 # package ID found on NDA download page
USRNAME=$2 # NDAR username found on NDA user profile
TARGETDIR=$3 # destination of downloaded data

# Set up ndatools
source ${HOME}/miniconda3/etc/profile.d/conda.sh
conda activate nda-tools

downloadcmd -dp $PACKID -u $USRNAME -d $TARGETDIR -wt 32
```


### References:


Last updated 2023-11-13 jmschabdach
