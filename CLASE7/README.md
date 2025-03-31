# Clase 7 - CALCULO DE VLSM Y FLSM

Se configuraron routers fisicos en esta clase

```bash
!Configuracion de VTP
enable
configure terminal
hostname SW_SERVIDOR

vtp mode server
vtp version 2
vtp domain redes1_lab

!Creacion de VLAN
vlan 10
name primaria
vlan 20
name basicos
vlan 30
name diversificado
exit

!El router fisico usa Gigabit Ethernet
interface gi1/0/1
switchport mode access
switchport access vlan 20
exit

interface gi1/0/3
switchport mode access
switchport access vlan 30
exit
```

### Complemento ejemplo en clase 8:

Revisar la grabación:
[Subnetting VLSM y FLSM](https://drive.google.com/file/d/1i8Ha8Mr2DVm1iCo9DPMAJokX3CDmQqvB/view?usp=sharing)