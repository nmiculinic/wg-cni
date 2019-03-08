Role Name
=========

A brief description of the role goes here.

Requirements
------------

Installed CNI plugins on the target host.

Role Variables
--------------

See defaults/main.yml for documented variables description

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

CNI plugins need to be installed on the target machine.
Tested with version 0.6.0 and bridge + loopback plugins

Example Playbook
----------------

TODO

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

MIT

Author Information
------------------

Feel free to reach out via Issues, PRs or email
