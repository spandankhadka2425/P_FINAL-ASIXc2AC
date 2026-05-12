```markdown
# Configuración del Servidor Web Backup (SRV2B)

## Instalación de Apache2

**Donde se ejecuta:** Servidor Backup (SRV2B - 192.168.140.6)

**Comando:**
```bash
sudo apt install apache2 -y
```

**Que estamos haciendo:**
Instalando el servidor web Apache2 en el servidor backup (SRV2B). Este servidor actuara como respaldo del servidor web principal. Si el servidor principal falla, HAProxy redirigira el trafico a este servidor backup.

![Apache Backup](../Imagen/mysql_b_install.png)

---

## Instalación de MySQL Server

**Donde se ejecuta:** Servidor Backup (SRV2B - 192.168.140.6)

**Comando:**
```bash
sudo apt install mysql-server -y
```

**Que estamos haciendo:**
Instalando MySQL Server en el servidor backup. La base de datos se replicara desde el servidor principal para que ambos servidores tengan los mismos datos y puedan funcionar de forma intercambiable.

![MySQL Backup](../Imagen/mysql_b_install_2.png)

---

## Compresion de la pagina web

**Donde se ejecuta:** Servidor Principal (SRV2 - 192.168.140.2)

**Comando:**
```bash
cd /var/www/html
sudo tar -czf /tmp/galeria-web.tar.gz .
```

**Que estamos haciendo:**
Comprimiendo toda la pagina web (Galería de Arte) que se encuentra en el directorio `/var/www/html/` del servidor principal (SRV2). El archivo comprimido se guarda en `/tmp/galeria-web.tar.gz` para poder copiarlo facilmente al servidor backup.

![Compresion Web](../Imagen/mysql_b_tar.png)

---

## Cambio de permisos del archivo comprimido

**Donde se ejecuta:** Servidor Principal (SRV2 - 192.168.140.2)

**Comando:**
```bash
sudo chmod 644 /tmp/galeria-web.tar.gz
```

**Que estamos haciendo:**
Cambiando los permisos del archivo comprimido a 644 (lectura y escritura para el propietario, solo lectura para los demas). Esto permite que el archivo pueda ser enviado a otro servidor mediante SCP sin problemas de permisos.

![Permisos Archivo](../Imagen/mysql_b_checking.png)

---

## Envio del archivo al servidor backup

**Donde se ejecuta:** Servidor Principal (SRV2 - 192.168.140.2)

**Comando:**
```bash
scp /tmp/galeria-web.tar.gz isard@192.168.140.6:/tmp/
```

**Que estamos haciendo:**
Enviando el archivo comprimido `galeria-web.tar.gz` desde el servidor principal (SRV2) al servidor backup (SRV2B con IP 192.168.140.6). El archivo se guarda en el directorio `/tmp/` del servidor backup.

![Envio SCP](../Imagen/mysql_b_unzip.png)

---

## Verificacion de la llegada del archivo

**Donde se ejecuta:** Servidor Backup (SRV2B - 192.168.140.6)

**Comando:**
```bash
ls -lh /tmp/galeria-web.tar.gz
```

**Que estamos haciendo:**
Verificando que el archivo comprimido ha llegado correctamente al servidor backup. Comprobamos que el tamaño es de 80KB y que los permisos son correctos (rw-r--r--).

![Verificacion Archivo](../Imagen/mysql_b_isard.png)

---

## Descompresion de la pagina web en el backup

**Donde se ejecuta:** Servidor Backup (SRV2B - 192.168.140.6)

**Comandos:**
```bash
sudo tar -xzf /tmp/galeria-web.tar.gz -C /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
sudo systemctl restart apache2
curl http://localhost
```

**Que estamos haciendo:**
Descomprimiendo la pagina web en el directorio `/var/www/html/` del servidor backup. Luego cambiamos el propietario de los archivos a `www-data` y reiniciamos Apache. Finalmente verificamos con `curl http://localhost` que la pagina funciona correctamente en el servidor backup.

![Descompresion Backup](../Imagen/mysql_b_permisos.png)


*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 04/05/2026*


