### File Permisions

| Symbol | Type          |
| ------ | ------------- |
| -      | Regular file  |
| d      | Directory     |
| l      | Symbolic Link |
| r      | Read          |
| w      | Write         |
| x      | Execute       |

# Permisions

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

##### Permsion string

When conducting an ls of a file the following is shown

    -rw-r--r-- 1 user group 8005 Apr 29 20:50 README.md

The following shows a breakdown of the first symbols

- First 3 - file type
- next 3 - user permision
- next 3 - group persmisions
- last 3 - other permissions

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
