# consul-release-windows

A BOSH release for running the Consul agent on Windows.

## Sample manifest

```yaml
name: consul-deployment
director_uuid: <uuid>

releases:
- name: consul-windows
  version: latest
- name: consul
  version: latest

networks:
- name: default
  subnets:
  - range: 172.16.1.0/24
    gateway: 172.16.1.1
    static: [172.16.1.2]
    dns: [172.16.1.1]
    cloud_properties:
      name: "network"

resource_pools:
- name: windows
  stemcell:
    name: bosh-vsphere-esxi-windows-2012R2-go_agent
    version: latest
  network: default
  cloud_properties:
    cpu: 2
    ram: 2_048
    disk: 2_048
    
- name: linux
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest
  network: default
  cloud_properties:
    cpu: 2
    ram: 2_048
    disk: 2_048

jobs:
- name: consul-windows-agent
  templates:
  - name: consul
    release: consul-windows
  instances: 1
  resource_pool: windows
  networks:
  - name: default

- name: consul-linux-server
  templates:
  - name: consul_agent
    release: consul
  instances: 1
  resource_pool: linux
  networks:
  - name: default
    static_ips:
    - 172.16.1.2
  properties:
    consul:
      agent:
        mode: server

properties:
  consul:
    agent:
      log_level:
      servers:
        lan:
        - 172.16.1.2
    require_ssl: false
```
