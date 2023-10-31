# Optimizing Your SLURM Queue

This can be applied to ANY type of job submitted on MSI and beyond. You don't have to use all of them every time, but it is advisable to use all of them whenever you're submitting a lot of jobs or a few resource intensive jobs. These steps will save you time in the long run even though they take a little bit of time prior to submission. They prevent your jobs from sitting in queue for days or weeks on end.

## Selecting a Suitable Partition

In order to maximize job queueing efficiency, you need to choose the [MSI partitions](partitions.md) you use wisely. Your partition choice should be made using your SBATCH parameters. With your already optimized job parameters, you want to pick a partition that fits your job specifications and the cluster you are currently using (Agate, Mesabi, or Mangi). The more partitions that work, the better. You can list multiple in your job specifications. An example of how to do that is shown below.

```
#SBATCH -p small,amdsmall,amd512,ram256g
```

Likewise, the more you've tailored your job parameters to fit the job you are running, the quicker you will queue. You can always over estimate, but that means you have to wait for more "space" on the node for each of your jobs. You want to find a happy medium where you are just over the maximum parameters you've seen for your jobs (see [this page](slurm.md) for information on how to optimize your job parameters and check your past jobs' usages.)

## Spread out Your Jobs

To take advantage of the breadth of MSI, it is advisable to submit your jobs in groups. Each partition, each person submitting, each cluster, and each group have different priority "rankings" within MSI. Intelligently spreading out your jobs amongst each of those categories will ensure you're queueing through at a reasonable speed for your jobs' sizes. 

Above, we touched on how to spread out your jobs on different partitions (use multiple in the SBATCH partition parameter), but that is not the only way we can distribute jobs. We can also submit under multiple PI groups and clusters and have multiple people submit the jobs for us.

## Utilizing Multiple Groups

Submitting jobs under multiple PI groups cannot be done in the same way as submitting under multiple partitions, unfortunately. The more groups you have access to, the more you can spread out your submissions. In order to choose which partitions to submit under you'll need to use [fairshare](fairshare.md) to see which accounts have the highest queueing efficiency at the time you are wanting to submit the jobs. This will change frequently. 

To submit the jobs, you will need to submit a subset under each group one at a time. For example, if I had 588 jobs to submit, I would submit half under the group with the highest queueing efficiency and the other half under the second highest queueing efficiency, modifying the group parameter between submissions. This is not an exact science, however, which means you might end up wanting to switch the group on some of the jobs if they've sat stagnant in the queue for awhile (days) - see scontrol under "Other useful commands" [here](slurm.md).

The [SBATCH parameter](slurm-params.md) to modify to change the group is shown below:
```
#SBATCH -A group_name
```

## Take Advantage of all of the Clusters

You can ssh into different clusters (Mesabi, Mangi, Agate) in order to submit jobs on each of them. Just be sure to change your partitions to match your new cluster each time. As you can imagine, this has a similar effect to submitting under different partitions. 

## Teamwork Makes the Dream Work

Utilize your team to submit groups of jobs as well. Do not wait to do this when you've maxed out your queue. This makes things go faster no matter how many jobs you are submitting. Make sure your team members are checking their [fairshare](fairshare.md) and modifying the [SBATCH group parameter}(slurm-params.md) as well.
