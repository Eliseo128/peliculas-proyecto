Aquí tienes el archivo `models.py` con **todos los nombres de clases, atributos, opciones de elección (choices) y comentarios traducidos al español**, manteniendo la funcionalidad original del código:

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from datetime import datetime

# Modelos de la aplicación

class Usuario(AbstractUser):
    def __str__(self):
        return f"{self.id}: {self.nombre} {self.apellido}"

    @property
    def nombre(self):
        return self.first_name

    @property
    def apellido(self):
        return self.last_name


class Lugar(models.Model):
    ciudad = models.CharField(max_length=64)
    aeropuerto = models.CharField(max_length=64)
    codigo = models.CharField(max_length=3)
    pais = models.CharField(max_length=64)

    def __str__(self):
        return f"{self.ciudad}, {self.pais} ({self.codigo})"


class Semana(models.Model):
    numero = models.IntegerField()
    nombre = models.CharField(max_length=16)

    def __str__(self):
        return f"{self.nombre} ({self.numero})"


class Vuelo(models.Model):
    origen = models.ForeignKey(Lugar, on_delete=models.CASCADE, related_name="salidas")
    destino = models.ForeignKey(Lugar, on_delete=models.CASCADE, related_name="llegadas")
    hora_salida = models.TimeField(auto_now=False, auto_now_add=False)
    dia_salida = models.ManyToManyField(Semana, related_name="vuelos_del_dia")
    duracion = models.DurationField(null=True)
    hora_llegada = models.TimeField(auto_now=False, auto_now_add=False)
    avion = models.CharField(max_length=24)
    aerolinea = models.CharField(max_length=64)
    tarifa_economica = models.FloatField(null=True)
    tarifa_ejecutiva = models.FloatField(null=True)
    tarifa_primera = models.FloatField(null=True)

    def __str__(self):
        return f"{self.id}: {self.origen} a {self.destino}"


GENERO = (
    ('masculino', 'Masculino'),
    ('femenino', 'Femenino')
)


class Pasajero(models.Model):
    nombre = models.CharField(max_length=64, blank=True)
    apellido = models.CharField(max_length=64, blank=True)
    genero = models.CharField(max_length=20, choices=GENERO, blank=True)

    def __str__(self):
        return f"Pasajero: {self.nombre} {self.apellido}, {self.genero}"


CLASE_ASIENTO = (
    ('economica', 'Económica'),
    ('ejecutiva', 'Ejecutiva'),
    ('primera', 'Primera')
)

ESTADO_TICKET = (
    ('PENDIENTE', 'Pendiente'),
    ('CONFIRMADO', 'Confirmado'),
    ('CANCELADO', 'Cancelado')
)


class Ticket(models.Model):
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE, related_name="reservas", blank=True, null=True)
    numero_referencia = models.CharField(max_length=6, unique=True)
    pasajeros = models.ManyToManyField(Pasajero, related_name="tickets_de_vuelo")
    vuelo = models.ForeignKey(Vuelo, on_delete=models.CASCADE, related_name="tickets", blank=True, null=True)
    fecha_salida = models.DateField(blank=True, null=True)
    fecha_llegada = models.DateField(blank=True, null=True)
    tarifa_vuelo = models.FloatField(blank=True, null=True)
    cargos_adicionales = models.FloatField(blank=True, null=True)
    cupon_usado = models.CharField(max_length=15, blank=True)
    descuento_cupon = models.FloatField(default=0.0)
    tarifa_total = models.FloatField(blank=True, null=True)
    clase_asiento = models.CharField(max_length=20, choices=CLASE_ASIENTO)
    fecha_reserva = models.DateTimeField(default=datetime.now)
    telefono = models.CharField(max_length=20, blank=True)
    correo = models.EmailField(max_length=45, blank=True)
    estado = models.CharField(max_length=45, choices=ESTADO_TICKET)

    def __str__(self):
        return self.numero_referencia
