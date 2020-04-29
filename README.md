# linux-tools

A list of commands for tasks and troubleshooting, commands will work with Debian and Ubuntu distributions.

Be sure to read the flags underneath each command. It may not always be nesscesray to use all of the flags. Only use the options which apply to the use case.

## **Contents**

[Managing Files](#managing-files)

[Containers](#containers)

[Permissions](#Permissions)

- [Users](#users)
- [Groups](#groups)

## Storage Devices

## Networking

### Network processes

##### Check what proccess ID running on what port

        sudo netstat -ltnp

- l - display only listening sockets
- t - display tcp connection
- n - display addresses in a numerical form
- p - display process ID/ Program name

## SSH

##### Secure Copy to remote machine

        scp /path/to/file user@target:/path/to/target

##### Start ssh agent

        eval 'ssh-agent -s'

### Logging

##### Check live log files

        sudo tail n10 -f /path/to/file

- n - number of lines
- f - keep file open

##### Run a script on remote ssh

        ssh root@MachineB 'bash -s' < local_script.sh

## Containers

### Docker

##### List images

        docker images ls

##### List containers

        docker container ls -a

- a - list all containers, without the flag it only shows active containers

##### Remove image

        docker rmi image_name

##### Remove all containers

        docker container prune

### Start a container

##### Build contianer from local Dockerfile

        docker build -t image_name .

- t - name you wish to give the container

##### Run container after building it

        docker run -d --name container_name -p 80:80 image_name

- d -
- name - give container name or a default one will be created
- p - assign port mapping first is host second is container port

## Managing Files

##### Make new directory

    mkdir /path/to/dir

##### Viewing contents of files

    cat /path/tofile            - Display the contents of file
    more /path/tofile           - Browse through text file
    less /path/tofile           - More features than more
    head -n10 /path/tofile       - Output the beggining or top portion of file
    tail -n10 /path/tofile       - Output the ending or bottom portion of the file

- number of lines to be displayed when using the head or tail command

##### Grep for contents

    cat /etc/shadow | grep myuser

- this will only show information regarding the user which you grepped for

##### Remove directory

    rm -rf /path/to/dir

- r - recursively remove all files
- f - forcefully (be carefull when using this variant)

##### Create file

    touch -p /path/to/file

- p - creates all parent folders if they dont exist

## Permissions

### File Permisions

| Symbol | Type          |
| ------ | ------------- |
| -      | Regular file  |
| d      | Directory     |
| l      | Symbolic Link |
| r      | Read          |
| w      | Write         |
| x      | Execute       |

##### Permision Catagories

| Symbol | Type  |
| ------ | ----- |
| u      | User  |
| g      | Group |
| o      | Other |
| a      | All   |

- Every user on Linux system belongs to one Group
- Users can belong to many groups
- Groups are use to organize Users
- The "groups" comamand displays a user's group
- You can also use id -Gn

**_Permsision String_**
First

- Type
  Next 3
- User
  Next 3
- Group
  Last 3
- Other

###### eg.

    "d" "rwx" "r-x" "r-x"

- Permision type is also called Mode

**_Changing permsion type_**

##### Change mode comamand

    chmod

##### Change file group

    chgrp

##### Read, write, executre

| r   | w   | x   |
| --- | --- | --- |
| 4   | 2   | 1   |
| U   | G   | O   |

##### Comparison

| Compared |     |
| -------- | --- |
| Symbolic | rwx | r-x | r-- |
| Binary   | 111 | 101 | 100 |
| Decimal  | 7   | 5 | 4 |

##### Overview

| Symbol      | Octal | Permision                                                  |
| ----------- | ----- | ---------------------------------------------------------- |
| -rwx------  | 700   | Only owner can read write and Execute                      |
| -rwxr-xr-xr | 755   | Everyone on system can execute but only user can edit file |
| -rw-rw-r--  | 664   | User read and write, Group read and write, other only read |
| -rw-rw----  | 660   | Only user and group can read and write file                |
| -rw-r--r--  | 664   | User read and write, group and other only read             |

- Permisions on a directory can effect files in the directory
- If the file permsiosns look correct, check directory Permisions
- Work your way up to root

- File creation mask determines defualt permissions
- If no mask were used permissions would be:
  - 777 for directories
  - 666 for files

#### The umask command

    umask -S mode

- this sets mode for file creation and allows groups specific Permisions
- setting the umask means subtrating values from group permsions to equal actual permsions
- Sets the file creation mask to mode, if given
- Use -S to for symbolic notation

| Permision            | Directory | File |
| -------------------- | --------- | ---- |
| Base Permission      | 777       | 666  |
| Subtract umask       | -022      | -022 |
| Creations Permsision | 755       | 644  |

**_Common umask modes_**

- 022
- 002
- 077
- 007

##### Resulting permsions from umask mode

| Octal | Binary | Dir  | perms |
| ----- | ------ | ---- | ----- |
| 0     | 0      | rwx  | rw-   |
| 1     | 1      | rw-  | rw-   |
| 2     | 10     | r-x  | r-x   |
| 3     | 11     | r--  | r--   |
| 4     | 100    | -wx  | -w-   |
| 5     | 101    | -w-  | -w-   |
| 6     | 110    | --xr | ---   |
| 7     | 111    | ---- | ---   |

##### Special Modes

    setuid
    setgid
    sticky

- umask 0022 is the same as umask 022
- chmod 0644 is the same as chmod 644
- The special modes are

### Users

#### Folders of interest

    /etc/password
    /etc/shadow

- the password folder contains all user info
- the shadow folder contains password information of users

##### Swithing user

    su username

- exclude the username if you wish to swith to root user

##### Add new user

    sudo adduser username

##### Delete user

    sudo -r userdel username

- r - Remove all user data
- If not using r flag the user directory is not deleted. Be sure to move old user home directory to the archive

##### Change user home directory

    sudo usermod -d /new/dir username -m

##### Change username

    sudo usermod -l oldname newname

##### Lock user accout

    sudo passwd -l username

- l - lock account
- u - unlock account

##### Check expiration date of user password

    sudo chage -l username

- l - list details
- d 0 - set number of days to expire to 0 this will disable account
- M 90 - set number of days after which a user needs to replace their password
- m - set minimum number of days for password to be active, good if someone keeps changing their password back

#### Plugable Authentication Module

Install this application to set minimum requirements for passwords. This increases password strength within the machine

### Groups

#### Folders of interest

    /etc/group

- this file shows all groups on the machine

##### Sudoers folder

    /etc/sudoers
    sudo EDITOR=vim visudo

- this file contains all the users who have sudo privelages, edit this folder with the following command
- this allows for the detection of any mistakes made in the sudoers file

##### Create new group

    sudo groupadd newgroup

##### Delete group

    sudo groupdel

##### Add user to secondary group

    sudo usermod -aG group user

- a - append a secondary group, if you dont add this flag it will replace all current groups
- G - This states a secondary group to add the user to
- aG sudo - this adds a user to the sudoers group. this allows the user to use the sudo command

##### Change user primary group

    sudo usermod -g groupname username

##### Remove user from group

    sudo gpasswd -d username grouptoremove

## Software Packages

## Processes

## Shell Scripting
