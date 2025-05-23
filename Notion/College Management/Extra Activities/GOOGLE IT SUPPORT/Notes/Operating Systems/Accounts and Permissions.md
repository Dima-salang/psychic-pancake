  

  

- **User Types:**
    - Standard Users: Restricted access, can't install software or change key settings.
    - Administrators: Full control over the system, including adding/removing users and file access.
- **Administrator Roles:**
    - Personal Machine: Owner is the default administrator.
    - Public Computers: IT specialists serve as administrators, controlling access and settings.
- **Group-Based Access:**
    
    - Users grouped based on access levels.
    - Administrators define different permissions for user groups.
    
      
    

  

**Computer Management Tool Overview:**

- Computer Management is a crucial tool for managing Windows systems.
- It offers various system tools and settings for local machine management.
- In enterprise environments, it can be used to manage multiple machines in a domain.

**System Tools in Computer Management:**

1. Task Scheduler: Schedule tasks and programs to run automatically.
2. Event Viewer: Store and view system logs.
3. Shared Folders: Manage folders shared among users.
4. Local Users and Groups: Used for user and group management.
5. Performance: Monitor system resources (CPU, RAM).
6. Device Manager: Manage hardware devices.
7. Disk Management: Control storage devices.
8. Services and Applications: Enable/disable services and programs.

**User Account Management:**

- Access "Local Users and Groups" for user account management.
- Built-in Windows accounts like "guest" and "administrator" exist.
- Local administrator account, while powerful, is disabled by default.
- Options include password change requirements, account enable/disable, and lockout.
- UAC (User Access Control) allows administrative tasks without being logged in as the local administrator.

**User Group Information:**

- In the "Groups" section, view available groups and their members.
- This tool provides a GUI-based method for managing users and groups in Windows.

**Command-Line User and Group Management:**

- You can also perform user and group management tasks using the Windows Command Line Interface (CLI).

  

  

# Windows Accounts and Group Information CLI

**Efficient User Account Checking in Windows using CLI:**

- Task: Check the local administrator account status on 10 machines.
- Traditional method: GUI-based check, time-consuming.
- Faster approach: Utilize Command Line Interface (CLI).

**User Account Check:**

- Use the command `**Get-LocalUser**` to list all local users on a machine.
- Identify the local administrator account status, which is quicker than the GUI method.

**Group Check:**

- Employ `**Get-LocalGroup**` to list all local groups, including built-in groups.
- Pay attention to the "Administrators" group, which controls administrative access.

**Verifying Group Members:**

- To see who is part of the "Administrators" group, use `**Get-LocalGroupMember**` and specify the group name.

  

  

# **Linux User Management:**

- Similar to Windows, Linux has different user types with varying privileges.
- Linux has standard users, administrators, and the Root User.
- The Root User is automatically created during Linux OS installation and has superuser privileges.
- Root User is similar to Windows' local administrator but with even more power.

**Accessing Restricted Files:**

- In Linux, some files can only be read by the Root User.
- You can log in as Root but it's risky due to unrestricted access.
- To run a single command as Root, use `**sudo**` (superuser do) followed by the command.
- `**sudo**` is similar to Windows' User Access Control (UAC) feature.

**Switching to Root Temporarily:**

- You can use the `**su**` (substitute user) command to change to the Root User.
- Be cautious when logged in as Root to avoid accidental system changes.

**Checking sudo Access:**

