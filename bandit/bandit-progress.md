# Bandit Progress

Documenting levels 1 → 20.

## Level 1
- **Goal:** find password in file `-`
- **Commands used:** `ls, ls -l, cat -, cat ./-`
- **Notes:** File is called `-`, so cat - would be interprated as a special argument meaning stdin or stdout. We have to do `cat ./-` so it access the file literally named `-`

## Level 2
- **Goal:** password located in a file called `--spaces in this filename--`
- **Commands used:** `cat "./--spaces in this filename--"`
- **Notes:** Using apostrophes to read file names with spaces

## Level 3
- **Goal:** password stored in a hidden file in the inhere dir
- **Commands used:** `ls -a`
- **Notes:** ls-a will list all directories including hidden

## Level 4
- **Goal:** stored in the only human-readable file in the inhere dir
- **Commands used:** `ls -la, file ./*`
- **Notes:** once in the inhere directory, `file ./*` showed the file types for all the files in the directory which helped find the only ASCII text file

## Level 5
- **Goal:** password stored in a file which has 3 properties: human-readable, 1033c, non-executable
- **Commands used:** `ls -la, find -size 1033c ! -executable`
- **Notes:** the commands used helped find files which were 1033c size and not executable. I tried to use `grep ASCII` but it was taking too long, so using -size and ! -executable narrowed it down to just 1 file which was file2 in the maybehere07 directory

## Level 6
- **Goal:** password is stored somewhere in the server and has has 3 properties: owened by user bandit7, owned by group bandit6, 33c size
- **Commands used:** `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null`
- **Notes:** `find / -type f` helps find certain files and then we can specify which user and group we are looking for. `2>/dev/null` we run into a lot of error messages so using this will hide them and we can find the exact file we are looking for.

## Level 7
- **Goal:** password stored in data.txt next to the word "millionth"
- **Commands used:** `cat data.tx | grep "millionth"`
- **Notes:** pretty straight forward. 

## Level 8
- **Goal:** password stored in data.txt and is the only line of text that occurs once
- **Commands used:** `sort data.txt | uniq -u`
- **Notes:** `sort` will sort the lines so the duplicates are next to each other. `uniq -u` will only print the line that occurs once.

## Level 9
- **Goal:** password stored in data.txt in one of the few human-readable string preceded by several = characters
- **Commands used:** `strings data.txt | grep -E '=+'`
- **Notes:** `strings data.txt` will extract the human-readable ASCII. `grep -E '=+'` highlight the chatacter that have = in them. 

## Level 10
- **Goal:** password stored in data.txt which contains base64 encoded data
- **Commands used:** `cat data.txt | base64 --decode`
- **Notes:** Fairly straightforward. I will read the file byt decode the base64 encoded data with original content

## Level 11
- **Goal:** stored in data.txt where all the lowercase and uppercase letter have been rotated by 13 positions
- **Commands used:** `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`
- **Notes:** This helped rotate the letters again by 13 positions to return it back to the original position. Alternatively, we can use the rot13.com website to just copy the file contents.

## Level 12
- **Goal:** stored in data.txt which is a hexdump of a file that has been repeatedly compressed.
- **Commands used:** `mktemp -d, mv, cp, xxd -r, gzip -d, bzip2 -d, tar -xf`
- **Notes:** Okay this was the definately hardest challenge yet. I tried using online resources and even used ChatGPT as a guide, but was making little to no progress. I decided to follow along a solution online and try to understand file compression. Thing I struggle with most is identifying which files are gzip, bzip2 or tar files. However, now I can identify what type of file it is by doing `file *` which will shwow me the file type if it is gzip, bzip2 or tar. 

## Level 13
- **Goal:**
- **Commands used:**
- **Notes:**

## Level 14
- **Goal:**
- **Commands used:**
- **Notes:**

