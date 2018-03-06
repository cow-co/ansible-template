# ansible-template
A template structure for Ansible projects, adhering to [best practices](http://docs.ansible.com/ansible/latest/playbooks_best_practices.html#directory-layout)

## How to Use

This walkthrough will outline how to use the template to set up an Ansible project in a flexible and extensible manner.

### Inventory

The inventory file lists all your hosts' IPs. If you are executing instructions on different *types* of host, for example having some hosts as ElasticSearch instances and others as Kibana instances, then you can split the inventory into "groups". This will come into play later, as well. The format of the inventory file is as follows:

```
[group1_name]
255.255.255.255
1.2.3.4
[group2_name]
3.141.59.27
```

If you only have the one group, you needn't name it:

```
127.0.0.1
```

You can have different inventory files for different environments (e.g. `staging` and `production` inventories).

### Group_Vars

The `group_vars` directory contains subdirectories for each group (those from the `inventory`). These subdirectories contain YAML files defining variables which will be applied to all hosts in the given group. For example, you might want to point all your Kibana instances at the same set of ElasticSearch instances, so the `group_vars` is where you might place the `es_list` vairable to be passed to the Kibana hosts. It is good practice to split your variables into files based on the prupose of the variables. For example, if you have some variables relating to the installation of Kibana (e.g. installation directory), you would put these in a separate file (still in `group_vars/kibana`) from the variables related to configuring Kibana itself.

### Host_Vars

This directory houses files named according to the host on which they will be used. These files contain variables specific to the host in question (perhaps security setup, for example). Note that the files do *not* have `.yml` extensions! Also note that not every host has to have a file here.

### Library

If you have custom modules used in multiple roles, then put them here. If you have a custom module which is used only by one role, then put it in the `roles/role/library` directory.

### Roles

This is where the meat of the playbook logic goes. This will have subdirectories for each distinct "role" in the playbook. For example, you may have a role "deployElasticSearch" and another "deployKibana". Think of the broad operations that you want your playbook to do; each of these will likely become a separate role.

#### Roles/Role/Tasks

This directory is where the main logic of the role lies. Within it, there is a `main.yml`, which executes the core instructions for that role (for example, downloading ElasticSearch and extracting it). 

#### Roles/Role/Vars

This directory stores variables specific to the role in question (which may, after all, be different than the groups). For example, if you want to install Java on all your hosts, you might have a role "installJava" which will be executed on all hosts regardless of which group they belong to. Hence, any vars associated with installing Java (e.g. Java version) will go in `roles/installJava/vars/main.yml`.

#### Roles/Role/Templates

This is where any Jinja template files go.

#### Roles/Role/Handlers

This is where you define listeners; these are operations which are executed when specific events in the main role have occurred (e.g. restartin ElasticSearch once a configuration file has been successfully changed).

#### Roles/Role/Files

This directory stores any raw files which need to be sent to the host(s). For example, it might store the ElasticSearch tarball so that it can be uploaded to the host rather than the host having to download it from the Web.

#### Roles/Role/Meta

This directory stores dependencies for the given role.

#### Roles/Role/Defaults

This houses default values for variables (this is where you might store variables whose values will rarely be changed from the defaults).

#### Roles/Role/Library

This is where you put any custom modules specific to this role.

### Summary

If you adhere to this structure and format, then a lot of work will be taken care of for you by Ansible. Ansible will look for certain files in the places defined in the best practices, and by following those practices you are not only saving yourself a lot of effort (there will be far less explicit inclusion of files), but will make it a lot simpler to extend your Ansible project. 

For more information on other best practices, and some aspects of directory structure that I have purposely omitted here (namely `*_plugins` and `module_utils` directories), please see [the official documentation.](http://docs.ansible.com/ansible/latest/playbooks_best_practices.html#directory-layout) 

I hope this is helpful, and thanks for reading!