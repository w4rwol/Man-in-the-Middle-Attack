# NETWORK ATTACKS

## MAN IN THE MIDDLE

### DEFINICIÓN

> El atacante intercepta y retransmite los paquetes de red entre dos máquinas que actúan como si la conexión fuera directa. El atacante controla la conexión, cambiando lo que reciben o dejan de recibir. Normalmente usado para interceptar credenciales, cuentas y datos privados.

![Alt text](./img/Attack/1.png)

> El ataque más común funciona a traves de un malware que infecta el navegador del usuario robando los datos que envíe o reciba y desencriptándolos conectándose con el sitio real actuando como un proxy.

- Hay diferentes tipos de ataques MitM:

![Alt text](./img/Attack/2.png)

- **Internet Protocol Spoofing:** Se hacen con la IP del servicio y la suplantan permitiendo recoger los datos de todos los que se conecten al falso dispositivo.

- **Domain Name System Spoofing:** Manipulan un DNS oficial para redireccionar el tráfico hacia sitios falsos.

- **HTTP Spoofing:** El atacante cambia la sesión de una página web con HTTPS a una con HTTP.

- **Secure Sockets Layer hijacking:** Usando un servidor seguro y otro ordenador para interceptar la información.

- **Email hijacking:** Posee los correos oficiales de organismos tanto para informase de los clientes como para actuar en nombre del organismo.

- **Wi-Fi eavesdropping:** Común en redes Wi-Fi públicas, el atacante puede crear una red falsa a la que accederán los usuarios y les robaría la información que transmitan.

- **Session hijacking:** Roban o suplantan las cookies del usuario pudiendo acceder a sesiones como del banco, tiendas y realizar acciones en su nombre.

- **Cache poisoning:** También llamado ARP cache poisoning, permite al atacante monitorear el trafico dentro de la subred en la que se encuentra.

### EJEMPLO

Vamos a ver un ejemplo de Internet Protocol Spoofing:

Usando una máquina con el SO de Kali Linux, descargamos e instalamos un programa llamado `bettercap`.

> - Recuerda hacer un `sudo apt-get update`.
> - Bettercap es un framework que ataca a redes Wi-Fi, dispositivos Blue-Tooth y redes IPv4/IPv6.

Una vez instalado, lo ejecutamos y con el comando `help` nos dará información de los módulos. Usaremos cada módulo según nuestra necesidad.

![Alt text](./img/Attack/3.png)

En este caso vamos a rastrear los equipos que hay conectados a la red.

```php
net.probe on        # Activará el módulo indicado. (Rastrea la red)
net.show            # Mostrará en una tabla todos los dispositivos rastreados.
```

![Alt text](./img/Attack/4.png)

Vemos como ha encontrado mi otra máquina de Windows 10.

Ahora vamos a activar el módulo arp.spoof, este módulo pretende enmascarar la máquina de Kali con otra MAC para que el Win10 se crea que es un destino para llegar al router y salir a internet.

```php
help arp.spoof      # Muestra las opciones y parámetros.
arp.spoof on        # Activa el módulo.
set arp.spoof.target <IP/MAC>   # Añade un objetivo a través de su IP o MAC (funciona con rangos de IP)
set arp.spoof.fullduplex true # La máquina finge no tener internet (es un router)
```

![Alt text](./img/Attack/5.png)

Vemos como ha empezado a suplantar como router al Windows 10.

Ahora necesitamos rastrear lo que manda y recibe de la red.
Para eso activamos el módulo `net.sniff`

```php
net.sniff on        # Rastrea todo lo que pasa por la red.
```

![Alt text](./img/Attack/6.png)

Como muestra la imagen ha empezado a rastrear la conexión que esta haciendo el Windows 10.

Ahora en el Windows 10 vamos a entrar en una página web usando el protocol HTTP e introducir datos de inicio de sesión.

![Alt text](./img/Attack/8.png)

Cuando pulsemos el botón de login, nuestra máquina recibe los datos que se han introducido.

![Alt text](./img/Attack/7.png)

### VULNERABILIDAD Y PROTECCIÓN

Este ataque afecta a toda la red y sistemas, ya que puede consigue los datos de conexión que pasan por la red, incluyendo credenciales de los sistemas.

Para protegerte asegúrate de que las conexiones sean por el protocolo HTTPs, evita abrir correos sospechosos (el banco no te va a pedir tu usuario), usar VPNs y usar programas anti-malware.
