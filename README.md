/****************************************************
					ROOTKIT
 		CSE 509, System Security ,Fall 2016

Team : Code Wizard				Team Members: 
       							      Devesh Sisodia (110951296)
Date: Nov 27, 2016				Salman Masood
 							            Tanvi Kawsar
 							            Mitesh Kumar Savita

****************************************************/


What is it?
========================

It is a robust kit which when installed after gaining access to a system , will hide itself from all other users and will not let them suspect that the system is compromised. Also, it creates a backdoor via which the attacker can anytime regain access to the machine and elevate itself to root.


Platform Used:-
=======================

It is tested on latest Ubuntu 16.04.01 LTS


Installation:-
========================









Functionalities:
======================
Once Loaded it will provide below functionality:
a)	Restrict other users to find any proof of the backdoor account, they can’t even find any traces from etc/passwd and etc/shadow file
b)	It hides the processes from the process table when user does a “ps”
c)	It can elevate the backdoor account to alleviate itself to root.
d)	It hides the specific files and directories while user tries to ls.


How each of the above features are Implemented?
==========================================
a.) Restrict other users to find any proof of the backdoor account , they can’t even find any traces from etc/passwd or etc/shadow file.

Implementation: 
Intercepting Read: We intercepted the read system call , and try to detect whenever other user issued command for reading  etc/passwd and etc/shadow file. Once the file is detected we try to check presence of “rootkit” in the file buffer. If the user “rootkit” we forward the buffer after removing backdoor entry to the original read.

Intercepting Write for Write protection: Although we remove the backdoor user entry from getting read. Other user can just safe the file which they are viewing and our backdoor entry will be removed as it is not present in the content displayed to user. So in this case we add the “rootkit” to the end of file whenever write is executed on these files
Function: rootkit_read , rootkit_write


b.) Elevating the Backdoor account user access to root.

Implementation:
We hijacked the get_uid system call and check if the uid is 61377 elevate access to root.


c) Hiding the specific files and directories while user tries to “ls”
Implementation:
Hijacking the getdent system call and hide the malicious files from getting displayed to other user.
Function: rootkit_getdents

d) Hiding the specific files and directories while user tries to “ps”
Implementation:
Hijacking the getdent system call and hide the malicious files from getting displayed to other user.
Function: rootkit_getdents

 
Contribution:-
===================================
All the members have clear understanding and helped eachother in their short comings. Although each member has put specificfically took interest and contributed to below parts:-
Mitesh and Devesh: Implemented hiding ls and ps command .Intercepted the read system call to restrict other user from reading the  backdoor account details from etc/passwd and etc/shadow.
Salman and Tanvi : Implemented Write protection to protect the etc/shadow and etc/passwd file from overwritten. Elevated the access of backdoor account to root.

