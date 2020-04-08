# Overview

This is an Ansible playbook that will build InProd on a centos/RHEL 7 operating system.

# What is InProd
InProd is a DevOps and change management tool for the Genesys Engage platform. Within InProd Changesets can be created that move changes between multiple environments or perform scripted changes such as automatically creating agents via sctipts, AD integraion or a web page as displayed here.

Learn more at https://www.inprod.io


# Design Notes
This playbook can be used to manage your production and none production instances. For this reason the playbook
as been intentionally limited to being able to run against one inventory entry at a time. This allows the user
to configure both production and none production entries within the host file but be protected from accidently
running the playbook against both.

The package `inprod-bow` has a requirement for Postgres 9.6 which must be installed. This playbook will
disable this service and install Postgres 10 instead.

By removing the `database` role `inprod-bow` could connect to an external Postgres instance instead of the local one.

Any additional settings to the `settings.json` file (such as LDAP) should be made to the template file directly.


# HTTPS
The role `inprod-nginx` can be adapted to enable HTTPS support. Within the `templates` folder create a subfolder that
contains the public and private certificates. This has been designed to support LetsEncrypt certificates but can easily
be adapted to suit other providers.


# Run the playbook

ansible-playbook main.yml -i hosts.yml --limit 192.168.20.55

The playbook will terminate if there is more then one host targeted, this is by design and the reason for needed to
provide the 'limit' option.
