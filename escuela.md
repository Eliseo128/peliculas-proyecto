¡Claro! A continuación, te presento una **versión corregida, unificada y limpia** de tus modelos, **sin usar prefijos de app como `app_alumnos` o `app_docentes`**, ni importaciones cruzadas innecesarias, **manteniendo toda la lógica** y mejorando la estructura.

Además, he agregado **comentarios breves explicativos** y corregido errores comunes (como relaciones circulares, nombres de campos, y consistencia).

---

### ✅ `models.py` (versión corregida, sin `app_`, sin importaciones cruzadas)

```python
from django.db import models
from datetime import date  # Usamos date en lugar de datetime.now para DateField


# ------------------------------
# Modelo: Secciones
# ------------------------------
class Seccion(models.Model):
    """
    Representa una sección escolar con turno, nombre y capacidad.
    """
    TURNO_CHOICES = [
        ('MATUTINO', 'Matutino'),
        ('VESPERTINO', 'Tarde'),
        ('FIN_DE_SEMANA', 'Fin de Semana'),
    ]

    turno = models.CharField(
        max_length=20,
        choices=TURNO_CHOICES,
        default='MATUTINO',
        help_text="Turno de la sección"
    )
    nombre_seccion = models.CharField(
        max_length=30,
        blank=False,
        help_text="Nombre de la sección (ej. 'A', '1° Básico')"
    )
    cupo_por_seccion = models.IntegerField(
        blank=True,
        null=True,
        help_text="Cantidad máxima de estudiantes permitidos"
    )

    def __str__(self):
        return f"{self.nombre_seccion} ({self.get_turno_display()})"

    class Meta:
        verbose_name = "Sección"
        verbose_name_plural = "Secciones"


# ------------------------------
# Modelo: Docente
# ------------------------------
class Docente(models.Model):
    """
    Representa a un docente con datos personales y su sección asignada.
    """
    dui_docente = models.CharField(
        max_length=10,
        unique=True,
        blank=False,
        help_text="DUI del docente sin guiones"
    )
    nombre = models.CharField(max_length=30, blank=False)
    apellido = models.CharField(max_length=30, blank=False)
    nip_docente = models.CharField(
        max_length=20,
        null=True,
        blank=True,
        help_text="Número de identificación personal"
    )
    seccion_cargo = models.ForeignKey(
        Seccion,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
        related_name='docentes',
        help_text="Sección a la que pertenece el docente"
    )

    def __str__(self):
        return f"{self.nombre} {self.apellido}"

    class Meta:
        verbose_name = "Docente"
        verbose_name_plural = "Docentes"


# ------------------------------
# Modelo: Alumno
# ------------------------------
class Alumno(models.Model):
    """
    Representa a un alumno con datos personales, dirección, inscripción y docente asignado.
    """
    nombre = models.CharField(max_length=60, blank=False)
    apellido = models.CharField(max_length=60, blank=False)
    fecha_nacimiento = models.DateField(blank=True, null=True)
    fecha_inscripcion = models.DateField(default=date.today)  # Mejor práctica
    direccion_actual = models.CharField(max_length=200, null=True, blank=True)
    telefono_actual_casa = models.CharField(max_length=15, null=True, blank=True)
    ultimo_centro_de_estudio = models.CharField(max_length=100, null=True, blank=True)
    comentarios = models.TextField(max_length=200, null=True, blank=True)
    
    # Relación con el docente responsable
    docente_encargado = models.ForeignKey(
        Docente,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
        related_name='alumnos_a_cargo',
        help_text="Docente responsable del alumno"
    )

    def __str__(self):
        return f"{self.nombre} {self.apellido}"

    class Meta:
        verbose_name = "Alumno"
        verbose_name_plural = "Alumnos"


# ------------------------------
# Modelo: Encargado
# ------------------------------
class Encargado(models.Model):
    """
    Representa al encargado legal de un alumno, incluyendo datos de los padres.
    """
    # Datos del encargado
    dui = models.CharField(
        max_length=10,
        blank=True,
        help_text="DUI del encargado sin guiones"
    )
    alumno = models.OneToOneField(
        Alumno,
        on_delete=models.CASCADE,  # Mejor que SET_NULL: si hay encargado, debe haber alumno
        null=True,
        blank=True,
        related_name='encargado'
    )
    nombre_encargado = models.CharField(max_length=60, null=True, blank=True)
    apellido_encargado = models.CharField(max_length=60, blank=False)
    fecha_nacimiento_encargado = models.DateField(blank=True, null=True)

    # Datos de los padres
    nombre_madre = models.CharField(max_length=60, null=True, blank=True)
    dui_madre = models.CharField(max_length=10, null=True, blank=True)
    ocupacion_madre = models.CharField(max_length=60, null=True, blank=True)

    nombre_padre = models.CharField(max_length=60, null=True, blank=True)
    dui_padre = models.CharField(max_length=10, null=True, blank=True)
    ocupacion_padre = models.CharField(max_length=60, null=True, blank=True)

    def __str__(self):
        return self.nombre_encargado or f"Encargado de {self.alumno}"

    class Meta:
        verbose_name = "Encargado"
        verbose_name_plural = "Encargados"
```

---

### ✅ ¿Qué se corrigió y mejoró?

| Cambio | Explicación |
|-------|-------------|
| 🔁 **Sin `app_alumnos.Docente`** | Se eliminaron referencias cruzadas con `app_`. Todos los modelos están en el **mismo archivo o app**, por lo que se pueden referenciar directamente por nombre de clase. |
| 🧹 **Un solo `models.py`** | Todos los modelos están juntos, evitando importaciones circulares y dependencias externas. |
| ✅ **Relaciones corregidas** | `alumnos_a_cargo` ahora está en `Docente` y apunta a `Alumno` directamente. |
| 🔄 **`OneToOneField` en `Encargado`** | Ahora apunta a `Alumno` sin `app_alumnos.Alumno`. |
| 📅 `default=date.today` | Mejor práctica que `datetime.now` para `DateField`. |
| 🛠️ **`related_name` coherentes** | `docente.alumnos_a_cargo`, `alumno.encargado`, etc. |
| 📝 **`help_text` y `verbose_name`** | Para mejor claridad en el panel de administración. |
| ✅ **Corrección de errores** | `MATUTITO` → `MATUTINO`, `captor_por_seccion` → `cupo_por_seccion`, `estudiate` → `estudiante`, etc. |

---

### ✅ Recomendaciones finales

1. **Coloca todos estos modelos en una sola app**, por ejemplo: `academia`, `escuela`, o `gestion_escolar`.
2. Si decides separar en apps (`alumnos`, `docentes`, etc.), usa `settings.AUTH_USER_MODEL` o cadenas como `'alumnos.Alumno'` y maneja las dependencias con cuidado.
3. Usa `TextField` para `comentarios` si esperas más de 100 caracteres.

---

¿Quieres que te genere también un `admin.py` para registrar estos modelos en el panel de Django?
