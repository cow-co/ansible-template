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

### Group_Vars

The `group_vars` directory contains subdirectories for each group (those from the `inventory`). These subdirectories contain YAML files defining variables which will be applied to all hosts in the given group. For example, you might want to point all your Kibana instances at the same set of ElasticSearch instances, so the `group_vars` is where you might place the `es_list` vairable to be passed to the Kibana hosts.