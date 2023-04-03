# Optimizing SBATCH parameters 

21. Run a few subjects with the pipeline. You can use the same 3-5 jobs you used for [storage estimates](storage.md) to estimate job specifications.

22. Run `seff` on the jobs 

    - `seff` is used to check the memory utilization and CPU efficiency for completed jobs. Note that for running and failed jobs, the efficiency numbers reported by `seff` are not reliable so please use this tool only for successfully completed jobs.

    - The job ID can be found first by using  `squeue -al –me` , and it will be the number associated with the most recent job submission listed under “JOBID”   

        - Use `sacct -S YYYY-MM-DD -u x500` to find job IDs for past jobs that occurred after the specified start date.

    - Command to run: `seff job_id`

    - CPU Efficiency is calculated as the ratio of the actual core time from all cores divided by the number of cores requested divided by the run time.

    - Memory Efficiency is calculated as the ratio of the maximum amount of RAM used by all tasks divided by the memory requested for the job.

    - `seff` output example:

        ![Example Seff Output](img/seff-output-example.png)

        - _Nodes _can be manipulated in the sbatch by the `--nodes` flag. 

        - _Cores per node_ can be manipulated with the `--ntasks-per-node` flag. 

        - _Job Wall-clock time_ is the length of the job in real hours, and can be manipulated with the `-t` or `--time` flags. Based on this output, a time specification of `72:00:00` should suffice. 

        - _Memory Utilized _can be manipulated with the `--mem` flag. Based on this output, a 40 GB specification of memory will allow for a more efficient job versus 60 GB. 
        
        - _CPU Utilized _is the total CPU hours that were allocated based on the request.
        
        - _CPU Efficiency_ is the proportion of the CPU that was utilized for the job compared to the total core-walltime of the CPU.

    - Recommended sbatch parameters per 1 subject process:

        <table>
        <tr>
        <td>
        <strong>Pipeline</strong>
        </td>
        <td><strong>SBATCH Parameters and Definition </strong>
        </td>
        </tr>
        <tr>
        <td>fmriprep
        </td>
        <td>Parameters involve grabbing a maximum time of 24 hours on 32 cpus on the msismall partition, with 240gb of total memory. 
        <p>
        <em>May need increased resources for: multi-echo input data, subjects with more minutes of data than typical ( > 1 hour)</em>
        </td>
        </tr>
        <tr>
        <td>nibabies
        </td>
        <td>Parameters involve grabbing a maximum time of 24 hours on 32 cpus on the msismall partition, with 240gb of total memory. 
        <p>
        <em>May need increased resources for: multi-echo input data, subjects with more minutes of data than typical ( > 1 hour), or data without precomputed derivatives for segmentation (if JLF segmentation is enabled with “--segmentation-atlases-dir” )</em>
        </td>
        </tr>
        <tr>
        <td>abcd-bids-pipeline
        </td>
        <td>The full job will typically take no more than 48 hours. It will utilize 8 tasks on one node (1 cpu per task) with 20gb of memory and 100gb of temporary storage to run effectively. Recommended partition msismalll.
        </td>
        </tr>
        <tr>
        <td>XCP-D
        </td>
        <td>This job will need 3 hours of maximum wall time, 8 cpus, and 96gb of memory. Recommended partition msismall.
        <p>
        <em>May need increased resources for: multi-echo input data, subjects with more minutes of data than typical ( > 1 hour)</em>
        </td>
        </tr>
        <tr>
        <td>dcan-infant
        </td>
        <td>The full job will typically take no more than 72 hours and will utilize 24 tasks on one node with 60gb of memory and 100gb of temporary storage. Recommended partition msismall.
        </td>
        </tr>
        </table>


*note: all job specifications can increase or decrease based on the amount of data (size or file number) in your dataset. These are recommendations based on job specifications for typical datasets with some buffer added. 

