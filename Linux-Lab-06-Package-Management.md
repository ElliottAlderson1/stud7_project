# Lab 06 Package Management

## In this lab you will cover

* [Accessing your student VM from the Command prompt using SSH](#task-1-accessing-your-student-vm-from-the-command-prompt-using-ssh)
* [Viewing details of Linux install packages and repositories](#task-2-viewing-details-of-linux-install-packages-and-repositories)
* [Hosting a web page using the Apache web server (httpd)](#task-3-hosting-a-web-page-using-the-apache-web-server-httpd)
* [Uninstalling a package](#task-4-uninstalling-a-package)
* [Working with system processes](#task-5-working-with-system-processes)

### Resources

* See instructional material

### Task 1 - Accessg your student VM from the Command prompt using SSH

#### Step 1 - Open the Command Prompt program on Windows

#### Step 2 - Remotely connect to your Student Virtual Machine(VM) using SSH

### Task 2 - Viewing details of Linux install packages and repositories

The `dnf` and `yum` commands are very similar but not entirely interchangeable. We will be using the `yum` command for this lab.

`Yum` is a program for installing, updating, and removing program packages and their dependencies. Program packages can be stored in online, or local repositories that `yum` can query for packages. The Linux system you are using for this lab stores information about these repositories in the file: `/etc/yum.repos.d`

#### Step 1 - View local `yum` repository data

* View the package repositories that `yum` is currently using:

  ```shell
  yum repolist
  ```
* You should see:

  ```
  repo id                                                                   repo name 
  appstream                                                                 Rocky Linux 8 - AppStream
  baseos                                                                    Rocky Linux 8 - BaseOS
  extras                                                                    Rocky Linux 8 - Extras
  ```

  > This list may vary based on what repositories are configured. These repositories are listed here because a config file for them is stored in the directory `/etc/yum.repos.d/`. For a repository to be listed in the output of the `yum repolist` command, it must have a configuration file in the `/etc/yum.repos.d/` folder and the `enabled` variable inside the config file must be equal to `1`. Example: `enabled=1`.
* View the yum repository config files:

  ```shell
  ls /etc/yum.repos.d
  ```
* You should see:

  ```shell
  Rocky-AppStream.repo Rocky-Debuginfo.repo Rocky-Extras.repo Rocky-Media.repo 
  Rocky-Plus.repo Rocky-ResilientStorage.repo Rocky-Sources.repo Rocky-BaseOS.repo  
  Rocky-Devel.repo Rocky-HighAvailability.repo Rocky-NFV.repo Rocky-PowerTools.repo Rocky-RT.repo
  ```

  > Your output may be different. You can view the contents of any of these files using the `cat` command.
* View the contents of one of the `.repo` files:

  ```shell
  cat /etc/yum.repos.d/<name_of_repo_file>
  ```

  > You should see the contents of the .repo file displayed. Notice the "enabled" line that was discussed earlier in this lab.

#### Step 2 - View remote `yum` repository data

* View a list of rpm packages available on the yum repositories configured on your student VM:

  ```shell
  yum list
  ```

  > You should see a very long list of program package names.
* View the same list, but use `grep` to filter for packages with a specific program name:

  ```shell
  yum list | grep -E "^wireshark"
  ```

  > The `grep -E "^wireshark"` command shows only those packages in the output of the `yum list` command that have `wireshark` at the start of the line.
* You should see:

  ```shell
  wireshark.x86_64                        1:2.6.2-14.el8         rhel-8-for-x86_64-appstream-rpms
  wireshark-cli.i686                      1:2.6.2-14.el8         rhel-8-for-x86_64-appstream-rpms
  wireshark-cli.x86_64                    1:2.6.2-14.el8         rhel-8-for-x86_64-appstream-rpms
  ```

  > Your output may be different depending on the .repo files you have on your Linux computer.
* Yum has a built in command to perform the above list with grep command, but shorter:

  ```shell
  yum search wireshark
  ```
* You should see:

  ```shell
  Last metadata expiration check: 0:52:46 ago on Sat 07 May 2022 09:25:04 AM EDT.
  ========= Name Exactly Matched: wireshark =========
  wireshark.x86_64 : Network traffic analyzer
  ========= Name Matched: wireshark =========
  wireshark-cli.i686 : Network traffic analyzer
  wireshark-cli.x86_64 : Network traffic analyzer
  ```

  > Notice that the same three packages are listed as before, but in a slightly different display format. Either way, you know that the `wireshark` program is available for install with the `yum` command. Your output may be different.

#### Step 3 - View `yum` data about installed packages

* View the rpm packages installed on your student VM:

  ```shell
  yum list installed
  ```

  > This will be a very long output because you have several packages installed on your student Linux VM.
* Filter the `yum list installed` command with `grep`:

  ```shell
  yum list installed | grep git
  ```

  > Your output list should include a package for the program "Git".

#### Step 4 - View the yum info file for a package

* View data about a specific package even if it is not installed:

  ```shell
  yum info httpd
  ```
* You should see:

  ```shell
  Last metadata expiration check: 1:05:39 ago on Sat 07 May 2022 09:25:04 AM EDT.
  Installed Packages
  Name         : httpd
  Version      : 2.4.37
  Release      : 43.module_el8.5.0+1022+b541f3b1
  Architecture : x86_64
  Size         : 4.3 M
  Source       : httpd-2.4.37-43.module_el8.5.0+1022+b541f3b1.src.rpm
  Repository   : @System
  From repo    : appstream
  Summary      : Apache HTTP Server
  URL          : https://httpd.apache.org/
  License      : ASL 2.0
  Description  : The Apache HTTP Server is a powerful, efficient, and extensible
               : web server.
  ```

  > Your output may be slightly different depending on the most recent verion of httpd.

### Task 3 - Hosting a web page using the Apache web server (httpd)

#### Step 1 - Install the `httpd` package

* Install the httpd package using elevated `root` privilege:

  ```shell
  sudo yum install httpd
  ```

  > You must use the `sudo` command to elevate your privileges to the root user. This is required to perform certain administrative tasks on a Linux system.
* When prompted, enter your user password, and press enter.
* You should see several lines of output from the command, and be promted with `Is this ok [y/N]:`. Press `y`, and press `enter`.
* You should see several more lines of code finally ending with `Complete!`.
* Verify that the `httpd` package was installed:

  ```shell
  yum list installed | grep httpd
  ```
* You should see:

  ```shell
  httpd.x86_64  2.4.37-43.module_el8.5.0+1022+b541f3b1
  ```

  > Your output may be slightly different depending on the most recent verion of httpd.

#### Step 2 - Enable the `httpd` service

* Check the current status of the `httpd` service:

  ```shell
  sudo systemctl status httpd
  ```
* You should see the following line in the output:

  ```shell
  Active: inactive (dead)
  ```

  > This indicates that the `httpd` service is not active.
* Activate the `httpd` service:

  ```shell
  sudo systemctl start httpd
  ```
* Check the current status of the `httpd` service:

  ```shell
  sudo systemctl status httpd
  ```
* You should now see the line:

  ```shell
  Active: active (running) ...
  ```

  > The Apache web server is now up and running.
* Ensure that the Apache service starts when the Linux computer (or VM) starts:

  ```shell
  sudo systemctl enable httpd
  ```

#### Step 3 - Create a webpage for your Apache web server (httpd)

* Navigate to the /var/www/html folder:

  ```shell
  cd /var/www/html
  ```
* Create the index.html file with nano by using the root privileges of `sudo`:

  ```shell
  sudo nano index.html
  ```

  > You should now be in the `index.html` file in the `nano` text editor program.
* Add the following html code:

  ```html
  <html>
    <center>
      <h1>My awesome webpage</h1>
      <a href="https://www.jcu.mil">DoD's Finest Communicators</a>
    </center>
  </html>
  ```
* Save the file by holding `ctrl` and pressing `o`.
* When prompted with: `File name to Write: index.html`, press `enter`.
* Exit the nano text editor program by holding `ctrl` and pressing `x`.
* Open a web browser on your student laptop and navigate to the IP address of your student VM.

  > This is the same IP address that you use to SSH into your VM.
* At this time, the web page will not load because the firewall on your VM is blocking traffic on http port 80.
* Open port 80 on your VM's firewall:

  ```shell
  sudo firewall-cmd --add-port=80/tcp --permanent
  ```

  > It may take a moment, but you should see the message `success` displayed.
* Reload the firewall:

  ```shell
  sudo firewall-cmd --reload
  ```

  > It may take a moment, but you should see the message `success` displayed.
* Go back to your web browser and again go to the IP address of your student VM.

  > You should see your web page displayed. If not, try closing the web browser and opening a new one, then navigating to your student VM's IP address.

### Task 4 - Uninstalling a package

#### Step 1 - Remove the port 80 access

* Close port 80 on your student VM's firewall:

  ```shell
  sudo firewall-cmd --remove-port=80/tcp --permanent
  ```

  > You can verify that the port is closed by trying to refresh the webpage `My awesome webpage`. it should not be reachable.

#### Step 2 - Stop `httpd` service

* Stop the `httpd` service:

  ```shell
  sudo systemctl stop httpd
  ```
* Verify that the `httpd` service has stopped:

  ```shell
  sudo systemctl status httpd
  ```
* You should see the following line in the output:

  ```shell
  Active: inactive (dead)
  ```

#### Step 3 - Uninstall the `httpd` package

* Remove the `httpd` package:

  ```shell
  sudo yum remove httpd
  ```
* When promted with `Is this ok [y/N]:`. Press `y`, and press `enter`.

  > You will see several lines of code, but when it is done installing, the message `Complete!` will be displayed.
* Verify that the `httpd` package was removed:

  ```shell
  yum list installed | grep httpd
  ```

  > You should see no output from this command, and a new terminal line should appear.

### Task 5 - Working with system processes

#### Step 1 - View processes with the `ps` command

* Navigate to your `user` home directory:

  ```shell
  cd
  ```
* View process that your user is running:

  ```shell
  ps
  ```

  > You should see a short list of process that are running.
* Use additional `ps` command syntax to view all the processes running on your student VM:

  ```shell
  ps aux
  ```
* Filter the above command to only view processes being run by the `root` user and save these to a file:

  ```shell
  ps aux | grep -E "^root" > root_proc
  ```
* View the content of the `root_proc` file with `nano`:

  ```shell
  nano root_proc
  ```

  > When you are done viewing the data, exit nano by holding the `ctrl` key and pressing `x`.

#### Step 2 - View system resource usage with the `top` command

* View the current status of system resources by using the `top` command:

  ```shell
  top
  ```

  > This will bring up a display showing the current status of system resources. Notice the `load average` at top of screen.
* Try pressing the following keys to change the `top` settings:
  * `f` key for fields.
  * `d` key for refresh delay.
  * `m` key to sort by memory usage.
  * `q` exit the top program.
* Press the `q` key to exit the `top` program.
* View the current availability of system memory using the `free` command:

  ```shell
  free
  ```
* Watch the free command periodically update by using the `watch` command:

  ```shell
  watch free
  ```

  > Not much is going to happen, but you should see the time changing, and small changes to the amount of free and available memory. The watch command can be used with several other Linux shell programs.
* Hold down the `ctrl` key, and press `z` to exit the `watch` command.

#### Step 3 - Kill a running process

* View the current running processes:

  ```shell
  ps
  ```
* You should see:

  ```shell
   PID TTY          TIME CMD
  2323 pts/0    00:00:00 bash
  3650 pts/0    00:00:00 watch
  3675 pts/0    00:00:00 ps
  ```

  > Your list may be different, but you should have the `watch` process still running.
* Kill the `watch` process using the following format: `kill -9 <process_id>`:

  ```shell
  kill -9 3650
  ```

  > The command here is using the process ID from my example above. You must use the process ID displayed in the output of your `ps` command.
* Verify that the `watch` process was killed:

  ```shell
  ps
  ```

  > You should no longer see the watch process listed in the output of the `ps` command.
* Change to the `/var/log` directory:

  ```shell
  cd /var/log
  ```
* View the contents of the `/var/log` directory:

  ```shell
  ls
  ```

  > Several types of logs and log backups are stored here.
* Use root permissions to view the contents of the `messages` log file:

  ```shell
  sudo cat messages
  ```
* View the content of the `messages` log and filter it to view those messages associated with the `Apache web service`:

  ```shell
  sudo cat messages | grep Apache
  ```
* You should see:

  ```shell
  May  7 10:45:15 orkostudent1 systemd[1]: Starting The Apache HTTP Server...
  May  7 10:45:15 orkostudent1 systemd[1]: Started The Apache HTTP Server.
  May  7 11:08:18 orkostudent1 systemd[1]: Stopping The Apache HTTP Server...
  May  7 11:08:19 orkostudent1 systemd[1]: Stopped The Apache HTTP Server.
  ```

  > Your output may be slightly different.
* View the content of the `secure` log and filter it to view those messages associated with the `httpd` package:

  ```shell
  sudo cat messages | grep httpd
  ```
* You should see several lines. Look for an entry that has `yum install httpd` at the end. This is the log entry for when you installed the `httpd` package. You should also see entries for starting and stopping the `httpd` process. You may see "Installed" and "Erased" instead depending on the Linux distribution you are using.
* If the above command does not display httpd messages, check the dnf.log file:

  ```shell
  sudo cat dnf.log | grep httpd
  ```