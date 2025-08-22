## Curso Acelerado de Django: Tu Primera Aplicación Web Profesional

¡Bienvenido a este curso intensivo de Django! Aquí aprenderás a construir una aplicación web profesional desde cero en tu sistema operativo Windows. Este tutorial está diseñado para principiantes con conocimientos básicos de preparatoria y te guiará paso a paso en la creación de un sistema para gestionar empleados, utilizando tecnologías de alta demanda como Python, Django, HTML, CSS, JavaScript y Bootstrap.

**Configuración Inicial y Recomendaciones**

Antes de sumergirnos en el código, asegúrate de tener todo listo.

*   **Tecnologías que usaremos**:
    *   **Python**: El lenguaje de programación principal.
    *   **Django**: El framework que nos permitirá construir la aplicación de forma rápida y segura.
    *   **HTML**: Para estructurar el contenido de nuestras páginas web.
    *   **CSS y Bootstrap**: Para dar un diseño atractivo y responsivo a nuestra aplicación.
    *   **JavaScript**: Para añadir interactividad a nuestras páginas.

*   **Software ya instalado**:
    *   Python (última versión)
    *   Visual Studio Code (VS Code)

*   **Extensiones de VS Code recomendadas**:
    *   **Python (de Microsoft)**: Esencial para el desarrollo con Python, ofrece linting, depuración y autocompletado.
    *   **Djaneiro - Django Snippets**: Proporciona atajos de código para plantillas y vistas de Django, agilizando el desarrollo.
    *   **HTML CSS Support**: Brinda autocompletado para HTML y CSS.
    *   **Live Server**: Útil para previsualizar cambios en archivos HTML estáticos al instante.

*   **Visor de Base de Datos Recomendado**:
    *   **DB Browser for SQLite**: Es una herramienta ligera, gratuita y de código abierto ideal para principiantes. Te permite abrir el archivo de base de datos de SQLite (`db.sqlite3`) para ver, editar y gestionar tus datos de forma visual sin necesidad de escribir consultas SQL complejas.

### Procedimiento Detallado Paso a Paso

Sigue estas instrucciones al pie de la letra para construir tu aplicación de gestión de empleados.

---

**1. Crear carpeta de trabajo “Curso_Django”**

En tu explorador de archivos, crea una nueva carpeta en una ubicación de fácil acceso (por ejemplo, en `C:\Users\TuUsuario\Desktop`) y nómbrala `Curso_Django`. Esta será la carpeta principal para todos tus proyectos de Django.

**2. Crear subcarpeta “practica_1”**

Dentro de `Curso_Django`, crea otra carpeta llamada `practica_1`. Cada proyecto que realices tendrá su propia carpeta.

**3. Abrir VS Code desde practica_1**

Abre VS Code. Ve a "Archivo" > "Abrir carpeta..." y selecciona la carpeta `practica_1` que acabas de crear. Esto abrirá la carpeta como tu espacio de trabajo actual.

**4. Abrir nueva terminal de VS Code y verificar versión de python y pip**

En VS Code, abre una nueva terminal (puedes usar el atajo `Ctrl+Ñ` o ir a "Terminal" > "Nuevo terminal"). En la terminal, ejecuta los siguientes comandos para asegurarte de que Python y pip (el gestor de paquetes de Python) están instalados correctamente:

```bash
python --version
pip --version
```

**5. En la terminal crear el entorno virtual .venv**

Un entorno virtual es una carpeta que contiene una instalación de Python independiente. Esto nos permite mantener las dependencias de diferentes proyectos separadas. Para crear uno, ejecuta:

```bash
python -m venv .venv
```

Verás que se crea una nueva carpeta `.venv` en tu proyecto.

**6. Activar entorno virtual**

Para empezar a usar el entorno virtual, necesitas activarlo. En la terminal de VS Code, ejecuta:

```bash
.venv\Scripts\activate
```

Notarás que el prompt de tu terminal cambia para mostrar `(.venv)` al principio, indicando que el entorno virtual está activo.

**7. Seleccionar intérprete de python**

VS Code necesita saber qué intérprete de Python usar. Presiona `Ctrl+Shift+P` para abrir la paleta de comandos, busca y selecciona "Python: Seleccionar Intérprete". Elige la opción que incluye `.venv` en la ruta.

**8. Instalar Django y verificar versión de Django**

