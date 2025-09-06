¡Claro! A continuación, te presento **todos tus modelos traducidos al español**, con nombres de clases y atributos adaptados para ser más naturales y comprensibles en español, manteniendo la funcionalidad original y buenas prácticas de Django.

He organizado los modelos en tres archivos lógicos (como parece que los tienes separados en tu proyecto), pero si los tienes en un solo archivo, también puedes pegarlos juntos sin problema.

---

## 🛒 1. `cart/models.py` — Carrito y Pedido

```python
from django.db import models
from django.contrib.auth import get_user_model
from products.models import Producto  # ¡Importante: cambiar a nombre en español!

# Obtener el modelo de usuario
Usuario = get_user_model()


# Modelo Carrito
class Carrito(models.Model):
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE)
    producto = models.ForeignKey('Producto', on_delete=models.CASCADE)  # Referencia por string si aún no está definido
    cantidad = models.IntegerField(default=1)
    comprado = models.BooleanField(default=False)
    creado = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f'{self.cantidad} de {self.producto.nombre}'

    def obtener_total(self):
        total = self.producto.precio * self.cantidad
        total_float = float("{0:.2f}".format(total))
        return total_float


# Modelo Pedido
class Pedido(models.Model):
    items_pedido = models.ManyToManyField(Carrito, related_name='pedidos')
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE)
    realizado = models.BooleanField(default=False)
    creado = models.DateTimeField(auto_now_add=True)
    id_pago = models.CharField(max_length=200, blank=True, null=True)
    id_pedido = models.CharField(max_length=200, blank=True, null=True)

    def __str__(self):
        return self.usuario.username

    def obtener_total_general(self):
        total = 0
        for item in self.items_pedido.all():
            total += item.obtener_total()
        return total
```

---

## 📦 2. `products/models.py` — Categoría y Producto

```python
from django.db import models
from django.urls import reverse


# Modelo Categoría
class Categoria(models.Model):
    titulo = models.CharField(max_length=300)
    categoria_principal = models.BooleanField(default=False)

    def __str__(self):
        return self.titulo

    class Meta:
        verbose_name_plural = "Categorías"


# Modelo Producto
class Producto(models.Model):
    imagen_principal = models.ImageField(upload_to='productos/', blank=True)
    nombre = models.CharField(max_length=300)
    slug = models.SlugField()
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE)
    texto_preview = models.TextField(max_length=200, verbose_name='Texto de vista previa')
    texto_detalle = models.TextField(max_length=1000, verbose_name='Texto detallado')
    precio = models.FloatField()

    def __str__(self):
        return self.nombre

    def get_absolute_url(self):
        return reverse("mainapp:producto", kwargs={
            'slug': self.slug
        })
```

> ✅ Nota: En `Carrito`, si `Producto` se define en otro archivo, puedes dejar la referencia como string:  
> `producto = models.ForeignKey('Producto', ...)`  
> O asegurarte de que el `import` apunte al modelo traducido: `from products.models import Producto`

---

## 📍 3. `billing/models.py` — Dirección de Facturación y Formulario

```python
from django.db import models
from django.forms import ModelForm
from django.contrib.auth import get_user_model

# Obtener el modelo de usuario
Usuario = get_user_model()


# Modelo Dirección de Facturación
class DireccionFacturacion(models.Model):
    usuario = models.ForeignKey(Usuario, on_delete=models.CASCADE)
    direccion = models.CharField(max_length=100)
    codigo_postal = models.CharField(max_length=50)
    ciudad = models.CharField(max_length=30)
    referencia = models.CharField(max_length=20)  # "Landmark" → "Referencia" (ej: cerca del parque)

    def __str__(self):
        return f'Dirección de facturación de {self.usuario.username}'

    class Meta:
        verbose_name_plural = "Direcciones de Facturación"


# Formulario de Dirección (ModelForm)
class FormularioFacturacion(ModelForm):
    class Meta:
        model = DireccionFacturacion
        fields = ['direccion', 'codigo_postal', 'ciudad', 'referencia']
```

---

## ✅ Resumen de Cambios Principales:

| Original (Inglés)          | Traducción (Español)             |
|----------------------------|----------------------------------|
| `Cart`                     | `Carrito`                        |
| `Order`                    | `Pedido`                         |
| `Product`                  | `Producto`                       |
| `Category`                 | `Categoria`                      |
| `BillingAddress`           | `DireccionFacturacion`           |
| `BillingForm`              | `FormularioFacturacion`          |
| `user`                     | `usuario`                        |
| `item`                     | `producto`                       |
| `quantity`                 | `cantidad`                       |
| `purchased`                | `comprado`                       |
| `created`                  | `creado`                         |
| `ordered`                  | `realizado`                      |
| `paymentId`                | `id_pago`                        |
| `orderId`                  | `id_pedido`                      |
| `title`                    | `titulo`                         |
| `primaryCategory`          | `categoria_principal`            |
| `mainimage`                | `imagen_principal`               |
| `name`                     | `nombre`                         |
| `slug`                     | `slug` (se mantiene, es técnico) |
| `preview_text`             | `texto_preview`                  |
| `detail_text`              | `texto_detalle`                  |
| `price`                    | `precio`                         |
| `address`                  | `direccion`                      |
| `zipcode`                  | `codigo_postal`                  |
| `city`                     | `ciudad`                         |
| `landmark`                 | `referencia`                     |

---

## 💡 Recomendaciones adicionales:

- Considera cambiar `FloatField` por `DecimalField` en `precio` para mayor precisión en cálculos monetarios:
  ```python
  precio = models.DecimalField(max_digits=10, decimal_places=2)
  ```
- Si usas `get_absolute_url`, asegúrate de que la URL `mainapp:producto` también esté definida con el nombre en español en tus `urls.py`.

---

✅ ¡Listo! Tus modelos ahora están completamente en español, con nombres claros, consistentes y adecuados para una aplicación dirigida a hispanohablantes.

¿Quieres que también te ayude a traducir los `verbose_name`, mensajes de validación, o incluso las plantillas y formularios? ¡Estoy aquí para ayudarte!
