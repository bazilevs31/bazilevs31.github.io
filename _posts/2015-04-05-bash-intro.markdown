---
layout: post
date: '2015-04-05 16:31'
title: 'How to work with bash terminal or introduction to hacking'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
---

## How to work in Unix Bash?

  Imagine you have 10 files with two columns of data in each file. You want to load them and take an average of all second columns. Sounds like an easy Excell program. But it is not. When you have not 10 but 10 000 files, it will take ages for Excell to load them, also Excell is not free and is not installed everywhere. Fortunatelly there is an amazing tool that you may use : Bash shell. It is available by default in Linux/Mac OS , it can be installed via CygWin on Windows machine. It takes a couple of hours to learn, couple of days/weeks to master, and it will save you couple of years in future. Interested? So lets get started.

  __Plan:__

  * What we need to know
  * Elementary operations
  * Secrets of mastering bash
  * Useful links
  * Here is how the linux console looks on Ubuntu 14.04

## Elementary Introduction


In a nutshell we see computer memmory as a bunch of files and folders.
When we use computer in our daily routine we navigate between different
files and folders. Even when we surf the web we navigate between
webpages which are the files on remote servers. So the most “intersting”
command would be ls

1.  List all files and folders

```bash
$ ls
empty_file1  empty_folder1
Here empty_folder1 is a directory, and empty_file1 is a text file.
```

2.  Navigate to directory

```bash
$ cd empty_folder1
Now we are in empty_folder1 directory. To test that we use
```

3.  Show current directory

```bash
$ pwd
/home/vasiliy/tests
```

To navigate back in the parent directory we use cd .. Now lets use this
4 commands together

```bash
vasiliy@vasiliy-office:~/tests/empty_folder1$ pwd
/home/vasiliy/tests/empty_folder1
vasiliy@vasiliy-office:~/tests/empty_folder1$ cd ..
vasiliy@vasiliy-office:~/tests$ pwd
/home/vasiliy/tests
vasiliy@vasiliy-office:~/tests$ ls
empty_file1  empty_folder1
vasiliy@vasiliy-office:~/tests$ cd empty_folder1/
```

To create a file we may use any text editor we like. I myself am a big
fan of Sublime-Text.. To copy file `file1` to `file2` we use cp `file1`
`file2` command. To remove files we use rm filename command. To create
an empty directory we use mkdir mynewcooldirectory.

4.  Creating/Copying/Removing files

```bash
$ ls
$ subl empty_file2 # edit file and save it
$ ls
empty_file2
$ cp empty_file2 empty_file2_copy
$ ls
empty_file2 empty_file2_copy
$ rm empty_file2_copy #delete the copy file
$ ls
empty_file2
```

If we want to rename file, we could copy it and consequently remove it,
or use a move mv `file1` `file2` command which combines this procedures.

Must-know commands
------------------

  -----------------------------------------------------------------
  So here is a little cheat sheet of required operations:

  1\. LiSt files and folders ls

  2\. Current directory pwd

  3\. Navigate to directory(ChangeDirectory) cd foldername, cd ..

  4\. CoPy/MoVe/ReMove files `cp file1 file2`, `mv file1 file2`,
  `rm file1`.

  A Project

  =========
  -----------------------------------------------------------------

__Problem:__

Create a binary bash script(a file that can be executed by linux shell)
that:

1.  lists all files in the directory
2.  creates a set of files file1, file2, file3, … file10
3.  copies file1 to file1\_copy, list all files
4.  removes file1, list all files
5.  removes all files in the directory

Additional information:

The files on computer are of two types human-readable - ASCII, and
computer readable - binary. We (humans:)) can’t understand computer code
because it is all 1s and 0s, at the same time computers can’t understand
ASCII files unless we explain them how to transfer ASCII files into
binary ones. To simplify the procedure of running the program we can
tell our system that this program is a bash program this is being done
by : `#!/bin/bash`. Then tell the system that we may run this as an
executable we do `chmod +x bashscript.sh` To create dummy empty file one
may use touch `file1` command. To delete all files in the directory we
use `*` wildcard so to delete all files it will be `rm *.` But we need
to be very careful because it will delete all files in directory, so
make sure that they match the pattern that you are using to delete
files.

algorithm:

1.  list all files in the directory ls
2.  create a list with for command
3.  `cp file1 file1_copy`, `ls`
4.  `rm file1`, `ls`
5.  delete all files in the directory `rm *`

Solution:

```bash
#!/bin/bash
echo "listing the files in the directory" # use echo command to print information in the terminal
ls
echo "starting the loop"
for (( i = 1; i <= 10; i++ )); do
    echo "doing loop on iteration # = $i"
    touch file$i
done
echo "done with the loop"
ls
echo "copy ``file1`` in file1_copy"
cp file1 file1_copy
ls
echo "remove file1"
rm file1
ls
```

OUTPUT:

```bash
$~/tests$ bash simple_prog.sh
listing the files in the directory
simple_prog.sh
starting the loop
doing loop on iteration # = 1
doing loop on iteration # = 2
doing loop on iteration # = 3
doing loop on iteration # = 4
doing loop on iteration # = 5
doing loop on iteration # = 6
doing loop on iteration # = 7
doing loop on iteration # = 8
doing loop on iteration # = 9
doing loop on iteration # = 10
done with the loop
file1  file10  file2  file3  file4  file5  file6  file7  file8  file9  simple_prog.sh
copy file1 in file1_copy
file1  file10  file1_copy  file2  file3  file4  file5  file6  file7  file8  file9  simple_prog.sh
remove file1
file10  file1_copy  file2  file3  file4  file5  file6  file7  file8  file9  simple_prog.sh
```

If we change the mode to executable `chmod +x simple_prog.sh`, then we
may run the program the following way `./simple_prog.sh`

Secrets of mastering Bash

There is only one secret of becoming good with bash - practice, you may
need to spend a little bit of energy now, but it will save you tons of
time in the future.

Useful links