```markdown
# Configuracion de Replicacion MySQL Master-Slave

## Explicacion General

El objetivo de estos pasos es crear una replica exacta de la base de datos del servidor principal (Master) en el servidor backup (Slave). De esta forma, cualquier cambio que hagamos en la base de datos principal se replica automaticamente en el backup. Si el servidor principal falla, el backup puede seguir funcionando sin perder datos.

---

## Exportacion de todas las bases de datos desde el Master

**Donde se ejecuta:** Servidor Principal (SRV2_A - 192.168.140.2)

**Que estamos haciendo:**
Exportamos todas las bases de datos del servidor principal, incluyendo la estructura, los datos, los procedimientos almacenados, triggers y eventos. El archivo generado se guarda en `/tmp/full_mysql_backup.sql`. Este archivo se utilizara para importar los datos en el servidor backup.

**Comando utilizado:**
```bash
mysqldump -u root -p --all-databases --source-data --routines --triggers --events > /tmp/full_mysql_backup.sql
```

**Resultado:**
Archivo generado de 1.3MB que contiene toda la base de datos del Master.

---

## Verificacion del log binario en el Master

**Donde se ejecuta:** Servidor Principal (SRV2_A - 192.168.140.2)

**Que estamos haciendo:**
Verificamos que el log binario esta activado en el servidor principal. El log binario es necesario para la replicacion porque registra todos los cambios que se hacen en la base de datos (INSERT, UPDATE, DELETE). El Master envia estos cambios al Slave.

**Comando utilizado:**
```bash
mysql -u root -p -e "SHOW VARIABLES LIKE 'log_bin';"
```

**Resultado:**
La variable `log_bin` muestra `ON`, lo que indica que la replicacion esta habilitada.

---

## Obtencion de la posicion actual del Master

**Donde se ejecuta:** Servidor Principal (SRV2_A - 192.168.140.2)

**Que estamos haciendo:**
Obtenemos la posicion actual del archivo de log binario en el Master. Esta informacion (archivo y posicion) es fundamental para que el Slave sepa desde donde empezar a leer los cambios. En este caso, el archivo es `binlog.000024` y la posicion es `157`.

**Comando utilizado:**
```bash
mysql -u root -p -e "SHOW MASTER STATUS;"
```

**Resultado:**
```
File: binlog.000024
Position: 157
```

---

## Verificacion del usuario replicador en el Master

**Donde se ejecuta:** Servidor Principal (SRV2_A - 192.168.140.2)

**Que estamos haciendo:**
Verificamos que el usuario `replicator` existe en el Master. Este usuario es el que utilizara el Slave para conectarse al Master y solicitar los cambios. El usuario tiene permisos especificos de replicacion.

**Comando utilizado:**
```bash
mysql -u root -p -e "SELECT User, Host FROM mysql.user WHERE User='replicator';"
```

**Resultado:**
El usuario `replicator` existe y puede conectarse desde cualquier host (%).

---

## Insercion de un usuario de prueba en el Master

**Donde se ejecuta:** Servidor Principal (SRV2_A - 192.168.140.2)

**Que estamos haciendo:**
Insertamos un nuevo usuario en la tabla `users` de la base de datos `soc_alerts`. Esta operacion se registrara en el log binario y posteriormente se replicara en el Slave para verificar que la replicacion funciona correctamente.

**Comando utilizado:**
```bash
mysql -u root -p -e "USE soc_alerts; INSERT INTO users (username, email, password_hash) VALUES ('nuevo_user', 'nuevo@test.com', 'hash_nuevo123');"
```

---

## Configuracion del Slave para conectarse al Master

**Donde se ejecuta:** Servidor Backup (SRV2_B - 192.168.140.7)

**Que estamos haciendo:**
Configuramos el servidor Slave para que se conecte al Master. Le indicamos la direccion IP del Master, el usuario de replicacion, la contraseña, y el archivo y posicion del log binario desde donde debe empezar a leer los cambios.

**Comando utilizado:**
```bash
mysql -u root -p -e "CHANGE MASTER TO
    MASTER_HOST = '192.168.140.2',
    MASTER_USER = 'replicator',
    MASTER_PASSWORD = 'Replica123',
    MASTER_LOG_FILE = 'binlog.000024',
    MASTER_LOG_POS = 157;"
```

**Importante:** Esta configuracion se hace en el Slave, no en el Master, porque es el Slave quien inicia la conexion hacia el Master.

---

## Verificacion del estado de la replicacion en el Slave

**Donde se ejecuta:** Servidor Backup (SRV2_B - 192.168.140.7)

**Que estamos haciendo:**
Comprobamos que la replicacion esta funcionando correctamente en el Slave. `Slave_IO_Running: Yes` indica que el Slave puede leer los cambios del Master. `Slave_SQL_Running: Yes` indica que el Slave puede aplicar esos cambios en su propia base de datos.

**Comando utilizado:**
```bash
mysql -u root -p -e "SHOW SLAVE STATUS\G" | grep -E "Slave_IO_Running|Slave_SQL_Running|Last_Io_Error"
```

**Resultado:**
Ambas variables muestran `Yes`, lo que confirma que la replicacion esta activa y funcionando.

---

---

## Por que hacemos cada paso en cada servidor

| Servidor | Motivo |
|----------|--------|
| Master | Contiene los datos originales. Desde aqui exportamos la base de datos y configuramos el log binario para registrar los cambios. |
| Slave | Recibe la copia de los datos y se conecta al Master para solicitar los cambios en tiempo real. |

---

*Documentado por: Anmolpreet Singh Kaur & Spandan Khadka*
*Fecha: 12/05/2026*


- [Index](../Index.md)
