# Quick reference guide

Useful hints and short information on issues that may vary between the different systems

## Installed software software

To see the installed software available through the modules system, issue the command

    module spider

To enquire about pre-requisites required for a specific package

    module spider <module_name/version>

To see the currently loaded modules

    module list

To load a module

    module add <module_name>

To unload a module

    module del <module_name>

## Resource allocation

### Number of cores

The number of cores for a job is specified in the batch script in the format

    #SBATCH -N <number_of_nodes>
    #SBATCH --tasks-per-node=<number_of_cores_per_node>

Aurora has 20 cores per node. On this system, 80 cores would be allocated through

    # 80 cores on Aurora
    #SBATCH -N 4 
    #SBACTH --tasks-per-node=20

### Memory per core

The amount of memory per core is specified in the format

    #SBATCH --mem-per-cpu=<amount_of_memory_per_core_in_MB>

Aurora has nodes with 64 GB of memory. The default allocation per core is therefore 3200 MB, allowing some memory for the operating system.  Please note that if you increase your memory request beyond 3200 MB per core, some cores on the system will be idle due to the lack of memory.  Your account gets charged for these cores as well.

## File systems

### Home directory

Currently all LUNARC systems have a home directory that is different for each system, i.e., the login directory for user xxxx is

    /home/xxxx

This directory can be referenced as **$HOME**.

### Global working directory

For job submission, we recommend using your home directory as the old /lunarc/nobackup/users directory is now decommisioned.

    
### Local working directory

When a job is running, it has access to a temporary directory on the local disk of each allocated node. The directory can be referenced as **$SNIC_TMP** (or **$TMPDIR**). It will be deleted when the job finishes.

If a job is terminated prematurely, for example, if it exceeds the requested walltime, the files on the local disk will be lost. Files that would still be useful can be listed in a special file **$SNIC_TMP/slurm_save_files**. Filenames are assumed to be relative to **$SNIC_TMP** and should be separated by spaces or listed on separate lines. These files will be copied to the submission directory regardless whether the job ends as planned or is deleted, unless there is a problem with the disk or node itself.

### Quotas

To limit the disk usage, quotas are set for each user and filesystem. The status can be seen at login. A quota report can also be obtained by issuing the command

    snicquota

The quota can be increased on request.

## Test queues

On Alarik, it is possible to request extra high priority to run short tests (maximum 1h) using at most 2 nodes using

    #SBATCH --qos=test

Floating reservations are used to free two nodes every second hour between 8.00 and 20.00 to reduce the queue time for test jobs, which means that a shorter walltime increases likelihood of an earlier start. Only two such test jobs are allowed to run at the same time.

On Erik there is one two-GPU node reserved for tests (maximum 1 h) in a partition of its own, which is specified with

    #SBATCH -p test

It is not allowed to submit long series of jobs to a test queue. 