```

---

### ✅ Cambios realizados:

#### 🔹 Nombres de clases:
- `User` → `Usuario`
- `Place` → `Lugar`
- `Week` → `Semana`
- `Flight` → `Vuelo`
- `Passenger` → `Pasajero`
- `Ticket` → `Ticket` (se mantiene igual por ser término común, pero puedes cambiarlo a `Boleto` si prefieres)

#### 🔹 Atributos y campos:
Todos los campos (`origin`, `destination`, etc.) fueron traducidos a español:
- `city` → `ciudad`
- `airport` → `aeropuerto`
- `code` → `codigo`
- `country` → `pais`
- `depart_time` → `hora_salida`
- `depart_day` → `dia_salida`
- `duration` → `duracion`
- `arrival_time` → `hora_llegada`
- `plane` → `avion`
- `airline` → `aerolinea`
- `economy_fare` → `tarifa_economica`
- `business_fare` → `tarifa_ejecutiva`
- `first_fare` → `tarifa_primera`
- `first_name`, `last_name` → `nombre`, `apellido`
- `gender` → `genero`
- `ref_no` → `numero_referencia`
- `passengers` → `pasajeros`
- `flight` → `vuelo`
- `flight_ddate` → `fecha_salida`
- `flight_adate` → `fecha_llegada`
- `flight_fare` → `tarifa_vuelo`
- `other_charges` → `cargos_adicionales`
- `coupon_used` → `cupon_usado`
- `coupon_discount` → `descuento_cupon`
- `total_fare` → `tarifa_total`
- `seat_class` → `clase_asiento`
- `booking_date` → `fecha_reserva`
- `mobile` → `telefono`
- `email` → `correo`
- `status` → `estado`

#### 🔹 Constantes:
- `GENDER` → `GENERO` con valores en español
- `SEAT_CLASS` → `CLASE_ASIENTO` con nombres traducidos
- `TICKET_STATUS` → `ESTADO_TICKET` en español

---

### 📝 Notas:
- Si deseas que `Ticket` también se llame `Boleto`, puedes cambiar `Ticket` → `Boleto` y `tickets` → `boletos` en relaciones.
- Se agregaron `@property` para mantener compatibilidad con `first_name` y `last_name` si se usan en otros lugares.
- Asegúrate de hacer migraciones después de estos cambios si ya tenías datos:  
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```

¿Quieres que también traduzca `Boleto` en vez de `Ticket`? Puedo ajustarlo.


Perfecto, aquí tienes el archivo `models.py` actualizado con **"Boleto" en lugar de "Ticket"** y todos los nombres traducidos al español, incluyendo clases, atributos, constantes y relaciones. También se ajustaron los nombres de las relaciones (`related_name`) para que sean coherentes en español.

---

### ✅ `models.py` (versión completamente en español)

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from datetime import datetime

# Modelos de la aplicación

class Usuario(AbstractUser):
    def __str__(self):
        return f"{self.id}: {self.nombre} {self.apellido}"

    @property
    def nombre(self):
        return self.first_name

    @property
    def apellido(self):
        return self.last_name


class Lugar(models.Model):
    ciudad = models.CharField(max_length=64)
    aeropuerto = models.CharField(max_length=64)
    codigo = models.CharField(max_length=3)
    pais = models.CharField(max_length=64)

    def __str__(self):
        return f"{self.ciudad}, {self.pais} ({self.codigo})"


class Semana(models.Model):
    numero = models.IntegerField()
    nombre = models.CharField(max_length=16)

    def __str__(self):
        return f"{self.nombre} ({self.numero})"


