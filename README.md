FORGE BIRT Reports
====================

Ansible playbook with two roles birtviewer and reports. 

1. The birtviewer role deploys Tomcat, SQL connectors and birt runtime webapp as a user (e.g. Ubuntu) that has ssh and sudo access to the target machine. This role also creates a birt user with authorized_ssh key (birt_user.pub)
2. Reports role deploys report designs for birt runtime. Reports are fetched from git as a birt user. Therefore the birt user has to have a correct private key in the target machines ~birt/.ssh/git_acess_key. You either transfer this file manually to the target machine or replace roles/birtviewer/keys/git_access_key with correct private key that can access git.

Check these
--------------------
 
group_vars/all.yml - Username defined here (E.g. Ubuntu) is being used to access target machine during birtviewer deployment. The user has to have a ssh access with sudo rights to the target machine.
roles/birtviewer/files - Contains birt runtime and mysql connector files. You might want to update these.

Before running the playbook
--------------------

roles/birtviewer/keys/birt_user.pub - Replace this file with the public key you want to use when making a ssh connection to target machine. This becomes autorized_key.
Note! You have to have a private key counterpart of birt_user.pub in your local ~/.ssh/id_rsa that can access the target machine user (E.g. Ubuntu)

After running the playbook for the first time
--------------------

Copy git_acess_key to target machine ~/birt/.ssh/git_acess_key manually.
Then rerun the playbook to again and it's able to fetch reports too with this key.

The inventory file
--------------------

This playbook targets at existing Tomcat node analytics.forgeservicelab.fi running birt-viwer web app. The node is configured in inventory file. Run the playbook as

    $ ansible-playbook -i inventory main.yml 

After running the playbook
--------------------

After running the playbook, the newly deployed BIRT reports are available at the target machine as follows.

   http://{{ target_ip }}/birt-viewer/run?__report={{ reports_dir }}/{{ report_name }}&sample=my+parameter   

   e.g.
   http://analytics.forgeservicelab.fi:8080/birt-viewer/run?__report=forge_birt_reports/forge_issues_status.rptdesign&sample=my+parameter
