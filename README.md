### Descripción del proyecto.
El proyecto consiste en la instalación de un servidor **Mariadb** en una de las máquinas y una combinación de **Nginx**, **Varnish** y **Wordpress** en la otra. En el caso de que se utilice con una única máquina será necesario modificar algunas de las variables, las cuales se detallarán más adelante.

### Descripción de ficheros.
#### actions.yaml
- Ejecuta el rol **commons** a todos los nodos.
- Ejecuta el rol **mariadb** a los nodos del grupo **db**.
- Ejecuta los roles **nginx**, **php**, **varnish** y **wordpress** a los nodos del grupo **web**

#### ansible.cfg
- Añade la lista de máquinas que se van a utilizar, fichero _hosts_.
- Indica el usuario que se va a utilizar, por defecto, **debian**.

#### hosts
Define los grupos y nodos que se van a utilizar, por defecto se definen los grupos **db** y **web**.

### Descripción de variables.
#### group_vars/db
- `iface_mdb`: Indica la interfaz por la que el servidor de base de datos escuchará peticiones.
- `db_mdb`: Indica el nombre de la nueva base de datos.
- `user_mdb`: Indica el nombre del nuevo usuario.
- `pass_mdb`: Indica la contraseña del nuevo usuario.
- `host_mdb`: Indica la dirección IP del nuevo usuario para el acceso remoto.

#### group_vars/web
- `name_nginx`: Indica el **server_name** del nuevo virtualhost.
- `root_nginx`: Indica el **document_root** del nuevo virtualhost.
- `fpm_socket_nginx`: Indica que socket utilizará **Nginx**.
- `fpm_socket_fpm`: Indica que socket utilizará **PHP-FPM**.
- `root_wp`: Indica la ruta de la instalación de Wordpress.
- `db_wp`: Indica la base de datos que utilizará Wordpress.
- `user_wp`: Indica el usuario que utilizará Wordpress para conectarse a la base de datos.
- `pass_wp`: Indica la contraseña del usuario que utilizará Wordpress para conectarse a la base de datos.
- `dbserver_wp`: Indica la dirección IP del servidor de base de datos.


### Descripción de roles.
#### commons/tasks/main.yaml
Actualiza el sistema de las distintas máquinas.

#### mariadb/tasks/main.yaml
- Instala **MariaDB**.
- Configura el acceso remoto [Template](roles/mariadb/templates/etc/mysql/mariadb.conf.d/50-server.cnf).
- Crea la nueva base de datos.
- Crea el nuevo usuario.
- Notifica a los handlers.

#### mariadb/handlers/main.yaml
Reinicia el servicio **mariadb**.

#### nginx/tasks/main.yaml
- Instala **Nginx**
- Configura el nuevo virtualhost [Template](roles/nginx/templates/etc/nginx/conf.d/wordpress.conf).
- Notifica a los handlers.

#### nginx/handlers/main.yaml
Reinicia el servicio **nginx**.

#### php/tasks/main.yaml
- Instala **php7.3**, **php7.3-mysql** y **php7.3-fpm**.
- Configura **PHP-FPM** [Template](roles/php/templates/etc/php/7.3/fpm/pool.d/www.conf)
- Notifica a los handlers.

#### php/handlers/main.yaml
Reinicia el servicio **php7.3-fpm**

#### varnish/tasks/main.yaml
- Instala **Varnish**.
- Configura **Varnish** [File](roles/varnish/files/etc/default/varnish).
- Configura la unidad _systemd_ de **Varnish** para que se inicie en el puerto 80 [File](roles/varnish/files/lib/systemd/system/varnish.service).
- Notifica a los handlers.

#### varnish/handlers/main.yaml
- Reinicia el servicio **varnish**.
- Recarga los demonios del sistema.

#### wordpress/tasks/main.yaml
- Instala unzip.
- Descarga la última versión de wordpress.
- Descomprime el paquete.
- Configura Wordpress [Template](roles/wordpress/templates/var/www/wordpress/wp-config.php)
- Elimina el virtualhost por defecto.
- Notifica a los handlers.

#### wordpress/handlers/main.yaml
Reinicia el servicio **nginx**.