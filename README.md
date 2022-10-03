User Management
=========
Role to create a list of users and change root password.

Users and Passwords will be defined as variables in default folder

Role Variables
--------------
Users and Passwords variables will be looking for inside of Default folder and they will have the following structure:

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
  - user: user2
    user_passwd: pass_user2
  - user: user3
    user_passwd: pass_user3

## New Root password
# This password will be the new root password of the systems.

new_root_password: root_password

```




ansible-playbook --ask-vault-pass playbook.yml -i inventories/hosts

***Integrating encrypted variables into the playbook***

If we want to integrate the encrypted variables in the task directly, we must replace the calls to the variables by the encrypted value:

e.g.

```yaml
- name: Create user
  ansible.builtin.user:
    name: "{{ item.user }}"
    password: "{{ item.user_passwd | password_hash('sha512') }}"
  with_items: "{{ manage_users }}"
```

```yaml
 name: Create user
  ansible.builtin.user:
    name: "{{ item.user }}"
    password: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      62313365396662343061393464336163383764373764613633653634306231386433626436623361
      6134333665353966363534333632666535333761666131620a663537646436643839616531643561
      63396265333966386166373632626539326166353965363262633030333630313338646335303630
      3438626666666137650a353638643435666633633964366338633066623234616432373231333331
      6564
  with_items: "{{ manage_users }}"
```
***How to use the variables in the group_vars file:***

We will need to create a file inside group_vars that will have the same name as the value of the hosts parameter of the playbook.

e.g.

If our playbook calls the hosts "servers"

```yaml
- hosts: servers
  roles:
     - { role: roles/user-management, become: yes }
```

In the file inside inventories there should be these lines:

```ini
[servers]
server1 ansible_ssh_hosts=192.168.1.5
```

Therefore, inside group_vars there must be a file called ***servers*** and inside it will be the variables that you are going to use.


To encrypt the value of a variable inside that file we must use the previous commands:

```console
ansible-vault encrypt_string 'variable_password' --name 'variable_name'
```
and copy what the console returns inside the file.



To encrypt the whole file we will use:

```console
ansible-vault encrypt group_vars/servers
```




Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: roles/user-management, become: yes }


