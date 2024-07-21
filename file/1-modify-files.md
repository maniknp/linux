To view the history of a file, such as when it was created and last modified, and to update these timestamps to custom dates, you can use several tools and commands in Linux.

### Viewing File Timestamps

1. **Basic Timestamps:**

You can use the `stat` command to view the detailed timestamps of a file.

```bash
stat filename
```

Example output:
```bash
  File: filename
  Size: 12345           Blocks: 24         IO Block: 4096   regular file
Device: 802h/2050d      Inode: 123456      Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/ username)   Gid: ( 1000/ username)
Access: 2024-07-20 12:34:56.000000000 +0000
Modify: 2024-07-19 12:34:56.000000000 +0000
Change: 2024-07-18 12:34:56.000000000 +0000
 Birth: -
```

Here:
- `Access` indicates the last access time.
- `Modify` indicates the last modification time.
- `Change` indicates the last change time.

2. **File Birth (Creation) Time:**

Some filesystems, like `ext4`, may not store the file creation time (`Birth`). You may need to use `debugfs` or another filesystem-specific tool to get the creation time.

```bash
sudo debugfs -R 'stat <inode_number>' /dev/sdXn
```

Replace `<inode_number>` with the inode number of the file, and `/dev/sdXn` with the appropriate filesystem.

### Updating File Timestamps

To update the creation, modification, and access times of a file to custom dates, you can use the `touch` command.

1. **Change Access and Modification Times:**

```bash
# Change access time (-a) and modification time (-m) to a specific date
touch -a -m -t YYYYMMDDHHMM.SS filename
```

Example:
```bash
touch -a -m -t 202307201230.45 filename
```
This sets the access and modification times to 2023-07-20 12:30:45.

2. **Change Only Access or Modification Time:**

- **Access Time:**
  ```bash
  touch -a -t YYYYMMDDHHMM.SS filename
  ```

- **Modification Time:**
  ```bash
  touch -m -t YYYYMMDDHHMM.SS filename
  ```

### Changing the Creation Time (Birth Time)

Changing the creation time is more complex and is not supported directly by `touch` or other standard commands. However, you can achieve this by copying the file or using advanced tools.

1. **Copying the File:**

Copying a file to a new file will set the new file's creation time to the current time. You can then change the modification and access times as needed.

```bash
cp filename newfile
touch -a -m -t YYYYMMDDHHMM.SS newfile
```

2. **Using `debugfs` (for ext4 Filesystem):**

```bash
sudo debugfs -w /dev/sdXn
debugfs:  stat /path/to/file
debugfs:  set_inode_field /path/to/file crtime <new_date>
debugfs:  quit
```

Replace `/dev/sdXn` with the appropriate filesystem and `<new_date>` with your custom date.

### Example Script to Change Timestamps

Here is a simple script to update the modification and access times:

```bash
#!/bin/bash

filename=$1
custom_date=$2

if [ -z "$filename" ] || [ -z "$custom_date" ]; then
  echo "Usage: $0 filename YYYYMMDDHHMM.SS"
  exit 1
fi

# Change access and modification times
touch -a -m -t $custom_date $filename

echo "Updated timestamps for $filename"
stat $filename
```

Save this script as `update_timestamps.sh`, make it executable, and run it:

```bash
chmod +x update_timestamps.sh
./update_timestamps.sh filename 202307201230.45
```

These steps should help you view and update the file timestamps as needed.