Con el entorno virtual activado, instala Django usando pip:

```bash
pip install Django
```

Una vez instalado, verifica la versión:

```bash
django-admin --version
```

**9. Crear el proyecto “backend_empleado”**

Ahora, crearás el proyecto principal de Django. El `.` al final del comando indica que los archivos del proyecto se deben crear en el directorio actual (`practica_1`):

```bash
django-admin startproject backend_empleado .```

**10. Ejecutar servidor integrado de Django**

Django viene con un servidor web ligero para desarrollo. Para iniciarlo, ejecuta:

```bash
python manage.py runserver
```

**11. Ver página web**

Abre tu navegador web y ve a la dirección `http://127.0.0.1:8000/`. Deberías ver la página de bienvenida de Django, lo que confirma que todo funciona correctamente.

**12. Detener servidor Django**

Para detener el servidor, vuelve a la terminal de VS Code y presiona `Ctrl+C`.

**13. Realizar migración**

Las migraciones son la forma en que Django maneja los cambios en el esquema de tu base de datos. El comando `migrate` aplica las migraciones que ya existen en el proyecto (Django viene con algunas por defecto para la autenticación de usuarios, sesiones, etc.).

```bash
python manage.py migrate```

**14. Hacer migraciones**

(Este paso usualmente se refiere a `makemigrations`, que se usa después de definir modelos. Lo veremos más adelante. Por ahora, `migrate` es suficiente).

**15. Crear superusuario indicando las opciones que conlleva**

Un superusuario tiene todos los permisos en el panel de administración de Django. Créalo con este comando:

```bash
python manage.py createsuperuser
```

Se te pedirá que proporciones:
*   **Nombre de usuario**: Elige un nombre fácil de recordar (ej: `admin`).
*   **Dirección de correo electrónico**: Puedes dejarlo en blanco o poner un correo.
*   **Contraseña**: Escribe una contraseña segura (no se verá mientras escribes).
*   **Confirmar contraseña**: Vuelve a escribir la contraseña.

**16. Ejecutar servidor y acceder al panel de administración de Django**

Inicia el servidor nuevamente:

```bash
python manage.py runserver
```

En tu navegador, ve a `http://127.0.0.1:8000/admin/`. Inicia sesión con las credenciales del superusuario que acabas de crear. Explora el panel de administración.

**17. Detener el servidor**

Regresa a la terminal y presiona `Ctrl+C`.

**18. Crear aplicación “app_empleado”**

En Django, un proyecto se compone de una o más "aplicaciones". Cada aplicación se encarga de una funcionalidad específica. Vamos a crear una para gestionar a nuestros empleados:

```bash
python manage.py startapp app_empleado
```

Verás una nueva carpeta `app_empleado` en tu proyecto.

**19. En app_empleado crear el archivo urls.py**

Dentro de la carpeta `app_empleado`, crea un nuevo archivo y nómbralo `urls.py`. Aquí definiremos las rutas específicas de esta aplicación.

**20. En app_empleado en views.py crear función ver_pagina utilizando httpresponse**

Abre el archivo `app_empleado/views.py` y añade el siguiente código. Esta es una vista muy simple que solo devuelve un mensaje de texto.

```python
# app_empleado/views.py
from django.http import HttpResponse

# Create your views here.
def ver_pagina(request):
    """
    Esta es una vista simple para mostrar un mensaje de bienvenida.
    HttpResponse se usa para enviar una respuesta de texto simple al navegador.
    """
    return HttpResponse("<h1>¡Hola, Mundo desde Django!</h1>")
```

**21. En backend_empleado de setting.py agregar la app_empleado**

Ahora, tenemos que decirle a nuestro proyecto de Django que la `app_empleado` existe. Abre `backend_empleado/settings.py`, busca la lista `INSTALLED_APPS` y añade `'app_empleado'` al final:

```python
# backend_empleado/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_empleado', # Añade esta línea
]
```

**22. En backend_empleado incluir la ruta para app_empleado**

Vamos a conectar las URLs de nuestra aplicación al proyecto principal. Abre `backend_empleado/urls.py` y modifícalo para que se vea así:

```python
# backend_empleado/urls.py
from django.contrib import admin
from django.urls import path, include # Asegúrate de importar 'include'

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_empleado.urls')), # Esta línea le dice a Django que busque las URLs en app_empleado.urls
]
```

