# Getting Started on Alpine

## First steps
### Requesting an account
If you do not already have a Rocky Mountain Advanced Computing Consortium (RMACC) account, please follow the steps [here](https://curc.readthedocs.io/en/latest/access/rmacc.html).

### Open OnDemand
Once you have an account, you can login to the RMACC OnDemand portal to access CURC resources! Follow the steps [here](https://curc.readthedocs.io/en/latest/access/rmacc.html#logging-in-to-open-ondemand) for more details.

## Accessing the terminal
Once logged into OnDemand, you can access login nodes by selecting >_Alpine Shell from the CLusters dropdown menu.
![Screenshot of OnDemand CLusters drop down menu, courtesy of CURC user guide.](https://curc.readthedocs.io/en/latest/_images/shell_access.png)

## Setting up your virtual environment
- Before you get started with anything else, you likely want to set up a conda virtual environment. [This documentation page](https://curc.readthedocs.io/en/latest/software/python.html) details how to set up Python and R with Anaconda on the cluster. Your conda environment can be used with interactive apps on OnDemand, or when submitting jobs with Slurm (see below).

## Interacive Applications
In addition to working in the terminal, you also have the option to launch graphical user interface (GUI) applications using CURC resources. The interactive apps dropdown menus includes applications such as Jupyter Notebook, RStudio, and VS Code. These can be launched using the preset configurations given, or via a custom configuration to better suit your needs. See the interactive applications [CURC guide](https://curc.readthedocs.io/en/latest/gateways/OnDemand.html#jupyter-session) for details.

## Submitting jobs with Slurm
Another way to use/access resources is to submit a job through Slurm. Slurm is a work load manager and job scheduler that allows cluster users to run code on the remote server without needing to interact with or "babysit" the terminal. Slurm will allocate resources, time, and execute your code for you. This is typically done by providing a series of Linux commands and resource requests in a job script. 
- Start by accessing an Alpine login node and requesting a compute node with the `acompile` command.
- Then, load the slurmtools module: `module load slurmtools` so that the Slurm directives and keywords are recognized.
- Navigate to your working directory and create a bash file using your favorite text editor. For instance, `vim example_script.sh`.
- Inside of the bash file, write the following linux commands, save, and exit.
    ```
    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH --ntasks=1
    #SBATCH --time=00:10:00
    #SBATCH --partition=atesting
    #SBATCH --output=sample-%j.out

    module purge

    module load intel
    module load mkl

    echo "== This is the scripting step! =="
    sleep 30
    ./executable.exe
    echo "== End of Job =="
    ```
    Notice the comment lines at the beginning of the script. These are directives that tell the shell information about the script. We can change these parameters to modify the resource allocation, duration, etc. for our job. See [here](https://curc.readthedocs.io/en/latest/running-jobs/batch-jobs.html) for more details and explanations of the different directives.
- Next, to submit the job execute the command `sbatch example_script.sh`. If successful, you will see a message that says Job (ID) submitted. You can use the job ID provided to inquire about the status of your job, or other information.
- To check on the status of your job do `squeue --user=your_rc_username`.
- To cancel a job do `scancel your_job_ID`. See [here](https://curc.readthedocs.io/en/latest/running-jobs/slurm-commands.html#) for more useful commands.

### More examples
- Certain partitions and memory requests may be more appropriate for different jobs. We can adjust the Slurm flags to accomodate your specific task. For example, computation on and manipulation of single-cell data may be very memory or time intensive. We may want to use the `amem` partition and the `mem` quality of service (qos) to get access to more memory. Here is an example of the directives for this task:
    ```
    #!/bin/bash


    #SBATCH --partition=amem
    #SBATCH --job-name=high_mem
    #SBATCH --nodes=1
    #SBATCH --ntasks-per-node=24
    #SBATCH --time=8:00:00
    #SBATCH --qos=mem
    #SBATCH --mem=1000000M
    #SBATCH --output=high_mem.%j.out
    #SBATCH --error=permute.%j.err
    ```
    Look [here](https://curc.readthedocs.io/en/latest/running-jobs/job-resources.html#) for more details about different flags, partitions, and QoS.

## More help
More information and tutorials can be found on the [CU Anschutz HPC Documentation Github](https://github.com/kf-cuanschutz/CU-Anschutz-HPC-documentation) as well as the [CU Research Computing documentation page](https://curc.readthedocs.io/en/latest/index.html). 