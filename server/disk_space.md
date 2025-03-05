# Managing your Ubuntu system disk space

A working server must use its resources efficiently. Disk space is one of the most important resources on a server. It is the <u>responsability of the users</u> to be aware of the disk space usage and to manage it properly.

## Checking disk space

You can get a general sense of the disk usage in your server by running the following command:

```bash
df -h
```
Thid command gives you a list of all the mounted filesystems in your server, along with their disk usage.
The output will look like this:

```bash
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           3.9G  3,4M  3,9G   1% /run
/dev/sda1        20G  3,4G   16G  18% /
/dev/sda2       8,6T  1,2T  7,5T  14% /home
```

Although it is relevant to check all mounted filesystems, the most important one is the root filesystem (`/`). This is the filesystem where the operating system is installed. If the root filesystem is full, the server will not be able to operate properly.<br> 

In some systems, like in the examplem above, the users data is stored in a separate disk, which is mounted in the `/home` directory. This is the most common place where disk space is used. If you see that the disk space is almost full, you should consider cleaning up some files.

## Checking your disk space
Unless you are a sudoer user, you will not be able to see the disk usage of other server users. However, you can check your own disk usage by running the following command:

```bash
du -sh ~
```
Remember ~ is a shortcut for your home directory (/home/your_user_name).

This will output the total disk usage of your home directory.

## Identifying disk space

You can hierarchically identify the directories that are using the most disk space in your user directory by running the following command:

```bash
du -h --max-depth=1 ~ | sort -h
```

This will show you the disk usage of each directory in your home directory, sorted by disk usage in ascending order.

From there you can navigate to the directories that are using the most disk space and clean up the files that you no longer need.

e.g. for checking the disk usage of the `Documents` directory:
    
```bash
du -h --max-depth=1 ~/Documents | sort -h
```

## Cleaning up disk space

There are several ways to clean up disk space in your server. Here are some suggestions:

1. Remove files or full directories you no longer need.
```bash
rm file_name
```
or 
```bash
rm -r directory_name
```
You can also use the '-f' flag to force the removal of files or directories, but only use it if you are certain that you want to remove the file or dir.

2. Compress files that you do not need to access frequently.
```bash
tar -czvf archive_name.tar.gz directory_name
```
This command will compress the directory `directory_name` into a file called `archive_name.tar.gz`. The flag `z` tells the command to compress the file using gzip.

You can also use this command to compress old user files that you no longer need and that can be stored in a backup server.