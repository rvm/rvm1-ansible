[![Build Status](https://travis-ci.org/rvm/rvm1-ansible.svg?branch=master)](https://travis-ci.org/rvm/rvm1-ansible)
[![OpenCollective](https://opencollective.com/rvm/backers/badge.svg)](#backers)
[![OpenCollective](https://opencollective.com/rvm/sponsors/badge.svg)](#sponsors)
[![Ansible Role](https://img.shields.io/badge/role-rvm_io-red.svg)](https://galaxy.ansible.com/rvm/ruby)

## What is rvm1-ansible?

It is an [Ansible](http://www.ansible.com/home) role to install and manage ruby versions using rvm.

### Why should you use rvm?

In production it's useful because compiling a new version of ruby can easily
take upwards of 10 minutes. That's 10 minutes of your CPU being pegged at 100%.

rvm has pre-compiled binaries for a lot of operating systems. That means you can
install ruby in about 1 minute, even on a slow micro instance.

This role even adds the ruby binaries to your system path when doing a system
wide install. This allows you to access them as if they were installed without
using a version manager while still benefiting from what rvm has to offer.

## Installation

`$ ansible-galaxy install rvm.ruby`

## Role variables

Below is a list of default values that you can configure:

```yaml
---

# Install 1 or more versions of ruby
# The last ruby listed will be set as the default ruby
rvm1_rubies:
  - 'ruby-2.3.1'

# Install the bundler gem
rvm1_bundler_install: True

# Delete a specific version of ruby (ie. ruby-2.1.0)
rvm1_delete_ruby:

# Install path for rvm (defaults to single user)
# NOTE: If you are doing a ROOT BASED INSTALL then make sure you
#       set the install path to something like '/usr/local/rvm'
rvm1_install_path: '~/.rvm'

# Add or remove any install flags
# NOTE: If you are doing a ROOT BASED INSTALL then
#       make sure you REMOVE the --user-install flag below
rvm1_install_flags: '--auto-dotfiles  --user-install'

# Add additional ruby install flags
rvm1_ruby_install_flags:

# Set the owner for the rvm directory
# NOTE: If you are doing a ROOT BASED INSTALL then
#       make sure you set rvm1_user to 'root'
rvm1_user: 'ubuntu'

# URL for the latest installer script
rvm1_rvm_latest_installer: 'https://raw.githubusercontent.com/rvm/rvm/master/binscripts/rvm-installer'

# rvm version to use
rvm1_rvm_version: 'stable'

# Check and update rvm, disabling this will force rvm to never update
rvm1_rvm_check_for_updates: True

# GPG key verification, use an empty string if you want to skip this
# Note: Unless you know what you're doing, just keep it as is
#           Identity proof: https://keybase.io/mpapis
#           PGP message: https://rvm.io/mpapis.asc
rvm1_gpg_keys: '409B6B1796C275462A1703113804BB82D39DC0E3'

# The GPG key server
rvm1_gpg_key_server: 'hkp://pool.sks-keyservers.net'

# autolib mode, see https://rvm.io/rvm/autolibs
rvm1_autolib_mode: 3
```

## Example playbooks

```yaml
---

- name: Configure servers with ruby support for single user
  hosts: all

  roles:
    - { role: rvm.ruby,
        tags: ruby,
        rvm1_rubies: ['ruby-2.3.1'],
        rvm1_user: 'ubuntu'
      }
```

If you need to pass a list of ruby versions, pass it in an array like so.

```yaml
---
- name: Configure servers with ruby support system wide
  hosts: all
  roles:
    - { role: rvm.ruby,
        tags: ruby,
        become: yes,

        rvm1_rubies: ['ruby-2.2.5','ruby-2.3.1'],
        rvm1_install_flags: '--auto-dotfiles',     # Remove --user-install from defaults
        rvm1_install_path: /usr/local/rvm,         # Set to system location
        rvm1_user: root                            # Need root account to access system location
      }
```
_rvm_rubies must be specified via `ruby-x.x.x` so that if you want_
_ruby 2.2.5, you will need to pass in an array **rvm_rubies: ['ruby-2.2.5']**_


#### System wide installation

The above example would setup ruby system wide. It's very important that you run the
play as root because it will need to write to a system location specified by rvm1_install_path

#### To the same user as `ansible_user`

In this case, just overwrite `rvm_install_path` and by default is set the `--user-install` flag:

```yaml
rvm1_install_flags: '--auto-dotfiles --user-install'
rvm1_install_path: '/home/{{ ansible_user }}/.rvm'
```

#### To a user that is not `ansible_user`

You **will need root access here** because you will be writing outside the ansible
user's home directory. Other than that it's the same as above, except you will
supply a different user account:

```yaml
rvm1_install_flags: '--auto-dotfiles --user-install'
rvm1_install_path: '/home/someuser/.rvm'
```

#### A quick note about `rvm1_user`

In some cases you may want the rvm folder and its files to be owned by a specific
user instead of root. Simply set `rvm1_user: 'foo'` and when ruby gets installed
it will ensure that `foo` owns the rvm directory.

## Upgrading and removing old versions of ruby

A common work flow for upgrading your ruby version would be:

1. Install the new version
2. Run your application role so that bundle install re-installs your gems
3. Delete the previous version of ruby

### Leverage ansible's `--extra-vars`

Just add `--extra-vars 'rvm1_delete_ruby=ruby-2.1.0'` to the end of your play book command and that version will be removed.

## Requirements

- Tested on CentOS 6 and 7
- Tested on Debian 8 and 9
- Tested on Ubuntu 14.04 and 16.04

## Ansible galaxy

You can find it on the official [ansible galaxy](https://galaxy.ansible.com/rvm/ruby) if you want to rate it.

## Contributing

### Backers

[Become a backer](https://opencollective.com/rvm#backer) and support us with a small monthly donation to help us continue our activities.

[![Backer](https://opencollective.com/rvm/backer/0/avatar.svg)](https://opencollective.com/rvm/backer/0/website)
[![Backer](https://opencollective.com/rvm/backer/1/avatar.svg)](https://opencollective.com/rvm/backer/1/website)
[![Backer](https://opencollective.com/rvm/backer/2/avatar.svg)](https://opencollective.com/rvm/backer/2/website)
[![Backer](https://opencollective.com/rvm/backer/3/avatar.svg)](https://opencollective.com/rvm/backer/3/website)
[![Backer](https://opencollective.com/rvm/backer/4/avatar.svg)](https://opencollective.com/rvm/backer/4/website)
[![Backer](https://opencollective.com/rvm/backer/5/avatar.svg)](https://opencollective.com/rvm/backer/5/website)
[![Backer](https://opencollective.com/rvm/backer/6/avatar.svg)](https://opencollective.com/rvm/backer/6/website)
[![Backer](https://opencollective.com/rvm/backer/7/avatar.svg)](https://opencollective.com/rvm/backer/7/website)
[![Backer](https://opencollective.com/rvm/backer/8/avatar.svg)](https://opencollective.com/rvm/backer/8/website)
[![Backer](https://opencollective.com/rvm/backer/9/avatar.svg)](https://opencollective.com/rvm/backer/9/website)
[![Backer](https://opencollective.com/rvm/backer/10/avatar.svg)](https://opencollective.com/rvm/backer/10/website)
[![Backer](https://opencollective.com/rvm/backer/11/avatar.svg)](https://opencollective.com/rvm/backer/11/website)
[![Backer](https://opencollective.com/rvm/backer/12/avatar.svg)](https://opencollective.com/rvm/backer/12/website)
[![Backer](https://opencollective.com/rvm/backer/13/avatar.svg)](https://opencollective.com/rvm/backer/13/website)
[![Backer](https://opencollective.com/rvm/backer/14/avatar.svg)](https://opencollective.com/rvm/backer/14/website)
[![Backer](https://opencollective.com/rvm/backer/15/avatar.svg)](https://opencollective.com/rvm/backer/15/website)
[![Backer](https://opencollective.com/rvm/backer/16/avatar.svg)](https://opencollective.com/rvm/backer/16/website)
[![Backer](https://opencollective.com/rvm/backer/17/avatar.svg)](https://opencollective.com/rvm/backer/17/website)
[![Backer](https://opencollective.com/rvm/backer/18/avatar.svg)](https://opencollective.com/rvm/backer/18/website)
[![Backer](https://opencollective.com/rvm/backer/19/avatar.svg)](https://opencollective.com/rvm/backer/19/website)

### Sponsors

[Become a sponsor](https://opencollective.com/rvm#sponsor) and get your logo on our README on Github with a link to your site.

[![Sponsor](https://opencollective.com/rvm/sponsor/0/avatar.svg)](https://opencollective.com/rvm/sponsor/0/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/1/avatar.svg)](https://opencollective.com/rvm/sponsor/1/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/2/avatar.svg)](https://opencollective.com/rvm/sponsor/2/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/3/avatar.svg)](https://opencollective.com/rvm/sponsor/3/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/4/avatar.svg)](https://opencollective.com/rvm/sponsor/4/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/5/avatar.svg)](https://opencollective.com/rvm/sponsor/5/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/6/avatar.svg)](https://opencollective.com/rvm/sponsor/6/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/7/avatar.svg)](https://opencollective.com/rvm/sponsor/7/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/8/avatar.svg)](https://opencollective.com/rvm/sponsor/8/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/9/avatar.svg)](https://opencollective.com/rvm/sponsor/9/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/10/avatar.svg)](https://opencollective.com/rvm/sponsor/10/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/11/avatar.svg)](https://opencollective.com/rvm/sponsor/11/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/12/avatar.svg)](https://opencollective.com/rvm/sponsor/12/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/13/avatar.svg)](https://opencollective.com/rvm/sponsor/13/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/14/avatar.svg)](https://opencollective.com/rvm/sponsor/14/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/15/avatar.svg)](https://opencollective.com/rvm/sponsor/15/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/16/avatar.svg)](https://opencollective.com/rvm/sponsor/16/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/17/avatar.svg)](https://opencollective.com/rvm/sponsor/17/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/18/avatar.svg)](https://opencollective.com/rvm/sponsor/18/website)
[![Sponsor](https://opencollective.com/rvm/sponsor/19/avatar.svg)](https://opencollective.com/rvm/sponsor/19/website)

## License

MIT
