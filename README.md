![image-20210506104427925](https://tva1.sinaimg.cn/large/008i3skNgy1gq8sv4q7cqj303k03kweo.jpg)



### ¿Que es este repositorio?

Este repositorio es una implementación de Træfik que te facilitará la vida para crear reverse proxy para los distintos servicios dockerizados.


## Configuración

Crear red externa para conectar las aplicaciones con Træfik 

```sh
docker network create web
```

clonar este repositorio

```sh
cd /opt
git clone https://github.com/aitorroma/docker-traefik.git traefik
cd traefik
```

Ajustar las variables de nombre de dominio principal y corre en el fichero `.env` .

### `ACME_EMAIL`

Dirección de correo electrónico utilizada para el registro ACME. Valor predeterminado: 'webmaster@my.domain.tld'

### `DOCKER_DOMAIN`

Dominio base predeterminado utilizado para las reglas de la interfaz.
Puede anularse estableciendo la etiqueta "traefik.domain" en un contenedor.
Valor predeterminado: 'my.domain.tld'

## Uso

```sh
cd /opt/traefik
docker-compose up -d
```

Este comando levantara un contenedor escuchando en el puerto 80 y 443 listo para realizar de proxy inverso.

### Usarlo en los contenedores.

Para que funcione en los contenedores tan solo tendrás que añadir este bloque  en el docker compose.

```
env_file: .env
labels:
      traefik.enable: 'true'
      traefik.frontend.rule: "Host:${HOSTNAME}"
      traefik.port: '3333'
    expose:
      - "80"
      - "443"
    networks:
      - default
      - web  
```

Por ejemplo en un docker compose completo.

```
version: '3'
services:
  notion-proxy:
    build:
      context: .
      dockerfile: Dockerfile
    image: notion-proxy
    container_name: notion-proxy
    restart: unless-stopped
    env_file: .env
    labels:
      traefik.enable: 'true'
      traefik.frontend.rule: "Host:${HOSTNAME}"
      traefik.port: '3333'
    expose:
      - "80"
      - "443"
    volumes:
      - ./config:/notion-proxy/config
      - ./cache:/notion-proxy/cache
    networks:
      - default
      - web  
   
```

En el fichero ```.env``` establecer la variable ```HOSTNAME=``` o también se puede añadir directamente el nombre de host.

El valor ``traefik.port`` define el puerto al que va a hacer el reverse proxy.



## Invitación a mi Canal.

Estás invitado a mi canal de telegram, donde publico más soluciones como esta.

![Telegram-icon](https://tva1.sinaimg.cn/large/008i3skNgy1guctnvd002j600w00w0r202.jpg)https://t.me/aitorroma

----------------------------------------------------------

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/J3J64AN17)

