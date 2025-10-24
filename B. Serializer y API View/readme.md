# Serializers y API View
La lección pertenece al curso práctico sobre Django REST Framework (DRF), en el cual es parte de un proyecto Django ya configurado y funcional. El punto de partida es la aplicación **"usuario"**, que contiene un **modelo de usuario personalizado**.
El objetivo de la lección es construir, desde esta base, la primera API funcional usando serializadores y vistas tipo API View, los dos pilares fundamentales para manejar datos en formato JSON dentro de un entorno Django REST.
## ¿Que es un Serializer en DRF?
El **serializador (serializer)** es un componente esencial en DRF. Su función es:
[!NOTE]
> Tomar la estructura de un modelo y convertirla a un formato JSON.
En otras palabras, actúa como un traductor entre los modelos de Django (objetos de Python) y las estructuras de datos que puede entender y enviar una API (por ejemplo, JSON).
* Los modelos de Django trabajan con objetos.
* Las APIs REST comunican información mediante JSON.
* El serializer hace la conversión entre ambos mundos.
**IMPORTANCIA**: Aunque su tarea parece simple, el serializer es fundamental para crear APIs REST, ya que permite que la información de la base de datos se transmita y se reciba de forma estructurada, segura y legible.
## Conceptos generales
* **Serialización**: proceso de convertir un objeto Python o QuerySet en JSON, y se hace mediante `ModelSerializer`.
* **APIView**: clase que reemplaza las vistas clásicas de Django. Permite definir métodos HTTP, y controla la lógica de negocio y las respuestas de la API.
* **Response**: objeto que devuelve datos formateados en JSON, y simplifica el envío de códigos de estado y mensajes.
* **Rutas REST**: cada endpoint debe estar conectado mediante `path()` o `re_path()`, y se pueden agrupar usando `include()` para modularidad.
* **Interfaz DRF**: herramienta visual que facilita probar los endpoints, y no forma parte de la API real.
## Estructura del proyecto para la API
Para mantener el código organizado, el instructor sugiere una estructura modular dentro de cada aplicación Django. Dentro de la carpeta de la app `usuario`, se crea una subcarpeta llamada `api/`.
Dentro de `api/`, se colocan tres archivos básicos:
1. `serializers.py`: define los serializadores (uno por modelo)
1. `api.py`: define las vistas  (APIView, etc)
1. `urls.py`: define las rutas (endpoints)

Esta estructura ayuda a mantener el código limpio y escalable. Además, se pueden eliminar archivos no esenciales al inicio (`views.py`, `tests.py`) si no se usan.

## Creación del primer Serializer
### Crear `serializers.py`

Dentro de `apps/usuario/api/`, se crea el archivo:

### Importar dependencias:

En el archivo se importan las clases necesarias.

### Definir el serializador:

La convención general es usar el nombre del modelo + la palabra **"Serializer"**, y heredar de `serializers.ModelSerializer`.

![Imagen del serializador](https://github.com/PublicStaticFun/curso_django-rest/blob/main/B.%20Serializer%20y%20API%20View/Imagenes/Imagen2A.png?raw=true)

La explicación es esta:

* `ModelSerializer`: versión simplificada de `Serializer` que trabaja directamente con modelos Django.
* `model = Usuario`: indica el modelo al que hace referencia.
* `fields = '__all__'`: incluye todos los campos del modelo (también se pueden especificar manualmente).

### Creación de vista (APIView)
La siguiente parte consiste en crear la lógica que respondera a las peticiones HTTP.

Dentro de `apps/usuario/api/`, se crea e importar lo necesario. Se define una clase que hereda de APIView.

![Imagen del código 2](https://github.com/PublicStaticFun/curso_django-rest/blob/main/B.%20Serializer%20y%20API%20View/Imagenes/Imagen2B.png?raw=true)

La explicación paso a paso es esta:

* `APIView` reemplaza la clase `View` tradicional de DJango. Se usa cuando se quiere manejar protocolos HTTP.
* Se define un método `get()`, que responde a solciitudes GET.
* Se obtiene una lista de todos los usuarios con `Usuario.objects.all()`.
* Se serializan los resultados:

    * `UsuarioSerializer(usuarios, many=True)` convierte un conjunto de objetos en JSON.
    * El parámetro `many=True` indica que hay más de una instancia.

* Finalmente, se retorna la respuesta con `Response(data)`.

El objeto `Response` convierte automáticamente el resultado a JSON y gestiona el código de estado HTTP (200 OK).

### Configurar las rutas (URLs)
Para que la API sea accesible, se define una ruta. Crear `urls.py` dentro de la carpeta `api/`. 

![Imagen del código 3](https://github.com/PublicStaticFun/curso_django-rest/blob/main/B.%20Serializer%20y%20API%20View/Imagenes/Imagen2C.png?raw=true)

En el archivo principal `urls.py` del proyecto:

![Imagen del código 4](https://github.com/PublicStaticFun/curso_django-rest/blob/main/B.%20Serializer%20y%20API%20View/Imagenes/Imagen2D.png?raw=true)

### Prueba de API
Al ejecutar el servidor con: `python manage.py runserver`
y acceder a `http://localhost:8000/usuario/`, Django REST Framework mostrará una interfaz de desarrollo muy útil.

![Imagen del código 5](https://github.com/PublicStaticFun/curso_django-rest/blob/main/B.%20Serializer%20y%20API%20View/Imagenes/Imagen2E.png?raw=true)

Esta interfaz incluye:

* La ruta actual
* El método HTTP permitido (GET)
* El código de estado (200 OK)
* El tipo de contenido (application/json)
* El resultado serializado.

## Detalles técnicos importantes
* Uso de `Response`:

`Response` viene de `rest_framework.response` y es el equivalente de `HttpResponse`, pero adaptado a JSON y a la lógica REST.

Puede aceptar:

    * Datos a devolver (JSON, listas, diccionarios).
    * Código de estado (status = 200, status = 400, etc).
    * Encabezados HTTP adicionales.

* Estado HTTP:

Si no se especifica, `Response` devuelve por defecto **200 OK**, lo que indica éxito.

* Serialización múltiple:

Cuando se serializa una consulta múltiple, se debe agregar `many=True` para que el serializador entienda que debe iterar sobre los resultados. Sin este parámetro, DRF lanzará un error de tipo, pues pensará que se trata de una sola instancia.
