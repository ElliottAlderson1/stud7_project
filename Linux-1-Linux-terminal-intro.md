# Linux terminal - Grasshopper

## Basic skillz

* [Accessing VM over SSH](#1-Accessing-VM-over-SSH)
* [Creating Directories and files in the Linux shell](#task-2-creating-directories-and-files-in-the-linux-shell)
* [Manipulate text using the ](#task-3-manipulating-text-using-the-echo-cat-and-redirection-commands)`echo`, `cat`, and redirection commands
* [Handling special characters with quotations and escape characters](#task-4-handling-special-characters-with-quotations-and-escape-characters)
* [Man Pages Documentation](#task-5-accessing-program-documentation-from-the-linux-shell)

### Resources

* See instructional material

### Task 1 - Accessing your student VM from the Command prompt using SSH

#### Step 1 - Open the Command Prompt program on Windows

* Click in the Windows search bar at the bottom left of your student laptop, type `Command Prompt`, and click on the Command Prompt program to open it.
* Once the Command Prompt program opens, click inside it's window.

#### Step 2 - Remotely connect to your Student Virtual Machine(VM) using SSH

* Enter the SSH commands into the Command Prompt program using `your` student number and student VM IP address.

  ```shell
  ssh <stud#>@<student_vm_ip_address>
  ```

> If you are prompted with "Are you sure you want to continue connecting (yes/no/\[fingerprint\])?" enter yes

* Press `enter`.
* When prompted, enter your student password for the student VM

  ```shell
  <stud#>@<student_vm_ip_address> password:
  ```
* Press `enter`.
* If login was successful, you will see a new prompt similar to the one below:

  ```shell
  [<stud#>@<student_vm> ~]$
  ```

### Task 2 - Creating directories and files in the Linux shell

#### Step 1 - Create directories in the Linux shell

* Create the new folder `Documents` using the `make directory` command `mkdir`:

  ```shell
  mkdir Documents
  ```
* Create the new folder `Configuration_files` using the `make directory` command `mkdir`:

  ```shell
  mkdir Configuration_files
  ```
* Change to the directory `Documents` using the `change directory` command `cd`:

  ```shell
  cd Documents
  ```

### Step 2 - View the path to your current working directory

* Verify that you are in the `Documents` directory using the `present working directory` command `pwd`:

  ```shell
  pwd
  ```
* You should see:

  ```shell
  /home/<stud#>/Documents
  ```

#### Step 3 - Create files from the Linux shell

* Create three new files using the `touch` command:

  ```shell
  touch file_1.txt
  touch file_2.txt
  touch file_3.txt
  ```
* View the contents of the `Documents` directory using the `list` command `ls`:

  ```shell
  ls
  ```
* You should see:

  ```shell
  file_1.txt  file_2.txt  file_3.txt
  ```

#### Step 4 - View a detailed file listing of a directory

* View the contents of the `Documents` directory in more detail by adding the `-l` and `-a` options to the `list` command `ls`:

  ```shell
  ls -la
  ```

  > The list individual line option: `-l`, and the show all files option: `-a` options were combined in the single option `-la`. This results in an output that shows all of the files including the hidden ones in a directory as one file per line of output.
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx  48 May  3 16:49 .
  drwx------. 8 studx studx 245 May  3 16:43 ..
  -rw-rw-r--. 1 studx studx 0 May  3 16:49 file_1.txt
  -rw-rw-r--. 1 studx studx 0 May  3 16:49 file_2.txt
  -rw-rw-r--. 1 studx studx 0 May  3 16:49 file_3.txt
  ```

  > Your output will be slightly different, but the names of the files will be the same.
* Notice that the file size of file_1 is currently 0. It is the value before the date of creation, which is May 3rd in this example:

  ```shell
  -rw-rw-r--. 1 studx studx 0 May  3 16:49 file_1
  ```

### Task 3 - Manipulating text using the `echo`, `cat`, and redirection commands

#### Step 1 - Output text to the Linux shell using the echo command

* Print some text to the terminal window using the `echo` command:

  ```shell
  echo This is some text
  ```
* You should see:

  ```shell
  This is some text
  ```

#### Step 2 - Redirect output to write to a file

You can redirect the output of one command to another command or to a file using an output redirection operator. The `>` redirection operator will replace the contents of the target file. A `>>` redirection operator will add/append the data to the end of the target file.

* Add some contents to file 1:

  ```shell
  echo "This is some text in file 1" > file_1.txt
  ```

  > The `>` command will overwrite any contents currently in the file.

#### Step 3 - Display the contents of a file using the cat command

The `cat` command will display the contents of the file it is executed on.

* View the contents of the `file_1.txt` file:

  ```shell
  cat file_1.txt
  ```
* You should see:

  ```shell
  
  This is some text in file 1
  ```

#### Step 4 - Redirect output to append to a file

* Append some text to the end of file 1:

  ```shell
  echo "This is some text appended to the end of the file" >> file_1.txt
  ```
* View the contents of the `file_1.txt` file:

  ```shell
  cat file_1.txt
  ```
* You should see:

  ```shell
  This is some text in file 1
  This is some text appended to the end of the file
  ```
* View the contents of the `Documents` directory again:

  ```shell
  ls -la
  ```
* You should see:

  ```shell
  drwxrwxr-x. 2 studx studx  48 May  3 16:49 .
  drwx------. 8 studx studx 245 May  3 16:43 ..
  -rw-rw-r--. 1 studx studx  78 May  3 16:49 file_1.txt
  -rw-rw-r--. 1 studx studx   0 May  3 16:49 file_2.txt
  -rw-rw-r--. 1 studx studx   0 May  3 16:49 file_3.txt
  ```

  > Notice that the size of the `file_1.txt` file has increased since you added the text `This is some text in file 1` and `This is some text appended to the end of the file` to the file.

### Task 4 - Handling special characters with quotations and escape characters

#### Step 1 - Using quotations to handle special characters

* Try to execute the following `echo` command:

  ```shell
  echo This is bad; text
  ```

  > This `echo` command fails because linux sees the `;` character as a seperator between two commands. Linux executes the command `echo This is bad` and then tries to execute the command `text`. `text` is not a command, so it fails.
* Use quotes to make linux ignore special characters like `;`

  ```shell
  echo "This is bad; text"
  ```
* You should see:

  ```shell
  This is bad; text
  ```

  > This time the `echo` command executes properly because the quotation marks tell Linux to treat the special character `;` as normal semi-colon, not a Linux special character seperating two commands.

#### Step 2 - Using the escape character to handle special characters

* Use the escape special character `\` before a special character to ignore a special characters like `;`

  ```shell
  echo This is bad\; text
  ```
* You should see:

  ```shell
  This is bad; text
  ```

  > When using the escape character, it is not necessary to use quotation marks.

Multiple commands can be seperated by a semi-colon `;`

* Use a semi-colon to execute a `pwd` command and a `ls -la` with only one line:

  ```shell
  pwd ; ls -la
  ```
* You should see:

  ```shell
  /home/<studx>/Documents
  total 0
  drwxrwxr-x. 2 studx studx  60 May  3 17:31 .
  drwx------. 8 studx studx 245 May  3 16:43 ..
  -rw-rw-r--. 1 studx studx  78 May  3 17:31 file_1.txt
  -rw-rw-r--. 1 studx studx   0 May  3 17:31 file_2.txt
  -rw-rw-r--. 1 studx studx   0 May  3 17:31 file_3.txt
  ```

  > Notice that the `pwd` command output was listed first, then the ouptut from the `ls -la` command was displayed.

### Task 5 - Accessing program documentation from the Linux shell

#### Step 1 - View program documentation on Linux `man` pages

`Man` or `Manual` pages contain information about a particular command and may be viewed with the syntax: `man <name_of_command>`.

* View the `man` page for the `ls` command:

  ```shell
  man ls
  ```
* You should see:

  ```shell
  NAME
         ls - list directory contents
  
  SYNOPSIS
         ls [OPTION]... [FILE]...
  
  DESCRIPTION
         List  information about the FILEs (the current directory by default).  Sort entries alphabetically if
         none of -cftuvSUX nor --sort is specified.
  
         Mandatory arguments to long options are mandatory for short options too.
  
         -a, --all
                do not ignore entries starting with .
  ...
  ```
* Scroll down by pressing the `Page Down` key or the `down arrow` until you find the entry for the `-l` option.
* You should see:

  ```shell
  ...
  -1     list one file per line.  Avoid '\n' with -q or -b
  ...
  ```

  > The exact wording of the man page may vary from one Linux distribution to another.
* Exit the `man` page by pressing the `q` key.

#### Step 2 - View program documentation on Linux `info` pages

`Info` pages are similar to `man` but usually have more detailed information about a command than the `man` page does. `Info` pages also have a different navigation structure.

* View the `info` page for the `ls` command:

  ```shell
  info ls
  ```

  > Navigate through the pages with the following commands: `p` previous page, `n` next page, `q` exit.