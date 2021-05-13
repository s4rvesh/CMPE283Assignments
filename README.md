# CMPE 283 Assignments for Spring 2021
### By Omri Levia and Sarvesh Upadhye


## Documentation for Assignment 1
(We both performed the assignment on our machines)

### Sarvesh's Contribution
Me along with Omri worked over a zoom meting to complete the assignment. I started by creating an VIrtual Machine (Ubuntu 20.0) on VMware Workstation. I then  downloaded the Makefile and the .c file on my VM, and added structs and definations looking up in SDM for different capability region, added remaining msrs for each capability and also report_capability. For testing the code, i performed "make" command, to make the module, and the inserted the module using insmod (using .ko file created) in kernel, and tracking the "dmesg".
Initially there was error, as the address would show 0x0, as i had not enabled VTX feature on my workstation, but was solved once i enabled it).
I then created the git repo for the assignment, and i along with omri started pushing our code to the repo.
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

----------------------------------------------------------------------------------------------------------------------------------------------------------

## Documentation for Assignment 2
### By Omri Levia and Sarvesh Upadhye
(We both performed the assignment on our machines)


### Sarvesh's Contribution
I worked on this assignment with Omri. Omri had forked the torvalds/linux repo, which i cloned. I first made an oldconfig for the kernel, and also build and installed updated kernel using make modules, make, make modules_install and make install. I then added leaf function to kvm_emulate_cpuid and also spent time reading about atomic variables while doing so. I also installed Virt-Manager and installed an Kali linux on it to test my code. I worked with Omri to add exit code counts in svm.c and catching it in cpuid.c leaf function.

### Omri's Contribution
I worked with Sarvesh to review the steps on retrieving and building the linux kernel and discussed some steps to take to completing the assignment. In my dev environment, I forked and locally cloned the linux kernel repo began experimenting with editing KVM's cpuid.c file by adding a leaf function to kvm_emulate_cpuid. In parallel I spent some time practicing building the kernel and preparing a nested vm for testing. Initially I tried executing qemu-system_x86-64 commands but it proved very difficult so I used Virtual Machine Manager instead. I worked with Sarvesh to add exit measurement code in svm.c, and reporting information in the cpuid.c leaf function. 

### Steps to reproduce
1. Fork linux tree
2. Clone locally into vm (ubuntu 20.04 used)
3. downloaded essential packages for building kernel from ubuntu wiki 
4. ran 'sudo cp /boot/{config-name} ./.config
5. ran 'sudo make oldconfig'
6. edited measurement code in svm.c and reporting code (writes to eax, ebx, ecx) in cpuid.c
7. ran 'sudo make -j {num_cores}'
8. ran 'sudo make -j {num_cores} modules_install'
9. ran 'sudo make'
10. ran 'sudo make install'
11. ran 'sudo reboot', on ubuntu, I had to go into the grub.cfg and change the default kernel and reboot again. 
12. Created nested vm with virtual machine manager (debian worked as a nested vm, ubuntu complained for some reason)
13. Created testing code that calls cpu in inline assembly 
14. Observed output and dmesg output

