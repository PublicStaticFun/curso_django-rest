# Instalación inicial y configuración del proyecto
Este material corresponde al curso de Django REST Framework (DRF). En esta lección se explica cómo instalar y configurar un proyecto base en Django REST Framework desde cero, incluyendo la organización del entorno, la estructura profesional de carpetas y la creación de un modelo de usuario personalizado.

El objetivo es preparar la base sobre la cual se desarrollará un proyecto de e-commerce completo. Aunque el enfoque inicial no usa todavía las funciones específicas de DRF, esta configuración es crucial para entender cómo estructurar un backend profesional antes de agregar endpoints y serializadores.

## Conceptos principales de esta lección
* **Django REST Framework:**
Es una librería de Django que facilita la creación de APIs RESTful. Se apoya en Django y lo extiende con herramientas para serialización, autenticación, permisos, vistas y más.
* **Entornos virtuales (venv):**
Es buena práctica crear un entorno virtual por proyecto para aislar dependencias. Así cada proyecto mantiene sus versiones de librerías sin conflictos.
* **Estructura profesional del proyecto Django:**
Se proponer dividir la configuración (settings) en varios archivos: `base.py`, `local.py` y `production.py`. Esto permite manejar diferentes configuraciones para desarrollo y despliegue.
* **Organización modular de aplicaciones:**
Se recomienda separar las apps en tres categorías:
    * Base apps: las que vienen por defecto con Django.
    * Local apps: creadas por el propio desarrollador.
    * Third apps: librerias externas como `rest_frameworks` o `django-simple-history`.
* **Modelo de usuario personalizado:** 
Se implementa un modelo de usuario propio, heredando de AbstractBaseUser y PermissionsMixin, para ampliar campos y gestionar mejor la autenticación.
* **Uso de librerías externas:** 
Se instala y configura `django-simple-history`para registrar los cambios historicos en los modelos (útil para auditoría de datos).

## Guía paso a paso para instalar y configurar
1. Crear el entorno virtual:
LO primero es crear un entorno virtual para el proyecto:
`python -m venv django_project`. 
En Linux/Mac: `source django_project/bin/activate`.
2. Instalar Django y Django REST Framework:
Con el entorno virtual activo, instalar ambos paquetes:
`pip install django djangorestframework`.
> [!NOTE] 
> DRF depende directamente de Django, por lo que al instalar `djangorestframework` también se descarga Django automáticamente.
3. Crear el proyecto base:
El autor crea un proyecto llamado e-commerce-rest:
`django-admin startproject ecommerce-rest`.
Luego entra en el directorio del proyecto:
`cd ecommerce_rest`.
4. Reestructurar las configuraciones (settings):
Para un desarrollo profesional, se recomienda separar las configuraciones en varios archivos.
La distribución sera la siguiente:
    * `base.py`: Contiene las configuraciones comunes a todos los entornos.
    * `local.py`: Configuración específica para desarrollo (debug, base de datos local, etc.).
    * `production.py`: Configuración para el entorno de producción (seguridad, logs, etc.).
5. Configurar `INSTALLED_APPS` de manera modular:
Dentro de `base.py`, se recomienda dividir las aplicaciones en tres listas:
![Código de base.py]()
6. Crear la carpeta de aplicaciones:
En el directorio raíz, crear una carpeta para las apps personalizadas.
7. Crear la aplicación de usuarios personalizados:
El siguiente paso es crear la app de usuarios, fundamental para la autenticación:
`python manage.py startapp users`.
8. Se agregan los modelos y configuraciones correspondientes a esa carpeta:
![Código de usuarios.py]()
Este modelo utiliza `django-simple-history` para registrar cambios en la base de datos.
9. Configurar `AUTH_USER_MODEL`:
En `base.py`, se define que el modelo de usuario personalizado será el principal del sistema.
![Código de auth user.py]()
10. Instalar dependencias adicionales:
Antes de crear las migraciones, Django necesita la libreria Pillow para manejar imagenes:
`pip install pillow`, y también se instala el historial de cambios: `pip install django-simple-history`.
11. Aplicar migraciones:
Primero, crear las migraciones iniciales: `python manage.py makemigrations`, y luego aplicarlas: `python manage.py migrate`.
Esto genera las tablas del modelo User y su correspondiente HistoricalUser.
12. Crear un superusuario:
Para acceder al panel de administración de Django: `python manage.py createsuperuser`.
13. Iniciar el servidor de desarrollo:
Ejecutar el proyecto: `python manage.py runserver`. Ir a tu navegador favorito y acceder a: `http://127.0.0.1:8000/admin`.
Iniciar sesión con el superusuario y verificar que aparece la sección Usuarios, donde se pueden ver y editar los registros creados.

## Configuración adicional explicada.
* Estructura del settings:
    * Se elimino el archivo `settings.py` original para usar la versión modular.
    * Los archivos `asgi.py`, `wsgi.py` y `manage.py` fueron actualizados para importar desde `settings.local`
* Internacionalización:
    * Se cambio el idioma a español en las configuraciones.
    * Esto no afecta el código, pero sí los mensajes del panel de administración.
* Importancia del orden en `INSTALLED_APPS`:
    * DRF y otras apps de terceros deben declararse después de las bases.
    * Las apps locales se declaran entre medias, para evitar conflictos con señales y modelos.
* Separación entre desarrollo y producción:
    * `local.py`:
    contiene configuraciones como `DEBUG=TRUE` y `DATABASES` con PostgreSQL.
    * `production.py`:
    Incluirá configuraciones seguras como (`DEBUG=True`, conexión a PostgreSQL, configuración de logs y seguridad).
