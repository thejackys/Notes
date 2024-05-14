# How to install PyTorch on CSE student HP cluster
Yu-Wei Su - CSE 582 - Spring 2024

## Intro
Students who have never used a lab machine (like me) may take some time to figure out what can be done and what cannot. I'll share notes on how to setup the lab machine. There are multiple ways to do it.

The first thing to notice is that installation and permissions are restricted because root privilege is not allowed when using a lab machine. So, installing may be different from your own machine.


It can be seperated into 3 parts to setup your lab machine.:

[1. Access the Lab Machine](#Accessing-Lab-the-machine)
[2. Activate Conda](#Activate-Conda-(05/07)) 
[3. Installing pytorch using conda](#Installing-pytorch-using-conda)

## Accessing Lab the machine
[PSU CSE Student Lab Access Information](https://www.eecs.psu.edu/cse-student-lab-access/index.aspx) has already provided ways to access Lab machines. 
Alternatively, if you are using vscode like me, You can use [vscode to ssh into the lab machine](https://code.visualstudio.com/docs/remote/ssh).

Assuming you know how to get into any one lab machine. There are 2 storage spaces that we can use:
1. **home directory** (2.5G): Shared through all machines and located in places like /home/grads/{PSU_id} and can be accessed using `cd ~ `. It has only limited 2.5Gb spaces for me, which is insufficient to install pytorch.
2. **/scratch/{PSU_id}**: After adding a directory with your PSU id. You are free to use it, but the files may be deleted indefinitely. 
It is better to have a backup for your work, either to maintain a GitHub repo or something else. The following instructions are copied from the Readme file in /scratch:
>This space (/scratch) is temporary storage for scratch files.
If you plan to use any of this spece please create a directory with your userid.
        `mkdir /scratch/<my psuid>`
        `cd /scratch/<my psuid>`
>
>This space is temporary it may be deleted at ANY TIME.
Do not put anything here that is sensitive or important and not backed up elsewhere.
>
>
>Do not abuse this space or it will be removed.
>For question or problems please email csehelpdesk@engr.psu.edu

## Activate Conda (05/07) 
(Thanks to Valerie Li for pointing out conda is preinstalled.)

After accessing the lab machine we will use conda to setup python virtual enviroments.  [Conda](https://docs.conda.io/projects/miniconda/en/latest/index.html) allows you to virtually seperate different python version. Using the conda, you are essentially building a new python version rather than using lab machine's version. But I found this more easier to install. Here are some quick [introduction](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-python "conda introduction") from conda web page. 

You can either :
1. [**Activate preinstalled anaconda (Recommended)**](#Activate-preinstalled-anaconda)
2. [Install miniconda in home directory.](#Install-miniconda-in-home-directory)

### Activate preinstalled Anaconda
There is an preinstalled Anaconda that can be used. You can edit ~/.software by commenting out the Anaconda3.9 from:
```bash
# Anaconda3.9: Anaconda 3.9 Python 3.9
# Anaconda3.9
```
to:
```bash
# Anaconda3.9: Anaconda 3.9 Python 3.9
Anaconda3.9
```

or just append it to the ~/.software using:
```bash
echo "Anaconda3.9" >> ~/.software
```

Then you should be able to see (base) after relogging in the lab machine.  
![image](https://hackmd.io/_uploads/r1ySsbq9T.png)


### Install miniconda in home directory.
You can also the latest version of Miniconda in your home directory:
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```
Export the conda the the environment.
```bash
export PATH=~/miniconda3/bin:$PATH
```
The command `conda` should be working now.

Then init your shell for conda.
Based on your shell, it could be any of the following:
```bash
conda init
~/miniconda3/bin/conda init bash
```
Then, using
```bash
source ~/.bashrc
source ~/.zshrc
```
You should see a (base) showing on your shell screen, either on the left or right based on your shell. 
![image](https://hackmd.io/_uploads/r1ySsbq9T.png)


## Installing pytorch using conda


Since there is not enough space to install it directly in the home directory. We'll have to specify the conda to install the environment in /scratch:
```bash
PSUID=<your_psu_id> #set your PSU id as a variable
conda config --add envs_dirs /scratch/$PSUID/conda/envs
conda config --add pkgs_dirs /scratch/$PSUID/conda/pkgs
```

This will add the path to `~/.condarc` and specify conda to install the package and environments in /scratch.
![image](https://hackmd.io/_uploads/BkVoj-qqT.png)

Then initialize your conda and create an environment called nlp with a python version you want, and activate it in CLI:

```bash
conda create --name nlp python=3.7.0 -y
```
![image](https://hackmd.io/_uploads/r1dRsWccp.png)
    
Activate the nlp environment, and you'll see your environment is changed from (base) to (nlp). Now we'll just install the pytorch package then we are done.

```bash
conda activate nlp
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia 
```

After that, I think you will be ready to use the Python and GPU resources.
![image](https://hackmd.io/_uploads/HJwTnW5qa.png)

 One thing to notice is that we installed our pkgs and envs in the scratch space. So if the scratch place is deleted, we'll have to reinstalled the enviroment again. 