![dmesg output after calling cpuid in nested vm](https://user-images.githubusercontent.com/34635965/116480252-d7ab6880-a835-11eb-9278-6f031e044f63.png)

### Question 3 Response
Directly after bootup (from shutoff) of the nested VM (Debian within Ubuntu) there are around a million exits counted:
![Exits after bootup](https://user-images.githubusercontent.com/34635965/116606161-295efc00-a8e5-11eb-9fbd-29f01a34f5a1.png)

After a sudo reboot, we can see the count increase by another 500k exits about:
![Count after sudo reboot](https://user-images.githubusercontent.com/34635965/116606318-60351200-a8e5-11eb-958c-cbe356830be5.png)

Waiting a few seconds and running the program again:

![Running program every few seconds](https://user-images.githubusercontent.com/34635965/116606393-85c21b80-a8e5-11eb-92e9-b6216eb0a92c.png)

Running the program every few seconds shows that the number of exits seems to monotonically increase. Now I will open up a bunch of browsers, and try to generate many more exits by opening lots of applications:

![Opening up browser windows and a bunch of programs](https://user-images.githubusercontent.com/34635965/116606638-dc2f5a00-a8e5-11eb-90e4-4dd71a7e96fb.png)

After opening up several browser windows and applications, the number of exits shot up by about 20k.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Documentation for Assignment 3
### By Omri Levia and Sarvesh Upadhye

### Omri's Contribution
In this assignment I worked with Sarvesh to set up the code in cpuid.c and svm.c. We examined the AMD SDM to view the 
available exit codes, and printed out the supported KVM exit codes to see what the numbers associated with the exit name 
were. After we added the new exit counters for each exit code, we added switch case statements in svm.c on the exit code. 
Once all the code was added, I modified my test .c file to loop through all the exit codes and set eax to the new leaf function. 
I then used this test to show the exit behavior with none or some network traffic. 

### Sarvesh's Contribution
I worked with Omri to write the function needed in cpuid.c and svm.c. As we worked on AMD machine, we used AMD's SDM for the exitcodes and printing them along with their exit_names.
We added counter for each exit_code in svm.c using switch case and incremented them. For testing, we created a C program to loop through all exit_codes and to set register eax values to leaf function. We used this testing program with generating some traffic.

### Question 2: Steps to reproduce
1. Begin with forked linux repo, cloned on local machine
2. Add cpuid.c 0x4ffffffe leaf, and define counters for supported kvm exit codes
3. In leaf, use switch case to determine which counter to report 
4. In svm.c use swtich case on exit_code to determine which counter to increment in exit_handler function
5. After code is added, perform steps to rebuild the kernel:
  5a. sudo make -j 4 modules --> sudo make -j 4 modules_install --> sudo make install --> sudo make --> sudo reboot
6. Open nested vm and call cpuid with eax = 0x4ffffffe

### Question 3 Response
Directly after bootup (from shutoff) of the nested VM (Debian within Ubuntu) there are around a million exits counted:
![Exits after bootup](https://user-images.githubusercontent.com/34635965/116606161-295efc00-a8e5-11eb-9fbd-29f01a34f5a1.png)

After a sudo reboot, we can see the count increase by another 500k exits about:
![Count after sudo reboot](https://user-images.githubusercontent.com/34635965/116606318-60351200-a8e5-11eb-958c-cbe356830be5.png)

Waiting a few seconds and running the program again:

![Running program every few seconds](https://user-images.githubusercontent.com/34635965/116606393-85c21b80-a8e5-11eb-92e9-b6216eb0a92c.png)

Running the program every few seconds shows that the number of exits seems to monotonically increase. Now I will open up a bunch of browsers, and try to generate many more exits by opening lots of applications:

![Opening up browser windows and a bunch of programs](https://user-images.githubusercontent.com/34635965/116606638-dc2f5a00-a8e5-11eb-90e4-4dd71a7e96fb.png)

After opening up several browser windows and applications, the number of exits shot up by about 20k. 

### Question 4 Response
Below are screenshots showing exit frequency for each exit code defined in the AMD SDM:

![exitfreq_1_final](https://user-images.githubusercontent.com/34635965/117212008-99222a80-adae-11eb-8e2b-2daad24b7a87.png)


![exitfreq_2_final](https://user-images.githubusercontent.com/34635965/117212026-9fb0a200-adae-11eb-8f4c-19ef891edcc9.png)


When it comes to the least number of exits, there are many codes which do not have any exits at all. Some exit codes that don't have exits are exit code 3 (move to CR3)
and exit code 81 (VMMCALL).

The most frequent exit code was 124 with 847948 exits. Exit code 124 corresponds to VMEXIT_MSR. 

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Documentation for Assignment 4
### By Omri Levia and Sarvesh Upadhye

### Omri's Contribution
In this assignment, Sarvesh and I worked on determining the amd specific commandline arg to turn off nested paging upon insertion of the kvm module,
e.g., npt=0. On my machine I ran the test code to produce the screenshots with npt off and on, both with some network traffic and without as well. 

### Sarvesh's Contribution 
I worked on this asignment with Omri. We determined AMD specific command line agruments to turn Off nested paging once inserted in KVM module.
On machine then we ran test code with Nested Page Table on and off, with and without network traffic as well.

### Question 2 Reponse

#### Exits with EPT enabled
![image](https://user-images.githubusercontent.com/34635965/117696969-cab44080-b176-11eb-9f86-5ea81d972fb0.png)
![image](https://user-images.githubusercontent.com/34635965/117696990-d1db4e80-b176-11eb-84f7-70dfbb4c4a6c.png)



#### Exits with EPT disabled
![npt_off_1](https://user-images.githubusercontent.com/34635965/117696770-8a54c280-b176-11eb-8882-8e502c425e27.PNG)
![npt_off_2](https://user-images.githubusercontent.com/34635965/117696777-8de84980-b176-11eb-8017-46be0f646ab9.PNG)

#### Exits with EPT disabled and inducing network traffic 
![ept_disabled_networktraffic](https://user-images.githubusercontent.com/34635965/117696836-9e98bf80-b176-11eb-9ab2-2a1d67bfb0fb.PNG)
![ept_disabled_networktraffic_2](https://user-images.githubusercontent.com/34635965/117696851-a0fb1980-b176-11eb-81d9-f076283f49c2.PNG)

### Question 3 Reponse
Interestingly the default behavior on each nested vm boot shows that move to cr0 and move to cr3 get 5 and 0 exits respectively every time. 
It is a little unexpected to see no exits on boot to move to cr3. Also the instruction that generated the most exits is VMCALL. 
This instruction makes use of underlying services of the VM monitor, so it makes sense that this one is used and exited on frequently. 

### Question 4 Reponse
The exit counts with ept disabled compared with enabled show what we would expect. With ept disabled, 
the exit code 0 and 3 shows significantly increased exits. This makes sense since each code corresponds to 
the instruction for move to CR0 and move to CR3 respectively. With nested paging, the guest can perform its page table operations 
with no intervention by the hypervisor, so when it is turned off, hypervisor intervention must resume. 
