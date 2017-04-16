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
Tianpei Luo && Chenhao Wang

## Basic idea:
### v1.0
Verfication:  
Valid and exist instance id  
Valid number  
Valid and exist directory  

fetching info:  
fetch username  
fetch information of the instance  

Create instance->  
Get the instance id and DNS->  
Wait for initializaion->  
Using `ssh hostA "tar -czf - dir" | ssh hostB "tar -xzf -"` to upload the source file->  
Done  

## UNIX utility
`tar -cf - -C srcdir . | tar -xpf - -C destdir`

Trick:
`ssh hostA "tar -czf - dir" | ssh hostB "tar -xzf -"`

