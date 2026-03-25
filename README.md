# [Whisparr][o]
Whisparr installation from public release tarball.

## [Requirements][i]
Requires [r_pufky.arr][g] galaxy-ng collection. See
[reference documentation][h] for troubleshooting and config variables.

Install size: ~240MB

See [FAQ for initial authentication configuration][p].

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
Existing databases are migrated automatically when new versions are installed.
Migration between database platforms is not supported and must be done
manually.

### NOTE
> New installs REQUIRE 'none' to create a login user; authentication can be
> toggled on after the user exists in the database. See
> [FAQ for initial authentication configuration][p].

### Database
PostgreSQL 14+ is highly recommended over default SQLite database.

Postgres requires pre-existing databases with root privileges to initialize
tables and manage migrations. Postgres databases are NOT backed up by Whisparr
and must be managed outside of this role. An example postgres [config.xml][m]
is provided within the role.

See [reference documentation][h] and [authoritative instructions][l].

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                     | Notes
 ------|--------------------------|-------
  1    | whisparr_flg_backup      | Backup config data. Exits role.
  2    | whisparr_flg_restore     | Restore config data. Exits role.
  3    | whisparr_flg_maintenance | Preform role maintenance tasks.
  4    | whisparr_flg_install     | Install required packages, users, etc.
  5    | whisparr_flg_config      | Install user-defined config.

### Example Playbooks
Whisparr will automatically generate a configuration file if one is not
provided. Example configuration files are located in [files][n].

#### New Deployments
``` yaml
# Config data: /var/opt/whisparr.
- name: 'Whisparr default install.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.whisparr'
```

#### Static Deployments
Config data is deployed as templates, allowing for vault use of sensitive
information. **However** this will deploy static files and overwrite existing
DB's, etc. Existing installs likely want to use **backup** and **restore**.

``` yaml
- name: 'Statically deploy Whisparr with pre-existing DB and config.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.whisparr'
  vars:
    whisparr_flg_config: true
    whisparr_cfg_dir: 'host_vars/whisparr.example.com/data'
```

#### Dynamic Deployments
Use **backup** and **restore** to carry over existing data to a new instance;
or leave whisparr_flg_config disabled to leave existing config untouched.

``` yaml
- name: 'Upgrade Whisparr version without touching configuration.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.whisparr'
  vars:
    whisparr_srv_version: 'v4.0.17.NEW_VERSION'
    whisparr_flg_config: false

- name: 'Create backup.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.whisparr'
  vars:
    whisparr_flg_backup: true
    whisparr_cfg_backup_dir: '/tmp'

- name: 'Restore from backup.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.whisparr'
  vars:
    whisparr_flg_restore: true
    whisparr_cfg_backup_dir: '/tmp'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

  Release | Debian | Ansible | Whisparr | Notes
 ---------|--------|---------|----------|-------
  5.x.x   | 13     | 2.20    | 2.2.0    | Ansible 2.20, feature flags, and semantic versioning.
  4.x.x   | 13     | 2.18    | N/A      | Migrate to r_pufky.arr.
  3.x.x   | 13     | 2.18    | N/A      | Migrate to Debian Trixie.
  2.x.x   | 12     | 2.18    | N/A      | Implement data annotations.
  1.x.x   | 12     | 2.11    | N/A      | Migration from private repository.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_whisparr/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_arr
[h]: https://r-pufky.github.io/docs/arr/whisparr
[i]: https://github.com/r-pufky/ansible_whisparr/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_whisparr/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_whisparr/blob/main/defaults/main/ports.yml
[l]: https://wiki.servarr.com/en/whisparr/postgres-setup
[m]: https://github.com/r-pufky/ansible_whisparr/blob/main/files/postgres/config.xml
[n]: https://github.com/r-pufky/ansible_whisparr/blob/main/files/
[o]: https://whisparr.com
[p]: https://wiki.servarr.com/whisparr/faq#help-i-have-locked-myself-out