# ansible-template
A template structure for Ansible projects

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
