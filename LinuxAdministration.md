### **Linux Administration Topics**

1. **Command-line Tools**
   - **Command-line tools** in Linux are essential for interacting with the system, managing files, processes, and networks. Some fundamental tools include:
     - **ls**: Lists directory contents.
     - **cd**: Changes the current directory.
     - **cp/mv/rm**: Copy, move, and remove files or directories.
     - **ps/top**: Displays currently running processes.
     - **grep**: Searches text using patterns.
     - **find**: Searches for files and directories in a directory hierarchy.
     - **awk/sed**: Text processing tools.
     - **chmod/chown**: Changes file permissions and ownership.
   - **Examples**:
     - Listing all files in a directory: `ls -la`
     - Finding a specific process: `ps aux | grep process_name`

2. **Shell Scripting**
   - **Shell scripting** is the process of writing a script (a series of commands) for the shell to execute. Shell scripts automate repetitive tasks, simplifying complex operations.
   - **Basic Components**:
     - **Shebang**: `#!/bin/bash` at the top of the script specifies the interpreter.
     - **Variables**: Store data for use in the script, e.g., `name="User"`.
     - **Conditionals**: `if`, `else`, and `elif` statements to control flow.
     - **Loops**: `for`, `while`, and `until` loops to repeat commands.
     - **Functions**: Reusable blocks of code, e.g., `my_function() { echo "Hello"; }`
   - **Example**: A script to back up a directory:
     ```bash
     #!/bin/bash
     BACKUP_DIR="/backup"
     SOURCE_DIR="/home/user"

     if [ ! -d "$BACKUP_DIR" ]; then
       mkdir -p "$BACKUP_DIR"
     fi

     tar -czf "$BACKUP_DIR/backup_$(date +%Y%m%d).tar.gz" "$SOURCE_DIR"
     echo "Backup completed."
     ```

3. **File Permissions**
   - **File permissions** in Linux determine who can read, write, or execute a file. Permissions are defined for three categories: owner, group, and others.
   - **Understanding Permissions**:
     - **rwx**: `r` (read), `w` (write), `x` (execute).
     - **chmod**: Changes file permissions. Use symbolic (`chmod u+x file`) or numeric (`chmod 755 file`) modes.
     - **chown**: Changes file ownership. Example: `chown user:group file`.
   - **Numeric Permissions**:
     - 4 = Read (`r`)
     - 2 = Write (`w`)
     - 1 = Execute (`x`)
     - Example: `chmod 755` gives `rwxr-xr-x` (owner can read, write, and execute; group and others can read and execute).

4. **System Monitoring**
   - **System monitoring** involves tracking the performance and health of a Linux system. Key aspects include CPU, memory, disk usage, and network activity.
   - **Tools**:
     - **top/htop**: Real-time process monitoring.
     - **vmstat**: Displays virtual memory statistics.
     - **iostat**: Monitors I/O statistics.
     - **netstat**: Network statistics.
     - **df/du**: Disk usage and free space.
     - **uptime**: Shows how long the system has been running.
   - **Example**:
     - Monitor CPU and memory usage: `top` or `htop`
     - Check disk space usage: `df -h`

5. **Process Management**
   - **Process management** involves controlling the processes running on a Linux system.
   - **Tools**:
     - **ps**: Displays the current processes.
     - **kill**: Terminates a process by PID, e.g., `kill 1234`.
     - **killall**: Kills all processes with a specific name, e.g., `killall firefox`.
     - **nice/renice**: Adjusts process priority.
     - **bg/fg**: Moves processes between the background and foreground.
   - **Example**:
     - List all processes: `ps aux`
     - Kill a process with PID 1234: `kill 1234`

6. **Networking Basics**
   - **Networking in Linux** involves configuring and managing network interfaces, routing, and network services.
   - **Tools**:
     - **ifconfig/ip**: Configures network interfaces.
     - **ping**: Tests connectivity to another host.
     - **traceroute**: Traces the route packets take to a destination.
     - **netstat/ss**: Network statistics and socket information.
     - **iptables**: Configures firewall rules.
   - **Example**:
     - Check network interface configuration: `ifconfig` or `ip a`
     - Test network connectivity: `ping google.com`

---

### **Answers to the Questions**

1. **How do you check the status of a process in Linux?**
   - You can check the status of a process in Linux using several commands:
     - **ps**: Lists running processes and their statuses.
       - Example: `ps aux | grep process_name` will show information about the process, including its status.
     - **top/htop**: Real-time process monitoring tools.
       - Run `top` and look for the `STAT` column, which shows the process state (e.g., `R` for running, `S` for sleeping).
     - **pgrep/pidof**: Finds the PID of a process.
       - Example: `pgrep -fl process_name` or `pidof process_name`.
     - **systemctl**: If the process is a service, you can check its status using `systemctl status service_name`.

2. **Explain how to manage file permissions in Linux.**
   - File permissions in Linux are managed using the `chmod` command. Permissions are divided into three categories: owner, group, and others, with each category having read (`r`), write (`w`), and execute (`x`) permissions.
   - **Symbolic Mode**: Use letters to modify permissions.
     - Example: `chmod u+x file` adds execute permission for the user (owner).
     - `u`: User (owner)
     - `g`: Group
     - `o`: Others
     - `a`: All
   - **Numeric Mode**: Use numbers to set permissions directly.
     - Example: `chmod 755 file` sets `rwxr-xr-x` (owner can read, write, and execute; group and others can read and execute).
     - Each permission has a numeric value: 4 (read), 2 (write), 1 (execute).
     - Combine values to set permissions: `chmod 644 file` gives `rw-r--r--`.
   - **Ownership**: Use `chown` to change the ownership of a file or directory.
     - Example: `chown user:group file` changes the owner to `user` and the group to `group`.

3. **How would you write a basic shell script to automate a task?**
   - A basic shell script can automate tasks like backups, system monitoring, or user management.
   - **Example**: A script to check disk usage and send an email alert if usage exceeds a threshold:
     ```bash
     #!/bin/bash
     THRESHOLD=80
     USAGE=$(df -h / | grep -Eo '[0-9]{1,3}%' | tr -d '%')

     if [ "$USAGE" -gt "$THRESHOLD" ]; then
       echo "Disk usage is above $THRESHOLD%. Current usage is $USAGE%." | mail -s "Disk Usage Alert" user@example.com
     fi
     ```
   - **Explanation**:
     - `df -h /`: Checks disk usage of the root filesystem.
     - `grep -Eo '[0-9]{1,3}%' | tr -d '%'`: Extracts the percentage value.
     - `if [ "$USAGE" -gt "$THRESHOLD" ]`: Compares usage with the threshold.
     - `mail`: Sends an email alert if the condition is met.

4. **What tools would you use to monitor a Linux server?**
   - Monitoring a Linux server requires tools that track CPU, memory, disk usage, network activity, and process performance.
   - **Common Tools**:
     - **top/htop**: For real-time process monitoring and system load.
     - **vmstat**: Provides information on processes, memory, paging, block I/O, traps, and CPU activity.
     - **iostat**: Monitors system input/output device loading.
     - **netstat/ss**: Provides network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.
     - **df/du**: Displays disk space usage and free space.
     - **uptime**: Shows how long the system has been running, along with the load average.
     - **nagios**: An enterprise-level monitoring system that monitors the status of hosts and services.
     - **Prometheus/Grafana**: A powerful combination for metrics collection and visualization.
   - **Example**:
     - To monitor real-time CPU and memory usage: `htop`
     - To monitor disk I/O: `iostat`

