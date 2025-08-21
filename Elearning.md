¡Perfecto! A continuación, te presento el archivo `models.py` **completamente traducido al español**, con:

- ✅ **Nombres de clases, campos y métodos en español**
- ✅ **Comentarios explicativos en español**
- ✅ **Conservación de la funcionalidad original**
- ✅ **Mejoras menores de claridad y estilo**

---

### ✅ `models.py` – Versión en español

```python
from django.db import models
from django.db.models import Avg
from django.conf import settings
from django.contrib.contenttypes.models import ContentType
from django.contrib.contenttypes.fields import GenericForeignKey
from courses.fields import OrderField  # Campo personalizado para ordenar
from django.template.loader import render_to_string
from django.utils.safestring import mark_safe
from django.utils import timezone
from autoslug import AutoSlugField
import numpy as np


# ================== TEMA O ASIGNATURA ==================
class Tema(models.Model):
    """
    Representa un tema o área de conocimiento (por ejemplo: Matemáticas, Programación).
    Se utiliza para categorizar cursos.
    """
    titulo = models.CharField(max_length=200, verbose_name="Título")
    slug = models.SlugField(max_length=200, unique=True, verbose_name="Identificador URL")

    class Meta:
        verbose_name = "Tema"
        verbose_name_plural = "Temas"
        ordering = ('titulo',)  # Orden alfabético por título

    def __str__(self):
        return self.titulo


# ================== CURSO ==================
class Curso(models.Model):
    """
    Representa un curso creado por un instructor.
    Contiene módulos, contenido, estudiantes inscritos y reseñas.
    """
    propietario = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        related_name='cursos_creados',
        on_delete=models.CASCADE,
        verbose_name="Propietario"
    )
    tema = models.ForeignKey(
        Tema,
        related_name='cursos',
        on_delete=models.CASCADE,
        verbose_name="Tema"
    )
    titulo = models.CharField(max_length=200, verbose_name="Título")
    slug = AutoSlugField(
        populate_from='titulo',
        unique_with='creado__month',
        verbose_name="Identificador URL"
    )
    descripcion = models.TextField(verbose_name="Descripción")
    creado = models.DateTimeField(auto_now_add=True, verbose_name="Fecha de creación")
    estudiantes = models.ManyToManyField(
        settings.AUTH_USER_MODEL,
        related_name='cursos_inscritos',
        blank=True,
        verbose_name="Estudiantes"
    )

    class Meta:
        verbose_name = "Curso"
        verbose_name_plural = "Cursos"
        ordering = ('-creado',)  # Más recientes primero

    def calificacion_promedio(self):
        """
        Calcula la calificación promedio de todas las reseñas del curso.
        Devuelve None si no hay reseñas.
        """
        resultado = self.reseñas.aggregate(Avg('calificacion'))
        return resultado['calificacion__avg']

    def __str__(self):
        return self.titulo


# ================== MÓDULO ==================
class Modulo(models.Model):
    """
    Un curso se divide en módulos (por ejemplo: Introducción, Avanzado).
    Cada módulo contiene contenidos como textos, videos, etc.
    """
    curso = models.ForeignKey(Curso, related_name='modulos', on_delete=models.CASCADE, verbose_name="Curso")
    titulo = models.CharField(max_length=200, verbose_name="Título")
    descripcion = models.TextField(blank=True, verbose_name="Descripción")
    orden = OrderField(blank=True, for_fields=['curso'], verbose_name="Orden")

    class Meta:
        verbose_name = "Módulo"
        verbose_name_plural = "Módulos"
        ordering = ['orden']

    def __str__(self):
        return f'{self.orden}. {self.titulo}'


# ================== CONTENIDO BASE (ABSTRACTO) ==================
class ContenidoBase(models.Model):
    """
    Modelo base abstracto para diferentes tipos de contenido.
    Heredan: Texto, Imagen, Archivo, Video.
    """
    propietario = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        related_name='%(class)s_relacionado',
        on_delete=models.CASCADE,
        verbose_name="Propietario"
    )
    titulo = models.CharField(max_length=250, verbose_name="Título")
    creado = models.DateTimeField(auto_now_add=True, verbose_name="Creado")
    actualizado = models.DateTimeField(auto_now=True, verbose_name="Actualizado")

    class Meta:
        abstract = True
        verbose_name = "Contenido Base"
        verbose_name_plural = "Contenidos Base"

    def render(self):
        """
        Devuelve el HTML renderizado del contenido según su tipo.
        Usa una plantilla específica (text.html, video.html, etc.).
        """
        return render_to_string(f'courses/content/{self._meta.model_name}.html', {'item': self})

    def __str__(self):
        return self.titulo


# ================== TIPOS DE CONTENIDO ==================

class Texto(ContenidoBase):
    """
    Contenido de tipo texto (por ejemplo: apuntes, explicaciones).
    """
    contenido = models.TextField(verbose_name="Contenido")


class Archivo(ContenidoBase):
    """
    Contenido que es un archivo descargable (PDF, Word, etc.).
    """
    archivo = models.FileField(upload_to='files', verbose_name="Archivo")


class Imagen(ContenidoBase):
    """
    Contenido que es una imagen.
    """
    archivo = models.FileField(upload_to='images', verbose_name="Imagen")


class Video(ContenidoBase):
    """
    Contenido que es un video externo (por URL).
    """
    url = models.URLField(verbose_name="URL del Video")


# ================== RELACIÓN DE CONTENIDO CON MÓDULO ==================
class Contenido(models.Model):
    """
    Relaciona un módulo con un contenido específico (Texto, Imagen, etc.)
    Usa relaciones genéricas para permitir diferentes tipos de contenido.
    """
    modulo = models.ForeignKey(Modulo, related_name='contenidos', on_delete=models.CASCADE, verbose_name="Módulo")
    tipo_contenido = models.ForeignKey(
        ContentType,
        limit_choices_to={'model__in': ('texto', 'video', 'imagen', 'archivo')},
        on_delete=models.CASCADE,
        verbose_name="Tipo de Contenido"
    )
    objeto_id = models.PositiveIntegerField(verbose_name="ID del Objeto")
    item = GenericForeignKey('tipo_contenido', 'objeto_id')
    orden = OrderField(blank=True, for_fields=['modulo'], verbose_name="Orden")

    class Meta:
        verbose_name = "Contenido"
        verbose_name_plural = "Contenidos"
        ordering = ['orden']

    def __str__(self):
        return f"Contenido {self.id} del módulo {self.modulo}"


# ================== RESEÑA O CALIFICACIÓN ==================
class Reseña(models.Model):
    """
    Opinión y calificación que un estudiante deja sobre un curso.
    """
    OPCIONES_CALIFICACION = (
        (1, '1'),
        (2, '2'),
        (3, '3'),
        (4, '4'),
        (5, '5')
    )
    curso = models.ForeignKey(Curso, related_name='reseñas', on_delete=models.CASCADE, verbose_name="Curso")
    fecha_publicacion = models.DateTimeField(auto_now_add=True, verbose_name="Fecha de publicación")
    usuario = models.ForeignKey(settings.AUTH_USER_MODEL, related_name='reseñadores', on_delete=models.CASCADE, verbose_name="Usuario")
    comentario = models.CharField(max_length=200, verbose_name="Comentario")
    calificacion = models.IntegerField(choices=OPCIONES_CALIFICACION, verbose_name="Calificación")

    class Meta:
        verbose_name = "Reseña"
        verbose_name_plural = "Reseñas"

    def __str__(self):
        return f"{self.usuario.username} - {self.calificacion} estrellas"


# ================== PREMIO O INSIGNIA (BADGE) ==================
class Premio(models.Model):
    """
    Representa una insignia o logro otorgado a un usuario por completar objetivos.
    Inspirado en sistemas como Pinax Badges.
    """
    usuario = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        related_name='premios_obtenidos',
        on_delete=models.CASCADE,
        verbose_name="Usuario"
    )
    fecha_otorgado = models.DateTimeField(default=timezone.now, verbose_name="Fecha de otorgamiento")
    slug = models.CharField(max_length=255, verbose_name="Identificador")
    nivel = models.IntegerField(verbose_name="Nivel")

    def __str__(self):
        puntos = getattr(self.usuario, 'profile', None)
        puntos = puntos.award_points if puntos else 0
        return f"{self.usuario.username} : {puntos} puntos - nivel {self.nivel}"

    class Meta:
        verbose_name = "Premio"
        verbose_name_plural = "Premios"
```

