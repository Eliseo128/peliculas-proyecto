¡Claro! Aquí tienes el modelo actualizado, reemplazando **`Rol`** por **`Puesto`** (que es un término más común en contextos laborales en español para referirse al cargo o función del empleado):

```python
from django.db import models

# Crea tus modelos aquí.


class Departamento(models.Model):
    nombre = models.CharField(max_length=100, null=False)
    ubicacion = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre


class Puesto(models.Model):
    nombre = models.CharField(max_length=100, null=False)

    def __str__(self):
        return self.nombre


class Empleado(models.Model):
    nombre = models.CharField(max_length=100, null=False)
    apellido = models.CharField(max_length=100)
    departamento = models.ForeignKey(Departamento, on_delete=models.CASCADE)
    salario = models.IntegerField(default=0)
    bono = models.IntegerField(default=0)
    puesto = models.ForeignKey(Puesto, on_delete=models.CASCADE)
    telefono = models.IntegerField(default=0)
    fecha_contratacion = models.DateField()

    def __str__(self):
        return "%s %s %s" % (self.nombre, self.apellido, self.telefono)
```

### Cambios clave:
- `Role` → `Puesto`
- `role` (atributo en `Empleado`) → `puesto`

### ¿Por qué “Puesto”?
En muchos países hispanohablantes, se usa comúnmente **“puesto de trabajo”** o simplemente **“puesto”** para referirse al cargo que ocupa una persona en una empresa (ej. “Puesto: Gerente de Ventas”). Es más natural y comprensible que “Rol” en este contexto laboral.

✅ Esta versión es más intuitiva para usuarios hispanohablantes y mantiene la lógica del modelo original.

¿Necesitas también traducir los mensajes de validación, verbose_name o ayuda en los formularios? ¡Con gusto te ayudo a completarlo!
