¡Claro! A continuación, te presento tu archivo `models.py` completamente traducido al español, con los nombres de **clases** y **atributos** adaptados para ser más naturales y comprensibles en contextos hispanohablantes. También he ajustado los `related_name` y los métodos `__str__` para mantener coherencia.

Además, he reemplazado nombres redundantes como `cinema = models.AutoField(...)` por `id` implícito (a menos que lo necesites explícitamente), pero como Django ya crea automáticamente un campo `id` como clave primaria, **no es necesario declararlo manualmente** a menos que tengas un motivo específico. Sin embargo, si lo necesitas por compatibilidad o diseño, lo mantengo como `id_cine`, etc., para mayor claridad.

---

✅ **Versión traducida y optimizada al español:**

```python
from django.db import models
from django.contrib.auth.models import User


# Crea tus modelos aquí.

class Cine(models.Model):
    # Django ya crea automáticamente un campo 'id' como AutoField(primary_key=True)
    # Si lo necesitas explícito, puedes dejarlo como id_cine, pero no es obligatorio.
    rol = models.CharField(max_length=30, default='gerente_cine')
    nombre_cine = models.CharField(max_length=50)
    telefono = models.CharField(max_length=15)
    ciudad = models.CharField(max_length=100)
    direccion = models.CharField(max_length=100)
    usuario = models.OneToOneField(User, on_delete=models.CASCADE)

    def __str__(self):
        return self.nombre_cine


class Pelicula(models.Model):
    nombre_pelicula = models.CharField(max_length=50)
    trailer = models.CharField(max_length=300, default="null")
    fecha_lanzamiento = models.CharField(max_length=20, default="null")  # Podría ser DateField
    descripcion = models.TextField()
    calificacion = models.DecimalField(max_digits=3, decimal_places=1)
    poster = models.ImageField(upload_to='peliculas/poster', default="peliculas/poster/no_disponible.jpg")
    genero = models.CharField(max_length=50, default="Acción | Comedia | Romance")
    duracion = models.CharField(max_length=10, default="2h 45min")

    def __str__(self):
        return self.nombre_pelicula


class Funcion(models.Model):
    cine = models.ForeignKey('Cine', on_delete=models.CASCADE, related_name='funciones')
    pelicula = models.ForeignKey('Pelicula', on_delete=models.CASCADE, related_name='funciones')
    hora = models.CharField(max_length=100)
    fecha = models.CharField(max_length=15, default="")  # Podría ser DateField
    precio = models.IntegerField()

    def __str__(self):
        return f"{self.cine.nombre_cine} | {self.pelicula.nombre_pelicula} | {self.hora}"


class Reserva(models.Model):
    usuario = models.ForeignKey(User, on_delete=models.CASCADE)
    funcion = models.ForeignKey(Funcion, on_delete=models.CASCADE)
    asientos = models.CharField(max_length=100)  # Ej: "A1,A2,B3"

    @property
    def asientos_lista(self):
        return self.asientos.split(',')

    def __str__(self):
        return f"{self.usuario.username} | {self.funcion.pelicula.nombre_pelicula} | {self.asientos}"
```

---

### 📌 Cambios clave realizados:

| Original (Inglés)       | Traducción (Español)         | Notas |
|--------------------------|------------------------------|-------|
| `Cinema`                 | `Cine`                       | Nombre más natural en español |
| `cinema` (campo)         | *Eliminado*                  | Django ya crea `id` automáticamente. Si lo necesitas explícito, puedes usar `id_cine`. |
| `role`                   | `rol`                        | Se mantiene, pero valor por defecto traducido a `'gerente_cine'` |
| `cinema_name`            | `nombre_cine`                | Más claro y descriptivo |
| `phoneno`                | `telefono`                   | Nombre común en español |
| `city`                   | `ciudad`                     | — |
| `address`                | `direccion`                  | — |
| `user`                   | `usuario`                    | — |
| `Movie`                  | `Pelicula`                   | — |
| `movie_name`             | `nombre_pelicula`            | — |
| `movie_trailer`          | `trailer`                    | Comúnmente usado tal cual, pero en español también se entiende |
| `movie_rdate`            | `fecha_lanzamiento`          | “rdate” = release date → fecha de lanzamiento |
| `movie_des`              | `descripcion`                | Abreviatura expandida |
| `movie_rating`           | `calificacion`               | — |
| `movie_poster`           | `poster`                     | Término ampliamente aceptado |
| `movie_genre`            | `genero`                     | — |
| `movie_duration`         | `duracion`                   | — |
| `Shows`                  | `Funcion`                    | En español, “función” es el término correcto para una proyección de cine |
| `shows` (campo)          | *Eliminado*                  | Django crea `id` automáticamente |
| `cinema` (FK)            | `cine`                       | — |
| `movie` (FK)             | `pelicula`                   | — |
| `time`                   | `hora`                       | — |
| `date`                   | `fecha`                      | — |
| `price`                  | `precio`                     | — |
| `Bookings`               | `Reserva`                    | Singular, más natural en Django (aunque plural también se usa) |
| `user`                   | `usuario`                    | — |
| `shows` (FK)             | `funcion`                    | — |
| `useat`                  | `asientos`                   | Abreviatura expandida y en plural (porque suele contener varios) |
| `useat_as_list`          | `asientos_lista`             | Propiedad renombrada para claridad |

---

### 💡 Recomendaciones adicionales:

1. **Cambiar `CharField` por `DateField`** en `fecha_lanzamiento` y `fecha` si vas a manejar fechas reales (más eficiente y permite validaciones).
2. Considera usar `DecimalField` para `precio` si manejas decimales (por ejemplo, 9.99).
3. El valor por defecto `"null"` no es lo ideal; considera usar cadenas vacías o `None` con `null=True, blank=True`.

---

✅ Este modelo ya está completamente adaptado al español, con nombres claros, coherentes y naturales para una base de datos de gestión de cines en español.

¿Quieres que también te ayude a traducir los `verbose_name`, `help_text`, o los mensajes de validación? ¡Con gusto lo hago!
