# almaops.common.user

Role user aims to configure users in a functional way: the user configuration will be exactly the same, which is described in the config and guaranteed has no side-effects.

Description
------------

The role is written on the assumption that the main entry point to the configuration is not the machine, but the user. That is, we do not want create a user on the machine, but we want to give user access to certain machines instead.

You can:
- manage your user public keys database
- can set multiple keys for one user
- can delete users (home directories ain't removed)
- can lock/unlock password auth
- set local configuration per host or ansible group of hosts

Role Variables
--------------

Parameters are divided into global and local. If the parameter is described locally to the host (in the hosts dictionary), then the global parameter is taken. If the global parameter is not described, the default value is taken.

username: Required, specifies the user name on the machine

hosts: mandatory parameter, defines the list of hosts to which the user will have access and their local configuration

give_sudo: the sudo/wheel group is taken to a separate parameter in view of of exceptional importance. This parameter determines whether to give the user has the right to start sudo or not to give. Can be used in the local host configuration. Default no.

password: sets the password for the user. Can be used in local configuration.

lock_password: sets the password lock. If set to yes, then for the user is not allowed to enter the password (in shadow is set '*'). May be used locally. Default no.

disable_user: blocking the login for the user. If set to yes, then the input under the given user is impossible including on ssh (the default shell is installed in nologin). Can be used locally. Default no.

delete_user: deletes the user (but not his home directory). Can be used locally. Default no. Use with caution, and it is better not to use at all - disable_user more the preferred option.

shell: Specifies the shell for the user. Can be used locally. The default is /bin/bash.

ssh_public_keys: list of objects with name, fullname fields (optional) and key. It is a base with all known keys to it was possible to operate them by name.

authorized_keys: list of names from the ssh_public_keys database. With this name in the name field of an object in the database, its key is added to the .ssh/authorized_keys. Can be used locally. Default [].

common_groups: an exclusively global parameter. Specifies groups that are common for all hosts on which the user will be created. Recommended to set ["users"], by default []. Do not add sudo to it. For this is give_sudo.

groups: an exclusively local parameter. Specifies groups that will be are added to common_groups on this particular host. Do not add here sudo. For this, there is give_sudo.

Example 1 (short)
----------------

### manage-users.yml

    - hosts:
        - all
      become: yes
      become_user: root
      vars_files:
        - vars/ssh_public_keys.yml
      vars:
        common_groups: [ users ]
      roles:
    
        - tags: [ admins, freehck ]
          role: almaops.common.user
          username: freehck
          give_sudo: yes
          authorized_keys: [ freehck ]
          hosts:
            - host: all
    
        - tags: [ special, jenkins ]
          role: almaops.common.user
          username: jenkins
          authorized_keys: [ jenkins, jenkins-slave01, jenkins-slave02 ]
          hosts:
            - host: all
            - host: jenkins-slave01
              groups: [ docker ]
    
        - tags: [ testers, tester ]
          role: almaops.common.user
          username: tester
          authorized_keys: [ tester ]
          hosts:
            - host: stand01
            - host: stand02
            - host: db01

### vars/ssh_public_keys.yml

    ssh_public_keys:
      - name: freehck
        fullname: Dmitrii Kashin
        key: ssh-rsa AAAA...fg46 freehck
    
      - name: jenkins
        key: ...
    
      - name: jenkins-slave01
        key: ...
    
      - name: jenkins-slave02
        key: ...

Example 2 (with all possible options)
----------------

    - role: almaops.common.user
      username: freehck # required
      give_sudo: false
      password: "mysecret"
      lock_password: false
      disable_user: false
      delete_user: false
      shell: "/bin/bash"
      common_groups: [ "users" ]
      authorized_keys: [ key_name, ... ]
      ssh_public_keys:
        - name: freehck
          fullname: Dmitrii Kashin
          key: <public-key>
      hosts: # required
        - host: host-or-inventory-group # required
          give_sudo: true
          password: "mysecret"
          lock_password: false
          disable_user: false
          delete_user: false
          shell: "/bin/zsh"
          groups: [ "vboxusers" ]
          authorized_keys: [ key_name, ... ]
        - host: host-or-inventory-group
          ...

License
-------

MIT

Author Information
------------------

This role was written by Dmitrii Kashin aka freehck
