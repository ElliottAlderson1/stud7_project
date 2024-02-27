# Lab 04 Search, Extract and Archive Data

## In this lab you will cover

* [Accessing your student VM from the Command prompt using SSH](#task-1-accessing-your-student-vm-from-the-command-prompt-using-ssh)
* [Using grep to filter through data](#task-2-using-grep-to-filter-through-data)
* [Using grep with other Linux shell commands](#task-3-using-grep-with-other-linux-shell-commands)
* [Archiving and extracting files with the ](#task-4-archiving-and-extracting-files-with-the-tar-command)`tar` command

### Resources

* See instructional material

### Task 1 - Access your student VM from the Command prompt using SSH

#### Step 1 - Open the Command Prompt program on Windows

#### Step 2 - Remotely connect to your Student Virtual Machine(VM) using SSH

### Task 2 - Using grep to filter through data

#### Step 1 - Create files and directories for the lab

* Change to your `home` directory using one of the following commands if you are not already there:

  ```shell
  cd
  ```

  ```shell
  cd ~
  ```
* Create a new directory named `Regex_practice`:

  ```shell
  mkdir Regex_practice
  ```
* Change to the new `Regex_practice` directory:

  ```shell
  cd Regex_practice
  ```
* Copy the `words` file from the `/usr/share/dict/` directory to the `Regex_practice` directory:

  ```shell
  cp /usr/share/dict/words .
  ```

  > The words file is a very long text listing of english dictionary words.
* Verify that the file was copied:

  ```shell
  ls
  ```
* You should see:

  ```shell
  words
  ```

  > The `words` file contains thousands of words that we will use to explore the grep command.
* View the number of line entries in the `words` file:

  ```shell
  wc -l words
  ```
* You should see:

  ```shell
  479828 words
  ```

  > Your number of lines may be slightly larger depending on when it was updated. I do not recommend viewing the contents of this file with the "cat" command.

#### Step 2 - Simple grep and line numbers

* Find all of the words in the `words` file that have the text `dogs` in them:

  ```shell
  grep 'dogs' words
  ```
* You should see several words listed that all have the text `dogs` in them.
* View the line number of the lines that match a grep command:

  ```shell
  grep -n 'dogs' words
  ```

  > The '-n' grep option displays the line number for the matching line.

#### Step 3 - Extended grep `beginning` and `end` of line markers

* Use `regular expressions` to Find all of the words in the `words` file that start with the text `dogs`:

  ```shell
  grep -E '^dogs' words
  ```

  > The `-E` grep option tells grep to read regular expressions (regex). The `^` character indicates the beginning of a line.
* Use `regular expressions` to Find all of the words in the `words` file that end with the text `dogs`:

  ```shell
  grep -E 'dogs$' words
  ```

  > The `$` character indicates the end of a line.

#### Step 4 - Extended grep `wildcard`, `escape`, `pipe`, and `range`

* Use `regular expressions` with period in the search text as a wildcard:

  ```shell
  grep -E 'd.gs' words
  ```

  > The period `.` character indicates a character that can be anything. Notice in the command's output that there are words containing the text `dogs`, but also the text `dugs`, `degs`, `dags`, and `digs`.
* Use `regular expressions` with the escape `\` character to treat a special character as simple text to search for:

  ```shell
  grep -E '\.' words
  ```

  > This grep command will show all of the lines that have a period in them. If the escape character were not used, grep would have read the period as a wildcard character.
* Use `regular expressions` with the pipe `|` character as an `OR` between various search criteria:

  ```shell
  grep -E 'dogs|cats' words
  ```

  > This grep command will return all lines that contain the text `dogs` or `cats`.
* Use `regular expressions` with the bracket `[` `]` characters and a range to search a range of possible text:

  ```shell
  grep -E '[1-3]' words
  ```

  > This grep command will return all lines that contain the text `1`, or `2`, or `3`.

### Task 3 - Using grep with other Linux shell commands

#### Step 1 - Redirection of an extended grep output to a file

* `Redirect` the standard output of grep regular expressions to the file `results`:

  ```shell
  grep -E '2' words > results
  ```
* View the contents of the `results` file:

  ```shell
  cat results
  ```

  > You should see lines that all contain the number `2`.

#### Step 2 - Filtering outputs of commands using `pipe` and extended grep output to a file

* Pipe the output of an `ls` command to a `grep` command to find specific files or directories:

  ```shell
  ls /etc | grep -E 'sys'
  ```

  > The `ls` command outputs the directory listing for the /etc directory, then `grep` displays all of the files or directories that have the text `sys` in their name.

### Task 4 - Archiving and extracting files with the `tar` command

The Tape Archive (tar) command is a useful way to bundle and compress files into a single directory item. Useful commands:

* `cf`: Creates a tar file.
* `tf`: Looks at the contents of a tar file.
* `rf`: Adds a file to a tar file.
* `xf`: Extracts a non-compressed tar file.
* `czf`: Creates a compressed tar file using gzip (.tgz file extension).
* `xzf`: Extracts a compressed tar file.

#### Step 1 - Create a `tar` archive without any compression

* Change to the user `home` directory:

  ```shell
  cd
  ```
* Create a new archive file `archive.tar` and archive the `Regex_practice` directory into the `archive.tar` file:

  ```shell
  tar cf archive.tar Regex_practice
  ```
* Verify that the tar file `archive.tar` was created:

  ```shell
  ls -l
  ```

  > You should see the file `archive.tar` listed.

#### Step 2 - Viewing the contents of a `tar` file

* View the contents of the `archive.tar` file:

  ```shell
  tar tf archive.tar
  ```
* You should see:

  ```shell
  Regex_practice/
  Regex_practice/words
  Regex_practice/results
  ```

#### Step 3 - Adding a file to an existing `tar` file

* Make a new file named `newfile.txt`:

  ```shell
  touch newfile.txt
  ```
* Add the `newfile.txt` file to the `archive.tar` archive:

  ```shell
  tar rf archive.tar newfile.txt
  ```
* Verify that the `newfile.txt` file was added to the `archive.tar` archive:

  ```shell
  tar tf archive.tar
  ```
* You should see:

  ```shell
  Regex_practice/
  Regex_practice/words
  Regex_practice/results
  newfile.txt
  ```

#### Step 4 - Create a compressed tar file

* Create a new compressed tar file `compressed.tgz` and archive the `Regex_practice` directory into the `compressed.tgz` file:

  ```shell
  tar czf compressed.tgz Regex_practice
  ```
* Verify that the compressed tar file `compressed.tgz` wsa created:

  ```shell
  ls -l
  ```
* You should see the following two files in your directory listing:

  ```shell
  -rw-rw-r--. 1 student1 student1 4966400 May  5 14:31 archive.tar
  -rw-rw-r--. 1 student1 student1 1476577 May  5 14:35 compressed.tgz
  ```

  > Notice that the `compressed.tgz` file is around one quarter the size of the un-compressed `archive.tar` file.
* Create a compressed tar file `archive.tgz` from the un-compressed `archive.tar` file:

  ```shell
  tar czf archive.tgz archive.tar
  ```
* Verify that the `archive.tgz` compressed archive was created:

  ```shell
  ls -lh
  ```

  > The `-h` option in the `ls` command makes the output "human readable" which displays file sizes in kilobytes, megabytes, and gigabytes. The `-h` option may not be available depending on the Linux distribution you are using.
* You should see the following two files in your directory listing:

  ```shell
  -rw-rw-r--. 1 student1 student1 4.8M May  5 14:31 archive.tar
  -rw-rw-r--. 1 student1 student1 1.5M May  5 14:55 archive.tgz
  ```

  > Notice that the compressed `archive.tgz` is 1.5 megabytes while the un-compressed `archive.tar` is 4.8 megabytes.

#### Step 5 - Extract the contents of a compressed `tar` file

* From the user `home` directory, create a new directory named `temp`:

  ```shell
  mkdir temp
  ```
* Move the compressed `archive.tgz` file to the new `temp` directory:

  ```shell
  mv archive.tgz temp/
  ```
* Change to the new `temp` directory:

  ```shell
  cd temp
  ```
* View the contents of the `temp` directory:

  ```shell
  ls -l
  ```

  > Notice that the compressed file is the only file in the directory.
* Extract the compressed contents of the `archive.tgz` file:

  ```shell
  tar xzf archive.tgz
  ```
* View the contents of the `temp` directory with the human readable option `-h`:

  ```shell
  ls -lh
  ```
* You should see:

  ```shell
  -rw-rw-r--. 1 student1 student1 4.8M May  5 14:31 archive.tar
  -rw-rw-r--. 1 student1 student1 1.5M May  6 08:12 archive.tgz
  ```

  > This archive file is a relatively small file, but image if it were several gigabytes in size and you wanted to transfer it across a relatively slow internet connection, compressed tar files would be one solution for transferring a large file more quickly.
* 