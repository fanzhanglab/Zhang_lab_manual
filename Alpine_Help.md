# Getting Started on Alpine

## First steps
### Requesting an account
If you do not already have a Rocky Mountain Advanced Computing Consortium (RMACC) account, please follow the steps [here](https://curc.readthedocs.io/en/latest/getting_started/logging-in.html).

### Open OnDemand
Once you have an account, you can login to the RMACC OnDemand portal to access CURC resources! Follow the steps [here](https://curc.readthedocs.io/en/latest/open_ondemand/index.html) for more details.

## Accessing the terminal
Once logged into OnDemand by visiting https://ondemand-rmacc.rc.colorado.edu, as seen below:

<img src="https://github.com/fanzhanglab/Zhang_lab_manual/blob/main/ondemand.png" width="600" align="center">

Then, you can access login nodes by selecting >_Alpine Shell from the Clusters dropdown menu.
 
## Setting up your virtual environment
- Before you get started with anything else, you likely want to set up a conda virtual environment. [This documentation page](https://curc.readthedocs.io/en/latest/software/python.html) details how to set up Python and R with Anaconda on the cluster. Your conda environment can be used with interactive apps on OnDemand, or when submitting jobs with Slurm (see below).

## Interacive Applications
In addition to working in the terminal, you also have the option to launch graphical user interface (GUI) applications using CURC resources. The interactive apps dropdown menus includes applications such as Jupyter Notebook, RStudio, and VS Code. These can be launched using the preset configurations given, or via a custom configuration to better suit your needs. See the interactive applications [CURC guide](https://curc.readthedocs.io/en/latest/open_ondemand/index.html) for details.

<img src="https://github.com/fanzhanglab/Zhang_lab_manual/blob/main/jupyter.png" width="600" align="center">


## File system and storage organization for Zhang Lab:
- **`/projects/$USER`**
  - 250 GB/user
  - Use if you have a few small files to do I/O on.
  - Your local "playground"! Suggested structure:
    - `/projects/fanzhanglab\@xsede.org`
      - `results` (your daily work)
         - `2022-06-15_chronic_infectious_inflammatory_data_aggregation`
         - `2023-03-12_plot_each_protein_gene_umap`
         -  ...
       - `software`
           - `anaconda`
             - `envs`
               - `mycustomenv`
        - `data`
          - `processed`
          - `intermediate`
          - `archived`
        - `docs`
               
           
- **`/scratch/alpine/$USER`**
  - 10 TB/user (Alpine) 
  - Data purged after 90 days

- **`/pl/active/fanzhanglab`**: Fee-based compute-capable storage platform. Below is the current folder substructure:

  - `shared` (shared data folder. "Freezer": so please do not change it)
    - `AMP_atrisk_RA` (defined based on the project name; data for Inamo, et al, bioRxiv, 2023)
      - `CITEseq` (split by data type)
      - `cytof`
      - `Genotype`
      - `Genotype_v2`
    - `amp_phase2_ra` (CITE-seq data for Zhang, et al, Nature, 2023)
      - `fine_cluster_all_314011cells_82samples_2023-03-12.rds`
      - `qc_mRNA_314011cells_log_normalized_matrix_2023-03-15.rds`
      - `qc_protein_CLR_normalized_filtered_matrix_2023-03-12.rds`
      - `myeloid_reference_2023-03-12.rds` (each cell type reference includes everything)
      -  ...
    - `amp_phase2_sle`
    - `complement_RA` 
      - `bulk_rna_complement_log2tpm_counts-9-25-2023.rds`
      - `Donlin-IM-14854_2023_09_05`
        - `Donlin-IM-14854_2023_09_05.md5sum.source`
        - `IM047_1`
          - `HD16_S1_L004_R1_001.fastq.gz`
          - `HD16_S1_L004_R2_001.fastq.gz`
        - `IM047_10`
          - ...
          - ...
  - `jinamo` (personal working repo)


More details: [file system](https://curc.readthedocs.io/en/latest/compute/filesystems.html)


Useful commands:
- Disk usage may be checked using the `curc-quota` command. 
- The space occupied by a particular directory and its subdirectories can be obtained via the `du -h` command.
    
    
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

## Example of GNU parallel
GNU parallel is very efficient if you want to run many jobs with just changing genes, seed (for permutation test?)... Please refere [here](https://github.com/kf-cuanschutz/CU-Anschutz-HPC-documentation/blob/main/Office-hours-presentation-files/GNU_parallel_presentation.pdf) 

    ```
    #!/bin/bash
    #SBATCH --nodes=3 # We request directly the corresponding amount of nodes
    #SBATCH --ntasks-per-node=4
    #SBATCH --cpus-per-task=14
    #SBATCH --time=5:00:00
    ##SBATCH --qos=normal # if walltime < 24:00:00 then --qos=normal
    #SBATCH --partition=amilan
    #SBATCH --output=job.log
    #SBATCH --error=job.log
    ##SBATCH --mail-type=ALL
    ##SBATCH --mail-user=your.email@example.com
    
    module load gnu_parallel
    
    # Define celltype
    export celltype=T
    #export celltype=B
    #export celltype=M
    #export celltype=NK
    
    # Outputting list of nodes into a textfile
    scontrol show hostname > $SLURM_SUBMIT_DIR/nodelist.txt
    
    # Initiating GNU parallel and its variables
    export data_path=/path/to/data/
    my_srun="srun --export=all --exclusive -n1 --cpus-per-task=$SLURM_CPUS_PER_TASK --cpu-bind=cores"
    # --joblog allow to track the gnu parallel signal worked across all workers
    my_parallel="parallel --env PATH --env celltype --env data_path --env HOME --env SLURM_CPUS_PER_TASK --delay .2 -j $SLURM_NTASKS_PER_NODE --sshloginfile nodelist.txt --joblog job.log --wd $SLURM_SUBMIT_DIR --sshdelay 0.1"
    # $my_parallel '$my_srun Rscript /path/to/sceQTL.R {1} ${celltype} ${SLURM_CPUS_PER_TASK}' :::: $data_path/genes_${celltype}.txt
    $my_parallel '$my_srun Rscript /path/to/sceQTL.R {1} ${celltype} ${SLURM_CPUS_PER_TASK} > /path/to/log/sceQTL_T_{1}.out 2> /path/to/log/sceQTL_T_{1}.err' :::: /path/to/genes_${celltype}.txt


    ```


## More help
More information and tutorials can be found on the [CU Anschutz HPC Documentation Github](https://github.com/kf-cuanschutz/CU-Anschutz-HPC-documentation) as well as the [CU Research Computing documentation page](https://curc.readthedocs.io/en/latest/index.html). 
