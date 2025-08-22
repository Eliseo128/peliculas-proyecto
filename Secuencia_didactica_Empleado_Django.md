Claro, aquí tienes una secuencia didáctica completa basada en el tutorial anterior, diseñada para un curso de 15 horas.

---

### **Secuencia Didáctica: Desarrollo de Aplicaciones Web con Python y Django**

**Nivel:** Principiante
**Duración Total:** 15 horas
**Modalidad:** Teórico-Práctico con actividades demostrativas y autónomas.

---

### **Momento 1: Apertura (2 Horas)**

#### **Objetivo:**
Introducir a los estudiantes en los conceptos fundamentales del desarrollo web, la arquitectura de Django y preparar el entorno de trabajo para el curso.

*   **Actividad 1: Introducción Teórica al Desarrollo Web (1 hora - Práctica Demostrativa)**
    *   **El Rol del Instructor:**
        *   Presentar qué es el desarrollo web backend y frontend.
        *   Explicar el modelo cliente-servidor y el ciclo de una petición web (request-response).
        *   Definir qué es un framework y por qué Django es una excelente opción.
        *   Describir la arquitectura MVT (Modelo-Vista-Plantilla) de Django y compararla con la más conocida MVC.
        *   Realizar una sesión de preguntas y respuestas para aclarar dudas iniciales.

*   **Actividad 2: Preparación del Entorno de Desarrollo (1 hora - Práctica Autónoma Guiada)**
    *   **El Rol del Estudiante (con supervisión del instructor):**
        *   Seguir los pasos del 1 al 8 del tutorial anterior:
            1.  Crear las carpetas de trabajo (`Curso_Django`, `practica_1`).
            2.  Abrir VS Code en el directorio correcto.
            3.  Verificar las versiones de Python y pip en la terminal.
            4.  Crear el entorno virtual `.venv`.
            5.  Activar el entorno virtual.
            6.  Seleccionar el intérprete de Python correcto en VS Code.
            7.  Instalar Django usando pip y verificar su versión.
    *   **Recursos:** Tutorial paso a paso, asistencia del instructor.

---

### **Momento 2: Desarrollo (10 Horas)**

#### **Objetivo:**
Construir una aplicación web CRUD (Crear, Leer, Actualizar, Borrar) completa, aplicando los conceptos de modelos, vistas, plantillas y URLs en Django.

*   **Actividad 3: Creación del Proyecto y la Primera App (2 horas - Práctica Demostrativa y Autónoma)**
    *   **Instructor (Demostrativo):**
        *   Explica la diferencia entre un "proyecto" y una "app" en Django.
        *   Ejecuta los comandos `django-admin startproject` y `python manage.py startapp`.
        *   Describe la estructura de archivos y la función de `manage.py`, `settings.py` y `urls.py`.
        *   Ejecuta el servidor de desarrollo (`runserver`) y muestra la página de bienvenida.
        *   Crea un superusuario y navega por el panel de administración.
    *   **Estudiante (Autónomo):**
        *   Replica los pasos del 9 al 24 del tutorial.
        *   Crea el proyecto `backend_empleado`.
        *   Ejecuta el servidor y lo detiene.
        *   Realiza la primera migración (`migrate`).
        *   Crea su propio superusuario.
        *   Crea la aplicación `app_empleado`.
        *   Configura las URLs y crea la primera vista con `HttpResponse`.

*   **Actividad 4: Modelos y Base de Datos (3 horas - Práctica Demostrativa y Autónoma)**
    *   **Instructor (Demostrativo):**
        *   Explica qué es un ORM (Object-Relational Mapper) y cómo facilita la interacción con la base de datos.
        *   Define un modelo simple en `models.py` (`Empleado`), explicando los tipos de campo (`CharField`, `IntegerField`).
        *   Muestra cómo registrar el modelo en `admin.py`.
        *   Ejecuta los comandos `makemigrations` y `migrate`, explicando qué hace cada uno.
        *   Usa "DB Browser for SQLite" para mostrar visualmente la tabla creada en el archivo `db.sqlite3`.
    *   **Estudiante (Autónomo):**
        *   Sigue los pasos del 25 al 31 del tutorial.
        *   Crea el modelo `Empleado`.
        *   Registra el modelo en el panel de administración.
        *   Genera y aplica las migraciones.
        *   Accede al panel de administración para agregar, editar y eliminar empleados directamente desde la interfaz de Django.

