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
**`uniq -c <file>`** – Count unique occurrences  
**`sort -nr <file>`** – Sort numerically (descending)  
**`head -1 <file>`** – Show top entry  

---

## File Permissions & Ownership

**`ls -l`** – Show permissions and ownership  
**`chmod 750 <file>`** – Numeric permissions  
**`chmod u+x <file>`** – Add execute permission for user  
**`chmod g+r <file>`** – Add read permission for group  
**`chmod o-w <file>`** – Remove write permission for others  
**`chown <user> <file>`** – Change file owner  
**`chgrp <group> <file>`** – Change file group  
**`chown <user>:<group> <file>`** – Change owner & group  
**`sudo <command>`** – Run a command as root  

**Permissions Reference:**  
- `r = 4`, `w = 2`, `x = 1`  
- Categories: **User** (owner), **Group**, **Others**  

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

**`<`** – Input from file  
**`>`** – Output to file (overwrite)  
**`>>`** – Append output to file  
**`2>`** – Redirect errors  
**`&>`** – Redirect both output & errors  

Examples:  
```bash
ls /noexist > out.txt 2> error.txt   # Separate stdout & stderr
ls /noexist &> all.txt               # Redirect both stdout & stderr
```

## Netorking & Archives

**`nc <host> <port>`** – Open TCP connection
**`openssl s_client -connect <host>:<port>`** – TLS/SSL connection
**`nmap -p31000-32000 localhost`** – Scan ports
**`tar -xf archive.tar`** – Extract tar archive
**`gzip -d file.gz`** – Extract gzip archive
**`bzip2 -d file.bz2`** – Extract bzip2 archive