# Sprint 2: Infraestructura Completa del SOC

## Fecha de Inicio
27/04/2026

## Objetivo del Sprint

Completar toda la infraestructura del SOC, incluyendo balanceador de carga, servidores web con redundancia, replicación de base de datos, firewall perimetral, instalación de Wazuh, Elasticsearch y Kibana, y despliegue de agentes de monitorización.

---

## Planificación Inicial

Para este sprint, nos propusimos completar todas las tareas pendientes del proyecto:

- Configurar HAProxy principal y backup
- Crear servidor web backup con la misma página web
- Configurar replicación MySQL (Master-Slave)
- Instalar firewall perimetral con reglas de redirección
- Instalar Wazuh Manager y agentes
- Instalar Elasticsearch, Kibana y Filebeat
- Realizar pruebas de validación (SSH fallido, Nmap, rutas sospechosas)
- Documentar todos los pasos

---

## ¿Qué hemos hecho hasta ahora?

### 1. Balanceador de Carga (HAProxy)

- Instalamos HAProxy en SRV0 (principal) y SRV0BAK (backup)
- Configuramos redirección de HTTP a HTTPS
- Configuramos el certificado SSL autofirmado
- Configuramos el backup para failover manual (añadir IP .4 cuando sea necesario)

### 2. Servidores Web (Apache + PHP + HTTPS)

- Instalamos Apache2 y PHP en SRV2_A (principal) y SRV2_B (backup)
- Configuramos HTTPS con certificado autofirmado
- Creamos la página web Galería de Arte con login y registro
- Copiamos los archivos del principal al backup

### 3. Base de Datos (MySQL Master-Slave)

- Configuramos MySQL Master en SRV2_A
- Configuramos MySQL Slave en SRV2_B con replicación
- Verificamos que los cambios en el Master se replican automáticamente al Slave

### 4. Firewall Perimetral

- Configuramos una máquina dedicada como firewall
- Activamos el reenvío de IP (ip_forward)
- Configuramos reglas DNAT para redirigir tráfico:
  - Puerto 443 → HAProxy (192.168.140.4:443)
  - Puertos 2221-2225 → SSH a cada servidor
- Configuramos políticas de denegación por defecto

### 5. Wazuh (SIEM)

- Instalamos Wazuh Manager en SRV1
- Desplegamos agentes Wazuh en todas las máquinas
- Registramos cada agente con su clave única

### 6. Elastic Stack (Elasticsearch + Kibana + Filebeat)

- Instalamos Elasticsearch y Kibana en SRV1
- Configuramos Filebeat para enviar logs de Wazuh a Elasticsearch
- Creamos el Data View en Kibana para visualizar las alertas

### 7. Pruebas de Validación

- Realizamos intentos de SSH con usuario falso
- Accedimos a rutas web sospechosas (/admin, /wp-login.php, etc.)
- Ejecutamos escaneos de puertos con Nmap
- Intentamos conexiones FTP fallidas
- Verificamos que todas las alertas aparecen en Kibana

### 8. Documentación

- Documentamos todos los pasos de configuración en Markdown
- Capturamos pantallas de cada paso importante
- Creamos diagramas de arquitectura
---

- [Index](../Index.md)

*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 12/05/2026*