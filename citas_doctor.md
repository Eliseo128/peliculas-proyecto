¡Perfecto! A continuación, te presento una versión **completa y completamente traducida al español** del archivo `models.py`, incluyendo:

- ✅ **Nombres de clases, campos y opciones en español**
- ✅ **Comentarios explicativos en español**
- ✅ **Traducción de `GENDER_CHOICES`, `department` y otros campos**
- ✅ **Estructura limpia y clara**

---

### ✅ `models.py` – Versión en español

```python
from django.db import models
from django.urls import reverse
from django.utils import timezone
from django.contrib.auth.models import AbstractUser  # Asumiendo que User hereda de AbstractUser
from accounts.managers import UserManager  # Gestor personalizado

# ================== OPCIONES DE GÉNERO ==================
OPCIONES_GENERO = (
    ('masculino', 'Masculino'),
    ('femenino', 'Femenino'),
)

# ================== OPCIONES DE DEPARTAMENTO MÉDICO ==================
OPCIONES_DEPARTAMENTO = (
    ('Odontología', 'Odontología'),
    ('Cardiología', 'Cardiología'),
    ('Otorrinolaringología', 'Otorrinolaringología'),
    ('Astrología', 'Astrología'),  # Nota: posiblemente error, debería ser "Neurología" o similar
    ('Neuroanatomía', 'Neuroanatomía'),
    ('Análisis de Sangre', 'Análisis de Sangre'),
    ('Cuidado Visual', 'Cuidado Visual'),
    ('Terapia Física', 'Terapia Física'),
)


# ================== MODELO DE USUARIO PERSONALIZADO ==================
class Usuario(AbstractUser):
    """
    Modelo personalizado de usuario que utiliza el correo como nombre de inicio de sesión.
    Incluye rol, género, correo y número de teléfono.
    """
    # Desactivamos el campo username
    username = None

    # Rol del usuario (por ejemplo: paciente, médico, admin)
    rol = models.CharField(
        max_length=12,
        error_messages={
            'required': "El rol debe ser proporcionado."
        }
    )

    # Género del usuario (opcional)
    genero = models.CharField(
        max_length=10,
        blank=True,
        null=True,
        default="",
        choices=OPCIONES_GENERO,
        verbose_name="Género"
    )

    # Correo electrónico único (usado como identificador principal)
    correo = models.EmailField(
        unique=True,
        blank=False,
        error_messages={
            'unique': "Ya existe un usuario con este correo electrónico."
        }
    )

    # Número de teléfono único (opcional)
    telefono = models.CharField(
        max_length=20,
        unique=True,
        blank=True,
        null=True,
        error_messages={
            'unique': "Ya existe un usuario con este número de teléfono."
        }
    )

    # Configuración de autenticación
    USERNAME_FIELD = "correo"  # Se usa el correo para iniciar sesión
    REQUIRED_FIELDS = []      # No se requieren otros campos al crear superusuario

    # Gestor personalizado para manejar creación de usuarios
    objects = UserManager()

    def __str__(self):
        """
        Representación en texto del usuario (se muestra el correo).
        """
        return self.correo


# ================== MODELO DE CITA MÉDICA (DISPONIBLE) ==================
class Cita(models.Model):
    """
    Representa una cita médica disponible que un profesional ofrece.
    Puede ser reservada por un paciente.
    """
    # Usuario al que pertenece la cita (médico o profesional)
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE, verbose_name="Usuario")

    # Nombre completo del profesional
    nombre_completo = models.CharField(max_length=100, verbose_name="Nombre completo")

    # Foto del profesional (opcional)
    imagen = models.ImageField(upload_to='doctores/', null=True, blank=True, verbose_name="Imagen")

    # Ubicación donde atiende
    ubicacion = models.CharField(max_length=100, verbose_name="Ubicación")

    # Hora de inicio de la cita
    hora_inicio = models.CharField(max_length=10, verbose_name="Hora de inicio")

    # Hora de fin de la cita
    hora_fin = models.CharField(max_length=10, verbose_name="Hora de fin")

    # Título académico (ej: Dr. en Medicina)
    titulo_academico = models.CharField(max_length=100, verbose_name="Título académico")

    # Institución donde estudió
    institucion = models.CharField(max_length=100, verbose_name="Institución")

    # Nombre del hospital o clínica
    hospital = models.CharField(max_length=100, verbose_name="Hospital")

    # Departamento médico al que pertenece
    departamento = models.CharField(
        max_length=100,
        choices=OPCIONES_DEPARTAMENTO,
        verbose_name="Departamento"
    )

    # Fecha de creación de la cita
    creado_en = models.DateTimeField(default=timezone.now, verbose_name="Creado el")

    def __str__(self):
        """
        Representación en texto de la cita.
        """
        return self.nombre_completo

    # def get_absolute_url(self):
    #     return reverse('cita:eliminar-cita', kwargs={'pk': self.pk})


# ================== MODELO DE RESERVA DE CITA ==================
class ReservaCita(models.Model):
    """
    Representa una reserva realizada por un paciente para una cita específica.
    Incluye mensaje, teléfono y fecha de solicitud.
    """
    # Usuario que realiza la reserva (paciente)
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE, verbose_name="Usuario")

    # Cita a la que se está aplicando
    cita = models.ForeignKey(Cita, on_delete=models.CASCADE, verbose_name="Cita")

    # Nombre completo del paciente
    nombre_completo = models.CharField(max_length=100, verbose_name="Nombre completo")

    # Mensaje opcional del paciente
    mensaje = models.TextField(verbose_name="Mensaje")

    # Teléfono de contacto del paciente
    telefono = models.CharField(max_length=20, verbose_name="Teléfono")

    # Fecha y hora de la solicitud
    fecha = models.DateTimeField(default=timezone.now, verbose_name="Fecha de solicitud")

    def __str__(self):
        """
        Representación en texto de la reserva.
        """
        return self.nombre_completo


# Meta opcional para configuraciones adicionales
# Puedes agregar verbose_name_plural si usas Django Admin
# class Meta:
#     verbose_name = "Reserva de Cita"
#     verbose_name_plural = "Reservas de Cita"
```

