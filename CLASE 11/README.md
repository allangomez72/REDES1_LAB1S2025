# Clase 11 - Ruteo InterVlan

### Configuración inicial `SWITCH`
```bash
enable
conf t
no ip domain-lookup
hostname SW1

vlan 14
name ADMIN
vlan 24
name ESTUDIANTES

interface range gi0/1-2
switchport mode trunk
switchport trunk allowed vlan 14,24
exit

interface fa0/1
switchport mode access
switchport access vlan 24
exit

interface fa0/2
switchport mode access
switchport access vlan 14
exit
```

## Protocolo HSRP

### Router(R1) Activo
```bash
enable
conf t
no ip domain-lookup
!192.168.4.192/28
interface gi0/0.14
encapsulation dot1q 14
!Verificar siempre la mascara de subred
ip add 192.168.4.194 255.255.255.240

standby 1 ip 192.168.4.193
standby priority 150
standby 1 priority 150
standby 1 preempt
exit

!192.168.4.128/26
interface gi0/0.24
encapsulation dot1q 24
ip add 192.168.4.130 255.255.255.192

standby 2 ip 192.168.4.129
standby priority 150
standby 2 priority 150
standby 2 preempt 
exit

interface gi0/0
no shut
exit

!Darle IP para el ruteo G0/1 10.0.4.20 /30
interface fa0/0/0
ip add 10.0.4.22 255.255.255.252
no shut
end
wr
```

### Router(R2) Standby
```bash
enable
conf t
no ip domain-lookup

!192.168.4.192/28
interface gi0/0.14
encapsulation dot1q 14
!Verificar siempre la mascara de subred
ip add 192.168.4.195 255.255.255.240
standby 1 ip 192.168.4.193
exit

!192.168.4.128/26
interface gi0/0.24 
encapsulation dot1q 24
ip add 192.168.4.131 255.255.255.192
standby 2 ip 192.168.4.129
exit

interface gi0/0
no shut
exit

!Darle IP para el ruteo G0/2 10.0.4.24 /30
interface gi0/1
ip add 10.0.4.26 255.255.255.252
no shut

end
wr
```

## Ruteo OSPF
```bash
!Corresponde al router R1
enable
conf t
router ospf 1
net 10.0.4.20 0.0.0.3 area 0
net 192.168.4.128 0.0.0.63 area 0
net 192.168.4.192 0.0.0.15 area 0
exit

!Corresponde al rotuer R2
enable
conf t
router ospf 1
net 10.0.4.24 0.0.0.3 area 0
net 192.168.4.128 0.0.0.63 area 0
net 192.168.4.192 0.0.0.15 area 0
exit

!Corresponde al router Central
enable
conf t
router ospf 1
net 10.0.4.20 0.0.0.3 area 0
net 10.0.4.24 0.0.0.3 area 0
exit
```

### Comandos para verificar
```bash
!HSRP
show standby brief
!Verificar la tabla de ruteo
show ip route
!Verificar configuración general
show run
```