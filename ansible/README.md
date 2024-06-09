
# Overview

This ansible script will configure an interface and bgp on one `Junos` router running at...

```none
+----------------+
|   dal.edge01   |
|   172.20.20.3  |---{ge-0/0/1 unit 0}---
|                |
+----------------+
```

If you don't have a physical Junos device to run this against, the `clab` directory has a `containerlab` topology file that can be used to run a `vJunos-switch` image to simulate this topology.

The configuration for the router is stored in `host_vars/dal.edge01.yml`:

```yaml
hostname: dal.edge01
routerid: 192.0.2.255
interfaces:
  lo0 unit 0:
    verb: set
    family: inet
    address: 192.0.2.255/32
  ge-0/0/1 unit 0:
    verb: set
    family: inet
    address: 192.0.2.1/30
bgp:
  asn: 64000
  peers:
    group:
      verb: set
      name: SomePeerName
      asn: 64000
      description: "Connection to SomePeerName"
      local_address: 192.0.2.1
      neighbor: 192.0.2.2
```

To configure multiple routers, you just add the router into `inventory.ini` and then add a yaml file for it under `host_vars/`.


## Configuring the switch

- This ansible playbook should run on linux
- The linux system this runs on should have `python3` installed in a virtualenv
- `make install` to install all Ansible dependencies required by the playbook.
- Edit the credentials in `inventory.ini` to reflect the proper username and password
- `ansible-playbook -i inventory.ini playbook.yml`

## Assumptions

- Juniper switches are configured with: `set system services netconf ssh`
- Juniper switches allow the user `mpenning` to configure the device
- `mpenning` credentials are stored in the `inventory.ini` file

## How it works

For every yaml file under `host_vars/*.yml`, there should be a matching Ansible `inventory.ini` entry.

When the ansible playbook runs, the variables listed in `host_vars/*.yml` are fed into `templates/juniper_config.j2` and rendered as a Junos set configuration file.  Any number of routers, BGP peers, and any number of interfaces can be configured with a given Ansible playbook run.

That Junos set configuration file is used to configure the router.
