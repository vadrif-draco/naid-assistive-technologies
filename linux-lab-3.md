## Using sed utility
**1. Display the lines that contain the word “lp” in /etc/passwd file.**
```bash
sed -n '/lp/p' /etc/passwd
```
**2. Display /etc/passwd file except the third line.**
```bash
sed -n '3!p' /etc/passwd
```
**3. Display /etc/passwd file except the last line.**
```bash
sed -n '$!p' /etc/passwd
```
**4. Display /etc/passwd file except the lines that contain the word “lp”.**
```bash
sed -n '/lp/!p' /etc/passwd
```
**5. Substitute all the words that contain “lp” with “mylp” in /etc/passwd file.**
```bash
sed 's/lp/mylp/g' /etc/passwd
```
<br>

## Using awk utility
**1. Print full name (comment) of all users in the system.**
```bash
 awk -F: '{print $5}' /etc/passwd
```
**2. Print login, full name (comment) and home directory of all users.( Print each line preceded by a line number)**
```bash
awk -F: '{print NR,"\tLogin: ",$1,"\tFName: ",$5,"\tHomeDir: ",$6}' /etc/passwd
```
```bash
# Result Sample:
# 1       Login:  root    FName:  root    HomeDir:  /root
# 2       Login:  daemon  FName:  daemon  HomeDir:  /usr/sbin
# 3       Login:  bin     FName:  bin     HomeDir:  /bin
# 4       Login:  sys     FName:  sys     HomeDir:  /dev
# 5       Login:  sync    FName:  sync    HomeDir:  /bin
# 6       Login:  games   FName:  games   HomeDir:  /usr/games
# 7       Login:  man     FName:  man     HomeDir:  /var/cache/man
# 8       Login:  lp      FName:  lp      HomeDir:  /var/spool/lpd
# 9       Login:  mail    FName:  mail    HomeDir:  /var/mail
# 10      Login:  news    FName:  news    HomeDir:  /var/spool/news
# 11      Login:  uucp    FName:  uucp    HomeDir:  /var/spool/uucp
# ...
```
**3. Print login, uid and full name (comment) of those uid is greater than 500**
```bash
awk -F: '{if ($3 > 500) print $1, $3, $5}' /etc/passwd
```
**4. Print login, uid and full name (comment) of those uid is exactly 500**
```bash
awk -F: '{if ($3 == 500) print $1, $3, $5}' /etc/passwd
```
**5. Print line from 5 to 15 from /etc/passwd**
```bash
awk 'NR>=5 && NR<=15 { print NR,$0 }' /etc/passwd
```
```bash
# Results:
# 5        sync:x:4:65534:sync:/bin:/bin/sync
# 6        games:x:5:60:games:/usr/games:/usr/sbin/nologin
# 7        man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
# 8        lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
# 9        mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
# 10       news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
# 11       uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
# 12       proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
# 13       www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
# 14       backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
# 15       list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
```
**6- Get the sum of all accounts id’s.**
```bash
awk -F: '{print NR,"\tCumulative sum so far:",sum += $3,"\tLast added UID:",$3}' /etc/passwd
```
```bash
# Results:
# 1     Cumulative sum so far: 0        Last added UID: 0
# 2     Cumulative sum so far: 1        Last added UID: 1
# 3     Cumulative sum so far: 3        Last added UID: 2
# 4     Cumulative sum so far: 6        Last added UID: 3
# 5     Cumulative sum so far: 10       Last added UID: 4
# 6     Cumulative sum so far: 15       Last added UID: 5
# 7     Cumulative sum so far: 21       Last added UID: 6
# 8     Cumulative sum so far: 28       Last added UID: 7
# 9     Cumulative sum so far: 36       Last added UID: 8
# 10    Cumulative sum so far: 45       Last added UID: 9
# 11    Cumulative sum so far: 55       Last added UID: 10
# 12    Cumulative sum so far: 68       Last added UID: 13
# 13    Cumulative sum so far: 101      Last added UID: 33
# 14    Cumulative sum so far: 135      Last added UID: 34
# 15    Cumulative sum so far: 173      Last added UID: 38
# 16    Cumulative sum so far: 212      Last added UID: 39
# 17    Cumulative sum so far: 253      Last added UID: 41
# 18    Cumulative sum so far: 65787    Last added UID: 65534
# 19    Cumulative sum so far: 65887    Last added UID: 100
# 20    Cumulative sum so far: 65988    Last added UID: 101
# 21    Cumulative sum so far: 66090    Last added UID: 102
# 22    Cumulative sum so far: 66193    Last added UID: 103
# 23    Cumulative sum so far: 66297    Last added UID: 104
# 24    Cumulative sum so far: 66402    Last added UID: 105
# 25    Cumulative sum so far: 66508    Last added UID: 106
# 26    Cumulative sum so far: 66615    Last added UID: 107
# 27    Cumulative sum so far: 66723    Last added UID: 108
# 28    Cumulative sum so far: 66832    Last added UID: 109
# 29    Cumulative sum so far: 66942    Last added UID: 110
# 30    Cumulative sum so far: 67053    Last added UID: 111
# 31    Cumulative sum so far: 68053    Last added UID: 1000
# 32    Cumulative sum so far: 68165    Last added UID: 112
# 33    Cumulative sum so far: 69166    Last added UID: 1001
```
<br>

