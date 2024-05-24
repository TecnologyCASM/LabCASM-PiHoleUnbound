# Link to Pi-hole official site https://pi-hole.net/
# Link to Pi-hole documentation on for getting unbound setup https://docs.pi-hole.net/guides/dns/unbound/
![image](https://github.com/TecnologyCASM/PiHoleUnbound/assets/107158068/c0177465-14c0-4fd7-af6a-fc1b355c84af)
![image](https://github.com/TecnologyCASM/PiHoleUnbound/assets/107158068/abea8b26-16e6-4af6-92b3-8a8d9f24ba02)

### Pi-hole environment variables

> Variables y descripciones replicadas del [official pihole container](https://github.com/pi-hole/docker-pi-hole/#environment-variables):

| Variable | Default | Value | Description |
| -------- | ------- | ----- | ---------- |
| `TZ` | UTC | `<Timezone>` | Configura tu [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) para asegurarse de que los registros roten a la medianoche local en lugar de a la medianoche UTC.
| `WEBPASSWORD` | random | `<Admin password>` | http://pi.hole/admin contraseña. Ejecuta `docker logs pihole \| grep random` para encontrar tu pase aleatorio.
| `FTLCONF_LOCAL_IPV4` | unset | `<Host's IP>` | Configure la IP LAN de su servidor, utilizada por los modos de bloqueo web y la dirección de enlace lighttpd.
| `REV_SERVER` | `false` | `<"true"\|"false">` | Habilite el reenvío condicional de DNS para la resolución de nombres de dispositivos |
| `REV_SERVER_DOMAIN` | unset | Dominio de Red | Si el reenvío condicional está habilitado, configure el dominio del enrutador de la red local |
| `REV_SERVER_TARGET` | unset | IP del enrutador | Si el reenvío condicional está habilitado, configure la IP del enrutador de la red local |
| `REV_SERVER_CIDR` | unset | Inverso DNS | Si el reenvío condicional está habilitado, configure la zona DNS inversa (e.g. `10.7.1.0/24`) |
| `WEBTHEME` | `default-light` | `<"default-dark"\|"default-darker"\|"default-light"\|"default-auto"\|"lcars">`| Tema de interfaz de usuario a utilizar.

Ejemplo `.env` archivo en el mismo directorio que su archivo `docker-compose.yaml`:

```
FTLCONF_LOCAL_IPV4=10.7.1.10
TZ=America/Santo_Domingo
WEBPASSWORD=password
REV_SERVER=true
REV_SERVER_DOMAIN=local
REV_SERVER_TARGET=10.7.1.1
REV_SERVER_CIDR=10.7.1.0/24
HOSTNAME=PiHole
DOMAIN_NAME=PiHole.local
PIHOLE_WEBPORT=8080
WEBTHEME=default-dark
```
# Link to Portainer official site https://www.portainer.io/
![image](https://github.com/TecnologyCASM/PiHoleUnbound/assets/107158068/afcc9c35-a15b-4ea3-8fe8-f29f6be312d2)