- To view users with `**sudo**` access, examine the `**/etc/group**` file.
- Locate the group named "sudo" in the file.
- The file displays the group name, password (defaulted to root's password), group ID, and group members.

**Viewing Users:**

- To view user information, consult the `**/etc/passwd**` file.
- It contains detailed user data, including usernames, encrypted passwords, and user IDs (UIDs).
- Each line in the file represents a user, and the UID is used for identification.
- The Root User has a UID of zero.

  

  

# **Password Management in Windows:**

- Passwords enhance security for user accounts and machines, ensuring only authorized access.
- Passwords should be kept secret, even by administrators managing user accounts.

**Resetting Passwords in GUI:**

- Open Computer Management.
- Navigate to "Local Users and Groups."
- Right-click a username, go to "Properties."
- Check "User must change password at next login" to force a password change.

**Setting a Password Manually:**

- In the GUI, right-click a user and choose "Set password."
- Be cautious; this method can lead to losing access to certain credentials.

**Changing Passwords in PowerShell:**

- Use the DOS-style `**net user**` command in PowerShell.
- Syntax: `**net user <username> ***` (use an asterisk to enter the password securely).
- Avoid displaying passwords in logs; this method is more secure than plaintext entry.
- To force a user to change their password at the next login, use `**/logonpasswordchange;yes**` parameter with the `**net user**` command.

  

  

# **Changing Password in Linux:**

- To change a password in Linux, use the `**passwd**` command.
- Run the command, followed by your username, to change your own password.

**Password Security in Linux:**

- Passwords are securely hashed and stored in the `**/etc/shadow**` file.
- The `**/etc/shadow**` file can only be read by the Root User for security.
- Passwords stored in this file are not reversible or easily decrypted.

**Forcing Password Change:**

- To force a standard user to change their password, use the `**e**` or `**-expire**` flag with the `**passwd**` command.
- For example: `**passwd -e username**`.
- This immediately expires the user's password, requiring them to set a new password during their next login.

  

  

  

# **Adding and Removing User Accounts in Windows:**

**Adding a User Account (GUI):**

1. Open Computer Management.
2. Navigate to "Local Users and Groups."
3. Right-click, select "New User."
4. Set username, full name, and a default password.
5. Ensure the "User must change password at next logon" box is checked.
6. Click "Create" to create the user account.

**Removing a User Account (GUI):**

1. In Computer Management, right-click the user to be removed.
2. Select "Delete."
3. Confirm the action.
4. Note: Deleted usernames cannot be reused for old resources.

**Adding and Removing User Accounts (CLI):**

**Adding a User Account (CLI):**

- Use the `**net user**` command with the `**/add**` parameter.
- For example: `**net user username * /add**`.
- To require a password change at the next login, use `**/logonpasswordchange:yes**`.

**Removing a User Account (CLI):**

- To remove a user, use the `**net user**` command with the `**/delete**` parameter.
- For example: `**net user username /delete**`.
- Alternatively, you can use the `**Remove-LocalUser**` command in PowerShell.

  

  

  

# **Adding and Removing User Accounts in Linux:**

**Adding a User Account:**

- To add a new user in Linux, use the `**useradd**` command.
- For example: `**sudo useradd username**`.
- This command sets up basic configurations and creates a home directory for the user.

**Forcing Password Change at First Login:**

- You can combine the `**useradd**` command with the `**passwd**` command to make the user change their password on the first login.

**Removing a User Account:**

- To remove a user in Linux, use the `**userdel**` command.
- For example: `**sudo userdel username**`.
- This command removes the user account.

  

  

**Mobile Device User Accounts and Security:**

- Mobile operating systems handle user accounts differently because most mobile devices are used by a single person.
- In mobile devices like smartphones and tablets (iOS and Android), you enter a username and password during the initial setup, creating a primary account.
- The primary account is used to create a user profile on the device, containing your accounts, preferences, and apps.
- Primary accounts can synchronize settings and data to the cloud for backup and restoration on new devices.
- Mobile devices may allow you to sign in with additional accounts (e.g., email, social media) for single sign-on (SSO) in apps.
- IT support specialists help users set up additional accounts but should never ask for or handle user passwords.
- Encourage users to change their password if they share it with anyone.
- Most mobile devices are designed for a single user, but some Android devices support multiple user profiles.
- Mobile operating systems offer various security measures, such as device passwords, PINs, unlock patterns, and biometrics (e.g., fingerprint, facial recognition) for access control.
- Organizations may use Mobile Device Management (MDM) policies to enforce device security rules and configurations.

  

  

To change folder permissions in Windows:

1. Right-click the folder.
2. Go to Properties > Security tab > Edit.
3. Add the username (e.g., Devan).
4. Set the desired permissions (e.g., Modify for adding pictures).
5. Use the "Deny" option cautiously.
6. Confirm and apply permissions.

To use the Command Line Interface (CLI):

- In PowerShell:

```PowerShell

iCacls 'C:\vacation pictures' /grant 'Devan:OI CI R'

```

- In Command Prompt:

```Plain

iCacls "C:\vacation pictures" /grant Devan:OI CI R

```

Replace "Devan" with group names like "Everyone" or "Authenticated Users" if needed.

  

  

  

In Linux, you change permissions using the `**chmod**` command. Here's how:

1. **Select Permission Set**:
    - Owner (u), Group (g), or Other users (o).
2. **Add or Remove Permissions**:
    - Use a plus (+) to add permissions.
    - Use a minus (-) to remove permissions.

Examples:

- Add execute permission for the owner:
    
    ```Shell
    bashCopy code
    chmod u+x my_cool_file
    
    ```
    
- Remove execute permission for the owner:
    
    ```Shell
    bashCopy code
    chmod u-x my_cool_file
    
    ```
    
- Add multiple permissions for the owner (read and execute):
    
    ```Shell
    bashCopy code
    chmod u+rx my_cool_file
    
    ```
    
- Add read permissions for owner, group, and others:
    
    ```Shell
    bashCopy code
    chmod ugo+r my_cool_file
    
    ```
    

1. **Numeric Format (Faster Option)**:
    - Numeric values represent permissions:
        - 4 for read (r)
        - 2 for write (w)
        - 1 for execute (x)

Example:

- Set owner to read, write, and execute (7), group to read and execute (5), and others to read (4):
    
    ```Shell
    bashCopy code
    chmod 754 my_cool_file
    
    ```
    

1. **Changing Owner and Group**:
    - `**chown**` to change the owner of a file.
    - `**chgrp**` to change the group of a file.

Examples:

- Change owner to Devan:
    
    ```Shell
    bashCopy code
    chown Devan my_cool_file
    
    ```
    
- Change group to "Best Group Ever":
    
    ```Shell
    bashCopy code
    chgrp "Best Group Ever" my_cool_file
    ```
    
      
    

In Windows, permissions can be simple or special. Simple permissions are sets of special permissions. When you set a simple permission like "Read," you're actually enabling multiple special permissions such as "List Folder/Read Data," "Read Attributes," "Read Extended Attributes," "Read Permissions," and "Synchronize."

To view special permissions on a file or folder in the Command Line Interface (CLI) using the `**icacls**` command:

1. Use the command: `**icacls path_to_file_or_folder**`.  
    Example:  
    `**icacls C:\Windows\Temp**`.
2. Review the output to see the list of special permissions assigned.

Special permissions include detailed control over file and folder actions. For example, you can grant specific permissions like "Create Files" and "Write Data" without allowing users to delete each other's files. Special permissions can also be inherited from parent folders.

In Windows, you can use special permissions and NTFS security descriptors to create customized, powerful permission sets tailored to your specific needs.

  

In Linux, special permissions like setuid, setgid, and sticky bits can be useful for specific use cases:

1. **Setuid (S)**: This special permission allows an executable file to be run with the permissions of the owner of the file, typically as root. It's useful when you want regular users to perform actions that require root privileges without giving them full root access. To enable the setuid bit, you can use symbolic notation (e.g., `**chmod u+s filename**`) or numeric notation (e.g., `**chmod 4755 filename**`).
2. **Setgid (G)**: Setgid is similar to setuid but works with group permissions. It allows an executable to be run with the permissions of the group that owns the file. To enable the setgid bit, you can use symbolic notation (e.g., `**chmod g+s filename**`) or numeric notation (e.g., `**chmod 2755 filename**`).
3. **Sticky Bit (T)**: The sticky bit is commonly used on directories, such as the `**/tmp**` directory. It allows anyone to create, modify, or add files to the directory but restricts the deletion of files to only the owner of the file or the root user. To enable the sticky bit, you can use symbolic notation (e.g., `**chmod +t directory**`) or numeric notation (e.g., `**chmod 1755 directory**`).