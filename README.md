[![Build Status](https://travis-ci.org/wahidsadik/ansible-role-harden-centos.svg?branch=master)](https://travis-ci.org/wahidsadik/ansible-role-harden-centos)

Role Name
=========

An Ansible role to harden CentOS 7.x after boot up.

The role is available on Ansible Galaxy: [https://galaxy.ansible.com/wahidsadik/ansible-role-harden-centos](https://galaxy.ansible.com/wahidsadik/ansible-role-harden-centos).

To add this role from Ansible Galaxy, run: `ansible-galaxy install wahidsadik.ansible-role-harden-centos`.

To add this from your Ansible `requirements.yml`, add this to the file:

    src: wahidsadik.ansible-role-harden-centos

Warning!
> - The role will prevent login of `root` user.
> - The role will only allow login via SSH. You need to provide one or more public of machines you want to access from.
> - Some parts non-idempotent, hence rerunning may not work.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

The role defines the following variables in `defaults/main.yml`:

    enable_auto_update: True
    enable_fail2ban: True
    enable_ufw: True

    add_deployment_user: True
    deployment_user: deployer
    deployment_group: deployer

    enable_stop_password_authentication: True
    enable_stop_root_login: True

Set the values of the first 4 variables to `False` disable those behavior. It is highly recommended that all flags should be used.

Users must pass the following parameters (i.e. variables):

TODO: Mention that `enable_stop_password_authentication: True` will impact remote login for `root` user; local SSH should be fine.

- `deployment_password`. Has to be encrypted password. See the below section on how to generate one.

#### Option 1: Generate it on the fly

See the first example for this.


#### Option 2: Pre-generate it

See the second example for this.

> Encrypted password, generated on a Linux box using:  
> `$ echo 'import crypt,getpass; print crypt.crypt(getpass.getpass(), "$6$AC3bdCF7")' | python -`
>
> Here, `$6$` represents SHA-512 algorithm, and `AC3bdCF7` is the salt.
> or, use mkpasswd which is available most linux systems; if not, install it from `whois` package, and then run:  
> `$ mkpasswd --method=SHA-512 # random salt`  
> `$ mkpasswd --method=SHA-512 --salt=AC3bdCF7 # fixed salt`
>
> **None of these works on a Mac.**

#### Option 3...n: Other possibilities

- Store hashed password in vault
- Store salt in vault
- Pass hashed password from CLI (and don't record the command :), etc.

- `public_keys`: List of filename(s) containing public keys of machines you want to access the new machine from. Define it like this:


    public_keys:
      - abc

Dependencies
------------

It is assumed that this role will be run right after creating a new machine. Hence, `root` or equivalent user with `root` equivalent permission has to drive this role.

Example Playbook
----------------

#### Example 1: Base example

    - hosts: all

      vars:
        # example uses on how to disable a variable
        - enable_ufw: False
      vars_prompt:
        - name: "deployment_password"
          prompt: "What password to use for new user?"
          private: yes
          encrypt: "sha512_crypt"
          confirm: yes
          salt_size: 7

      roles:
        - {
            role: wahidsadik.ansible-role-harden-ubuntu,
            public_keys: [
              '~/.ssh/id_rsa.pub'
            ]

          }

Assuming you saved this playbook as `test-hardening-role.yml`, run it like this:

- `$ ansible-playbook -i <my-ip>, test-hardening-role.yml --user=<connection-user> --ask-pass`. For password-based login. Enter user's password at prompt and then new user's password.
- `$ ansible-playbook -i <my-ip>, test-hardening-role.yml --user=<connection-user> --become --ask-become-pass`. For SSH-based login. Enter sudo user's password at prompt and then new user's password. 

#### Example 2: With non-root user and pre-configured password

    - hosts: servers
      vars:
        public_keys:
          - '~/.ssh/id_rsa.pub'
        # the password is 'test'
        deployment_password: $6$AC3bdCF7$MA5sPtsGsOei6fCtyyzHeOqBpEzsi.yl9wS1yaP1.nKhuNR6ZBmcouWh6XJkrFdzreENtvUF4Gr2R0gfIQ/PT.
      roles:
         - { 
             role: wahidsadik.ansible-role-harden-ubuntu,
           }

You can run it the same way it's shown in last example.

#### Example 3: Ready to use playbook

TBD

License
-------

MIT

Author Information
------------------

Wahid Sadik

Repo: [https://github.com/wahidsadik/ansible-role-harden-centos](https://github.com/wahidsadik/ansible-role-harden-centos).

TODO
------------------

- [ ] Update the example playbook section
- [ ] Add a git gist with a sample playbook
