# Lab Work Task. Tomcat AS Provisioning
#### Review
 Using Ansible v2.2.1 for provisioning tomcat application stack. Learning by doing.
#### Task

#### On Host Node (Control Machine):

1. Install Ansible v2.3.1 with python pip. Report details where ansible has been installed.

```
[student@epbyminw2468 ansible]$ pip show ansible
Name: ansible
Version: 2.3.1.0
Summary: Radically simple IT automation
Home-page: https://ansible.com/
Author: Ansible, Inc.
Author-email: info@ansible.com
License: GPLv3
Location: /usr/lib/python2.7/site-packages
Requires: paramiko, pycrypto, setuptools, PyYAML, jinja2
```

2. Create folder ~/cm/ansible/day-1. All working files are supposed to be placed right there.

3. Spin up clear CentOS7 VM using vagrant (“vagrant init sbeliakou/centos-7.3-minimal”). Verify connectivity to the host using ssh keys (user: vagrant)
<img src="day-1/1.png">

4. Create ansible [inventory](day-1/inventory) file  with remote host connection details:

- Remote VM hostname/ip/port
- Remote ssh login username
- Connection type

```
ansible	ansible_port=22	ansible_host=192.168.56.10	ansible_connection=ssh	ansible_user=vagrant	ansible_ssh_private_key_file=.vagrant/machines/ansible/virtualbox/private_key
```

5. Test ansible connectivity to the VM with ad-hoc command: 

```
$ ansible ansible -i inventory -m setup
```

Find out host details:
- Number of CPUs
- Host name
- Host IP(s)
- Total RAM

<img src="day-1/2.png">

```
$ ansible-playbook playbook.yml -i inventory -m
```

<img src="day-1/3.png">

6. Develop a playbook [tomcat_provision.yml](day-1/tomcat_provision.yml) which is supposed to run against any host (specified in [inventory](day-1/inventory) )

Use following modules (at least):
* copy
* file
* get_url
* group
* service
* shell
* unarchive
* user
* yum

Define play variables (at least):

```
tomcat_version
java_version
```

Every task should have a name section with details of task purpose.
Examples:

```
name: Ensure user student exists
name: Fetch artifact form the Shared repository
```

Ensure tomcat is up and running properly with module “shell” (at least 3 different checks).
Second (and other) run(s) of playbook shouldn’t interrupt the service – one of checks should show tomcat uptime.

7. Software installation requirements:

Tomcat AS should be installed from sources (tar.gz) – download from the official site (http://archive.apache.org/dist/tomcat/).

Tomcat AS should be owned (and run) by user tomcat_as:tomcat_as_group

Tomcat AS version should be 8.x

Tomcat installation folder (CATALINA_HOME) is /opt/tomcat/$version, where $version is the version of tomcat defined in playbook

Java can be installed from CentOS Repositories

8. Verification Procedure: playbook will be checked by instructor’s CI system as follows:

  8.1. Connect to student’s host by ssh (username “student”) with own ssh key.

  8.2 Check the version of ansible installed on the system (as mentioned in point 1)

  8.3 Go into the folder mentioned in point 2

  8.4 Destroy/Launch VM: vagrant destroy && vagrant up

  8.5 Execute VM provisioning: ansible-playbook [tomcat_provision.yml](day-1/tomcat_provision.yml) -i [inventory](day-1/inventory) -vv 

  8.6 If previous steps are done successfully, instructor will check the report

9. Feedback: report issues/problems you had during the development of playbook and time spent for development.
