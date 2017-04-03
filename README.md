# EC2-Tooling
## Goal
Implement the afewmore command as specified in this manual page.
Your program will behave exactly as outlined in that manual page, with no additional output or functionality.

## Target platform:

The tool you write will be executed (and graded) on an Ubuntu instance ami-6de0dd04 with the 'awscli' package installed via 'sudo apt-get install awscli'. If your tool does not work in this environment, you will not get any points. See also: general homework guidelines.

## Basic idea:
### v1.0
Find out the image id of this specific instance --> create number of instance according to the argv -n (default 10)-->find out all instances IP/domain_name -->send to the first instance using `rsync(1)` -->  ssh to the first instance -> compress the target directory using `tar(1)`(default /data) -->using a for loop to copy the file to the target intance with `rsync(1)` --> done!!!

### `scp` vs `rsync`
` scp ` basically reads the source file and writes it to the destination. It performs a plain linear copy, locally, or over a network.
`rsync` also copies files locally or over a network. But it employs a special delta transfer algorithm and a few optimizations to make the operation a lot faster. Consider the call.

`rsync A host:B`
* rsync will check files sizes and modification timestamps of both A and B, and skip any further processing if they match.
* If the destination file B already exists, the delta transfer algorithm will make sure only differences between A and B are sent over the wire.
* rsync will write data to a temporary file T, and then replace the destination file B with T to make the update look "atomic" to processes that might be using B.
Anther difference between them concerns invocation. rsync has a plethora of command line options, allowing the user to fine tune its behavior. It supports complex filter rules, runs in batch mode, daemon mode, etc. scp has only a few switches.

In summary, use scp for your day to day tasks. Commands that you type once in a while on your interactive shell. Its simpler to use, and in those cases rsync optimizations won't help much.

For recurring tasks, like `cron` jobs, use rsync. As mentioned, on multiple invocations it will take advantage of data already transferred, performing very quickly and saving on resources. It is an excellent tool to keep two directories synchronized over a network.

Also, when dealing with large files, use rsync with the -P option. If the transfer is interrupted, you can resume it where it stopped by reissuing the command.
