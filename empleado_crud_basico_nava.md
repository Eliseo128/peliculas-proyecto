Perfecto 🙌
Te prepararé un **curso paso a paso** para crear un proyecto en **Django con Python**, pensado para **estudiantes de preparatoria** nivel principiante en **Windows**.
El proyecto será un CRUD de empleados, sin usar `forms.py` ni `base.html`, pero con explicaciones breves en el código para que sea claro y atractivo.

---

# 📘 Curso Django con Python – Proyecto CRUD de Empleados

**Sistema operativo:** Windows
**Nivel:** Principiante
**Tecnologías:** Python, Django, HTML, CSS, JS, Bootstrap
**Software instalado:**

* Python (última versión)
* Visual Studio Code (VS Code)
* Extensiones recomendadas en VS Code:

  * Python
  * Django
  * HTML CSS Support
  * SQLite Viewer (para ver la base de datos SQLite desde VS Code)

---

## 🚀 Procedimiento Paso a Paso

### 1. Crear carpeta de trabajo

Abrir la terminal de Windows (`cmd`) y ejecutar:

```bash
mkdir Curso_Django
cd Curso_Django
```

### 2. Crear subcarpeta del proyecto

```bash
mkdir practica_1
cd practica_1
```

### 3. Abrir VS Code desde la carpeta

```bash
code .
```

### 4. Verificar versión de Python y pip

En la terminal integrada de VS Code:

```bash
python --version
pip --version
```

### 5. Crear entorno virtual

```bash
python -m venv .venv
```

### 6. Activar entorno virtual

```bash
.venv\Scripts\activate
```

### 7. Seleccionar intérprete de Python en VS Code

En VS Code:
`Ctrl + Shift + P` → escribir **Python: Select Interpreter** → elegir el de `.venv`.

### 8. Instalar Django

```bash
pip install django
django-admin --version
```

### 9. Crear proyecto Django

```bash
django-admin startproject backend_empleado .
```

### 10. Ejecutar servidor

```bash
python manage.py runserver
```

### 11. Ver página web

Abrir navegador: `http://127.0.0.1:8000/`

### 12. Detener servidor

`Ctrl + C`

### 13-14. Migraciones iniciales

```bash
python manage.py makemigrations
python manage.py migrate
```

### 15. Crear superusuario

```bash
python manage.py createsuperuser
```

👉 Introducir nombre de usuario, email (opcional), contraseña.

### 16. Ejecutar servidor y entrar a admin

```bash
python manage.py runserver
```

Ir a: `http://127.0.0.1:8000/admin/`
👉 Iniciar sesión con superusuario.

### 17. Detener servidor

`Ctrl + C`

### 18. Crear aplicación

```bash
python manage.py startapp app_empleado
```

### 19. Crear `urls.py` en `app_empleado`

📂 `app_empleado/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.ver_pagina, name='ver_pagina'),
]
```

### 20. Crear función en `views.py`

📂 `app_empleado/views.py`

```python
from django.http import HttpResponse

# Función simple que muestra texto en la web
def ver_pagina(request):
    return HttpResponse("<h1>Bienvenido a la página de Empleados</h1>")
```

### 21. Registrar app en `settings.py`

📂 `backend_empleado/settings.py`

```python
INSTALLED_APPS = [
    ...
    'app_empleado',
]
```

### 22. Incluir rutas de `app_empleado`

📂 `backend_empleado/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('empleados/', include('app_empleado.urls')),  # incluir rutas
]
```

### 23-24. Ejecutar servidor e inspeccionar página

👉 Ir a `http://127.0.0.1:8000/empleados/`

---

## 🗂️ Modelo Empleado

### 25. Crear modelo en `models.py`

📂 `app_empleado/models.py`

```python
from django.db import models

# Modelo Empleado con nombre y edad
class Empleado(models.Model):
    nombre = models.CharField(max_length=100)  # campo de texto
    edad = models.IntegerField()               # número entero

    def __str__(self):
        return self.nombre

    class Meta:
        verbose_name = "Empleado"
        verbose_name_plural = "Empleados"
```

### 26. Registrar modelo en `admin.py`

📂 `app_empleado/admin.py`

```python
from django.contrib import admin
from .models import Empleado

# Registrar el modelo para verlo en el panel de admin
admin.site.register(Empleado)
```

### 27-28. Migraciones

```bash
python manage.py makemigrations
python manage.py migrate
```

### 29. Ver base de datos SQLite

👉 Usar extensión **SQLite Viewer** en VS Code.

### 30. Ejecutar servidor y acceder a admin

👉 `http://127.0.0.1:8000/admin/`
👉 Ver empleados.

---

## 📄 Listar empleados

### 32. Función en `views.py`

📂 `app_empleado/views.py`

