# Adicionar certificado a los certificados de confianza de Ubuntu/Debian

## Premisas

En muchas ocaciones es necesario confiar en certificados inválidos, para poder automatizar o mejorar la automatización dell uso de herramientas que se conecten a esos sitios, por ejemplo, utilizar repositorios de paquetes de `maven`, `npm`, `nuget`, etc.

## Problema

Si se trata de acceder a estos sitios, en dependencia de la herramienta que se utilice sera necesario buscar los settings necesarios para que no se verifiquen los certificados del sitio, o que se confie en los mismos, una solución unica sería hacer que el sistema confie en el certificado del sitio.

## Pasos

1- Obtener el certificado del sitio.

Para esto, utilizar la guía [1](ssl-obtain-site-certificate.md)

2- Adicionar el certificado a los certificados de confianza del sistema

2.1- Copiar el cetificado, con extensión `.crt` a `/usr/local/share/ca-certificates/`, ejemplo:

```bash
sudo cp ~/sitio.crt /usr/local/share/ca-certificates/
```

2.2- Actualizar los certificados de confianza del sistema.

```bash
sudo update-ca-certificates
```

Checkpoint: Verificar que el certificado se adicionó correctamente a los certificados de confianza del sistema:

```bash
openssl s_client -showcerts -connect nexus.datys.cu:443 </dev/null 2>/dev/null
```
