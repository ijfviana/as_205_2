## Introducción

* [nc](#/4/2)
* [netstat](#/4/8)
* [tcpdump](#/4/17)
* [wireshark](#/4/20)
* [nmap](#/4/23)

note:
Several network monitoring tools exist that can help you verify proper network functioning, check what programs
are using the network, and monitor network traffic.



## nc (I)

* [`nc`](http://www.manpagez.com/man/1/nc/) es una herramienta de red de propósito general que sirve para monitorizar tráfico tcp y udp
* Su sintaxis es:
```bash
nc [options] [hostname] [ports]
```

<aside class="notes">
Using nc
The nc utility (also known as netcat) is a very powerful general-purpose networking tool. It can send arbitrary data,
monitor network ports, and more. Its basic syntax is:
nc [options] [hostname] [ports]
</aside>



## nc (II)
### Ejemplos

* Comprobar conexión entre dos puntos y puerto
```bash
ijfviana-dev@vagalume-dev:~$  nc -l 1234 #escucha
```
```bash
ijfviana-dev@vagalume-dev:~$ nc locahost 1234 # envía
```
* Interacción con servicios conocidos
```bash
ijfviana-dev@vagalume-dev:~$ echo -n "GET / HTTP/1.0\r\n\r\n" | nc www.uhu.es 80
```
* Abrir conexión entre puertos distintos
```bash
ijfviana-dev@vagalume-dev:~$ nc -p 3133 -w 5 hostname,.com 42
```



## nc (III)
### Opciones (I)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="250pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-d`</td>
<td>No lee de la entrada estándar</td>
</tr>
<tr>
<td>`-h`</td>
<td>Muestra información de ayuda</td>
</tr>
<tr>
<td>`-i interval`</td>
<td>Introduce un retraso entre envío y recepción de texto</td>
</tr>
<tr>
<td>`-k`</td>
<td>Se mantiene a la escucha de una luego conexión una vez acabada la actual. Se usa con la opción `-l`</td>
</tr>

</tbody>
</table>
</div>

note:
Option Purpose
-d Does not read from standard input.
-h Displays help information.
-i interval Inserts a delay between text sent and received.
-k Keeps listening for a new connection after the current one terminates. Must be used in conjunction with -l .
-l Listens for an incoming connection rather than establish an outgoing one.
-n Does not perform DNS lookups.
-p port Makes a connection using the specified outgoing port. May not be used with -l .
-r Selects port numbers randomly.
-s ip-address Specifies the IP address of an outgoing connection. May not be used with -l .
-U Uses Unix domain sockets.
-u Uses UDP rather than TCP.
-v Produces verbose output.
-w timeout Terminates a connection if it has been inactive for timeout seconds.
-X version Uses the specified proxy version; valid values are 4 (SOCKS v.4), 5 (SOCKS v.5; the default), and connect (HTTPS proxy).
Used in conjunction with -x .
-
x address [: port ] Makes a connection via the specified proxy server.
-z Scans for a listening server without sending data to it.



## nc (IV)
### Opciones (II)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="250pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>

<td>`-l`</td>
<td>Listens for an incoming connection rather than establish an outgoing one.</td>
</tr>
<tr>
<td>`-n`</td>
<td>Does not perform DNS lookups.</td>
</tr>
<tr>
<td>`-p port`</td>
<td>Makes a connection using the specified outgoing port. May not be used with -l .</td>
</tr>
</tbody>
</table>
</div>

note:
TABLE 5.6 Common nc options
Option Purpose
-d Does not read from standard input.
-h Displays help information.
-i interval Inserts a delay between text sent and received.
-k Keeps listening for a new connection after the current one terminates. Must be used in conjunction with -l .
-l Listens for an incoming connection rather than establish an outgoing one.
-n Does not perform DNS lookups.
-p port Makes a connection using the specified outgoing port. May not be used with -l .
-r Selects port numbers randomly.
-s ip-address Specifies the IP address of an outgoing connection. May not be used with -l .
-U Uses Unix domain sockets.
-u Uses UDP rather than TCP.
-v Produces verbose output.
-w timeout Terminates a connection if it has been inactive for timeout seconds.
-X version Uses the specified proxy version; valid values are 4 (SOCKS v.4), 5 (SOCKS v.5; the default), and connect (HTTPS proxy).
Used in conjunction with -x .
-
x address [: port ] Makes a connection via the specified proxy server.
-z Scans for a listening server without sending data to it.



## nc (V)
### Opciones (III)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="250pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-r`</td>
<td>Selecciona un puerto aleatoriamente</td>
</tr>
<tr>
<td>`-s ip-add`</td>
<td>Especifica IP a la que conectarse</td>
</tr>
<tr>
<td>`-U Uses`</td>
<td>Unix domain sockets.</td>
</tr>
<tr>
<td>`-u`</td>
<td>Usa UDP en vez de TCP.</td>
</tr>
<tr>
<td>`-v`</td>
<td>Salida detallada.</td>
</tr>
<tr>
<td>`-w timeout`</td>
<td>Finaliza conexión después de periodo de inactividad</td>
</tr>
</tbody>
</table>
<div>

note:
TABLE 5.6 Common nc options
Option Purpose
-d Does not read from standard input.
-h Displays help information.
-i interval Inserts a delay between text sent and received.
-k Keeps listening for a new connection after the current one terminates. Must be used in conjunction with -l .
-l Listens for an incoming connection rather than establish an outgoing one.
-n Does not perform DNS lookups.
-p port Makes a connection using the specified outgoing port. May not be used with -l .
-r Selects port numbers randomly.
-s ip-address Specifies the IP address of an outgoing connection. May not be used with -l .
-U Uses Unix domain sockets.
-u Uses UDP rather than TCP.
-v Produces verbose output.
-w timeout Terminates a connection if it has been inactive for timeout seconds.
-X version Uses the specified proxy version; valid values are 4 (SOCKS v.4), 5 (SOCKS v.5; the default), and connect (HTTPS proxy). Used in conjunction with -x .
- x address [: port ] Makes a connection via the specified proxy server.
-z Scans for a listening server without sending data to it.



## nc (VI)
### Opciones (IV)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="250pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-X version`</td>
<td>Especifica versión del proxy; 4, (SOCKS v.4), 5 (SOCKS v.5, and connect (HTTPS proxy).</td>
</tr>
<tr>
<td>`-x address [: port ]`</td>
<td>Realiza la conexión usando proxy</td>
</tr>
<tr>
<td>`-z`</td>
<td>Comprueba servidor sin mandarle datos</td>
</tr>
</tbody>
</table>
<div>

note:
TABLE 5.6 Common nc options
Option Purpose
-d Does not read from standard input.
-h Displays help information.
-i interval Inserts a delay between text sent and received.
-k Keeps listening for a new connection after the current one terminates. Must be used in conjunction with -l .
-l Listens for an incoming connection rather than establish an outgoing one.
-n Does not perform DNS lookups.
-p port Makes a connection using the specified outgoing port. May not be used with -l .
-r Selects port numbers randomly.
-s ip-address Specifies the IP address of an outgoing connection. May not be used with -l .
-U Uses Unix domain sockets.
-u Uses UDP rather than TCP.
-v Produces verbose output.
-w timeout Terminates a connection if it has been inactive for timeout seconds.
-X version Uses the specified proxy version; valid values are 4 (SOCKS v.4), 5 (SOCKS v.5; the default), and connect (HTTPS proxy). Used in conjunction with -x .
- x address [: port ] Makes a connection via the specified proxy server.
-z Scans for a listening server without sending data to it.



## Netstat

* La herramienta [`Netstat`](http://www.manpagez.com/man/1/netstat/) es de diagnóstico.
* Se considera la navaja suiza de las aplicaciones de red.
* Permite obtener información que no sería posible obtener, de forma sencilla, por otros medios.
* Su sintaxis es:
```bash
 netstat [-AaLlnW] [-f address_family | -p protocol]
     netstat [-gilns] [-v] [-f address_family] [-I interface]
     netstat -i | -I interface [-w wait] [-c queue] [-abdgqRt]
     netstat -s [-s] [-f address_family | -p protocol] [-w wait]
     netstat -i | -I interface -s [-f address_family | -p protocol]
     netstat -m [-m]
     netstat -r [-Aaln] [-f address_family]
     netstat -rs [-s]
```

note:
* Another useful diagnostic tool is netstat.
* This is something of a Swiss Army knife of network tools because it can be used in place of several others, depending on the parameters it’s passed.
* It can also return information that’s not easily obtained in other ways.



## Netstat (II)
### Ejemplos (I)

* Obtener estadísticas de uso de los interfaces de red (`ifconfig`):
```bash
ijfviana-dev@vagalume-dev:~$  netstat -i
Iface       MTU Met    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500 0    186525       0    331 0        122083      0      0      0 BMU
lo        65536 0     47659       0      0 0         47659      0      0      0 LRU
wlan0      1500 0    823255       0     13 0        297940      0      0      0 BMRU
```



## Netstat (III)
### Ejemplos (II)

* Obtener información tablas de enrutamiento
```bash
ijfviana-dev@vagalume-dev:~$ netstat -r
Tabla de rutas IP del núcleo
Destino         Pasarela        Genmask         Indic   MSS Ventana irtt Interfaz
default         172.16.207.254  0.0.0.0         UG        0 0          0 wlan0
172.16.200.0    *               255.255.248.0   U         0 0          0 wlan0
```



## Netstat (IV)
### Ejemplos (III)

* Obtener información sobre los programas que tienen conexiones abiertas
```bash
ijfviana-dev@vagalume-dev:~$ netstat -r
Conexiones activas de Internet (servidores w/o)
Proto  Recib Enviad Dirección local         Dirección remota       Estado       PID/Program name
tcp        0      0 vagalume-dev.loca:38197 snt-re3-6d.sjc.dro:http ESTABLECIDO 4379/dropbox
tcp        1      0 vagalume-dev.loca:37135 mulberry.canonical:http CLOSE_WAIT  4425/ubuntu-geoip-p
Activar zócalos de dominio UNIX (servidores w/o)
Proto RefCnt Flags       Type       State         I-Node   PID/Program name    Ruta
unix  2      [ ]         DGRAM                    3362576  8994/wpa_supplicant /var/run/wpa_supplicant/wlan0
unix  21     [ ]         DGRAM                    11577    698/rsyslogd        /dev/log
unix  3      [ ]         FLUJO      CONECTADO     12921    3086/dbus-daemon    @/tmp/dbus-nUPp4dXpYx
unix  3      [ ]         FLUJO      CONECTADO     15528    763/dbus-daemon     /var/run/dbus/system_bus_socket
```



## Netstat (V)
### Opciones

<div class="table-responsive">
<table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="140pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-a`</td>
<td>Show all info host, network, and default routes are shown.</td>
</tr>
<tr>
<td>`-n`</td>
<td>Show network addresses as numbers. netstat normally displays addresses as symbols. This option may be used with any of the display formats.</td>
</tr>
<tr>
<td>`-v`</td>
<td>Verbose. Show additional information for the sockets and the routing table.</td>
</tr>
</tbody>
</table>
</div>



## Netstat (VI)
### Opciones (II)

<div class="table-responsive">
<table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="140pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-g`</td>
<td>Show the multicast group memberships for all interfaces.</td>
</tr>
<tr>
<td>`-m`</td>
<td>Show the STREAMS statistics.</td>
</tr>
<tr>
<td>`-p`</td>
<td>Show the address resolution (ARP) tables.</td>
</tr>
<tr>
<td>`-s`</td>
<td>Show per-protocol statistics. When used with the -M option, show multicast routing statistics instead.</td>
</tr>

</tbody>
</table>
</div>



## Netstat (VII)
### Opciones (III)

<div class="table-responsive">
<table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="140pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-i`</td>
<td>Show the state of the interfaces that are used for TCP/IP traffic.</td>
</tr>
<tr>
<td>`-r`</td>
<td>Show the routing tables.</td>
</tr>
<tr>
<td>`-M`</td>
<td>Show the multicast routing tables. When used with the -s option, show multicast routing statistics instead.</td>
</tr>
<tr>
<td>`-d`</td>
<td>Show the state of all interfaces that are under Dynamic Host Configuration Protocol (DHCP) control.</td>
</tr>
</tbody>
</table>
</div>



## Netstat (VIII)
### Opciones (IV)

<div class="table-responsive">
<table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th>Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>-D</td>
<td>Show the status of DHCP configured interfaces.</td>
</tr>
<tr>
<td>-f address_family</td>
<td>Limit statistics or address control block reports to those of the specified address_family (AF_INET, AF_Unix),</td>
</tr>
<tr>
<td>-P protocol</td>
<td>Limit display of statistics or state of all sockets to those applicable to protocol.</td>
</tr>
</tbody>
</table>
</div>



## Netstat (IX)
### Opciones (V)

<div class="table-responsive">
<table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th width="320pt">Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`- I interface`</td>
<td>Show the state of a particular interface. interface can be any valid interface such as ie0 or le0.</td>
</tr>
</tbody>
</table>
</div>



## tcpdump

* El [tcpdump](http://www.manpagez.com/man/1/tcpdump/) es un [sniffer](http://es.wikipedia.org/wiki/Analizador_de_paquetes) de paquetes
* Los paquetes obtenidos pueden ser analizados por utilidades de diagnóstico para verificar la información que transitar por la red
```bash
tcpdump [ -AbdDefhHIJKlLnNOpqRStuUvxX ] [ -B buffer_size ] [ -c count ]
               [ -C file_size ] [ -G rotate_seconds ] [ -F file ]
               [ -i interface ] [ -j tstamp_type ] [ -m module ] [ -M secret ]
               [ -P in|out|inout ]
               [ -r file ] [ -V file ] [ -s snaplen ] [ -T type ] [ -w file ]
               [ -W filecount ]
               [ -E spi@ipaddr algo:secret,...  ]
               [ -y datalinktype ] [ -z postrotate-command ] [ -Z user ]
               [ expression ]
```

note:
* The tcpdump utility is a packet sniffer, which is a program that can intercept network packets and log them or
display them on the screen.
* Packet sniffers can be useful diagnostic tools because they enable you to verify that a computer is actually receiving data from other computers. * * They also enable you to examine the data in its raw form, which can be useful if you understand enough of the protocol’s implementation details to spot problems.
* The first thing to note about this command is that you must run it as root; ordinary users aren’t allowed to
monitor network traffic in this way.



## tcpdump (II)
### Ejemplos

* Examinar tráfico de la red (-cnum para indicar paquetes máximos)
``` bash
ijfviana-dev@vagalume-dev:~$ tcpdump
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
19:31:55.503759 IP speaker.example.com.631 > 192.168.1.255.631: UDP, length: 139
19:31:55.505400 IP nessus.example.com.33513 > speaker.example.com.domain:
46276+ PTR? 255.1.168.192.in-addr.arpa. (44)
19:31:55.506086 IP speaker.example.com.domain > nessus.example.com.33513:
46276 NXDomain* 0/1/0 (110)
```

note:
* Once it’s run, tcpdump summarizes what it’s doing and then begins printing lines, one for each packet it monitors. (Some of these lines can be quite long and so may take more than one line on your display.) These lines include a time stamp, a stack identifier (IP in all of these examples), the origin system name or IP address and port, the destination system name or IP address and port, and packet-specific information.
* Ordinarily, tcpdump keeps displaying packets indefinitely, so you must terminate it by pressing Ctrl+C.
* Even this basic output can be very helpful. For instance, consider the preceding example of three packets, which was captured on nessus.example.com. This computer successfully received one broadcast packet (addressed to 192.168.1.255) from speaker.example.com’s UDP port 631, sent a packet to speaker.example.com, and received a packet from that system directed at nessus.example.com rather than sent as a broadcast. This sequence verifies that at least minimal communication exists between these two computers. If you were having problems establishing a connection, you could rule out a whole range of possibilities based on this evidence, such as faulty cables or a firewall that was blocking traffic.



## tcpdump (III)
### Opciones

<div class="table-responsive">
<table class="table table-condensed  table-hover table-bordered">
<thead>
<tr>
<th>Opción</th>
<th>Decripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-A`</td>
<td>Muestra contenido de paquetes</td>
</tr>
<tr>
<td>`-D`</td>
<td>Listado de interfaces</td>
</tr>
<tr>
<td>`-v`  `-vv`</td>
<td>Información adicional de paquetes</td>
</tr>
<tr>
<td>`-wfile`</td>
<td>Escribe paquetes capturados a un fichero</td>
</tr>
</tbody>
</table>
</div>

note:
If you need more information, tcpdump provides several options that enhance or modify its output. These
include -A to display packet contents in ASCII, -D to display a list of interfaces to which tcpdump can listen, -n to
display all addresses numerically, -v (and additional -v options, up to -vvv) to display additional packet information,
and -wfile to write the captured packets to the specified file. Consult tcpdump’s man page for more details on
these options and for additional options.



## [Wireshark]((http://www.wireshark.org)

* Antes denominado Ethereal
* Permite hacer un análisis completo de paquetes, profundizando en sus contenidos
* Podemos usar la linea de comando o un interfaz gráfico

note:
* Formerly known as Ethereal, is another packet sniffer.
* It features deep analysis of network packets, meaning that the program can help you dig into network packets to discover the meanings they carry.



## [Wireshark]((http://www.wireshark.org)
### Ejemplo

```bash
# tshark
Running as user "root" and group "root". This could be dangerous.
Capturing on eth1
0.000000 08:10:74:24:1b:d4 -> Broadcast ARP Who has 192.168.1.1? Tell
192.168.1.254
0.308644 192.168.1.8 -> 192.168.1.2 SSH Encrypted response packet len=128
0.308721 192.168.1.8 -> 192.168.1.2 SSH Encrypted response packet len=48
```

<aside class="notes">
This example (which is truncated; real output will continue indefinitely, or until you press Ctrl+C) shows an
Address Resolution Protocol (ARP) broadcast followed by two SSH packets. If you want to restrict Wireshark’s
output, you can do so with a wide array of tshark options.
</aside>



## [Wireshark]((http://www.wireshark.org)
### Ejemplo (II)

<a class="fancybox" href="img/wireshark.png" data-fancybox-group="gallery" title="Pasos para trabajar con volúmenes lógicos">
<img height="450px" src="img/wireshark.redimensionado75.png" alt="Pasos para trabajar con volúmenes lógicos">
</a>

<aside class="notes">
You’ll probably find it easier to use Wireshark with its GUI, which is often installed via a separate package from
the main Wireshark package. On Fedora, for instance, you must install wireshark-gnome. You can then type
wireshark to launch the main Wireshark GUI program. Selecting Capture ⇒ Start then begins a capture of all traffic.
(You can limit what’s captured by using the Capture ⇒ Capture Filters... option.) Selecting Capture ⇒ Stop
terminates the capture operation. The result resembles Figure 5.4. This window has three panes. The top pane
presents a summary of each network packet, the middle pane presents protocol-specific analysis of the packet
selected in the top pane, and the bottom pane shows the raw packet data.
</aside>



## Nmap (I)

* [Nmap](http://nmap.org/man/es/) es un escaner de puertos
* No monitoriza, me devuelve información sobre los puestos que tiene abierto un ordenador o conjunto de ellos
* El escaneo de puertos puede ser detectado y penado.
* Nmap soporta escaneo  “stealth”, escaneo mediante pings, etc.

```bash
nmap [Scan Type...] [Options] {target specification}
```

<aside class="notes">
* Network scanners, such as Nmap (http://www.insecure.org/nmap/), can scan for open ports on the local computer or on other computers.
* Network scanners are used by crackers for locating likely target systems, as well as by network administrators for legitimate purposes.
* Many organizations have policies forbidding the use of network scanners except under specific conditions.
* Therefore, you should check these policies and obtain explicit permission, signed and in writing, to perform a network scan.
</aside>



## Nmap (II)
### Ejemplos (I)

* Escaneo básico de puertos

```bash
$ nmap -sT hindmost
Starting Nmap 5.21 ( http://nmap.org ) at 2010-10-15 10:39 EDT
Nmap scan report for hindmost (192.168.1.1)
Host is up (0.0054s latency).
rDNS record for 192.168.1.1: hindmost.example.com
Not shown: 997 filtered ports
PORT STATE SERVICE
21/tcp open ftp
23/tcp open telnet
80/tcp open http
```

<aside class="notes">
This output shows three open ports—21, 23, and 80—used by ftp, telnet, and http, respectively. If you weren’t
aware that these ports were active, you should log into the scanned system and investigate further, using netstat or
ps to locate the programs using these ports and, if desired, shut them down.
</aside>



## Nmap (III)
### Ejemplos (II)

* Escaneo de puertos TCP

```bash
$ nmap -sT hindmost
Starting Nmap 5.21 ( http://nmap.org ) at 2010-10-15 10:39 EDT
Nmap scan report for hindmost (192.168.1.1)
Host is up (0.0054s latency).
rDNS record for 192.168.1.1: hindmost.example.com
Not shown: 997 filtered ports
PORT STATE SERVICE
21/tcp open ftp
23/tcp open telnet
80/tcp open http
```



## Nmap (IV)
### Ejemplos (III)

* Escaneo de puertos UDP

```bash
$ sudo nmap -sU hindmost
Starting Nmap 5.21 ( http://nmap.org ) at 2010-10-15 10:39 EDT
Nmap scan report for hindmost (192.168.1.1)
Host is up (0.0054s latency).
rDNS record for 192.168.1.1: hindmost.example.com
Not shown: 997 filtered ports
PORT STATE SERVICE
21/tcp open ftp
23/tcp open telnet
80/tcp open http
```

<aside class="notes">
A few servers, though, run on UDP ports, so you need to scan them by typing nmap -sUhostname. (This
usage requires root privileges, unlike scanning TCP ports.)
</aside>
