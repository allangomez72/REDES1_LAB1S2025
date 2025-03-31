# Clase 8 - Ruteo InterVlan

El siguiente fragmento de configuración pertenece a un método conocido como Router on a Stick, utilizado para permitir la comunicación entre múltiples VLANs a través de un solo enlace físico en un router. Este enfoque es común en redes donde se necesita interconectar VLANs sin el uso de un switch de nivel 3.

La línea `interface fa0/0.10` indica que se está creando una subinterfaz en el puerto FastEthernet 0/0. El número .10 al final representa la VLAN 10, lo que significa que esta subinterfaz manejará el tráfico de dicha VLAN. En redes con VLANs, un router no puede enviar tráfico directamente entre ellas a menos que tenga configuradas subinterfaces para cada una.

El comando `encapsulation dot1q 10` establece que esta subinterfaz usará el estándar IEEE 802.1Q para la encapsulación de tramas VLAN y especifica que pertenece a la VLAN 10. Este estándar permite que un solo enlace troncal transporte tráfico de múltiples VLANs, etiquetando los paquetes con el identificador de VLAN correspondiente.

Por último, ip address `<ip_vlan_id_red>` asigna una dirección IP a la subinterfaz. Esta dirección debe pertenecer a la red de la VLAN configurada y actuará como la puerta de enlace predeterminada para los dispositivos dentro de esa VLAN. Es decir, cualquier dispositivo en la VLAN 10 utilizará esta dirección para comunicarse con otras VLANs.


```bash
enable
conf t
hostname R1

!.10 es el numero de vlan
interface fa0/0.10
encapsulation dot1q 10
ip address <ip_vlan_id_red>
```

Para los comando del switch son los que generalmente se hacen, en un switch con el servidor vtp, solo se crea las vlans, ejemplo de uno:

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
exit

!El router fisico usa Gigabit Ethernet
interface gi1/0/1
switchport mode access
switchport access vlan 10
exit

interface gi1/0/3
switchport mode access
switchport access vlan 20
exit
```