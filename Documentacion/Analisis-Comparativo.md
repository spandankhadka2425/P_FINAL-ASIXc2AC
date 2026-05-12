
# Análisis Comparativo: Nuestro SOC vs. Centros de Seguridad Existentes

## Lo que Investigamos

Antes de construir nuestro propio SOC, investigamos cómo funcionan los centros de operaciones de seguridad existentes. Encontramos dos tipos principales: los SOC tradicionales internos y los proveedores comerciales de SOC como servicio.

---

## ¿Qué es un SOC Tradicional Interno?

Un SOC interno es un equipo de analistas de seguridad que trabajan dentro de una empresa. Vigilan los sistemas de la empresa las 24 horas del día, los 7 días de la semana.

**Cómo funciona:**
- La empresa contrata su propio personal de seguridad
- Ellos construyen su propia infraestructura de monitorización
- Ellos crean sus propias reglas de detección y planes de respuesta

**Por qué es caro:**
Una empresa necesita al menos 4 o 5 analistas para cubrir turnos de 24/7. Cada analista cuesta alrededor de 100.000 dólares al año. Eso significa más de 500.000 dólares solo en salarios. Sumando software, hardware y formación, el coste total supera 1,5 millones de dólares al año.

---

## ¿Qué es un SOC como Servicio (SOCaaS)?

SOCaaS es cuando una empresa paga a un proveedor externo para que vigile su seguridad. El proveedor se encarga de todo: tecnología, analistas y respuesta a incidentes.

**Cómo funciona:**
- La empresa paga una suscripción mensual
- Los analistas del proveedor vigilan los sistemas de la empresa de forma remota
- Se envían alertas cuando se detecta actividad sospechosa

**Por qué las empresas lo eligen:**
- No necesitan contratar personal de seguridad
- Es más rápido de configurar (semanas en lugar de meses)
- Menores costes iniciales

**Lo que no les gusta a las empresas:**
- Menos control sobre sus propios datos
- Difícil cambiar de proveedor una vez que empiezan
- Personalización limitada para necesidades específicas

---

## Nuestra Arquitectura SOC

Antes de explicar las diferencias, recordemos cómo está construido nuestro proyecto:

![Página Web Apache](../Imagen/Diagrama_red.png)


**Componentes de nuestro proyecto:**
- **Firewall perimetral:** Filtra quién puede entrar y quién no
- **HAProxy principal:** Balanceador de carga activo (192.168.140.4)
- **HAProxy backup:** Balanceador de carga de respaldo (sin IP, se activa manualmente)
- **SRV2_A:** Servidor web principal con Apache y base de datos
- **SRV2_B:** Servidor web backup con Apache y base de datos (réplica)
- **SRV1:** Servidor SOC con Wazuh Manager, Elasticsearch y Kibana

**Cómo funciona el failover:**
- Si SRV2_A (web principal) falla, HAProxy redirige automáticamente a SRV2_B (web backup)
- Si HAProxy principal falla, podemos añadir manualmente su IP al HAProxy backup para que tome el control

---

## En qué se Diferencia Nuestro SOC

### 1. Lo Construimos para Aprender, No Solo para Vigilar

La mayoría de los SOC están diseñados para ser lo más automatizados posible. No ves lo que pasa detrás. Nuestro SOC es completamente transparente. Puedes ver cada archivo de configuración, cada regla de detección y cada registro.

### 2. Usamos Herramientas Reales de la Industria

No usamos herramientas falsas ni simplificadas. Usamos:
- **HAProxy** para balanceo de carga (usado por empresas como Reddit y GitHub)
- **Wazuh** para monitorización de seguridad (usado por miles de organizaciones)
- **Elastic Stack** para gestión de registros (usado por Netflix, Uber y Microsoft)

### 3. Construimos Todo con Redundancia

La mayoría de los proyectos de estudiantes ejecutan todo en un solo servidor. Nosotros construimos:
- Dos servidores web (uno activo, uno de respaldo)
- Dos servidores HAProxy (uno activo, uno de respaldo con failover manual)
- Replicación MySQL (los datos se copian automáticamente a la base de datos de respaldo)
- Firewall perimetral para controlar el acceso

Si un servidor falla, los otros siguen funcionando.

### 4. Monitorizamos una Aplicación Web Real

Muchos SOC solo monitorizan registros genéricos del sistema. Nuestro SOC monitoriza una galería de arte real con inicio de sesión,imagenes y registro de usuarios. Esto nos da escenarios de ataque realistas para probar nuestras reglas de detección.

---

## Cuándo Deberías Usar Nuestro SOC

### Perfecto para Aprender

Nuestro SOC es ideal para:
- Estudiantes de ciberseguridad que quieren experiencia práctica
- Profesionales de TI que aprenden monitorización de seguridad
- Escuelas que enseñan operaciones de seguridad

### Bueno para Pruebas Pequeñas

Nuestro SOC puede funcionar como:
- Una prueba de concepto antes de comprar soluciones comerciales caras
- Un entorno de pruebas para reglas de seguridad
- Una demostración para interesados

### No es Adecuado Para

Nuestro SOC NO está listo para:
- Grandes empresas con miles de empleados
- Organizaciones que necesitan monitorización 24/7 (no tenemos analistas trabajando toda la noche)
- Industrias reguladas como bancos u hospitales (no tenemos certificaciones de cumplimiento)

---

## Qué Hace Único a Nuestro SOC

| Característica | SOC Comerciales | Nuestro SOC |
|----------------|-----------------|-------------|
| Coste | 30.000 - 200.000+ dólares al año | Coste del hardware del laboratorio |
| Transparencia | Caja negra (no ves el interior) | Completamente abierto (cada archivo está documentado) |
| Valor de aprendizaje | Bajo (diseñado para operaciones) | Alto (diseñado para entender) |
| Personalización | Limitada a las opciones del proveedor | Control total sobre todo |
| Failover | Automático (pero oculto) | Manual (aprendes cómo funciona) |
| Firewall | Generalmente incluido pero opaco | Configurado y documentado |
| Redundancia | Depende del contrato | Completa (web, DB, HAProxy) |

---

## La Conclusión Final

**Nuestro SOC no intenta reemplazar los productos comerciales.** Sirve para un propósito diferente.

Si necesitas monitorizar una gran empresa con requisitos de cumplimiento, compra un SOC comercial.

Si quieres aprender cómo funciona realmente un SOC, construye nuestro proyecto.

Si eres una pequeña organización que prueba la monitorización de seguridad, nuestro proyecto puede servir como prueba de concepto antes de invertir en soluciones caras.

---

*Investigación realizada como parte del Proyecto SOC Security*
*ASIXcAC - Proyecto Final*
*Fecha: 12/05/2026*

- [Index](../Index.md)
