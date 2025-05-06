# Mikrotik RouterOS Ansible tests

My routeros ansible tests / experiments.

Uses the following software:
- https://github.com/ansible-collections/community.routeros
- https://docs.ansible.com/ansible/latest/collections/community/routeros/index.html

## Network Layout


## Scenarios

### Run command

[Run a single command and print output](ros-cmd-output.yml)


### Set - Flat

Flat settings are those not grouped in any way whatsoever, an example of which is IP or routerboard settings.

```
[admin@RTR_R2] > /ip/settings/print
                  ip-forward: yes
              send-redirects: yes
         accept-source-route: no
            accept-redirects: no
            secure-redirects: yes
                   rp-filter: no
  ipv4-multipath-hash-policy: l3
              tcp-syncookies: no
        max-neighbor-entries: 16384
                 arp-timeout: 30s
             icmp-rate-limit: 10
              icmp-rate-mask: 0x1818
             allow-fast-path: yes
       ipv4-fast-path-active: yes
      ipv4-fast-path-packets: 0
        ipv4-fast-path-bytes: 0
       ipv4-fasttrack-active: no
      ipv4-fasttrack-packets: 0
        ipv4-fasttrack-bytes: 0
[admin@RTR_R2] > /system/routerboard/settings/print
              auto-upgrade: yes
               boot-device: nand-if-fail-then-ethernet
         preboot-etherboot: disabled
  preboot-etherboot-server: any
             cpu-frequency: auto
             boot-protocol: bootp
       force-backup-booter: no
               silent-boot: no
      protected-routerboot: disabled
      reformat-hold-button: 20s
  reformat-hold-button-max: 10m
```

### Set - Unique items

These are scenarios where the settings are grouped by some sort of unique identifier, an example is IP -> Service (the unique identifier being the service name) or Interfaces. 

```
[admin@RTR_bedroom] > /ip/service/print
Flags: X - DISABLED, I - INVALID
Columns: NAME, PORT, CERTIFICATE, VRF, MAX-SESSIONS
#   NAME     PORT  CERTIFICATE  VRF   MAX-SESSIONS
0   telnet     23               main            20
1   ftp        21                               20
2   www        80               main            20
3   ssh        22               main            20
4 X www-ssl   443  none         main            20
5   api      8728               main            20
6   winbox   8291               main            20
7   api-ssl  8729  none         main            20
```

### Set - Common Group

These are scenarios where there is a group of (usually a list) of settings under a common name, ordering does not matter. An example is firewall address-list.

```
[admin@RTR_R2] > /ip/firewall/address-list/print
Columns: LIST, ADDRESS, CREATION-TIME
# LIST     ADDRESS         CREATION-TIME      
;;; RFC1918
3 RFC1918  10.0.0.0/8      2024-11-07 18:43:30
;;; RFC1918
4 RFC1918  172.16.0.0/12   2024-11-07 18:43:30
;;; RFC1918
5 RFC1918  192.168.0.0/16  2024-11-07 18:43:30
```

### Set - Ordered group

These are scenarios where there are ordered groups of configuration. An example would be firewall rules. Ordering is important within each group (as well as possibly between groups).
