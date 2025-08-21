Aquí tienes una explicación breve y clara del código en `models.py`, con comentarios que describen cada parte:

```python
from django.db import models  # Importa el módulo de modelos de Django
from django.utils import timezone  # Para manejar fechas y horas con zona horaria

# Modelo para representar al personal (peluqueros o administradores)
class Personal(models.Model):
    # Opciones de rol: solo puede ser "Peluquero" o "Administrador"
    ROL_CHOICES = (
        ('Peluquero', 'Peluquero'),
        ('Administrador', 'Administrador')
    )
    nombre = models.CharField(max_length=100)  # Nombre del empleado
    apellido = models.CharField(max_length=100)  # Apellido del empleado
    correo_electronico = models.EmailField()  # Correo, validado automáticamente
    telefono = models.CharField(max_length=15)  # Teléfono de contacto
    rol = models.CharField(max_length=20, choices=ROL_CHOICES)  # Rol con opciones predefinidas

    def __str__(self):
        # Representación en texto del objeto (útil en el admin de Django)
        return f"{self.nombre} {self.apellido}"

# Modelo para representar a los clientes
class Cliente(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    correo_electronico = models.EmailField()
    telefono = models.CharField(max_length=15)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"

# Modelo para gestionar reservas de servicios
class Reserva(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)  # Relación: una reserva pertenece a un cliente
    personal_asignado = models.ForeignKey(Personal, on_delete=models.CASCADE)  # Relación con el personal
    fecha_hora_reserva = models.DateTimeField(default=timezone.now)  # Fecha y hora de la reserva
    servicio_solicitado = models.CharField(max_length=100)  # Nombre del servicio (ej: corte de cabello)
    estado_reserva = models.CharField(max_length=20)  # Estado: pendiente, confirmada, cancelada, etc.

    def __str__(self):
        return f"Reserva de {self.cliente.nombre} {self.cliente.apellido}"

# Modelo para registrar pagos asociados a una reserva
class Pago(models.Model):
    reserva_asociada = models.ForeignKey(Reserva, on_delete=models.CASCADE)  # Relación con la reserva
    metodo_pago = models.CharField(max_length=100)  # Ej: efectivo, tarjeta, transferencia
    monto = models.DecimalField(max_digits=10, decimal_places=2)  # Monto del pago
    fecha_hora_pago = models.DateField(default=timezone.now)  # Fecha del pago (aquí solo fecha, no hora)

    def __str__(self):
        return f"Pago finalizado de {self.monto} para {self.reserva_asociada}"

# Modelo para productos vendidos (ej: champú, peines, etc.)
class Productos(models.Model):
    nombre = models.CharField(max_length=100)  # Nombre del producto
    descripcion = models.TextField()  # Descripción detallada
    precio = models.DecimalField(max_digits=10, decimal_places=2)  # Precio con 2 decimales
    cantidad_disponible = models.IntegerField()  # Cantidad en stock

    def __str__(self):
        return self.nombre  # Muestra el nombre del producto en listas
```

### Resumen:
Este archivo define cinco modelos en Django para una peluquería:
- **Personal**: Empleados (peluqueros o admins).
- **Cliente**: Datos de los clientes.
- **Reserva**: Gestiona citas con cliente, personal, servicio y estado.
- **Pago**: Registra pagos por reservas.
- **Productos**: Inventario de productos vendibles.

Cada modelo se relaciona con otros (usando `ForeignKey`) para mantener consistencia de datos, y todos incluyen un `__str__` para mejor visualización en Django Admin.
