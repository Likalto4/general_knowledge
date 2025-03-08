# Files and directories in Ubuntu (Linux in general)

Here we discuss some basic commands to manage ownership, access and permissions of files and directories in an Ubuntu server.

## File general information

-  We can see the overall file permissions with:
```bash
ls -lh
```

We recommend the h flag to make the output human-readable.

The command will show the files and directories in the current directory, along with their permissions, owner, group, size, and date of last modification.
You will see something like this:
    
```bash
-rw-r--r-- 1 user user  1.2K Mar  1 10:00 file1.txt
drwxr-xr-x 2 user user  4.0K Mar  1 10:00 directory1
```

The first column shows the file type and permissions. The first character indicates the file type: a dash (-) for a regular file, a d for a directory, and an l for a symbolic link.<br>

The <u>next nine characters</u> represent the file permissions. The first three characters represent the owner's permissions, the next three represent the group's permissions, and the last three represent everyone else's permissions.<br>

The permissions are represented by the following characters:
- r: read
- w: write
- x: execute

If a permission is not granted, a dash (-) will appear in its place.

The next two columns show the owner and group of the file or directory. The fourth column shows the file size in bytes (depending on the permissions you may see a misleading size for directories). The fifth column shows the date and time of the last modification. <br>

- If the file has ACLs (Access Control Lists) or extended attributes, you will see a + sign at the end of the permissions string. You can see the ACLs with the command:
```bash
getfacl file1.txt
```

## Changing file ownership

> **Note:** You need to be the owner of a file or directory or have sudo (superuser) privileges to change its permissions or ownership.

- You can change the owner of a file with the command:
```bash
chown new_owner file1.txt
```

- You can change the group of a file with the command:
```bash
chgrp new_group file1.txt
```

- Additionally, you may change both at the same time with:
```bash
chown new_owner:new_group file1.txt
```

## Changing file permissions

Recall permissions are represented by the following characters:
- r: read
- w: write
- x: execute

When using the `chmod` command, you can specify the permissions in two ways: symbolic or octal.

### Symbolic permissions

- You can add or remove permissions with the following syntax:
```bash
chmod u+x file1.txt
```

This command adds the execute permission to the owner of the file. You can also remove permissions with the - sign:
```bash
chmod u-x file1.txt
```

You can also specify the permissions for the group and everyone else with the following syntax:
```bash
chmod g+r,o-r file1.txt
```

This command adds the read permission to the group and removes the read permission from everyone else.

### Octal permissions

You can also specify the permissions in octal format. The permissions are represented by the following numbers:
- 4: read
- 2: write
- 1: execute

The combinations of these numbers represent the permissions. For example:
- 7=4+2+1: read, write, and execute
- 6=4+2: read and write
- 5=4+1: read and execute
etc.

That way, you can set the permissions for the owner, group, and everyone else with a single number. For example, to give read and write permissions to the owner, read permissions to the group, and no permissions to everyone else, you can use:
```bash
chmod 640 file1.txt
```

This can also be implemented for directories. To apply recursively to all files and directories within a directory, you can use the -R flag:
```bash
chmod -R 640 directory1
```

