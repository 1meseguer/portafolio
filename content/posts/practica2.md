---
title: 'Practica2'
date: '2026-05-27T17:40:55-07:00'
draft: false
---

##  Paradigmas de la Programación

###  Práctica 2: Programación Orientada a Objetos

__Saúl Ignacio Meseguer Delgado__

---

### Introducción
Se desarrollará un sistema de gestion de estacionamiento utilizando el paradigma de Programación Orientada a Objetos.

### Objetivos
- Desarrollar un sistema utilizando los conceptos de la POO.
- Permitir utilizar el sistema en consola y via web con Flask.

### Diagrama UML

![UML](/images/uml.jpeg)

### Clases y responasbilidades

__Vehicle__ - Abstracción de un vehiculo

__Car__ - subtipo de Vehiculo para carros

__Motorcycle__ - Subtipo de Vehiculo para motocicletas

__ParkingLot__ - Administra los ParkingSpot, Tickets y el cobro

__ParkingSpot__ - Representa un cajón de estacionamiento

__Ticket__ - Registra el vehiculo estacionado, espacio y hora

__RatePolicy__ - Interfaz para cobro del espacio

__SimpleRatePolicy__ - Tarifa por vehiculo

__FlatRatePolicy__ - Tarifa comun para cualquier vehiculo

__WeekendRatePolicy__ - Tarifa el fin de semana

### Evidencias de conceptos POO

#### Encapsulación

```
class ParkingSpot:
    def __init__(self, spot_id: str, allowed_type: str):
        self._id = spot_id
        self._allowed_type = allowed_type
        self._is_occupied = False

    def is_available(self):
        return not self._is_occupied

    def occupy(self):
        self._is_occupied = True

    def release(self):
        self._is_occupied = False

    def get_id(self):
        return self._id

    def get_allowed_type(self):
        return self._allowed_type
```

#### Abstracción

```
from abc import ABC, abstractmethod

class RatePolicy(ABC):
    @abstractmethod
    def calculate(self, hours, vehicle):
        pass

class SimpleRatePolicy(RatePolicy):
    def calculate(self, hours, vehicle):
        rate = 20 if vehicle.get_type() == "car" else 10
        return hours * rate

class FlatRatePolicy(RatePolicy):
    def calculate(self, hours, vehicle):
        # Tarifa plana por hora
        return 15 * hours

class WeekendRatePolicy(RatePolicy):
    def calculate(self, hours, vehicle):
        # Tarifa más cara los fines de semana (simulado)
        rate = 25 if vehicle.get_type() == "car" else 15
        return hours * rate
```

#### Composición

```
from .ticket import Ticket

class ParkingLot:
    def __init__(self, rate_policy):
        self._spots = []
        self._active_tickets = {}
        self._rate_policy = rate_policy
        self._total_earned = 0

    def add_spot(self, spot):
        self._spots.append(spot)

    def find_available_spot(self, vehicle_type):
        for spot in self._spots:
            if spot.is_available() and spot.get_allowed_type() == vehicle_type:
                return spot
        return None

    def register_entry(self, vehicle):
        spot = self.find_available_spot(vehicle.get_type())
        if not spot:
            raise Exception(f"No hay spots disponibles para {vehicle.get_type()}")
        spot.occupy()
        ticket = Ticket(vehicle, spot)
        self._active_tickets[ticket.get_id()] = ticket
        return ticket

    def register_exit(self, ticket_id):
        if ticket_id not in self._active_tickets:
            raise Exception(f"Ticket #{ticket_id} no existe o ya está cerrado")
        ticket = self._active_tickets.pop(ticket_id)
        ticket.close()
        ticket.get_spot().release()
        cost = self._rate_policy.calculate(ticket.get_duration_hours(), ticket.get_vehicle())
        self._total_earned += cost
        return cost

    def get_occupation_status(self):
        total = len(self._spots)
        occupied = sum(1 for spot in self._spots if not spot.is_available())
        return total, occupied

    def get_active_tickets(self):
        return list(self._active_tickets.values())
```

#### Herencia/Subtipos

```
class Vehicle:
    def __init__(self, plates: str, vehicle_type: str):
        self._plates = plates
        self._type = vehicle_type

    def get_plates(self):
        return self._plates

    def get_type(self):
        return self._type

class Car(Vehicle):
    def __init__(self, plates: str):
        super().__init__(plates, "car")

class Motorcycle(Vehicle):
    def __init__(self, plates: str):
        super().__init__(plates, "motorcycle")
```

#### Polimorfismo

```
def select_rate_policy():
    print("\n--- Políticas de tarifa disponibles ---")
    print("1. Simple (car: $20/h, motorcycle: $10/h)")
    print("2. Tarifa plana ($15/h)")
    print("3. Fin de semana (car: $25/h, motorcycle: $15/h)")
    op = input("Selecciona una opción: ")
    if op == "1":
        return SimpleRatePolicy()
    elif op == "2":
        return FlatRatePolicy()
    elif op == "3":
        return WeekendRatePolicy()



    policy = select_rate_policy()
    lot = ParkingLot(policy)
```

### MVC con Flask

__Model__ - Vehicle, Ticket, Spot, ParkingLot, Rates

El modelo se encarga de realizar el cobro y asignar el spot dependiendo el vehiculo.

__View__ - base, dashboard, entry, exit

La vista muestra una interfaz al usuario para despues hacer una interaccion con los datos.

__Controller__ - app

El controlador invoca los metodos del modelo y muestra las vistas al usuario

### Pruebas manuales

#### Flask

![](/images/1.png)
![](/images/2.png)
![](/images/3.png)
![](/images/4.png)
![](/images/5.png)

#### CLI

![](/images/posts/6.png)
![](/images/posts/7.png)

### Conclusion

Una clase es una abstraccion de un objeto y tiene atributos (datos) y metodos (funciones).

Un objeto es una instancia de una clase, encapsula sus atributos y metodos.

La herencia permite que las clases hereden atributos y metodos de otras clases.

El encapsulamiento haace que los atributos de un objeto no pueda ser modificado desde afuera.

La abstracción se concentra en realizar procedimientos sin enfocarse en demostrar detalles de los pasos.

El polimorfismo permite que un metodo realize una accion dependiendo del objeto que lo ejecute.

---

El permitir la herencia entre clases permite que el codigo pueda reutilizarse, ahorrando espacio y tiempo en escribir codigo.
El codigo no debe se debe reescribir tanto codigo si se desea modificar.
El código tiene mayor estructura y facilita su lectura.

### Referencias

Valencia, A. (2024, 9 mayo). ¿Qué es la Programación Orientada a Objetos (POO) y cuáles son sus principios fundamentales? CodersLink. https://coderslink.com/talento/blog/que-es-la-programacion-orientada-a-objetos-poo-y-cuales-son-sus-principios-fundamentales/

López, Á. (2026, 10 marzo). Ventajas de la programación orientada a objetos (POO). Ciberaula. https://www.ciberaula.com/cursos/java/ventajas_poo.php#reusabilidad