class Vuelo(models.Model):
    origen = models.ForeignKey(Lugar, on_delete=models.CASCADE, related_name="salidas")
    destino = models.ForeignKey(Lugar, on_delete=models.CASCADE, related_name="llegadas")
    hora_salida = models.TimeField(auto_now=False, auto_now_add=False)
    dia_salida = models.ManyToManyField(Semana, related_name="vuelos_del_dia")
    duracion = models.DurationField(null=True)
    hora_llegada = models.TimeField(auto_now=False, auto_now_add=False)
    avion = models.CharField(max_length=24)
    aerolinea = models.CharField(max_length=64)
    tarifa_economica = models.FloatField(null=True)
    tarifa_ejecutiva = models.FloatField(null=True)
    tarifa_primera = models.FloatField(null=True)

    def __str__(self):
        return f"{self.id}: {self.origen} a {self.destino}"


GENERO = (
    ('masculino', 'Masculino'),
    ('femenino', 'Femenino')
)


class Pasajero(models.Model):
    nombre = models.CharField(max_length=64, blank=True)
    apellido = models.CharField(max_length=64, blank=True)
    genero = models.CharField(max_length=20, choices=GENERO, blank=True)

    def __str__(self):
        return f"Pasajero: {self.nombre} {self.apellido}, {self.genero}"


CLASE_ASIENTO = (
    ('economica', 'Económica'),
    ('ejecutiva', 'Ejecutiva'),
    ('primera', 'Primera')
)

ESTADO_BOLETO = (
    ('PENDIENTE', 'Pendiente'),
    ('CONFIRMADO', 'Confirmado'),
    ('CANCELADO', 'Cancelado')
)


class Boleto(models.Model):
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE, related_name="boletos", blank=True, null=True)
    numero_referencia = models.CharField(max_length=6, unique=True)
    pasajeros = models.ManyToManyField(Pasajero, related_name="boletos_vuelo")
    vuelo = models.ForeignKey(Vuelo, on_delete=models.CASCADE, related_name="boletos", blank=True, null=True)
    fecha_salida = models.DateField(blank=True, null=True)
    fecha_llegada = models.DateField(blank=True, null=True)
    tarifa_vuelo = models.FloatField(blank=True, null=True)
    cargos_adicionales = models.FloatField(blank=True, null=True)
    cupon_usado = models.CharField(max_length=15, blank=True)
    descuento_cupon = models.FloatField(default=0.0)
    tarifa_total = models.FloatField(blank=True, null=True)
    clase_asiento = models.CharField(max_length=20, choices=CLASE_ASIENTO)
    fecha_reserva = models.DateTimeField(default=datetime.now)
    telefono = models.CharField(max_length=20, blank=True)
    correo = models.EmailField(max_length=45, blank=True)
    estado = models.CharField(max_length=45, choices=ESTADO_BOLETO)

    def __str__(self):
        return self.numero_referencia
```

---

### 🔁 Cambios clave realizados:

| Antes | Después |
|------|---------|
| `Ticket` | `Boleto` |
| `TICKET_STATUS` | `ESTADO_BOLETO` |
| `related_name="bookings"` | `related_name="boletos"` |
| `related_name="flight_tickets"` | `related_name="boletos_vuelo"` |
| `related_name="tickets"` (en Vuelo) | `related_name="boletos"` |

---

### 📝 Recomendaciones:

1. **Migraciones**: Si ya tienes una base de datos, este cambio de nombre de modelo implica que debes crear una migración para renombrar la tabla en la base de datos. Puedes hacerlo manualmente o con `makemigrations` y luego editar la migración para usar `RenameModel`.

   Ejemplo en migración:
   ```python
   operations = [
       migrations.RenameModel(
           old_name='Ticket',
           new_name='Boleto',
       ),
   ]
   ```

2. **Actualiza vistas, formularios y plantillas**: Asegúrate de cambiar cualquier referencia a `Ticket` por `Boleto` en tus vistas, formularios, URLs y templates.

3. **Traducción consistente**: Si usas internacionalización (`i18n`), considera mantener los nombres en inglés en el código y usar traducciones en las plantillas. Pero si prefieres todo en español (como en este caso), esta versión es perfecta.

---

¿Quieres que genere también la migración de renombrado o que traduzca los `verbose_name` en español para el panel de administración?
