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
## Estructura del proyecto para la API
Para mantener el código organizado, el instructor sugiere una estructura modular dentro de cada aplicación Django. Dentro de la carpeta de la app `usuario`, se crea una subcarpeta llamada `api/`.
Dentro de `api/`, se colocan tres archivos básicos:
1. `serializers.py`: define los serializadores (uno por modelo)
1. `api.py`: define las vistas  (APIView, etc)
1. `urls.py`: define las rutas (endpoints)

Esta estructura ayuda a mantener el código limpio y escalable. Además, se pueden eliminar archivos no esenciales al inicio (`views.py`, `tests.py`) si no se usan.

## Creación del primer Serializer
1. Crear `serializers.py`

Dentro de `apps/usuario/api/`, se crea el archivo:

2. Importar dependencias:

En el archivo se importan las clases necesarias.

3. Definir el serializador:

La convención general es usar el nombre del modelo + la palabra **"Serializer"**, y heredar de `serializers.ModelSerializer`.

[!Imagen del serializador]()
