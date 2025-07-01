# Whisparr
Whisparr installation from public release tarball.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_whisparr/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_whisparr/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_whisparr/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.

## Example Playbook
Read defaults documentation.

Install latest release of Whisparr; ensuring media files have proper permissions.
Version (and databases) will be migrated and updated on new releases. Media
permissions will be set to ensure Whisparr can read files.
``` yaml
- name: 'whisparr server'
  hosts: 'whisparr.example.com'
  become: true
  roles:
     - 'r_pufky.whisparr'
  vars:
    whisparr_srv_version: 'latest'
    whisparr_cfg_api_key: '{{ vault_whisparr_api_key }}'
    whisparr_cfg_update_automatically: true
    whisparr_cfg_theme: 'dark'
    whisparr_media_root_folders:
      - '/data/media'
    whisparr_media_set_perms_file_enable: true
```

### Initial Deployment with No User
New installs REQUIRE 'none' to create a login user; authentication can be
toggled on after the user exists in the database . Suggest running initial role
application temporarily disabling this option:

``` bash
ansible-playbook whisparr.yml -e 'whisparr_cfg_authentication_method=none'
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_srv/blob/main/docs/dev/environment/README.md)

Run all unit tests:
``` bash
molecule test --all
```

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_whisparr/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
