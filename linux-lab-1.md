**1. Create a user account with the following attributes**
 - Username: *islam*
 - Fullname/comment: *Islam Askar*
 - Password: *islam*
```bash
sudo useradd -c "Islam Askar" -p islam
```
**2. Create a user account with the following attributes**
 - Username: *baduser*
 - Full name/comment: *Bad User*
 - Password: *baduser*
```bash
sudo useradd baduser -c "Bad User" -p baduser
```
**3. Create a supplementary (Secondary) group called *pgroup* with group ID of *30000***
```bash
sudo groupadd pgroup -g 30000
```
**4. Create a supplementary group called *badgroup***
```bash
sudo groupadd badgroup
```
**5. Add *islam* user to the group *pgroup* as a supplementary group**
```bash
sudo usermod -a -G pgroup islam
```
**6. Modify the password of *islam*'s account to *password***
```bash
sudo passwd islam
```
**7. Modify *islam*'s account so the password expires after *30 days***
```bash
sudo chage islam -M 30
```
**8. Lock *baduser*'s account so he can't log in**
```bash
sudo usermod -L baduser
sudo chage -E0 baduser
```
**9. Delete *baduser*'s account**
```bash
sudo userdel baduser
```
**10. Delete the supplementary group called *badgroup***
```bash
sudo groupdel badgroup
```
**13. Create a folder called *myteam* in your home directory and change its permissions to *read-only for the owner***
```bash
cd ~
mkdir -m 477 myteam
```
**14. Log out and log in by another user**
```bash
# Doing so on WSL as follows
logout
wsl -d ubuntu -u islam
```
**15. Try to access (by cd command) the folder (myteam)**
```bash
# Assuming sequential tasks flow, I should be on islam's account currently...
cd /home/ubuntu/myteam
# Result: Accessible, because I made the owner (named "ubuntu") have read-only permissions but "other" have full access; islam classifying as "other"
```
**16. Using the command Line**
***Change the permissions (using chmod in 2 different ways) of oldpasswd file to give:***
- ***owner:*** read and write permissions
- ***group:*** write and execute
- ***other:*** execute only
```bash
# Assuming the "oldpasswd" file actually means "passwd-"...
sudo chmod 631 /etc/passwd-
sudo chmod u=rw,g=wx,o=x /etc/passwd-
```
***Change your default permissions to be as above.***
```bash
# Note that it is impossible to set the execute permission as default for a file
umask 146
```
***What is the maximum permission a file can have, by default when it is just created? And what is that for directory.***
```bash
# 666 (rw-) for files
# 777 (rwx) for directories
```
***Change your default permissions to be no permission to everyone then create a directory and a file to verify.***
```bash
umask 777
mkdir a
touch b
ls -l
```
**17. What are the minimum permission needed for:**
***Copy a directory (permission for source directory and permissions for target parent directory)***
```bash
# r-x for source directory
# -wx for target directory
```
***Copy a file (permission for source file and and permission for target parent directory)***
```bash
# r-- for file
# -wx for target directory
```
***Delete a file***
```bash
# -w-
```
***Change to a directory***
```bash
# --x
```
***List a directory content (ls command)***
```bash
# r--
```
***View a file content (more/cat command)***
```bash
# r--
```
***Modify a file content***
```bash
# The minimum required permission to append data to a file is write-only (-w-)
# The minimum required permission to modify data in a file is read-write (rw-)
```
***18. Create a file with permission 444. Try to edit in it and to remove it? Note what happened.***
```bash
touch file444
chmod 444 file444
vi file444
# "file444" [readonly]
ls
file444
cat file444
rm file444
rm: remove write-protected regular empty file 'file444'? yes
```
***19. What is the difference between the “x” permission for a file and for a directory?***
```bash
# If the file is executable, it means to execute (run) its code, otherwise it has no meaning
# For directories, it means you can change directory (cd) to it (i.e., make it the current working directory)
```
***20. Issue the command "sleep 100" in background.***
```bash
sleep 100 &
# [1] 508
jobs
# [1]+  Running                 sleep 100 &
```
***21. Kill the sleep command.***
```bash
kill %1
jobs
# [1]+  Terminated              sleep 100
```
***22. Display your processes only***
```bash
ps u
# USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
# ubuntu     191  0.0  0.0  10836  5888 pts/0    Ss   14:54   0:00 -bash
# ubuntu     509  0.0  0.0  10620  3184 pts/0    R+   22:12   0:00 ps u
```
***23. Display all processes except yours***
```bash
# I opened another terminal on WSL and logged in as "islam" and ran "sleep 1000" for demonstration purposes, to show that the shown processes do not belong to the current user, "ubuntu", rather are root's and islam's only
ps u -NU ubuntu
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
# root         1  0.0  0.0    896   580 ?        Sl   14:27   0:00 /init
# root       189  0.0  0.0    904    80 ?        Ss   14:54   0:00 /init
# root       190  0.0  0.0    904    88 ?        R    14:54   0:01 /init
# root       867  0.0  0.0    904    80 ?        Ss   22:52   0:00 /init
# root       868  0.0  0.0    904    88 ?        S    22:52   0:00 /init
# islam      869  0.8  0.0  10056  5008 pts/1    Ss   22:52   0:00 -bash
# islam      882  0.0  0.0   7232   588 pts/1    S+   22:52   0:00 sleep 1000

```
***24. Use the pgrep command to list your processes only***
```bash
# I opened another terminal on WSL and ran sleep on it in order to have multiple processes for demonstration purposes (otherwise the only process run by my user, "ubuntu", is bash)
pgrep -l -u ubuntu
# 191 bash
# 575 bash
# 589 sleep
```
