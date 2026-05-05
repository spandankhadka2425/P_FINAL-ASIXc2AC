 # En esta parte, documentaremos todas las pruebas que realizamos durante el período de nuestro proyecto.

## Pruebas realizadas durante la configuración de la base de datos.

## 1. 

![prova_mysql](../Imagen/prova_mysql.png)

**Comando:**
```bash
mysql -u root -p -e "SHOW DATABASES;"
```

**Que estoy haciendo:**
Estoy listando todas las bases de datos en MySQL para verificar que la base de datos `soc_alerts` se haya creado correctamente.

**Por qué es necesario para nuestro proyecto:**
Esta comprobación es fundamental porque nuestro SOC necesita almacenar todas las alertas de seguridad en una base de datos estructurada. Sin la base de datos `soc_alerts`, no podríamos guardar las alertas generadas por Wazuh, ni mostrarlas en nuestro panel HTML personalizado. Verificar que la base de datos existe antes de continuar con la creación de tablas nos asegura que todo el sistema de almacenamiento de alertas funcionará correctamente.

**Que muestra esto:**
La base de datos `soc_alerts` aparece en la lista, lo que confirma que se creó correctamente. Las otras cuatro bases de datos son del sistema de MySQL y vienen por defecto.

**Conclusión:**
Esta verificación era necesaria para garantizar que nuestro SOC tendrá donde almacenar las alertas de seguridad. Si la base de datos no apareciese, significaría que hubo un error en su creación y tendríamos que crearla de nuevo antes de avanzar con el proyecto.

---

## 2.

![prova_mysql](../Imagen/prova_mysql_2.png)


**Comando:**
```bash
mysql -u root -p -e "DESCRIBE soc_alerts.alerts;"
```

**Qué estoy haciendo:**
Estoy mostrando la estructura de la tabla `alerts` dentro de la base de datos `soc_alerts` para verificar que se creó correctamente con todos los campos necesarios.

**Por qué es necesario para nuestro proyecto:**
Esta comprobación es fundamental porque la tabla `alerts` es donde se almacenarán todas las alertas de seguridad generadas por Wazuh. Necesitamos asegurarnos que la tabla tiene los campos correctos para guardar información como la IP del atacante, el tipo de alerta, la gravedad, la descripción y el estado de cada incidente. Sin una tabla bien estructurada, nuestro SOC no podría registrar ni gestionar las alertas de seguridad.

**Qué muestra esto:**
La tabla `alerts` tiene todos los campos que diseñamos:
- `id` como identificador único y automático
- `timestamp` para registrar cuándo ocurrió la alerta
- `source_ip` para guardar la IP del atacante
- `machine_name` para saber qué máquina fue atacada
- `alert_type` para clasificar el tipo de ataque
- `severity` para indicar la gravedad (Crítica, Alta, Media, Baja)
- `description` para detalles del ataque
- `wazuh_rule_id` para identificar la regla de Wazuh que detectó el ataque
- `status` con valor por defecto "new" para seguimiento de alertas

**Conclusión:**
Esta verificación era necesaria para confirmar que la tabla de alertas está correctamente estructurada antes de que el SOC comience a recibir y almacenar alertas reales. Si algún campo faltase o tuviese un tipo incorrecto, las alertas no se guardarían correctamente y el sistema fallaría.


# Pruebas realizadas durante la creación del Web Backup (SRV2B)


# 1.

## Contexto del Servidor Backup

Este servidor (SRV2B) actúa como respaldo del servidor web principal (SRV2). Su función es mantener la misma página web y la misma base de datos para que, si el servidor principal falla, HAProxy pueda redirigir automáticamente el tráfico hacia este servidor backup. De esta forma, la conexión con los usuarios se mantiene sin interrupciones.

---

## Explicación de la imagen

**Comando:**
```bash
curl http://localhost
```

