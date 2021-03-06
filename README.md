# WireGuard
Configuraci贸n de WireGuard con Docker

## Pre-requisitos 馃搵
Instalar Docker, sustituir $Username por su nombre de usuario.
```
curl -sSL https://get.docker.com | sh
sudo apt-get update && sudo apt-get install -y docker-ce docker-compose
sudo usermod -a -G docker $Username
```
## Comenzando 馃殌
Descargar/Crear el archivo **docker-compose.yaml** y guardarlo en *$HOME/username* o (se puede guardar en otra ubicaci贸n). 

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

***Nota:*** Puedes cambiar **SERVERURL** por la IP de tu servidor. Cambiar el valor de **PEERS** por el n煤mero de clientes que tendra tu **VPN**.

## Instalaci贸n 馃敡

En la terminal ubicandose en **$Home/username** o en la ruta del archivo **docker-compose.yaml** ejecutar lo siguiente:
```
docker-compose up -d
```
El contenedor se debe descargar e iniciar correctamente. Si presentas problemas verifica tu instalaci贸n de Docker y que hayas agregado tu usuario al grupo Docker.

Verificar los contenedores corriendo:
```
docker ps
```

## Obtener los ficheros de configuraci贸n 馃敡
Los archivos se encuentran en el servidor en */root/wireguard* puedes copiarlos a tu m谩quina y compartirlos con los dispositivos a usar la VPN. Las carpetas se llaman ***peer1, peer2 .... peerN*** dependiendo del n煤mero de **PEERS** configurados en el archivo **.yaml**. Para cada usuario le corresponde una carpeta peer.

```bash
scp -r root@ip_server:/root/wireguard/peer /home/usuario/
```

## Conectarse a la VPN desde un m贸vil 馃敡
Descargar la aplicaci贸n de **WireGuard** disponible para Android e iOS y escanear el **C贸digo QR**, que viene en la carpeta peer, aceptar y listo ya estar谩 configurada la **VPN**, verificar en una p谩gina web que estas navegando a tr谩ves de la **ip p煤blica** de tu servidor.

## Conectarse a la VPN desde GNU/LINUX 馃敡
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

**Copiar el arhivo de configuraci贸n** en */etc/wireguard/*

Ejemplo:

```bash
sudo cp peer2.conf /etc/wireguard/wg0.conf
```

**Activar la VPN**
```bash
wg-quick up wg0
```
verificar en una p谩gina web que estas navegando a tr谩ves de la **ip p煤blica** de tu servidor

**Desactivar la VPN**
```bash
wg-quick down wg0
```

**Ver el estado de WireGuard**
```bash
sudo wg
```

Con esto queda configurado nuestra **VPN con WireGuard**

鈱笍 con 鉂わ笍 por [Jaime Rodr铆guez](https://resume.rpjosejaime.com) 馃槉

