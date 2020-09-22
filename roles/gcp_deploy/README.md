Role Name
=========

Deploy a simple compute instance to GCP for hosting purposes with a static and public IP addreess.

Requirements
------------

* The `google.cloud` collection -- installable via `ansible-galaxy collection install google.cloud`
* The Google Cloud command line utility, see [https://cloud.google.com/sdk](https://cloud.google.com/sdk) for details

Role Variables
--------------

All variables can be defined in `vars/main.yml` or overriden at the import of the role. ** ALL** of the below values must be defined.


```gcp_project``` Is the name of previously created GCP project that must be available

```gcp_cred_kind``` Specifies what type of credential being used

```gcp_cred_file```Specifies what credential file to be use. The credentials must have appropriate permissions to modify all necessary objects in the tasks.

```admin_user``` Specifies the name of the user you'd like to interact with the system

```ssh_pub_key``` Specifies the public portion of an ssh-key that corresponds with the above user

```gcp_region``` The region to deploy objects to

```gcp_zone```  The region variable plus it's corresponding zone. Ex if region is us-east1 then zone is us-east1-b

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

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
        gcp_source_image: "projects/rhel-cloud/global/images/rhel-8-v20200413"ubuntu-2004-focal-v20200907"yes
        gcp_region: us-east1
        gcp_zone: "{{ gcp_region }}-b"
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
