# EC2-Tooling
## Goal
Implement the afewmore command as specified according to the [manual page](https://github.com/ltp19930730/EC2-Tooling/blob/master/manual.txt) .
This script will behave exactly as outlined in that manual page, with no additional output or functionality.

## Target platform:

The tool can executed on an Ubuntu instance ami-6de0dd04(or other platform) with the `awscli` package installed via `sudo apt-get install awscli`. This script also based on the configuration of your ssh file which is under `~/.ssh/config`, make sure you have the right configuration like this :
```
Host *amazonaws.com
  IdentityFile ~/.ssh/{ your_key.pem }
```

## Team:
Tianpei Luo && Chenyao Wang

## Basic idea:
### Verification:
Valid and exist instance id.
Valid argument.
Valid and exist directory on target instance.

### Fetching information:
Found the which kind of user the target instance it is by trying all types in turn(ubuntu,ec2-user,centos,fedora,admin.root).
Fetch the target instance's information such as availability zone, security group id etc in order to create new instance.

### Create instance:
Create instance with same attributes of the target instance.
Save all the information to as file as json and grep all the new instances' id.
Search the public dns of new instances by using instance's id.

### Waiting initializing
Ping all new instances and get the status for every 10s.

### transfer data
After all new instances initialized we can start transfer information.
Version 1.0
	Using scp to download the target data into current instances and saved in temporary directory.
	Using scp to upload the temporary directory into all the instances.
Version 2.0
	Using ssh target instance and tar command to download target directory and compress and then use pipe,
	ssh and tar command again to unzip and upload.

##  Problem and Overcome
1.Problem: We can't make sure what kind of user since there is no such argument in this tool.
Overcome: We use `ssh user@dns` to ping all kind of users(ubuntu,ec2-user,centos,etc).if it's timeout,try next one .
2.Problem: We can't make sure how long we should wait before we starting upload data to new instances since
we can't upload before it's initialized.
Overcome: We use `aws ec2 describe-instance-status --instance-ids $id | grep '"Status": "ok",' | wc -l` command to 
check if it's initialized.
3.Problem: There is no the dns of new instance in the output of `aws ec2 run-instances`.
Overcome: Use the instances' id and command `aws ec2 describe-instances` to grab all new instances' dns.
4.Problem: Can't make use the string after -n is a number.
Overcome: Use a regular expression to check it.
5.Problem: There will be a yes/no question when using scp or ssh command.
Overcome: By adding an argument `-o "StrictHostKeyChecking no"` into the command.

##  Tests
1. Test the tool on all platforms(ubuntu,ec2-user,entos,fedora,admin,root).
2. Test the case that target directory doesn't exist.
3. Test the string after -n is not number.
4. Test the time when in bad network or no network.
5. Test the case that target instance doesn't exist.
6. Test the case handle can't create so many instances.

##  Trick:
	`ssh hostA "tar -czf - dir" | ssh hostB "tar -xzf -"`

