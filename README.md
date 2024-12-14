[![Discord](https://img.shields.io/discord/423767742546575361.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/vpEv3HJ)

# RAKF_XA
This is the same old RAKF system (made by ESG Systems Inc) that is used in MVS 3.8 TK4, but adapted back again to MVS/SP and MVS/XA.

The original RAKF was backported from ESG to MVS 3.8 which uses a different way to verify access than later versions of MVS. Also, several bugs were fixed. 

MVS/SP and MVS/XA use the RACROUTE macro instead of the older RACHECK, RACDEF, RACINIT and so on, of MVS 3.8). Knowing this, it was simply a matter of looking at the differences in the RACROUTE parameter list vs. the parameter lists used by the old macros.  And then adjusting RAKF code to grab the stuff it needed from the new RACROUTE parameter lists. In short, it works. 

This RAKF applies cleanly to MVS/SP 1.3.5 and MVS/XA, for those who still run these slightly older z/OS versions. 

Installation
============

1. Load the RAKF-for-XA materials from the provided tape image. Use the simple IEBCOPY jobstream from the file ‘job000.txt’ in this distro to get started and load the install library.  
After the install library is loaded, the subsequent steps below refer to various jobs and members within the install library (RAKF.V1R2.XA.INSTALL). Use TSO to edit and submit jobs from this dataset.  
2. Verify and submit member JOB010. This job will load the remainder of the RAKF materials from the tape.  
3. Verify and submit member JOB100. This job will create the RAKF control datasets which contain the users and profile rules, set up the JCL procs, and install an updated TSO VTOC command for XA.  
4. Verify and submit member JOB110. This job reassembles and links the updated MSTJCL00 module in SYS1.LINKLIB, as it now must contain the DD statements for the RAKF datasets.  
5. Verify and submit member JOB120. This job will assemble and link all of the RAKF code base.  
6. Edit the RAKF control dataset SYS1.RAKF.CNTL(USERS) to reflect your desires for security. Ensure
that the userids in the USERS member match the users that you have already defined to TSO in SYS1.UADS. The passwords in the USERS member should match the actual password; after IPL the passwords in the USERS member will be the passwords for userids, regardless of what is in SYS1.UADS. Remember that userids need to be in ascending sort order.  
7. Edit SYS1.RAKF.CNTL(PROFILES) to reflect your desires for system access to various datasets. Add or remove additional datasets as desired. Generally, in the PROFILES member provided, general users have READ access to everything except where overridden by other profiles (see the “DATASET *” profile). For example, if you don’t want them to have general READ access, you could change this profile to NONE and then add new profiles specifically for what they can access. Remember that all profiles must be in ascending sort order starting with the profile name (e.g., DASDVOL, DATASET, etc), and within those, by dataset name.  
8. Shutdown and IPL the system. YOU MUST IPL WITH THE CLPA OPTION.  

Read the tape in. 

Moshix  
Moscow, January 2025
