Aquí tienes la conversión del script SQL a un archivo `models.py` de **Django**, respetando las relaciones, claves foráneas, restricciones y tipos de datos lo más fielmente posible.

> ⚠️ **Nota**: Django maneja automáticamente la creación de bases de datos (`CREATE DATABASE`) y el uso de esquemas, por lo que eso no va en `models.py`. Además, Django usa por defecto `AUTO_INCREMENT`, `utf8mb4`, etc., así que no es necesario especificarlo.

---

### ✅ `models.py` (versión Django)

```python
from django.db import models

# Modelo base: Persona
class Persona(models.Model):
    SEXO_CHOICES = [
        (1, 'Masculino'),
        (2, 'Femenino'),
        (3, 'Otro'),
    ]

    cedula = models.CharField(max_length=10, unique=True)
    nombre = models.CharField(max_length=255, blank=True, null=True)
    apellido = models.CharField(max_length=255, blank=True, null=True)
    fecha_nacimiento = models.DateField(blank=True, null=True)
    sexo = models.IntegerField(choices=SEXO_CHOICES, blank=True, null=True)
    email = models.EmailField(max_length=255, blank=True, null=True)

    def __str__(self):
        return f"{self.nombre} {self.apellido} ({self.cedula})"

    class Meta:
        db_table = 'personas'


# Modelo Actor (relación uno a uno con Persona)
class Actor(models.Model):
    persona = models.OneToOneField(
        Persona,
        on_delete=models.CASCADE,
        primary_key=True,
        db_column='id'
    )
    num_peliculas = models.IntegerField(blank=True, null=True)
    premios_ganados = models.IntegerField(blank=True, null=True)
    biografia = models.TextField(blank=True, null=True)

    def __str__(self):
        return f"Actor: {self.persona.nombre} {self.persona.apellido}"

    class Meta:
        db_table = 'actores'


# Modelo Director (relación uno a uno con Persona)
class Director(models.Model):
    ESTILO_CHOICES = [
        (1, 'Drama'),
        (2, 'Acción'),
        (3, 'Comedia'),
        (4, 'Terror'),
        (5, 'Ciencia Ficción'),
        # Puedes ajustar según tus necesidades
    ]

    persona = models.OneToOneField(
        Persona,
        on_delete=models.CASCADE,
        primary_key=True,
        db_column='id'
    )
    num_peliculas_dirigidas = models.IntegerField(blank=True, null=True)
    estilo_director = models.IntegerField(choices=ESTILO_CHOICES, blank=True, null=True)
    premios_ganados = models.IntegerField(blank=True, null=True)
    biografia = models.TextField(blank=True, null=True)

    def __str__(self):
        return f"Director: {self.persona.nombre} {self.persona.apellido}"

    class Meta:
        db_table = 'directores'


# Modelo Película
class Pelicula(models.Model):
    GENERO_CHOICES = [
        (1, 'Drama'),
        (2, 'Acción'),
        (3, 'Comedia'),
        (4, 'Terror'),
        (5, 'Romance'),
        (6, 'Ciencia Ficción'),
        (7, 'Documental'),
        # Ajusta según necesidad
    ]

    titulo = models.CharField(max_length=255, blank=True, null=True)
    fecha_estreno = models.DateField(blank=True, null=True)
    director = models.ForeignKey(
        Director,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
        db_column='director_id',
        related_name='peliculas_dirigidas'
    )
    genero = models.IntegerField(choices=GENERO_CHOICES, blank=True, null=True)
    actor = models.ForeignKey(
        Actor,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
        db_column='actor_id',
        related_name='peliculas_actuadas'
    )

    def __str__(self):
        return self.titulo or "Película sin título"

    class Meta:
        db_table = 'peliculas'
```

---

### 🔍 Notas importantes:

1. **Relaciones uno a uno**:  
   - `Actor` y `Director` heredan de `Persona` mediante `OneToOneField`. Esto equivale al `FOREIGN KEY (id) REFERENCES personas(id)` en tu SQL.

2. **Claves primarias personalizadas**:  
   - Usamos `primary_key=True` en el `OneToOneField` para que el `id` del `Actor` o `Director` sea el mismo que el de `Persona`.

3. **db_column='id'**:  
   - Asegura que Django use el campo `id` como en tu esquema SQL original.

4. **Relaciones en Película**:  
   - `director_id` → ForeignKey a `Director`  
   - `actor_id` → ForeignKey a `Actor`  
   - Ambas permiten `NULL` (`SET_NULL`) para simular `DEFAULT NULL`.

5. **Campos de elección (choices)**:  
   - `sexo`, `estilo_director`, `genero` se modelan con `choices`. Puedes ajustar los valores según tu lógica de negocio.

6. **Migraciones**:  
   - Django generará automáticamente las tablas con `makemigrations` y `migrate`. No necesitas `DROP TABLE` ni `CREATE`.

7. **Base de datos**:  
   - Asegúrate de configurar `gestion_peliculas` como base de datos en `settings.py`.

---

### 🛠️ Siguiente paso (Django):

1. Guarda esto en `tu_app/models.py`
2. Ejecuta:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

Y tendrás tu esquema SQL replicado en Django.

¿Quieres también la versión con **Django con herencia de modelos** (por ejemplo, `Actor(Persona)`)? Puedo darte una alternativa más orientada a objetos.
