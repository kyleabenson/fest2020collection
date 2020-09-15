# Ansible Collection - kyleabenson.fest2020collection

This collection exists to offer a simple demo of calling a collection. It provides 2 roles that can be called sequentially for the purpose of creating a simple, static website that is hosted on a VM deployed to Google Cloud Platform. The roles are:
- **gcp_deploy**: This deploys a simple compute instance to GCP before adding it to the runtime inventory
- **host_config**: This prepares the system to host a static website built by the Hugo project, and then hosts it via a standard web server.

To use this collection it's recommended you setup a simple playbook to call the collections with the relevant variables and also include a requirements.yml file.

## Example playbook:
```
---
- name: Deploy GCP instanace
  hosts: localhost
  collections:
    - kyleabenson.fest2020collection
  tasks:
    - import_role:
        name: gcp_deploy
      vars:  
        admin_user: kyleabenson
        ssh_pub_key: "{{lookup('file', '~/.ssh/my_key.pub')}}"
        gcp_project: festProject-2020
        gcp_cred_kind: serviceaccount
        gcp_cred_file: /path/to/my_cred_file.json
        gcp_region: us-east1
        gcp_zone: "{{ gcp_region }}-b"

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

## Example requirements.yml
```
---
collections:
  # Install a collection from Ansible Galaxy.
  - name: kyleabenson.fest2020collection
```

Before running the playbook, use the requirements file to install the collection:

`ansible-galaxy collection install -r requirements.yml -p ./`