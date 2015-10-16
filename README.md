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

# Installation parameters
moodle_adminuser:   root
moodle_adminpass:   root
moodle_adminemail:  you@example.com
moodle_fullname:    "Moodle Virtual Machine"
moodle_shortname:   "MoodleVM"
moodle_summary:     "A virtual machine for developing Moodle."

# Version of moodle to get, defaults to HEAD
moodle_tag:         v2.8.5

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
    url: "https://github.com/HCPSS/moodle-theme_vision.git"
    destination: theme

    # Optional, defaults to HEAD
    tag: v1.0.2
    service: github

  # An Example of a plugin hosted on a github private repo
  - name: local_mycoplugin
    shortname: mycoplugin
    url: "https://github.com/VENDOR/REPO.git"
    destination: local
    service: github
    tag: v1.0.0
    
    # Your github personal access token. I'll use mine from above...
    auth: "MYSECRETGITHUBTOKEN"

# Patches to apply to the Moodle installation. The "patches" folder is supplied
# and ignored by .gitignore as a convenient place to put the patch files. But
# they can be put anywhere
moodle_patches:
  - src: patches/mod_forum_lib.php.patch
    dest: "{{ moodle_location }}/mod/forum/lib.php"
```