---

### ✅ Cambios realizados

| Original (Inglés)       | Traducido (Español)         | Nota |
|-------------------------|-----------------------------|------|
| `User` → `Usuario`       | ✔️ | Modelo principal |
| `Appointment` → `Cita`   | ✔️ | Cita disponible |
| `TakeAppointment` → `ReservaCita` | ✔️ | Mejor que "Tomar Cita" |
| `gender` → `genero`      | ✔️ | Con opciones traducidas |
| `department` → `departamento` | ✔️ | Áreas médicas en español |
| `full_name` → `nombre_completo` | ✔️ | |
| `start_time` → `hora_inicio` | ✔️ | Más claro |
| `created_at` → `creado_en` | ✔️ | |

---

### 📌 Notas importantes

1. **`upload_to='doctores/'`**: Se añadió una carpeta lógica para guardar imágenes de médicos. Puedes cambiarla según tu estructura.

2. **Astrología**: Aunque está en la lista, parece fuera de lugar en un sistema médico. Podría ser un error por "Neurología" o "Psiquiatría". Si es un error, puedes corregirlo en `OPCIONES_DEPARTAMENTO`.

3. **Django Admin**: Si usas el panel de administración, puedes agregar `verbose_name` y `verbose_name_plural` en `class Meta` para que se muestre todo en español.

4. **Migraciones**: Después de este cambio, si ya tenías datos, considera usar `db_column` para mantener compatibilidad con los nombres en inglés en la base de datos.

---

### ✅ Ejemplo con `db_column` (opcional, para compatibilidad)

Si no puedes cambiar los nombres en la base de datos, usa:

```python
correo = models.EmailField(unique=True, db_column='email')
telefono = models.CharField(max_length=20, db_column='phone_number')
```

Esto mantiene el campo `email` en la BD, pero en el código usas `correo`.

---

¿Quieres que te genere también el `admin.py` traducido o un `forms.py` en español para este modelo?
