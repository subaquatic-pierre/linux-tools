# Linux Tools

A list of commands for tasks and troubleshooting, commands will work with Debian and Ubuntu distributions.

Be sure to read the flags underneath each command. It may not always be nesscesray to use all of the flags. Only use the options which apply to the use case.

---

## Contents

[Software](#sofware)

[Processes](#processes)

- [Process Management](#process-management)
- [Memory Management](#memory-management)
- [Cron Scheduling](#cron-scheduling)

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

[Usefull Packages](#usefull-packages)

- [Aptitude](#aptitude)
- [AWS CLI](#storage-devices)
- [Tmux](#tmux)
- [Htop](#htop)
- [Nginx](#nginx)
- [Supervisor](#supervisor)

---

## Packages

#### Folders of interest

    /etc/apt/sources.list

- this is where apt searches for repos for packages. To add a new package to the repo list create a new file called _packagename.list.d_. this will have apt search any file with .list extension

#### Install packes backed up with dpkg

##### Export list of installed packages

    dpkg --get-selections > packages.list

##### Install dselect

    sudo apt install dselect

##### Update dselect

    sudo dselect update

##### Add packages from list just created

    sudo dpkg --set-selections < packages.list

##### Install packages

    sudo apt-get dselect-upgrade

##### Update repositories

    sudo apt update

##### Upgrade to latest package versions

    sudo apt upgrade

##### Install Package

    sudo apt intall -y packagename

- sy -automatically say yes to requests to install packages

##### Remove package

    sudo apt remove --purge packagename

- purge - remove package configuration

##### Search for apt package

    sudo apt search packagename

##### Show package details

    sudo apt-cache show packagename

##### Install Snap packages

    sudo snap install packagename

##### Find snap package

    sudo snap find packagename

##### Check which path package is using

    which packagename

##### Update snap package

    sudo snap refresh packagename

##### Remove snap package

    sudo snap remove

## Processes

### Process management

##### Show PID of process name

    pidof vim

- shows PID of the vim program

##### Show all running processes

    ps aux

- add | grep programname to reduce results

##### Show process with particular string

    ps u -C string

##### Minimize current terminal program

    CTRL + Z

##### Bring program back

    fg

##### HTOP

_Check htop program in usefull program list_

##### Kill process

    sudo pkill -9 programname

- 9 - send imidiate terminate command
- without 9 the standard sigterm is sent to the program

##### Kill proceess by PID

    sudo kill 90000

##### Show all running system programs

    systemctl

##### Get status of daemon

    systemctl status -l ssh

- this shows the status of the ssh daemon
- l - shows full list of status

##### Starting and stopping proecesses

    sudo systemctl start / stop / restart / reload ssh

- this starts, stops, resart or reload the ssh daemon. restart or reload are only available with some units

##### Enable or Disable unit

    sudo systemctl enable / disable ssh

- disable or enable ssh daemon

### Memory management

##### Check free memory

    free -m

### Cron Scheduling

Crontab is a task scheduling tool. Layout of the task in the cron file is as follows

m h dom mon dow path/to/command

| Symbol | Meaning           |
| ------ | ----------------- |
| m      | minute            |
| h      | hour (0-23)       |
| dom    | day of month      |
| mon    | month             |
| dow    | day of week (0-6) |

eg.

    4 0 * * 4 /home/user/cleanup.sh

This task will run Friday at 12:03am

##### View user crontab

    crontab -l

##### Create cron job

    crontab -e

## Networking

#### Folders of interest

    /ect/hostname
    /etc/hosts
    /etc/netplan/01-netcfg.yaml
    /etc/nsswitch.conf

- within the hostname folder is where the hostname of the current machine is
- the hosts folder contains a list of the hostnames with IP addresses to resolve those names to
- within the netplan directory are config files for network devices and their addresses. the files containing the configurations can be called _01-netcfg.yaml_ or _50-cloud-init.yaml_
- nsswith.conf file determines order in which the machine checks DNS

##### Change hostname

    sudo hostnamectl set-hostname my.new.hosname

##### Show current IP address

    ip addr show

##### Bring up or down network device

    sudo ip link set enp0s3 up / down

##### Restart all network services

    sudo systemctl restart network.service

##### Check current DNS resolvers

    systemd-resolve --status | grep DNS\ Servers

### Network processes

##### Check what proccess ID running on what port

        sudo netstat -tulpn

- l - display only listening sockets
- t - display tcp connection
- n - display addresses in a numerical form
- p - display process ID/ Program name
- u - does something

## SSH

### Simplify Connections

Create file ~/.ssh/config inside the file add the following

    host myserver
        Hostname 192.0.0.1
        Port 22
        User username
        IdentityFile ~/.ssh/targaryen.key
        ServerAliveInterval seconds to ping remote server

##### SSH into remote macine

    ssh -p 30 user@10.0.0.1

- p - specify which port to use, by default ssh traffic is over port 22

##### Generate SSH Key

    ssh-keygen -p

- p - create passphrase with key

##### Start SSH agent

    eval 'ssh-agent -s'

##### Add key to SSH agent

    ssh-add /path/to/key

##### Copy public key to remote machine

    ssh-copy-id -i ~/.ssh/id_rsa.pub fortress

- this copies the key to server named fortress

##### Secure Copy to remote machine

    scp /path/to/file user@target:/path/to/target

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

_sample file_
[Dockerfile](docker/Dockerfile)

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

| Symbol     | Octal | Permision                                                  |
| ---------- | ----- | ---------------------------------------------------------- |
| -rwx------ | 700   | Only owner can read write and Execute                      |
| -rwxr-x-xr | 755   | Everyone on system can execute but only user can edit file |
| -rw-rw-r-- | 664   | User read and write, Group read and write, other only read |
| -rw-rw---- | 660   | Only user and group can read and write file                |
| -rw-r--r-- | 644   | User read and write, group and other only read             |

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

## Usefull Packages

### Aptitude

This software is used to manage packes, offers a terminal GUI to navigate packages

##### Install Aptitude

    sudo apt install aptitude

##### Unmark package as automatically installed

    sudo aptitude unmarkauto packagename

##### Run aptitude

    sudo aptitude

### AWS CLI

##### Test cloud formation template

    aws cloudformation validate-template --template-body file://sampletemplate.json

### TMUX

This program is used to keep a shell terminal running on a remote machine once the connection is lost. It is usefull when setting up network connections and having to restart the connection kicks you out of the system. The commandd will still keep running on the remote machine

##### Install TMUX

    sudo apt install tmux

##### Activate tmux

    tmux

### Htop

This program is best used to display current running processes. It offers a terminal GUI to navigate processes.

##### Install Htop

    sudo apt install htop

### Nginx

Nginx is software to serve websites from a machine. It can also act as a reverse proxy for other services.

#### Folders of interest

    /etc/nginx/nginx.conf
    /etc/nginx/conf.d/
    /etc/nginx/sites-enabled/

- nginx.conf file is where main system settings for nginx are configured, this file includes paths to directories which contain configurations for other servers
- conf.d/ this directory is where a user should put all other servers which will be hosted on the machine. The extension should be conf
- sites-enable - this is the old directory where configuration settings are installed. A user should remove the defualt server file

_sample configuraion file_
[sample1-server.conf](nginx/sample1-server.conf)
[sample2-server.conf](nginx/sample2-server.conf)

##### Install nginx

    sudo apt install nginx

### Supervisor

This software is able to run programs on remote machnies, restart them if they go down or send alerts if there is an issue with an application

#### Folders of interest

    /etc/supervisor/conf.d/

- any files stored on this directory with the extension .conf will be run when the supervisor command is run

_sample configuration file_
[gunicorn-supervisor.conf](supervisor/gunicorn-supervisor.conf)
[sample-supervisor.conf](supervisor/sample-supervisor.conf)

##### Install supervisor

    sudo apt install supervisor

### Gunicorn

This software is to run a WSGI interface for python websites, in particular it is used for Django websites

##### Install Gunicorn

    sudo apt instasll gunicorn

_sample configuration file_
[sample-gunicorn.py](gunicorn/sample-gunicorn.py)