## Bash Scripting
**1. Write a script called mycase, using the case utility to checks the type of *character* entered by a user:**
- ***a. Upper Case.***
- ***b. Lower Case.***
- ***c. Number.***
- ***d. Nothing.***
```bash
touch mycase.sh; chmod u+x mycase.sh; nano mycase.sh
```
```bash
#! /bin/bash
while :
do
        read -p "Enter character: " c
        case $c in
                [A-Z]) echo "Upper Case";;
                [a-z]) echo "Lower Case";;
                [0-9]) echo "Number";;
                *) echo "Nothing";;
        esac
done
```
```bash
# Results:
# Enter character: a
# Lower Case
# Enter character: b
# Lower Case
# Enter character: y
# Lower Case
# Enter character: z
# Lower Case
# Enter character: A
# Upper Case
# Enter character: B
# Upper Case
# Enter character: Y
# Upper Case
# Enter character: Z
# Upper Case
# Enter character: 0
# Number
# Enter character: 1
# Number
# Enter character: 8
# Number
# Enter character: 9
# Number
# Enter character: @
# Nothing
# Enter character: ;
# Nothing
# Enter character: #
# Nothing
# Enter character: (Entered a space here)
# Nothing
# Enter character: (Just pressed enter)
# Nothing
# Enter character: ^C
```
**2. Enhance the previous script, by checking the type of *string* entered by a user:**
- ***a. Upper Cases.***
- ***b. Lower Cases.***
- ***c. Numbers.***
- ***d. Mix.***
- ***e. Nothing.***
```bash

```
**3. Write a script called mychmod using for utility to give execute permission to all files and directories in your home directory.**
```bash
touch mychmod.sh; chmod u+x mychmod.sh; nano mychmod.sh
```
```bash
#! /bin/bash
cd ~ 
chmod u+x *
```
**4. Write a script called mybackup using for utility to create a backup of only files in your home directory.**
```bash
touch mybackup.sh; chmod u+x mybackup.sh; nano mybackup.sh
```
```bash
#! /bin/bash
cd ~
tar -cf homebackup.tar ~
```
**5. Write a script called mymail using for utility to send a mail to all users in the system.**
- ***Note: write the mail body in a file called mtemplate.***
```bash
touch mymail.sh; chmod u+x mymail.sh; nano mymail.sh
```
```bash
#! /bin/bash
for user in `awk -F: '{ print $1 }' /etc/passwd :
do
	$user mail -s "" $user < $mtemplate