*   **Actividad 5: Vistas y Plantillas - Mostrando Datos (3 horas - Práctica Demostrativa y Autónoma)**
    *   **Instructor (Demostrativo):**
        *   Explica la función `render` para combinar una plantilla HTML con datos de la base de datos.
        *   Modifica la vista `listar_empleados` para obtener todos los objetos del modelo `Empleado`.
        *   Introduce el Lenguaje de Plantillas de Django (DTL): variables (`{{ empleado.nombre }}`) y etiquetas (`{% for %}`).
        *   Muestra cómo integrar Bootstrap a través de un CDN para un diseño rápido y atractivo.
    *   **Estudiante (Autónomo):**
        *   Realiza los pasos del 32 al 38 del tutorial.
        *   Crea la vista `listar_empleados`.
        *   Crea la estructura de carpetas `templates`.
        *   Escribe el código HTML en `listar_empleados.html` usando Bootstrap y DTL para mostrar los datos en una tabla.
        *   Actualiza `urls.py` y prueba la nueva página.

*   **Actividad 6: Implementación del CRUD Completo (2 horas - Práctica Demostrativa y Autónoma)**
    *   **Instructor (Demostrativo):**
        *   Explica cómo manejar peticiones `GET` y `POST` en una misma vista.
        *   Detalla la lógica de las vistas `crear_empleado`, `actualizar_empleado` y `borrar_empleado`.
        *   Muestra cómo crear formularios HTML básicos sin usar la librería `forms.py` de Django.
        *   Explica la importancia de la etiqueta `{% csrf_token %}` para la seguridad.
    *   **Estudiante (Autónomo):**
        *   Completa los pasos del 39 al 42 del tutorial.
        *   Codifica las vistas para crear, actualizar y borrar.
        *   Crea los archivos HTML correspondientes (`crear_empleado.html`, `actualizar_empleado.html`, `borrar_empleado.html`).
        *   Enlaza todo el flujo en las URLs y prueba la funcionalidad completa de la aplicación.

---

### **Momento 3: Cierre (3 Horas)**

#### **Objetivo:**
Consolidar el aprendizaje, resolver dudas finales y evaluar la adquisición de competencias a través del desarrollo de un proyecto final.

*   **Actividad 7: Revisión de Conceptos y Buenas Prácticas (1 hora - Práctica Demostrativa)**
    *   **Instructor:**
        *   Realiza un repaso completo del flujo de una petición en el proyecto creado.
        *   Resuelve las dudas más comunes que hayan surgido durante el desarrollo.
        *   Ofrece consejos sobre cómo organizar el código, la importancia de los comentarios y los siguientes pasos para aprender Django (formularios, autenticación de usuarios, despliegue).

*   **Actividad 8: Presentación y Desarrollo del Proyecto Final (2 horas - Práctica Autónoma)**
    *   **Instructor:**
        *   Presenta los requisitos del proyecto final (descrito más abajo).
        *   Explica cómo manejar la subida de archivos en Django, introduciendo el `ImageField`, la librería `Pillow` y la configuración de `MEDIA_URL` y `MEDIA_ROOT`.
        *   Asesora a los estudiantes mientras comienzan a desarrollar su proyecto final.
    *   **Estudiante (Autónomo):**
        *   Comienza a trabajar en el proyecto final, aplicando todo lo aprendido.
        *   Crea un nuevo proyecto o una nueva app, define el modelo `Producto`, y empieza a construir las vistas y plantillas para su CRUD.

---

### **Glosario de Términos**

*   **Framework:** Un conjunto de herramientas y librerías que proporciona una estructura base para desarrollar software, agilizando el proceso.
*   **Django:** Un framework de alto nivel para Python que promueve el desarrollo rápido y el diseño limpio y pragmático.
*   **MVT (Modelo-Vista-Plantilla):** Un patrón de arquitectura de software. El **Modelo** maneja los datos (base de datos), la **Vista** maneja la lógica de negocio y la **Plantilla** maneja la presentación al usuario.
*   **ORM (Object-Relational Mapper):** Técnica que permite interactuar con una base de datos como si se tratara de objetos de Python, sin necesidad de escribir código SQL directamente.
*   **CRUD:** Acrónimo de las cuatro operaciones básicas en bases de datos: **C**reate (Crear), **R**ead (Leer), **U**pdate (Actualizar) y **D**elete (Borrar).
*   **Migración:** Un archivo que describe los cambios realizados en los modelos para que Django pueda aplicarlos al esquema de la base de datos.
*   **Entorno Virtual:** Un directorio aislado que contiene una instalación específica de Python y sus paquetes, permitiendo que cada proyecto tenga sus propias dependencias sin conflictos.
*   **Vista (View):** Una función o clase en Python que recibe una petición web y devuelve una respuesta web.
*   **Plantilla (Template):** Un archivo de texto (generalmente HTML) que define la estructura de la página que se mostrará al usuario.
*   **SQLite:** Un motor de base de datos relacional ligero y sin servidor, incluido por defecto en Python, ideal para desarrollo y aplicaciones pequeñas.
*   **Bootstrap:** Un popular framework de CSS para crear diseños web atractivos y responsivos de manera rápida.

