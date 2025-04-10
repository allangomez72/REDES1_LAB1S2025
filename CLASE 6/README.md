# Clase 6 - Port-Channel

Un Port-Channel (también llamado EtherChannel, término propietario de Cisco) es una tecnología que permite combinar múltiples enlaces físicos de red en un único enlace lógico


### LACP
LACP es un estándar abierto, compatible con dispositivos de diferentes fabricantes. Opera en dos modos: Active (inicia la negociación del canal) y Passive (responde a solicitudes, pero no inicia).

#### Modos de configuración
[LACP](/CLASE%206/assets/LACP.png)

#### Configuración de LACP (Link Aggregation Control Protocol)
```bash 
interface range fa0/1-3
! puede ser active o passive
channel-group 1 mode passive/active
interface port-channel 1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```



### PAgP
PAgP es propietario de Cisco y solo funciona entre dispositivos de esta marca. Sus modos son Desirable (inicia negociación activamente) y Auto (responde, pero no inicia).

#### Modos de configuración
[PAgP](/CLASE%206/assets/PAgP.png)

#### Configuración de PAgP (Port Aggregation Protocol)
```bash 
interface range fa0/4-6
! puede ser auto o desirable
channel-group 2 mode auto/desirable
interface port-channel 2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30

```


### Configuración completa SW1
```bash
enable
conf t

vtp domain MI_RED
vtp mode server
vtp version 2  
vtp password claveVTP 

vlan 10
name Ventas
exit
vlan 20
name IT
exit
vlan 30
name Finanzas
exit

interface range fa0/1-3
channel-group 1 mode active
interface port-channel 1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
end

conf t
interface range fa0/4-6
channel-group 2 mode desirable
interface port-channel 2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30
end

wr
```