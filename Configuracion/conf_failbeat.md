# Instalación y Configuración de Filebeat

## Imagen 1 - Instalación de Filebeat

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Comando:**
```bash
sudo apt install -y filebeat
```

**Que estamos haciendo:**
Instalamos Filebeat desde el repositorio oficial de Elastic. Filebeat es un agente ligero que se encarga de recoger los logs de Wazuh y enviarlos a Elasticsearch para su almacenamiento y posterior visualización en Kibana.

**Salida relevante:**
```
Get:1 https://artifacts.elastic.co/packages/9.x/apt stable/main amd64 filebeat amd64 9.3.4 [69.3 MB]
After this operation, 264 MB of additional disk space will be used.
```

---

## Imagen 2 - Configuración de Filebeat para enviar logs de Wazuh a Elasticsearch

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Que estamos haciendo:**
Configuramos Filebeat para que lea los archivos de alertas generados por Wazuh Manager en `/var/ossec/logs/alerts/alerts.json` y los envíe a Elasticsearch para su indexación y almacenamiento centralizado.

**Ruta de logs configurada:**
```
/var/ossec/logs/alerts/alerts.json
```

---

## Imagen 3 - Verificación de la integración con Elasticsearch

**Donde se ejecuta:** Servidor SOC (SRV1 - 192.168.140.5)

**Que estamos haciendo:**
Comprobamos que los logs de Wazuh están llegando correctamente a Elasticsearch. Verificamos la existencia del índice `wazuh-alerts-*` que contiene los eventos almacenados.

**Resultado:**
```
282 documentos indexados en Elasticsearch
```

**Que confirma:**
La integración entre Wazuh, Filebeat y Elasticsearch está funcionando correctamente. Los logs se están enviando, procesando y almacenando sin errores.

---

## Resumen de la Configuración de Filebeat

| Paso | Acción | Estado |
|------|--------|--------|
| 1 | Instalar filebeat | Completado |
| 2 | Configurar filebeat para leer logs de Wazuh | Completado |
| 3 | Verificar llegada de logs a Elasticsearch | Completado |

---

## Resumen de la Integración del Stack SOC

| Componente | Función | Estado |
|------------|---------|--------|
| Wazuh Manager | SIEM - Genera alertas de seguridad | Activo |
| Filebeat | Recoge logs y los envía a Elasticsearch | Configurado |
| Elasticsearch | Almacena y indexa logs | Activo |
| Kibana | Visualiza logs y alertas | Activo |

---

