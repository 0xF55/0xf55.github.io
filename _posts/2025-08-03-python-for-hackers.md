---
layout: post
title: "Python For Offensive Security"
date: 2025-08-03
categories: [Intro]
---

Welcome to the Python For Offensive Security handbook.  
This is a personal reference and guide I’m putting together to organize all the Python stuff I’ve used in security and offensive tools.

---

## Requirements

1. Good experience in Python 
2. Networks Basics
3. Web Basics

---

## Table of Contents

### Basics

1. Interacting with system and files  
2. Strings and log parsing  
3. Multi-threading and multi-processing  
4. Basics for networks and web  
5. Small projects and utilities  

### Networks

1. Overview of common network attacks  
2. Useful Python libraries for network interaction  
3. Writing tools based on what’s explained  

### Web 

1. Explaining common web attacks  
2. Python libraries for web requests and parsing  
3. Practical tools based on real use-cases  

### Encryption & Hashing

1. Concepts behind encryption and hashing  
2. Libraries used in Python  
3. Simple projects like encrypting/decrypting files  

### Recon

1. Gathering IPs, DNS, Whois info  
2. Searching for usernames, phone numbers  
3. Using APIs like Shodan, Censys  
4. Writing own recon tools

### Malware Development

1. Loaders and droppers  
2. Shellcode execution  
3. Backdoors, stealers, and similar payloads  
4. Simple C2 and RAT  
5. Code obfuscation
6. Converting Python scripts to executables  
7. Basics of malware analysis  

### Real Projects

1. Building full tools based on what’s covered above  
2. Adding simple UIs like ASCII banners, colored output

---

## Starting

### System & Files 

- In this part we will use these three libraries.

```python
import sys,os,subprocess,shutil
```

[sys](https://docs.python.org/3/library/sys.html)

- Library allows you to interact with the Python interpreter we can use it to:
* handle command-line arguments
* managing i/o
* exit from the program

First we need to know what is command line arguments ?
its a value passed to the program when executing it:

```bash
python3 script.py val1 val2 val3
```
We can access these values using:
```python
script = sys.argv[0] # programe name
val1 = sys.argv[1] 
val2 = sys.argv[2]
```
sys.argv is a list that conatins all the arguments

- this is another usages

```python
sys.stdout.write("Hello World") # like print
sys.stderr.write("[!] Error ...") # print to stderr
sys.exit(0) # exit from the program
```

[subprocess](https://docs.python.org/3/library/subprocess.html)
- allows you to run external commands and programs from your Python code.

- Examples:

```python
cmd = 'powershell -ep bypass -c Write-Host "Hello World"' # Command to execute
cmd2 = 'python -m http.server 8080'
subprocess.run(cmd) # The Execution will stop until it ends
subprocess.Popen(cmd2,stdout=subprocess.DEVNULL) # Background Process
result = subprocess.check_output(cmd,text=True) # result = "Hello World"
```
[os](https://docs.python.org/3/library/os.html)

- provides a way to interact with the operating system.
- It allows you to perform common tasks like managing files and directories
- working with environment variables, and executing system commands

- Useful Examples:

```python
os_name = os.name # "nt" if windows "posix" if linux
currdir = os.getcwd() # Get Current Directory
files = os.listdir() # return list with files,dirs in current or specific dir
partitions = os.listdrives() # ['C:\\','D:\\'....]
env_vars = os.getenv("TEMP") # Env Variables C:\Users\...\AppData\Local\Temp
os.system("clear") # execute command
os.popen("any code") # background process
os.mkdir("NewDir") # make directory
os.chdir("Downloads") # change Directory
os.rename("old.txt","new.txt") # rename file
os.remove("file.txt") # remove file
os.rmdir("temporary") # remove empty directory
os.chmod("new.txt",777) # change file privilages
if os.path.exists("path"): pass # check if path exists
if os.path.isfile("path"): pass # check is file
if os.path.isdir("path"): pass # check if is directory
size = os.path.getsize("new.txt") # get file size (bytes)
full_path = os.path.join("C:\\Windows","System32\\calc.exe") # Merge two pathes
file_name = os.path.basename(full_path) # calc.exe
dir_path = os.path.dirname(full_path) # C:\Windows\System32
```

- That was the most used functions in os module

