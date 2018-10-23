# Manejo determinístico de imágenes de docker utilizando su sha256

Las etiquetas de las imagenes de docker NO son inmutables, lo que quiere decir que en el tiempo la imagen denotada por mongo:33.2 puede variar. Si queremos trabajar con una imagen de docker que sea siempre exactamente la misma, debemos usar el sha256 de la imagen.

## Obtener el sha256 de una imagen existente

```bash
export IMAGE=mongo:3.2
docker inspect --format='{{index .RepoDigests 0}}' $IMAGE
```

La salida de este comande es algo como:

```bash
mongo@sha256:eb46c95ca2434d193552144bffc83a09154b1a0548e0e530f720e4a8ae2154db
```

## Utilizar el sha256 de la imagen para crear un container

```bash
docker create ... mongo@sha256:eb46c95ca2434d193552144bffc83a09154b1a0548e0e530f720e4a8ae2154db
```
