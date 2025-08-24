¡Perfecto! Vamos a convertir tu **SQL** a un archivo **models.py** para Django.
Cada tabla se convertirá en una clase de modelo, usando `ForeignKey`, `DecimalField`, `CharField`, etc. También incluiremos **`on_delete`** según tus llaves foráneas.

---

```python
from django.db import models

# --- Tabla de Marcas ---
class Marca(models.Model):
    nombre_marca = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre_marca


# --- Tabla de Categorías ---
class Categoria(models.Model):
    nombre_categoria = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre_categoria


# --- Tabla de Tipos de Motor ---
class TipoMotor(models.Model):
    GASOLINERO = 'Gasolinero'
    PETROLERO = 'Petrolero'
    TIPO_MOTOR_CHOICES = [
        (GASOLINERO, 'Gasolinero'),
        (PETROLERO, 'Petrolero'),
    ]
    tipo_motor = models.CharField(max_length=20, choices=TIPO_MOTOR_CHOICES)

    def __str__(self):
        return self.tipo_motor


# --- Tabla de Tipos de Productos ---
class TipoProducto(models.Model):
    nombre_tipo_producto = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre_tipo_producto


# --- Tabla de Proveedores ---
class Proveedor(models.Model):
    nombre_proveedor = models.CharField(max_length=100)
    contacto = models.CharField(max_length=100, blank=True, null=True)
    telefono = models.CharField(max_length=20, blank=True, null=True)
    email = models.EmailField(max_length=100, blank=True, null=True)
    direccion = models.CharField(max_length=255, blank=True, null=True)

    def __str__(self):
        return self.nombre_proveedor


# --- Tabla de Clientes ---
class Cliente(models.Model):
    nombre_cliente = models.CharField(max_length=150)
    contacto_cliente = models.CharField(max_length=100, blank=True, null=True)
    telefono_cliente = models.CharField(max_length=20, blank=True, null=True)
    email_cliente = models.EmailField(max_length=100, blank=True, null=True)
    direccion_cliente = models.CharField(max_length=255, blank=True, null=True)
    ruc_cliente = models.CharField(max_length=20, blank=True, null=True)
    fecha_registro = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre_cliente


# --- Tabla de Productos ---
class Producto(models.Model):
    nombre_producto = models.CharField(max_length=150)
    marca = models.ForeignKey(Marca, on_delete=models.SET_NULL, null=True, blank=True)
    categoria = models.ForeignKey(Categoria, on_delete=models.SET_NULL, null=True, blank=True)
    tipo_motor = models.ForeignKey(TipoMotor, on_delete=models.SET_NULL, null=True, blank=True)
    descripcion = models.TextField(blank=True, null=True)
    imagen_url = models.URLField(max_length=255, blank=True, null=True)
    qr_code = models.CharField(max_length=255, blank=True, null=True)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    fecha_modificacion = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.nombre_producto


# --- Tabla de Relación Productos_Tipo ---
class ProductoTipo(models.Model):
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    tipo_producto = models.ForeignKey(TipoProducto, on_delete=models.CASCADE)


# --- Tabla de Compras ---
class Compra(models.Model):
    proveedor = models.ForeignKey(Proveedor, on_delete=models.SET_NULL, null=True)
    fecha_compra = models.DateTimeField(auto_now_add=True)
    total_compra = models.DecimalField(max_digits=10, decimal_places=2, blank=True, null=True)

    def __str__(self):
        return f"Compra #{self.id} - {self.fecha_compra}"


# --- Detalles Compras ---
class DetalleCompra(models.Model):
    compra = models.ForeignKey(Compra, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.PositiveIntegerField()
    precio_unitario = models.DecimalField(max_digits=10, decimal_places=2)
    subtotal = models.DecimalField(max_digits=10, decimal_places=2, editable=False)

    def save(self, *args, **kwargs):
        self.subtotal = self.cantidad * self.precio_unitario
        super().save(*args, **kwargs)


# --- Tabla de Tipos Documentos ---
class TipoDocumento(models.Model):
    BOLETA = 'Boleta'
    FACTURA = 'Factura'
    TIPO_DOC_CHOICES = [
        (BOLETA, 'Boleta'),
        (FACTURA, 'Factura'),
    ]
    tipo_documento = models.CharField(max_length=20, choices=TIPO_DOC_CHOICES)

    def __str__(self):
        return self.tipo_documento


# --- Tabla Ventas ---
class Venta(models.Model):
    fecha_venta = models.DateTimeField(auto_now_add=True)
    total_venta = models.DecimalField(max_digits=10, decimal_places=2, blank=True, null=True)
    tipo_documento = models.ForeignKey(TipoDocumento, on_delete=models.SET_NULL, null=True)
    cliente = models.ForeignKey(Cliente, on_delete=models.SET_NULL, null=True)
    cliente_nombre = models.CharField(max_length=150, blank=True, null=True)
    cliente_contacto = models.CharField(max_length=100, blank=True, null=True)
    numero_documento = models.CharField(max_length=50, blank=True, null=True)

    def __str__(self):
        return f"Venta #{self.id}"


# --- Detalles Ventas ---
class DetalleVenta(models.Model):
    venta = models.ForeignKey(Venta, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.PositiveIntegerField()
    precio_unitario = models.DecimalField(max_digits=10, decimal_places=2)
    subtotal = models.DecimalField(max_digits=10, decimal_places=2, editable=False)

    def save(self, *args, **kwargs):
        self.subtotal = self.cantidad * self.precio_unitario
        super().save(*args, **kwargs)


# --- Boletas de Venta ---
class BoletaVenta(models.Model):
    venta = models.ForeignKey(Venta, on_delete=models.CASCADE)
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)
    numero_boleta = models.CharField(max_length=50, unique=True)
    fecha_emision = models.DateTimeField(auto_now_add=True)


# --- Facturas ---
class Factura(models.Model):
    venta = models.ForeignKey(Venta, on_delete=models.CASCADE)
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)
    numero_factura = models.CharField(max_length=50, unique=True)
    fecha_emision = models.DateTimeField(auto_now_add=True)
    ruc_cliente = models.CharField(max_length=20, blank=True, null=True)
    direccion_cliente = models.CharField(max_length=255, blank=True, null=True)


# --- Cambios de Aceite ---
class CambioAceite(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)
    fecha_cambio = models.DateTimeField(auto_now_add=True)
    kilometraje = models.PositiveIntegerField()
    producto = models.ForeignKey(Producto, on_delete=models.SET_NULL, null=True, blank=True)
    tipo_aceite = models.CharField(max_length=50, blank=True, null=True)
    descripcion = models.TextField(blank=True, null=True)
    numero_voucher = models.CharField(max_length=50, unique=True, blank=True, null=True)

    def __str__(self):
        return f"Cambio Aceite #{self.id} - {self.cliente}"
```

---

🔹 **Claves de la conversión:**

* `ENUM` → `CharField` con `choices`.
* `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` → `DateTimeField(auto_now_add=True)`.
* `ON UPDATE CURRENT_TIMESTAMP` → `auto_now=True`.
* `AUTO_INCREMENT` → Django lo hace automáticamente con `id`.
* Subtotales calculados (`AS (cantidad * precio_unitario)`) → cálculo en `save()`.

---

¿Quieres que también te genere **el diagrama ERD** (modelo entidad-relación) para este `models.py`?
