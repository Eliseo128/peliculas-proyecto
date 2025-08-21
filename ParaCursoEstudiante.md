¡Claro! A continuación, te presento tu guía completa **con comentarios breves explicativos** integrados en cada paso, para que sea más clara, educativa y útil como material de aprendizaje. Todo el contenido está organizado y enriquecido con anotaciones pedagógicas.

---

# 🐍 Django First App: Tu primera app web usando Django  
### (Hola Mundo + CRUD de Estudiantes)

> **Objetivo**: Aprender los conceptos básicos de Django creando una aplicación simple que muestre "Hola Mundo" y permita gestionar estudiantes mediante el panel de administración y una vista HTML.

---

## 🌍 Parte 1: Hola Mundo en Django

### 1. Creación de un entorno virtual  
**(Windows)**

> 💡 **¿Por qué?** Un entorno virtual aísla las dependencias de tu proyecto, evitando conflictos con otros proyectos.

```bash
python -m venv venv
```
- Crea un entorno virtual llamado `venv` en la carpeta actual.

```bash
venv\Scripts\activate
```
- Activa el entorno. Verás `(venv)` en la línea de comandos, indicando que estás dentro del entorno.

> ✅ **Consejo**: Usa `deactivate` para salir del entorno cuando termines.

---

### 2. Instalación de Django

```bash
pip install django
```
- Instala el framework Django, necesario para crear aplicaciones web en Python.

> 📦 `pip` es el gestor de paquetes de Python. Django se descarga desde PyPI (Python Package Index).

---

### 3. Creación de un proyecto

```bash
django-admin startproject base
cd base
```
- `startproject base`: Crea una estructura base de proyecto Django llamado `base`.
- `cd base`: Entra en la carpeta del proyecto para trabajar dentro de él.

> 🏗️ Estructura generada:
> - `manage.py`: Herramienta para gestionar el proyecto (correr servidor, migraciones, etc.).
> - Carpeta `base/`: Contiene la configuración principal (`settings.py`, `urls.py`, etc.).

---

### 4. Creación de una aplicación

```bash
python manage.py startapp myapp
```
- Crea una **app** llamada `myapp`. En Django, las apps son módulos independientes que agrupan funcionalidades (ej: estudiantes, cursos, etc.).

📌 **Registra la app** en `base/settings.py`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    # ... otros apps por defecto
    'myapp',  # ✅ Añade tu app aquí para que Django la reconozca
]
```

> 🔁 Django carga todas las apps listadas aquí al iniciar. Sin esto, no funcionará `myapp`.

---

### 5. Crear una vista "Hola Mundo"

En `myapp/views.py`:

```python
from django.http import HttpResponse

def hola_mundo(request):
    return HttpResponse("Hola mundo con django")
```

- `HttpResponse`: Permite devolver texto plano como respuesta HTTP.
- `request`: Objeto que contiene información sobre la solicitud del usuario (navegador).

📌 **Configura la URL** en `base/urls.py`:

```python
from django.contrib import admin
from django.urls import path
from myapp.views import hola_mundo  # 🔹 Importa la vista

urlpatterns = [
    path('admin/', admin.site.urls),
    path('hola/', hola_mundo),  # 🔹 Ruta: /hola/ → llama a hola_mundo
]
```

> 🌐 `path('hola/', ...)` define la URL. Cuando alguien visite `/hola/`, se ejecuta `hola_mundo`.

✅ **Levanta el servidor**:

```bash
python manage.py runserver
```

- Inicia un servidor de desarrollo local.
- Accede a: `http://127.0.0.1:8000/hola/` → ¡Verás el mensaje!

> 🛑 Detén el servidor con `Ctrl + C` cuando quieras.

---

## 🧩 Parte 2: Mini Proyecto de Estudiantes (CRUD con HTML y Admin)

### 6. Crear el Modelo `Estudiante`

En `myapp/models.py`:

```python
from django.db import models

class Estudiante(models.Model):
    nombre = models.CharField(max_length=100)      # Nombre del estudiante (texto)
    edad = models.IntegerField()                   # Edad (número entero)
    correo = models.EmailField()                   # Correo (validado automáticamente)

    def __str__(self):
        return self.nombre  # Muestra el nombre en el panel admin y consultas
```

> 🗃️ Este modelo define una tabla en la base de datos con 3 columnas.  
> 🔁 `__str__` mejora la legibilidad en el admin y en la consola.

---

### 7. Migrar el modelo a la base de datos

```bash
python manage.py makemigrations myapp
```
- Django crea un archivo de migración que describe los cambios en los modelos (como un "script de versión de base de datos").

```bash
python manage.py migrate
```
- Aplica los cambios a la base de datos (por defecto: `db.sqlite3`).

> 💾 Se crea automáticamente un archivo `db.sqlite3` en tu proyecto. Es una base de datos ligera y sin servidor.

---

### 8. Registrar el Modelo en el Admin

