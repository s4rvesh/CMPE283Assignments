# CMPE 283 Assignments for Spring 2021
### By Omri Levia and Sarvesh Upadhye

## Documentation for Assignment 1
### Sarvesh's Contribution
### Omri's contribution
I worked with Sarvesh in a zoom meeting to go over the assignment details and review the lecture recording about the assignment details. On my machine, I retrieved the starter .c file and makefile and began to add the four remaining structs and definitions for the different capability info regions. In the detect_vmx_features function, I added the four remaining reads of the msrs for each capability, and subsequently added the calls to report_capability. I then tried to perform testing by running make, then sudo insmod on the newly created .ko file. 

### Steps to reproduce 
1. retrieve .c file and makefile from canvas
2. Add capability structs based on info from SDM and make calls to rdmsr and report_capability in the .c file
3. in the directory of the .c file and makefile, call 'make'
4. call 'sudo insmode ./cmpe283-1.ko
5. call dmesg to see capabilities 

![make command output](https://user-images.githubusercontent.com/34635965/116323174-517a1e00-a772-11eb-8c7a-218f535ba9fd.png)
![Capability info 1](https://user-images.githubusercontent.com/34635965/116323214-648cee00-a772-11eb-8b0e-dbca7324c155.png)
![Capability info 2](https://user-images.githubusercontent.com/34635965/116323249-72427380-a772-11eb-86f6-ab66a6a60340.png)



