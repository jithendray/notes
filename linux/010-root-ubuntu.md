---
tags:
  - linux
---
# Root Access in Ubuntu

There are many ways to gain root privileges and it is very confusing for linux noobs(like myself) to understand how different all the existing ways are. So, I am writing few points I observed while accessing root privileges in Ubuntu.


Note: This is post for my future self to look back and review the material. So, this’ll be very unpolished!

I always used to wonder why Linux (ubuntu) always have a super user aka root and in general it is locked so no user can login as root directly on their systems. `Root` is the super account that by default has access to all the files and can do anything/ everything within the system. But why is this locked in general ?

Ever came across this famous saying ?
> With great power comes great responsibility

Root account has to be handled with great care because it gives the complete access and power to do anything with the linux internal files and system configurations. If an over-curious mind (like mine) wants to play with system configurations without having much idea, then any small mistake might destroy the system! This is why many linux distros lock the root account by default to save the system from over-curious and careless people. For general tasks like downloading files from internet, watching videos on YouTube, creating and moving files etc. - There is no need of root privilege.

But for some important tasks (mostly not done daily), One might need a root privilege. Tasks like adjusting kernel configuration, changing anything present in `/etc` folder (system configurations), adjusting systen quotas, load kernel modules, build make files, use `chroot` or `reboot`, perform system administration operations like `mount` / `swapon` etc. and many other tasks.

## "su" command
The `su` is a short word for "switch user". It allows the current user to perform operations with another user's privileges. `su [user_name]` can be used to switch to the respective user. In general, it is mostly used to change the ownership from user to the root. `su` command without any prefix switched the ownership to the `root` account. 

The `su` command prompts the current user to enter the root account's password because it's just logging into the root account. With the `su` command, you can get access to root environment and root home directory.

When you are running `su` to gain root access on a system, you must know the root password. Without knowing the root account password, you authentication will fail.

## "sudo su" command

I used to try a lot to gain root access using the `su` command, and when my system is asking me to enter a root password, I used to wonder when did I set a password for root? I searched all my system configuration files to find the root password but I never found it. I even tried to re-create a root password on my grub - but that crashed my whole Ubuntu system and I ended up loosing all my data and up re-booted Ubuntu on my windows system. 

Then I came to know that, on some Linux distributions like Ubuntu, the root account is disabled by default for security reasons. This means that no password is set for root, and you cannot use `su` to switch to root. 

`sudo su` is similar to the `su` command, but it asks current user's password instead of the root account's password. Instead of directly asking your system to to switch users, you're making your system to run the `su` command as root. 

If the user is granted with `sudo` assess, the `su` command is called as root. Running `sudo su` and then typing the user password is completely same as running `su` and typing the root password.Any command that is prefixed with `sudo`  is given root privileges.

## "sudo -i" command
The `sudo -i` command is similar to `sudo su` command and allows a user to access root privileges without knowing a root password and with just user password. `sudo -i` is a much better way of gaining root account privileges.

The only difference is that - when swapping to the root environment with `sudo su`,  it is challenging to figure out what environmental variables will be kept and which ones will be changed. Whereas, `sudo -i` gains root privileges without directly interacting with the root user.

## "sudo -s" command

When swapping to the root environment with `sudo su` and `sudo -i`, the system starts environmental files like ".bashrc", ".profile" etc. The `-s` prefix for the “sudo” command reads the $SHELL variable of the current user executing commands. This command works as if the user is running `sudo /bin/bash`. The system will not read any environmental files. 

When a user runs `sudo -s`, the user gains root privileges but the user environment will not be changed. The user's home will not be root home or root environment.

## Conclusion
Based on my experience, `sudo -i` is a better way to gain a root privileges. On the other hand, using `sudo -s` can gain a root privilege without the ability to touch the root environment, something that has added security benefits.

To be on safer side and to avoid accidental damages to your Ubuntu system configurations (I literally crashed my system more than 25 times), It’s often better to just type `sudo` before a command if you want to access root for a particular task. This is essentially running ypur command under an interactive root shell. This would be what the `sudo -s` command does.