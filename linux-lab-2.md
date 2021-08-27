**1. What is the option used to view the content of a compressed file.**
```bash
touch file1 file2 file3
tar -cvzf files.tar.gz file*
# file1
# file2
# file3
ls
# file1  file2  file3  files.tar.gz
tar -tf files.tar.gz
# file1
# file2
# file3
```
**2. Backup /etc directory using tar utility.**
```bash
sudo tar -cf etcbackup.tar /etc
```
**3. List the inode numbers of /, /etc, /etc/hosts.**
```bash
ls -id / /etc /etc/hosts
# 2 /  16385 /etc  39914 /etc/hosts
```
**4. Copy /etc/passwd to your home directory, use the commands diff and cmp, and Edit in the file you copied, and then use these commands again, and check the output.**
```bash
cp /etc/passwd ~/etcpasswdcopy
cmp /etc/passwd ~/etcpasswdcopy > cmpresults
diff /etc/passwd ~/etcpasswdcopy > diffresults
ls -l
# total 3096
# -rw-r--r-- 1 ubuntu ubuntu       0 Aug 27 23:24 cmpresults
# -rw-r--r-- 1 ubuntu ubuntu       0 Aug 27 23:25 diffresults
# -rw-r--r-- 1 root   root   3164160 Aug 27 23:10 etcbackup.tar
# -rw-r--r-- 1 ubuntu ubuntu    1757 Aug 27 23:23 etcpasswdcopy```
# As can be seen above, cmpresults and diffresults are both of size 0, meaning no differences between the two files were found (i.e., a perfect copy)
```
**5. Create a symbolic link of /etc/passwd in /boot.**
```bash
sudo ln -s /etc/passwd /boot/etcpasswdlink
ls -l /boot
# total 0
# lrwxrwxrwx 1 root root 11 Aug 27 23:32 etcpasswdlink -> /etc/passwd
```
**6. Create a hard link of /etc/passwd in /boot. Could you? Why?**
```bash
sudo ln -v /etc/passwd /boot/etcpasswdhardlink
# '/boot/etcpasswdhardlink' => '/etc/passwd'
# Used the verbose flag just to make sure it is working since the question implies it won't work
ls -l /boot
# total 4
# -rw-r--r-- 2 root root 1757 Aug 27 14:36 etcpasswdhardlink
# lrwxrwxrwx 1 root root   11 Aug 27 23:32 etcpasswdlink -> /etc/passwd

# It works on my system configuration because both the target and destination lie in the same file system
cd ~
stat -f -c %T .
# ext2/ext3
cd /boot
stat -f -c %T .
# ext2/ext3
```
