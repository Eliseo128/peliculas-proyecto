¡Perfecto! Vamos a **traducir al español** los nombres de clases y atributos manteniendo buenas prácticas de Django (nombres de clase en singular y PascalCase, campos en minúsculas con snake\_case).
Aquí tienes el código adaptado:

```python
from django.db import models


# --- Estado de Vuelo ---
class Estado(models.Model):
    codigo_pnr = models.CharField(max_length=10, primary_key=True, verbose_name="Código PNR")
    estado_embarque = models.CharField(max_length=10, verbose_name="Estado de Embarque")
    descripcion = models.CharField(max_length=10, verbose_name="Descripción")

    def __str__(self):
        return f"{self.codigo_pnr} - {self.estado_embarque}"


# --- Pasajero ---
class Pasajero(models.Model):
    nombre = models.CharField(max_length=20, verbose_name="Nombre")
    estado = models.ForeignKey('Estado', on_delete=models.CASCADE, verbose_name="Estado")
    vuelo_id = models.CharField(max_length=5, verbose_name="ID de Vuelo")
    origen = models.CharField(max_length=4, verbose_name="Origen")
    destino = models.CharField(max_length=4, verbose_name="Destino")

    def __str__(self):
        return self.nombre


# --- Mercancía ---
class Mercancia(models.Model):
    articulo = models.CharField(max_length=20, verbose_name="Artículo")
    estado = models.ForeignKey('Estado', on_delete=models.CASCADE, verbose_name="Estado")
    vuelo_id = models.CharField(max_length=5, verbose_name="ID de Vuelo")
    origen = models.CharField(max_length=4, verbose_name="Origen")
    destino = models.CharField(max_length=4, verbose_name="Destino")

    def __str__(self):
        return self.articulo


# --- Vuelo en Vivo ---
class VueloEnVivo(models.Model):
    vuelo = models.ForeignKey('Vuelo', on_delete=models.CASCADE, verbose_name="Vuelo")
    estado = models.CharField(max_length=30, verbose_name="Estado")

    def __str__(self):
        return f"Vuelo {self.vuelo} - {self.estado}"


# --- Vuelo ---
class Vuelo(models.Model):
    vuelo_id = models.CharField(max_length=5, primary_key=True, verbose_name="ID de Vuelo")
    origen = models.CharField(max_length=4, verbose_name="Origen")
    destino = models.CharField(max_length=4, verbose_name="Destino")
    lunes = models.BooleanField(verbose_name="Lunes")
    martes = models.BooleanField(verbose_name="Martes")
    miercoles = models.BooleanField(verbose_name="Miércoles")
    jueves = models.BooleanField(verbose_name="Jueves")
    viernes = models.BooleanField(verbose_name="Viernes")
    sabado = models.BooleanField(verbose_name="Sábado")
    domingo = models.BooleanField(verbose_name="Domingo")
    capacidad_pasajeros = models.IntegerField(verbose_name="Capacidad de Pasajeros")

    def __str__(self):
        return f"Vuelo {self.vuelo_id}"


# --- Avión ---
class Avion(models.Model):
    avion_id = models.CharField(max_length=10, primary_key=True, verbose_name="ID de Avión")
    fabricante = models.CharField(max_length=15, verbose_name="Fabricante")
    modelo = models.CharField(max_length=20, verbose_name="Modelo")
    capacidad_pasajeros = models.IntegerField(verbose_name="Capacidad de Pasajeros")

    def __str__(self):
        return f"{self.fabricante} {self.modelo}"


# --- Horario ---
class Horario(models.Model):
    vuelo = models.ForeignKey('Vuelo', on_delete=models.CASCADE, verbose_name="Vuelo")
    origen = models.CharField(max_length=4, verbose_name="Origen")
    destino = models.CharField(max_length=4, verbose_name="Destino")
    llegada = models.TimeField(verbose_name="Hora de Llegada")
    salida = models.TimeField(verbose_name="Hora de Salida")

    class Meta:
        unique_together = (('vuelo', 'origen'),)

    def __str__(self):
        return f"{self.vuelo} - {self.origen} → {self.destino}"
```

---

### 🔍 Cambios que hice:

✅ Traducción de **clases** al español: `Status → Estado`, `Passenger → Pasajero`, `Goods → Mercancia`, etc.
✅ Traducción de **campos**: `flightid → vuelo_id`, `source → origen`, `destination → destino`, etc.
✅ Añadí `verbose_name` para que en el **admin de Django** los nombres sean legibles.
✅ `__str__` en cada clase para facilitar la visualización.
✅ `unique_together` adaptado al español.

---

¿Quieres que también te haga **un diagrama ERD** con estos modelos en español para visualizar relaciones?
