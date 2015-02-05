FORGE BIRT Reports
====================

Ansible playbook with two roles birtviewer and reports. 

1. The birtviewer role deploys Tomcat, SQL connectors and birt runtime webapp as a user (e.g. Ubuntu) that has ssh and sudo access to the target machine. This role also creates a birt user with authorized_ssh key (birt.pub) and an authorized key for e.g. Jenkins CI machine if you want let Jenkins to access the target as birt user.
2. Reports role deploys report designs for birt runtime. Reports are fetched from git as a birt user. Therefore the birt user has to have a correct private key in the target machines ~birt/.ssh/{{ git_access_key }}. You either transfer this file manually to the target machine or let the role transfer it by having it in files/{{ git_access_key }} file and uncomment lines in ./roles/birtviewer/tasks/main.yml

Check these
--------------------
 
group_vars/all.yml - Username defined here (E.g. Ubuntu) is being used to access target machine during birtviewer deployment. The user has to have a ssh access with sudo rights to the target machine.
roles/birtviewer/files - Contains birt runtime and mysql connector files. You might want to update these.

Before running the playbook
--------------------

files/birt.pub - Replace this file with the public key you want to use when making a ssh connection to target machine. This becomes autorized_key and you should keep the secret key counterpart to connect.
files/jenkins.pub - Replace this file with the public key you want e.g. Jenkins to use when you let it to run this playbook. This becomes autorized_key too. Jenkins should have the secret key counterpart which it can use to connect.

After running the playbook for the first time
--------------------

Copy {{ git_acess_key }} to target machine ~/birt/.ssh/{{ git_acess_key }} manually and then then rerun the playbook to again and it's able to fetch reports from git with this key.  Another option is to put the key to files/{{ git_access_key }} and uncommented lines in /roles/birtviewer/tasks/main.yml and let the role handle key transfer then there is no need to rerun the playbook. If you let the Jenkins to run the playbook and use git, then make sure that the variable {{ git_access_key }} matches to the key that Jenkins has.

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
