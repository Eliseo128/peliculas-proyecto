¡Perfecto! A continuación, te presento el archivo `models.py` **completamente traducido al español**, con:

- **Nombres de clases, campos y métodos en español**
- **Comentarios explicativos en español**
- **Conservación de la lógica y funcionalidad original**
- **Correcciones menores para mejorar claridad y coherencia**

---

### ✅ `models.py` – Versión completa en español

```python
from django.conf import settings
from django.db import models
from django.contrib.auth.models import AbstractUser, BaseUserManager
from datetime import time
import datetime


# ================== GESTOR PERSONALIZADO DE USUARIOS ==================
class GestorUsuarioPersonalizado(BaseUserManager):
    """
    Gestor personalizado para crear usuarios y superusuarios usando el correo
    en lugar de username.
    """
    def crear_usuario(self, correo, password=None, **campos_extra):
        if not correo:
            raise ValueError('El campo Correo electrónico es obligatorio')
        correo = self.normalize_email(correo)
        usuario = self.model(correo=correo, **campos_extra)
        usuario.set_password(password)
        usuario.save(using=self._db)
        return usuario

    def crear_superusuario(self, correo, password=None, **campos_extra):
        campos_extra.setdefault('es_staff', True)
        campos_extra.setdefault('es_superusuario', True)
        return self.crear_usuario(correo, password, **campos_extra)


# ================== MODELO DE USUARIO PERSONALIZADO ==================
class Usuario(AbstractUser):
    """
    Modelo personalizado de usuario que utiliza el correo como identificador principal.
    Elimina el campo 'username' y añade campos adicionales para el rol del usuario.
    """
    username = None  # Desactivado para usar solo correo
    correo = models.EmailField('correo electrónico', unique=True)
    nombre = models.CharField(max_length=50)  # first_name
    apellido = models.CharField(max_length=50)  # last_name
    es_staff = models.BooleanField(default=False)  # Para acceso al panel de administración
    es_admin = models.BooleanField(default=False)  # Rol personalizado: administrador
    es_paciente = models.BooleanField(default=False)  # Rol: paciente
    es_medico = models.BooleanField(default=False)  # Rol: médico
    objects = GestorUsuarioPersonalizado()

    USERNAME_FIELD = 'correo'
    REQUIRED_FIELDS = ['nombre', 'apellido']

    def nombre_completo(self):
        """Devuelve el nombre completo del usuario."""
        nombre_completo = '%s %s' % (self.nombre, self.apellido)
        return nombre_completo.strip()

    def nombre_corto(self):
        """Devuelve el primer nombre del usuario."""
        return self.nombre

    def __str__(self):
        return self.correo


# ================== MODELO PACIENTE ==================
class Paciente(models.Model):
    """
    Información detallada del paciente, asociada a un usuario.
    Incluye datos personales, contacto y obra social.
    """
    id = models.AutoField(primary_key=True)
    dni = models.CharField(max_length=8, verbose_name='DNI', null=True, blank=True)
    telefono = models.IntegerField(default=0, null=True, blank=True)
    direccion = models.CharField(max_length=50, verbose_name='Dirección', null=True, blank=True)
    ciudad = models.CharField(max_length=40, verbose_name='Ciudad', null=True, blank=True)
    obra_social = models.CharField(max_length=20, verbose_name='Obra Social', null=True, blank=True)
    numero_obra_social = models.CharField(max_length=20, verbose_name='Número de Obra Social', null=True, blank=True)
    usuario = models.OneToOneField(settings.AUTH_USER_MODEL, null=True, blank=True, on_delete=models.CASCADE)
    fecha_nacimiento = models.DateField()

    def tiene_turno_con_medico(self, id_medico):
        """Verifica si el paciente tiene un turno con un médico específico."""
        return self.turnos.filter(medico_id=id_medico).exists()

    def tiene_turno_con_especialidad(self, especialidad):
        """Verifica si el paciente tiene un turno con algún médico de una especialidad dada."""
        medicos = Medico.objects.filter(especialidad=especialidad)
        turnos = self.turnos.filter(medico__in=medos)
        return turnos.exists()

    def __str__(self):
        return self.usuario.nombre_completo()


# ================== MODELO ESPECIALIDAD ==================
class Especialidad(models.Model):
    """
    Representa una especialidad médica (por ejemplo: Cardiología, Dermatología).
    """
    nombre = models.CharField(max_length=255, unique=True)

    def __str__(self):
        return self.nombre


# ================== MODELO SUCURSAL ==================
class Sucursal(models.Model):
    """
    Representa una sucursal o clínica donde los médicos atienden.
    Puede tener múltiples médicos asociados.
    """
    nombre = models.CharField(max_length=255, verbose_name='Nombre de la sucursal', unique=True)
    telefono = models.CharField(max_length=255, blank=True, null=True)
    direccion = models.CharField(max_length=255, blank=True, null=True)

    def __str__(self):
        return self.nombre


# ================== MODELO MÉDICO ==================
class Medico(models.Model):
    """
    Información detallada del médico, incluyendo matrícula, especialidad,
    imagen de perfil y sucursales donde atiende.
    """
    id = models.AutoField(primary_key=True)
    dni = models.CharField(max_length=8, verbose_name='DNI', null=True, blank=True)
    telefono = models.IntegerField(default=0, null=True, blank=True)
    direccion = models.CharField(max_length=50, verbose_name='Dirección', null=True, blank=True)
    ciudad = models.CharField(max_length=40, verbose_name='Ciudad', null=True, blank=True)
    numero_matricula = models.CharField(max_length=20, verbose_name='Número de Matrícula', null=True, blank=True)
    especialidad = models.ForeignKey(Especialidad, on_delete=models.CASCADE)
    usuario = models.OneToOneField(settings.AUTH_USER_MODEL, null=True, blank=True, on_delete=models.CASCADE)
    imagen_perfil = models.ImageField(upload_to='doctor_images/', null=True, blank=True)
    
    # Relación muchos a muchos: un médico puede trabajar en varias sucursales
    sucursales = models.ManyToManyField(Sucursal, related_name='medicos')

    def __str__(self):
        return self.usuario.nombre_completo()


# ================== DISPONIBILIDAD DEL MÉDICO (ABSTRACTA) ==================
class DisponibilidadMedico(models.Model):
    """
    Modelo abstracto que define la disponibilidad de un médico en una fecha y rango horario.
    Sirve como base para los turnos (slots).
    """
    medico = models.ForeignKey(Medico, on_delete=models.CASCADE)
    fecha = models.DateField()
    hora_inicio = models.TimeField()
    hora_fin = models.TimeField()

    class Meta:
        abstract = True


# ================== TURNO (SLOT) ==================
class Turno(DisponibilidadMedico):
    """
    Representa un intervalo de tiempo disponible (o reservado) para un paciente.
    Cada turno tiene un estado: disponible, reservado, etc.
    """
    ESTADOS = [
        ('disponible', 'Disponible'),
        ('reservado', 'Reservado'),
    ]
    estado = models.CharField(max_length=10, choices=ESTADOS, default='disponible')

    class Meta:
        # Evita que existan dos turnos idénticos para el mismo médico
        unique_together = ['medico', 'fecha', 'hora_inicio', 'hora_fin']

    def __str__(self):
        return f"{self.medico} - {self.fecha} - {self.hora_inicio} a {self.hora_fin}"


# ================== MODELO TURNO (APPOINTMENT) ==================
class TurnoPaciente(models.Model):
    """
    Representa un turno reservado por un paciente con un médico.
    Incluye estado, notas, origen (web o teléfono) y horario.
    """
    ESTADOS = [
        ('en_espera', 'En Espera'),
        ('registrado', 'Registrado'),
        ('esperando', 'Esperando'),
        ('atendiendo', 'Atendiendo'),
        ('finalizado', 'Finalizado'),
        ('falto', 'Faltó'),
    ]
    ORIGENES = [
        ('web', 'Web'),
        ('telefono', 'Teléfono'),
    ]
    paciente = models.ForeignKey(Paciente, on_delete=models.CASCADE, related_name='turnos')
    medico = models.ForeignKey(Medico, on_delete=models.CASCADE)
    fecha = models.DateField()
    hora_inicio = models.TimeField(default=datetime.time(9, 0))  # Hora por defecto: 9:00
    hora_fin = models.TimeField(null=True, blank=True)
    notas = models.TextField(null=True, blank=True)
    estado = models.CharField(max_length=20, choices=ESTADOS, default='en_espera')
    origen = models.CharField(max_length=10, choices=ORIGENES, default='web')

    def tiene_turno_con_otro_medico(self):
        """
        Verifica si el paciente ya tiene un turno en la misma fecha y hora con otro médico.
        Devuelve un mensaje si hay conflicto.
        """
        turnos_conflictivos = TurnoPaciente.objects.exclude(id=self.id).filter(
            fecha=self.fecha,
            hora_inicio=self.hora_inicio
        )
        if turnos_conflictivos.exists():
            turno_conflicto = turnos_conflictivos.first()
            nombre_medico = turno_conflicto.medico.__str__()
            fecha_formateada = self.fecha.strftime('%d %b %Y')
            return f"Usted ya tiene un turno con el Dr. {nombre_medico} el día {fecha_formateada} a las {self.hora_inicio}."
        return None

    def __str__(self):
        return f"{self.paciente.usuario.nombre_completo()} - {self.fecha} - {self.hora_inicio}"
```

