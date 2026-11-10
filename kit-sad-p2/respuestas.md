># Respuestas · Práctica 2 · Bodegas Atlántico

> Rellena este fichero en tu ordenador. Va en la entrega (`./lab.sh entregar`) y en la defensa te pueden preguntar por él.

## Bloque A · Piensa como un atacante (nmap)

Desde la calle: `./lab.sh cliente` y dentro `nmap -Pn -p 22,80,443,5432,8080 198.51.100.254`. Pega la salida:

```text
root@cliente:/# nmap -Pn -p 22,80,443,5432,8080 198.51.100.254
Starting Nmap 7.95 ( https://nmap.org ) at 2026-11-05 11:06 UTC
Nmap scan report for sad-p2-fw.sad-p2_calle (198.51.100.254)
Host is up (0.000042s latency).

PORT     STATE    SERVICE
22/tcp   filtered ssh
80/tcp   open     http
443/tcp  open     https
5432/tcp filtered postgresql
8080/tcp filtered http-proxy
MAC Address: 22:CC:CA:C6:DB:04 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.27 seconds
```

## Bloque A · El contador

Después de `./lab.sh comprobar`, mira `./lab.sh ver`. ¿Cuántos paquetes lleva el contador de A6? ¿Quién los ha mandado?

> iifname "dmz" oifname "lan" counter packets 12 bytes 720 drop # handle 17
>Lleva un total de 12 paquetes, los cuales provienen de la DMZ, los ha podido enviar un cliente, un ciberdelincuente
>o algún empleado despistado que intenta entrar en las oficinas a través de la tienda.


## Bloque B · La prueba del cifrado

En el firewall (`./lab.sh fw`): `tcpdump -ni wan host 198.51.100.62 -c 6`. Mientras, en otra terminal, el comercial
(`./lab.sh comercial`) hace `curl http://172.16.10.20:8080`. Pega lo que sale en el firewall:

```text
root@fw:/# tcpdump -ni wan host 198.51.100.62 -c 6
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on wan, link-type EN10MB (Ethernet), snapshot length 262144 bytes
09:49:16.555558 IP 198.51.100.62.53180 > 198.51.100.254.51820: UDP, length 96
09:49:16.555676 IP 198.51.100.62.53180 > 198.51.100.254.51820: UDP, length 148
09:49:16.555681 IP 198.51.100.254.51820 > 198.51.100.62.53180: UDP, length 96
09:49:16.555874 IP 198.51.100.254.51820 > 198.51.100.62.53180: UDP, length 92
09:49:16.555998 IP 198.51.100.62.53180 > 198.51.100.254.51820: UDP, length 96
09:49:16.556007 IP 198.51.100.62.53180 > 198.51.100.254.51820: UDP, length 176
6 packets captured
13 packets received by filter
0 packets dropped by kernel
```

En dos líneas: ¿qué se ve por la calle y qué NO se ve?
>Por la calle se ve que los paquetes utilizan UDP para comunicarse y su tamaño en bytes, 
>y no se ve su contenido, ni la IP interna del comercial ya que viajan de manera cifrada por la VPN.

## Bloque C · Para la defensa (C4)

Del **mismo ataque** (la inyección a la web): ¿qué ve el vigilante de la red (el IDS) y qué ve el portero de la web? ¿Por
qué hacen falta los dos?

>

## Bloque D · La nota del servidor (D0 y D4)

Nota del análisis **antes** de bastionar (D0):

>

Nota **después** (D4). ¿Por qué no llega a 100? ¿Significa que el servidor sigue inseguro? Comenta un aviso que siga saliendo.

>
