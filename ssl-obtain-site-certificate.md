# Obtener site.crt

Para obtener el certificado de cualquier sitio expuesto por ssl:

```bash
openssl s_client -showcerts -connect sitio:443 </dev/null 2>/dev/null| openssl x509 -outform PEM > sitio.crt
```
