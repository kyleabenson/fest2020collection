# Ansible Collection - kyleabenson.fest2020collection

This collection exists to offer a simple demo of calling a collection. It provides 2 roles that can be called sequentially for the purpose of creating a simple, static website that is hosted on a VM deployed to Google Cloud Platform. The roles are:
- **gcp_deploy**: This deploys a simple compute instance to GCP before adding it to the runtime inventory
- **host_config**: This prepares the system to host a [static website](https://github.com/kyleabenson/fest2020frontEnd) built by the [Hugo](https://gohugo.io/) project, and then hosts it via a standard web server.

To use this collection it's recommended you setup a simple playbook to call the collections with the relevant variables and also include a requirements.yml file.

## Example playbook:
```
---
- name: Deploy GCP instanace
  hosts: localhost
  gather_facts: no
  collections:
    - kyleabenson.fest2020collection
  tasks:
    - import_role:
        name: gcp_deploy
      vars:  
        admin_user: kyleabenson
        #ssh_pub_key can be optionally set with the contents of a key or a lookup such as  "{{lookup('file', '~/.ssh/my_key.pub')}}" . If not used the play will generate a temporary ssh key to pass to next play when run locally
        ssh_pub_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCq77xtDlMuEPZUqlHdF+KpsPTSvycyhfXyJnMEIsVmxvANq6ENyE7RMpN819Zpx7vg2gRpjIPGGjx/jYW85SrTJB90a/AESQoa9mKPPSNbt6ElGNKsrQaZkcG/S0OFtwkl6n7SyMzHHOXxc3AEgy9NNjraWLBhqZ5JBc1pid/py33y3eYJnPzthOr2CpMQAWTxnbZSQ9nigJ3B3SN4xM50ZVa3bBNMDzSZqfrnWAfAYtLxLpauoO6OjH3RO+lbYhoCw0Gp3gde8DLV/c+TLDZAHJDmJLzHFBarEaTyRXVy8GALzvzesvJ++0JrqAEqc/RPeLeM4PhKEK4YBY6VlAv7Fn9Egg5xlP/EN9aK8wsQobVW6U7Izx2wNiejX4OpV2YIHtNTho0VdeEQ1RxYPSEYjURutx/6WCOqSqa/WHsYF8traqBZhEpgF0iVKf4uRzl39OyBtZsrFH1rbg4jmjPFqrU4pRRwcBHISwhMKPTVtNVMiLl1jxMWsJ0gF0qAxl0= kyleabenson"
        gcp_project: fest2020-289112
        # gcp_cred_kind: SET IF GCP_AUTH_KIND ENV VAR UNSET
        # gcp_cred_file: SET IF GCP_SERVICE_ACCOUNT_FILE ENV VAR UNSET
        gcp_region: us-east1
        gcp_zone: "{{ gcp_region }}-b"
        desired_state: present
      tags:
      - deploy


- name: Config deployed system
  hosts: all
  collections:
    - kyleabenson.fest2020collection
  tasks:
    - import_role:
        name: host_config
      vars:
        admin_user: kyleabenson
        book_title: "Be Kind To One Another"
        book:
            text:
                url: "http://gutenberg.org/cache/epub/63261/pg63261.txt"
                file: "/home/{{admin_user}}/fest2020frontEnd/content/post/{{ book_title |replace(' ','_')| lower()}}.md"
            image:
                url: "https://www.gutenberg.org/cache/epub/63261/pg63261.cover.medium.jpg"
                file: "/home/{{admin_user}}/fest2020frontEnd/static/images/{{ book_title |replace(' ','_')| lower()}}.jpg"
 
  tags: 
    - config
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

Cleanup
----
At the end of playbook creation you'll