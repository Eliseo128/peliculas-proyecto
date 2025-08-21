A continuación, te presento el archivo `models.py` **traducido al español**, con:

- Nombres de clases y campos en español.
- Relaciones apropiadas entre modelos usando `ForeignKey`.
- Comentarios breves explicando cada parte.
- Mejoras en tipos de campo (por ejemplo, usar `DecimalField` en lugar de `FloatField` para precios).
- Eliminación de `db_table` innecesario (Django lo maneja automáticamente, pero si se necesita se puede mantener).

---

```python
from django.db import models

# Modelo que representa un alimento o producto de comida
class Alimento(models.Model):
    id_alimento = models.AutoField(primary_key=True)  # Identificador único del alimento
    nombre = models.CharField(max_length=30)  # Nombre del alimento (ej: "Hamburguesa")
    categoria = models.CharField(max_length=30)  # Categoría (ej: "Plato principal", "Bebida")
    precio = models.DecimalField(max_digits=10, decimal_places=2)  # Precio con 2 decimales
    imagen = models.ImageField(upload_to='alimentos/', default='alimentos/default.jpg')  # Imagen del alimento

    def __str__(self):
        return self.nombre  # Mostrar el nombre en el admin

    class Meta:
        verbose_name = "Alimento"
        verbose_name_plural = "Alimentos"


# Modelo que representa a un cliente
class Cliente(models.Model):
    id_cliente = models.AutoField(primary_key=True)  # Identificador único
    nombre = models.CharField(max_length=30)  # Nombre del cliente
    apellido = models.CharField(max_length=30)  # Apellido del cliente
    contacto = models.CharField(max_length=10)  # Número de contacto (teléfono)
    correo = models.EmailField(unique=True)  # Correo único
    contrasena = models.CharField(max_length=60)  # Contraseña (idealmente usar auth.User)
    direccion = models.CharField(max_length=150, default='')  # Dirección de entrega

    def __str__(self):
        return f"{self.nombre} {self.apellido}"

    class Meta:
        verbose_name = "Cliente"
        verbose_name_plural = "Clientes"


# Modelo para el administrador del sistema
class Administrador(models.Model):
    id_administrador = models.CharField(primary_key=True, max_length=20)  # ID como cadena (ej: "admin01")
    contrasena = models.CharField(max_length=60)  # Contraseña del admin

    def __str__(self):
        return self.id_administrador

    class Meta:
        verbose_name = "Administrador"
        verbose_name_plural = "Administradores"


# Modelo del carrito de compras: almacena los alimentos que el cliente quiere comprar
class Carrito(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)  # Relación con el cliente
    alimento = models.ForeignKey(Alimento, on_delete=models.CASCADE)  # Relación con el alimento
    cantidad = models.PositiveIntegerField()  # Cantidad de unidades del alimento

    def __str__(self):
        return f"{self.cantidad}x {self.alimento.nombre} - {self.cliente.nombre}"

    class Meta:
        verbose_name = "Carrito"
        verbose_name_plural = "Carritos"


# Modelo que representa un pedido realizado por un cliente
class Pedido(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)  # Cliente que realizó el pedido
    fecha = models.DateTimeField(auto_now_add=True)  # Fecha y hora del pedido
    total = models.DecimalField(max_digits=10, decimal_places=2)  # Factura total del pedido

    def __str__(self):
        return f"Pedido {self.id} - {self.cliente.nombre} - {self.total}€"

    class Meta:
        verbose_name = "Pedido"
        verbose_name_plural = "Pedidos"
```

---

### ✅ Cambios y mejoras realizadas:

| Cambio | Explicación |
|------|-------------|
| **Nombres en español** | Todas las clases y campos ahora están en español. |
| **Relaciones (`ForeignKey`)** | Se reemplazaron los campos de texto como `CustEmail` o `FoodId` por relaciones reales entre modelos. |
| **Tipos de campo mejorados** | `precio` ahora es `DecimalField` (más preciso que `FloatField`). |
| **`upload_to` corregido** | Se usa una carpeta específica para imágenes de alimentos. |
| **`unique=True` en correo** | Asegura que no haya clientes con el mismo correo. |
| **`auto_now_add` en fecha** | La fecha del pedido se guarda automáticamente al crearlo. |
| **Comentarios y `__str__`** | Mejora la legibilidad y uso en el panel de administración. |

---

> 💡 **Nota**: Para mayor seguridad, considera usar el modelo `User` de Django (`auth.User`) para clientes y administradores en lugar de crear tus propios campos de contraseña.

¿Quieres que lo convierta usando `AbstractUser` o con autenticación integrada?