**Qué estoy haciendo:**
Estoy probando que el servidor web backup (SRV2B) es capaz de servir la página web correctamente desde su propio directorio local.

**Por qué es necesario para nuestro proyecto:**
Esta comprobación es fundamental porque este servidor debe ser idéntico al servidor principal para que HAProxy pueda redirigir el tráfico sin problemas. Si el servidor backup no puede mostrar la página web correctamente, los usuarios experimentarían errores cuando el servidor principal fallase. Verificar que `curl http://localhost` devuelve el código HTML de la Galería de Arte nos asegura que el servidor backup está listo para asumir el servicio cuando sea necesario.

**Salida del comando:**
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Galería</title>

<link rel="preload" as="image" href="img/CLB.png">

<style>
* {
    box-sizing: border-box;
}
</style>
```

**Qué muestra esto:**
El servidor backup devuelve correctamente el código HTML de la página web. Vemos el título "Galería", la referencia a la imagen `img/CLB.png` y el estilo CSS. Esto confirma que:
- Apache está funcionando correctamente en el backup
- Los archivos de la página web se descomprimieron correctamente en `/var/www/html/`
- La página web es accesible localmente en el servidor backup

**Conclusión:**
Esta prueba confirma que el servidor backup está correctamente configurado y puede servir la misma página web que el servidor principal. Si el servidor principal (SRV2) falla, HAProxy puede redirigir el tráfico a este backup y los usuarios seguirán viendo la Galería de Arte sin interrupciones.

---

## Comando extraído

```bash
curl http://localhost
```


# Verificación de la Página Web en Servidores Principal y Backup

## Contexto

Tenemos dos servidores web configurados para nuestro proyecto SOC Security:
- **Servidor Principal (SRV2_A)**: IP 192.168.140.2
- **Servidor Backup (SRV2_B)**: IP 192.168.140.7

El servidor backup actúa como réplica exacta del principal. Si el servidor principal falla, HAProxy redirigirá automáticamente el tráfico al backup para que los usuarios no noten ninguna interrupción del servicio.

---

## Imagen 1 - Página Web del Servidor Principal (192.168.140.2)

![Pagina Principal](../Imagen/prova_mysql_3.png)

**Características visibles:**
- Título: "ArtGalería"
- Subtítulo: "SOC Security Project | Monitorización en Tiempo Real"
- Menú de navegación: Galería, Lista de Obras, Acerca de
- Barra de búsqueda y filtros por categoría (Todos, Renacimiento, Romanticismo, Impresionismo)
- Contador: "6 Obras de Arte"
- Contador de visitas: "3 Visitas"
- Fecha de última actualización: "May 2026"
- Botón "Obra Aleatoria"
- Badge "SOC Monitorizada 24/7"

---

## Imagen 2 - Página Web del Servidor Backup (192.168.140.7)

![Pagina Backup](../Imagen/prova_mysql_4.png)

**Características visibles:**
- Título: "ArtGalería"
- Subtítulo: "SOC Security Project | Monitorización en Tiempo Real"
- Menú de navegación: Galería, Lista de Obras, Acerca de
- Barra de búsqueda y filtros por categoría (Todos, Renacimiento, Romanticismo, Impresionismo)
- Contador: "6 Obras de Arte"
- Contador de visitas: "5 Visitas"
- Fecha de última actualización: "May 2026"
- Botón "Obra Aleatoria"
- Badge "SOC Monitorizada 24/7"
---

## Comandos utilizados para verificar

```bash
# Verificar servidor principal
curl -I http://192.168.140.2

# Verificar servidor backup
curl -I http://192.168.140.7

# Verificar página completa desde navegador
# Principal: http://192.168.140.2
# Backup: http://192.168.140.7
```
La única diferencia son las visitas, eso se debe a que he visitado el backup más veces que la página web del servidor principal, por lo que cuenta la cantidad de visitas.

*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 05/05/2026*


- [Index](../Index.md)
