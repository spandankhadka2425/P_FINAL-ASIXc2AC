```markdown
# Configuración del Load Balancer HAProxy (SRV0)

## Imagen 1 - Instalación de HAProxy

**Donde se ejecuta:** Servidor Load Balancer (SRV0 - 192.168.140.4)

**Comando:**
```bash
sudo apt install haproxy -y
```

**Que estamos haciendo:**
Instalando HAProxy en el servidor load balancer (SRV0). HAProxy será el encargado de distribuir el tráfico entre los servidores web principal y backup, y también redirigirá las peticiones al servidor SOC cuando sea necesario.

![Instalacion HAProxy](../Imagen/haproxy_install.png)

---

## Imagen 2 - Verificación de la instalación y arranque del servicio

**Donde se ejecuta:** Servidor Load Balancer (SRV0 - 192.168.140.4)

**Comandos:**
```bash
haproxy -v
sudo systemctl enable haproxy
sudo systemctl start haproxy
sudo systemctl status haproxy
```

**Que estamos haciendo:**
Verificamos que HAProxy se ha instalado correctamente con `haproxy -v` (versión 2.4.30). Luego habilitamos el servicio para que arranque automáticamente con el sistema con `systemctl enable`. Arrancamos el servicio con `systemctl start` y comprobamos que está activo y corriendo con `systemctl status`. El estado muestra "active (running)" lo que indica que HAProxy funciona correctamente.

![Verificacion HAProxy](../Imagen/haproxy_status.png)

---

## Imagen 3 - Copia de seguridad y edición del archivo de configuración

**Donde se ejecuta:** Servidor Load Balancer (SRV0 - 192.168.140.4)

**Comandos:**
```bash
sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak
sudo nano /etc/haproxy/haproxy.cfg
```

**Que estamos haciendo:**
Primero creamos una copia de seguridad del archivo de configuración original de HAProxy con `sudo cp`. Esto nos permite recuperar la configuración anterior si algo sale mal. Luego editamos el archivo de configuración `haproxy.cfg` con nano para definir los servidores web (principal y backup), las rutas de redirección y los puertos de escucha.

![Configuracion HAProxy](../Imagen/haproxy_config.png)

---

## Contenido del archivo de configuración (haproxy.cfg)

**Donde se ejecuta:** Servidor Load Balancer (SRV0 - 192.168.140.4)

**Archivo:** `/etc/haproxy/haproxy.cfg`

**Comando para ver el contenido:**
```bash
sudo cat /etc/haproxy/haproxy.cfg
```

**Que estamos haciendo:**
A continuación se muestra el contenido del archivo de configuración que hemos creado para HAProxy. En este archivo definimos los servidores web (SRV2_A como principal y SRV2_B como backup), el puerto de escucha (80), y las reglas de balanceo.

![Contenido Configuracion HAProxy](../Imagen/haproxy_config_contenido.png)

---

## Resumen de Comandos por Servidor

| Servidor | IP | Comandos ejecutados |
|----------|-----|---------------------|
| **SRV0 (Load Balancer)** | 192.168.140.4 | `apt install haproxy`, `haproxy -v`, `systemctl enable haproxy`, `systemctl start haproxy`, `systemctl status haproxy`, `cp`, `nano`, `cat` |

---

## Archivos de Configuración Modificados

| Archivo | Propósito |
|---------|-----------|
| `/etc/haproxy/haproxy.cfg` | Configuración principal de HAProxy (frontend, backend, servidores) |
| `/etc/haproxy/haproxy.cfg.bak` | Copia de seguridad de la configuración original |

---

*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 04/05/2026*

```markdown
# Configuracion de HTTPS en HAProxy

## Imagen 1 - Copia del certificado PEM al servidor HAProxy

**Que estamos haciendo:**
Copiamos el archivo `artgaleria.pem` desde el servidor web principal (SRV2) al servidor HAProxy (SRV0). Este archivo contiene el certificado SSL y la clave privada combinados, necesarios para que HAProxy pueda terminar las conexiones HTTPS y redirigir el trafico de forma segura.

**Comando utilizado:**
```bash
scp /tmp/artgaleria.pem isard@192.168.140.4:/home/isard/
```

---

## Imagen 2 - Verificacion del contenido del archivo PEM

**Que estamos haciendo:**
Visualizamos el contenido del archivo `artgaleria.pem` generado. Este archivo contiene el certificado SSL (bloque BEGIN CERTIFICATE) y la clave privada (bloque BEGIN PRIVATE KEY). Ambos son necesarios para que HAProxy pueda cifrar y descifrar las conexiones HTTPS.

**Archivo generado:** `/tmp/artgaleria.pem`

---

## Imagen 3 - Generacion del archivo PEM combinado

**Que estamos haciendo:**
Combinamos el certificado SSL y la clave privada en un unico archivo PEM. Utilizamos `sudo cat` para leer el certificado (`artgaleria.crt`) y la clave (`artgaleria.key`), y redirigimos la salida a `/tmp/artgaleria.pem`. Este formato PEM es el que requiere HAProxy para funcionar con HTTPS.

**Comando utilizado:**
```bash
sudo cat /etc/ssl/artgaleria/artgaleria.crt /etc/ssl/artgaleria/artgaleria.key | sudo tee /tmp/artgaleria.pem
```

---

## Imagen 4 - Configuracion final de HAProxy para HTTPS

**Que estamos haciendo:**
Configuramos HAProxy para que escuche en el puerto 443 (HTTPS) y termine las conexiones SSL utilizando el certificado PEM. La configuracion incluye:
- `frontend http_front`: redirige todo el trafico HTTP a HTTPS (301 redirect)
- `frontend https_front`: escucha en el puerto 443, aplica el certificado SSL y envia el trafico al backend
- `backend web_servers`: balancea la carga entre el servidor principal (192.168.140.2) y el backup (192.168.140.7), ambos en puerto 443 con verificacion SSL activada

**Archivo modificado:** `/etc/haproxy/haproxy.cfg`

**Contenido principal:**
```haproxy
frontend http_front
    bind *:80
    redirect scheme https code 301 if !{ ssl_fc }

frontend https_front
    bind *:443 ssl crt /etc/haproxy/ssl/artgaleria.pem
    mode http
    default_backend web_servers

backend web_servers
    mode http
    balance roundrobin
    option httpchk GET /
    http-check expect status 200
    server srv2_main 192.168.140.2:443 check ssl verify none
    server srv2_backup 192.168.140.7:443 check ssl verify none backup
```

---

## Resumen de la Configuracion

| Paso | Accion | Estado |
|------|--------|--------|
| 1 | Copiar archivo PEM al servidor HAProxy | Completado |
| 2 | Verificar contenido del archivo PEM | Completado |
| 3 | Generar el archivo PEM combinado | Completado |
| 4 | Configurar HAProxy para HTTPS | Completado |

---

*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 12/05/2026*



- [Index](../Index.md)
