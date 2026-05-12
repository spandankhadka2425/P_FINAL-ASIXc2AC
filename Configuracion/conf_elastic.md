Perfecto. He leído el PDF. Empezamos documentando la instalación y configuración de **Elasticsearch**.

---

# Instalación y Configuración de Elasticsearch

## Imagen 1 - Instalación de Elasticsearch

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
sudo apt install -y elasticsearch
```

**Que estamos haciendo:**
Instalamos Elasticsearch desde el repositorio oficial de Elastic. Este servicio es el motor de almacenamiento y búsqueda de logs. En la salida vemos que se descargan 670 MB y que la instalación ocupa 1301 MB en disco.

**Salida relevante:**
```
Get:1 https://artifacts.elastic.co/packages/9.x/apt stable/main amd64 elasticsearch amd64 9.3.4 [670 MB]
After this operation, 1301 MB of additional disk space will be used.
```

---

## Imagen 2 - Configuración de Elasticsearch

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Archivo modificado:** `/etc/elasticsearch/elasticsearch.yml`

**Contenido de la configuración:**
```yaml
cluster.name: soc-lab
node.name: srv1
network.host: 127.0.0.1
http.port: 9200
discovery.type: single-node
```

**Que estamos haciendo:**
Configuramos Elasticsearch como un cluster de un solo nodo. Definimos el nombre del cluster como `soc-lab`, el nombre del nodo como `srv1`, y configuramos la red para que escuche en `127.0.0.1` (solo local) y en el puerto `9200`.

---

## Imagen 3 - Reseteo de la contraseña del usuario elastic

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

**Que estamos haciendo:**
Reseteamos la contraseña del usuario `elastic` (usuario administrador por defecto de Elasticsearch). El sistema genera automáticamente una nueva contraseña que debemos guardar para futuras conexiones.

**Contraseña generada:**
```
J0wRlkV3Ftbrs7Cskup
```

---

## Imagen 4 - Exportación de la contraseña como variable de entorno

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
export ELASTIC_PASSWORD='J0wRlkV3Ftbrs7Cskup'
```

**Que estamos haciendo:**
Exportamos la contraseña de Elasticsearch como una variable de entorno para poder utilizarla fácilmente en futuros comandos sin tener que escribirla cada vez.

---

## Imagen 5 - Verificación del estado de Elasticsearch

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
sudo systemctl status elasticsearch --no-pager -l
```

**Salida relevante:**
```
Active: active (running) since Mon 2026-05-04 16:55:46 CEST; 9min ago
Memory: 2.3G
```

**Que estamos haciendo:**
Verificamos que el servicio de Elasticsearch está activo y funcionando correctamente. El estado muestra `active (running)`, lo que confirma que la instalación y configuración fueron exitosas.

---

## Imagen 6 - Comprobación de que Elasticsearch responde a peticiones

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5) o desde cualquier cliente

**URL consultada:**
```
https://192.168.140.5:9200/
```

**Que estamos haciendo:**
Accedemos a la API de Elasticsearch para comprobar que el servicio responde correctamente. La respuesta incluye información del cluster, la versión y el nombre del nodo, confirmando que Elasticsearch está operativo.

**Respuesta destacada:**
```json
{
  "name": "srv1",
  "cluster_name": "soc-lab",
  "version": {
    "number": "9.3.4",
    "lucene_version": "10.3.2"
  },
  "tagline": "You Know, for Search"
}
```

---

## Resumen de la Instalación de Elasticsearch

| Paso | Acción | Estado |
|------|--------|--------|
| 1 | Instalar paquete elasticsearch | Completado |
| 2 | Configurar elasticsearch.yml | Completado |
| 3 | Resetear contraseña del usuario elastic | Completado |
| 4 | Exportar contraseña como variable de entorno | Completado |
| 5 | Verificar estado del servicio | Completado |
| 6 | Comprobar respuesta de la API | Completado |

---

**¿Continuamos con la documentación de Kibana?**