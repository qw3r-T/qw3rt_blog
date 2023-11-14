+++
title = "Linux Permissions"
date = 2023-11-14
description = "How do Linux permissons work?"
taxonomies.tags = [
    "linux",
]
+++

One of the key features that makes Linux so powerful is its robust permission system, which enables users to control who can access files, directories, and other resources on their systems. In this post, we'll dive into the basics of Linux permissions.

## Permission Groups
In Linux each file, folder or symlink has three permission groups.
1. The owner of the file (u)
2. The group of the file (g)
3. Others (o)

Typically if your logged in as user1 you will create files owned by the user user1 and by the group user1.

To change the owner of a file you have to use the command `chown` the syntax is:
```
  chown owner:group file
```
For example if you want to change the owner of myfile user to user2 and to the wheel group enter:
```
  chown user2:wheel myfile
```

## Permission Types
Each file has three permission types that can be assigned to each permission group.
1. read (r) 
2. write (w)
3. execute (e)

### List the Permissions
To list the permissions use the `ls` command with the `-l` option.
```
  ls -l
```
An example output would be.
```
  _rwxrwxr-- 1 user1 group1 149 Oct 27 20:22 myfile
```
Let's look at where we can find what.
1. The first character (`_`) is a special permission flag that can vary
2. The next three characters (`rwx`) are the permissions for the owner of the file. In this case he can read, modify and execute the file.
3. The next three characters (`rwx`) are for the group of the file. In this case the group has the same permissions as the user.
4. The next three characters (`r--`) are the permissions for all other users on the system. In this case they can only look at the file.
5. The integer after the long set of character (`1`) displays the number of hard links to a file.
6. The next to words (`user1 group1`) refers to the owner and to the group of the file. In this case the owner would be user1 and the group group1.

### Change Permissions
To change permission the `chmod` command is used.

The short form of the permission groups are:
- `u` for the owner
- `g` for the group
- `o` for other users
- `a` for all three permission groups

To change the permissions of a file run chmod followed by the permission group you wanna change, an assignment operator (+ or -), a permission and the name of the file. For example:
```
  chmod u+x file
```
This would allow the owner of file to execute it.

#### Binary References
The method explained above can be laborious. The binary reference method is easier sometimes.

To use the binary reference method you need to calculate the permission of every permission group. I know it sounds tedious but it's not.
- r = 4
- w = 2
- x =1
To calculate the permission of a permission group simply add the permission values. For example r-x would have the value 5, r being 4 and x being 1.
For defining the value of the entire permission group simply append all permission values of every group. For example `_rwx--xr--` would become `714`.

Using `chmod` to assign permissions is relatively easy. Enter `chmod`, append the number you obtained from the above explanation and the name of the file. For example:
```
  chmod 714 file
```
This would set `rwx--xr--` as permissions for file.

## Special Permissions 
In the part about listing permissions we quickly discussed the that there was a flag for special permissions in the beginning of the character string when we run `ls -l`. We will list some special permissions here.
1. If the first character is `-` or `_` no special permission is set.
2. `d` stands for a directory
3. `l` means that the file or directory is a symbolic link.
4. `s` indicates the setuid or setgid permission. However the `s` is not displayed in the special permission part, but it's represented as an s in the read portion of the group or user.
5. `t` indicates the sticky bit permission. The `t` is not displayed in the special permission part. It's instead represented as a `t` in the executable portion of all permission groups.

### Setuid/Setgid
The setuid/setgid permissions are used to tell the system to run an executable as the owner (setuid) or the group (setgid) of the file.
This can be dangerous and make your system vulnerable. Be careful with it!

To setup setuid/setgid we will use the `chmod` command.
To setup setuid for a file run:
```
  chmod g+s file
```
To setup setgid for a file run:
```
  chmod u+s file
```
Notice the **g** standing for **user** and the **g** standing for **group**.

### Sticky Bit
The sticky bit permission can be very useful especially in a shared environment. When a directory has the sticky bit permission, then the files contained in it can be only be deleted and renamed by the owner of the file.
To setup the sticky bit permission we will use the `chmod` command.
To add the sticky bit permission to dir run:
```
  chmod +t dir
```


