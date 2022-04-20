# confined-users-policy
This is repository, where will be implementation of diploma thesis practical output. The SELinux policies for new confined users.

The security policies are under Apache 2.0 license.

# How to apply policy on system

### Requirements
selinux-policy-devel package

1. Install via:

$ sudo dnf install selinux-policy-devel

### Steps to add into system

1. Compile .te file into .pp file

    $ make -f /usr/share/selinux/devel/Makefile nameOfModule.pp

2. Load compiled file into kernel

    $ semodule -i nameOfModule.pp
    
    
### How map linux users to SELinux users

I will show how mapping specially basic user, but for other new users will be same procedure.

1. You will need to create a file for a new user in /etc/selinux/targeted/contexts/users, the easiest way is to copy an existing user and change the user name in
the file using:

    $ sed -e ’s | user | basic |g ’ user_u > basic_u
    
2. Then you need to create a new SELinux user based on the new type and role.

    $ sudo semanage user -a basic_u -R " basic_r "
   
3. Then you need linked linux user to SELinux user.

    $ sudo semanage login -a -s basic_u yourLinuxUser

4. Last step is change the labels in the home directory of yourLinuxUser
because it got the default security context.

    $ sudo restorecon -RvF /home/yourLinuxUser


