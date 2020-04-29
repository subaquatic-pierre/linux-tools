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

Check what proccess ID running on what port

        sudo netstat -ltnp

- l - display only listening sockets
- t - display tcp connection
- n - display addresses in a numerical form
- p - display process ID/ Program name

## SSH

Secure Copy to remote machine

        scp /path/to/file user@target:/path/to/target

Start ssh agent

        eval 'ssh-agent -s'

### Logging

Check live log files

        sudo tail n10 -f /path/to/file

- n - number of lines
- f - keep file open

Run a script on remote ssh

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

Build contianer from local Dockerfile

        docker build -t image_name .

- t - name you wish to give the container

Run container after building it

        docker run -d --name container_name -p 80:80 image_name

- d -
- name - give container name or a default one will be created
- p - assign port mapping first is host second is container port

## Managing Files

Make new directory

    mkdir /path/to/dir

Viewing contents of files

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

### Users

#### Folders of interest

    /etc/password

- This folder contains all user info

  /etc/shadow

- This folder contains password information of users

Swithing user

    su username

- exclude the username if you wish to swith to root user

Add new user

    sudo adduser username

Delete user

    sudo -r userdel username

- r - Remove all user data
- If not using r flag the user directory is not deleted. Be sure to move old user home directory to the archive

### Groups

#### Folders of interest

    /etc/group

- this file shows all groups on the machine

## Software Packages

## Processes

## Shell Scripting
