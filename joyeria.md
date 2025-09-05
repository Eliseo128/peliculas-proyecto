¡Claro! Aquí tienes una versión **actualizada y moderna** del archivo `models.py`, compatible con **Django 3.2+** y **Python 3**, con las siguientes mejoras:

- Uso de `__str__` en lugar de `__unicode__` (Python 3).
- `on_delete=models.CASCADE` explícito (requerido desde Django 2.0).
- Nombres en español, como pediste.
- Uso de `verbose_name` y `verbose_name_plural` para que el panel de administración también muestre los nombres en español.
- Mejor organización y comentarios en español.
- Uso de `upload_to` con carpetas organizadas.
- Añadido `blank=True, null=True` donde sea necesario para mayor flexibilidad.

---

### ✅ `models.py` (versión moderna y en español)

```python
from django.db import models
from django.contrib.auth.models import User
from django.utils.translation import gettext_lazy as _

# -------------------------------
# Modelo: Tipo de Metal
# -------------------------------
class TipoMetal(models.Model):
    tipo_metal = models.CharField(
        max_length=30,
        verbose_name=_("Tipo de Metal")
    )

    class Meta:
        verbose_name = _("Tipo de Metal")
        verbose_name_plural = _("Tipos de Metal")

    def __str__(self):
        return self.tipo_metal


# -------------------------------
# Modelo: Categoría
# -------------------------------
class Categoria(models.Model):
    tipo_metal = models.ForeignKey(
        TipoMetal,
        on_delete=models.CASCADE,
        verbose_name=_("Tipo de Metal")
    )
    nombre_categoria = models.CharField(
        max_length=50,
        verbose_name=_("Nombre de la Categoría")
    )

    class Meta:
        verbose_name = _("Categoría")
        verbose_name_plural = _("Categorías")

    def __str__(self):
        return self.nombre_categoria


# -------------------------------
# Modelo: Producto
# -------------------------------
class Producto(models.Model):
    categoria = models.ForeignKey(
        Categoria,
        on_delete=models.CASCADE,
        verbose_name=_("Categoría")
    )
    nombre_producto = models.CharField(
        max_length=100,
        verbose_name=_("Nombre del Producto")
    )
    detalle_producto = models.TextField(
        verbose_name=_("Detalle del Producto")
    )
    costo_producto = models.DecimalField(
        max_digits=10,
        decimal_places=2,
        verbose_name=_("Costo del Producto")
    )
    foto_producto = models.ImageField(
        upload_to='productos/',
        blank=True,
        null=True,
        verbose_name=_("Foto del Producto")
    )

    class Meta:
        verbose_name = _("Producto")
        verbose_name_plural = _("Productos")

    def __str__(self):
        return f"{self.nombre_producto} - {self.categoria.nombre_categoria}: ${self.costo_producto}"


# -------------------------------
# Modelo: Reseña de Producto
# -------------------------------
class ReseñaProducto(models.Model):
    usuario = models.ForeignKey(
        User,
        on_delete=models.CASCADE,
        verbose_name=_("Usuario")
    )
    producto = models.ForeignKey(
        Producto,
        on_delete=models.CASCADE,
        verbose_name=_("Producto")
    )
    reseña = models.TextField(
        verbose_name=_("Reseña")
    )
    calificacion = models.IntegerField(
        default=1,
        verbose_name=_("Calificación")
    )

    class Meta:
        verbose_name = _("Reseña de Producto")
        verbose_name_plural = _("Reseñas de Productos")

    def __str__(self):
        return f"{self.usuario.username} : {self.producto.nombre_producto} : {self.reseña[:30]}..."


# -------------------------------
# Modelo: Ítem de Compra
# -------------------------------
class ItemCompra(models.Model):
    usuario = models.ForeignKey(
        User,
        on_delete=models.CASCADE,
        verbose_name=_("Usuario")
    )
    producto = models.ForeignKey(
        Producto,
        on_delete=models.CASCADE,
        verbose_name=_("Producto")
    )
    comprado = models.BooleanField(
        default=False,
        verbose_name=_("¿Comprado?")
    )
    cantidad = models.IntegerField(
        default=0,
        verbose_name=_("Cantidad")
    )

    class Meta:
        verbose_name = _("Ítem de Compra")
        verbose_name_plural = _("Ítems de Compra")

    def __str__(self):
        return f"{self.usuario.username} : {self.producto.nombre_producto} (x{self.cantidad})"


# -------------------------------
# Modelo: Detalle de Compra
# -------------------------------
class DetalleCompra(models.Model):
    nombre_completo = models.CharField(
        max_length=100,
        verbose_name=_("Nombre Completo")
    )
    calle = models.CharField(
        max_length=100,
        verbose_name=_("Calle")
    )
    ciudad = models.CharField(
        max_length=100,
        verbose_name=_("Ciudad")
    )
    distrito = models.CharField(
        max_length=100,
        verbose_name=_("Distrito")
    )
    estado = models.CharField(
        max_length=100,
        verbose_name=_("Estado/Provincia")
    )
    pais = models.CharField(
        max_length=100,
        verbose_name=_("País")
    )
    codigo_postal = models.IntegerField(
        verbose_name=_("Código Postal")
    )

    class Meta:
        verbose_name = _("Detalle de Compra")
        verbose_name_plural = _("Detalles de Compra")

    def __str__(self):
        return f"{self.nombre_completo} - {self.ciudad}, {self.pais}"
```

---

### ✅ Cambios clave realizados:

| Cambio | Descripción |
|-------|-------------|
| ✅ `__str__` | Reemplaza `__unicode__` para Python 3. |
| ✅ `on_delete=models.CASCADE` | Obligatorio en relaciones ForeignKey. |
| ✅ `verbose_name` | Para que el admin de Django muestre nombres en español. |
| ✅ `null=True` en `ImageField` | Mejor práctica para manejar imágenes opcionales. |
| ✅ Carpetas organizadas (`upload_to='productos/'`) | Mejor estructura de archivos. |
| ✅ F-strings en `__str__` | Más legibles y modernos. |
| ✅ Traducción con `gettext_lazy` | Listo para futura internacionalización. |

---

### ✅ Recomendaciones adicionales:

1. **Admin de Django**: Asegúrate de registrar estos modelos en `admin.py` con nombres en español.
2. **Migraciones**: Ejecuta:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```
3. **Traducciones**: Si usas i18n, puedes agregar traducciones completas en archivos `.po`.

---

¿Quieres que te genere también el `admin.py` traducido y modernizado?
