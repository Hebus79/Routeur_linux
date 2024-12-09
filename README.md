# Routeur_linux
Quête WCS routeur sous Linux

## Schéma du réseau sous GNS3

*Toutes les machines comuniquent entre elles.*


![Schema_GNS3](https://github.com/Hebus79/Routeur_linux/blob/main/Images/GNS3-Validation_quete_routeur_IP_Linux-27-11-2024.png)



## Adresse aléatoire IPV6

IPv6 aléatoire : fd47:ce6b:fb82::1/64 obtenue via ce [site](https://www.unique-local-ipv6.com/#).


## Fichier de configuration du routeur0 dans /etc/network/interfaces


_This file describes the network interfaces available on your system
 and how to activate them. For more information, see interfaces(5)._

source /etc/network/interfaces.d/*

The loopback network interface
    auto lo
    iface lo inet loopback

Static config for ens4
	  auto ens4
   configuration IPV4 pour interface réseau ens4
iface ens4 inet static
 	address 10.0.0.1/24
  dns-nameservers 192.168.1.1

Configuration IPV6 pour interface réseau ens4
	iface ens4 inet6 static
	address fd47:ce6b:fb82::1/64

Static config for ens5
   auto ens5
configuration IPV4 pour interface réseau ens5
	iface ens5 inet static
	address 192.168.0.250/24
	#gateway 192.168.0.250

configuration IPV6 pour interface réseau ens5
	iface ens5 inet6 static
	address fd47:ce6b:fb82:192::250/64

Routes to gateways
  up ip route add 10.0.1.0/24 via 192.168.0.251 dev ens5
  up ip route add fd47:ce6b:fb82:1::/64 via fd47:ce6b:fb82:192::251 dev ens5


## Fichier de configuration du routeur1 dans /etc/network/interfaces


This file describes the network interfaces available on your system
and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

The loopback network interface
  auto lo
  iface lo inet loopback

Static config for ens4
  auto ens4
Config IPV4 interface ens4	
  iface ens4 inet static
	address 192.168.0.251/24


configuration IPV6 pour interface réseau ens4
	 iface ens4 inet6 static
 	 address fd47:ce6b:fb82:192::251/64


Config IPV4 interface ens5
	auto ens5
	iface ens5 inet static
	address 10.0.1.1/24

configuration IPV6 pour interface réseau ens5
	 iface ens5 inet6 static
 	 address fd47:ce6b:fb82:1::1/64

Routes to gateways
	up ip route add 10.0.0.0/24 via 192.168.0.250 dev ens4
	up ip route add fd47:ce6b:fb82::/64 via fd47:ce6b:fb82:192::250 dev ens4
	
	
-----------------------------------------------------------------

Tests ping :

1)
depuis console ipterm11, ping vers ipterm12
debian@debian:~$ ping 10.0.1.12
PING 10.0.1.12 (10.0.1.12) 56(84) bytes of data.
64 bytes from 10.0.1.12: icmp_seq=1 ttl=62 time=2.17 ms
64 bytes from 10.0.1.12: icmp_seq=2 ttl=62 time=1.11 ms
64 bytes from 10.0.1.12: icmp_seq=3 ttl=62 time=1.07 ms
64 bytes from 10.0.1.12: icmp_seq=4 ttl=62 time=1.11 ms
^C
--- 10.0.1.12 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 1.068/1.363/2.165/0.463 ms



2)
depuis console ipterm12, ping vers ipterm11

debian@debian:~$ ping 10.0.0.11
PING 10.0.0.11 (10.0.0.11) 56(84) bytes of data.
64 bytes from 10.0.0.11: icmp_seq=1 ttl=62 time=1.08 ms
64 bytes from 10.0.0.11: icmp_seq=2 ttl=62 time=1.07 ms
64 bytes from 10.0.0.11: icmp_seq=3 ttl=62 time=1.07 ms
^C
--- 10.0.0.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 1.066/1.073/1.082/0.006 ms
