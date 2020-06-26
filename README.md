BorgBackup with borgmatic Client
=========
![CI](https://github.com/jprltsnz/ansible-role-borgmatic/workflows/CI/badge.svg)


Sets up borgmatic to make per-application backups with optional encryption and compression. Currently supports Debian/Ubuntu and CentOS/Red Hat.

Mainly based of https://github.com/borgbase/ansible-role-borgbackup, but this role is a little more flexible and doesn't support borgbase.


Requirements
------------
The `respositories` parent directory from borgmatic_configs need to exist, or this role will fail.

Role Variables
--------------
- `borgmatic_init_encryption`: The encryption algorithm to use see https://torsion.org/borgmatic/docs/how-to/set-up-backups/#initialization by default it is set no `none`
`borgmatic_timer`: The `borgmatic.timer`'s `OnCalendar` value.
`borgmatic_configs`: A dict with the contents of borgmatic per application configuration file. This is copied verbatim to the borgmatic config folder. This role is set for per-application setup (https://torsion.org/borgmatic/docs/how-to/make-per-application-backups/) See https://torsion.org/borgmatic/docs/reference/configuration/ for a complete list of available configuration.

Dependencies
------------
This role requires `geerlingguy.repo-epel` to be installed — it will called appropriately when needed.


Example Playbook
----------------
``` yaml
- name: Converge
  hosts: all
  tasks:
    - name: "Include borgmatic"
      include_role:
        name: jprltsnz.borgmatic
      vars:
        borgmatic_configs:
          backup-etc:
            location:
              source_directories:
                - /etc
              repositories:
                - /srv/backup_etc
              atime: false
              exclude_patterns:
                - icon_cache
            retention:
              keep_daily: 7
              keep_weekly: 4
              keep_monthly: 12

          backup-home:
            location:
              source_directories:
                - /home
              repositories:
                - /srv/backup_home
              atime: false
              exclude_patterns:
                - icon_cache
            retention:
              keep_daily: 7
              keep_weekly: 4
              keep_monthly: 12
```


License
-------

MIT

