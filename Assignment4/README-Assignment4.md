
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
