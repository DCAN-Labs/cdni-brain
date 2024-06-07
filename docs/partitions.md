# Partitions

Read:  [Partitions @ MSI](https://www.msi.umn.edu/partitions)

MSI has two different clusters to use for submitting jobs, agate and mangi, each of which has several options for paritions. These partitions each manage different sets of hardware and have different limits for computing resources. Check [this site](https://status.msi.umn.edu/) for the status of the clusters. 

When [submitting jobs](slurm.md), it is important to consider which paritions are best for your job. Read [this page](optimizing.md) to learn how to determine the optimal resources for a job. You also need to make sure that the partitions you are requesting are a part of the cluster you are submitting from or a federated partition. 

Each partition you may choose is specific to the HPC resource that is being used. “Federated” partitions such as **msismall** can be used from any login node, and automatically delegate jobs to a partition accessible via the current login node.

![Federated Partitions](img/federated_partitions.png)

## Nodes

Login nodes (e.g. `ah0123` or `ln4567`) have access to less resources than a compute node. These are good for file browsing, text editing, small file transfers, and other minimal tasks. Any command taking longer than 15 minutes to run will be killed. To login to a compute node (e.g. `cn0123` or `acn4567`), run `ssh -Y agate/mangi`.

For submitting sbatch runs, you can submit these from a login or a compute node. For grabbing a srun, you need to be on a compute node. Read more about sbatch/srun jobs [on this page.](slurm-params.md) 

If you try to submit a job on a partition that doesn't have access to the amount of resources you requested or that can't be accessed from your current node, the job submission will fail and throw an error. 

## Partition Resources

![Partions of MSI](img/partitions-example.jpeg)

**"Partition name"** (`-p=name`) specifies the string for the partition. Note that some partitions are only accessible if connected to a corresponding node, e.g. **ahXXXX** for Agate. You can also list multiple partitions. 

**“Node sharing?”** specifies whether a partition allows for multiple jobs to be allocated on the same node across resources. The `--exclusive` flag will prevent the node from being shared with other jobs/users. 

**“Cores per node”** (`--ntasks=N`) specifies the range of core processors that may be allocated for one job per node. 

**“Walltime limit”** (`--time=hrs:min:sec`) specifies the maximum amount of time that is allocated for a job to use resources. 

**“Total node memory”** (`--mem=NG`) is the range of memory allocated for each node (in GB). 

**“Advised memory per core”** (`--mem-per-cpu=NG`) is effectively the amount of [RAM (Random Access Memory)](https://www.howtogeek.com/697659/what-is-ram-everything-you-need-to-know/) allocated to a given [CPU (Central Processing Unit)](https://www.freecodecamp.org/news/what-is-cpu-meaning-definition-and-what-cpu-stands-for/). 

**“Local scratch per node”** (`--tmp`) is the total amount of temporary storage that you can specify for that partition. 

**“Maximum nodes per job”** (`--nodes=N`) is the highest number of nodes one may be allocated for each job.


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).