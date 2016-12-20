Role Name
=========

A role to install the postgreSQL server on RedHat or Debian based systems.
Filched from https://github.com/ANXS/postgresql

Requirements
------------

Ansible 2+


Role Variables
--------------

See defaults/main.yml for a thorough explanation of the main default variables.
OS family specific variables in vars/<os family>/yml

```yml
# Basic settings
postgresql_version: 9.3
postgresql_encoding: 'UTF-8'
postgresql_locale: 'en_US.UTF-8'
postgresql_ctype: 'en_US.UTF-8'

postgresql_admin_user: "postgres"
postgresql_default_auth_method: "trust"

postgresql_service_enabled: false # should the service be enabled, default is true

postgresql_cluster_name: "main"
postgresql_cluster_reset: false

# List of databases to be created (optional)
# Note: for more flexibility with extensions use the postgresql_database_extensions setting.
postgresql_databases:
  - name: foobar
    owner: baz          # optional; specify the owner of the database
    hstore: yes         # flag to install the hstore extension on this database (yes/no)
    uuid_ossp: yes      # flag to install the uuid-ossp extension on this database (yes/no)
    citext: yes         # flag to install the citext extension on this database (yes/no)
    encoding: 'UTF-8'   # override global {{ postgresql_encoding }} variable per database
    lc_collate: 'en_GB.UTF-8'   # override global {{ postgresql_locale }} variable per database
    lc_ctype: 'en_GB.UTF-8'     # override global {{ postgresql_ctype }} variable per database

# List of users to be created (optional)
postgresql_users:
  - name: baz
    pass: pass
    encrypted: no       # denotes if the password is already encrypted.

# List of user privileges to be applied (optional)
postgresql_user_privileges:
  - name: baz                   # user name
    db: foobar                  # database
    priv: "ALL"                 # privilege string format: example: INSERT,UPDATE/table:SELECT/anothertable:ALL
    role_attr_flags: "CREATEDB" # role attribute flags
```


Example Playbook
----------------

- hosts: all
  become: true
  vars_files:
    - ./postgresql/tests/vars.yml
  roles:
    - postgresql

License
-------

MIT
