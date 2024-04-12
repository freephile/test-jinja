Test Jinja
=========

This role allows you to quickly see the various ways that Jinja whitespace is handled.

For example, see [./files/manual_blocks.md](./files/manual_blocks.md)

Requirements
------------

None

Role Variables
--------------

vars/main.yml has `output_dir` which is set to './files/' so that is where any output will go by default.

Dependencies
------------

none

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: freephile.test-jinja }

License
-------

GNU Affero

Author Information
------------------

Gregory Rundlett https://freephile.org/wiki
