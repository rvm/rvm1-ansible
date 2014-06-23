## What is ansible-ruby? [![Build Status](https://secure.travis-ci.org/nickjj/ansible-ruby.png)](http://travis-ci.org/nickjj/ansible-ruby)

It is an [ansible](http://www.ansible.com/home) role to install a specific version of ruby without having to compile ruby on the machine you're setting up. It does this by using rvm under the hood.

### What problem does it solve and why is it useful?

I wasn't happy with any of the solutions that existed because they required you to either compile ruby on your server or they were not idempotent. Compiling ruby on a low end VPS/instance could easily take 10 minutes and during that time your CPU is going to be pegged at 100%.

ansible-ruby solves this by using rvm to install 1 or more versions of ruby. It makes installing and upgrading ruby versions very easy.

## Role variables

Below is a list of default values along with a description of what they do.

```
# Set the version of ruby you want to install.
ruby_version: 2.1.2 # X.X.X-pXXX format

# Where should the rvm-installer and other temp files be downloaded to?
ruby_temp_download_path: /usr/local/src

# Where should rvm be installed to?
ruby_rvm_install_path: /usr/local/rvm

# Which url or local path contains the rvm-installer script?
# If you use a url, it must include http:// or https:// at the start.
ruby_rvm_latest_installer: https://raw.githubusercontent.com/wayneeseguin/rvm/master/binscripts/rvm-installer

# Which url or value contains the stable version number of rvm?
# If you use a url, it must include http:// or https:// at the start.
# If you use a value, it should be something like 1.25.27 or any legal version number.

# If you want to lock rvm to a specific version you could enter in the version number
# that you have installed and the role would assume you always have the latest stable version.
ruby_rvm_stable_version_number: https://raw.githubusercontent.com/wayneeseguin/rvm/master/VERSION

# Force upgrade the rvm-installer to the latest installer.
ruby_rvm_force_upgrade_installer: false
```

## Example playbook

For the sake of this example let's assume you have a group called **app** and you have a typical `site.yml` file.

To use this role edit your `site.yml` file to look something like this:

```
---
- name: ensure app servers are configured
- hosts: app

  roles:
    - { role: nickjj.ruby, tags: ruby }
```

Let's say you want to edit a few values, you can do this by opening or creating `group_vars/app.yml` which is located relative to your `inventory` directory and then making it look something like this:

```
---
ruby_version: 2.0.0
ruby_rvm_stable_version_number: 1.25.26
```

## Installation

`$ ansible-galaxy install nickjj.ruby`

## Requirements

Tested on ubuntu 12.04 LTS but it should work on other versions that are similar.

## Ansible galaxy

You can find it on the official [ansible galaxy](https://galaxy.ansible.com/list#/roles/796) if you want to rate it.

## License

MIT
