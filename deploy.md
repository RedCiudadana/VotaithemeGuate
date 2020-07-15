# Publicar aplicación
Luego de realizar la instalación segun las instrucciones de votainteligente. Podemos proceder a configurar y personalizar nuestra instancia de votainteligente.

## Configurar instancia
En vez de realizar los cambios en `votainteligente/settings.py` podemos realizar los cambios en `votainteligente/local_settings.py` de manera que no sobrescribimos las configuraciones por defecto. 
> `local_settings.py` es cargado automaticamente por django.

## Agregar tema
Votainteligente soporta temas, un tema puede cambiar el aspecto de la plataforma e incluso el funcionamiento en la mayoria de casos solo consiste en cambiar el aspecto. Alguno temas están incluidos en el repositorio original de votainteligente como por ejemplo `rioxinteiro` si ningun tema es seleccionado votainteligente utilizara el tema por defecto `votai_general_theme` que está incluido en la aplicación por defecto.

Para configurar un tema debemos cambiar la variable `THEME` en _settings_.
```python
# votainteligente/local_settings.py
THEME='rioxinteiro'
```

### Temas externos
> Esta no pareciera ser la manera ideal sin embargo es la única manera en la que he logrado hacerlo funcionar, la manera ideal creo que es crear un paquete de python e importarlo en la aplicación.

Para crear nuestro propio tema podemos crear un nuevo repositorio y copiar la estructura de carpetas de `votai_general_theme`
```
theme
├── static
│   ├── css
│   ├── js
│   └── img
└── templates
```
Practicamente cualquier archivo del tema sobrescribira los archivos por defecto (votai_general_theme).

### Servidor web
Mientras `python manage.py runserver`  es muy útil en el desarrollo de la aplicación la instancias de producción debe utilizar otros servidores web.

Los servidores web como Nginx y Apache suele trabajar con archivos estaticos o  contenido dinamico en PHP. Sin embargo no pueden procesar o ejecutar archivos de python.  Para logras publicar nuestra aplicación web con un servidor web con nginx debemos utilizar un programa intermedio como por ejemplo Passenger. [Passenger](https://www.phusionpassenger.com/) puede ejecutar aplicaciones web desarrolladas con python.  

La guía de instalación de Passenger y Nginx está aquí [https://www.phusionpassenger.com/library/walkthroughs/deploy/python/](https://www.phusionpassenger.com/library/walkthroughs/deploy/python/)

Las configuraciones basica del sitio en nginx es la siguiente:
```conf
server {
    server_name example.com www.example.com;

    passenger_app_type wsgi;
    passenger_python /home/user_application/.virtualenvs/vota/bin/python; // path de python, en este caso utlizando virtualenvwrapper.
    passenger_app_env development; // production or development
    passenger_friendly_error_pages on; // off or on
    passenger_startup_file votainteligente/wsgi.py;

    # Tell Nginx and Passenger where your app's 'public' directory is
    root /home/user_application/votainteligente-portal-electoral/static;    

    # Turn on Passenger
    passenger_enabled on;

    # Servir archivos estáticos
    location /static {
        alias /home/user_application/votainteligente-portal-electoral/static;
    }
}
```
El archivo inicial para passenger es `votainteligente/wsgi.py`, si utlizamos virtualenvwrapper debemos hacer unas modificaciones para que cuando passenger reciba una petición active el entorno virtual que creamos:
```python
...
import os
...
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "votainteligente.settings")

# Active virtualenv with virtualenvwrapper. <---- ACTIVE VIRTUALENVWRAPPER
# Editar path al entorno de desarrollo de virtualenvwrapper.
activate_env=os.path.expanduser("~/.virtualenvs/vota/bin/activate_this.py")
execfile(activate_env, dict(__file__=activate_env))
...
```

### HTTPS (SSL)
### OAuth (Autenficación con redes sociales)
### Search engine
### Jobs
### Correos electrónicos 
### Configuraciones generales
### Carga de datos iniciales
