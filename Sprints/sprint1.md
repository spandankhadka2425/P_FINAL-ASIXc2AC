# Sprint 1: Base del Proyecto - Arquitectura, Documentación y Backlog

Para comenzar nuestro proyecto, decidimos añadir estos tres elementos como base de nuestro proyecto:

- Crear el diagrama de arquitectura
- Preparar la estructura de Git para la documentación
- Desglosar nuestro proyecto en tareas y añadirlas a nuestro backlog

Así es como se ve nuestro ProofHub por ahora.

![ProofHub Backlog - Sprint 1](../Imagen/proofhub_principio.png)

Hoy en dia 21/04/2026

# Sprint 1: Inicio del Proyecto SOC Security

## Fecha de Inicio
21/04/2026

## Objetivo del Sprint

Poner en marcha la base del proyecto SOC Security. Esto incluye crear el entorno de trabajo, definir la arquitectura, instalar los servicios básicos y organizar las tareas en ProofHub.

---

## Estado del Proyecto al Iniciar el Sprint 1

Como se puede ver en la captura de nuestro ProofHub, hemos comenzado a organizar el trabajo con las siguientes tareas:

![ProofHub Sprint 1](../Imagen/proofhub_principio.png)

### Tareas Pendientes (To Do)

| Tarea | Descripción | Fecha |
|-------|-------------|-------|
| **Preparación entorno** | Crear todas las VMs necesarias para el SOC (manager, elastic, db, web, clientes) | 21 Abr |
| **Servidor web** | Instalar Apache HTTP Server, desplegar servidor web y crear página HTML básica | 21 Abr |
| **Base de datos** | Instalar MySQL, crear base de datos `soc_alerts` y tabla de alertas | 21 Abr |
| **Arquitectura (diagrama)** | Definir arquitectura, tecnologías elegidas y justificación | 20 Abr |

### Tareas Asignadas (Assignades)

| Tarea | Asignado a | Estado |
|-------|------------|--------|
| Arquitectura (diagrama) | Ambos | En curso |

### Tareas en Curso (Doing)

| Tarea | Asignado a | Estado |
|-------|------------|--------|
| Preparación entorno | Spandan | En curso |
| Servidor web | Anmolpreet | En curso |


## ¿Qué hemos hecho hasta ahora?

### 1. Preparación del Entorno

- Creamos las máquinas virtuales en IsardVDI
- Configuramos las IPs estáticas y DHCP
- Verificamos la conectividad entre máquinas (ping)

### 2. Servidor Web (Apache)

- Instalamos Apache2 en SRV2
- Configuramos el firewall para abrir el puerto 80
- Creamos una página web temporal (Galería de Arte)
- Verificamos que la página es accesible desde el navegador

### 3. Base de Datos (MySQL)

- Instalamos MySQL server
- Configuramos la contraseña del usuario root
- Creamos la base de datos `soc_alerts`
- Creamos la tabla `alerts` con los campos necesarios
- Creamos el usuario `soc_user` con permisos limitados
- Probamos la conexión y la inserción de alertas

### 4. Arquitectura y Documentación

- Definimos el diagrama de red del proyecto
- Decidimos usar IsardVDI como plataforma (comparativa vs AWS)
- Decidimos usar Apache como servidor web (comparativa vs Nginx)
- Creamos la estructura de carpetas en Git
- Documentamos todos los pasos en Markdown

---

## Resumen de Tareas Completadas en Sprint 1

| Tarea | Estado | Responsable |
|-------|--------|-------------|
| Crear VMs en IsardVDI |Completado | Spandan |
| Configurar red de las VMs |Completado | Spandan |
| Instalar Apache2 |Completado | Anmolpreet |
| Crear página web HTML |Completado | Anmolpreet |
| Instalar MySQL |Completado | Anmolpreet |
| Crear base de datos y tabla |Completado | Anmolpreet |
| Crear usuario y permisos |Completado | Anmolpreet |
| Diagrama de arquitectura |Completado | Ambos |
| Comparativa de software |Completado | Ambos |
| Estructura de documentación |Completado | Ambos |

---

## Próximos Pasos (Sprint 2)

Ahora que tenemos la base, en el Sprint 2 empezaremos con:

1. **Instalar Elasticsearch y Kibana** en el servidor SOC
2. **Instalar Wazuh Manager e indexer**
3. **Desplegar agentes de Wazuh** en las máquinas monitorizadas
4. **Configurar Filebeat** para enviar logs a Elasticsearch
5. **Verificar que los logs llegan a Kibana**

---

## Conclusión del Sprint 1

Hemos completado con éxito la primera fase del proyecto. Tenemos:

- Infraestructura base funcionando
- Servidor web con página accesible
- Base de datos lista para recibir alertas
- Documentación organizada
- Backlog en ProofHub actualizado

El proyecto está listo para continuar con la implementación del SOC en el Sprint 2.

---

*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 21/04/2026*


- [Index](../Index.md)
