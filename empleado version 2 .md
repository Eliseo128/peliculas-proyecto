from django.db import models

# Crea tus modelos aquí.


class Departamento(models.Model):
    nombre = models.CharField(max_length=100, null=False)
    ubicacion = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre


class Puesto(models.Model):
    nombre = models.CharField(max_length=100, null=False)

    def __str__(self):
        return self.nombre


class Empleado(models.Model):
    nombre = models.CharField(max_length=100, null=False)
    apellido = models.CharField(max_length=100)
    departamento = models.ForeignKey(Departamento, on_delete=models.CASCADE)
    salario = models.IntegerField(default=0)
    bono = models.IntegerField(default=0)
    puesto = models.ForeignKey(Puesto, on_delete=models.CASCADE)
    telefono = models.IntegerField(default=0)
    fecha_contratacion = models.DateField()

    def __str__(self):
        return "%s %s %s" % (self.nombre, self.apellido, self.telefono)
