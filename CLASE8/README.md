# Clase 8 - Ruteo Estatico

### Enrutamiento Estático

interface s0/0/0: Se configura la interfaz serial con la IP 10.0.0.1/30. Esta será el enlace hacia Router B.

interface fa0/0: Se configura la interfaz FastEthernet con la IP 192.168.100.1/24. Es la red LAN conectada a este router.

```bash
enable
conf t
no ip domain-lookup
hostname routerA

interface s0/0
ip address 10.0.0.1 255.255.255.252
no shut
exit
interface fa0/0
ip address 192.168.100.1 255.255.255.0
no shut

```

interface s0/0: Se configura la interfaz serial con la IP 10.0.0.2/30. Conecta con Router A.

interface fa0/0: Se configura la interfaz FastEthernet con la IP 192.168.200.1/24. Es la red LAN de este router.

ROUTER B

```bash
conf t
no ip domain-lookup
hostname routerB

interface s0/0
ip address 10.0.0.2 255.255.255.252
no shut
exit
interface fa0/0
ip address 192.168.200.1 255.255.255.0
no shut
```

La máscara /30 (255.255.255.252) permite solo 2 direcciones útiles en la red punto a punto entre routers.

### ENRUTAMIENTO ESTATICO

Los routers no conocen automáticamente las redes conectadas a sus interfaces opuestas, por lo que debemos agregar rutas estáticas.


```bash
!Para router A
!Indica que para llegar a la red 192.168.200.0/24 (LAN de Router B), debe enviar el tráfico a 10.0.0.2, que es la interfaz serial de Router B.

ip route 192.168.200.0 255.255.255.0 10.0.0.2

!Para router B
!Indica que para llegar a la red 192.168.100.0/24 (LAN de Router A), debe enviar el tráfico a 10.0.0.1, que es la interfaz serial de Router A.

ip route 192.168.100.0 255.255.255.0 10.0.0.1
```


- Cada router tiene una conexión directa a su LAN y a la otra usando la interfaz serial.
- Se usan rutas estáticas para dirigir el tráfico entre redes.