This issue is typically caused by SSH timing out due to inactivity. You can configure the SSH server to send keep-alive packets to maintain the connection during periods of inactivity. 

### Step 1: Configure the Server-Side SSH Settings

1. Edit the SSH daemon configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

2. Add or modify the following lines to send keep-alive messages from the server to the client:

```bash
# ClientAliveInterval is the interval (in seconds) the server will wait before sending a keep-alive message
ClientAliveInterval 60

# ClientAliveCountMax is the number of keep-alive messages the server will send before closing the connection
ClientAliveCountMax 3
```

### Step 2: Restart the SSH Service

Restart the SSH service to apply the changes:

```bash
sudo systemctl restart ssh
```

### Step 3: Configure the Client-Side SSH Settings (Optional)

You can also configure your SSH client to send keep-alive messages to the server. This is useful if the server-side configuration cannot be changed.

1. Edit or create the SSH client configuration file:

```bash
nano ~/.ssh/config
```

2. Add the following lines to send keep-alive messages from the client to the server:

```bash
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 60
```

### Explanation

- **ClientAliveInterval 60**: The server will send a keep-alive message every 60 seconds.
- **ClientAliveCountMax 3**: If the server doesn't receive a response to 3 keep-alive messages, it will close the connection.
- **ServerAliveInterval 60**: The client will send a keep-alive message every 60 seconds.
- **ServerAliveCountMax 3**: If the client doesn't receive a response to 3 keep-alive messages, it will close the connection.

These settings should prevent your SSH session from becoming inactive due to periods of inactivity. 

### Additional Tips

1. **Using `screen` or `tmux`**: You can use terminal multiplexers like `screen` or `tmux` to keep your sessions alive even if the SSH connection drops. These tools allow you to detach from a session and reattach later.

   - Install `tmux`:

     ```bash
     sudo apt install tmux
     ```

   - Start a `tmux` session:

     ```bash
     tmux
     ```

   - Detach from a `tmux` session:

     ```bash
     Ctrl+b d
     ```

   - Reattach to a `tmux` session:

     ```bash
     tmux attach
     ```

These changes should help you maintain your SSH sessions without needing to reconnect frequently due to inactivity.