# Lab 02 Manipulating files and directories

## In this lab you will cover

* [Accessing your student VM from the Command prompt using SSH](#task-1-accessing-your-student-vm-from-the-command-prompt-using-ssh)
* [Creating Directories and files in the Linux shell](#task-2-creating-directories-and-files-in-the-linux-shell)
* [Viewing directory contents using ](#task-3-viewing-directory-contents-using-tree-command-relative-paths-and-absolute-paths)`tree` command, relative paths, and absolute paths
* [Manipulating files with the move command](#task-4-manipulating-files-with-the-move-command)
* [Manipulating files with the copy command](#task-5-manipulating-files-with-the-copy-command)
* [Removing files and directories](#task-6-removing-files-and-directories)
* [Working with symbolic links](#task-7-working-with-symbolic-links)
* [Understanding the Linux directory structure](#task-8-understanding-the-linux-directory-structure)

### Resources

* See instructional material

### Task 1 - Access your student VM from the Command prompt using SSH

#### Step 1 - Open the Command Prompt program on Windows

#### Step 2 - Remotely connect to your Student Virtual Machine(VM) using SSH

### Task 2 - Creating Directories and files in the Linux shell

#### Step 1 - Remove any existing files or folders in the home directory

* Change to the user's home directory if you are not already there:

  ```shell
  cd ~
  ```

  > The tilde `~` character is located above the left tab on the keyboard.
* View the contents of the home directory:

  ```shell
  ls -la
  ```

  > You should see some directories listed.
* Delete all files and folders in the directory:

  ```shell
  rm -rf *
  ```
* Delete any hidden files:

  ```shell
  rm -rf .*
  ```
* View the contents of the home directory:

  ```shell
  ls -la
  ```

  > You should no longer see any directories listed.

#### Step 2 - Create directories in the Linux shell

* Create the new folder `Documents`:

  ```shell
  mkdir Documents
  ```
* Change to the new `Documents` directory:

  ```shell
  cd Documents
  ```
* Create the new directory `Folder1`:

  ```shell
  mkdir Folder1
  ```
* Create the new directory `Folder2`:

  ```shell
  mkdir Folder2
  ```

#### Step 3 - Create files from the Linux shell

* Create four new files using a single line:

  ```shell
  touch bale.txt ball.txt bowl.txt bull.txt
  ```

  > Filenames only need to be seperated by a single space.
* Create a new hidden file:

  ```shell
  touch .balehidden
  ```
* View the contents of the `Documents` directory as a list:

  ```shell
  ls -l
  ```
* You should see:

  ```shell
  -rw-rw-r--. 1 student1 student1 0 May  3 21:34 bale.txt
  -rw-rw-r--. 1 student1 student1 0 May  3 21:34 ball.txt
  -rw-rw-r--. 1 student1 student1 0 May  3 21:34 bowl.txt
  -rw-rw-r--. 1 student1 student1 0 May  3 21:34 bull.txt
  drwxrwxr-x. 2 student1 student1 6 May  3 21:34 Folder1
  drwxrwxr-x. 2 student1 student1 6 May  3 21:34 Folder2
  ```

  > Notice that the hidden file `.balehidden` was not listed in the output.
* Display all files, including the hidden files:

  ```shell
  ls -la
  ```
* You should see:

  ```shell
  drwxrwxr-x. 4 studx studx 119 May  3 21:35 .
  drwx------. 7 studx studx 234 May  3 21:31 ..
  -rw-rw-r--. 1 studx studx   0 May  3 21:35 .balehidden
  -rw-rw-r--. 1 studx studx   0 May  3 21:34 bale.txt
  -rw-rw-r--. 1 studx studx   0 May  3 21:34 ball.txt
  -rw-rw-r--. 1 studx studx   0 May  3 21:34 bowl.txt
  -rw-rw-r--. 1 studx studx   0 May  3 21:34 bull.txt
  drwxrwxr-x. 2 studx studx   6 May  3 21:34 Folder1
  drwxrwxr-x. 2 studx studx   6 May  3 21:34 Folder2
  ```

  > Notice that the hidden file `.balehidden` is now listed in the output.

#### Step 4 Move files into a folder

* Move the `bull.txt` file to the `Folder2` folder:

  ```shell
  mv bull.txt Folder2/bull.txt
  ```

  > It is not necessary to add the name of the file after "Folder2/". We will use this feature later to rename files when they are moved.
* Verify that the `bull.txt` file has been moved to the `Folder2` folder:

  ```shell
  ls -l Folder2
  ```

  > The syntax `Folder2` is a relative file path used to tell Linux that we want to see the directory listing in the `Folder2` folder.
* You should see:

  ```shell
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 bull.txt
  ```
* Move the `bowl.txt` file to the `Folder2` folder:

  ```shell
  mv bowl.txt Folder2/bowl.txt
  ```
* Verify that the `bowl.txt` file has been moved to the `Folder2` folder:

  ```shell
  ls -l Folder2
  ```

  > The syntax `Folder2`, is the relative path used to tell Linux that we want to see the directory listing in the `Folder2` folder.
* You should see:

  ```shell
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 bowl.txt
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 bull.txt
  ```
* Move any remaining files in the `Documents` folder that end with `.txt` to the `Folder1` folder:

  ```shell
  mv *.txt Folder1
  ```

  > The asterix character "\*" acts as a wildcard to allow any files that end with ".txt" to be selected by the "mv" command
* Verify that the `bale.txt` and `ball.txt` files have been moved to the `Folder1` folder:

  ```shell
  ls -l Folder1
  ```

  > The syntax `Folder1` is a relative file path used to tell Linux that we want to see the directory listing in the `Folder1` folder.
* You should see:

  ```shell
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 bale.txt
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 ball.txt
  ```
* List all files currently in the `Documents` folder:

  ```shell
  ls -la
  ```

  > The directory listing of the directory that you are currently in is displayed because no relative path was given after the command `ls` and the options `-la`.
* You should see:

  ```shell
  drwxrwxr-x. 4 studx studx  55 May  3 21:58 .
  drwx------. 3 studx studx 100 May  3 21:31 ..
  -rw-rw-r--. 1 studx studx   0 May  3 21:35 .balehidden
  drwxrwxr-x. 2 studx studx  38 May  3 22:00 Folder1
  drwxrwxr-x. 2 studx studx  38 May  3 22:00 Folder2
  ```
* Move the remaining `.balehidden` file to the `Folder1` directory:

  ```shell
  mv .balehidden Folder1
  ```
* Verify that there are no more file in the `Documents` folder:

  ```shell
  ls -la
  ```
* You should see:

  ```shell
  drwxrwxr-x. 4 studx studx  55 May  3 21:58 .
  drwx------. 3 studx studx 100 May  3 21:31 ..
  drwxrwxr-x. 2 studx studx  57 May  3 22:00 Folder1
  drwxrwxr-x. 2 studx studx  38 May  3 22:00 Folder2
  ```

### Task 3 - Viewing directory contents using `tree` command, relative paths, and absolute paths

#### Step 1 - View directory contents with the `tree` command

* Verify that the `tree` package is installed.

  ```shell
  tree --version
  ```
* You should see:

  ```shell
  tree v1.7.0 (c) 1996 - 2014 by Steve Baker, Thomas Moore, Francesc Rocher, Kyosuke Tokoro
  ```
* If you do not see this output, install tree with the command:

  ```shell
  sudo yum install tree -y
  ```

  > Once it has finished installing you may move to the next step.
* View the current directory structure of all files and directories in the `Documents` directory:

  ```shell
  tree -a
  ```
* You should see:

  ```shell
  .
  ├── Folder1
  │   ├── .balehidden
  │   ├── bale.txt
  │   └── ball.txt
  └── Folder2
      ├── bowl.txt
      └── bull.txt
  ```

  > The period "`.`" at the top of the output represents the current directory `Documents`. Notice that the directory names `Folder1` and `Folder2` are indented the same amount to indicate that they are at the same level of the directory structure; they are both in the `Documents` directory. The files `.balehidden`, `bale.txt`, and `ball.txt` are indented under `Folder1` because they are located in the directory `Folder1`. The files `bowl.txt`, and `bull.txt` are indented under `Folder2` because they are located in the directory `Folder2`.

#### Step 2 - View the contents of a directory using the directory's absolute path

* Change to the `Folder1` directory:

  ```shell
  cd Folder1
  ```
* View the contents of the `Folder1` directory:

  ```shell
  ls -la
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx 57 May  4 04:18 .
  drwxrwxr-x. 4 studx studx 36 May  3 22:04 ..
  -rw-rw-r--. 1 studx studx  0 May  3 21:35 .balehidden
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bale.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 ball.txt
  ```
* View the absolute path to the `Folder1` directory that you are currently in:

  ```shell
  pwd
  ```
* You should see:

  ```shell
  /home/<stud#>/Documents/Folder1
  ```
* View the contents of the `Folder2` directory from the `Folder1` directory using an absolute path:

  ```shell
  ls /home/<your_student#>/Documents/Folder2
  ```
* You should see:

  ```shell
  bowl.txt  bull.txt
  ```

#### Step 3 - View the contents of a directory using the directory's relative path

* View the contents of the `Folder2` directory using a relative path starting from your home folder:

  ```shell
  ls ~/Documents/Folder2
  ```

  > The `~` is a shorthand for the home directory path of the current user. It will change based on what user is executing this command. If student1 is logged in to this Linux shell, then the `~` path equals `/home/student1. Likewise, if student2 is logged in to this Linux shell, then the`\~`path equals`/home/student2.
* You should see:

  ```shell
  bowl.txt  bull.txt
  ```
* View the contents of the `Folder2` directory using a relative path starting from the `Folder1` directory that you are currently in:

  ```shell
  ls ../Folder2
  ```

  > The two periods `..` tell the Linux shell to move up one diretory from your current directory. That puts you in the `/home/<your_student#>/Documents` directory. You then add the `/Folder2` text to tell the Linux shell that you want to view the contents of the `Folder2` folder.
* You should see:

  ```shell
  bowl.txt  bull.txt
  ```

### Task 4 - Manipulating files with the move command

#### Step 1 - Selecting and moving files with wildcards

* Ensure that you are still in the `Folder1` folder:

  ```shell
  pwd
  ```
* You should see:

  ```shell
  /home/<your_student#>/Documents/Folder1
  ```
* Move files from `Folder2` to your current directory using a relative path and wildcards:

  ```shell
  mv ../Folder2/b??l.txt .
  ```

  > The question mark `?` wildcard will match a single character. The period `.` at the end of the command is a shorthand way of writting the current directory that you are in. You could have entered your current working directory `/home/<your_student#>/Documents/Folder1` instead of the period.
* View all the contents of the `Folder1` directory:

  ```shell
  ls -la
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx 89 May  4 05:04 .
  drwxrwxr-x. 4 studx studx 36 May  3 22:04 ..
  -rw-rw-r--. 1 studx studx  0 May  3 21:35 .balehidden
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bale.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 ball.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bowl.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bull.txt
  ```
* Use wildcards to view only specific files in the `Folder1` dictory:

  ```shell
  ls -la b[ao][wl]l.txt
  ```

  > The `b[ao][wl]l.txt` entry tells the Linux shell that you only want to see files that begin with the letter `b`, have either an `a` or an `o` as the second letter, have either a `w` or an `l` as the third letter, and end with the text string `l.txt`.
* You should see:

  ```shell
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 ball.txt
  -rw-rw-r--. 1 studx studx 0 May  3 21:34 bowl.txt
  ```

#### Step 2 - Rename a file using the `mv` command

* Move up one directory from `Folder1` to `Documents`:

  ```shell
  cd ..
  ```
* You can easily verify that you are now in the `Documents` directory by the terminal prompt that is currently displayed:

  ```shell
  [<stud#>@<studx-VM> Documents]$
  ```

  > The `Documents` portion of your terminal prompt will change as you change directories.
* View the contents of the `Documents` directory:

  ```shell
  ls
  ```
* You should see:

  ```shell
  Folder1  Folder2
  ```
* Create the `Folder3` directory:

  ```shell
  mkdir Folder3
  ```
* Verify that the `Folder3` directory was created in the `Documents` directory:

  ```shell
  ls
  ```
* You should see:

  ```shell
  Folder1  Folder2  Folder3
  ```
* Rename the `Folder3` directory to `Jason`:

  ```shell
  mv Folder3 Jason
  ```

  > The move command `mv` creates a new file or directory and deletes the existing file or directory.
* Verify that the `Folder3` directory was renamed to `Jason`:

  ```shell
  ls
  ```
* You should see:

  ```shell
  Folder1  Folder2  Jason
  ```

### Task 5 - Manipulating files with the copy command

#### Step 1 - Copy the contents of one directory to another

* Create the new directory `Backups`:

  ```shell
  mkdir Backups
  ```
* Copy the contents of `Folder1` to the new `Backups` directory:

  ```shell
  cp -a Folder1/. Backups
  ```

  > The archive `-a` option copies all files in a directory. The `/.` indicates the files within the directory. Without the "/." it would copy the folder and the files.
* Verify the contents of the the `Backups` directory:

  ```shell
  ls -la Backups
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx 89 May  4 07:16 .
  drwxrwxr-x. 6 studx studx 64 May  4 07:17 ..
  -rw-rw-r--. 1 studx studx  0 May  4 07:17 .balehidden
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bale.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 ball.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bowl.txt
  -rw-rw-r--. 1 studx studx  0 May  3 21:34 bull.txt
  ```

#### Step 2 - Create a new directory and copy files into it in one step

* Create and copy files into the new directory `Folder1_copy`:

  ```shell
  cp -r Folder1 Folder1_copy
  ```

  > The recursive `-r` option tells the Linux shell to copy all files and directories in the `Folder1` directory into the new `Folder1_copy` directory. The `cp` command would fail without it.
* Verify the contents of the the `Backups` directory:

  ```shell
  ls -la Folder1_copy
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx 89 May  4 07:17 .
  drwxrwxr-x. 7 studx studx 76 May  4 07:17 ..
  -rw-rw-r--. 1 studx studx  0 May  4 07:17 .balehidden
  -rw-rw-r--. 1 studx studx  0 May  4 07:17 bale.txt
  -rw-rw-r--. 1 studx studx  0 May  4 07:17 ball.txt
  -rw-rw-r--. 1 studx studx  0 May  4 07:17 bowl.txt
  -rw-rw-r--. 1 studx studx  0 May  4 07:17 bull.txt
  ```

### Task 6 - Removing files and directories

#### Step 1 - Remove an empty directory

* Remove the `Jason` directory:

  ```shell
  rmdir Jason
  ```

  > The `rmdir` command will only remove empty directories.
* Verify that the `Jason` directory was removed:

  ```shell
  ls
  ```
* You should see:

  ```shell
  Backups  Folder1  Folder1_copy  Folder2
  ```
* Remove the `Folder2` directory:

  ```shell
  rmdir Folder2
  ```

  > The `rmdir` command will only remove empty directories.
* Verify that the `Folder2` directory was removed:

  ```shell
  ls
  ```
* You should see:

  ```shell
  Backups  Folder1  Folder1_copy  
  ```
* Attempt to remove `Folder1`:

  ```shell
  rmdir Folder1
  ```

  > You will receive an error because `Folder1` is not an empty directory. We will addresses how to bypass this in a future step.

#### Step 2 - Use a wildcard to Remove a group of files from a directory

* Change to the `Folder1` directory:

  ```shell
  cd Folder1
  ```
* Use a wildcard to remove all of the files that end with \*.txt:

  ```shell
  rm *.txt
  ```

  > The asterix `*` wildcard will match any character or set of characters including no characters at all.
* Move up one directory level to the `Documents` directory:

  ```shell
  cd ..
  ```
* Attempt to remove `Folder1` again:

  ```shell
  rmdir Folder1
  ```

  > You will still receive an error because `Folder1` is not an empty directory. It still contains the hidden file `.balehidden`

#### Step 3 - Remove a directory that still has files in it

* Remove the `Folder1` directory using the `rm` command with the recursive `-r` option:

  ```shell
  rm -r Folder1
  ```

  > The recursive `-r` option will remove any directories or files that are in the directory you are deleting.
* View the contents of the `Documents` directory to verify that `Folder1` was deleted:

  ```shell
  ls
  ```
* You should see:

  ```shell
  Backups  Folder1_copy
  ```

#### Step 4 Remove the remaining directories in the `Documents` directory

* Use a wildcard with the remove command to remove multiple files or directories:

  ```shell
  rm -r *
  ```

  > Extreme caution should be used with this command because it can easily delete all files and directories in a directory.
* Verify that all files and directories have been removed from the `Documents` directory:

  ```shell
  ls -la
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx   6 May  4 07:28 .
  drwx------. 8 studx studx 249 May  4 07:15 ..
  ```

### Task 7 - Working with symbolic links

A soft link, sometimes called a symbolic link or symlink:

* Points to the location or path of the original file.
* It works like a hyperlink on the internet.
* If the symbolic link file is deleted, the original data remains.
* If the original file is moved or deleted, the symbolic link won’t work.
* A soft link can refer to a file on a different file system.
* Soft links are often used to quickly access a frequently-used file without typing the whole location.
* Syntax for symbolic link:

  ```shell
  ln -s [source] [destination]
  ```

  > The `source` section is the file path of the symbolic link target, the `destination` section is the file path for the pointer that will be used to access the link target.

#### Step 1 - Create a symbolic link

* Change to your `home` directory:

  ```shell
  cd ~/
  ```
* Create the `link_file.txt` file using a relative file path:

  ```shell
  echo "This is text from the linked file" > Documents/link_file.txt
  ```
* Create a symblic link from your user's home directory to the `link_file.txt` file you created:

  ```shell
  ln -s Documents/link_file.txt test_file.txt
  ```
* Verify that you created the symbolic link in the home directory:

  ```shell
  ls -l
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx 27 May  4 11:44 Documents
  lrwxrwxrwx. 1 studx studx 23 May  4 11:51 test_file.txt -> Documents/link_file.txt
  ```

  > Notice that the pointer file `test_file.txt`, and the file it points to `Documents/link_file.txt` are listed in the directory contents on a single line. The directory listing for the symbolic link may appear in light blue, depending on your distribution of Linux.

#### Step 2 - Use a symbolic link to access the data in the target file

* View the contents of the `Documents/link_file.txt` file:

  ```shell
  cat Documents/link_file.txt
  ```

  > This command reads the contents of the `link_file.txt` directly from the `link_file.txt` file.
* You should see:

  ```shell
  This is text from the linked file
  ```
* View the contents of the `test_file.txt` symbolic link:

  ```shell
  cat test_file.txt
  ```

  > This command is reading through the symbolic link which point to the `Documents/link_file.txt` file.
* You should see:

  ```shell
  This is text from the linked file
  ```

#### Step 3 - Remove the target file of a symbolic link

* Change to your `Documents` folder:

  ```shell
  cd Documents
  ```
* Remove the `link_file.txt` file that the symbolic link is pointing to:

  ```shell
  rm link_file.txt
  ```
* Change to your `home` directory:

  ```shell
  cd ..
  ```
* View your `home` directory's file listing:

  ```shell
  ls -l
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx  6 May  4 12:08 Documents
  lrwxrwxrwx. 1 studx studx 23 May  4 11:51 test_file.txt -> Documents/link_file.txt
  ```

  > While the symbolic link is still lited, the target file portion `Documents/link_file.txt` may be listed in red depending on your distribution of linux. This red coloring indicates that the target of the symbolic link no longer exists.

#### Step 4 - Remove a symbolic link

* Change to your `home` directory if you are not still there:

  ```shell
  cd
  ```

  > You can use the command "cd" to return to your home directory from anywhere in the Linux file structure.
* Remove the symbolic link using one of the following methods:

  ```shell
  rm test_file.txt
  ```

  ```shell
  unlink test_file.txt
  ```

### Task 8 - Understanding the Linux directory structure

#### Step 1 - Navigating to the user's `home` directory

* Change to the `home` directory using one of the commands below:

  ```shell
  cd
  ```

  ```shell
  cd ~
  ```

  ```shell
  cd $HOME
  ```

  > Each of these command will take you to the user home directory of the user currently logged in to the Linux shell.
* Create the `home` folder in the current directory:

  ```shell
  mkdir home
  ```

  > The absolute path of the `home` directory that you have just created is /home/<your_student#>/home.
* Change into the `home` directory that you just created:

  ```shell
  cd home
  ```
* Veiw the absolute path of this directory:

  ```shell
  pwd
  ```
* You should see:

  ```shell
  /home/<stud#>/home
  ```

  > The `home` on the left is a default linux directory that all user accounts are stored in, and has the absolute path of `/home`. The `home` on the right is the directory that you just created and it's absolute path is the output that you just received from the `pwd` command.
* Create a file in the `home` directory you created:

  ```shell
  touch my_file.txt
  ```

#### Step 2 - Navigate to the root directory's `home` directory

* Change to the root directory of the Linux computer:

  ```shell
  cd /
  ```

  > The `/` in this command refers to the root level of the Linux directory structure.
* View the contents of the root directory:

  ```shell
  ls
  ```

  > Notice that there is a `home` directory here as well.
* Change to the `home` directory in the root directory:

  ```shell
  cd /home
  ```
* View the contents of the `/home` folder:

  ```shell
  ls -l
  ```
* You should see a listing of all of the user accounts on this Linux computer:

  ```shell
  drwx------.  8 studX studX  249 May  4 05:56 studX
  ```

  > Your ouptput may include different users, but should at least include a user with your student #. The absolute path `/home/<your_student#>` is what is commonly referred to as a user's "home" directory or "personal" directory.
* View the contents of the `~/home` directory you created:

  ```shell
  ls -l ~/home
  ```

  > The `~` is a variable that holds the path to your personal directory. It represents the absolute path: `/home/<your_student#>`.
* You should see the file that you created in the previous step:

  ```shell
  -rw-rw-r--. 1 studx studx 0 May  4 06:10 my_file.txt
  ```

  > In this example, it is very important to understand the difference between the directories `/home` and `~/home`. The `/home` directory is a directory that is created by the Linux operating system to hold the user folders of users created on the Linux operating system. The `~/home` folder is an arbitrary folder that you created for this lab and is not present unless it is created.