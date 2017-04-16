# EC2-Tooling
## Goal
Implement the afewmore command as specified in this manual page.
Your program will behave exactly as outlined in that manual page, with no additional output or functionality.

## Target platform:

The tool you write will be executed (and graded) on an Ubuntu instance ami-6de0dd04 with the `awscli` package installed via `sudo apt-get install awscli`. If your tool does not work in this environment, you will not get any points. See also: general homework guidelines.

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

