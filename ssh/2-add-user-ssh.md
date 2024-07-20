Sure, here’s how you can set up a new SSH user who can log in with a password, while allowing root or other users with `my_key_pair.pem` to log in as well.

### Step 1: Create a New User
Create a new user who will be allowed to log in with a password.

```bash
# Replace 'sshuser' with your desired username
sudo adduser sshuser

# Follow the prompts to set the user's password and other details
```

### Step 2: Configure SSH to Allow Password Authentication
Edit the SSH configuration file to allow password authentication.

```bash
# Open the SSH configuration file
sudo nano /etc/ssh/sshd_config
```

Ensure the following lines are set:

```bash
# Allow password authentication
PasswordAuthentication yes

# Allow root login (if not set, set it as required)
PermitRootLogin prohibit-password  # or 'yes' if you prefer

# Specify the key-based authentication settings
RSAAuthentication yes
PubkeyAuthentication yes
```

### Step 3: Configure SSH Key-Based Authentication for Root and Other Users
1. **For root user:**

```bash
# Create .ssh directory if it does not exist
sudo mkdir -p /root/.ssh

# Set appropriate permissions
sudo chmod 700 /root/.ssh

# Add the public key to the authorized_keys file
sudo nano /root/.ssh/authorized_keys

# Paste your public key here (from my_key_pair.pem)
ssh-rsa AAAAB3Nza... your_key_comment
```

2. **For other users (e.g., another_user):**

```bash
# Replace 'another_user' with the username
sudo mkdir -p /home/another_user/.ssh
sudo chmod 700 /home/another_user/.ssh
sudo nano /home/another_user/.ssh/authorized_keys

# Paste your public key here (from my_key_pair.pem)
ssh-rsa AAAAB3Nza... your_key_comment

# Change ownership of .ssh and authorized_keys
sudo chown -R another_user:another_user /home/another_user/.ssh
```

### Step 4: Restart SSH Service
Restart the SSH service to apply the changes.

```bash
sudo systemctl restart ssh
```

### Step 5: Verify Access
1. **Password Login for sshuser:**

```bash
ssh sshuser@your_server_ip
```

2. **Key-Based Login for root:**

```bash
ssh -i /path/to/my_key_pair.pem root@your_server_ip
```

3. **Key-Based Login for another_user:**

```bash
ssh -i /path/to/my_key_pair.pem another_user@your_server_ip
```

### Step 6: Additional Security Measures
1. **Disable Root Login via Password:**

```bash
# Edit sshd_config
PermitRootLogin prohibit-password
```

2. **Enable UFW Firewall and Allow SSH:**

```bash
# Allow SSH
sudo ufw allow 22

# Enable the firewall
sudo ufw enable
```

This setup allows `sshuser` to log in with a password while maintaining key-based authentication for root and other users. The SSH configuration ensures secure access by allowing different authentication methods as required.