# Bandit Progress

Documenting my progress through the OverTheWire Bandit game. I will list the commands used and some of my thoughts on how I worked through each level to find the password.

## Level 1
- **Goal:** find password in file `-`.
- **Commands used:** 
    -`ls`
    -`ls -l`
    -`cat ./-`
- **Notes:** File is called `-`, so cat - would be interprated as a special argument meaning stdin or stdout. We have to do `cat ./-` so it access the file literally named `-`.

## Level 2
- **Goal:** password located in a file called `--spaces in this filename--`.
- **Commands used:** 
    -`cat "./--spaces in this filename--"`
- **Notes:** Using "" or '' to read file names with spaces.

## Level 3
- **Goal:** password stored in a hidden file in the inhere dir.
- **Commands used:** 
    -`ls -a`
- **Notes:** ls-a will list all directories including hidden.

## Level 4
- **Goal:** stored in the only human-readable file in the inhere dir.
- **Commands used:** 
    -`ls -la`
    -`file ./*`
- **Notes:** once in the inhere directory, `file ./*` showed the file types for all the files in the directory which helped find the only ASCII text file.

## Level 5
- **Goal:** password stored in a file which has 3 properties: human-readable, 1033c, non-executable.
- **Commands used:** 
    -`ls -la`
    -`find . -size 1033c ! -executable`
- **Notes:** the commands used helped find files which were 1033c size and not executable. I tried to use `grep ASCII` but it was taking too long, so using -size and ! -executable narrowed it down to just 1 file which was file2 in the maybehere07 directory.

## Level 6
- **Goal:** password is stored somewhere in the server and has has 3 properties: owned by user bandit7, owned by group bandit6, 33c size.
- **Commands used:** 
    -`find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null`
- **Notes:** `find / -type f` helps find certain files and then we can specify which user and group we are looking for. `2>/dev/null` we run into a lot of error messages so using this will hide them and we can find the exact file we are looking for.

## Level 7
- **Goal:** password stored in data.txt next to the word "millionth".
- **Commands used:** 
    -`cat data.txt | grep "millionth"`
- **Notes:** pretty straight forward. Find the word that matches 'millionth' in the data.txt file. Alternatively, we can do `grep "millionth" data.txt`.

## Level 8
- **Goal:** password stored in data.txt and is the only line of text that occurs once.
- **Commands used:** 
    `sort data.txt | uniq -u`
- **Notes:** `sort` will sort the lines so the duplicates are next to each other. `uniq -u` will only print the line that occurs once.

## Level 9
- **Goal:** password stored in data.txt in one of the few human-readable string preceded by several = characters.
- **Commands used:** 
    -`strings data.txt | grep -E '=+'`
- **Notes:** `strings data.txt` will extract the human-readable ASCII. `grep -E '=+'` highlight the character that have = in them. 

## Level 10
- **Goal:** password stored in data.txt which contains base64 encoded data.
- **Commands used:** 
    -`cat data.txt | base64 --decode`
- **Notes:** Fairly straightforward. I will read the file by decoding the base64 encoded data with original content.

## Level 11
- **Goal:** stored in data.txt where all the lowercase and uppercase letter have been rotated by 13 positions.
- **Commands used:** 
    -`cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`
- **Notes:** This helped rotate the letters again by 13 positions to return it back to the original position. Alternatively, we can use the rot13.com website to just copy the file contents.

## Level 12
- **Goal:** stored in data.txt which is a hexdump of a file that has been repeatedly compressed.
- **Commands used:** 
    -`mktemp -d`
    -`mv`
    -`cp`
    -`xxd -r [file.hex] > file.gz`
    -`gzip -d [file.gz]`
    -`bzip2 -d [file.bz]`
    -`tar -xf [file.tar]`
- **Notes:** The hardest challenge yet so I followed an online solution online. I was struggling to identify which files are gzip, bzip2 or tar files. However, now I can identify what type of file it is by doing `file *` which will show me the file type if it is gzip, bzip2 or tar. I was then decompressing the file repeatedly until I finally got a readable file which contains the password.

