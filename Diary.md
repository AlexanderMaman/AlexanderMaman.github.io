# Alexander Diary

## bsub commands
### Resources:
IBM guide for the LSF cluster: [Link](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0)
IBM Documentation-bsub options: [Link](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=bsub-options)
Weizmann IT powerpoint : [Link](https://www.weizmann.ac.il/WIT/sites/WIT/files/uploads/it/files/wexac_introduction.pdf)
WEXAC’s RTM (real-time monitoring of jobs and server loads): [Link](http://rtm.wexac.weizmann.ac.il/)

### Common commands
```markdown
-q "queue_name ..." which queue to submit the job. The wexac cluster has several queues where jobs can wait to be allocated according to the queue priority. We have our own queue, "schwartz" with 152 available slots; our current priority is very high, perhaps due to us barely using the service. Other general slots are "new-short" (jobs shorter then 24hrs) ,"molgen-q", "molgen-short".
to see queue details, either use the "bqueues" command or visit [link](https://rtm.wexac.weizmann.ac.il/cacti/plugins/grid/grid_bqueues.php)
-n min_tasks[,max_tasks] number of tasks the job is allowed to launch, and the number of slots that will be reserved for this task. For when you want to submit a job with multiple parallel tasks that require multiple CPUs. (note that your requested memory will be for each slot, so submitting -n 10  -R 'rusage[mem=128]  will result in 1280 reserved memory)
-R A resource requirement string describes the resources a job needs. LSF uses resource requirements to select hosts for job execution. There are many options.
Here are some useful ones:
	-R 'span[hosts=1]' Should a parallel job span multiple hosts (here only 1)
	-R 'rusage[mem=xxx] how much memory to reserve for your job, the lower it is, the more machines that will be available for you and the faster you will be allocated out of the queue. A job that will require more than the reserved memory might crash and exit; in addition, asking for much more than what your program will need can result in “memory waste,” where reserved memory turns out idle, supposedly causing a future penalty for your “queue priority”. Notes: 1) memory is in increments of 128 MB, 256 MB, 512 MB, etc. 2) in this method, the requested memory will be cumulatively allocated multiplied by the amount of task/threads requsted.
	-R ‘affinity[thread*n]’ somewhat similar to -n, here you ask the LSF to allocate you n number of threads, IT guy said using this might be better then-n, although couldn’t supply a convincing explanation. Note that allocated memory for the whole job will be -R’rusage[mem=xxx] * number of threads
Please add any other  -R options you find useful.

**Additional options**
-u Sends mail about the job to the specified email destination. The email destination can be an email address or an OS user account.
/dev/null will result in no mail sent, in case you send many jobs and don’t want to flood your email box.
-J  assigns the specified name to the job.
-P name of the project your jobs will be under if you want to easily access all of your jobs later.
-o dir to send the output report file of the job. If specified, an email won’t be sent.
-e where to send the error files. This also seems to include some of the console print of the job.
-cwd the directory to be set as the current working directory for the command. Able to create a folder. Accepts dynamic patterns like : %J - printing job ID %P - project name, and others. If not specified, the dir from where the bsub was sent will be set as the working directory for the command.
-C 1 core file received should not exceed 1 KB, prevents getting a huge GB size error file if the job had an error(??).
-B Issue e-mail notification upon job initiation
-N Issue e-mail notification upon job completion
