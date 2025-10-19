# Linux Commands Cheat Sheet

---

## Navigation & File Management

**`pwd`** – Show current working directory  
**`ls`** – List files in the directory  
**`ls -a`** – List all files including hidden  
**`ls -l`** – Detailed listing with permissions and ownership

**`cd <dir>`** – Change directory  
**`cd ..`** – Move up one directory  

**`mkdir <dir>`** – Create a directory  
**`mkdir -p dir/subdir`** – Create nested directories  
**`rmdir <dir>`** – Remove empty directory  
**`rm -r <dir>`** – Remove directory recursively  
**`touch <file>`** – Create a file  
**`rm <file>`** – Delete a file  

**`cp <source> <dest>`** – Copy file  
**`cp -r <source_dir> <dest_dir>`** – Copy directory recursively  
**`mv <old> <new>`** – Move/rename file or directory  

---

## File Content & Search

**`cat <file>`** – Display file content  
**`cat file1 file2 > combined.txt`** – Merge files into a new file  

**`head <file>`** – View first 10 lines  
**`head -n 5 <file>`** – View first 5 lines  
**`tail <file>`** – View last 10 lines  
**`tail -n 5 <file>`** – View last 5 lines  

**`less <file>`** – View large files page by page  

**`grep "pattern" <file>`** – Search for text  
**`grep "^txt" <file>`** – Lines starting with “txt”  
**`grep "txt$" <file>`** – Lines ending with “txt”  

**`awk '{print $1}' <file>`** – Extract first column  
**`awk '{print $2}' <file>`** – Extract second column  

**`sort <file>`** – Sort entries  
**`sort -nr <file>`** – Sort numerically (descending)  
**`uniq -c <file>`** – Count unique occurrences  
**`head -1 <file>`** – Show top entry  

---

## File Permissions & Ownership

**Binary & Octal permissions**  
- Each permission is represented by a number:  
  - **r = 4**, **w = 2**, **x = 1**  
- Add them up for each category (User, Group, Others):  
  - **`rwx = 7`**, **`rw- = 6`**, **`r-- = 4`**  
- Example:  
  - **`chmod 750 file`** → user (7 = rwx), group (5 = r-x), others (0 = ---)

**`chmod u+x <file>`** – Add execute permission for user  
**`chmod g+r <file>`** – Add read permission for group  
**`chmod o-w <file>`** – Remove write permission for others  
**`chmod ug=rw,o=r <file>`** – Performing multiple permission operations at once (user + group = read + write, others = read)

**`chown <user> <file>`** – Change file owner  
**`chgrp <group> <file>`** – Change file group  
**`chown <user>:<group> <file>`** – Change owner & group  

**`sudo <command>`** – Run a command as root if permission is denied
**`sudo adduser <username>`** – Creates user & home directory
**`sudo passwd <username>`** – Set password
**`su <username>`** – Switch user
**`sudo usermod -aG sudo <username>`** – Grant sudo to new user (adds them to group)
**`sudo deluser <username> sudo`** – Revoke sudo from user

**`sudo groupadd <groupname>`** – Create new group
**`sudo groupdel <groupname>`** – Delete group
**`groups`** – Check user groups (whilst logged in as user)
**`sudo usermod -aG <groupname> <username>`** – Add user to group
**`sudo gpasswd -d <username> <groupname>`** – Remove user from group
**`sudo usermod -aG <group1>,<group2> <username>`** – Adds user to multiple groups

**`sudo chown newuser <file>`** – Change owner of file
**`sudo chgrp admin <file>`** – Change group
**`chown user:group <file>`** – Change both at once

---

## Vim Editor

**Modes:**  
- `Esc` – Command mode  
- `i` – Insert mode  
- `v` – Visual mode  

**Commands:**  
- `:w` – Save  
- `:q` – Quit  
- `:wq` – Save & quit  
- `dd` – Delete line  
- `yy` – Copy line  
- `p` – Paste  
- `/word` – Search word  

---

## Standard Streams & Redirection

### Redirection Basics

| Symbol | Meaning                        | Example                     | Description                                 |
|--------|--------------------------------|-----------------------------|---------------------------------------------|
| `<`    | Input from file                | `cat < input.txt`           | Takes input from a file instead of keyboard |
| `>`    | Output to file                 | `echo "Hi" > file.txt`      | Overwrites file with output                 |
| `>>`   | Append output                  | `echo "More" >> file.txt`   | Adds output to end of file                  |
| `2>`   | Redirect errors                | `ls /noexist 2> errors.txt` | Saves errors separately                     |
| `&>`   | Redirect all (stdout + stderr) | `ls /noexist &> all.txt`    | Saves both normal and error output          |


### Discarding Output

**`ls /noexist > /dev/null 2>&1`**
- **`/dev/null`** → the “black hole” for unwanted output
- **`2>&1`** → send errors (2) to the same place as output (1)

---

## Environment Variables

- **`env/printenv`** – View all variables
- **`echo $PATH`** – Check one variable
- **`NAME="Ibrahim"`** – Set temporary variable
- **`export NAME="Ibrahim"`** – Export variable
- **`~/.bashrc or ~/.zshrc and run source ~/.bashrc`** – Permanent variable

**Common variables:**
| Variable | Description                        |
|----------|----------------------------------- |
| `PATH`   | Directories searched for commands |
| `HOME`   | User's home directory              |
| `USER`   | Logged in username                 |
| `PWD`    | Present working directory          |
| `SHELL`  | Default shell                      |

---

## Advanced / Helpful commands

# File reading & Inspection

- **`./-`** – Read a file with special characters in current directory
- **`cat "./--spaces in this directory--"`** – Read file with spaces
- **`file ./*`** – Show file types in current directory
- **`file */{,.}*`** – Include hidden files
- **`strings <file?`** – Extract human-readable strings from binaries
- **`xxd <file>`** – View file in hex
- **`xxd -r <file>`** – Convert hex back to binary

# Searching & Filtering

- **`find / -type f`** – All files
- **`find -size 33c`** – Files of 33 bytes
- **`find . -size 1033 ! -executable`** – Non-executable 1033 byte files
- **`find -user <user> -group <group>`** – Match owner and group
- **`grep ASCII`** – Only human-readable files
- **`grep -v "with very long lines"`** – Exclude lines
- **`cat <file> | grep "millionth"`** – Search for text

# Sorting & Counting
- **`du -b -a | grep 1033`** – All files in bytes
- **`sort <file> | uniq -u`** – Unique lines only 
- **`wc -l <file>`** – Count lines

# Encoding & Decoding
- **`cat <file> | base64 --decode`** – Decode Base64
- **`cat <file> | tr 'A-Za-z' 'N-ZA-Mn-za-m'`** – ROT13
- **``** – 
- **``** – 

# Networking

- **`nc <host> <port>`** – Open TCP connection
- **`openssl s_client -connect <host>:<port>`** – TLS/SSL connection
- **`openssl s_client -connect <host>:<port> -quiet`** – Hide extra info
- **`nmap -p31000-32000 localhost`** – Scan port range
- **`nmap -sV localhost -p31000-32000`** – Detect service/version

# Archives

| Format  | Magic            | Extension | Decompress |
|-------- |----------------- |-----------|------------|
| `Gzip`  | `1F 8B 08`       | `.gz`     | `gzip -d`  |
| `Bzip2` | `425A 68 (BZh)`  | `.bz2`    | `bzip2 -d` |
| `Tar`   | contains `ustar` | `.tar`    | `tar -xf`  |

- `tar -xvf <file.tar>` – Extract archive

---