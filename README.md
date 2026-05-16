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
kea_dhcp4_pxe_next_server: 192.168.30.100
kea_dhcp4_http_boot_url_base: http://192.168.30.100
kea_dhcp4_match_client_id: false
kea_dhcp4_dns_servers:
	- 192.168.30.100
tftpd_netboot_xyz_enable: true
httpboot_netboot_xyz_enable: true
```

dhcp グループの 1 台目が primary、2 台目が standby になる。Kea の HA 通信は各ホストの ansible_host を使って構成される。

PXE で netboot.xyz を返す場合、Kea は DHCP option 93 を見て BIOS と UEFI を切り替える。既定では以下のブートローダを TFTP に配置する。

- BIOS: netboot.xyz.kpxe
- UEFI x86_64 PXE: netboot.xyz-snponly.efi
- UEFI arm64 PXE: netboot.xyz-arm64-snponly.efi
- UEFI HTTP Boot x86_64: netboot.xyz.efi
- UEFI HTTP Boot arm64: netboot.xyz-arm64.efi

UEFI HTTP Boot を使う場合は、Kea が vendor class HTTPClient を見て HTTP URL を返す。既定では same VIP の 80/tcp で同じ EFI ファイルを配布するので、BIOS は TFTP、UEFI HTTP Boot 対応ホストは HTTP のまま併用できる。

必要なら inventory で以下の変数を上書きできる。

- kea_dhcp4_pxe_bios_boot_file_name
- kea_dhcp4_pxe_uefi_x64_boot_file_name
- kea_dhcp4_pxe_uefi_arm64_boot_file_name
- kea_dhcp4_http_boot_url_base
- kea_dhcp4_http_boot_uefi_x64_boot_file_name
- kea_dhcp4_http_boot_uefi_arm64_boot_file_name
- httpboot_netboot_xyz_enable
- tftpd_netboot_xyz_bootloaders
- tftpd_netboot_xyz_download_base_url
