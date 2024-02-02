> **¿Quieres ver 42 Quebec en 3D?**
> **:arrow_right: https://mini42qc.vercel.app/ :arrow_left:**

# Guide to NetPractice

![netpractice logo](img/NetPractice.png)

<div id="top"></div>

## Otros idiomas

[Korean](README.ko.md)
<br>
[French](README.fr.md)
<br>
[English](README.md)

## Tabla de contenido

- [Conceptos importantes](#conceptos-importantes)
  - [TCP](#tcp-capa-de-transporte)
  - [dirección IP](#dirección-ip-capa-de-red)
  - [Máscara de subred](#máscara-de-subred)
  - [Switch](#switch)
  - [Router](#router)
- [Niveles](#niveles)

---

## Conceptos importantes

### TCP: capa de transporte

</br>
<p align="center">
  <img src="./img/tcp-ip-stack.png" height=300 width=200 alt="mask">
</p>
</br>

TCP es el acrónimo de **Transmission Control Protocol** (Protocolo de control de transmisión). Es un estándar de comunicaciones que permite que los programas de aplicación y los dispositivos intercambien mensajes a través de una red. Se utiliza para enviar paquetes a través de Internet.

TCP garantiza la integridad de los datos que se comunican a través de una red. Antes de transmitir datos, TCP establece una conexión entre un origen y su destino, que permanece activa hasta que comienza la comunicación. Luego divide grandes cantidades de datos en paquetes más pequeños, al tiempo que garantiza la entrega de un extremo a otro sin pérdida de datos.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

---

### Dirección IP: Capa de red

</br>
<p align="center">
  <kbd><img src="https://github.com/lpaube/NetPractice/blob/main/img/ip1.png?raw=true" height=250 alt="mask"></kbd>
</p>
</br>

IP es parte de un conjunto de protocolos de Internet, que también incluye el protocolo de control de transmisión. Juntos, estos dos se conocen como TCP/IP. El conjunto de protocolos de Internet rige las reglas para empaquetar, direccionar, transmitir, enrutar y recibir datos a través de redes.

El direccionamiento IP es un medio lógico de asignar direcciones a dispositivos en una red. Cada dispositivo conectado a Internet requiere una dirección IP única.

Una dirección IP tiene dos partes; una parte identifica el host, como una computadora u otro dispositivo, y la otra parte identifica la red a la que pertenece. TCP/IP utiliza una [máscara de subred](#máscara-de-subred) para separarlos.
</br>
</br>

#### IPv4 vs. IPv6

Las direcciones IP vienen en 2 versiones: IPv4 e IPv6:
<br>

<p align="center">
  <kbd><img src="https://github.com/lpaube/NetPractice/blob/main/img/ip_version.png?raw=true" height=100 alt="ip_versions"></kbd>
</p>
<br>

El Protocolo de Internet versión 4 (IPv4) define una dirección IP como un número de 32 bits. Sin embargo, debido al crecimiento de Internet y al agotamiento de las direcciones IPv4 disponibles, en 1998 se estandarizó una nueva versión de IP (IPv6), que utiliza 128 bits para la dirección IP. Sin embargo, en NetPractice sólo se utilizan direcciones IPv4.
</br>
</br>

#### Dirección pública vs. Dirección privada

Una dirección IP pública es una dirección IP a la que se puede acceder directamente a través de Internet y que su proveedor de servicios de Internet (ISP) asigna a su enrutador de red. Una dirección IP pública (o externa) le ayuda a conectarse a Internet desde dentro y fuera de su red.

Una dirección IP privada es una dirección que su enrutador de red asigna a su dispositivo. A cada dispositivo dentro de la misma red se le asigna una dirección IP privada única (a veces llamada dirección de red privada); así es como los dispositivos en la misma red interna se comunican entre sí.

Cuando una red está conectada a Internet, no puede utilizar una dirección IP de las direcciones IP privadas reservadas. Los siguientes rangos están reservados para direcciones IP privadas:

```
192.168.0.0 – 192.168.255.255 (65,536 dirección IP)
172.16.0.0 – 172.31.255.255   (1,048,576 dirección IP)
10.0.0.0 – 10.255.255.255     (16,777,216 dirección IP)
```

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

---

### Máscara de subred

</br>
<p align="center">
  <kbd><img src="https://github.com/lpaube/NetPractice/blob/main/img/mask1.png?raw=true" height=250 alt="mask"></kbd>
</p>
</br>

Una máscara de subred es una dirección de 32 bits (4 bytes) que se utiliza para distinguir entre una dirección de red y una dirección de host en la dirección IP. Define el rango de direcciones IP que se pueden utilizar dentro de una red o subred.
</br>
</br>

#### Encontrar la dirección de red

La _Interfaz A1_ anterior tiene las siguientes propiedades:

```
Dirección IP | 104.198.241.125
Máscara      | 255.255.255.128
```

Para determinar qué parte de la dirección IP es la dirección de red, debemos aplicar la máscara a la dirección IP. Primero conviertamos la máscara a su forma binaria:

```
Máscara | 11111111.11111111.11111111.10000000
```

Los bits de una máscara que son 1 representan la dirección de red, mientras que los bits restantes de una máscara que son 0 representan la dirección del host. Ahora conviertamos la dirección IP a su forma binaria:

```
Dirección IP | 01101000.11000110.11110001.01111101
Máscara      | 11111111.11111111.11111111.10000000
```

Ahora podemos aplicar la máscara a la dirección IP a través de una operación [AND bit a bit](https://en.wikipedia.org/wiki/Bitwise_operation#AND) para encontrar la dirección de red de la IP:

```
Dirección de red | 01101000.11000110.11110001.00000000
```

Lo que se traduce en una dirección de red de `104.198.241.0`.
</br>
</br>

#### Encontrar el rango de direcciones de host

Para determinar qué direcciones de host podemos usar en nuestra red, tenemos que usar los bits de nuestra dirección IP dedicados a la dirección de host. Usemos nuestra dirección IP y máscara anteriores:

```
Dirección IP | 01101000.11000110.11110001.01111101
Máscara      | 11111111.11111111.11111111.10000000
```

El rango posible de nuestras direcciones de host se expresa a través de los últimos 7 bits de la máscara, que son todos 0. Por lo tanto, el rango de direcciones de host es:

```
BINARIO | 0000000 - 1111111
DECIMAL | 0 - 127
```

Para obtener el rango de direcciones IP posibles para nuestra red, agregamos el rango de direcciones de host a la dirección de red. Nuestro rango de posibles direcciones IP pasa a ser `104.198.241.0 - 104.198.241.127`.

<ins>SIN EMBARGO</ins>, los extremos del rango están reservados para usos específicos y no pueden usarse en una interfaz:

```
104.198.241.0   | Reservado para representar la dirección de red.
104.198.241.127 | Reservado como dirección de transmisión; Se utiliza para enviar paquetes a todos los hosts de una red.
```

Por lo tanto, nuestro rango de IP real es `104.198.241.1 - 104.198.241.126`, que podría haberse encontrado usando una [calculadora de IP](https://www.calculator.net/ip-subnet-calculator.html).
</br>
</br>

#### Notación CIDR (/24)

La máscara también se puede representar con el Classless Inter-Domain Routing (CIDR). Este formulario representa la máscara como una barra diagonal "/", seguida del número de bits que sirven como dirección de red.

Por lo tanto, la máscara en el ejemplo anterior de `255.255.255.128`,es equivalente a una máscara de `/25` usando la notación CIDR, ya que 25 bits de 32 bits representan la dirección de red.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

---

### Switch

</br>
<p align="center">
  <kbd><img src="https://github.com/lpaube/NetPractice/blob/main/img/switch1.png?raw=true" height=150 alt="switch"></kbd>
</p>
</br>

Un switch (conmutador) conecta varios dispositivos en una sola red. A diferencia de un router (enrutador), el switch no tiene ninguna interfaz, ya que solo distribuye paquetes a su red local y no puede comunicarse directamente con una red fuera de la suya.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

---

### Router

</br>
<p align="center">
  <kbd><img src="https://github.com/lpaube/NetPractice/blob/main/img/route1.png?raw=true" height=200 alt="router"></kbd>
</p>
</br>

Así como el switch conecta varios dispositivos en una sola red, el router conecta varias redes juntas. El router tiene una interfaz para cada red a la que se conecta.

Dado que el router separa diferentes redes, el rango de posibles direcciones IP en una de sus interfaces no debe superponerse con el rango de sus otras interfaces. Una superposición en el rango de direcciones IP implicaría que las interfaces están en la misma red.
</br>
</br>

#### Tabla de enrutamiento

</br>
<p align="center">
  <kbd><img src="https://github.com/lpaube/NetPractice/blob/main/img/routing_table1.png?raw=true" height=150 alt="routing_table"></kbd>
</p>
</br>

Una tabla de enrutamiento es una tabla de datos almacenada en un router o host de red que enumera las rutas a destinos de red particulares. En NetPractice, la tabla de enrutamiento consta de 2 elementos:

- **Destination**: El destino especifica una dirección de red en la que un host es el destino final de los paquetes. La ruta `default` o `0.0.0.0/0`, es la ruta que entra en vigor cuando no hay otra ruta disponible para una dirección IP de destino. La ruta predeterminada utilizará la dirección del siguiente salto para enviar los paquetes en su camino sin proporcionar un destino específico. La ruta predeterminada coincidirá con cualquier red.

- **Next hop**: El next hop (siguiente salto) se refiere al siguiente router más cercano por el que puede pasar un paquete. Es la dirección IP del siguiente router en el camino del paquete. Cada router mantiene su tabla de enrutamiento con una dirección de siguiente salto.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

## Niveles

<details>
  <summary>Level 1</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level1_paint.png?raw=true" alt="level1">
  <br>
  <br>

**1.** Since _Client A_ and _Client B_ are on the same network, their IP address must represent the same network in accordance with the subnet mask.
<br>
The subnet mask is _255.255.255.0_, which means that the first 3 bytes of the IP address represent the network, and the 4th byte represents the host. Since we are on the same network, only the host can change.
<br>
The solution will be anything in the range of **104.96.23.0 - 104.96.23.255** excluding the following 3:

- **104.96.23.0:** The first number in the range of hosts (0 in this case) represents the network and cannot be used by a host.
- **104.96.23.255:** The last number in the range of hosts (255 in this case) represents the broadcast address.
- **104.96.23.12:** This address is already used by the host _Client B_.

**2.** The same reasoning as _1._, however the subnet mask is _255.255.0.0_ in this case. The first 2 bytes of the IP address will represent the network; and the last 2 bytes, the host address.
<br>
The solution will be anything in the range of **211.191.0.0 - 211.191.255.255**, excluding:

- **211.191.0.0:** Represents the network address.
- **211.191.255.255:** Represents the broadcast address.
- **211.191.89.75:** Already taken by host _Client C_.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 2</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level2_paint.png?raw=true" alt="level2">
  <br>
  <br>

**1.** Since _Client B_ is on the same private network as _Client A_, they should have the exact same subnet mask.
<br>
The solution can only be **255.255.255.224**.

**2.** To understand the subnet mask of _255.255.255.224_, let's look at it in binary form, along with the IP _192.168.20.222_ of _Client B_:

<center>

```
MASK: 11111111.11111111.11111111.11100000
IP:   11000000.10101000.00010100.11011101
```

</center>
As we can see, the first 27 bits represent the IP address, while only the last 5 bits represent the host address.
<br>
All these 27 bits representing the network must stay the same in the IP addresses of hosts on the same network. To get the answer, we can only change the last 5 bits.
<br>
<br>
The answer is in the range of:

```
BIN:  11000000.10101000.00010100.11000000 - 11000000.10101000.00010100.11011111
or
DEC:  192.168.20.192 - 192.168.20.223
```

Excluding:
<br>

- **11000000.10101000.00010100.11000000:** Represents the network address (notice all 0 in the last 5 bits).
- **11000000.10101000.00010100.11011111:** Represents the broadcast address (notice all 1 in the last 5 bits).
- **11000000.10101000.00010100.11011110:** _Client B_ already has that address.

**3.** Here we are introduced the slash "/" notation for the subnet mask on _Interface D1_. A subnet mask of _/30_ means that the first 30 bits of the IP address represent the network address, and the remaining 2 bits represent the host address:

<center>

```
Mask /30: 11111111.11111111.11111111.11111100
```

</center>

We can see that this binary number corresponds to the decimal _255.255.255.252_, therefore it is identical to the mask found on _Interface C1_.
<br>
<br>
The answers can then be any address, as long as they meet the following conditions:

- The network address (first 30 bits) must be identical for _Client D_ and _Client C_.
- The host bits (last 2 bits) cannot be all 1, nor all 0.
- _Client D_ and _Client C_ do not have identical IP addresses.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 3</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level3_paint.png?raw=true" alt="level3">
  <br>
  <br>

This exercise introduces the use of the **switch** (_Switch S_ in this example). The switch links multiple hosts of the same network together.
<br>
<br>

**1.** _Client A_, _Client B_, and _Client C_ are all on the same network. Therefore, they must all have the same subnet mask. Since _Client C_ already has the mask _255.255.255.128_, the mask for _Interface B1_ and for _Interface A1_ will also be _255.255.255.128_ (or in slash notation: _/25_).
<br>
<br>
The IP address of _Interface B1_ and _Interface C1_ must be on the same network range as the IP of _Client A_. This range is:

  <center>

```
104.198.241.0 - 104.198.241.128
```

  </center>
  Excluding of course the network address and the broadcast address.

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 4</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level4_paint.png?raw=true" alt="level4">
  <br>
  <br>

This exercise introduces the **router**. The router is used to link multiple networks together. It does so with the use of multiple interfaces (_Interface R1_, _Interface R2_, and _Interface R3_ in this example).
<br>
<br>

**1.** Since none of the masks on _Interface B1_, _Interface A1_, and _Interface R1_ are entered, we are free to choose our own subnet mask. A mask of **/24** is ideal as it leaves us with the entire 4th byte for the host address, and does not require binary calculations to find the range of possible host addresses.
<br>
<br>
The IP address of _Interface B1_ and _Interface R1_ must have the same network address as the IP address of _Interface A1_. With a subnet of _/24_, the possible range is:

  <center>

```
85.17.5.0 - 85.17.5.255
```

  </center>
  Excluding the network address and the broadcast address.
  <br>
  <br>

Note that we did not interact with the router _Interface R2_ and _Interface R3_, since none of our communications had to reach these sides of the router.

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 5</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level5_paint.png?raw=true" alt="level5">
  <br>
  <br>

This level introduces **routes**. A route contains 2 fields, the first one is the **destination** of outbound packets, the second one is the **next hop** of the packets.
<br>

The **destination** _default_ is equivalent to _0.0.0.0/0_, which will send the packets indiscriminately to the first network address it encounters. A destination address of _122.3.5.3/24_ would send the packets to the network _122.3.5.0_.

  <br>
  The **next hop** is the IP address of the next  router (or internet) interface to which the interface of the current machine must send its packets.
  <br>
  <br>

**1.** _Client A_ only has 1 route through which it can send its packets. There is no use specifying a numbered destination. The destination _default_ will send the packets to the only path available.
<br>
<br>
The next hop address must be the IP address of the next router's interface on the packets' way. The next interface is _Interface R1_, with the IP address of _54.117.30.126_. Note that the next interface is not _Interface A1_, since this is the sender's own interface.

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 6</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level6_paint.png?raw=true" alt="level6">
  <br>
  <br>

This level introduces the **internet**. The internet behaves like a router. However, if an interface is connected directly or indirectly to the internet, it **cannot** have an IP address in the following reserved private IP ranges:

```
192.168.0.0 - 192.168.255.255 (65,536 IP addresses)
172.16.0.0 - 172.31.255.255   (1,048,576 IP addresses)
10.0.0.0 - 10.255.255.255     (16,777,216 IP addresses)
```

**1.** The **next hop** of the internet is already entered, and matches the IP address of the _Interface R2_. Therefore we only need to bother with the destination of the internet.
<br>
<br>
The internet must send its packets to _Client A_. To do so, the internet's destination must match the network address of _Client A_. Let's find the network address of _Client A_:
<br>
_Client A_'s mask is _255.255.255.128_, which is equivalent to _/25_. This means that the first 25 bits of its IP address are its network address. We know then that the first 3 bytes (24 bits) of its IP address make part of its network address:

  <center>

```
40.178.145.?
```

  </center>

We now only need to find out if the 25th bit is a 1 or a 0.
<br>
If we convert the number 227 to binary, we get `11100011`. The first digit, which corresponds to the 25th bit, is a 1. Since only the 25th bit is part of the network address and not the remaining 7 bits, we get `10000000` for the last byte of the network address, which is 128 in decimal.
<br>
<br>
The full network address is:

  <center>

```
40.178.145.128
```

  </center>

With a range of _40.178.145.129 - 40.178.145.254_ for its host addresses.
<br>
<br>
We can now put this address of **40.178.145.128** in the Internet destination. The **/25** following the destination address represents the mask applied to its address.
<br>
<br>
A destination of _40.178.145.227/25_ is equivalent to the destination address _40.178.145.128/25_, since the mask of _/25_ will turn all the bits after the 25th to 0 to get the destination's network address.

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 7</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level7_paint.png?raw=true" alt="level7">
  <br>
  <br>

This level introduces the concept of **overlaps**. The range of IP addresses of a network must not overlap the range of IP addresses of a separate network. Networks are separated by routers.
<br>
<br>

**1.** We have 3 separate networks:
<br>

1. Between _Client A_ and _Router R1_.
2. Between _Router R1_ and _Router R2_.
3. Between _Router R2_ and _Client C_.

For _Interface A1_, we cannot choose our IP address freely since the IP of _Interface R11_ is already entered. Also, if we give it a mask of _/24_, the IP address range will overlap with the range of _Interface R12_, which is already entered. They would both be in the range of _93.198.14.0 - 93.198.14.255_.
<br>
<br>

Since we need addresses for 3 separate networks, it is convenient to split the last bytes of the address into 4 or more address ranges. We do this by using a mask of _/26_ or higher. The mask of _/28_ for example will give us 16 ranges, from which we use the following 3:

```
93.198.14.1 - 93.198.14.14    (Client A to Router R1)
93.198.14.65 - 93.198.14.78   (Router R1 to Router R2)
93.198.14.241 - 93.198.14.254 (Router R2 to Client C)
```

To calculate the possible ranges of a mask:
<br>
https://www.calculator.net/ip-subnet-calculator.html?cclass=any&csubnet=28&cip=93.198.14.2&ctype=ipv4&printit=0&x=97&y=13

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 8</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level8_paint.png?raw=true" alt="level8">
  <br>
  <br>

**1.** The hosts _Client C_ and _Client D_ will send packets to the internet, then the internet will respond by sending packets all the way back to the initial sender. To send these packets, the internet uses the destination _49.175.13.0/26_ to send the packets to the networks in the range of `49.175.13.0 - 49.175.13.63`.
<br>
<br>
All the receiving networks must be in this range, without overlapping each other.
<br>
<br>

**2.** On _Interface R23_ and _Interface R22_ we use the mask _255.255.255.240_ (or _/28_), to conveniently split the range of _/26_ from the destination address, into 4 separate ranges. This separation of 4 is necessary since we have the following 3 networks that must not overlap:
<br>

1. _Router R1_ to _Router R2_.
2. _Router R2_ to _Client C_.
3. _Router R2_ to _Client D_.

Each of these networks can then be attributed one of the following IP ranges with a mask of _/28_:

```
49.175.13.0 - 49.175.13.15
49.175.13.16 - 49.175.13.31
49.175.13.32 - 49.175.13.47
49.175.13.48 - 49.175.13.63
```

Note that the network address (first) and the broadcast address (last) must be excluded from each range.
<br>
<br>

**3.** The destination and next hop for the internet are already entered. We only need to enter the next hop for the _Router R2_, which is the IP on the _Interface R21_.

<div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 9</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level9_paint.png?raw=true" alt="level9">
  <br>
  <br>

This level is quite straightforward since the internet does not initially send its packets to a specific network. Therefore, the separate networks do not need to share a common address range. I would suggest simply following the 6 goals of the level one by one until the level is completed.
<br>
<br>
Remember not to use the network addresses from the reserved private IP ranges.
<br>
<br>

**1.** **Goal 3** states that we must connect _meson_ with the _internet_. The _internet_ will then have to respond to _meson_, so we enter _meson's_ network address in the _internet's_ destination.
<br>
<br>
**Goal 6** states that we must connect _cation_ with the _internet_, so we enter _cation's_ network address in the _internet's_ destination.
<br>
<br>
It is normal to have an empty field for the 3rd destination of the _internet_, and in _Router R1's_ destination. Not all fields of the routing tables need to be filled.

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>

---

<details>
  <summary>Level 10</summary>
  <br>
  <img src="https://github.com/LPaube/42_NetPractice/blob/main/img/level10_paint.png?raw=true" alt="level10">
  <br>
  <br>

At this level, there are 4 different networks:
<br>

1. _Router R1_ to _Switch S1_
2. _Router R1_ to _Router R2_
3. _Router R2_ to _Client H4_
4. _Router R2_ to _Client H3_
   <br>

**1.** The internet must be able to send its packets to all the hosts, so its destination must cover the range of networks of all the hosts.
<br>
<br>

_Interface R11_ and _Interface R13_ already have an IP address entered. This IP address only differs in its last byte. _Interface R11_ has for last byte **1**, and _Interface R13_ has for last byte **254**. To cover this wide range to IP addresses, we take a mask of **/24** for the _internet's_ destination. This destination will cover a range of `70.101.30.0 - 70.101.30.255`.

  <br>
  <br>

**2.** When choosing the IP addresses, we must make sure of 2 things:
<br>

1. The IP address is covered by the _internet_ destination.
2. The IP address range of the various networks does not overlap.
   <br>

With the IP addresses already entered (greyed out), let's examine the ranges covered by the various networks:
<br>

1. _Router R1_ to _Switch S1_ - Covers the range **70.101.30.0 - 70.101.30.127** (mask /25).
2. _Router R2_ to _Client H4_ - Covers the range **70.101.30.128 - 70.101.30.191** (mask /26).
3. _Router R1_ to _Router R2_ - Covers the range **70.101.30.252 - 70.101.30.255** (mask /30).
4. _Router R2_ to _Client H3_ - ??? (mask ???).

The only IP addresses left for the network "Router R2 to Client H3" are **70.101.30.192 - 70.101.30.251**. We can pick any mask that will let us take 2 IP addresses from that range to put in _Interface R22_ and _Interface R31_.

  <div align="right">
  <b><a href="#top">↥ back to top</a></b>
</div>
</br>

</details>