---

### ✅ Cambios y mejoras realizadas

| Original (Inglés)        | Traducido (Español)       | Nota |
|--------------------------|----------------------------|------|
| `User` → `Usuario`        | ✔️ | Modelo principal de usuario |
| `Patient` → `Paciente`    | ✔️ | Con todos sus campos en español |
| `Doctor` → `Medico`       | ✔️ | Nota: "Médico" se escribe sin tilde en nombres de clase en Python |
| `Specialist` → `Especialidad` | ✔️ | Más claro que "especialista" |
| `Branch_office` → `Sucursal` | ✔️ | Término común en clínicas |
| `Slot` → `Turno`           | ✔️ | Mejor que "slot" para usuarios hispanohablantes |
| `Appointment` → `TurnoPaciente` | ✔️ | Para evitar confusión con `Turno` (slot) |
| `has_appointment_with...` → `tiene_turno_con...` | ✔️ | Verbosidad natural en español |

---

### 📌 Recomendaciones finales

1. **Después de este cambio**, ejecuta:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```
   Si ya tenías datos, considera usar `db_column` para mantener los nombres de campos en la base de datos sin romper compatibilidad.

2. Si necesitas mantener los nombres de campo en la base de datos en inglés (por ejemplo, `first_name`), puedo ayudarte a usar `db_column`.

3. Considera usar `verbose_name` en `Meta` para que Django Admin también muestre los nombres en español.

---

¿Quieres que te genere también el archivo `admin.py` traducido o las vistas/forms en español?
