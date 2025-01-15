# Instalación ODOO

Para instalar ODOO lo haremos mediante un docker compose en el cual tendra esta estructura

```
services:
  odoo:
    image: odoo:17
    restart: always
    ports:
      - "8069:8069"
    depends_on:
      - db
    environment:
      - USER=odoo
      - PASSWORD=odoo
    volumes:
      - ./config:/etc/odoo
      - ./extra-addons:/mnt/extra-addons
  db:
    image: postgres:latest
    restart: always
    environment:
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_DB=postgres
    volumes:  
      - local_pgdata:/var/lib/postgresql/data
  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin4_container
    restart: always
    depends_on:
      - db
    ports:
      - "8888:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: vquinoamunoz@danielcastelao.org
      PGADMIN_DEFAULT_PASSWORD: 1234
    volumes:
      - pgadmin_data:/var/lib/pgadmin
volumes:
  local_pgdata:
  pgadmin_data:
```

En este docker instalaremos ODOO, pgadmin y una base de datos en común. Para la persistencia de datos tanto la base de datos como pgadmin contarán con volúmenes.

La imagen de odoo cuenta con la version 17 y puerto 8069. Se establece la dependencia de la base de datos (depends on) y un usuario y contraseña (enviroment). Con restart aseguramos un reinicio automático.

La base de datos contará con la version mas reciente de postgres, cuenta con reinicio igualmente. En el enviroment ponemos la información de usuario y el nombre de la base.

Por último pgadmin cuenta con la imagen pgadmin5 con soporte para administración de bases de datos. Le establecemos un nombre de contenedor (pgadmin4_container), el reinicio , puerto 8888:80 , la dependencia a la base de datos y el enviroment con la información de administrador.


Instalación:

Una vez lanzado el docker e instalado, entramos en odoo mediate la ip y creamos una base de datos, una vez creada iniciaremos sesión y tendremos acceso a odoo en su totalidad.

![](https://github.com/VictorQuinoa/SXE_Odoo/blob/main/Inicio.png?raw=true)

![](https://github.com/VictorQuinoa/SXE_Odoo/blob/main/Odoo.png)

Despues entraremos en pgadmin y con los datos de sesion puestos en el compose, accederemos facilmente y podremos crear un servidor en este caso ServidorOdoo-Victor .

![](https://github.com/VictorQuinoa/SXE_Odoo/blob/main/a.png?raw=true)

La base creada en odoo anteriormente aparecera ahora si desplegamos el servidor, al acceder podremos ver 114 tablas en la base.

![](https://github.com/VictorQuinoa/SXE_Odoo/blob/main/a2.png?raw=true)

# Preguntas

¿Que ocurre si en el ordenador local el puerto 5432 está ocupado? ¿Y si lo estuviese el 8069? ¿Como puedes solucionarlo?

Si los puertos estubiesen en uso se crearian problemas al no poder mapear el puerto debido a que ya esta ocupado, lo que produciria un error al no poder iniciarse el servicio.

Como solución se deberia cambiar el puerto del host asignado a cada servicio para evitar complicaciones.









