### Step 1: Check if SFTP and Dependencies are Installed
First, we need to ensure that OpenSSH is installed, as it provides the SFTP server.

```bash
# Update package list
sudo apt update

# Install OpenSSH server if not already installed
sudo apt install openssh-server

# Check if OpenSSH server is running
sudo systemctl status ssh
```

### Step 2: Create a New User
Create a new user for SFTP access.

```bash
# Replace 'sftpuser' with your desired username
sudo adduser sftpuser
```

### Step 3: Restrict User to Specific Directory
1. Create the directory the user will have access to.

```bash
# Replace '/var/sftp' with your desired directory path
sudo mkdir -p /var/sftp/uploads
```

2. Set ownership and permissions.

```bash
# Set the root as the owner of the /var/sftp directory
sudo chown root:root /var/sftp

# Give the sftpuser ownership of the uploads directory
sudo chown sftpuser:sftpuser /var/sftp/uploads

# Set the permissions to allow the user to write to the uploads directory
sudo chmod 755 /var/sftp
sudo chmod 755 /var/sftp/uploads
```

### Step 4: Configure SSH for SFTP
Edit the SSH configuration file to set up SFTP access.

```bash
# Open the SSH configuration file in an editor
sudo nano /etc/ssh/sshd_config
```

Add the following configuration at the end of the file:

```bash
# Subsystem sftp internal-sftp is already included in most cases, add if not present
Subsystem sftp internal-sftp

# SFTP user configuration
Match User sftpuser
    ChrootDirectory /var/sftp
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

### Step 5: Restart the SSH Service
Restart the SSH service to apply the changes.

```bash
sudo systemctl restart ssh
```

### Step 6: Verify SFTP Access
1. Connect to the server via SFTP using the new user.

```bash
sftp sftpuser@your_server_ip
```

2. Navigate to the allowed directory and check permissions.

```bash
# Within the SFTP session
cd /uploads
put somefile.txt
```

### Additional Considerations
1. **Logging and Monitoring:** Set up logging to monitor SFTP activities.
    - Check `/var/log/auth.log` for SSH and SFTP activities.

2. **Security:** Use strong passwords or SSH keys for authentication.
    - You can generate SSH keys using `ssh-keygen` and copy the public key to `/home/sftpuser/.ssh/authorized_keys`.

3. **Firewall:** Ensure the firewall allows SFTP traffic.
    - Use `sudo ufw allow ssh` to allow SSH/SFTP traffic.

4. **Backup:** Regularly back up user data and configuration files.

Following these steps will create a secure and restricted SFTP user on your Ubuntu server, suitable for industry-level use.