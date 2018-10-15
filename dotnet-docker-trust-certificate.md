# Workaround para que el container de dotnet no verifique los certificados de lor repos de nuget

## Premisas

Es necesario bajar paquetes de nuget de un repositorio hosteado pos https que no tiene un certificado válido instalado.

## Problema

En la version del container de dotnet `microsoft/dotnet:2.1.403-sdk` aunque se le adicione correctamente el certificado al sistema siguiendo esta [guía](add-cert-to-ubuntudebian-trust-store.md), el dotnet restore al tratar de bajar los paquetes da el error:

```bash
The remote certificate is invalid according to the validation procedure.
```

Un workaround existente es exportar lo siguiente:

```bash
export DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER=0
```

### Referencias

[Github: https://github.com/dotnet/core-setup/issues/4295](https://github.com/dotnet/core-setup/issues/4295)
