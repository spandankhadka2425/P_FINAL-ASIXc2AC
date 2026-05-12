Here is the **complete documentation in Spanish** for the MySQL database setup, with simple explanations of each command:

```markdown
# Instalación y Configuración de MySQL para el SOC

## Objetivo

Instalar una base de datos MySQL para almacenar las alertas generadas por Wazuh. Así podremos consultarlas desde nuestro panel HTML personalizado.

---

## Paso 1: Instalar MySQL

### Comando:
```bash
sudo apt install mysql-server -y
```

![MySQL](../Imagen/mysql_instal.png)


## Paso 2: Verificar que MySQL está funcionando

### Comando:
```bash
sudo systemctl status mysql
```

![MySQL](../Imagen/mysql_status.png)



## Paso 3: Entrar a MySQL como root (sin contraseña)

### Comando:
```bash
sudo mysql
```

![MySQL](../Imagen/mysql_root_pass.png)

---

## Paso 4: Cambiar la contraseña del usuario root

### Comando (dentro de MySQL):
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'TuContraseñaAquí';
FLUSH PRIVILEGES;
```
![MySQL](../Imagen/mysql_root_pass.png)

### ¿Qué hace?
- Establece una contraseña para el usuario root
- `FLUSH PRIVILEGES` guarda los cambios

### Ejemplo:
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'SocRoot123!';
FLUSH PRIVILEGES;
```

### ¿Qué puede pasar?
- **Importante:** ¡No olvides esta contraseña!
- Si la pierdes, tendrás que reiniciar MySQL en modo seguro

---

## Paso 5: Salir de MySQL

### Comando:
```sql
exit;
```

### ¿Qué hace?
Cierra la consola de MySQL y vuelve al terminal normal.

---

## Paso 6: Probar la nueva contraseña de root

### Comando:
```bash
mysql -u root -p
```
![MySQL](../Imagen/mysql_test_loggin.png)


### ¿Qué hace?
Intenta entrar a MySQL con el usuario root. Te pedirá la contraseña.

### ¿Qué puede pasar?
- Si la contraseña es correcta, entras a MySQL
- Si es incorrecta, da error `ACCESS DENIED`

---

## Paso 7: Crear la base de datos para las alertas

### Comando:
```bash
mysql -u root -p -e "CREATE DATABASE soc_alerts;"
```
![MySQL](../Imagen/mysql_alerts.png)

### ¿Qué hace?
- `-u root` → usa el usuario root
- `-p` → pide contraseña
- `-e` → ejecuta un comando SQL
- Crea una base de datos llamada `soc_alerts`

### ¿Qué puede pasar?
- Si ya existe, dará error
- Los nombres de bases de datos pueden ser sensibles a mayúsculas

---

## Paso 8: Verificar que la base de datos se creó

### Comando:
```bash
mysql -u root -p -e "SHOW DATABASES;"
```

![MySQL](../Imagen/mysql_verificacion.png)

### ¿Qué hace?
Muestra todas las bases de datos existentes.

### Resultado esperado:
```
soc_alerts          ← debe aparecer aquí
```

---

## Paso 9: Crear la tabla de alertas

### Comando:
```bash
mysql -u root -p -e "
CREATE TABLE soc_alerts.alerts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    source_ip VARCHAR(45),
    machine_name VARCHAR(50),
    alert_type VARCHAR(100),
    severity VARCHAR(20),
    description TEXT,
    wazuh_rule_id INT,
    status VARCHAR(20) DEFAULT 'new'
);
"
```

### ¿Qué hace?
Crea una tabla llamada `alerts` dentro de la base de datos `soc_alerts`.

### Explicación de cada campo:

| Campo | Tipo | ¿Qué guarda? |
|-------|------|---------------|
| `id` | Número | Identificador único de cada alerta (automático) |
| `timestamp` | Fecha/Hora | Cuándo ocurrió el ataque |
| `source_ip` | Texto (45 letras) | IP del atacante |
| `machine_name` | Texto (50 letras) | Qué máquina fue atacada |
| `alert_type` | Texto (100 letras) | Tipo de ataque (escaneo, fuerza bruta, etc.) |
| `severity` | Texto (20 letras) | Gravedad: Crítica, Alta, Media, Baja |
| `description` | Texto largo | Detalles del ataque |
| `wazuh_rule_id` | Número | ID de la regla de Wazuh que detectó el ataque |
| `status` | Texto (20 letras) | Estado: nueva, investigando, resuelta |

---

![MySQL](../Imagen/mysql_alert_table.png)


## Paso 10: Verificar la estructura de la tabla

### Comando:
```bash
mysql -u root -p -e "DESCRIBE soc_alerts.alerts;"
```
![MySQL](../Imagen/mysql_alerts_table_verificacion.png)


### ¿Qué hace?
Muestra todos los campos de la tabla y sus tipos de datos.

---

## Paso 11: Crear un usuario para la aplicación

### Comando:
```bash
mysql -u root -p -e "
CREATE USER 'soc_user'@'localhost' IDENTIFIED BY 'Soc@123';
GRANT INSERT, SELECT, UPDATE ON soc_alerts.* TO 'soc_user'@'localhost';
FLUSH PRIVILEGES;
"
```

![MySQL](../Imagen/mysql_user_soc.png)


### ¿Qué hace?
- `CREATE USER` → Crea un usuario nuevo (no root)
- `IDENTIFIED BY` → Asigna una contraseña
- `GRANT` → Da permisos específicos
- `FLUSH PRIVILEGES` → Guarda los cambios

### Permisos que damos:

| Permiso | ¿Qué permite? |
|---------|----------------|
| `INSERT` | Añadir nuevas alertas |
| `SELECT` | Leer/consultar alertas |
| `UPDATE` | Modificar alertas (ej: cambiar estado) |

### ¿Qué NO permite?
- `DELETE` → No puede borrar alertas
- `DROP` → No puede borrar tablas

### ¿Qué puede pasar?
- El usuario solo puede acceder a `soc_alerts`
- Es más seguro que usar root

---

## Paso 12: Probar el nuevo usuario

### Comando:
```bash
mysql -u soc_user -p -e "SELECT * FROM soc_alerts.alerts;"
```

![MySQL](../Imagen/mysq![MySQL](../Imagen/mysql_root_pass.png)
l_test_soc_user.png)


### ¿Qué hace?
Intenta entrar como `soc_user` y mostrar las alertas.

---

## Resumen de Credenciales

| Campo | Valor |
|-------|-------|
| **Base de datos** | `soc_alerts` |
| **Tabla** | `alerts` |
| **Usuario root** | `root` |
| **Contraseña root** | `pirineus` |
| **Usuario SOC** | `soc_user` |
| **Contraseña SOC** | `Soc@123` |

---

## Comandos Útiles para el Día a Día

| Qué quiero hacer | Comando |
|------------------|---------|
| Entrar a MySQL como root | `mysql -u root -p` |
| Ver todas las bases de datos | `SHOW DATABASES;` |
| Ver todas las tablas | `SHOW TABLES FROM soc_alerts;` |
| Ver todas las alertas | `SELECT * FROM soc_alerts.alerts;` |
| Ver alertas sin formato | `SELECT * FROM soc_alerts.alerts\G` |
| Contar cuántas alertas hay | `SELECT COUNT(*) FROM soc_alerts.alerts;` |
| Salir de MySQL | `exit;` |

---
```markdown
# Configuración de HTTPS en el Servidor Web Principal

