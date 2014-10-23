## Introducción

* [Antecedentes](#/3/2)
* [Gestión de claves](#/3/4)
* [Configuración de vpn](#/3/12)
* [Establecimiento de conexión](#/3/16)

note:
* Most networks rely, in part, on physical security to keep them safe.
* With an Ethernet local area network (LAN), for instance, traffic on the LAN itself cannot be intercepted without either physical access to the network wires or a compromise of a computer on the network.
* When a network must span multiple locations using an untrusted intermediary carrier (such as the Internet), security becomes more of a problem.
* In these cases, a virtual private network (VPN) configuration is often employed.



## Antecedentes (I)

<a class="fancybox" href="img/vpn.png" data-fancybox-group="gallery" title="VPN">
<img height="450px" src="img/vpn.redimensionado50.png" alt="VPN">
</a>

[OpenVPN](http://openvpn.net)

note:
* In a VPN, the two private network segments are linked via a router that encrypts data being sent to the remote location.
* Linux provides several VPN options:  OpenVPN



## Antecedentes (II)

* Rango de redes privadas usualmente usados (RFC-1918)
<div class="table-responsive">
<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th>Class</th>
<th>Range</th>
<th>Prefix</th>
</tr>
</thead>
<tbody>
<tr>
<td>A</td>
<td>10.0.0.0–10.255.255.255</td>
<td>10.0.0.0/8</td>
</tr>
<tr>
<td>B</td>
<td>172.16.0.0.0–172.31.255.255</td>
<td>172.16.0.0/12</td>
</tr>
<tr>
<td>C</td>
<td>192.168.0.0–192.168.255.255</td>
<td>192.168.0.0/16</td>
</tr>
</tbody>
  </table>
</div>
* No puede haber direcciones repetidas entre las dos redes a conectar

note:
* The networks linked by the VPN are inherently private
* They should have no or limited access to the Internet as a whole.
* Frequently, VPNs are used to link networks that use the private addresses summarized  which are defined in the Internet standards document RFC-1918.



## Gestión de claves (I)
###Criptografía asimétrica

<a class="fancybox" href="img/privadapublicaclave.png" data-fancybox-group="gallery" title="Esquema cifrado asimétrico">
<img height="450px" src="img/privadapublicaclave.png" alt="Esquema cifrado asimétrico">
</a>

note:
* Like many encryption technologies, OpenVPN relies on keys—large numbers that can be used to encode or decode
data.
* OpenVPN uses public key cryptography, which relies on private keys and public keys for encryption. As their
names imply, private keys should be kept private (secret), whereas public keys can be distributed widely. Data
encrypted by one key can be decrypted by the other. Thus, to communicate in a secure manner, two computers
exchange their public keys. Each computer encrypts data using the other computer’s public key; only the recipient
can then decrypt the data using the matched private key.



## Gestión de claves (II)
### CA, entidad autorizadora

<a class="fancybox" href="img/ca.png" data-fancybox-group="gallery" title="Entidad autorizadora">
<img height="450px" src="img/ca.png" alt="Entidad autorizadora">
</a>

note:
Added to this system of public key cryptography is a certificate authority (CA), which is a computer that issues
keys for other computers to use. The CA embeds identifying information in the keys and digitally signs them,
thereby enabling both sides of the VPN connection to verify that the other side has obtained a key from the same
CA and can therefore be trusted.



## Gestión de claves (III)

* Cuando queremos establecer una conexión vpn, debemos tener una CA
* Primero tenemos que instalar el paquete [openvpn](https://openvpn.net/)
```bash
yum install openssl lzo pam openvpn
```
* OpenVPN proporciona una serie de scripts localizados en  `/usr/share/openvpn/easy-rsa` o  `/usr/share/doc/packages/openvpn`
* Copiamos este directorio en `/etc/openvpn`
```bash
$ cp -R /usr/share/openvpn/easy-rsa/ /etc/openvpn/
```



## Gestión de claves (IV)

* Editamos  el fichero `vars` y asignamos valores a las variables
````bash
$ cd /etc/openvpn/easy-rsa/2.0/
$ vi vars
export KEY_COUNTRY="ES"
export KEY_PROVINCE="HU"
export KEY_CITY="Huelva"
export KEY_ORG="Univeridad de Huelva"
export KEY_EMAIL="ca@uhu.es"
```
* Ejecutamos los comando siguiente para generar los distintos certificados:
```bash
$ ./vars
$ ./clean-all
$ ./build-ca
```



## Gestión de claves (V)

* Responderemos a preguntas como:
```bash
Common Name (eg, your name or your server's hostname)
```
* El siguiente paso consiste en generar claves para el servidor OpenVPN y  los clientes
```
# ./build-key-server server
# ./build-key client1
# ./build-key client2
```
* En cada ejecución me preguntarán:
```bash
Sign the certificate? [y/n]:
```
```
1 out of 1 certificate requests certified, commit? [y/n]
```



## Gestión de claves (VI)

* Una vez generadas las claves, obtenemos "un número primo muy grande" que se usará durante el proceso de cifrado
```bash
# ./build-dh
```



## Gestión de claves (VII)

* Contenidos de  `/etc/openvpn/easy-rsa/2.0/keys`:

<div class="table-responsive">
<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th>Fichero</th>
<th>Copiar a</th>
<th>Propósito</th>
<th width="160em">Secreto</th>
</tr>
</thead>
<tbody>
<tr>
<td>`ca.crt`</td>
<td>Todos</td>
<td>Certificados CA</td>
<td>No</td>
</tr>
<tr>
<td>`ca.key`</td>
<td>Firmante</td>
<td>CA key</td>
<td>Sí</td>
</tr>
<tr>
<td>`dh1024.pem`</td>
<td>Servidor</td>
<td>Parámetros Diffie Hellman</td>
<td>No</td>
</tr>
<tr>
<td>`server.crt`</td>
<td>Servidor</td>
<td>Certificado del servidor</td>
<td>No</td>
</tr>
<tr>
<td>`server.key`</td>
<td>Servidor</td>
<td>Clave servidor</td>
<td>Sí</td>
</tr>
</tbody>
</table>
</div>



## Gestión de claves (VIII)

* Contenidos de  `/etc/openvpn/easy-rsa/2.0/keys`:
<div class="table-responsive">
<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th>Fichero</th>
<th>Copiar a</th>
<th>Propósito</th>
<th>Secreto</th>
</tr>
</thead>
<tbody>
<tr>
<td>`clientn.crt`</td>
<td>Sólo cliente `n`</td>
<td>Certificado cliente `n`</td>
<td>No</td>
</tr>
<tr>
<td>`clientn.key`</td>
<td>Sólo cliente `n`</td>
<td>Clave cliente `n`</td>
<td>Sí</td>
</tr>
</table>
</div>
* Copiamos en el `/etc/openvpn` de cada equipo



## Configuración de vpn

* [Servidor](#/3/13)
* [Clientes](#/3/15)



### Configuración de vpn (II)
#### Servidor (I)

Edito el fichero `/etc/openvpn/openvpn.conf` y pongo
```bash
mode server
local 192.168.1.3 #IP of your OpenVPN server
port 1194
proto tcp
dev tap
ca ca.crt
cert server.crt
key server.key
dh dh1024.pem
server 10.10.10.0 255.255.255.0
ifconfig 10.10.10.1 255.255.255.0
push "dhcp-option DNS 192.168.1.2" #IP of your DNS server
push "dhcp-option WINS 192.168.1.2"IP of you WINS server
push "route 192.168.1.0 255.255.255.0 10.10.10.1"
ifconfig-pool-persist ipp.txt
keepalive 10 120
tls-server
tls-auth ta.key 0
comp-lzo
user nobody
group users
persist-key
persist-tun
status openvpn-status.log
verb 1
client-to-client
```



## Configuración de vpn (III)
### Servidor (II)

Es necesario ajustar forwarding y firewall

```bash
#!/bin/bash
# Basic Firewall script for OpenVPN

IPT="/sbin/iptables" #Path to iptables binary
LAN="eth0" #Your LAN interface
VNET="10.10.10.0/24" #VPN network
VPNIF="tap+" #VPN tap interfaces

echo 1 > /proc/sys/net/ipv4/ip_forward

#Flush existing rules
$IPT -P INPUT ACCEPT
$IPT -F INPUT
$IPT -F OUTPUT
$IPT -F FORWARD
$IPT -t nat -F

$IPT -A INPUT -i lo -j ACCEPT
$IPT -A OUTPUT -o lo -j ACCEPT
$IPT -A INPUT -i $VPNIF -j ACCEPT
$IPT -A FORWARD -i $VPNIF -j ACCEPT
$IPT -t nat -A POSTROUTING -s $VNET -o $LAN -j MASQUERADE
```



## Configuración de vpn (VI)
###  Cliente

Edito el fichero `/etc/openvpn/server.conf` y pongo
```bash
client
proto tcp
dev tap
port 1194
remote REMOTEHOST FQDN or IP
tls-client
ca ca.crt
cert vpnclient1.crt
key vpnclient1.key
tls-auth ta.key 1
comp-lzo
pull
verb 1
```



## Establecimiento de conexión

* Ejecutamos el fichero de configuración del cliente y del servidor
```bash
/etc/openvpn/openvpn.conf
```
```bash
/etc/openvpn/server.conf
```

note:
* With the server and clients configured, you can now test your VPN. To launch the VPN, type openvpnconfig-file,
where config-file is the complete path to your VPN configuration file. This path will, of course, point to the server configuration for the server and the client configuration for each of your clients.
* You can check the output provided by the program for any error messages. The output should conclude with the message Initialization Sequence Completed. If all seems in order, you can test basic connectivity from the client to the server:
$ ping 10.8.0.1
The 10.8.0.1 address is given to the server in the default configuration files; change it if you altered this detail
yourself. If this works, you can begin expanding the configuration to enable other computers on the client and/or
server sides to communicate using the VPN. Suppose the server is on the 10.66.0.0/24 network. On the server
configuration, add the following line to the configuration file to enable server-side systems to use the VPN:
push "route 10.66.0.0 255.255.255.0"
Change the addresses to match your network, of course. This enables links between machines on the local
network and the remote (client) network. You must also adjust the server-side network’s router to include an
appropriate route, for the server’s VPN subnet (10.8.0.0/24 by default). This change isn’t necessary if the VPN
router is also the network’s conventional router. Note that you do not need to route traffic to the client’s local
subnet; as far as other systems on the server’s network are concerned, an isolated VPN client will have an address
in the 10.8.0.0/24 subnet.

you must make more changes. On the server, you must create a directory called /etc/openvpn/ccd and set the
following directives in the server’s configuration file:
client-config-dir ccd
route 192.168.4.0 255.255.255.0
Adjust the IP address and network mask on the route line for your client’s network. In the /etc/openvpn/ccd
directory, create a configuration file with the common name given to the OpenVPN client. This file should
duplicate the route line in the main configuration file but using the keyword iroute:
iroute 192.168.4.0 255.255.255.0
If you want to enable the OpenVPN server to direct traffic between VPN client networks, add the following lines
to the server’s configuration file (again, changing the addresses as appropriate for your network):
client-to-client
push "route 192.168.4.0 255.255.255.0"
Finally, you must make changes to the default router configuration for both the server and client networks to
direct traffic for the remote networks through their respective VPN systems.
With these changes in place, restart the client and server and test your connections using ping or other tools. If
you have problems, consult the OpenVPN Web site, which contains extended instructions and troubleshooting
information.
Many distributions ship with startup scripts that scan the /etc/openvpn directory for client and server
configuration files and launch openvpn in the appropriate mode. If your system contains such files, you can shut
down the program and restart it using these tools and configure the computer to start OpenVPN automatically
when it boots.