---

### 🔍 Cambios realizados

| Original (Inglés)       | Traducido (Español)         | Nota |
|-------------------------|-----------------------------|------|
| `Subject` → `Tema`       | ✔️ | Más natural que "Asignatura" |
| `Course` → `Curso`       | ✔️ | |
| `Module` → `Modulo`      | ✔️ | Sin tilde por convención en código |
| `ItemBase` → `ContenidoBase` | ✔️ | Mejor descripción del propósito |
| `Text`, `File`, etc. → `Texto`, `Archivo`, etc. | ✔️ | |
| `Content` → `Contenido`  | ✔️ | |
| `Review` → `Reseña`      | ✔️ | Más natural que "Calificación" |
| `BadgeAward` → `Premio`  | ✔️ | |

---

### ✅ Ventajas de esta versión

- ✅ **Legibilidad** para equipos hispanohablantes.
- ✅ **Mantenimiento** más fácil con comentarios claros.
- ✅ **Integración con Django Admin** si agregas `verbose_name`.
- ✅ **No se altera la funcionalidad**.

---

### 📌 Recomendaciones

1. **Si ya tienes datos en la base de datos**, considera usar `db_column` para mantener los nombres de campos en inglés a nivel de BD, mientras el código está en español.

   Ejemplo:
   ```python
   titulo = models.CharField(max_length=200, db_column='title')
   ```

2. Asegúrate de tener instalado `autoslug`:
   ```bash
   pip install django-autoslug
   ```

3. Ejecuta las migraciones después de cambios:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

---

¿Quieres que te genere también la versión de `admin.py` en español o las vistas/forms traducidas?
