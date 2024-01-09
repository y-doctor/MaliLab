# MaliLab

### TSCC One Time Setup

Open a VS Code terminal. ssh into tscc (change **ydoctor** to your personal login name given by tscc)
```
ssh ydoctor@login.tscc.sdsc.edu
```
Note: sometimes it will appear as if nothing is happening. I reccomend clicking the `details` in the bottom left of VS code. It will likely show that it is waiting for an input for 2FA such as Duo Push. Type in 1 for Duo push. 

```
nano .bashrc
```
Scroll to the bottom of the `.bashrc` file and paste in these lines:
```
module load singularitypro/3.11
export PATH="/tscc/projects/ps-malilab/$USER/MaliLab/galyleo:${PATH}"
export PATH="/tscc/nfs/home/$USER/.local/bin:${PATH}"
alias compute='galyleo launch --account htl169 --partition hotel --qos hotel --cpus 24 --memory 128 --time-limit 08:00:00 --sif /tscc/projects/ps-malilab/$USER/containers/data_science_box.sif --env-modules singularitypro --bind /tscc/projects --notebook-dir /tscc/projects/ps-malilab/$USER'
```

Create your own directory in our data storage where all your personal files will be housed
```
mkdir /tscc/projects/ps-malilab/$USER && cd $_
```

Copy the MaliLab github repository into your personal space
```
git clone https://github.com/y-doctor/MaliLab.git
```

Activate galyleo
```
cd MaliLab/galyleo && ./galyleo configure --reverse-proxy tscc-user-content.sdsc.edu --partition hotel --scratch-dir '"/scratch/${USER}/job_${SLURM_JOB_ID}"
```
Run the .bashrc file
```
source /tscc/nfs/home/$USER/.bashrc
```
Make a folder to hold the software container:
```
mkdir /tscc/projects/ps-malilab/$USER/containers && cd $_
singularity build data_science_box.sif docker://jupyter/scipy-notebook:latest
```
