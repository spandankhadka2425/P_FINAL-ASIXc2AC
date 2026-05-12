# Instalación y Configuración de Kibana

## Instalación de Kibana

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
sudo apt install -y kibana
```

**Que estamos haciendo:**
Instalamos Kibana desde el repositorio oficial de Elastic. Kibana es la interfaz gráfica que nos permitirá visualizar y explorar los logs almacenados en Elasticsearch de forma intuitiva.

**Salida relevante:**
```
Get:1 https://artifacts.elastic.co/packages/9.x/apt stable/main amd64 kibana amd64 9.3.4 [385 MB]
After this operation, 1173 MB of additional disk space will be used.
```

---

## Configuración de Kibana

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Archivo modificado:** `/etc/kibana/kibana.yml`

**Contenido de la configuración:**
```yaml
server.port: 5601
server.host: "0.0.0.0"
```

**Que estamos haciendo:**
Configuramos Kibana para que escuche en todas las interfaces de red (`0.0.0.0`) en el puerto `5601`. De esta forma, podremos acceder a la interfaz web desde cualquier máquina de la red, no solo desde el servidor local.

---

## Generación del token de enrolamiento para Kibana

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

**Que estamos haciendo:**
Generamos un token temporal que permitirá a Kibana autenticarse y conectarse de forma segura con Elasticsearch. Este token es válido por 30 minutos y debe usarse durante la configuración inicial de Kibana.

**Token generado (ejemplo parcial):**
```
eyJ2ZXIoiI4Lje0LjAILcJhZHIIoIsIMtKylJE2OC4xMjIumJQxOjkyMDAIXSwIZmdyIjoIyZc2OWF1...
```

![kibana](../Imagen/kibana_enrollment_token.png)


---

## Acceso a la interfaz web de Kibana

**Donde se ejecuta:** Navegador web desde cualquier máquina de la red

**URL de acceso:**
```
http://192.168.140.5:5601
```
![kibana](../Imagen/kibana_verification_code.png)


**Que estamos haciendo:**
Accedemos a la interfaz web de Kibana para verificar que está funcionando correctamente. Una vez conectado, solicitaremos el token de enrolamiento generado anteriormente para enlazar Kibana con Elasticsearch.


---

## Verificación del stack completo (Elasticsearch + Kibana)

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Direcciones accesibles:**
- Elasticsearch: `https://192.168.140.5:9200`
- Kibana: `http://192.168.140.5:5601`

**Que estamos haciendo:**
Confirmamos que tanto Elasticsearch como Kibana están operativos y accesibles desde la red. Elasticsearch se utiliza mediante HTTPS con autenticación, mientras que Kibana está configurado en HTTP para facilitar el acceso inicial.

---

## Resumen de la Instalación de Kibana

| Paso | Acción | Estado |
|------|--------|--------|
| 1 | Instalar paquete kibana | Completado |
| 2 | Configurar kibana.yml (host: 0.0.0.0, port: 5601) | Completado |
| 3 | Generar token de enrolamiento para Kibana | Completado |
| 4 | Acceder a la interfaz web de Kibana | Completado |
| 5 | Verificar integración con Elasticsearch | Completado |

---

## Resumen del Stack Elastic (Elasticsearch + Kibana)

| Componente | Puerto | Estado |
|------------|--------|--------|
| Elasticsearch | 9200 (HTTPS) | Activo |
| Kibana | 5601 (HTTP) | Activo |
| Usuario elastic | - | Contraseña generada |
| Cluster name | soc-lab | Configurado |

---

- [Index](../Index.md)
