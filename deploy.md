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
Practicamente cualquier archivo del tema sobrescribira los archivos por defector (votai_general_theme).
### Servidor web
### HTTPS (SSL)
### OAuth (Autenficación con redes sociales)
### Search engine
### Jobs
### Correos electrónicos 
### Configuraciones generales
### Carga de datos iniciales
