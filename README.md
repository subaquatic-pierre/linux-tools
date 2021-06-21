# Linux Tools

A list of commands for tasks and troubleshooting, commands will work with Debian and Ubuntu distributions.

Be sure to read the flags underneath each command. It may not always be necessary to use all of the flags. Only use the options which apply to the use case.

---

## Contents

[Software](#software)

[Processes](#processes)

- [Process Management](#process-management)
- [Memory Management](#memory-management)
- [Cron Scheduling](#cron-scheduling)

[Networking](#networking)

- [Network Processes](#network-processes)

[SSH](#ssh)

[Shell Scripting](#shell-scripting)

[Logging](#logging)

[Managing Files](#managing-files)

[Permissions](#Permissions)

- [Modes](#modes)
- [Users](#users)
- [Groups](#groups)

[Storage Devices](#storage-devices)

- [Disk Usage](#disk-usage)
- [Managing Devices](#managing-devices)

[Useful Packages](#useful-packages)

- [Docker](docker)
- [React](react)
- [Postgres](postgres)
- [Ansible](#ansible)
- [Aptitude](#aptitude)
- [AWS CLI](#aws-cli)
- [Tmux](#tmux)
- [Htop](#htop)
- [Nginx](#nginx)
- [Supervisor](#supervisor)
- [Gunicorn](#gunicorn)

---

## Software

### Folders of interest

    /etc/apt/sources.list

- This is where apt searches for repos for packages. To add a new package to the repo list create a new file called _packagename.list.d_. This will have apt search any file with .list extension

#### Update and install software

    sudo apt update && sudo apt install

- Use && to run 2 commands, if the first one fails the second one will not run

### Install packages backed up with dpkg

#### Export list of installed packages

    dpkg --get-selections > packages.list

#### Install dselect

    sudo apt install dselect

- This software is used to install all packages which have been exported with dpkg

#### Update dselect

    sudo dselect update

#### Add packages from list just created

    sudo dpkg --set-selections < packages.list

#### Install packages

    sudo apt-get dselect-upgrade

#### Update repositories

    sudo apt update

#### Upgrade to latest package versions

    sudo apt upgrade

#### Install Package

    sudo apt install -y packagename

- y - Automatically say yes to requests to install packages

#### Remove package

    sudo apt remove --purge packagename

- purge - Remove package and configuration

#### Search for an apt package

    sudo apt search packagename

#### Show package details

    sudo apt-cache show packagename

#### Install Snap packages

    sudo snap install packagename

- Snap is a newer package type, it is independent of system libraries which allows upgrades of software packages to be independent of OS upgrades

#### Find snap package

    sudo snap find packagename

#### Check which path package is using

    which packagename

#### Update snap package

    sudo snap refresh packagename

#### Remove snap package

    sudo snap remove

#### Locate package files

    locate packagename

#### Unpack .tgz file

    tar -xzvf data.tgz -C /tmp

- x - Extract file
- z - Deal with compressed file i.e. filter the archive through gzip
- v - Verbose output i.e. show progress
- f - File, work on data.tgz file.
- C - /path/to/file
- t - List all files stored in the archive

## Processes

### Process management

#### Update system clock

    sudo date -s "$(wget -qSO- --max-redirect=0 google.com 2>&1 | grep Date: | cut -d' ' -f5-8)Z"

#### Show PID of process name

    pidof vim

- Shows process ID of the vim program

#### Show all running processes

    ps -aux

- add | grep programme to reduce results to the package you are looking for

#### Show process with particular string

    ps u -C string

#### Minimize current terminal program

    CTRL + Z

#### Bring program back

    fg

- Add a number after fg if there are multiple terminals which have been minimized

#### HTOP

_Check htop program in useful program list_

This software gives a full list of available and installed packages. It offers a GUI in the terminal to manage packages

#### Kill process

    sudo pkill -9 programname

- 9 - Send immediate terminate command
- Without 9 the standard SIGTERM is sent to the program

#### Kill process by PID

    sudo kill 90000

#### Show all running system programs

    systemctl

- Use sudo if you want access to info only root user can see

#### Get status of daemon

    systemctl status -l ssh

- This shows the status of the ssh daemon
- l - Shows full list of status

#### Starting and stopping processes

    sudo systemctl start / stop / restart / reload ssh

- This starts, stops, restart or reload the ssh daemon. Restart or reload are only available with some units

#### Enable or Disable unit

    sudo systemctl enable / disable ssh

- disable or enable ssh daemon

### Memory management

#### Check free memory

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

- This task will run Friday at 12:03am
- Always use for path to file

#### View user crontab

    crontab -l

#### Create cron job

    crontab -e

## Networking

### Folders of interest

    /ect/hostname
    /etc/hosts
    /etc/netplan/01-netcfg.yaml
    /etc/nsswitch.conf

- Within the hostname folder is where the hostname of the current machine is
- The hosts folder contains a list of the hostnames with IP addresses to resolve those names to
- Within the netplan directory are config files for network devices and their addresses. The files containing the configurations can be called _01-netcfg.yaml_ or _50-cloud-init.yaml_
- nsswith.conf file determines order in which the machine checks DNS

#### Change hostname

    sudo hostnamectl set-hostname my.new.hostname

#### Show current IP address

    ip addr show
    ifconfig -a

#### Get public IP address

    curl ifconfig.me
    curl ident.me

#### Check DHCP journal entries

    journalctl | grep -Ei 'dhcp' | tail -n30

#### Use netplan to get further DHCP info

    netplan ip leases ens5

#### Bring up or down network device

    sudo ip link set enp0s3 up / down

#### Restart all network services

    sudo systemctl restart network.service

#### Check current DNS resolvers

    systemd-resolve --status | grep DNS\ Servers

### Network processes

#### Check what process ID is running on what port

    sudo netstat -tulpn

- l - Display only listening sockets
- t - Display TCP connection
- n - Display addresses in a numerical form
- p - Display process ID/ Program name
- u - Display UDP connections
- c - Continuous

#### Check ports in use

    sudo lsof -i -P -n | grep LISTEN
    sudo netstat -tulpn | grep LISTEN
    sudo lsof -i:22 ## see a specific port such as 22 ##
    sudo nmap -sTU -O IP-address-Here

## SSH

### Simplify Connections

Create file ~/.ssh/config inside the file add the following

    host myserver
        Hostname 192.0.0.1
        Port 22
        User username
        IdentityFile ~/.ssh/targaryen.key
        ServerAliveInterval seconds to ping remote server

- Adding a configuration file allows automatic use of settings when connecting to specific host

#### SSH into remote machine

    ssh -p 30 user@10.0.0.1

- p - Specify which port to use, by default ssh traffic is over port 22

#### Generate SSH Key

    ssh-keygen -p

- p - create passphrase with key

#### Start SSH agent

    eval "$(ssh-agent)"

#### Add key to SSH agent

    ssh-add /path/to/key

#### Copy public key to remote machine

    ssh-copy-id -i ~/.ssh/id_rsa.pub fortress

- This copies the key to server named fortress

#### Secure Copy to remote machine

    scp /path/to/file user@target:/path/to/target

#### Run a script on remote ssh

    ssh root@MachineB 'bash -s' < local_script.sh

## Shell Scripting

Shell scripting is the act of writing commands into a text file which can be run. These commands are then executed by the terminal line by line. When creating a script it is important to change the mode of the file to be executable

    chmod +x /path/to/file

_sample script_
[sample-script.sh](bash/sample-script.sh)

### Files of interest

    /home/user/.bashrc

- This is where configuration settings for the bash terminal are. If any variables or aliases are added to this file they will be initialized when a user opens a terminal

#### Change back to previous working directory

    cd -

#### Check current bash variables

    env

#### Check bash history

    history

- Run the command by typing _!numberofcommand_

#### Delete something from history

    history -d numberofcommand

#### Run previous command with sudo privileges

    sudo !!

#### Echo out a variable

    echo $variablename

#### Check status code of previous command

    echo !?

#### Add alias to a command

    alias install='sudo apt install'

- This allows you to set up custom commands

#### Add variable to environment

    export VARIABLE=something

## Logging

#### Check live log files

    sudo tail n10 -f /path/to/file

- n - Number of lines
- f - Keep file open, view logs files live

## Managing Files

#### Find File

    find -name path/to/search filename

- -name - Name of file
- -type f - Type is file
- -type d - Type of directory
- 03 - Efficient use of resources and likelihood

#### Create symlink

    ls -s originalfile linkedfile

- This command create a soft symbolic link to the first file, the second file can be moved around anywhere and manipulated. When it is open it points to the original file. It is similar to a shortcut on Windows OS

#### Make new directory

    mkdir /path/to/dir

#### Viewing contents of files

    cat /path/to/file            - Display the contents of file
    more /path/to/file           - Browse through text file
    less /path/to/file           - More features than more
    head -n10 /path/to/file       - Output the beginning or top portion of file
    tail -n10 /path/to/file       - Output the ending or bottom portion of the file

- Number of lines to be displayed when using the head or tail command

#### Grep for contents

    cat /etc/shadow | grep myuser

- This will only show information regarding the user which you grepped for

#### Remove directory

    rm -rf /path/to/dir

- r - Recursively remove all files
- f - Forcefully (be careful when using this variant)

#### Create file

    touch /path/to/file

## Permissions

Permissions dictate who has access to what files, the permissions are broken into 3 groups. The user, group and other. Permissions can exist of both directories and on files. Another name for permissions is mode

- Permissions on a directory can effect files in the directory
- When having an issue with a file check directory permissions
- Work your way up to root

### Modes

#### Examples

| Symbol     | Octal | Permission                                                 |
| ---------- | ----- | ---------------------------------------------------------- |
| -rwx------ | 700   | Only owner can read write and Execute                      |
| -rwxr-xr-x | 755   | Everyone on system can execute but only user can edit file |
| -rw-rw-r-- | 664   | User read and write, Group read and write, other only read |
| -rw-rw---- | 660   | Only user and group can read and write file                |
| -rw-r--r-- | 644   | User read and write, group and other only read             |

#### Read, write, execute

| r   | w   | x   |
| --- | --- | --- |
| 4   | 2   | 1   |

#### Comparison

| Compared |     |     |     |
| -------- | --- | --- | --- |
| Symbolic | rwx | r-x | r-- |
| Binary   | 111 | 101 | 100 |
| Decimal  | 7   | 5   | 4   |

#### Change directory mode recursively

    chmod 770 -R mydir

- R - Change all sub files and folders

#### Change owner of file

    sudo chown -R username file.txt

- R - Recursively change the permissions

#### Change owner and group

    sudo chown user:group file.txt

## Users

### Folders of interest

    /etc/password
    /etc/shadow

- The password folder contains all user info
- The shadow folder contains password information of users

#### Switching user

    su username

- Exclude the username if you wish to switch to root user

#### Add new user

    sudo adduser username

- This command is a script for the useradd command

#### Delete user

    sudo -r userdel username

- r - Remove all user data
- If not using r flag the user directory is not deleted. Be sure to move old user home directory to the archive

#### Change user home directory

    sudo usermod -d /new/dir username -m

#### Change username

    sudo usermod -l oldname newname

#### Lock user account

    sudo passwd -l username

- l - Lock account
- u - Unlock account

#### Check expiration date of user password

    sudo chage -l username

- l - List details
- d 0 - Set number of days to expire to 0 this will disable account
- M 90 - Set number of days after which a user needs to replace their password
- m - Set minimum number of days for password to be active, good if someone keeps changing their password back

_Plugable Authentication Module_

Install this application to set minimum requirements for passwords. This increases password strength within the machine

## Groups

### Folders of interest

    /etc/group

- This file shows all groups on the machine

#### Sudoers folder

    /etc/sudoers
    sudo EDITOR=vim visudo

- This file contains all the users who have sudo privileges, edit this folder with the following command
- This allows for the detection of any mistakes made in the sudoers file

#### Create new group

    sudo groupadd newgroup

#### Delete group

    sudo groupdel

#### Add user to secondary group

    sudo usermod -aG group user

- a - Append a secondary group, if you don't add this flag it will replace all current groups
- G - This states a secondary group to add the user to
- aG sudo - This adds a user to the sudoers group. this allows the user to use the sudo command

#### Change user primary group

    sudo usermod -g groupname username

#### Remove user from group

    sudo gpasswd -d username grouptoremove

## Storage Devices

### Disk Usage

#### Install ncdu

    sudo apt install ncdu

- This program allows a person to navigate though the directory tree while viewing disk usage

#### Show disk usage

    df -h

- h - Shows usage in human readable form

#### Show inodes

    df -i

- i - Shows free inodes

#### Check disk usage

    du -hsc *

- h - Shows human readable disk usage
- s - Summary
- c - Current working directory

## Managing Devices

### Folders of interest

    /etc/fstab

- This folder lists all active file storage devices on the machine. It also shows where devices should be mounted on startup

#### Show all disks

    sudo fdisk -l

#### List all block devices

    lsblk

#### Show device UID

    blkid

#### Mount all available disks

    sudo mount -a

- a - Automatically mount all available disks

## Aptitude

This software is used to manage packages, offers a terminal GUI to navigate packages

#### Install Aptitude

    sudo apt install aptitude

#### Unmark package as automatically installed

    sudo aptitude unmarkauto packagename

#### Run aptitude

    sudo aptitude

## AWS CLI

#### Test cloud formation template

    aws conformation validate-template --template-body file://sampletemplate.json

#### Invalidate cloudfront cache

    aws cloudfront create-invalidation --distribution-id 000000 --paths "/*"

#### Copy directory to S3

    aws s3 cp directory_to_copy/ s3://bucket_name/ --recursive --acl public-read

## TMUX

This program is used to keep a shell terminal running on a remote machine once the connection is lost. It is useful when setting up network connections and having to restart the connection kicks you out of the system. The command will still keep running on the remote machine

#### Install TMUX

    sudo apt install tmux

#### Activate tmux

    tmux

#### Create new new tab

    [ctrl +b] + c

#### Switch tab

    [ctrl + b] $tab_number

#### Activate scroll

    [ctrl + b] + [

## Htop

This program is best used to display current running processes. It offers a terminal GUI to navigate processes.

#### Install Htop

    sudo apt install htop

## Nginx

Nginx is software to serve websites from a machine. It can also act as a reverse proxy for other services.

### Folders of interest

    /etc/nginx/nginx.conf
    /etc/nginx/conf.d/
    /etc/nginx/sites-enabled/

- nginx.conf file is where main system settings for nginx are configured, this file includes paths to directories which contain configurations for other servers
- conf.d/ this directory is where a user should put all other servers which will be hosted on the machine. The extension should be conf
- sites-enable - this is the old directory where configuration settings are installed. A user should remove the default server file

_sample configuration file_
[mywebserver.conf](nginx/mywebserver.conf)

### ApolloServer - ApolloClient configuration settings

_sample Nginx and JS file_
[configuration](./apollo/apollo-server-client-config.md)

#### Install nginx

    sudo apt install nginx

## Supervisor

This software is able to run programs on remote machines, restart them if they go down or send alerts if there is an issue with an application

### Docs

    http://supervisord.org/configuration.html#supervisorctl-section-settings

### Folders of interest

    /etc/supervisor/conf.d/

- Any files stored on this directory with the extension .conf will be run when the supervisor command is run

_sample configuration file_
[gunicorn-supervisor.conf](supervisor/supervisor.conf)

#### Install supervisor

    sudo apt install supervisor

## Gunicorn

This software is to run a WSGI interface for python websites, in particular it is used for Django websites

### Docs

    https://docs.gunicorn.org/en/stable/settings.html

#### Install Gunicorn

    sudo apt install gunicorn

_sample configuration file_
[gunicorn.config.py](gunicorn/gunicorn.config.py)