En `myapp/admin.py`:

```python
from django.contrib import admin
from .models import Estudiante  # Importa el modelo

admin.site.register(Estudiante)  # Registra el modelo para gestionarlo desde el admin
```

> 🎛️ El **Admin Panel** es una interfaz lista para usar donde puedes gestionar tus datos sin escribir HTML ni vistas.

---

### 9. Crear superusuario para acceder al Admin

```bash
python manage.py createsuperuser
```

- Sigue las instrucciones para crear un usuario administrador:
  - Usuario
  - Correo (opcional)
  - Contraseña (segura)

> 🔐 Este usuario podrá acceder a `/admin/` y gestionar todos los modelos registrados.

---

### 10. Levantar servidor y entrar al Admin

```bash
python manage.py runserver
```

Accede a:  
👉 `http://127.0.0.1:8000/admin/`

- Inicia sesión con tu superusuario.
- Verás el modelo **Estudiante** listo para:
  - ✅ Crear nuevos estudiantes
  - ✏️ Editar existentes
  - 🗑️ Eliminar registros

> ✅ Has hecho el **C** (Crear), **R** (Leer), **U** (Actualizar) y **D** (Eliminar) → ¡un CRUD completo!

---

### 11. Crear una vista para listar estudiantes

En `myapp/views.py`, añade:

```python
from django.shortcuts import render
from .models import Estudiante  # Importa el modelo

def listar_estudiantes(request):
    estudiantes = Estudiante.objects.all()  # Obtiene todos los estudiantes de la BD
    return render(request, 'listar_estudiantes.html', {'estudiantes': estudiantes})
    # Renderiza una plantilla HTML y le pasa los datos
```

> 📥 `objects.all()` consulta la base de datos y devuelve todos los registros del modelo.

---

### 12. Crear plantilla HTML

Crea la estructura:
```
myapp/
 └── templates/
      └── listar_estudiantes.html
```

Contenido del archivo `listar_estudiantes.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Lista de Estudiantes</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 50%; border-collapse: collapse; }
        th, td { border: 1px solid black; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h1>Lista de Estudiantes</h1>
    <table>
        <tr>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Correo</th>
        </tr>
        {% for estudiante in estudiantes %}
        <tr>
            <td>{{ estudiante.nombre }}</td>
            <td>{{ estudiante.edad }}</td>
            <td>{{ estudiante.correo }}</td>
        </tr>
        {% empty %}
        <tr>
            <td colspan="3">No hay estudiantes registrados.</td>
        </tr>
        {% endfor %}
    </table>
</body>
</html>
```

> 🧩 **Template Tags**:
> - `{% for %}`: Bucle para recorrer la lista.
> - `{% empty %}`: Muestra un mensaje si no hay datos.
> - `{{ }}`: Muestra valores de variables.

> 📁 Django busca plantillas en la carpeta `templates/` de cada app (gracias a `DjangoTemplates` en `settings.py`).

---

### 13. Configurar la URL para la nueva vista

En `base/urls.py`:

```python
from myapp.views import hola_mundo, listar_estudiantes  # 🔹 Importa ambas vistas

urlpatterns = [
    path('admin/', admin.site.urls),
    path('hola/', hola_mundo),
    path('estudiantes/', listar_estudiantes),  # 🔹 Nueva ruta
]
```

✅ **Resultado final**:
- Visita `http://127.0.0.1:8000/estudiantes/`
- Verás una tabla con todos los estudiantes creados en el admin.

---

## ✅ ¡Listo! Has completado tu primera app Django

### 🎉 ¿Qué aprendiste?

| Concepto | ¿Qué hiciste? |
|--------|----------------|
| **Vistas** | Creaste funciones que responden a URLs. |
| **URLs** | Mapeaste rutas a funciones (en `urls.py`). |
| **Modelos** | Definiste estructura de datos (tabla `Estudiante`). |
| **Migraciones** | Sincronizaste modelos con la base de datos. |
| **Admin Panel** | Gestionaste datos sin escribir HTML. |
| **Plantillas** | Mostraste datos dinámicos en una página web. |

---

### 🧪 Prueba final

1. Crea 3 estudiantes desde el admin.
2. Ve a `http://127.0.0.1:8000/estudiantes/` y verifica que se muestren.
3. Agrega uno nuevo y actualiza la página → ¡debe aparecer!

> 🎉 **¡Felicidades!** Ya tienes una app web funcional con Django.

---

### 📚 ¿Qué sigue?

- Añadir formularios para crear estudiantes desde la web.
- Usar `DetailView`, `CreateView`, etc. (vistas basadas en clases).
- Añadir estilos con Bootstrap.
- Desplegar tu app en la nube (ej: Render, Railway, PythonAnywhere).

---

¿Quieres que convierta esta guía en un **PDF**, o que genere un **repositorio inicial en GitHub** con toda la estructura lista para usar?
