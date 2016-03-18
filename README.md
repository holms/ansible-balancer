# balancer [![Build Status](https://travis-ci.org/holms/ansible-balancer.svg?branch=master)](https://travis-ci.org/holms/ansible-balancer)

Load Balancer with nginx role.

This role is only for SSL termination setup. Everything from HTTP will be redirected to HTTPS. You can fork and change that or you can contribute to make this optional.

## Requirements

Latest Ansible from pip

## Platforms

- Ubuntu
- Debian

## Role Variables

Name                     | Default                                                                                                                                              | Value
:----------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------- | :-----------------------------------------------
balancer_vhosts          | - { name: "example1",  upstreams: ['myserver1.com', 'myserver2.com' ] } <br> - { name: "example2",  upstreams: ['myserver1.com', 'myserver2.com' ] } | a list of vhosts
balancer_ssl_key         | *None*                                                                                                                                               | Path to SSL key
balancer_ssl_pem         | *None*                                                                                                                                               | Path to PEM file
balancer_ssl_crt         | *None*                                                                                                                                               | Path to CRT file
balancer_ssl_trusted_crt | *None*                                                                                                                                               | Path to CRT file of your trusted ssl certificate
balancer_ssl_trusted_key | *None*                                                                                                                                               | Path to KEY file of your trusted ssl certificate

You need to use either trusted certificates or self signed, using will fail your nginx config

## Example

```
- hosts: localhost
  remote_user: root

  vars:
      - balancer_ssl_key: "/etc/ssl/certs/server.key"
      - balancer_ssl_crt: "/etc/ssl/certs/server.crt"
      - balancer_ssl_pem: "/etc/ssl/certs/server.pem"
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
        - balancer_ssl_key: "/etc/ssl/certs/server.key"
        - balancer_ssl_crt: "/etc/ssl/certs/server.crt"
        - balancer_ssl_pem: "/etc/ssl/certs/server.pem"
        - balancer_vhosts:
            - { name: "my.project.com",  upstreams: "{{ groups.myproject }}" }
    roles:
      - { role: balancer }
```

## License

MIT

## Author Information

Roman Gorodeckij ([holms@holms.lt](mailto:holms@holms.lt))
