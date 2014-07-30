## What is rvm1-ansible? [![Build Status](https://secure.travis-ci.org/rvm/rvm1-ansible.png)](http://travis-ci.org/rvm/rvm1-ansible)

It is an [ansible](http://www.ansible.com/home) role to install and manage ruby versions using rvm.

### What problem does it solve and why is it useful?

I wasn't happy with any of the solutions that existed because they required you to either compile ruby on your server or they were not idempotent. Compiling ruby on a low end VPS/instance could easily take 10 minutes and during that time your CPU is going to be pegged at 100%.

rvm1-ansible solves this by using rvm to install 1 or more versions of ruby. It makes installing and upgrading ruby versions very easy.

## Installation

`$ ansible-galaxy install rvm_io.rvm1-ruby`

## Example playbook 

For the sake of this example let's assume you have a group called **app** and you have a typical `site.yml` file.

To use this role edit your `site.yml` file to look something like this:

```
---
- name: ensure app servers are configured
  hosts: app

  roles:
    # If you use the default rvm install location you must enable sudo
    # because it will install to /usr/local/rvm.
    #
    # If you are installing to your user's home directory then you can drop sudo.
    - { role: rvm_io.rvm1-ruby, tags: ruby, sudo: true }
```

Let's say you want to edit a few values, you can do this by opening or creating `group_vars/app.yml` which is located relative to your `inventory` directory and then making it look something like this:

```
---
# Let's install rvm locally to a user and install multiple versions of ruby.

# We can dump the rvm install script to /tmp.
rvm1_temp_download_path: '/tmp'

# If you're going to use the ~ shortcut and you have sudo enabled then make sure
# you set sudo_user on the role to the correct user or it will be root.
# ie. { role: rvm_io.rvm1-ruby, tags: ruby, sudo: true, sudo_user: '{{ rvm1_user }}' }

# Another option is just to not set sudo for the rvm role at all.
rvm1_install_path: '~/.rvm'

rvm1_rubies:
  - 'ruby-2.1.0'
  - 'ruby-2.1.2'
```

## Role variables

Below is a list of default values along with a description of what they do.

```
# Install 1 or more versions of ruby, just add them to the list.
# The last version listed will be set as the default ruby.
# Change it to `ruby_rubies:` if you want no rubies installed.
rvm1_rubies:
  - 'ruby-2.1.2'

# Which version of ruby do you want to delete?
# Example: `rvm1_delete_ruby: ruby-2.1.0`
rvm1_delete_ruby: ''

# Which user should own all of rvm's files?
rvm1_user: '{{ ansible_ssh_user }}'

# Which group should rvm be installed to?
rvm1_group: 'rvm'

# Where should the rvm-installer and other temp files be downloaded to?
rvm1_temp_download_path: '/usr/local/src'

# Where should rvm be installed to?
rvm1_install_path: '/usr/local/rvm'

# Which url or local path contains the rvm-installer script?
# If you use a url, it must include http:// or https:// at the start.
rvm1_rvm_latest_installer: 'https://raw.githubusercontent.com/wayneeseguin/rvm/master/binscripts/rvm-installer'

# Which url or value contains the stable version number of rvm?
# If you use a url, it must include http:// or https:// at the start.
# If you use a value, it should be something like 1.25.27 or any legal version number.

# If you want to lock rvm to a specific version you could enter in the version number
# that you have installed and the role would assume you always have the latest stable version.
rvm1_rvm_stable_version_number: 'https://raw.githubusercontent.com/wayneeseguin/rvm/master/VERSION'

# Force upgrade the rvm-installer to the latest version.
rvm1_rvm_force_upgrade_installer: false

# The amount in seconds to cache apt-update.
apt_cache_valid_time: 86400
```

## Exposed variables

You will likely want to use various ruby related commands in other roles. This role exposes a number of popular paths and variables that you can use.

- `rvm1_default_ruby_version`
    - The default ruby version that is selected

- `rvm1`
    - The path to the rvm binary

- `rvm1_default_wrappers`:
    - The path containing all of the wrapped ruby related binaries

- `rvm1_ruby`
    - The path to the ruby binary

- `rvm1_gem`
    - The path to the gem binary

- `rvm1_bundle`
    - The path to the bundle binary

- `rvm1_rake`
    - The path to the rake binary with bundle exec already applied

### Example

If you wanted to run a database migration in rails you would use `{{ rvm1_rake }} db:migrate`.

## Upgrading and removing old versions of ruby

A common work flow for upgrading your ruby version would be:

1. Install the new version
2. Run your application role so that bundle install re-installs your gems
3. Delete the previous version of ruby

You can delete a version of ruby with this role in 1 of 2 ways:

### Edit your inventory directly

You could overwrite `rvm1_delete_ruby: 'ruby-2.1.0'` and then run your play book. This would work but then you would have to go back and edit your inventory to make it an empty string afterwards.

The delete task is idempotent but it still requires you to make 2 edits.

### Leverage ansible's `--extra-vars`

Just add `--extra-vars 'rvm1_delete_ruby=ruby-2.1.0'` to the end of your play book command and that version will be removed without having to manually edit your inventory.

Not having to edit anything sure sounds better to me.

## Requirements

Tested on ubuntu 12.04 LTS but it should work on other versions that are similar.

## Ansible galaxy

You can find it on the official [ansible galaxy](https://galaxy.ansible.com/list#/roles/1087) if you want to rate it.

## License

MIT
