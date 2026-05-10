# playbooks

Ansible Playbook

```
$ ansible-playbook playbooks/common.yaml
```

## kea

Kea DHCPv4 を hot-standby で使う場合は、inventory に dhcp グループを 2 台だけ定義し、各対象ホストか group_vars で以下を与える。

```yaml
kea_dhcp4_enable: true
kea_dhcp4_subnet: 192.168.30.0/24
kea_dhcp4_pool: 192.168.30.150 - 192.168.30.199
kea_dhcp4_router: 192.168.30.1
kea_dhcp4_dns_servers:
	- 192.168.30.100
```

dhcp グループの 1 台目が primary、2 台目が standby になる。Kea の HA 通信は各ホストの ansible_host を使って構成される。
