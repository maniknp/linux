To give `sftpuser` permission to edit files owned by `ubuntu` and `www-data` users, you'll need to carefully manage user permissions and groups. Here’s how you can do this:

### Step 1: Add `sftpuser` to the Appropriate Groups

1. Add `sftpuser` to the `ubuntu` and `www-data` groups.

```bash
# Add sftpuser to the ubuntu group
sudo usermod -aG ubuntu sftpuser

# Add sftpuser to the www-data group
sudo usermod -aG www-data sftpuser
```

### Step 2: Set Group Permissions on Directories

Ensure the directories and files you want `sftpuser` to edit have the correct group permissions.

1. Set the group ownership of the directories to `ubuntu` or `www-data`.

```bash
# Example: Change the group ownership of a directory to ubuntu
sudo chgrp -R ubuntu /path/to/ubuntu/directory

# Example: Change the group ownership of a directory to www-data
sudo chgrp -R www-data /path/to/www-data/directory
```

2. Set the correct permissions to allow group members to read and write.

```bash
# Example: Set group read/write permissions for ubuntu directory
sudo chmod -R 775 /path/to/ubuntu/directory

# Example: Set group read/write permissions for www-data directory
sudo chmod -R 775 /path/to/www-data/directory
```

### Step 3: Verify `sftpuser` Permissions

Ensure `sftpuser` has the correct permissions by listing the groups and checking access.

1. List the groups `sftpuser` belongs to.

```bash
# List groups for sftpuser
groups sftpuser
```

2. Verify `sftpuser` can access and edit files in the directories.

```bash
# Switch to sftpuser
sudo su - sftpuser

# Try accessing and editing files
cd /path/to/ubuntu/directory
nano somefile.txt

cd /path/to/www-data/directory
nano anotherfile.txt
```

### Step 4: Configure SFTP Server (Optional)
Ensure your SFTP server configuration allows `sftpuser` to access the required directories.

1. Edit the SSH configuration file.

```bash
sudo nano /etc/ssh/sshd_config
```

2. Add or modify the SFTP configuration for `sftpuser`.

```bash
# Add this section if not already present
Match User sftpuser
    ChrootDirectory /home/sftpuser
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no

# Allow sftpuser to access specific directories
    # Example: Allow access to /path/to/ubuntu and /path/to/www-data
    PermitTunnel yes
    AllowAgentForwarding yes
    AllowTcpForwarding yes
    X11Forwarding yes
    Subsystem sftp internal-sftp
```

3. Restart the SSH service to apply changes.

```bash
sudo systemctl restart ssh
```

### Additional Notes
1. **Security Considerations**: Granting `sftpuser` write access to directories owned by `ubuntu` and `www-data` can pose security risks. Ensure this is necessary and consider the implications.
2. **File Ownership and Permissions**: If `sftpuser` creates new files, they may be owned by `sftpuser`. You may need to set up cron jobs or scripts to change ownership if necessary.
3. **Fine-Grained Permissions**: For more fine-grained control, consider using ACLs (Access Control Lists).

By following these steps, `sftpuser` should have the necessary permissions to edit files owned by `ubuntu` and `www-data`.