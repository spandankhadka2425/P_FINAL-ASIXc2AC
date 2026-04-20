# Análisis Comparativo: IsardVDI vs. AWS para el Proyecto SOC Security

## Resumen

Para nuestro proyecto escolar "SOC Security" (monitorizar una web con Elastic Stack + Wazuh), **IsardVDI es la mejor opción** frente a AWS. Es gratis, fácil de usar y está hecho para educación.

---

## Comparación Rápida

| Criterio | IsardVDI | AWS | Ganador |
|----------|----------|-----|---------|
| **Coste** | Gratis | Pago por uso (caro para estudiantes) | IsardVDI |
| **Propósito** | Hecho para educación | Hecho para empresas | IsardVDI |
| **Aprendizaje** | Aprendes administración de sistemas | Aprendes consola AWS | IsardVDI |
| **Control** | Control total de las máquinas | Servicios gestionados esconden cosas | IsardVDI |
| **Plantillas** | Clonar máquinas fácil | También hay pero más complejo | IsardVDI |
| **Colaboración** | Acceso web fácil para todo el equipo | Configuración complicada | IsardVDI |

---

## ¿Por qué IsardVDI es mejor para nuestro proyecto?

| Característica | Cómo nos ayuda |
|----------------|----------------|
| **Gratis** | Sin riesgo financiero |
| **Gestión central** | Todas las máquinas desde un solo sitio |
| **Clonar máquinas** | Creamos 4 agentes fácilmente |
| **Acceso navegador** | Trabajamos desde cualquier PC |
| **Redes aisladas** | Simulamos un SOC real |

---


---

## Decisión Final

**Usamos IsardVDI porque:**

- Es **gratis** – sin riesgo económico
- Está **hecho para educación** – plantillas, laboratorios aislados, acceso web
- Enseña **habilidades reales** – Linux, Elasticsearch, Wazuh, MariaDB
- **Todo el equipo puede acceder** fácilmente
- Funciona en **PCs modestos** del instituto
- Los **profesores conocen la plataforma** y pueden ayudar

AWS es potente para empresas, pero para nuestro proyecto académico es demasiado caro y complicado.

---

## Conclusión

**Usamos IsardVDI.** Es una plataforma VDI libre pensada para educación, permite gestionar todas las máquinas virtuales desde un punto central, escala fácilmente, funciona bien en PCs modestos, se accede por navegador y simplifica plantillas, snapshots y laboratorios reproducibles para todo el equipo.


# Análisis Comparativo: Apache vs Nginx para el Proyecto SOC Security

## Resumen

Para nuestro proyecto "SOC Security" (monitorizar una web con Wazuh), **Apache es la mejor opción** frente a Nginx. Es más fácil de configurar, tiene mejor integración con Wazuh y sus logs son más claros para detectar ataques.

---

## Comparación Rápida

| Criterio | Apache | Nginx | Ganador |
|----------|--------|-------|---------|
| **Logs** | Claros y fáciles de leer | Más compactos y complejos | Apache |
| **Integración con Wazuh** | Reglas predefinidas | Requiere configuración extra | Apache |
| **Facilidad** | Fácil de instalar y configurar | Curva de aprendizaje más alta | Apache |
| **Documentación** | Muy extensa y comunidad grande | Más técnica y especializada | Apache |
| **Rendimiento** | Suficiente para nuestro proyecto | Excelente pero no lo necesitamos | Apache |
| **Ver ataques** | Logs muy claros (404, SQLi) | Logs más difíciles de interpretar | Apache |

---

## ¿Por qué Apache es mejor para nuestro proyecto?

| Característica | Cómo nos ayuda |
|----------------|----------------|
| **Logs claros** | Vemos fácilmente quién ataca y qué hace |
| **Reglas Wazuh** | Wazuh ya tiene reglas hechas para Apache |
| **Fácil de instalar** | `sudo apt install apache2` y listo |
| **.htaccess** | Podemos probar reglas de seguridad fácilmente |
| **Documentación** | Cualquier duda la resolvemos rápido |

---

## Decisión Final

**Usamos Apache porque:**

- Es **más fácil** – instalar y configurar en minutos
- Tiene **mejores logs** – ideales para nuestro SOC
- Wazuh **integra perfectamente** – reglas ya listas
- Podemos **ver ataques** claramente (nmap, hydra, SQLi)
- La **documentación es enorme** – cualquier problema se soluciona rápido

Nginx es más eficiente para alto tráfico, pero para nuestro proyecto académico Apache es más sencillo y didáctico.

---

## Conclusión

**Usamos Apache.** Es el servidor web perfecto para aprender SOC porque sus logs son muy claros, la integración con Wazuh es inmediata y podemos centrarnos en monitorizar ataques en lugar de complicarnos con la configuración del servidor.