## Directory and File Operations
<img width="945" height="536" alt="image" src="https://github.com/user-attachments/assets/624f99e0-5b32-4d76-8c29-cc5a4e25042e" />

## Reflection: File System Commands
### What command did you use to create a directory?
mkdir (Make Directory).

How can you view file content without a GUI editor?
Using the cat command (which prints the entire file to the terminal), or less (which lets you scroll through a long file).

What is the difference between cp and mv?
cp (copy) duplicates a file, leaving the original intact. mv (move) relocates the file, effectively removing it from the original location (or renaming it).

## Basic Bash Script
<img width="957" height="258" alt="image" src="https://github.com/user-attachments/assets/6f269e61-6489-4498-921e-7f06ad6d1b6f" />

## Reflection: Script Basics
What is chmod +x for?
It adds "execute" permission, telling Linux that the file is a program and is safe to run.

Why is #!/bin/bash used?
This is called the "shebang". It tells the system which interpreter (program) to use to run the script—in this case, the Bash shell.

How can you personalize script output?
By using variables (e.g., USERNAME="Benjamin" and then echo "Hello $USERNAME"), or by using command substitution (echo "I am $(whoami)").
