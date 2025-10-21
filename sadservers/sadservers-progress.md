# SadServers progress

Documenting my progress through the SadServers challenges. I will list the commands used and some of my thoughts on how I worked through each level to find the password.

## Level 1 - "Saint John": What is writing to this log file?
- **Goal:**  A developer created a testing program that is continuously writing to a log file /var/log/bad.log and filling up disk. You can check for example with tail -f /var/log/bad.log.
This program is no longer needed. Find it and terminate it. Do not delete the log file.
- **Commands used:** 
    - `tail -f`
    - `lsof`
- **Notes:** The log file is constantly being updates so we can see the entries by using `tail -f /var/log/bad.log`. We then use `lsof /var/log/bad.log` which will list all processes currently using a file. The output will give us a PID (process ID) of the process writing to the bad.log file. We can then terminate the process using `kill -9 <PID>` which will forcefully stop the process. 

## Level 2 - "Saskatoon": Counting IPs.
- **Goal:** There's a web server access log file at /home/admin/access.log. The file consists of one line per HTTP request, with the requester's IP address at the beginning of each line.
- **Commands used:** 
    - `awk {print $1}`
    - `sort uniq -c`
    - `sort -nr`
    - `head -1`
- **Notes:** WWE can use `awk {print $1} /home/admin/access.log` to print the IP address which is on the first field of each line. To count how many times each IP occurs, we can do `awk {print $1} /home/admin/access.log | sort | uniq -c` which will show each IP alongside the number of requests it has made. Then to sort them numerically and take the first result, we can add `sort -nr | head -1`. Finally, we need to write the IP address to the given file **home/admin/highestip.txt** -> `<IP ADDRESS> > /home/admin/highestip.txt`.

## Level 3 - "The Command Line Murders"
- **Goal:** This is the Command Line Murders with a small twist as in the solution is different.
Enter the name of the murderer in the file /home/admin/mysolution, for example echo "John Smith" > ~/mysolution
- **Commands used:**
    - `grep`
    - `head -n | tail -n`
    - `grep -C4 "error" filename.txt`
- **Notes:** We need to extract all the clues from the files which will help us solve this challenge; `grep "clue" crimescene`. Once we read the clues, we can look for a female called Annabel in the people file; `cat people | grep "Annabel"`. We see 2 females called Annabel so we have to search their address in the streets file and get the specific line we want; `cat streets/address | head -n 40 | tail -n 1`. Both outputs give us an interview number which we can look into both interviews in the interviews folder; `cat interviews/interview-******`. Only 1 interview give us the next clue we need which decribes a car that fled the crime scene. We have a vehicles file which can use to find the specific car we are looking for and we can also use `-C4` to include 4 lines of context about and below a match to give us more information; `cat vehicles | grep -C4 "Honda" | grep C-4 "Blue"`. Once we have filtered all the Blue Hondas, we can then look for the remaining clues to find the correct vehicle - 'Licence plate starts with L337 and ends with 9' + 'Person is a tall male, at least 6 foot'. We have a few suspects who fit this description: Jacqui Maher, Jeremy Bowers, and Joe Germuska. Another clue we had was the suspect has 4 specific membership cards. To list the membership cards for each of our suspects, we can do; `grep "suspect_name" memberships/*`. This command will search every file inside the memberships directory for the matching suspect. The only suspect who matches all 4 of the membership cards is Joe Germuska. Next we have to enter the murderer in the file /home/admin/mysolution; `echo "Joe Germuska" > ~/mysolution`. To test the solution, we can run `md5sum ~/mysolution` which will return `9bba101c7369f49ca890ea96aa242dd5` if it is correct.

## Level  - ""
- **Goal:** 
- **Commands used:**
- **Notes:** 