## - Creación de la tabla de usuarios en MySQL

**Que estamos haciendo:**
Estamos creando la tabla `users` dentro de la base de datos `soc_alerts` para almacenar los usuarios que se registrarán en nuestra página web. Esta tabla guarda el nombre de usuario, correo electrónico, contraseña (encriptada), fecha de creación, último acceso y si la cuenta está activa.

**Comando utilizado:**
```sql
USE soc_alerts;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login TIMESTAMP NULL,
    is_active BOOLEAN DEFAULT TRUE
);
SHOW TABLES;
DESCRIBE users;
```

---

## Verificación de la estructura de la tabla users

**Que estamos haciendo:**
Verificamos que la tabla `users` se ha creado correctamente y mostramos su estructura con el comando `DESCRIBE users`. Esto nos permite comprobar que todos los campos tienen el tipo de datos correcto y que las restricciones (como clave primaria y valores únicos) están aplicadas.

**Comando utilizado:**
```sql
SHOW TABLES;
DESCRIBE users;
```

---

## Habilitación del módulo SSL en Apache

**Que estamos haciendo:**
Activamos el módulo SSL de Apache con el comando `sudo a2enmod ssl`. Este módulo es necesario para que el servidor web pueda servir contenido cifrado mediante HTTPS. También se habilitan automáticamente los módulos necesarios como `setenvif`, `mime` y `socache_shmcb`.

**Comando utilizado:**
```bash
sudo a2enmod ssl
```

---

## Verificación del certificado SSL y reinicio de Apache

**Que estamos haciendo:**
Verificamos que el certificado SSL generado tiene las propiedades correctas. Comprobamos que `CA:FALSE` (no es un certificado de autoridad), que tiene usos de clave para cifrado y que está destinado a servidores web TLS. Luego reiniciamos Apache para aplicar los cambios.

**Comando utilizado:**
```bash
openssl x509 -in /etc/ssl/artgaleria/artgaleria.crt -text | grep -A5 "X509v3 Basic Constraints"
sudo systemctl restart apache2
```

---

## Configuración de redirección HTTP a HTTPS con .htaccess

**Que estamos haciendo:**
Creamos un archivo `.htaccess` en el directorio `/var/www/html/` para forzar la redirección de todo el tráfico HTTP a HTTPS. Las reglas `RewriteEngine On` activan el motor de reescritura, `RewriteCond` comprueba si la conexión no es HTTPS, y `RewriteRule` redirige todas las peticiones a la versión segura.

**Archivo creado:** `/var/www/html/.htaccess`

**Contenido:**
```apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R=301,L]
```

---

## - Generación del certificado SSL autofirmado

**Que estamos haciendo:**
Generamos un certificado SSL autofirmado para el servidor web. Este certificado permitirá cifrar las conexiones HTTPS. Utilizamos `openssl req -x509` para crear un certificado autofirmado con una validez de 365 días. Especificamos el país (ES), provincia (Barcelona), organización (ITB) y la dirección IP del servidor (192.168.140.2). Además, configuramos las extensiones para que no sea un certificado de autoridad, que tenga usos de clave para cifrado y que sea para autenticación de servidor web.


*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 12/05/2026*

- [Index](../Index.md)
