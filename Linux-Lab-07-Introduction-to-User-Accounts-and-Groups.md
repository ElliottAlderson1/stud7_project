# Lab 07 Introduction to User Accounts and Groups

## In this lab you will cover

* [Accessing your student VM from the Command prompt using SSH](#task-1-accessing-your-student-vm-from-the-command-prompt-using-ssh)
* [Creating and editing users](#task-2-creating-and-editing-users)
* [Creating, and editing groups](#task-3-creating-and-editing-groups)
* [Performing user account maintenance](#task-4-performing-user-account-maintenance)
* [Elevating a user to root to perform admin tasks](#task-5-elevating-a-user-to-root-to-perform-admin-tasks)
* [Displaying useful user data with various commands](#task-6-displaying-useful-user-data-with-varios-commands)

### Resources

* See instructional material

### Task 1 - Accessing your student VM from the Command prompt using SSH

#### Step 1 - Open the Command Prompt program on Windows

* Click in the Windows search bar at the bottom left of your student laptop, type `Command Prompt`, and click on the Command Prompt program to open it.
* Once the Command Prompt program opens, click inside its window.

#### Step 2 - Remotely connect to your Student Virtual Machine(VM) using SSH

* Enter the SSH commands into the Command Prompt program using `your` student number and student VM IP address:

  ```shell
  ssh <your_student#>@<student_vm_ip_address>
  ```
* Press `enter`.
* When prompted, enter your student password for the student VM:

  ```shell
  <your_student#>@<student_vm_ip_address> password:
  ```
* Press `enter`.
* If login was successful, you will see a new prompt similar to the one below:

  ```shell
  [<your_student#>@<student_vm> ~]$
  ```

### Task 2 - Creating and editing users

#### Step 1 - Create new user accounts

* Add a new user named `bill`using sudo permission:

  ```shell
  sudo useradd -m bill
  ```

  > Notice the `-m` option whcih creates a home directory for the `bill` user in the `/home` directory, if you forget this, they can't login. The `-p` option sets the password for the user.
* Add a new user named `sally`using sudo permission:

  ```shell
  sudo useradd -m sally
  ```
* Add a new user named `jane` using sudo permission:

  ```shell
  sudo useradd -m jane
  ```
* Add a new user named `dan` using sudo permission:

  ```shell
  sudo useradd -m dan
  ```
* Verify that the four new users were created by viewing the last 5 lines of the `/etc/passwd` file:

  ```shell
  tail -5 /etc/passwd
  ```
* You should see:

  ```shell
  ...
  bill:x:1004:1004::/home/bill:/bin/bash
  sally:x:1005:1005::/home/sally:/bin/bash
  jane:x:1006:1006::/home/jane:/bin/bash
  dan:x:1007:1007::/home/dan:/bin/bash
  ```

  > Your output may be slightly different. As users are created, they are added to the bottom of the `/etc/passwd` file.
* Verify that a `home` directory was created for each of the three new users by viewing the contents of the `/home` directory:

  ```shell
  ls /home
  ```
* You should see:

  ```shell
  bill  dan  jane  sally  student1
  ```

  > Your output may be different, but should at least contain the directories named `bill`, `sally`, `jane`, and `dan`.
* Verify that four new groups were created (one for each new user) by viewing the last 5 lines of the `/etc/group` file:

  ```shell
  tail -5 /etc/group
  ```
* You should see:

  ```shell
  ...
  bill:x:1004:
  sally:x:1005:
  jane:x:1006:
  dan:x:1007:
  ```

  > Your output may be different, but should at least contain the groups named `bill`, `sally`, `jane`, and `dan`.

#### Step 2 - Add comments to a user's entry in `/etc/passwd`

* Add a comment to the `bill` account entry in the `/etc/passwd` file using sudo permission:

  ```shell
  sudo usermod -c "Phone 808-441-7862" bill
  ```
* Verify that the comment was added to the `/etc/passwd` file:

  ```shell
  cat /etc/passwd | tail -5
  ```

  > You could also use the command `tail -5 /etc/passwd` and get the same results.
* You should see:

  ```shell
  ...
  bill:x:1004:1004:Phone 808-441-7862:/home/bill:/bin/bash
  ...
  ```

  > Your user ID (1004), and group ID (1004) may be different.

### Task 3 - Creating, and editing groups

#### Step 1 - Create new groups

* Make the new group `students` using sudo permission:

  ```shell
  sudo groupadd students
  ```
* Make the new group `teachers` using sudo permission:

  ```shell
  sudo groupadd teachers
  ```
* Make the new group `admin` using sudo permission:

  ```shell
  sudo groupadd admin
  ```
* Verify that the three new groups were created by viewing the last 5 lines of the `/etc/group` file:

  ```shell
  tail -5 /etc/group
  ```
* You should see:

  ```shell
  ...
  students:x:1008:
  teachers:x:1009:
  admin:x:1010:
  ```

  > Your output may be different, but should at least contain the groups named `students`, `teachers`, and `admin`.

#### Step 2 - Assign users to multiple groups

* Assign `bill` to the `students` group using sudo permission:

  ```shell
  sudo usermod -aG students bill
  ```

  > The `-aG` options adds a user to a group. You can find out more by using the built in `man` pages.
* Verify that `bill` was added to the `students` group:

  ```shell
  grep students /etc/group
  ```

  > This uses the `grep` command to search the `/etc/group` file for the line containing `students` which will display the user added to this group.
* You should see:

  ```shell
  students:x:1008:bill
  ```

  > Your group id may not be 1008 as in the example here; that's ok. We are looking for bill to be listed as a user in the students group.
* Assign `sally` to the `students` group using sudo permission:

  ```shell
  sudo usermod -aG students sally
  ```
* Verify that `sally` was added to the `students` group:

  ```shell
  grep students /etc/group
  ```
* You should see:

  ```shell
  students:x:1008:bill,sally
  ```

  > Your group id may not be 1008 as in the example here; that's ok.
* Assign `jane` to the `students`, and `teachers` groups using sudo permission:

  ```shell
  sudo usermod -aG students,teachers jane
  ```
* Assign `dan` to the `teachers`, and `admin` groups using sudo permission:

  ```shell
  sudo usermod -aG teachers,admin dan
  ```
* Verify that your users were added to the correct group(s):

  ```shell
  tail /etc/group
  ```
* You should see:

  ```shell
  ...
  bill:x:1004:
  sally:x:1005:
  jane:x:1006:
  dan:x:1007:
  students:x:1008:bill,sally,jane
  teachers:x:1009:jane,dan
  admin:x:1010:dan
  ```

  > Your group ID's may be different.

#### Step 3 - View groups a user is assigned to

* View all the groups that a user is in:

  ```shell
  groups jane
  ```
* You should see:

  ```shell
  jane : jane students teachers
  ```

  > This indicates that the user `jane` is in the groups: `jane` (default), `students`, and `teachers`.

#### Step 4 - Remove a user from a group

* Remove `jane` from the `students` group using sudo permission:

  ```shell
  sudo usermod -G teachers jane
  ```

  > Use the -G alone to remove a user from a group. The -G option removes the user from any group not listed, so jane is now only in the teachers group.
* Verify that `jane` was removed from the `students` group:

  ```shell
  grep students /etc/group
  ```
* You should see:

  ```shell
  students:x:1008:bill,sally
  ```
* Verify that `jane` is only in the `jane`, and `teachers` groups:

  ```shell
  groups jane
  ```
* You should see:

  ```shell
  jane : jane teachers
  ```

#### Step 5 - Rename a group

* Rename the `admin` group to `Staff`:

  ```shell
  sudo groupmod -n Staff admin
  ```

  > The `-n` option renames a group.
* Verify that the user `dan` is now in the `Staff` group, and no longer in the `admin` group:

  ```shell
  groups dan
  ```
* You should see:

  ```shell
  dan : dan teachers Staff
  ```
* Verify that the group name has been updated in the `/etc/group` file:

  ```shell
  tail /etc/group
  ```
* You should see:

  ```shell
  ...
  bill:x:1004:
  sally:x:1005:
  jane:x:1006:
  dan:x:1007:
  students:x:1008:bill,sally
  teachers:x:1009:jane,dan
  Staff:x:1010:dan
  ```

  > Notice that name of the `admin` group has changed to `Staff`, as noted by the ID # remaining the same. Your group ID #'s may be different.

### Task 4 - Performing user account maintenance

#### Step 1 - Change a user's password

* Change the password for `bill` using sudo permission:

  ```shell
  sudo passwd bill
  ```
* When prompted, enter the password: `supersecret`.

  > You may recieve an error indicating that this is a `BAD PASSWORD`. Ignore it for now, but make better passwords in production systems.
* When prompted, re-enter the password: `supersecret`.

#### Step 2 - Delete a user account

* Delete the `sally` user account and it's `home` directory using sudo permission:

  ```shell
  sudo userdel -r sally
  ```

  > The `-r` option removes files in the user's home directory, and removes the home directory itself.
* Verify that the user `sally` has been removed from the `/etc/passwd` file:

  ```shell
  tail /etc/passwd
  ```

  > You should not see a line entry for the user `sally`. You could also use the `grep sally /etc/passwd` command to search the file. In that case, no data will be returned because the user `sally` no longer exists.
* Verify that the `home` directory of the `sally` user no longer exists:

  ```shell
  ls /home
  ```

  > You should not see a directory named `sally` because it was deleted with the "userdel -r" command.

#### Step 3 - View log entries related to deletion of a user

* View the `userdel` entries in the /var/log/secure file to see the log entries for the deletion of the `sally` user using sudo permission:

  ```shell
  sudo grep userdel /var/log/secure
  ```
* You should see:

  ```shell
  ...
  May  8 12:54:03 orkostudent1 userdel[19981]: delete user 'sally'
  May  8 12:54:03 orkostudent1 userdel[19981]: delete 'sally' from group 'students'
  May  8 12:54:03 orkostudent1 userdel[19981]: removed group 'sally' owned by 'sally'
  May  8 12:54:03 orkostudent1 userdel[19981]: removed shadow group 'sally' owned by 'sally'
  May  8 12:54:03 orkostudent1 userdel[19981]: delete 'sally' from shadow group 'students'
  ```

  > your output may have a different username, different dates, and different times.

### Task 5 - Elevating a user to root to perform admin tasks

The `root` account on a Linux system has unlimited permissions and is required to perform most administrative tasks. It should be protected with a strong password, and only used to perform administrative tasks.

#### Step 1 - Switch your user to the `root` user

* Change to the root user using sudo permissions:

  ```shell
  sudo su
  ```

  > `su` is the `switch user` command. `su` may be used to switch between any account, but defaults to the `root` account when no account is specified in the `su` command. Example syntax to switch to the bill account would be: `sudo su bill`.
* You should now see that the prompt has changed to:

  ```shell
  [root@orkostudent1 student1]#
  ```

  > Your output may be slightly different, but should have `root` on the far left, and a pound sign `#` on the far right. Both of these indicate that you are now using the root account.
* Verify the current user with the `whoami` command:

  ```shell
  whoami
  ```
* You should see:

  ```shell
  root
  ```

#### Step 2 - Perform admin tasks as the `root` user without using the `sudo` command

* Create a new user `adam` while logged in as root:

  ```shell
  useradd -m adam
  ```

  > Notice that you did not need to use the `sudo` command. This is because you are logged in as the `root` user and have `sudo` permission by default.
* Delete the user `adam` while logged in as root:

  ```shell
  userdel -r adam
  ```
* Log out of the root user account:

  ```shell
  exit
  ```

  > Notice that your prompt has changed back to showing your student# on the far left, and the dollar sign `$` on the far right. This indicates that you are no longer logged in as the root user.
* Verify that you are logged in with your student user account:

  ```shell
  whoami
  ```

  > You should see the name of your student account (your student#).

### Task 6 - Displaying useful user data with various commands

#### Step 1 - Using the `id` command

* You can view detailed information about a particular user with the `id` command:

  ```shell
  id bill
  ```
* You should see:

  ```shell
  uid=1004(bill) gid=1004(bill) groups=1004(bill),1008(students)
  ```

#### Step 2 - Using the `who` command

* You can see who is currently logged into this Linux system with the `who` command:

  ```shell
  who
  ```
* You should see:

  ```shell
  student1 pts/0        2022-05-08 10:39 (192.168.200.254)
  ```

  > Your output may be slightly different.

#### Step 3 - Using the `w` command

* You can see more detailed information on who is currently logged into this Linux system with the `w` command:

  ```shell
  w
  ```
* You should see:

  ```shell
  13:20:00 up 1 day,  2:35,  1 user,  load average: 0.00, 0.00, 0.00
  USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
  student1 pts/0    192.168.200.254  10:39    0.00s  0.32s  0.01s w
  ```

  > Your output may be slightly different.