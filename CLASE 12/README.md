# Clase 11 - Configuración Firewall y Access Point

```bash
conf t

interface gi1/2
 nameif OUTSIDE
 security-level 0
 ip address 192.168.5.1 255.255.255.0
 no shutdown

interface gi1/1
 nameif INSIDE
 security-level 100
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface gi1/3
 nameif NOVO
 security-level 100
 ip address 192.168.40.1 255.255.255.0
 no shutdown
 
router ospf 1
network 192.168.5.0 255.255.255.0 area 0
network 192.168.20.0 255.255.255.0 area 0
```

ACL

```bash
access-list INSIDE_TO_OUTSIDE extended permit ip any any
access-group INSIDE_TO_OUTSIDE in interface INSIDE

```

```bash
access-list INSIDE_TO_OUTSIDE extended permit ip host 192.168.20.4 host 192.168.5.4
access-group INSIDE_TO_OUTSIDE in interface INSIDE
```