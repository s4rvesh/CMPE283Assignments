# CMPE 283 Assignments for Spring 2021
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




