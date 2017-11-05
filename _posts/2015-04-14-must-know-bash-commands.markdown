---
layout: post
date: "2015-04-14"
title: "Must know bash commands or hackers cheat sheet"
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
---

## What are the must know Bash commands?What are tricky bash commands that I consider useful:

Basic ls, rm, grep commands
---------------------------

```bash
ls file # does the file exist?
ls -l file # info about file
ls -lt  #  list files in time order with info
ls -ltr  #  list files in reverse time order with info
ls -a # list files including hidden files
ls - R $ show dirs and subdirs

rm !(u|p) # delete everything but
```

Creating a symbolic link:

```bash
ln -s original linkname
```

Grepping:

```bash
ls | grep something # prints something, that matches something
less file1 | grep something # looks for something in the file
grep "hi there" file1 # look for hi there in the file1
ls | grep something | xargs rm # removes everything that matches something in the directory
```

Grepping `str1` or/and `str2`:

```bash
egrep -i "str1|str2" file # str1 or str2
egrep -i "str1.\*str2" file # str1 and str2
```

How to find files?
------------------

Here are a few ways to use find:

```bash
$ find ./ -name 'name.\*'
```

./

```bash
Start searching from the current directory (i.e ./ directory)
```

-name

```bash
Given search text is the filename rather than any other attribute of a file
```

'name.\*'

```bash
Search text that we have entered. Always enclose the filename in single quotes.. why to do this is complex.. so simply do so.
```

```bash
$ find /home/david -name 'index\*'
$ find /home/david -iname 'index\*'
```

The 1st command would find files having the letters index as the
beginning of the file name. The search would be started in the directory
/home/david and carry on within that directory and its subdirectories
only. The 2nd command would search for the same, but the case of the
filename wouldn't be considered. So all files starting with any
combination of letters in upper and lower case such as INDEX or indEX or
index would be returned.

Parsing through text files using sed/awk
----------------------------------------

Sed:

Replace `foo` with `bar`:

```bash
sed 's/foo/bar/'
```

Replace `Word1` with `Word2`, `-n` - gets rid of unnecessary stuff

```bash
sed -n '/Word1/Word2/' fromfile.txt > newfile.txt
```

Deleting lines:

```bash
sed -i '/^\s*$/d'  fromfile.txt # delete empty lines fromfile.txt (modifies the file)
sed -i '1d' filewithfirstline.txt # deletes first line of a file
sed -i '1i\\' filewithfirstline.txt # adds empty first line to file
```

```bash
sed -e '/^$/d' $filename
# The -e option causes the next string to be interpreted as an editing instruction.
#  (If passing only a single instruction to sed, the "-e" is optional.)
#  The "strong" quotes ('') protect the RE characters in the instruction
#+ from reinterpretation as special characters by the body of the script.
# (This reserves RE expansion of the instruction for sed.)
#
# Operates on the text contained in file $filename.


8d  #Delete 8th line of input.
/^$/d   #Delete all blank lines.
1,/^$/d #Delete from beginning of input up to, and including first blank line.

/Jones/p  #  Print only lines containing "Jones" (with -n option).
s/Windows/Linux/   # Substitute "Linux" for first instance of "Windows" found in each input line.
s/BSOD/stability/g  #Substitute "stability" for every instance of "BSOD" found in each input line.

s/ \*$// #Delete all spaces at the end of every line.
s/00\*/0/g   #Compress all consecutive sequences of zeroes into a single zero.

echo "Working on it." | sed -e '1i How far are you along?'  #Prints "How far are you along?" as first line, "Working on it" as second.
5i 'Linux is great.' file.txt   #Inserts 'Linux is great.' at line 5 of the file file.txt.
/GUI/d  #Delete all lines containing "GUI".
s/GUI//g    #Delete all instances of "GUI", leaving the remainder of each line intact.
```

Aliasing
--------

The alias command makes it possible to launch any command or group of
commands (inclusive of any options, arguments and redirection) by
entering a pre-set string (i.e., sequence of characters). In other words
you find some command that you use a lot, say go to certain directory,
or ssh to certain server give it a nickname (alias) and use it.

For example I know that I go to certain folder a lot, it has all my
libraries in it -
/Dropbox/Lammps\_simulation/my\_git\_repo/polymer\_simulation/CreateMelt
So I just add the following line to \~/.bashrc (\~/bash\_profile),
source it source \~/.bashrc and use it.

```bash
alias createmelt='cd ~/Dropbox/Lammps_simulation/my_git_repo/polymer_simulation/CreateMelt'
```

For now on, whenever I need to go to the
/Dropbox/Lammps\_simulation/my\_git\_repo/polymer\_simulation/CreateMelt
directory I use

```bash
$ ~/blog @ Vasiliys-MacBook-Pro (bazilevs)
$ => pwd
/Users/bazilevs/blog

$ ~/blog @ Vasiliys-MacBook-Pro (bazilevs)
$ => createmelt

$ ~/Dropbox/Lammps_simulation/my_git_repo/polymer_simulation/CreateMelt @ Vasiliys-MacBook-Pro (bazilevs)

$ => pwd
    /Users/bazilevs/Dropbox/Lammps_simulation/my_git_repo/polymer_simulation/CreateMelt


source
```

```bash
# very handy tool to go any number of directories up
up(){
  local d=""
  limit=$1
  for ((i=1 ; i <= limit ; i++))
    do
      d=$d/..
    done
  d=$(echo $d | sed 's/^\///')
  if [ -z "$d" ]; then
    d=..
  fi
  cd $d
}

# very handy tool to extract files
extract () {
    if [ -f $1 ] ; then
      case $1 in
        \*.tar.bz2)   tar xjf $1     ;;
        \*.tar.gz)    tar xzf $1     ;;
        \*.bz2)       bunzip2 $1     ;;
        \*.rar)       unrar e $1     ;;
        \*.gz)        gunzip $1      ;;
        \*.tar)       tar xf $1      ;;
        \*.tbz2)      tar xjf $1     ;;
        \*.tgz)       tar xzf $1     ;;
        \*.zip)       unzip $1       ;;
        \*.Z)         uncompress $1  ;;
        \*.7z)        7z x $1        ;;
        \*)     echo "'$1' cannot be extracted via extract()" ;;
         esac
     else
         echo "'$1' is not a valid file"
     fi
}
```

Github basic commands:
----------------------

I assume one does have a working account, then everything is pretty
straightforward:

```bash
#initialize this directory to be github directory
git init .
# when you have files to push
git add .
# adding information on the commit
git commit -m "i added a lot of stuff"
git push origin master
```

When you are cloning a directory that it gets initialized automatically.