## Level 13
- **Goal:** password is stored in a file that can only be read by user bandit14. We get a private SSH key that can be used to log into the next level.
- **Commands used:** 
    -`chmod 600 sshkey.private`
    -`ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org`
- **Notes:** Another tricky challenge. I had to exit bandit13 and run the SSH command in my own machine. First I was warned the file was too opened so I did `chmod 600 sshkey.private` so SSH accepts it. `-i` tells SSH which key to use, and `-p 2220` tells it which port to connect to.

## Level 14
- **Goal:** password can be retrieved by submitting the password of current level to port 30000 on localhost.
- **Commands used:** 
    -`nc localhost 30000`
- **Notes:** using this command opens a raw TCP connection to port 30000 on localhost. It asks for an input so we enter the bandit14 password and the output we recieve is the password for the next level.

## Level 15
- **Goal:** password can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
- **Commands used:** 
    -`openssl s_client -connect localhost 30001`
- **Notes:** `openssl s_client` will open an encrypted (SSL/TLS) connection to the server, we then specify which host using `-connect localhost 30001`. Again, it asks for an input which we enter the current level password, we then retrieve the next level password.

## Level 16
- **Goal:** credentials for next level can be retrieved by submitting the current level password to a port on localhost in the range of 31000-32000. First need to find out which of these ports have a server listening on them, then find out which of those speak SSL/TLS, and which of those don't. Only 1 server will give the next credentials.
- **Commands used:** 
    -`nmap -sV localhost -p 31000-32000`
    -`scp -P 2220 bandit16@bandit.labs.overthewire.org`
    -`chmod 600 rsafile`
- **Notes:** I did `nmap -sV` to scan ports and services on a host. After returning 2 ports, I opened an encrypted SSL connection to each server, only 31790 returned a RSA privatekey. I saved the key in a file called rsafile in bandit16 but exited, then copied the key to my local machine using `scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/game99/rsafile ./rsafile`. I then secured the key using `chmod 600 rsafile`, then SSH'ed into bandit17 with the rsafile key.

## Level 17
- **Goal:** password is the only line that has changed between 2 files; password.old & password.new
- **Commands used:** 
    -`diff [oldfile] [newfile]`
- **Notes:** I used `diff` to compare the 2 files and the output was `< -output from first file` ----- `> output from second file`. We know the 2nd line with `>` is the only line that has been changed so that is the password.

## Level 18
- **Goal:** password stored in a readme fle but someone has modified `.bashrc` which logs us out when we log in with SSH
- **Commands used:** 
    -`ls -la`
    - `cat`
- **Notes:** We automatically get logged out when we enter Bandit18 using SSH, so instead we can run a command that runs directly on login instead of opening a shell. `ssh bandit18@bandit.overthewire.org -p 2220 ls -l` lets us see the files in the home directory, which we see there is a readme file. Then replacing the command to `cat readme` will read the contents of the readme file which we get the password.

## Level 19
- **Goal:** for the next level, we should use the setuid binary in the home directory. password can be found in /etc/bandit_pass after using the setuid binary
- **Commands used:**
    -`ls -l`
    -`ls -la`
    -`./bandit20-do`
    -`./bandit20-do [command]`
- **Notes:** when we list the files we see `-rwsr-x--- | bandit20-do`. The `s` tells us it is a **setuid binary** so it runs with the owner's priviliges (bandit20), not the runners (bandit19). We can run commands or read files as bandit20 by using `./bandit20-do [command]` which will give us access. Without it, permissions stay as bandit19.

## Level 20
- **Goal:** there is a setuid binary in the home directory that will make a connection to localhost on the specified port as an commandline argument. It will then read a line of text from the connection and compare it to the password in the prev level (bandit20). If password is correct, it will transmit the password for the next level.
- **Commands used:**
    -`ls -l`
    -`./suconnect [port]`
    -`nc -lvp [new port]`
- **Notes:** For this level, we need to interact with network connections using `nc` to communicate with a program. We open a listener using a high port to avoid privileged port issues: `nc -lvp 3333` and then on a new terminal, we log into bandit20. There is a binary file called **suconnect**, so again we can run `./suconnect 3333` which will connect to the port and we can enter the password for bandit20 via the listener terminal (first terminal). We will then get the password for the next level.
