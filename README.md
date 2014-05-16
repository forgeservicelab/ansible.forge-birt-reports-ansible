FORGE BIRT Reports
==================

Ansible playbook to deploy FORGE BIRT reports to existing Tomcat running birt-viewer web app.

Prerequisites
-------------

- The target machine(s) have to be already instantiated and running Tomcat and birt-viwer web app as a redmine user.
- You have a ssh key to access target machine as a redmine user.

### The inventory file

This playbook targets at existing Tomcat node 193.166.24.102 running birt-viwer web app. The node is configured in inventory file. Run the playbook as

    $ ansible-playbook -i inventory main.yml 

### Configuration variables

This playbook requires a number of configuration variables which are pre defined except ssh key to access target machine. See vars/defaults for more info and copy the ssh key to vars/id_rsa before running the playbook.

### After running the playbook

After running the playbook, the newly deployed BIRT reports are available at the target machine as follows.

   http://[target machine]/birt-viewer/run?__report=[report name]&sample=my+parameter   

   e.g.
   http://193.166.24.102/birt-viewer/run?__report=forge_issues_status.rptdesign&sample=my+parameter