done
```
**6. Write a script called chkmail to check for new mails every 10 seconds. Note: mails are saved in /var/mail/username.**
```bash
touch chkmail.sh; chmod u+x chkmail.sh; nano chkmail.sh
```
The following script should be run in the background forever, maybe launch it when shell starts by putting it in .bashrc...
```bash
#! /bin/bash
numofmails=$(ls /var/mail/username | wc -l) # Initialize current no. of mails
while : # Infinite loop
do
  sleep 10 # Sleep for 10 seconds every iteration
  if [[ $(ls /var/mail/username | wc -l) > $numofmails ]]; then # If number of files in the directory (aka mails) increases...
	  echo "NEW MAIL!!!!" # Print a prompt to the user
	  numofmails=$(ls /var/mail/username | wc -l) # Then update the counter
  fi
done
```
**7. Create the following menu:**
- ***a. Press 1 to ls***
- ***b. Press 2 to ls –a***
- ***c. Press 3 to exit***
  - ***Note: Using select utility then while utility.***
```bash
touch mymenu.sh; chmod u+x mymenu.sh; nano mymenu.sh
```
```bash
#! /bin/bash
select choice in ls lsa exit
do
        case $choice in
                'ls') ls;;
                'lsa') ls -a;;
                'exit') exit;;
        esac
done
```
```bash
# # Test run:
# 1) ls
# 2) lsa
# 3) exit
# #? 1
# myarr.sh  myavg.sh  mycase.sh  mymenu.sh  mywhilemenu.sh
# #? 2
# .  ..  myarr.sh  myavg.sh  mycase.sh  mymenu.sh  mywhilemenu.sh
# #? 4
# #?
# 1) ls
# 2) lsa
# 3) exit
# #? 3
```
```bash
#! /bin/bash
while :
do
        read -p "Press 1 to ls, 2 to ls -a, 3 to exit: " r
        case $r in
                1) ls;;
                2) ls -a;;
                3) exit;;
        esac
done
```
```bash
# Test run:
# Press 1 to ls, 2 to ls -a, 3 to exit: 1
# myarr.sh  myavg.sh  mycase.sh  mymenu.sh  mywhilemenu.sh
# Press 1 to ls, 2 to ls -a, 3 to exit: 2
# .  ..  myarr.sh  myavg.sh  mycase.sh  mymenu.sh  mywhilemenu.sh
# Press 1 to ls, 2 to ls -a, 3 to exit: 4
# Press 1 to ls, 2 to ls -a, 3 to exit:
# Press 1 to ls, 2 to ls -a, 3 to exit: 3
```
**8. Write a script called myarr that ask a user how many elements he wants to enter in an array, fill the array and then print it.**
```bash
touch myarr.sh; chmod u+x myarr.sh; nano myarr.sh
```
```bash
#! /bin/bash
read -p "No. of elements to store in array: " n
declare -a myarr
for ((i=1; i<=n; i++))
do
	read -p "Please enter element $i: " e
	myarr+=($e)
done
echo ${myarr[*]}
```
```bash
# Test run:
# No. of elements to store in array: 5
# Please enter element 1: 1
# Please enter element 2: 4
# Please enter element 3: 6
# Please enter element 4: 2
# Please enter element 5: 9
# 1 4 6 2 9
```
**9. Write a script called myavg that calculate average of all numbers entered by a user using arrays.**
```bash
touch myavg.sh; chmod u+x myavg.sh; nano myavg.sh
```
```bash
#! /bin/bash
declare -a myarr
while :
do
        read -p "Please enter a number (to terminate, enter anything else): " i
        if [[ $i =~ [0-9] ]]; then myarr+=($i)
        else break
        fi
done
total=0
for i in "${myarr[@]}"
do
        total="$(($total+$i))"
done
echo "Average is $(($total/${#myarr[@]}))"
```
```bash
# Test run:
# Please enter a number (to terminate, enter anything else): 14
# Please enter a number (to terminate, enter anything else): 11
# Please enter a number (to terminate, enter anything else): 5
# Please enter a number (to terminate, enter anything else): 6
# Please enter a number (to terminate, enter anything else):
# Average is 9
```