```python
from django.shortcuts import render
from .models import Empleado

# Listar empleados en una tabla HTML
def listar_empleados(request):
    empleados = Empleado.objects.all()
    return render(request, "listar_empleados.html", {"empleados": empleados})
```

### 33-34. Crear `templates/listar_empleados.html`

📂 `app_empleado/templates/listar_empleados.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Lista de Empleados</title>
    <!-- Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="container mt-5">
    <h1 class="text-center">Lista de Empleados</h1>
    <a href="{% url 'crear_empleado' %}" class="btn btn-success mb-3">➕ Agregar Empleado</a>
    <table class="table table-striped table-hover shadow">
        <thead class="table-dark">
            <tr>
                <th>ID</th>
                <th>Nombre</th>
                <th>Edad</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
        {% for empleado in empleados %}
            <tr>
                <td>{{ empleado.id }}</td>
                <td>{{ empleado.nombre }}</td>
                <td>{{ empleado.edad }}</td>
                <td>
                    <a href="{% url 'actualizar_empleado' empleado.id %}" class="btn btn-warning btn-sm">✏️ Editar</a>
                    <a href="{% url 'borrar_empleado' empleado.id %}" class="btn btn-danger btn-sm">🗑️ Borrar</a>
                </td>
            </tr>
        {% endfor %}
        </tbody>
    </table>
</body>
</html>
```

### 36. Actualizar `urls.py`

📂 `app_empleado/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_empleados, name='listar_empleados'),
    path('crear/', views.crear_empleado, name='crear_empleado'),
    path('actualizar/<int:id>/', views.actualizar_empleado, name='actualizar_empleado'),
    path('borrar/<int:id>/', views.borrar_empleado, name='borrar_empleado'),
]
```

---

## 🛠️ CRUD Completo

### 39. Views para CRUD

📂 `app_empleado/views.py`

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Empleado

# Crear
def crear_empleado(request):
    if request.method == "POST":
        nombre = request.POST["nombre"]
        edad = request.POST["edad"]
        Empleado.objects.create(nombre=nombre, edad=edad)
        return redirect("listar_empleados")
    return render(request, "crear_empleado.html")

# Actualizar
def actualizar_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    if request.method == "POST":
        empleado.nombre = request.POST["nombre"]
        empleado.edad = request.POST["edad"]
        empleado.save()
        return redirect("listar_empleados")
    return render(request, "actualizar_empleado.html", {"empleado": empleado})

# Borrar
def borrar_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    if request.method == "POST":
        empleado.delete()
        return redirect("listar_empleados")
    return render(request, "borrar_empleado.html", {"empleado": empleado})
```

### 40. HTMLs para CRUD

📂 `app_empleado/templates/crear_empleado.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Crear Empleado</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="container mt-5">
    <h1>Crear Empleado</h1>
    <form method="post">
        {% csrf_token %}
        <input type="text" name="nombre" class="form-control mb-2" placeholder="Nombre" required>
        <input type="number" name="edad" class="form-control mb-2" placeholder="Edad" required>
        <button type="submit" class="btn btn-success">Guardar</button>
        <a href="{% url 'listar_empleados' %}" class="btn btn-secondary">Cancelar</a>
    </form>
</body>
</html>
```

📂 `app_empleado/templates/actualizar_empleado.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Actualizar Empleado</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="container mt-5">
    <h1>Actualizar Empleado</h1>
    <form method="post">
        {% csrf_token %}
        <input type="text" name="nombre" class="form-control mb-2" value="{{ empleado.nombre }}" required>
        <input type="number" name="edad" class="form-control mb-2" value="{{ empleado.edad }}" required>
        <button type="submit" class="btn btn-warning">Actualizar</button>
        <a href="{% url 'listar_empleados' %}" class="btn btn-secondary">Cancelar</a>
    </form>
</body>
</html>
```

📂 `app_empleado/templates/borrar_empleado.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Borrar Empleado</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="container mt-5">
    <h1>¿Seguro que deseas eliminar a {{ empleado.nombre }}?</h1>
    <form method="post">
        {% csrf_token %}
        <button type="submit" class="btn btn-danger">Sí, eliminar</button>
        <a href="{% url 'listar_empleados' %}" class="btn btn-secondary">Cancelar</a>
    </form>
</body>
</html>
```

---

## ✅ Proyecto Final

* **CRUD básico de empleados (crear, leer, actualizar, borrar).**
* **Sin forms.py ni base.html** (cada plantilla es independiente).
* **Comentarios en Python y HTML para guiar a estudiantes.**
* **Estilo con Bootstrap para hacerlo atractivo.**

---

👉 ¿Quieres que prepare también un **diagrama visual del CRUD de empleados** (flujo y relación con la base de datos) para que los estudiantes lo entiendan mejor?
