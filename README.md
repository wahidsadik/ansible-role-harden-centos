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

    deployment_user: deployer
    deployment_group: deployer

Users must pass the following parameters (i.e. variables):

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
