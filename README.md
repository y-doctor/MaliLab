# MaliLab

### TSCC One Time Setup

Open a VS Code terminal. ssh into tscc (change **ydoctor** to your personal login name given by tscc)
```
ssh ydoctor@login.tscc.sdsc.edu
```
Open the .bashrc file

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
cd MaliLab
git clone https://github.com/sdsc/galyleo.git
```

Activate galyleo
```
cd galyleo && ./galyleo configure --reverse-proxy tscc-user-content.sdsc.edu --partition hotel --scratch-dir '"/scratch/${USER}/job_${SLURM_JOB_ID}"'
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

### Running a Jupyter Notebook
- Make sure you have the `ssh` and `jupyter notebook` and `python` extensions downloaded in vscode
- From your VS code Welcome page, click `connect to` and select `host`
  - The first time you do this, it will ask you to add a host. That host is `ssh ydoctor@login.tscc.sdsc.edu` (change **ydoctor** to your login name). It will ask you to add this to the list of hosts; say yes. Restart your VS code after doing this the first time and do it again, this time selecting the `login.tscc.sdsc.edu` as a host.
    - Note: sometimes VS code will aappear to stall and do nothing when trying to connect. I would click `details` in the bottom right -- it's usually just waiting for a Duo Push
  
Open a new terminal in VS code. (At the top, Terminal --> New Terminal) Then, in the terminal, type `compute`
- This will request 24 CPU Cores, 128 GB of RAM for 8 hours. You can modify the alias `compute` in the `.bashrc` to ask for more power, but for most tasks this should be enough.
- Copy and paste the url output into internet browser. Once the server is up and running, in VS code, open a jupyter notebook, click select kernel --> existing jupyter kernel --> paste the url there --> select python3 as the kernel. 

