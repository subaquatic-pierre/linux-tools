# linux-tools

A list of commands for tasks and troubleshooting, commands will work with Debian and Ubuntu distributions.

Be sure to read the flags underneath each command. It may not always be nesscesray to use all of the flags. Only use the options which apply to the use case.

## Contents

[Software](#sofware)

[Processes](#processes)

[Networking](#networking)

- [Network Processes](#network-processes)

[SSH](#ssh)

[Shell Scripting](#shell-scripting)

[Logging](#logging)

[Containers](#containers)

- [Docker](#docker)

[Managing Files](#managing-files)

[Permissions](#Permissions)

- [Modes](#modes)
- [Users](#users)
- [Groups](#groups)

[Storage Devices](#storage-devices)

- [Disk Usage](#disk-usage)
- [Managin Devices](#managing-devices)

## Software

## Processes

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

##### Run a script on remote ssh

        ssh root@MachineB 'bash -s' < local_script.sh

## Shell Scripting

## Logging

##### Check live log files

        sudo tail n10 -f /path/to/file

- n - number of lines
- f - keep file open

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

#### Start a container

##### Build contianer from local Dockerfile

        docker build -t image_name .

- t - name you wish to give the container

##### Run container after building it

        docker run -d --name container_name -p 80:80 image_name

- d -
- name - give container name or a default one will be created
- p - assign port mapping first is host second is container port

## Managing Files

##### Create symlink

    ls -s originalfile linkedfile

- this command create a soft symbolic link to the first file, the second file can be moved around anywhere and minipulated. When it is open it points to the original file. it is similar to a shortcut on Windows OS

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

Permsions dictate who has access to what files, the persmions are broken into 3 groups. The user, group and other. Permisions can exist of both directories and on files. Another name for permisions is mode

- Permisions on a directory can effect files in the directory
- If the file permsiosns look correct, check directory Permisions
- Work your way up to root

### Modes

##### Examples

| Symbol      | Octal | Permision                                                  |
| ----------- | ----- | ---------------------------------------------------------- |
| -rwx------  | 700   | Only owner can read write and Execute                      |
| -rwxr-xr-xr | 755   | Everyone on system can execute but only user can edit file |
| -rw-rw-r--  | 664   | User read and write, Group read and write, other only read |
| -rw-rw----  | 660   | Only user and group can read and write file                |
| -rw-r--r--  | 664   | User read and write, group and other only read             |

##### Read, write, execute

| r   | w   | x   |
| --- | --- | --- |
| 4   | 2   | 1   |

##### Comparison

| Compared |     |     |     |
| -------- | --- | --- | --- |
| Symbolic | rwx | r-x | r-- |
| Binary   | 111 | 101 | 100 |
| Decimal  | 7   | 5   | 4   |

##### Chnage directory mode recursively

    chmod 770 -R mydir

- R - change all sub files and folders

##### Change owner of file

    sudo chown -R username file.txt

- R - recuresivley change the permisions

##### Change owner and group

    sudo chown user:group file.txt

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

- this command as a script for the useradd command

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

## Storage Devices

### Disk Usage

##### Install ncdu

    sudo apt install ncdu

- this program allows a person to naviage though the directory tree while viewing disk usage

##### Show disk usage

    df -h

- h - shows usage in human readable form

##### Show inodes

    df -i

- i - shows free inodes

##### Check disk usage

    du -hsc *

- h - shows human readbale disk usage
- s - summary
- c - current working directory

### Manageing Devices

#### Important folders

    /etc/fstab

- this folder lists all active file storage devices on the machine. it also whows where devices should be mounted on startup

##### Show all disks

    sudo fdisk -l

##### List all block devices

    lsblk

##### Show device UID

    blkid

##### Mount all available disks

    sudo mount -a

- a - automatically mount all available disks
