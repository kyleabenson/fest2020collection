Role Name
=========

A role to quickly configure a host for simple web hosting


Role Variables
--------------

```admin_user``` This must be a user that can SSH to the system and has the necessary sudo priviledges. If using as part of the larger fest collection demo, it must be the same user defined in the `gcp_deploy` role.


Example Playbook
----------------

```
- name: Config deployed system
  hosts: webservers
  collections:
    - kyleabenson.fest2020collection
  tasks:
    - import_role:
        name: host_config
      vars:
        admin_user: kyleabenson
```

License
-------

BSD

Author Information
------------------

Kyle Benson is a Principal Product Manager at Red Hat. 