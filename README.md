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
- `db_wp`: Indica la base de datos que utilizará Wordpress.
- `user_wp`: Indica el usuario que utilizará Wordpress para conectarse a la base de datos.
- `pass_wp`: Indica la contraseña del usuario que utilizará Wordpress para conectarse a la base de datos.
- `dbserver_wp`: Indica la dirección IP del servidor de base de datos.

### Descripción de roles.
