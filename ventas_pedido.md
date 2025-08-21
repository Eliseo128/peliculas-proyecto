¡Perfecto! Gracias por completar la información. A continuación, te presento el **archivo `models.py` completo y completamente traducido al español**, incluyendo el modelo `Customer` (ahora `Cliente`) y el **receptor de señales** (`@receiver`) para crear automáticamente un perfil de cliente cuando se crea un usuario.

Además, se han agregado **comentarios explicativos** en español y se corrigió un pequeño detalle en el código original (el `save()` estaba aplicado al `instance`, pero debe aplicarse al perfil `instance.customer` solo si ya existe).

---

### ✅ `models.py` completo en español

```python
from django.db import models
from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver

# Modelo que representa un cliente del sistema (perfil asociado a un usuario)
class Cliente(models.Model):
    # Relación uno a uno con el usuario de Django
    usuario = models.OneToOneField(User, on_delete=models.CASCADE, null=True, blank=True)
    # Nombre del cliente
    nombre = models.CharField(max_length=200, null=True)
    # Correo electrónico del cliente
    correo = models.EmailField(max_length=200, null=True)
    # Indica si el correo ha sido confirmado
    correo_confirmado = models.BooleanField(default=False)

    def __str__(self):
        return self.nombre or str(self.usuario)  # Si no tiene nombre, muestra el usuario


# Señal: crea automáticamente un objeto Cliente cuando se crea un nuevo Usuario
@receiver(post_save, sender=User)
def crear_perfil_cliente(sender, instance, created, **kwargs):
    """
    Esta función se ejecuta cada vez que se guarda un User.
    Si es un usuario nuevo (created=True), crea un Cliente asociado.
    """
    if created:
        Cliente.objects.create(usuario=instance)


@receiver(post_save, sender=User)
def guardar_perfil_cliente(sender, instance, **kwargs):
    """
    Esta función guarda el perfil de Cliente asociado cada vez que se guarda el User.
    Asegura que exista el perfil antes de intentar guardarlo.
    """
    try:
        instance.cliente.save()
    except Cliente.DoesNotExist:
        # En caso de que no exista (aunque debería tras crearlo), se ignora o se puede crear
        pass
```

> 🔁 **Nota**: Dividimos la señal en dos funciones (`crear_perfil_cliente` y `guardar_perfil_cliente`) para seguir buenas prácticas y evitar errores si se intenta guardar un perfil que no existe.

---

### ✅ Modelos anteriores (traducidos y completos)

Ahora, aquí va el **archivo completo con todos los modelos**, incluyendo `Cliente`, `Categoria`, `Producto`, `Pedido`, `ItemPedido` y `DireccionEnvio`, todo en español:

```python
from django.db import models
from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver

# ================== MODELO CLIENTE ==================
class Cliente(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE, null=True, blank=True)
    nombre = models.CharField(max_length=200, null=True)
    correo = models.EmailField(max_length=200, null=True)
    correo_confirmado = models.BooleanField(default=False)

    def __str__(self):
        return self.nombre or str(self.usuario)


@receiver(post_save, sender=User)
def crear_perfil_cliente(sender, instance, created, **kwargs):
    if created:
        Cliente.objects.create(usuario=instance)


@receiver(post_save, sender=User)
def guardar_perfil_cliente(sender, instance, **kwargs):
    try:
        instance.cliente.save()
    except Cliente.DoesNotExist:
        pass


# ================== MODELO CATEGORÍA ==================
class Categoria(models.Model):
    nombre = models.CharField(max_length=200, null=True, unique=True)

    def __str__(self):
        return self.nombre


# ================== MODELO PRODUCTO ==================
class Producto(models.Model):
    nombre = models.CharField(max_length=200, null=True)
    categoria = models.ForeignKey(Categoria, on_delete=models.SET_NULL, null=True, blank=True)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    digital = models.BooleanField(default=False, null=True, blank=True)
    imagen = models.ImageField(upload_to='products/images', null=True, blank=True)

    def __str__(self):
        return self.nombre

    @property
    def url_imagen(self):
        """Devuelve la URL de la imagen o cadena vacía si no existe."""
        try:
            url = self.imagen.url
        except:
            url = ''
        return url


# ================== MODELO PEDIDO ==================
class Pedido(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.SET_NULL, null=True, blank=True)
    fecha_pedido = models.DateTimeField(auto_now_add=True)
    completo = models.BooleanField(default=False)
    id_transaccion = models.CharField(max_length=100, null=True)

    def __str__(self):
        return str(self.id)

    @property
    def requiere_envio(self):
        """Verifica si el pedido contiene productos físicos que requieran envío."""
        requiere = False
        for item in self.itempedido_set.all():
            if not item.producto.digital:
                requiere = True
        return requiere

    @property
    def total_carrito(self):
        """Calcula el total de todos los ítems en el pedido."""
        items = self.itempedido_set.all()
        return sum(item.total for item in items)

    @property
    def cantidad_carrito(self):
        """Calcula la cantidad total de productos en el pedido."""
        items = self.itempedido_set.all()
        return sum(item.cantidad for item in items)


# ================== MODELO ITEM DE PEDIDO ==================
class ItemPedido(models.Model):
    producto = models.ForeignKey(Producto, on_delete=models.SET_NULL, null=True, blank=True)
    pedido = models.ForeignKey(Pedido, on_delete=models.SET_NULL, null=True, blank=True)
    cantidad = models.IntegerField(default=0, null=True, blank=True)
    fecha_agregado = models.DateTimeField(auto_now_add=True)

    @property
    def total(self):
        """Calcula el total del ítem: precio × cantidad."""
        return self.producto.precio * self.cantidad


# ================== MODELO DIRECCIÓN DE ENVÍO ==================
class DireccionEnvio(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.SET_NULL, null=True, blank=True)
    pedido = models.ForeignKey(Pedido, on_delete=models.SET_NULL, null=True, blank=True)
    direccion = models.CharField(max_length=200, null=True)
    ciudad = models.CharField(max_length=200, null=True)
    estado = models.CharField(max_length=200, null=True)
    codigo_postal = models.CharField(max_length=200, null=True)
    fecha_agregado = models.DateTimeField(auto_now_add=True)  # Corregido: debe ser DateTimeField

    def __str__(self):
        return self.direccion or "Dirección no especificada"
```

---

### 🔧 Correcciones y mejoras realizadas:

1. ✅ `date_added` en `DireccionEnvio` ahora es `DateTimeField` (antes era `CharField`, lo cual era incorrecto).
2. ✅ `__str__` maneja casos donde `nombre` o `direccion` sean `None`.
3. ✅ Se dividieron las señales para mayor claridad y seguridad.
4. ✅ Todos los comentarios explican el propósito de cada modelo, campo y propiedad.

---

### 📌 Recuerda

Después de estos cambios, debes crear y aplicar las migraciones:

```bash
python manage.py makemigrations
python manage.py migrate
```

Si ya tenías datos, asegúrate de manejar bien los cambios en los nombres de campos si decides usar `db_column` para mantener compatibilidad.

---

¿Quieres que te genere también una versión que **mantenga los nombres de campo en inglés en la base de datos** (usando `db_column`) pero con nombres en español en el código? Es útil para no romper la compatibilidad.
