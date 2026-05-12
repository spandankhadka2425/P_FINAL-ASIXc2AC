# Sprint 2 Review - SOC Security (Sprint Final)

## Fecha de la Review
12/05/2026

## Participantes

| Rol | Nombre |
|-----|--------|
| Scrum Master | Spandan (SK) |
| Product Owner | Anmolpreet (SA) |

---

## Resumen del Sprint 2

El Sprint 2 ha sido el sprint final del proyecto (del 27/04/2026 al 12/05/2026). El objetivo era completar toda la infraestructura del SOC, incluyendo la configuracion de HAProxy, la replicacion de bases de datos, la instalacion de Wazuh y Elastic Stack, y la preparacion de los agentes de monitorizacion.

---

## Tareas Completadas (Done) - Sprint 2

| Tarea | Descripcion |
|-------|-------------|
| Arquitectura (diagrama) | Definir la arquitectura del proyecto y justificar las tecnologias elegidas |
| Preparacion entorno | Crear todas las maquinas virtuales necesarias para el SOC (manager, elastic, db, web, clientes) |
| Elastic Stack | Instalar Elasticsearch y configurar el motor de almacenamiento de logs |
| Wazuh | Instalar Wazuh Manager para la monitorizacion centralizada |
| Crear backup de la maquina HAProxy | Configurar HAProxy backup para failover manual |
| Backup de server de BD y WebServer | Crear servidor backup con replicacion MySQL y misma configuracion web |
| Automatizacion | Crear script de estado de agentes para detectar si estan activos o caidos |
| Logs | Instalar Filebeat y definir rutas de logs a monitorizar |
| Agentes | Desplegar agentes en las 4 maquinas monitorizadas y registrarlos en Wazuh |

---

## Tareas En Curso (Doing) - Sprint 2

| Tarea | Descripcion | Responsable |
|-------|-------------|-------------|
| Validacion SOC | Generar eventos de prueba (logins, errores) para generar logs y verificar alertas en Wazuh | SA |
| Pruebas reales | Escaneo con Nmap y ataque de fuerza bruta SSH para simular reconocimiento de red | SA |

---

## Tareas Pendientes (To Do) - Sprint 2

| Tarea | Descripcion |
|-------|-------------|
| Seguridad | Configurar firewall limitando acceso a puertos necesarios y configurar SSH seguro |

---

## Estado del ProofHub al Final del Sprint 2

| Columna | Tareas | Estado |
|---------|--------|--------|
| Finalizadas (Done) | 8 tareas | Completadas |
| En curso (Doing) | 2 tareas | En progreso |
| Pendientes (To Do) | 1 tarea | Pendiente |

---

## Completados del Sprint 2

| Completado | Descripcion |
|------------|-------------|
| Diagrama de arquitectura | Esquema de red del proyecto con todas las maquinas, IPs y conexiones |
| Maquinas virtuales | 5 VMs creadas en IsardVDI (HAProxy, HAProxy backup, Web principal, Web backup, SOC) |
| HAProxy principal | Balanceador de carga configurado con HTTPS y redireccion HTTP a HTTPS |
| HAProxy backup | Servidor HAProxy de respaldo configurado y en espera para failover manual |
| Script de failover | Scripts para cambiar IP y activar HAProxy backup automaticamente |
| Servidor web principal | Apache con PHP, HTTPS y pagina de galeria de arte |
| Servidor web backup | Apache con PHP, HTTPS y misma pagina web (replica) |
| Base de datos Master | MySQL en servidor principal con base de datos soc_alerts |
| Base de datos Slave | MySQL en servidor backup con replicacion desde el Master |
| Pagina web completa | Galeria de arte con login, registro y visitas |
| Wazuh Manager | SIEM instalado y configurado en el servidor SOC |
| Agentes Wazuh | Agentes desplegados en las 4 maquinas monitorizadas |
| Filebeat | Configurado para enviar logs a Elasticsearch |
| Elasticsearch | Motor de busqueda y almacenamiento de logs |
| Kibana | Interfaz de visualizacion de logs |
| Script de salud | Script para verificar estado de agentes y servicios |
| Documentacion | Todos los archivos Markdown con la configuracion del proyecto |

---
*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha de la review: 12/05/2026*

- [Index](../Index.md)
