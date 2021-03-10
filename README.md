# WireGuard
Configuración de WireGuard con Docker

## Pre-requisitos 📋
Instalar Docker, sustituir $Username por su nombre de usuario.
```
curl -sSL https://get.docker.com | sh
sudo apt-get update && sudo apt-get install -y docker-ce docker-compose
sudo usermod -a -G docker $Username
```
## Comenzando 🚀
Descargar/Crear el archivo **docker-compose.yaml** y guardarlo en *$HOME/username* o (se puede guardar en otra ubicación). 

``` yaml
version: '3.3'
services:
  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Mexico_City
      - SERVERURL=1.1.1.1 #optional
      - SERVERPORT=51820 #optional
      - PEERS=2 #optional
      - PEERDNS=auto #optional
      - INTERNAL_SUBNET=10.13.13.0 #optional
    volumes:
      - /root/wireguard:/config
      - /lib/modules:/lib/modules
      - /usr/src:/usr/src
    ports:
      - 51820:51820/udp
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    restart: unless-stopped
```

***Nota:*** Puedes cambiar **SERVERURL** por la IP de tu servidor. Cambiar el valor de **PEERS** por el número de clientes que tendra tu **VPN**.

## Instalación 🔧

En la terminal ubicandose en **$Home/username** o en la ruta del archivo **docker-compose.yaml** ejecutar lo siguiente:
```
docker-compose up -d
```
El contenedor se debe descargar e iniciar correctamente. Si presentas problemas verifica tu instalación de Docker y que hayas agregado tu usuario al grupo Docker.

Verificar los contenedores corriendo:
```
docker ps
```

## Obtener los ficheros de configuración 🔧
Los archivos se encuentran en el servidor en */root/wireguard* puedes copiarlos a tu máquina y compartirlos con los dispositivos a usar la VPN. Las carpetas se llaman ***peer1, peer2 .... peerN*** dependiendo del número de **PEERS** configurados en el archivo **.yaml**. Para cada usuario le corresponde una carpeta peer.

```bash
scp -r root@ip_server:/root/wireguard/peer /home/usuario/
```

## Conectarse a la VPN desde un móvil 🔧
Descargar la aplicación de **WireGuard** disponible para Android e iOS y escanear el **Código QR**, que viene en la carpeta peer, aceptar y listo ya estará configurada la **VPN**, verificar en una página web que estas navegando a tráves de la **ip pública** de tu servidor.

## Conectarse a la VPN desde GNU/LINUX 🔧
Tener previamente el archivo *.conf* de la carpeta *peer* que te fue asignada.

**Ubuntu**

Descargar WireGuard y Resolvconf
``` bash
sudo apt-get install wireguard resolvconf
```

**Fedora**
``` bash
sudo dnf install wireguard-tools
```

**Copiar el arhivo de configuración** en */etc/wireguard/*

Ejemplo:

```bash
sudo cp peer2.conf /etc/wireguard/wg0.conf
```

**Activar la VPN**
```bash
wg-quick up wg0
```
verificar en una página web que estas navegando a tráves de la **ip pública** de tu servidor

**Desactivar la VPN**
```bash
wg-quick down wg0
```

**Ver el estado de WireGuard**
```bash
sudo wg
```

Con esto queda configurado nuestra **VPN con WireGuard**

⌨️ con ❤️ por [Jaime Rodríguez](https://resume.rpjosejaime.com) 😊