---

### **Recursos y Referencias**

*   **Documentación Oficial de Django:** [https://docs.djangoproject.com/en/stable/](https://docs.djangoproject.com/en/stable/) (La fuente de información más completa y fiable).
*   **Tutorial de Django Girls:** [https://tutorial.djangogirls.org/es/](https://tutorial.djangogirls.org/es/) (Un tutorial muy amigable y bien explicado para principiantes).
*   **MDN Web Docs (Mozilla):** [https://developer.mozilla.org/es/](https://developer.mozilla.org/es/) (Referencia indispensable para HTML, CSS y JavaScript).
*   **Documentación de Bootstrap:** [https://getbootstrap.com/docs/5.3/getting-started/introduction/](https://getbootstrap.com/docs/5.3/getting-started/introduction/)

#### **Bibliografía Recomendada**

*   "Django for Beginners: Build websites with Python and Django" por William S. Vincent.
*   "Two Scoops of Django 3.x: Best Practices for the Django Web Framework" por Daniel Roy Greenfeld y Audrey Roy Greenfeld (Nivel intermedio).

---

### **Proyecto Final: Catálogo de Productos**

#### **Objetivo:**
Crear una aplicación web para gestionar un catálogo de productos, aplicando todos los conocimientos del curso y añadiendo la funcionalidad de subida de imágenes.

#### **Requisitos:**
1.  Crear un nuevo proyecto de Django o una nueva app dentro del proyecto existente.
2.  Definir un modelo `Producto` con los campos especificados a continuación.
3.  Implementar un CRUD completo (Crear, Leer, Actualizar, Borrar) para los productos.
4.  La página principal debe mostrar una lista de todos los productos, incluyendo su imagen, nombre y precio.
5.  Utilizar Bootstrap para el diseño de la interfaz.

#### **Modelo `Producto` (products/models.py)**

```python
# products/models.py
from django.db import models
from django.utils import timezone

class Producto(models.Model):
    """
    Este modelo representa un producto en el catálogo.
    """
    nombre = models.CharField(max_length=200, verbose_name="Nombre")
    descripcion = models.TextField(verbose_name="Descripción")
    # DecimalField es ideal para precios para evitar problemas de redondeo.
    precio = models.DecimalField(max_digits=10, decimal_places=2, verbose_name="Precio")
    # PositiveIntegerField asegura que el stock no sea un número negativo.
    stock = models.PositiveIntegerField(default=0, verbose_name="Cantidad en Stock")
    # auto_now_add=True guarda la fecha y hora de creación automáticamente.
    fecha_creacion = models.DateTimeField(default=timezone.now, verbose_name="Fecha de Creación")
    # ImageField requiere la instalación de la librería Pillow (pip install Pillow).
    # 'upload_to' especifica la subcarpeta dentro de 'media' donde se guardarán las imágenes.
    imagen = models.ImageField(upload_to='productos/', null=True, blank=True, verbose_name="Imagen")

    def __str__(self):
        return self.nombre

    class Meta:
        verbose_name = "Producto"
        verbose_name_plural = "Productos"

```

#### **Configuración para la Subida de Archivos:**

1.  **Instalar Pillow:** `pip install Pillow`
2.  **En `backend_proyecto/settings.py` (añadir al final):**
    ```python
    import os
    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```
3.  **En `backend_proyecto/urls.py` (modificar para servir archivos en desarrollo):**
    ```python
    from django.conf import settings
    from django.conf.urls.static import static

    urlpatterns = [
        # ... tus otras urls ...
    ]

    if settings.DEBUG:
        urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```
4.  **En los formularios HTML, para que acepten archivos, la etiqueta `<form>` debe incluir `enctype="multipart/form-data"`:**
    ```html
    <form method="POST" enctype="multipart/form-data">
        <!-- ... -->
    </form>
    ```
