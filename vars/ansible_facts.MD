# Ansible Facts
## What are Ansible Facts
Ansible facts are system properties and information automatically gathered by Ansible from the target machines. These facts include details like:

- OS type and version
- IP addresses
- Hostname
- Memory, disk, and CPU info
- Network interfaces
- Ansible version, and more

They are collected using the setup module and stored as variables that you can reference in your playbooks.

## When Are Facts Collected
By default, Ansible collects facts at the beginning of a playbook run.
You can disable or manually gather them:

- Disable: gather_facts: false
- Manually gather: use setup module explicitly

## How to use ansible facts

>[!WARNING]
>When u going to use the ansible facts, then don use the ansible_ in your var name

You can use the ansible facts as following
- ansible_facts['apparmor']['status']
- ansible_facts.apparmor.status

## Example of ansible facts

```JSON
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "10.11.0.2",
            "10.0.0.10"
        ],
        "ansible_all_ipv6_addresses": [],
        "ansible_apparmor": {
            "status": "enabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_bios_date": "04/01/2014",
        "ansible_bios_vendor": "SeaBIOS",
        "ansible_bios_version": "rel-1.16.3-0-ga6ed6b701f0a-prebuilt.qemu.org",
        "ansible_board_asset_tag": "NA",
        "ansible_board_name": "NA",
        "ansible_board_serial": "NA",
        "ansible_board_vendor": "NA",
        "ansible_board_version": "NA",
        "ansible_chassis_asset_tag": "NA",
        "ansible_chassis_serial": "NA",
        "ansible_chassis_vendor": "QEMU",
        "ansible_chassis_version": "pc-i440fx-9.2",
        "ansible_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-6.11.0-25-generic",
            "quiet": true,
            "ro": true,
            "root": "UUID=c9217b02-0af4-488d-a553-4701b01c390b",
            "splash": true,
            "vt.handoff": "7"
        },
        "ansible_date_time": {
            "date": "2025-05-22",
            "day": "22",
            "epoch": "1747898696",
            "epoch_int": "1747898696",
            "hour": "09",
            "iso8601": "2025-05-22T07:24:56Z",
            "iso8601_basic": "20250522T092456208367",
            "iso8601_basic_short": "20250522T092456",
            "iso8601_micro": "2025-05-22T07:24:56.208367Z",
            "minute": "24",
            "month": "05",
            "second": "56",
            "time": "09:24:56",
            "tz": "CEST",
            "tz_dst": "CEST",
            "tz_offset": "+0200",
            "weekday": "Thursday",
            "weekday_number": "4",
            "weeknumber": "20",
            "year": "2025"
        },
        "ansible_default_ipv4": {
            "address": "10.0.0.10",
            "alias": "ens18",
            "broadcast": "10.0.0.255",
            "gateway": "10.0.0.1",
            "interface": "ens18",
            "macaddress": "bc:24:11:35:b8:cc",
            "mtu": 1500,
            "netmask": "255.255.255.0",
            "network": "10.0.0.0",
            "prefix": "24",
            "type": "ether"
        },
        "ansible_default_ipv6": {},
        "ansible_device_links": {
            "ids": {
                "sda": [
                    "scsi-0QEMU_QEMU_HARDDISK_drive-scsi0"
                ],
                "sda1": [
                    "scsi-0QEMU_QEMU_HARDDISK_drive-scsi0-part1"
                ],
                "sda2": [
                    "scsi-0QEMU_QEMU_HARDDISK_drive-scsi0-part2"
                ],
                "sr0": [
                    "ata-QEMU_DVD-ROM_QM00003"
                ]
            },
            "labels": {
                "sr0": [
                    "Ubuntu\\x2024.04.2\\x20LTS\\x20amd64"
                ]
            },
            "masters": {},
            "uuids": {
                "sda2": [
                    "c9217b02-0af4-488d-a553-4701b01c390b"
                ],
                "sr0": [
                    "2025-02-15-09-15-26-00"
                ]
            }
        },
        "ansible_devices": {
            "loop0": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "8",
                "sectorsize": "512",
                "size": "4.00 KB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop1": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "151328",
                "sectorsize": "512",
                "size": "73.89 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop10": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "91008",
                "sectorsize": "512",
                "size": "44.44 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop11": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "104232",
                "sectorsize": "512",
                "size": "50.89 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop12": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "1136",
                "sectorsize": "512",
                "size": "568.00 KB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop13": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "495512",
                "sectorsize": "512",
                "size": "241.95 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop2": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "0",
                "sectorsize": "512",
                "size": "0.00 Bytes",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop3": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "151328",
                "sectorsize": "512",
                "size": "73.89 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop4": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "495504",
                "sectorsize": "512",
                "size": "241.95 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop5": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "22800",
                "sectorsize": "512",
                "size": "11.13 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop6": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "1056784",
                "sectorsize": "512",
                "size": "516.01 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop7": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "187776",
                "sectorsize": "512",
                "size": "91.69 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop8": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "22056",
                "sectorsize": "512",
                "size": "10.77 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop9": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "22176",
                "sectorsize": "512",
                "size": "10.83 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "sda": {
                "holders": [],
                "host": "SCSI storage controller: Red Hat, Inc. Virtio SCSI",
                "links": {
                    "ids": [
                        "scsi-0QEMU_QEMU_HARDDISK_drive-scsi0"
                    ],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": "QEMU HARDDISK",
                "partitions": {
                    "sda1": {
                        "holders": [],
                        "links": {
                            "ids": [
                                "scsi-0QEMU_QEMU_HARDDISK_drive-scsi0-part1"
                            ],
                            "labels": [],
                            "masters": [],
                            "uuids": []
                        },
                        "sectors": "2048",
                        "sectorsize": 512,
                        "size": "1.00 MB",
                        "start": "2048",
                        "uuid": null
                    },
                    "sda2": {
                        "holders": [],
                        "links": {
                            "ids": [
                                "scsi-0QEMU_QEMU_HARDDISK_drive-scsi0-part2"
                            ],
                            "labels": [],
                            "masters": [],
                            "uuids": [
                                "c9217b02-0af4-488d-a553-4701b01c390b"
                            ]
                        },
                        "sectors": "209709056",
                        "sectorsize": 512,
                        "size": "100.00 GB",
                        "start": "4096",
                        "uuid": "c9217b02-0af4-488d-a553-4701b01c390b"
                    }
                },
                "removable": "0",
                "rotational": "1",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "209715200",
                "sectorsize": "512",
                "size": "100.00 GB",
                "support_discard": "4096",
                "vendor": "QEMU",
                "virtual": 1
            },
            "sr0": {
                "holders": [],
                "host": "IDE interface: Intel Corporation 82371SB PIIX3 IDE [Natoma/Triton II]",
                "links": {
                    "ids": [
                        "ata-QEMU_DVD-ROM_QM00003"
                    ],
                    "labels": [
                        "Ubuntu\\x2024.04.2\\x20LTS\\x20amd64"
                    ],
                    "masters": [],
                    "uuids": [
                        "2025-02-15-09-15-26-00"
                    ]
                },
                "model": "QEMU DVD-ROM",
                "partitions": {},
                "removable": "1",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "mq-deadline",
                "sectors": "12389100",
                "sectorsize": "2048",
                "size": "5.91 GB",
                "support_discard": "2048",
                "vendor": "QEMU",
                "virtual": 1
            }
        },
        "ansible_distribution": "Ubuntu",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "24",
        "ansible_distribution_release": "noble",
        "ansible_distribution_version": "24.04",
        "ansible_dns": {
            "nameservers": [
                "127.0.0.53"
            ],
            "options": {
                "edns0": true,
                "trust-ad": true
            },
            "search": [
                "gmartens.home"
            ]
        },
        "ansible_domain": "",
        "ansible_effective_group_id": 1000,
        "ansible_effective_user_id": 1000,
        "ansible_ens18": {
            "active": true,
            "device": "ens18",
            "features": {
                "esp_hw_offload": "off [fixed]",
                "esp_tx_csum_hw_offload": "off [fixed]",
                "fcoe_mtu": "off [fixed]",
                "generic_receive_offload": "on",
                "generic_segmentation_offload": "on",
                "highdma": "on [fixed]",
                "hsr_dup_offload": "off [fixed]",
                "hsr_fwd_offload": "off [fixed]",
                "hsr_tag_ins_offload": "off [fixed]",
                "hsr_tag_rm_offload": "off [fixed]",
                "hw_tc_offload": "off [fixed]",
                "l2_fwd_offload": "off [fixed]",
                "large_receive_offload": "off [fixed]",
                "loopback": "off [fixed]",
                "macsec_hw_offload": "off [fixed]",
                "netns_local": "off [fixed]",
                "ntuple_filters": "off [fixed]",
                "receive_hashing": "off [fixed]",
                "rx_all": "off [fixed]",
                "rx_checksumming": "on [fixed]",
                "rx_fcs": "off [fixed]",
                "rx_gro_hw": "on",
                "rx_gro_list": "off",
                "rx_udp_gro_forwarding": "off",
                "rx_udp_tunnel_port_offload": "off [fixed]",
                "rx_vlan_filter": "on [fixed]",
                "rx_vlan_offload": "off [fixed]",
                "rx_vlan_stag_filter": "off [fixed]",
                "rx_vlan_stag_hw_parse": "off [fixed]",
                "scatter_gather": "on",
                "tcp_segmentation_offload": "on",
                "tls_hw_record": "off [fixed]",
                "tls_hw_rx_offload": "off [fixed]",
                "tls_hw_tx_offload": "off [fixed]",
                "tx_checksum_fcoe_crc": "off [fixed]",
                "tx_checksum_ip_generic": "on",
                "tx_checksum_ipv4": "off [fixed]",
                "tx_checksum_ipv6": "off [fixed]",
                "tx_checksum_sctp": "off [fixed]",
                "tx_checksumming": "on",
                "tx_esp_segmentation": "off [fixed]",
                "tx_fcoe_segmentation": "off [fixed]",
                "tx_gre_csum_segmentation": "off [fixed]",
                "tx_gre_segmentation": "off [fixed]",
                "tx_gso_list": "off [fixed]",
                "tx_gso_partial": "off [fixed]",
                "tx_gso_robust": "on [fixed]",
                "tx_ipxip4_segmentation": "off [fixed]",
                "tx_ipxip6_segmentation": "off [fixed]",
                "tx_lockless": "off [fixed]",
                "tx_nocache_copy": "off",
                "tx_scatter_gather": "on",
                "tx_scatter_gather_fraglist": "off [fixed]",
                "tx_sctp_segmentation": "off [fixed]",
                "tx_tcp6_segmentation": "on",
                "tx_tcp_ecn_segmentation": "on",
                "tx_tcp_mangleid_segmentation": "off",
                "tx_tcp_segmentation": "on",
                "tx_tunnel_remcsum_segmentation": "off [fixed]",
                "tx_udp_segmentation": "off",
                "tx_udp_tnl_csum_segmentation": "off [fixed]",
                "tx_udp_tnl_segmentation": "off [fixed]",
                "tx_vlan_offload": "off [fixed]",
                "tx_vlan_stag_hw_insert": "off [fixed]",
                "vlan_challenged": "off [fixed]"
            },
            "hw_timestamp_filters": [],
            "ipv4": {
                "address": "10.0.0.10",
                "broadcast": "10.0.0.255",
                "netmask": "255.255.255.0",
                "network": "10.0.0.0",
                "prefix": "24"
            },
            "macaddress": "bc:24:11:35:b8:cc",
            "mtu": 1500,
            "pciid": "virtio1",
            "promisc": false,
            "speed": -1,
            "timestamping": [],
            "type": "ether"
        },
        "ansible_ens19": {
            "active": true,
            "device": "ens19",
            "features": {
                "esp_hw_offload": "off [fixed]",
                "esp_tx_csum_hw_offload": "off [fixed]",
                "fcoe_mtu": "off [fixed]",
                "generic_receive_offload": "on",
                "generic_segmentation_offload": "on",
                "highdma": "on [fixed]",
                "hsr_dup_offload": "off [fixed]",
                "hsr_fwd_offload": "off [fixed]",
                "hsr_tag_ins_offload": "off [fixed]",
                "hsr_tag_rm_offload": "off [fixed]",
                "hw_tc_offload": "off [fixed]",
                "l2_fwd_offload": "off [fixed]",
                "large_receive_offload": "off [fixed]",
                "loopback": "off [fixed]",
                "macsec_hw_offload": "off [fixed]",
                "netns_local": "off [fixed]",
                "ntuple_filters": "off [fixed]",
                "receive_hashing": "off [fixed]",
                "rx_all": "off [fixed]",
                "rx_checksumming": "on [fixed]",
                "rx_fcs": "off [fixed]",
                "rx_gro_hw": "on",
                "rx_gro_list": "off",
                "rx_udp_gro_forwarding": "off",
                "rx_udp_tunnel_port_offload": "off [fixed]",
                "rx_vlan_filter": "on [fixed]",
                "rx_vlan_offload": "off [fixed]",
                "rx_vlan_stag_filter": "off [fixed]",
                "rx_vlan_stag_hw_parse": "off [fixed]",
                "scatter_gather": "on",
                "tcp_segmentation_offload": "on",
                "tls_hw_record": "off [fixed]",
                "tls_hw_rx_offload": "off [fixed]",
                "tls_hw_tx_offload": "off [fixed]",
                "tx_checksum_fcoe_crc": "off [fixed]",
                "tx_checksum_ip_generic": "on",
                "tx_checksum_ipv4": "off [fixed]",
                "tx_checksum_ipv6": "off [fixed]",
                "tx_checksum_sctp": "off [fixed]",
                "tx_checksumming": "on",
                "tx_esp_segmentation": "off [fixed]",
                "tx_fcoe_segmentation": "off [fixed]",
                "tx_gre_csum_segmentation": "off [fixed]",
                "tx_gre_segmentation": "off [fixed]",
                "tx_gso_list": "off [fixed]",
                "tx_gso_partial": "off [fixed]",
                "tx_gso_robust": "on [fixed]",
                "tx_ipxip4_segmentation": "off [fixed]",
                "tx_ipxip6_segmentation": "off [fixed]",
                "tx_lockless": "off [fixed]",
                "tx_nocache_copy": "off",
                "tx_scatter_gather": "on",
                "tx_scatter_gather_fraglist": "off [fixed]",
                "tx_sctp_segmentation": "off [fixed]",
                "tx_tcp6_segmentation": "on",
                "tx_tcp_ecn_segmentation": "on",
                "tx_tcp_mangleid_segmentation": "off",
                "tx_tcp_segmentation": "on",
                "tx_tunnel_remcsum_segmentation": "off [fixed]",
                "tx_udp_segmentation": "off",
                "tx_udp_tnl_csum_segmentation": "off [fixed]",
                "tx_udp_tnl_segmentation": "off [fixed]",
                "tx_vlan_offload": "off [fixed]",
                "tx_vlan_stag_hw_insert": "off [fixed]",
                "vlan_challenged": "off [fixed]"
            },
            "hw_timestamp_filters": [],
            "ipv4": {
                "address": "10.11.0.2",
                "broadcast": "255.255.255.255",
                "netmask": "0.0.0.0",
                "network": "0.0.0.0",
                "prefix": "0"
            },
            "macaddress": "bc:24:11:8e:0d:ad",
            "mtu": 1500,
            "pciid": "virtio2",
            "promisc": false,
            "speed": -1,
            "timestamping": [],
            "type": "ether"
        },
        "ansible_env": {
            "DBUS_SESSION_BUS_ADDRESS": "unix:path=/run/user/1000/bus",
            "DEBUGINFOD_URLS": "https://debuginfod.ubuntu.com ",
            "HOME": "/home/gmartens",
            "LANG": "en_US.UTF-8",
            "LESSCLOSE": "/usr/bin/lesspipe %s %s",
            "LESSOPEN": "| /usr/bin/lesspipe %s",
            "LOGNAME": "gmartens",
            "LS_COLORS": "rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=00:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.avif=01;35:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:*~=00;90:*#=00;90:*.bak=00;90:*.crdownload=00;90:*.dpkg-dist=00;90:*.dpkg-new=00;90:*.dpkg-old=00;90:*.dpkg-tmp=00;90:*.old=00;90:*.orig=00;90:*.part=00;90:*.rej=00;90:*.rpmnew=00;90:*.rpmorig=00;90:*.rpmsave=00;90:*.swp=00;90:*.tmp=00;90:*.ucf-dist=00;90:*.ucf-new=00;90:*.ucf-old=00;90:",
            "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin",
            "PWD": "/home/gmartens",
            "SHELL": "/bin/bash",
            "SHLVL": "1",
            "SSH_CLIENT": "10.0.0.154 57232 22",
            "SSH_CONNECTION": "10.0.0.154 57232 10.11.0.2 22",
            "SSH_TTY": "/dev/pts/1",
            "TERM": "xterm",
            "USER": "gmartens",
            "XDG_DATA_DIRS": "/usr/share/gnome:/usr/local/share:/usr/share:/var/lib/snapd/desktop",
            "XDG_RUNTIME_DIR": "/run/user/1000",
            "XDG_SESSION_CLASS": "user",
            "XDG_SESSION_ID": "717",
            "XDG_SESSION_TYPE": "tty",
            "_": "/usr/bin/ansible"
        },
        "ansible_fibre_channel_wwn": [],
        "ansible_fips": false,
        "ansible_form_factor": "Other",
        "ansible_fqdn": "stepstone",
        "ansible_hostname": "stepstone",
        "ansible_hostnqn": "",
        "ansible_interfaces": [
            "ens19",
            "lo",
            "ens18"
        ],
        "ansible_is_chroot": false,
        "ansible_iscsi_iqn": "",
        "ansible_kernel": "6.11.0-25-generic",
        "ansible_kernel_version": "#25~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Apr 15 17:20:50 UTC 2",
        "ansible_lo": {
            "active": true,
            "device": "lo",
            "features": {
                "esp_hw_offload": "off [fixed]",
                "esp_tx_csum_hw_offload": "off [fixed]",
                "fcoe_mtu": "off [fixed]",
                "generic_receive_offload": "on",
                "generic_segmentation_offload": "on",
                "highdma": "on [fixed]",
                "hsr_dup_offload": "off [fixed]",
                "hsr_fwd_offload": "off [fixed]",
                "hsr_tag_ins_offload": "off [fixed]",
                "hsr_tag_rm_offload": "off [fixed]",
                "hw_tc_offload": "off [fixed]",
                "l2_fwd_offload": "off [fixed]",
                "large_receive_offload": "off [fixed]",
                "loopback": "on [fixed]",
                "macsec_hw_offload": "off [fixed]",
                "netns_local": "on [fixed]",
                "ntuple_filters": "off [fixed]",
                "receive_hashing": "off [fixed]",
                "rx_all": "off [fixed]",
                "rx_checksumming": "on [fixed]",
                "rx_fcs": "off [fixed]",
                "rx_gro_hw": "off [fixed]",
                "rx_gro_list": "off",
                "rx_udp_gro_forwarding": "off",
                "rx_udp_tunnel_port_offload": "off [fixed]",
                "rx_vlan_filter": "off [fixed]",
                "rx_vlan_offload": "off [fixed]",
                "rx_vlan_stag_filter": "off [fixed]",
                "rx_vlan_stag_hw_parse": "off [fixed]",
                "scatter_gather": "on",
                "tcp_segmentation_offload": "on",
                "tls_hw_record": "off [fixed]",
                "tls_hw_rx_offload": "off [fixed]",
                "tls_hw_tx_offload": "off [fixed]",
                "tx_checksum_fcoe_crc": "off [fixed]",
                "tx_checksum_ip_generic": "on [fixed]",
                "tx_checksum_ipv4": "off [fixed]",
                "tx_checksum_ipv6": "off [fixed]",
                "tx_checksum_sctp": "on [fixed]",
                "tx_checksumming": "on",
                "tx_esp_segmentation": "off [fixed]",
                "tx_fcoe_segmentation": "off [fixed]",
                "tx_gre_csum_segmentation": "off [fixed]",
                "tx_gre_segmentation": "off [fixed]",
                "tx_gso_list": "on",
                "tx_gso_partial": "off [fixed]",
                "tx_gso_robust": "off [fixed]",
                "tx_ipxip4_segmentation": "off [fixed]",
                "tx_ipxip6_segmentation": "off [fixed]",
                "tx_lockless": "on [fixed]",
                "tx_nocache_copy": "off [fixed]",
                "tx_scatter_gather": "on [fixed]",
                "tx_scatter_gather_fraglist": "on [fixed]",
                "tx_sctp_segmentation": "on",
                "tx_tcp6_segmentation": "on",
                "tx_tcp_ecn_segmentation": "on",
                "tx_tcp_mangleid_segmentation": "on",
                "tx_tcp_segmentation": "on",
                "tx_tunnel_remcsum_segmentation": "off [fixed]",
                "tx_udp_segmentation": "on",
                "tx_udp_tnl_csum_segmentation": "off [fixed]",
                "tx_udp_tnl_segmentation": "off [fixed]",
                "tx_vlan_offload": "off [fixed]",
                "tx_vlan_stag_hw_insert": "off [fixed]",
                "vlan_challenged": "on [fixed]"
            },
            "hw_timestamp_filters": [],
            "ipv4": {
                "address": "127.0.0.1",
                "broadcast": "",
                "netmask": "255.0.0.0",
                "network": "127.0.0.0",
                "prefix": "8"
            },
            "ipv6": [
                {
                    "address": "::1",
                    "prefix": "128",
                    "scope": "host"
                }
            ],
            "mtu": 65536,
            "promisc": false,
            "timestamping": [],
            "type": "loopback"
        },
        "ansible_loadavg": {
            "15m": 0.79345703125,
            "1m": 1.12109375,
            "5m": 1.3359375
        },
        "ansible_local": {},
        "ansible_locally_reachable_ips": {
            "ipv4": [
                "10.0.0.10",
                "10.11.0.2",
                "127.0.0.0/8",
                "127.0.0.1"
            ],
            "ipv6": [
                "::1"
            ]
        },
        "ansible_lsb": {
            "codename": "noble",
            "description": "Ubuntu 24.04.2 LTS",
            "id": "Ubuntu",
            "major_release": "24",
            "release": "24.04"
        },
        "ansible_lvm": "N/A",
        "ansible_machine": "x86_64",
        "ansible_machine_id": "3df41f311d184662abf0b5c1e8310342",
        "ansible_memfree_mb": 3333,
        "ansible_memory_mb": {
            "nocache": {
                "free": 6337,
                "used": 1603
            },
            "real": {
                "free": 3333,
                "total": 7940,
                "used": 4607
            },
            "swap": {
                "cached": 0,
                "free": 4095,
                "total": 4095,
                "used": 0
            }
        },
        "ansible_memtotal_mb": 7940,
        "ansible_mounts": [
            {
                "block_available": 21412306,
                "block_size": 4096,
                "block_total": 25655790,
                "block_used": 4243484,
                "device": "/dev/sda2",
                "fstype": "ext4",
                "inode_available": 6304185,
                "inode_total": 6553600,
                "inode_used": 249415,
                "mount": "/",
                "options": "rw,relatime",
                "size_available": 87704805376,
                "size_total": 105086115840,
                "uuid": "c9217b02-0af4-488d-a553-4701b01c390b"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 1,
                "block_used": 1,
                "device": "/dev/loop0",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 29,
                "inode_used": 29,
                "mount": "/snap/bare/5",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 131072,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 87,
                "block_used": 87,
                "device": "/dev/loop9",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 68,
                "inode_used": 68,
                "mount": "/snap/snap-store/1270",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 11403264,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 90,
                "block_used": 90,
                "device": "/dev/loop5",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 65,
                "inode_used": 65,
                "mount": "/snap/firmware-updater/167",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 11796480,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 87,
                "block_used": 87,
                "device": "/dev/loop8",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 68,
                "inode_used": 68,
                "mount": "/snap/snap-store/1248",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 11403264,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 5,
                "block_used": 5,
                "device": "/dev/loop12",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 61,
                "inode_used": 61,
                "mount": "/snap/snapd-desktop-integration/253",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 655360,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 592,
                "block_used": 592,
                "device": "/dev/loop1",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 14262,
                "inode_used": 14262,
                "mount": "/snap/core22/1748",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 77594624,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 592,
                "block_used": 592,
                "device": "/dev/loop3",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 14261,
                "inode_used": 14261,
                "mount": "/snap/core22/1963",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 77594624,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 734,
                "block_used": 734,
                "device": "/dev/loop7",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 76208,
                "inode_used": 76208,
                "mount": "/snap/gtk-common-themes/1535",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 96206848,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 1936,
                "block_used": 1936,
                "device": "/dev/loop4",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 812,
                "inode_used": 812,
                "mount": "/snap/firefox/6159",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 253755392,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 356,
                "block_used": 356,
                "device": "/dev/loop10",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 608,
                "inode_used": 608,
                "mount": "/snap/snapd/23545",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 46661632,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 4129,
                "block_used": 4129,
                "device": "/dev/loop6",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 21133,
                "inode_used": 21133,
                "mount": "/snap/gnome-42-2204/202",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 541196288,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 408,
                "block_used": 408,
                "device": "/dev/loop11",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 608,
                "inode_used": 608,
                "mount": "/snap/snapd/24505",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 53477376,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 1936,
                "block_used": 1936,
                "device": "/dev/loop13",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 812,
                "inode_used": 812,
                "mount": "/snap/firefox/6198",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 253755392,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 2048,
                "block_total": 3097275,
                "block_used": 3097275,
                "device": "/dev/sr0",
                "fstype": "iso9660",
                "inode_available": 0,
                "inode_total": 0,
                "inode_used": 0,
                "mount": "/media/gmartens/Ubuntu 24.04.2 LTS amd64",
                "options": "ro,nosuid,nodev,relatime,nojoliet,check=s,map=n,blocksize=2048,uid=1000,gid=1000,dmode=500,fmode=400,iocharset=utf8",
                "size_available": 0,
                "size_total": 6343219200,
                "uuid": "2025-02-15-09-15-26-00"
            }
        ],
        "ansible_nodename": "stepstone",
        "ansible_os_family": "Debian",
        "ansible_pkg_mgr": "apt",
        "ansible_proc_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-6.11.0-25-generic",
            "quiet": true,
            "ro": true,
            "root": "UUID=c9217b02-0af4-488d-a553-4701b01c390b",
            "splash": true,
            "vt.handoff": "7"
        },
        "ansible_processor": [
            "0",
            "GenuineIntel",
            "QEMU Virtual CPU version 2.5+",
            "1",
            "GenuineIntel",
            "QEMU Virtual CPU version 2.5+",
            "2",
            "GenuineIntel",
            "QEMU Virtual CPU version 2.5+",
            "3",
            "GenuineIntel",
            "QEMU Virtual CPU version 2.5+"
        ],
        "ansible_processor_cores": 2,
        "ansible_processor_count": 2,
        "ansible_processor_nproc": 4,
        "ansible_processor_threads_per_core": 1,
        "ansible_processor_vcpus": 4,
        "ansible_product_name": "Standard PC (i440FX + PIIX, 1996)",
        "ansible_product_serial": "NA",
        "ansible_product_uuid": "NA",
        "ansible_product_version": "pc-i440fx-9.2",
        "ansible_python": {
            "executable": "/usr/bin/python3",
            "has_sslcontext": true,
            "type": "cpython",
            "version": {
                "major": 3,
                "micro": 3,
                "minor": 12,
                "releaselevel": "final",
                "serial": 0
            },
            "version_info": [
                3,
                12,
                3,
                "final",
                0
            ]
        },
        "ansible_python_version": "3.12.3",
        "ansible_real_group_id": 1000,
        "ansible_real_user_id": 1000,
        "ansible_selinux": {
            "status": "disabled"
        },
        "ansible_selinux_python_present": true,
        "ansible_service_mgr": "systemd",
        "ansible_ssh_host_key_ecdsa_public": "AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBP+uqIegZ8VDDVYm/sDWG3CRAAXxwcKofUbdFLP8e/e+CcPUZ1NMVP5jj9Uv+SGOTFFfACWMzBctCWvY6YJf7z4=",
        "ansible_ssh_host_key_ecdsa_public_keytype": "ecdsa-sha2-nistp256",
        "ansible_ssh_host_key_ed25519_public": "AAAAC3NzaC1lZDI1NTE5AAAAIEBjLdEp0gwLChfYpnKLcFzUqArvBBGwFUvyahud3J3J",
        "ansible_ssh_host_key_ed25519_public_keytype": "ssh-ed25519",
        "ansible_ssh_host_key_rsa_public": "AAAAB3NzaC1yc2EAAAADAQABAAABgQDpXBCEAHI4UdaZZTeeTw/9/ugq/uiuAWlbgwI2BbxGuDAsUhy56yGNflbfUV1x+ILMySEOrrjWM96F5lcBKZxYmsfKHp3VBbjHy3M0T9c0rctKAPpOW4VsDjSLCTYTl/ZuaEvbxAukzSJWCq4KtvS8CKmQ0NxvT3k9CEb0l8Qi7XBy1Wy9HGVhzPw/AFaY4My9dJDXiD0QbGGRwqDAs23WYWPPBlGAHjBF6kMFm0P1dCFg6s83dNqoRXI1VO5qiFcaRW0ebPWp4cR30CKfwPSVEpsyo5UdlzdYE3PEm67dhIePVO1gMGAshouluszgCTyDC3Rv5k4tLaa+lgtooVxU1EibTzLXOW/6JKtrp2pSQGvO5RS5VsL0iC3UqgDNQ02kpbdOfU+jnXyRHh0XjafGIqM1/ibCoz4dtkWGljONKyK/X9Y0y1WQUixMaoizsMiBET3cwmOPJC0Bb+6rJ1tvFkMr+FittRqQikVTz2opUw7jf7cdmHCNpgzHlUF6oBc=",
        "ansible_ssh_host_key_rsa_public_keytype": "ssh-rsa",
        "ansible_swapfree_mb": 4095,
        "ansible_swaptotal_mb": 4095,
        "ansible_system": "Linux",
        "ansible_system_capabilities": [
            ""
        ],
        "ansible_system_capabilities_enforced": "True",
        "ansible_system_vendor": "QEMU",
        "ansible_uptime_seconds": 328422,
        "ansible_user_dir": "/home/gmartens",
        "ansible_user_gecos": "gmartens",
        "ansible_user_gid": 1000,
        "ansible_user_id": "gmartens",
        "ansible_user_shell": "/bin/bash",
        "ansible_user_uid": 1000,
        "ansible_userspace_architecture": "x86_64",
        "ansible_userspace_bits": "64",
        "ansible_virtualization_role": "guest",
        "ansible_virtualization_tech_guest": [
            "kvm"
        ],
        "ansible_virtualization_tech_host": [],
        "ansible_virtualization_type": "kvm",
        "gather_subset": [
            "all"
        ],
        "module_setup": true
    },
    "changed": false
}

```