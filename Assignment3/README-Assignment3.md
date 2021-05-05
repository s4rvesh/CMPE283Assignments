# CMPE 283 Assignments for Spring 2021
## Documentation for Assignment 3
### By Omri Levia and Sarvesh Upadhye

### Omri's Contribution
### Sarvesh's Contribution 

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
