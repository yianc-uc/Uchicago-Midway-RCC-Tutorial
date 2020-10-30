# Uchicago Midway RCC Tutorial

This is a tutorial for using Uchicago Midway research computing center (RCC). 

*This tutorial was adapted from [RCC Website](https://rcc.uchicago.edu/docs/connecting/index.html). I will keep it short to provide you necessary information to interact with the computing cluster and run jobs on it. I highly recommend you to check out the website for detailed questions.*

## Connecting to RCC

### Connecting with SSH

SSH provides command-line access and interaction with the Midway2 computing cluster. For Linux and MacOS user, open the terminal and type in the command line

```
ssh <CNetID>@midway2.rcc.uchicago.edu
```

Please repalce `<CNetID>` with your own CNetID at Uchicago. Then enter the password associated to your ID when prompted for a password. A Duo two-factor autentication window will then pop up requesting you select from the available 2FA options to authenticate to midway2.

![](https://rcc.uchicago.edu/docs/_images/duo-2fa.png)

You can also enable X11 forwarding to run graphical user interface (GUI) from the server on your local machine. To do that, make sure to install [XQuartz](https://www.xquartz.org/) and include the `-Y` flag in the command

```
ssh -Y <CNetID>@midway2.rcc.uchicago.edu
```

### Connecting with ThinLinc

Alternatively ThinLinc is a remote desktop server that can be used to connect to RCC systems and obtain a remote GUI. RCC recommands that ThinLinc be used when running software that requires a GUI. To use ThinLinc to connect to Midway2, one can either use the web browser version or download and install the ThinLinc Desktop Client. The web browser version does not require any setup, but the performance will be less optimal than the Desktop Client version.

#### ThinLinc Web browser

To use ThinLinc in web browser, point to the [address](https://midway2.rcc.uchicago.edu). Log in with your CNetID and password, and complete the 2FA as in last subsection.

#### ThinLinc Deskop Client

Download and install the proper ThinLinc client from [ThinLinc download page](https://www.cendio.com/thinlinc/download).

Open the client and use the following information to log in

![](https://rcc.uchicago.edu/docs/_images/thinlinc-client.png)

The default setting for ThinLinc is for the client to open in a fullscreen window that fills “all monitors”. This may cause problems if you want to switch to other windows on your local machine. If you would prefer to work with ThinLinc from its own window, click Options from the initial login interface and then Screen to adjust your settings as desired. The following is an example of a setup that places the ThinLinc client in its own window:

<img src="https://rcc.uchicago.edu/docs/_images/thinlinc-options.png" width="600"/>

Then you can log in and complete the 2FA as before. Upon successfully logging in, you will be presented with an IceWM desktop. Select Applications tab in the top left corner to access the terminal, file browser, and other utilities.

<img src="https://rcc.uchicago.edu/docs/_images/thinlinc-desktop.png" width="600"/>

To copy/paste between Thinlinc webaccess client and your computer, first, you need to open the side toolbar by clicking the handle. After that you click the Clipboard icon. The text field that just open will be synced with the clipboard on the server, so you can copy and paste to and from this text field.

With ThinLinc it is possible to maintain an active session after you have closed your connection to Midway. To disconned from Midway but maintain an active session (e.g. when you log back into the ThinLinc client you will resume your remote session from where you left off), simply close the ThinLinc window. NOTE: You must have End existing session unchecked for this to occur.

To exit ThinLinc and terminate your session completely, simply exit or close the ThinLinc application.

## Running Jobs on Midway

### Checking Your Account Balance

Firstly, when you connect to the cluster, you are in one of its login nodes. Login nodes may be used for compiling and debugging code, installing software, editing and managing files, submitting jobs, or any other work that is not long-running or computationally intensive. Login nodes should not be used for computionally intensive work. If you find your process is terminated and restarted, be sure to use compute node following the instructions below.

In Midway RCC, all jobs running on the compute nodes consume Service Units (SUs). All consumptions of SUs will be recorded and the amount of the SUs used will be deducted from the account balance. If you have multiple accounts, make sure the account for our course is **caam37830**. 

You can check the account balance by type in the command line

```
rcchelp balance
```

To see the overall summary of your usage, type

```
rcchelp usage
```

### Submitting Jobs

The Midway computing cluster uses the **Slurm** resource manager to schedule jobs and control interactive access to compute nodes. I will introduce two major ways of how to submit job using Slurm scheduler.

#### Batch Jobs

You can use the `sbatch` command to request certain amount of computational resources and submit jobs. Users typically write an “sbatch script” that contains all the commands and parameters neccessary to run the program on the cluster. Here is an example of an sbatch script

```
#!/bin/bash
#SBATCH --job-name=example_python
#SBATCH --output=script.out
#SBATCH --error=script.err
#SBATCH --time=00:05:00
#SBATCH --partition=broadwl
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=2000

module load python
python script.py
```

Here is a detailed explanation of what each of the option does:

| Option  |  Description |
|:--|:--|
| `#SBATCH --job-name=example_python`  | Assigns label example_python to the job        |
| `#SBATCH --output=script.out`        | Writes console output to file `script.out`     |
| `#SBATCH --error=script.err`         | Writes an error messages to file `script.err`  |
| `#SBATCH --time=00:05:00`            | Reserves the computing resources for 5 minutes (or less if program completes before 5 min) |
| `#SBATCH --partition=broadwl`        | Requests compute nodes from the broadwl partition on the Midway cluster |
| `#SBATCH --nodes=1`                  | Requests 1 compute nodes |
| `#SBATCH --ntasks-per-node=1`        | Requests 1 core (CPU) per node, for a total of 1 * 1 = 1 core |
| `#SBATCH --mem-per-cpu=2000`         | Requests 2000 MB (2 GB) of memory (RAM) per core, for a total of 2 * 1 = 2 GB per node |

Then the last two lines of the script load the python module and start to execute `script.py`. In this case, please make sure the `script.py` file is in the proper directory or you may also want to include change of directory commands in the sbatch script.

Finally suppose the sbatch script is saved in the current directory into a file `example.sbatch`. Use the following command to submit it to the cluster

```
sbatch example.sbatch
```

#### Interactive Jobs

The `sinteractive` command is used for requesting an interactive session. The sinteractive will direct you to the compute node, set up X11 forwarding, and transfer your current shell environment, including all the modules have previously loaded. To use it, type in the command line

```
sinteractive --partition=broadwl --nodes=1 --ntasks-per-node=1 --time=00:05:00 --mem-per-cpu=2000
```

The options are basically the same as `sbatch`. For example using the command above, one can request a 5-minuate interactive session from the broadwl partition. It includes 1 node with 1 core in each node and 2000 MB of memory for each core. The differences are now you can directly see the program output from the shell.

### Managing Jobs

The Slurm job scheduler provides several command-line tools for checking on the status of your jobs and for managing them. Here are a few commands that you may find particularly useful:

* **squeue**: finds out the status of jobs submitted by you and other users.
* **sacct**: retrieves job history and statistics about past jobs.
* **scancel**: cancels jobs you have submitted.

For example you can check the status of your job by using 

```
squeue --user=<CNetID>
```

The `squeue` command will also display the id of the your submitting jobs. And you can use the id to cancel a specific job, for example, to cancel a job with id 8885128, do the following:

```
scancel 8885128
```

If you want to cancel all the jobs submitted by a specific user, type

```
scancel --user=<CNetID>
```

## Data Transfer

RCC provides a number of methods for accessing and transferring data in/out of the systems. Here I will only introduce the command-line approach. For the more methods for different systems, please refer to [page](https://rcc.uchicago.edu/docs/data-transfer/index.html#http-web-access).

Most UNIX-like operating systems (Mac OS X, Linux, etc) provide a scp command which can be accessed from the command line. To transfer files from your local computer to your home directory on Midway2, open a terminal window and issue the command:

```
scp <path to the local file> <CNetID>@midway2.rcc.uchicago.edu:<path you want to store the file>
scp -r <path to local dir> <CNetID>@midway2.rcc.uchicago.edu:<path you want to store the directory>
```

To download the files or directories to your local machine, use the following:

```
scp <CNetID>@midway2.rcc.uchicago.edu:<path you want to store the file> <path to the local file>
scp -r <CNetID>@midway2.rcc.uchicago.edu:<path you want to store the directory> <path to local dir>
```

Here are all the necessary information you need to interact with the Uchicago RCC. For specific questions, please check out the [RCC Website](https://rcc.uchicago.edu/docs/connecting/index.html) and contact with the teaching team.
