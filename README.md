# Ansible Role: Moodle

Installs Moodle on Ubuntu

## Requirements

Apache, MySQL, PHP

## Role Variables

```yml
---
# Mandatory Moodle params
moodle_db_name:     moodle
moodle_db_username: moodle
moodle_db_password: moodle
moodle_location:    /srv/moodle
moodle_dataroot:    /srv/moodledata
moodle_wwwroot:     http://moodle-vm.dev
moodle_version:     2.8

# If set to true, moodle will be assembled and configured, but not installed 
# (the database will not be touched).
moodle_assemble_only: false 

# Installation parameters (needed if you are not doing an assemble-only build)
moodle_adminuser:   root
moodle_adminpass:   root
moodle_adminemail:  you@example.com
moodle_fullname:    "Moodle Virtual Machine"
moodle_shortname:   "MoodleVM"
moodle_summary:     "A virtual machine for developing Moodle."

# Extra configuration. Optional configuration to write to config.php
moodle_extra_config:
  - name: divertallemailsto

    # Notice the double set of quotes. This is going to be output to a php file
    # like this:
    #
    # $CFG->{{ name }} = {{ value }};
    #
    # So, if the value is a string and therefor needs to be quoted, you need to
    # do it twice as the first set are ignored by YAML.
    value: "'you@company.com'"
  - name: debugdisplay
    value: 1
  - name: debug
    value: "(E_ALL | E_STRICT)"
  - name: noemailever
    value: "true"

# Where do you want to get the Moodle core code from? Available options are:
#
# git:      Get Moodle from github
# manifest: Defer to a Tasc Manifest
# local:    Don't get the code. Specify where the code is by setting 
#           moodle_location
moodle_install_method:  git

# Version of moodle to get if using git, defaults to HEAD
moodle_tag:             v2.8.5

# Moodle plugins from moodle.org or github
moodle_plugins:
  # This is an example of a plugin from moodle.org
  - name: block_rss_plus
    shortname: rss_plus
    url: "https://moodle.org/plugins/download.php/8660/block_rss_plus_moodle28_2015060101.zip"
    
    # Where to put the plugin
    destination: blocks
  
  # This is an example of a plugin hosted on github
  - name: theme_vision
    shortname: vision
    url: "https://github.com/VENDOR/PUBLIC_REPO.git"
    destination: theme

    # Optional, defaults to HEAD
    tag: v1.0.2
    service: github

  # An Example of a plugin hosted on a github private repo
  - name: local_mycoplugin
    shortname: mycoplugin
    url: "https://github.com/VENDOR/PRIVATE_REPO.git"
    destination: local
    service: github
    tag: v1.0.0
    
    # Your github personal access token.
    auth: "MYSECRETGITHUBTOKEN"

# If you are developing plugins, you can make use of an rsync script that will
# sync a folder on the server to a plugin installation location. This example
# uses a Vagrant location.
moodle_plugins_dev:
  -
    # Full frankenstyle name for the plugin
    name:           enrol_ldapgroup

    # Non frankenstyle shortname
    shortname:      ldapgroup

    # Where should we put this plugin relative to moodle root?
    destination:    enrol

    # Where is the plugin on the remote host?
    location:       /vagrant/plugins/enrol_ldapgroup
  - name:           theme_vision
    shortname:      vision
    destination:    theme
    location:       /vagrant/plugins/moodle-theme_vision

# Patches to apply to the Moodle installation. The "dest" should be relative to 
# playbook that is running this role.
moodle_patches:
  - src: patches/mod_forum_lib.php.patch
    dest: "{{ moodle_location }}/mod/forum/lib.php"
    
# This role now uses PyTasc the generic Tool for Assembling Source Code to 
# assemble Moodle. That means that you can specify a remote Manifest to use to
# build Moodle. Take a look at Tasc documentation for details on Manifast files:
# 
# <https://github.com/hcpss-banderson/py-tasc>
# 
# Your manifest and patches can be stored in a github repo:
moodle_manifest:
  type: git
  url: "https://github.com/hcpss-banderson/moodle-manifest.git"
  
  # Branch or tag
  version: master
  
  # Or in a remote zip file
  type: zip
  url: https://github.com/hcpss-banderson/moodle-manifest/archive/master.zip
```