Ahora, abre el archivo `app_empleado/urls.py` que creaste antes y añade el siguiente código:

```python
# app_empleado/urls.py
from django.urls import path
from . import views # Importa las vistas de la aplicación actual

urlpatterns = [
    path('hola/', views.ver_pagina, name='ver_pagina'), # Asocia la URL 'hola/' con la vista ver_pagina
]
```

**23. Ejecutar servidor para observar ver_pagina**

Inicia el servidor:

```bash
python manage.py runserver
```

Ve a `http://127.0.0.1:8000/hola/` en tu navegador. Deberías ver el mensaje "¡Hola, Mundo desde Django!".

**24. Inspeccionar pagina web**

Haz clic derecho en la página y selecciona "Inspeccionar" o "Inspeccionar elemento". Esto abrirá las herramientas de desarrollador de tu navegador, una herramienta esencial para el desarrollo web.

**25. En app_empleado models.py crear la clase “Empleado”**

Un modelo es la representación de una tabla en tu base de datos. Abre `app_empleado/models.py` y define el modelo `Empleado`:

```python
# app_empleado/models.py
from django.db import models

# Create your models here.
class Empleado(models.Model):
    """
    Este modelo representa a un empleado en nuestra base de datos.
    Cada atributo de la clase se convierte en una columna en la tabla de la base de datos.
    """
    nombre = models.CharField(max_length=100) # Un campo de texto para el nombre
    edad = models.IntegerField() # Un campo para números enteros para la edad

    def __str__(self):
        """
        Este método define cómo se mostrará un objeto Empleado en el panel de administración.
        """
        return self.nombre

    class Meta:
        """
        La clase Meta nos permite añadir configuraciones adicionales al modelo.
        """
        verbose_name = "Empleado"
        verbose_name_plural = "Empleados"
```

**26. Registrar el modelo “Empleado” en admin.py de app_empleado**

Para poder ver y gestionar el modelo `Empleado` en el panel de administración, debemos registrarlo. Abre `app_empleado/admin.py` y añade:

```python
# app_empleado/admin.py
from django.contrib import admin
from .models import Empleado # Importa el modelo Empleado

# Register your models here.
admin.site.register(Empleado) # Registra el modelo para que aparezca en el admin
```

**27. Hacer migraciones**

Ahora que hemos creado un nuevo modelo, necesitamos crear un archivo de migración que le indique a la base de datos cómo crear la tabla correspondiente.

```bash
python manage.py makemigrations
```

**28. Realizar migrate**

Aplica la migración que acabas de crear a la base de datos:

```bash
python manage.py migrate
```

**29. Utilizar visor de base de datos para sqlite**

Cierra el servidor si está en ejecución (`Ctrl+C`). Abre "DB Browser for SQLite" y haz clic en "Abrir base de datos". Navega a tu carpeta `practica_1` y abre el archivo `db.sqlite3`. Ve a la pestaña "Explorar datos" y selecciona la tabla `app_empleado_empleado` para ver su estructura.

**30. Ejecutar servidor y acceder al panel de administración de Django**

Inicia el servidor y ve al panel de administración (`http://127.0.0.1:8000/admin/`). Verás una nueva sección "Empleados". Entra y crea un par de empleados de ejemplo.

**31. Detener servidor**

Detén el servidor con `Ctrl+C`.

**32. En app_empleado en views.py crear función listar empleados utilizando render**

Vamos a crear una vista que muestre todos los empleados de la base de datos en una página HTML. Modifica `app_empleado/views.py`:

```python
# app_empleado/views.py
from django.shortcuts import render # Importa render
from .models import Empleado # Importa el modelo Empleado

# ... tu vista ver_pagina ...

def listar_empleados(request):
    """
    Esta vista obtiene todos los empleados de la base de datos y los pasa a una plantilla.
    'render' combina una plantilla con un diccionario de contexto y devuelve una HttpResponse.
    """
    empleados = Empleado.objects.all() # Obtiene todos los objetos Empleado
    contexto = {'empleados': empleados} # Crea un diccionario para pasar los datos a la plantilla
    return render(request, 'listar_empleados.html', contexto) # Renderiza la plantilla
```

**33. Crear carpeta “templates” dentro de app_empleado**

