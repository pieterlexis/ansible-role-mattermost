Mattermost
==========

A role that installs and can configure [Mattermost](https://mattermost.com).

This role creates a secure installation of Mattermost, by installing it in `/opt/mattermost` and disallowing the user running mattermost to edit files in that directory.
Uploaded files are saved to `/srv/mattermost` and the configuration file is not writable by default.

Requirements
------------

An ansible 2.0+ installation and the [pieterlexis.json_file](https://galaxy.ansible.com/pieterlexis/json_file/) module.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    mattermost_version: 3.10.0
    mattermost_tar_checksum: 'sha256:ED64CB5357A8A3669386FD73B9A3F4934A10F0A9DA02DC4BE085E3D2E36886ED'

The version of Mattermost to install and the tarball's checksum.
This role installs Mattermost to `/opt/mattermost/{{ mattermost_version }}` and sym-links `/opt/mattermost/current` to this directory.
This allows for upgrades and downgrades.

    mattermost_listen_address: '127.0.0.1:8065'

The address and port Mattermost listens on.

    mattermost_database_driver: postgres
    mattermost_database_host: localhost
    mattermost_database_name: mattermost
    mattermost_database_user: mattermost
    mattermost_database_password:

The driver, host, database, username and password to connect to the database.
Only the "postgresql" driver is supported by this role at the moment.

    mattermost_database_create: true

Create the database and database user. Works only if the database is on localhost at the moment.

    mattermost_user_create: true
    mattermost_user: mattermost
    mattermost_group_create: true
    mattermost_group: mattermost

The system user and group to run mattermost under. The `*_create` variables dictate is the user and/or group should be created by this role.

    mattermost_config_writable: false

Whether or not the mattermost configuration file should we writable by the `mattermost_user`.
Setting this to `true` means that system administrators can use the system-console to change settings.
On the default, only admins with access to the system (the ones running Ansible) can change settings.

    mattermost_config: []

A list of dicts with configuration items and their values, see the [`json_file` documentation](https://github.com/pieterlexis/ansible-json_file/blob/master/README.md#usage-examples) and the Example Playbook below for more information.

Dependencies
------------

 * [pieterlexis.json_file](https://galaxy.ansible.com/pieterlexis/json_file/)

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: pieterlexis.mattermost,
         mattermost_config: [
          { key: 'EmailSettings.FeedbackEmail', value: 'noreply@example.com' },
          { key: 'EmailSettings.FeedbackName', value: 'Example.com Inc. Mattermost' },
          { key: 'EmailSettings.RequireEmailVerification', value: true },
          { key: 'EmailSettings.SMTPPort', value: "25", as_string: yes },
          { key: 'EmailSettings.SMTPServer', value: 'localhost' },
          { key: 'EmailSettings.SendEmailNotifications', value: true }],
         mattermost_database_host: 'db1.example.com',
         mattermost_database_password: 'Ex4mpl3' }
```

License
-------

MIT

Author Information
------------------

 * Pieter Lexis
