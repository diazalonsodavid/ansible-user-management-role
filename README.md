User Management
=========
Role to create a list of users and change root password.

Users and Passwords will be defined as variables in default folder

Role Variables
--------------
Users and Passwords variables will be looking for inside of Default folder and they will have the following structure:

You must add the ssh keys in the files folder and add the file name inside the variables of manage_users

The user's groups must be defined in the variable "groups".
If it does not belong to any group the value will be left blank.

```yaml
---
## defaults file for user-management

## Users and password variables.

manage_users: # Main loop variable
# Users and passwords variables
# user: will be user name
# user_passwd: will be user password

  - user: user1
    user_passwd: pass_user1
    groups: group1, group2
    key: id_rsa1.pub
  - user: user2
    user_passwd: pass_user2
    groups: group2
    key: id_rsa2.pub
  - user: user3
    user_passwd: pass_user3
    groups: ""
    key: id_rsa3.pub

## New Root password
# This password will be the new root password of the systems.

new_root_password: root_password



```


Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: roles/user-management, become: yes }
