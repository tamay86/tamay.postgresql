Ansible Role: tamay.postgresql
=========

This role installs PostgreSQL.

Requirements
------------

None.

Role Variables
--------------

List of all variables, including default values.

    # Postgresql version
    # Leave empty to use centos default repo
    # Define version number to use postgres repo
    # Possible values: '', '94', '95', '96', '10', '11'
    postgresql_version: ''

Version of **postgresql** that should be installed. If the variable is left empty, then the version from the system repo manager will be installed. Otherwise it will use the packages from the [postgresql-repo](https://yum.postgresql.org/repopackages.php).
    
    # Authentication in pg_hba.conf
    postgresql_hba:
      - name: '"local" is for Unix domain socket connections only'
        type: local
        database: all
        user: all
        method: trust
      - name: 'IPv4 local connections:'
        type: host
        database: all
        user: all
        address: 127.0.0.1/32
        method: trust
      - name: 'IPv6 local connections:'
        type: host
        database: all
        user: all
        address: '::1/128'
        method: trust

```postgresql_hba``` is used to configure the content of ```pg_hba.conf```. This role will only add the entries defined in this variable, so take care to add above example, if you do not want to delete the three default connections.
    
    # Databases to create
    postgresql_databases: []
    # Example:
    #postgresql_databases:
    #  - name: test
    #  - name: test2
    #    encoding: UTF-8
    #    lc_collate: de_DE.UTF-8
    #    lc_ctype: de_DE.UTF-8
    #    template: template0
    #  - name: test3
    #    encoding: UTF-8
    #    lc_collate: en_US.UTF-8
    #    lc_ctype: en_US.UTF-8
    #    template: template0

Use ```postgresql_databases``` to create databases. Only **name** is mandatory.
    
    # Database users to create
    postgresql_users: []
    # Example:
    #postgresql_users:
    #  - name: test
    #    password: test
    #    db: test
    #    priv: ALL

Use ```postgresql_users``` to create users. Only **name** is mandatory.

Dependencies
------------

None.

Example Playbook
----------------

- Install postgresql version 10
- Create database "webserver" with en_US.UTF-8 locale
- Create user "webserver" with access to database "webserver"

Playbook:

    ---
    
    - hosts: all
    
      vars:
        postgresql_version: 10
        postgresql_databases:
          - name: webserver
            encoding: UTF-8
            lc_collate: en_US.UTF-8
            lc_ctype: en_US.UTF-8
            template: template0
        postgresql_users:
          - name: webserver
            db: webserver
            priv: ALL
    
      roles:
        - tamay.postgresql

License
-------

MIT

Author Information
------------------

tamay.mueller@gmail.com
