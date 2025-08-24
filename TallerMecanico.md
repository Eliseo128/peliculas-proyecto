¡Perfecto! Vamos a **traducir todo al español** manteniendo buenas prácticas de Django:

* Clases en singular y en **PascalCase**.
* Campos en **snake\_case** (minúsculas y con guiones bajos).
* Uso de `verbose_name` para mejorar la visualización en el **admin**.
* Traducción de `choices`.

Aquí está el código convertido:

```python
from django.db import models
from django.contrib.auth.models import User


# --- Cliente ---
class Cliente(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE, verbose_name="Usuario")
    foto_perfil = models.ImageField(
        upload_to='profile_pic/ClientePerfil/',
        null=True, blank=True,
        verbose_name="Foto de Perfil"
    )
    direccion = models.CharField(max_length=40, verbose_name="Dirección")
    telefono = models.CharField(max_length=20, null=False, verbose_name="Teléfono")

    @property
    def obtener_nombre(self):
        return f"{self.usuario.first_name} {self.usuario.last_name}"

    @property
    def obtener_instancia(self):
        return self

    def __str__(self):
        return self.usuario.first_name


# --- Mecánico ---
class Mecanico(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE, verbose_name="Usuario")
    foto_perfil = models.ImageField(
        upload_to='profile_pic/MecanicoPerfil/',
        null=True, blank=True,
        verbose_name="Foto de Perfil"
    )
    direccion = models.CharField(max_length=40, verbose_name="Dirección")
    telefono = models.CharField(max_length=20, null=False, verbose_name="Teléfono")
    habilidad = models.CharField(max_length=500, null=True, verbose_name="Habilidad")
    salario = models.PositiveIntegerField(null=True, verbose_name="Salario")
    activo = models.BooleanField(default=False, verbose_name="Activo")

    @property
    def obtener_nombre(self):
        return f"{self.usuario.first_name} {self.usuario.last_name}"

    @property
    def obtener_id(self):
        return self.usuario.id

    def __str__(self):
        return self.usuario.first_name


# --- Solicitud de Servicio ---
class Solicitud(models.Model):
    CATEGORIAS = (
        ('Motocicleta con cambios', 'Motocicleta con cambios'),
        ('Motocicleta sin cambios', 'Motocicleta sin cambios'),
        ('Triciclo', 'Triciclo'),
        ('Automóvil', 'Automóvil'),
    )
    categoria = models.CharField(max_length=50, choices=CATEGORIAS, verbose_name="Categoría")

    numero_vehiculo = models.PositiveIntegerField(null=False, verbose_name="Número de Vehículo")
    nombre_vehiculo = models.CharField(max_length=40, null=False, verbose_name="Nombre del Vehículo")
    modelo_vehiculo = models.CharField(max_length=40, null=False, verbose_name="Modelo del Vehículo")
    marca_vehiculo = models.CharField(max_length=40, null=False, verbose_name="Marca del Vehículo")

    descripcion_problema = models.CharField(max_length=500, null=False, verbose_name="Descripción del Problema")
    fecha = models.DateField(auto_now=True, verbose_name="Fecha")
    costo = models.PositiveIntegerField(null=True, verbose_name="Costo")

    cliente = models.ForeignKey('Cliente', on_delete=models.CASCADE, null=True, verbose_name="Cliente")
    mecanico = models.ForeignKey('Mecanico', on_delete=models.CASCADE, null=True, verbose_name="Mecánico")

    ESTADOS = (
        ('Pendiente', 'Pendiente'),
        ('Aprobado', 'Aprobado'),
        ('En reparación', 'En reparación'),
        ('Reparación finalizada', 'Reparación finalizada'),
        ('Entregado', 'Entregado'),
    )
    estado = models.CharField(max_length=50, choices=ESTADOS, default='Pendiente', null=True, verbose_name="Estado")

    def __str__(self):
        return self.descripcion_problema


# --- Asistencia ---
class Asistencia(models.Model):
    mecanico = models.ForeignKey('Mecanico', on_delete=models.CASCADE, null=True, verbose_name="Mecánico")
    fecha = models.DateField(verbose_name="Fecha")
    estado_asistencia = models.CharField(max_length=10, verbose_name="Estado de Asistencia")

    def __str__(self):
        return f"Asistencia de {self.mecanico} - {self.fecha}"


# --- Retroalimentación ---
class Retroalimentacion(models.Model):
    fecha = models.DateField(auto_now=True, verbose_name="Fecha")
    autor = models.CharField(max_length=40, verbose_name="Autor")
    mensaje = models.CharField(max_length=500, verbose_name="Mensaje")

    def __str__(self):
        return f"Feedback de {self.autor} - {self.fecha}"
```

---

### 🔍 Cambios que realicé:

✅ **Traducción de nombres de clases**:

* `Customer → Cliente`
* `Mechanic → Mecanico`
* `Request → Solicitud`
* `Attendance → Asistencia`
* `Feedback → Retroalimentacion`

✅ **Traducción de campos**:

* `profile_pic → foto_perfil`
* `address → direccion`
* `mobile → telefono`
* `skill → habilidad`
* `salary → salario`
* `status → activo / estado`
* `vehicle_no → numero_vehiculo`, etc.

✅ **Choices traducidos** a español (categorías de vehículos y estados de solicitudes).
✅ Se añadió `verbose_name` para que en **Django Admin** se vean nombres claros.
✅ Métodos `__str__` más descriptivos.

---

¿Quieres que también prepare **un diagrama ERD** (en español) con las relaciones entre estas clases?