Dentro de la carpeta `app_empleado`, crea una nueva carpeta llamada `templates`. Django buscará automáticamente las plantillas HTML en esta carpeta.

**34. Dentro de templates crear el archivo listar_empleados.html**

Dentro de la carpeta `app_empleado/templates`, crea un archivo llamado `listar_empleados.html`.

**35. Crear el código para listar empleados en listar_empleados.html con Bootstrap**

Pega el siguiente código en `listar_empleados.html`. Este código utiliza Bootstrap para crear una tabla atractiva y responsiva.

```html
<!-- app_empleado/templates/listar_empleados.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Empleados</title>
    <!-- CSS de Bootstrap desde un CDN para un diseño rápido y atractivo -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div class="container mt-5">
        <h1 class="mb-4">Gestión de Empleados</h1>
        
        <!-- Botón para ir al formulario de creación de nuevos empleados -->
        <a href="{% url 'crear_empleado' %}" class="btn btn-primary mb-3">Añadir Nuevo Empleado</a>

        <!-- Tabla para mostrar la lista de empleados -->
        <table class="table table-striped table-hover">
            <thead class="table-dark">
                <tr>
                    <th>Nombre</th>
                    <th>Edad</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                <!-- Bucle de Django para iterar sobre la lista de empleados -->
                {% for empleado in empleados %}
                <tr>
                    <td>{{ empleado.nombre }}</td>
                    <td>{{ empleado.edad }}</td>
                    <td>
                        <!-- Botones de acción para cada empleado -->
                        <a href="{% url 'actualizar_empleado' empleado.id %}" class="btn btn-warning btn-sm">Editar</a>
                        <a href="{% url 'borrar_empleado' empleado.id %}" class="btn btn-danger btn-sm">Borrar</a>
                    </td>
                </tr>
                {% endfor %}
            </tbody>
        </table>
    </div>
</body>
</html>
```

**36. Actualizar urls.py de app_empleado**

Añade la ruta para la nueva vista en `app_empleado/urls.py`:

```python
# app_empleado/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('hola/', views.ver_pagina, name='ver_pagina'),
    path('', views.listar_empleados, name='listar_empleados'), # Ruta principal que muestra la lista
    # Las siguientes rutas las crearemos en los próximos pasos
    path('crear/', views.crear_empleado, name='crear_empleado'),
    path('actualizar/<int:id>/', views.actualizar_empleado, name='actualizar_empleado'),
    path('borrar/<int:id>/', views.borrar_empleado, name='borrar_empleado'),
]
```
*(Nota: Hemos añadido las rutas para el CRUD completo de una vez para simplificar los siguientes pasos).*

**37. Ejecutar servidor para ver listar empleados**

Inicia el servidor y ve a `http://127.0.0.1:8000/`. Verás la tabla con los empleados que creaste en el panel de administración.

**38. Detener servidor**

Detén el servidor con `Ctrl+C`.

**39. En app_empleado en views.py crear las demás funciones para un CRUD básico**

Ahora, implementaremos la lógica para crear, actualizar y borrar empleados. Añade las siguientes funciones a `app_empleado/views.py`.

```python
# app_empleado/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Empleado

# ... vista ver_pagina y listar_empleados ...

def crear_empleado(request):
    """
    Vista para crear un nuevo empleado.
    Si el método es POST, procesa los datos del formulario.
    Si es GET, muestra el formulario vacío.
    """
    if request.method == 'POST':
        # Obtenemos los datos del formulario enviado
        nombre = request.POST['nombre']
        edad = request.POST['edad']
        # Creamos una nueva instancia del modelo Empleado y la guardamos
        Empleado.objects.create(nombre=nombre, edad=edad)
        # Redirigimos al usuario a la lista de empleados
        return redirect('listar_empleados')
    return render(request, 'crear_empleado.html')

def actualizar_empleado(request, id):
    """
    Vista para actualizar un empleado existente.
    """
    # Obtenemos el empleado específico o mostramos un error 404 si no existe
    empleado = get_object_or_404(Empleado, id=id)
    if request.method == 'POST':
        # Actualizamos los datos del empleado con la información del formulario
        empleado.nombre = request.POST['nombre']
        empleado.edad = request.POST['edad']
        empleado.save()
        return redirect('listar_empleados')
    # Pasamos los datos del empleado a la plantilla para que se muestren en el formulario
    contexto = {'empleado': empleado}
    return render(request, 'actualizar_empleado.html', contexto)

def borrar_empleado(request, id):
    """
    Vista para borrar un empleado.
    """
    empleado = get_object_or_404(Empleado, id=id)
    if request.method == 'POST':
        # Borramos el empleado y redirigimos a la lista
        empleado.delete()
        return redirect('listar_empleados')
    # Si es una petición GET, mostramos una página de confirmación
    contexto = {'empleado': empleado}
    return render(request, 'borrar_empleado.html', contexto)

```

