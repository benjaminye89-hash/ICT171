## Creating Users and Group
<img width="962" height="972" alt="image" src="https://github.com/user-attachments/assets/f2c8997c-a137-4585-840d-7ecbc39351a0" />

### Alice and Bob added to shared Group
<img width="1068" height="121" alt="image" src="https://github.com/user-attachments/assets/eeee70f7-711b-4221-934e-96fe8be5241c" />

## Setting up the Shared Directory

### Create the directory
<img width="960" height="126" alt="image" src="https://github.com/user-attachments/assets/9d1f811f-9dbf-4b59-aece-21ae91fda04d" />

### Change the group ownership of the directory to sharedgroup
<img width="960" height="126" alt="image" src="https://github.com/user-attachments/assets/e482a7e8-8187-4788-bf5a-ce5d8d9f7709" />

### Ten files created
<img width="960" height="126" alt="image" src="https://github.com/user-attachments/assets/4a225671-5f1f-47b8-bdec-afcabb7fbcca" />
<img width="948" height="381" alt="image" src="https://github.com/user-attachments/assets/d83c7b27-d1e7-4829-a1ef-3cc2a549bc80" />

## Assigning Permissions
### se of Recursive Flags (-R)
We will do this right now to set the main folder permissions.
<img width="966" height="93" alt="image" src="https://github.com/user-attachments/assets/74347812-eb1d-49bf-a1b2-d0a8d6ac73de" />

### Testing Access as Each User
<img width="952" height="433" alt="image" src="https://github.com/user-attachments/assets/fc126867-c858-43d9-b321-475f00c7224f" />
<img width="807" height="432" alt="image" src="https://github.com/user-attachments/assets/cb022b7e-6890-47b1-9ea0-ff2c3ae25728" />
<img width="945" height="77" alt="image" src="https://github.com/user-attachments/assets/79941d78-2154-466f-bbba-b84e4ec5dc98" />

## Challenge Task - Sudo Access
### Now we are going to make Mallory into a superuser.
<img width="963" height="210" alt="image" src="https://github.com/user-attachments/assets/c1103d71-9806-4ffd-a433-5525a501bd90" />

#Clean Up

<img width="921" height="58" alt="image" src="https://github.com/user-attachments/assets/16197fdc-fead-4d8a-8eb1-ee38d77fc3fb" />

# Reflection
1. Why group permissions are more scalable than user-by-user permissions
In Linux, managing permissions on a user-by-user basis is highly inefficient. If a company has 100 employees and 20 specific users need access to a project folder, an administrator would have to manually assign permissions to those 20 individual users. If an employee leaves the company, the admin must remember to revoke their specific access across multiple directories.

Group permissions solve this scalability issue. Administrators can create a logical group (e.g., sharedgroup), assign permissions to the group once, and simply add or remove users from that group (using usermod -aG). This allows for centralized access management, reducing human error and significantly streamlining system administration in multi-user environments.

2. The security implications of adding a user to the sudo group
Granting a user access to the sudo group (which we did for the user mallory) is effectively giving them administrative "superuser" powers. While this is necessary for performing system updates and installing software, it comes with major security risks.
A user in the sudo group can alter critical system files, delete entire directories (as demonstrated when we ran sudo rm -r /home/shared), and access restricted directories (such as /root). If a user account with sudo privileges is compromised by a hacker, that hacker immediately gains full control over the entire server. Because of this, the principle of "least privilege" should always be applied—users should only be given the bare minimum permissions necessary to do their job.

3. Difference between symbolic (chmod u+rwx) and numeric (chmod 750) permission settings
Linux allows permissions to be modified using either symbolic or numeric (octal) modes.
Symbolic mode (e.g., chmod u+rwx) is more readable and intuitive for beginners. It allows you to modify permissions for a specific entity: user (u), group (g), or others (o). For example, u+rwx explicitly says "add read, write, and execute permissions to the file owner."
Numeric mode (e.g., chmod 750) is more compact and commonly used by experienced administrators. It uses a base-8 system where the permissions are calculated as a sum: Read (r = 4), Write (w = 2), and Execute (x = 1). Therefore, 7 (4+2+1) means Read-Write-Execute, 5 (4+0+1) means Read-Execute, and 0 means no permissions. Using chmod 750 is often faster for bulk operations and ensures the exact permission mask is applied.

4. What I learned from verifying with su and whoami
One of the most valuable lessons in this lab was the importance of verifying permissions by actually switching users. Running ls -l as the root user shows the theoretical permissions set on a file, but it does not guarantee that another user can actually interact with it. By using su - alice and whoami, I was able to physically step into the shoes of a standard user and test the file access in real-time. Watching bob get a "Permission denied" error when trying to write to a file, despite being in the group, highlighted exactly how the permissions system behaves in practice. This verification step is vital for security audits to ensure policies are actually enforced.







