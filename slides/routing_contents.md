## Introducción

* Un [router](http://es.wikipedia.org/wiki/Router) es un dispositivo que permite interconectar dos redes.

<a class="fancybox" href="img/router.jpg" data-fancybox-group="gallery" title="eduroam">
<img height="550px" src="img/router.redimensionado50.jpg" alt="eduroam">
</a>

note:
* As explained earlier, routers pass traffic from one network to another.
* You configure your Linux system to directly contact systems on the local network.
* You also give the computer a router’s address, which your system uses as a gateway to the Internet at large.
* Any traffic that’s not destined for the local network is directed at this router, which passes it on to its destination.
* In practice, there are likely to be a dozen or more routers between you and most Internet sites.
* Each router has at least two network interfaces and keeps a table of rules concerning where to send data based on the destination IP address.
* Your own Linux computer has such a table, but it’s probably very simple compared to those on major Internet routers.



## Introducción (II)

* Si queremos configurar uno de los nodos de nuestra red como si fuera un router, además del dispositivo hardware, tendrías que:
 * [Usar el comando route](#/2/3)
 * [Activar forwarding](#/2/6)
 * [Comprobar la resolución de direcciones](#/2/7)
 * [Usar el comando `ip`](#/2/10)

note:
* In addition to routing configuration, some routing-related commands are available that can help you understand network addressing on your network.



## Comando route (I)

* Debemos usar el comando [`route`](http://linux.die.net/man/8/route) que me permite ajustar la tabla de enrutamiento
```bash
route {add | del} [-net | -host] target [netmask nm] [gw gw]
[reject] [[dev] interface]
```
* En base al destino del paquete, le asignamos un interfaz u otro.

note:
* This task is handled, in part, by the route command.
* This command can be used to do much more than specify a single gateway system, though, as described earlier.
* A simplified version of the route syntax is as follows:
route {add | del} [-net | -host] target [netmask nm] [gw gw]
[reject] [[dev] interface]
</aside>



## Comando route (II)
### Ejemplos

<a class="fancybox" href="img/example_routing.png" data-fancybox-group="gallery" title="Ejemplo enrutamiento">
<img height="450px" src="img/example_routing.redimensionado50.png" alt="Ejemplo enrutamiento">
</a>

<span class="fragment fade-in">
```bash
route add -net 10.10.10.0 netmask 255.255.255.0 gw 172.24.22.1
```
</span>

note:
As an example and particularly the two workstations in the 172.24.22.0/24 network. These
computers both have a single network interface, but the network has two routers: The Linux computer in the
center , which leads to the 192.168.5.0/24 network and the Internet; and the 172.24.22.1 router,
which leads to the 10.10.10.0/24 network. Presumably these computers will have a default route set, as described
earlier, to link to the Internet. You would then use the route command to add a second route to enable traffic to
pass to the 10.10.10.0/24 network. You can set up this route with the following command:
# route add -net 10.10.10.0 netmask 255.255.255.0 gw 172.24.22.1



## Comando route (III)
### Opciones (I)

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
        <td width="170pt">`add`
        </td>
        <td>Añade nuevo elemento de la tabla de enrutamiento
        </td>
      </tr>

      <tr>
        <td>`del`
        </td>
        <td>
          Borra nuevo elemento de la tabla de enrutamiento
        </td>
      </tr>

      <tr>
        <td>`-net`
        </td>
        <td> Interpretamos el objetivo como una dirección de red
        </td>
      </tr>

      <tr>
        <td>`-host`
        </td>
        <td>Interpretamos el objetivo como una dirección de host
        </td>
      </tr>

      <tr>
        <td>`network`
        </td>
        <td>Establece mascara de red
        </td>
      </tr>

    </tdody>
  </table>
</div>

note:
* You specify add or del along with a target (a computer or network address) and optionally other parameters.
* The -net and -host options force route to interpret the target as a network or computer address, respectively.
* The netmask option lets you set a netmask as you desire,
* The reject keyword installs a blocking route, which refuses all traffic destined for the specified network.
* gw  lets you specify a router through which packets to the specified target should go.
* Blocking routes appear in routing tables with an exclamation mark (!) under the Fstlags column.
* Finally, although route can usually figure out the interface device (for instance, eth0) on its own, you can force the issue with the dev option.



## Comando route (III)
### Opciones (II)

<div class="table-responsive">
  <table class="table table-condensed table-hover table-bordered">
    <thead>

      <tr>
        <th width="170pt">Opción</th>
        <th>Descripción</th>
      </tr>
    </thead>
    <tbody>

       <tr>
        <td>`gw`
        </td>
        <td>Indica el router por donde deben pasar los paquetes que van a un determinado objetivo
        </td>
      </tr>

      <tr>
        <td>`reject`
        </td>
        <td>crea una routa bloqueante (aparecen marcadas como !)
        </td>
      </tr>

      <tr>
        <td>`dev`
        </td>
        <td>
          Dispositivo
        </td>
      </tr>
    </tdody>
  </table>
</div>

note:
* You specify add or del along with a target (a computer or network address) and optionally other parameters.
* The -net and -host options force route to interpret the target as a network or computer address, respectively.
* The netmask option lets you set a netmask as you desire,
* The reject keyword installs a blocking route, which refuses all traffic destined for the specified network.
* gw  lets you specify a router through which packets to the specified target should go.
* Blocking routes appear in routing tables with an exclamation mark (!) under the Fstlags column.
* Finally, although route can usually figure out the interface device (for instance, eth0) on its own, you can force the issue with the dev option.



## Activación forwarding

<a class="fancybox" href="img/example_routing.png" data-fancybox-group="gallery" title="Pasos para trabajar con volúmenes lógicos">
<img height="450px" src="img/example_routing.redimensionado50.png" alt="Pasos para trabajar con volúmenes lógicos">
</a>

```bash
$ echo "1" > /proc/sys/net/ipv4/ip_forward
```
```bash
$ echo "net.ipv4.ip_forward = 1" >>/etc/sysctl.conf
```

note:
* A Linux computer that’s connected to two networks can communicate with both of them;
* however, this configuration does not automatically link the two networks together.
* That is, computers on the 192.168.5.0/24 and 172.24.22.0/24 networks will not be able to communicate with each other unless that central Linux computer is configured as a router.
* To enable this feature, you must modify a key file in the /proc filesystem:
# echo "1" > /proc/sys/net/ipv4/ip_forward
* This command enables IP forwarding.
* Permanently setting this option requires modifying a configuration file.
* Some distributions set it in /etc/sysctl.conf:
net.ipv4.ip_forward = 1
Other distributions use other configuration files and options, such as /etc/sysconfig/sysctl and its
IP_FORWARD line. If you can’t find it, try using grep to search for ip_forward or IP_FORWARD, or modify a local
startup script to add the command to perform the change.



## Comprobando resolución de direcciones

* [ARP (Address Resolution Protocol)](http://es.wikipedia.org/wiki/ARP) es el protocolo responsable de encontrar la dirección hardware (Ethernet MAC) que corresponde a una determinada dirección IP.
* Obtenemos la dirección física de una dispositivo de red ejecutando
```bash
# ifconfig eth0 | grep HWaddr
eth0 Link encap:Ethernet HWaddr 00:A0:CC:24:BA:02
```
* El comando [arp](http://linux.die.net/man/8/arp) en linux implementa el protocolo ARP

note:
* TCP/IP relies on a protocol known as the Address Resolution Protocol (ARP).
* A Linux utility of the same name but in lowercase (arp) is used to monitor ARP activity on a single computer.
* Network hardware devices, such as Ethernet cards, have built-in hardware addresses.
* You can determine a network interface’s hardware address with ifconfig; it’s on the first line of output, labeled HWaddr:
# ifconfig eth0
eth0 Link encap:Ethernet HWaddr 00:A0:CC:24:BA:02



## ARP (II)

<a class="fancybox" href="img/arp_request.png" data-fancybox-group="gallery" title="Pasos para trabajar con volúmenes lógicos">
<img height="450px" src="img/arp_request.png" alt="Pasos para trabajar con volúmenes lógicos">
</a>

``` bash
$ arp
Address              HWtype HWaddress         Flags Mask Iface
hindmost.example.com ether  00:a0:c5:24:e1:4e C          eth0
seeker.example.com   ether  00:1c:c0:b0:35:fc C          eth0
```

note:
* When two computers on the same network segment communicate, they do so by using each others’ hardware addresses.
* When routers are involved, the sender sends the traffic to the router’s hardware address; the router then repackages the data, using the next router’s hardware address.
* To communicate by hardware address, though, the IP address must be translated into a hardware address.  This is ARP’s job.
* A computer can send a broadcast message (that is, one that’s received by all the local computers) asking the computer with a particular IP address to reply.
* When the target computer replies, ARP reads its hardware address, associates it with the requested IP address, and adds the two to the ARP cache.
* The arp utility enables you to examine and modify the ARP cache. Typing arp alone displays the ARP cache:



## ARP (III)
### Opciones

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
    <thead>

      <tr>
        <th width="230pt">Larga</th>
        <th width="270pt">Corta</th>
        <th>Descripción</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>N/A
        </td>
        <td>
          `-d address`
        </td>
        <td>
          Borra una entrada de la caché ARP
        </td>
      </tr>

      <tr>
        <td>N/A
        </td>
        <td>
          `-s address hw_address`
        </td>
        <td>
          Asociada una IP o nombre con una dirección MAC
        </td>
      </tr>

      <tr>
        <td>`--verbose`
        </td>
        <td>
           `-v`
        <td>
            Información adicional
        </td>
        </td>
      </tr>

      <tr>
        <td>`--numeric`
        </td>
        <td>
           `-n`
        <td>
            Muestra la IP en lugar del hostname
        </td>
        </td>
      </tr>

    </tdody>
  </table>
</div>



## ARP(IV)
### Opciones (II)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
    <thead>

      <tr>
        <th width="310pt">Larga</th>
        <th width="180pt">Corta</th>
        <th>Descripción</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>`--hw-type type`
        </td>
        <td>
          `-t type` <br> `-H type`
        </td>
        <td>
          Establece tipo de hardware (ether | arcnet |pronet | netrom )
        </td>
      </tr>

      <tr>
        <td>N/A
        </td>
        <td>
          `-a`
        </td>
        <td>
         Salida corta estilo BSD-style.
        </td>
      </tr>

      <tr>
        <td> `--use-device`
        </td>
        <td>
           `-D`
        <td>
            Interpreta `hw_address` como un dispositivo
        </td>
        </td>
      </tr>

    </tdody>
  </table>
</div>



## ARP(V)
### Opciones (III)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
    <thead>

      <tr>
        <th width="310pt">Larga</th>
        <th width="270pt">Corta</th>
        <th>Descripción</th>
      </tr>
    </thead>
    <tbody>

      <tr>
        <td>`--device interface`
        </td>
        <td>
           `-i interface`
        <td>
            Restricción de accione a un interfaz
        </td>
        </td>
      </tr>
 <tr>
        <td>`--file filename`
        </td>
        <td>
            `-f filename`
        <td>
             Obtiene la información de dirección de un fichero
        </td>
        </td>
      </tr>
    </tdody>
  </table>
</div>



## Comando [ip](http://linux.die.net/man/8/ip) (I)

* Es una alternativa a los comando `ifconfig`, `route`, etc
```bash
ip [ OPTIONS ] OBJECT { COMMAND | help }
```
* El elemento fundamemntal es el objeto

note:
* To understand ip, you should begin with the OBJECT, which can take any of the values shown in the following table.
* Each of these values has its own unique set of options and commands. * The number of operations that can be performed with ip is dizzying.



## Comando ip (II)
### Ejemplos
```bash
$ ip route list
172.24.21.0/24 dev eth1 proto kernel scope link src 172.24.21.2
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.2
127.0.0.0/8 dev lo scope link
default via 192.168.1.1 dev eth0
```

note:
This example reveals that the ip utility doesn’t work exactly like the separate utilities that perform similar
tasks—this output is different from the route output that can also reveal (or change) the routing table. As a
practical matter, you might want to try ip and the many specific tools to determine which you prefer for particular
tasks.



## Comando ip (III)
### Posibles objetos (I)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">
    <thead>
<tr>
<th width="250pt">Nombre</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`link`</td>
<td>Realiza acciones sobre el hardware de red</td>
</tr>
<tr>
<td>`addr`</td>
<td>Asocia o desasocia un dispositivo con una dirección</td>
</tr>
<tr>
<td>`addrlabel`</td>
<td>Muestra o ajusta la configuración IPv6</td>
</tr>
<tr>
<td>`route`</td>
<td>Muestra o ajusta la tabla de entutamiento</td>
</tr>
<tr>
<td>`rule`</td>
<td>Muestra o ajusta las reglas de cortafuegos</td>
</tr>
</tbody>

  </table>
</div>



## Comando ip
### Posibles objetos (II)

<div class="table-responsive">
  <table class="table table-condensed  table-hover table-bordered">

<thead>
<tr>
<th width="170pt">Nombre</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`neigh`</td>
<td>Muestra o ajusta entradas ARP</td>
</tr>
<tr>
<td>`tunnel`</td>
<td>Muestra o ajusta características del [tunneling](http://en.wikipedia.org/wiki/Tunneling_protocol) features</td>
</tr>
<tr>
<td>`maddr`</td>
<td>Muestra o ajusta direcciones multicast</td>
</tr>
<tr>
<td>`mroute`</td>
<td>Muestra o ajusta routers multicast</td>
</tr>
<tr>
<td>`monitor`</td>
<td>Monitoriza tráfico en la red</td>
</tr>
</tbody>
</table>
</div>
