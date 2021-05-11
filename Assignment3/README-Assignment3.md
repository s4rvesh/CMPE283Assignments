# CMPE 283 Assignments for Spring 2021
## Documentation for Assignment 3
### By Omri Levia and Sarvesh Upadhye

### Omri's Contribution
In this assignment I worked with Sarvesh to set up the code in cpuid.c and svm.c. We examined the AMD SDM to view the 
available exit codes, and printed out the supported KVM exit codes to see what the numbers associated with the exit name 
were. After we added the new exit counters for each exit code, we added switch case statements in svm.c on the exit code. 
Once all the code was added, I modified my test .c file to loop through all the exit codes and set eax to the new leaf function. 
I then used this test to show the exit behavior with none or some network traffic. 
### Sarvesh's Contribution

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
