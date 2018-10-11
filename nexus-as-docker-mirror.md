# Cómo configurar Sonatype Nexus 3 como mirror del registro de docker

Esta guia se probó en Ubuntu 16.04 y 18.04, pero pasos similares se pueden hacer para otras distribuciones de linux

## Prerequisitos

- Sonatype Nexus 3 instalado y configurado
- Tener en el nexus un proxy del registro de docker
- El proxy del registro de docker debe estar expuesto por https

## Parámetros

nexus-registry-proxy: nexus.local:50001 => este es el host y puerto donde esta publicado el proxy de nexus que apunta al docker registry
nexus-certificate: es el certificado PEM base 64 con el que está expuesto [nexus-registry-proxy]

## Pasos

### 1- Configurar el docker engine para que confíe en el certificado del nexus

1.1: Crear el camino `/etc/docker/certs.d/[nexus-registry-proxy]`

ejemplo:

```bash
sudo mkdir -p /etc/docker/certs.d/nexus.local:50001
```

1.2: Crear el fichero `/etc/docker/certs.d/[nexus-registry-proxy]/ca.crt`

ejemplo:

```bash
sudo touch /etc/docker/certs.d/nexus.local:50001/ca.crt
```

1.3 Copiar el contenido de [nexus-certificate] al fichero `/etc/docker/certs.d/[nexus-registry-proxy]/ca.crt`

Checkpoint: Verificar los pasos anteriores

```bash
sudo docker pull nexus.local:50001/nginx:1.13.9-alpine
1.13.9-alpine: Pulling from nginx
Digest: sha256:7129a88c455160dc25ce8499e1a4bfc4cb09ca5a67402ec974b9ae96bf2d94d0
Status: Downloaded newer image for nexus.local:50001/nginx:1.13.9-alpine
```

Si da algun error, es necesario revizar los pasos anteriores.

### 2 - Configurar el proxy de nexus como mirror del registro de docker

2.1 Modificar o crear el fichero `/etc/docker/daemon.json` para adicionarle la configuracion del mirror

ejemplo:

```file
{
  "registry-mirrors":["https://nexus.local:50001"]
}
```

2.2 Reiniciar el servicio de docker

ejemplo:

```bash
sudo systemctl restart docker.service
```

Checkpoint: Verificar que el nexus está actuando como mirror del registro de docker

```bash
docker pull nginx:1.13.9-alpine
1.13.9-alpine: Pulling from library/nginx
Digest: sha256:7129a88c455160dc25ce8499e1a4bfc4cb09ca5a67402ec974b9ae96bf2d94d0
Status: Downloaded newer image for nginx:1.13.9-alpine
```
