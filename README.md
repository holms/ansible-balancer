balancer [![Build Status](https://travis-ci.org/holms/ansible-balancer.svg?branch=master)](https://travis-ci.org/holms/ansible-balancer)
====

Load Balancer with nginx role.

Feel free to contribute vhosts parameters.

Requirements
------------

Ansible version 1.6

Platforms
---------

* Ubuntu
* Debian

Role Variables
--------------

|Variable name    | Variable value
|-----------------|---------------
| balancer_vhosts | a list of vhosts

Example
-------

```
- hosts: localhost
  remote_user: root

  vars:
      - balancer_vhosts:
        - { name: "my.project.com",  
            upstreams: [
               'node01.myproject.com',
               'node02.myproject.com'
            ]}

  roles:
      - { role: balancer }
```

Or generate `upstreams` list from inventory vars group.

Inventory file:
```
[myproject]
node[01:02].myproject.com
```

Playbook:
```
- hosts: localhost
  remote_user: root
  vars:
      - balancer_vhosts:
        - { name: "my.project.com",  upstreams: "{{ groups.myproject }}" }
  roles:
      - { role: balancer }
```


License
-------

MIT

Author Information
------------------

Roman Gorodeckij (<holms@holms.lt>)