**40. Crear los archivos HTML correspondientes dentro de templates**

Necesitamos crear las plantillas para las vistas de creación, actualización y borrado.

*   **crear_empleado.html**:

```html
<!-- app_empleado/templates/crear_empleado.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Añadir Empleado</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div class="container mt-5">
        <h1>Añadir Nuevo Empleado</h1>
        <!-- Formulario para enviar los datos del nuevo empleado -->
        <form method="POST">
            {% csrf_token %} <!-- Token de seguridad de Django -->
            <div class="mb-3">
                <label for="nombre" class="form-label">Nombre del Empleado</label>
                <input type="text" class="form-control" id="nombre" name="nombre" required>
            </div>
            <div class="mb-3">
                <label for="edad" class="form-label">Edad</label>
                <input type="number" class="form-control" id="edad" name="edad" required>
            </div>
            <button type="submit" class="btn btn-success">Guardar</button>
            <a href="{% url 'listar_empleados' %}" class="btn btn-secondary">Cancelar</a>
        </form>
    </div>
</body>
</html>
```

*   **actualizar_empleado.html**:

```html
<!-- app_empleado/templates/actualizar_empleado.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Editar Empleado</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div class="container mt-5">
        <h1>Editar Empleado</h1>
        <!-- El formulario se llena con los datos actuales del empleado -->
        <form method="POST">
            {% csrf_token %}
            <div class="mb-3">
                <label for="nombre" class="form-label">Nombre del Empleado</label>
                <input type="text" class="form-control" id="nombre" name="nombre" value="{{ empleado.nombre }}" required>
            </div>
            <div class="mb-3">
                <label for="edad" class="form-label">Edad</label>
                <input type="number" class="form-control" id="edad" name="edad" value="{{ empleado.edad }}" required>
            </div>
            <button type="submit" class="btn btn-success">Actualizar</button>
            <a href="{% url 'listar_empleados' %}" class="btn btn-secondary">Cancelar</a>
        </form>
    </div>
</body>
</html>
```

*   **borrar_empleado.html**:

```html
<!-- app_empleado/templates/borrar_empleado.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Confirmar Borrado</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div class="container mt-5">
        <div class="alert alert-danger" role="alert">
            <h4 class="alert-heading">¡Atención!</h4>
            <p>¿Estás seguro de que deseas borrar al empleado <strong>{{ empleado.nombre }}</strong>?</p>
            <hr>
            <p class="mb-0">Esta acción no se puede deshacer.</p>
        </div>
        <!-- Formulario para confirmar la eliminación -->
        <form method="POST">
            {% csrf_token %}
            <button type="submit" class="btn btn-danger">Sí, borrar</button>
            <a href="{% url 'listar_empleados' %}" class="btn btn-secondary">Cancelar</a>
        </form>
    </div>
</body>
</html>
```

**41. Actualizar la urls.py de En app_empleado**

Este paso ya lo realizamos en el punto 36, donde definimos todas las URLs necesarias para el CRUD.

**42. Ejecutar servidor para ver el proyecto totalmente funcionando**

¡Es el momento de la verdad! Ejecuta el servidor:

```bash
python manage.py runserver
```

Abre `http://127.0.0.1:8000/` en tu navegador. Ahora puedes:
*   Ver la lista de empleados.
*   Hacer clic en "Añadir Nuevo Empleado" para crear uno nuevo.
*   Hacer clic en "Editar" para modificar los datos de un empleado.
*   Hacer clic en "Borrar" para eliminar un empleado (con una página de confirmación).

¡Felicidades! Has construido una aplicación web CRUD completa y profesional con Django. Has aprendido a configurar un proyecto, crear modelos, vistas y plantillas, y a manejar datos sin depender de los formularios automáticos de Django, todo ello con un diseño atractivo gracias a Bootstrap.
