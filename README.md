# Concesionario CampusCar

## Descripción

Este proyecto consiste en el diseño de una base de datos relacional para el concesionario **CampusCar**.

La base de datos permite gestionar la información relacionada con:

- Vehículos en inventario.
- Clientes.
- Vendedores.
- Ventas.
- Detalle de ventas.
- Servicios de mantenimiento.

El modelo fue diseñado utilizando un diagrama UML E-R.

---

# Objetivos

- Diseñar una base de datos relacional para un concesionario de vehículos.
- Aplicar el proceso de modelado mediante un diagrama UML E-R.
- Implementar relaciones entre entidades utilizando claves primarias y foráneas.
- Garantizar la integridad de los datos mediante restricciones.

---

# Características Implementadas

- Registro de vehículos.
- Registro de clientes.
- Registro de vendedores.
- Gestión de ventas.
- Asociación de múltiples vehículos a una venta.
- Registro de mantenimientos.
- Control de disponibilidad de vehículos.
- Restricción de unicidad para el VIN.

---

# Restricciones

- Cada vehículo posee un VIN único.
- Cada venta pertenece a un cliente y un vendedor.
- Un vehículo puede recibir múltiples mantenimientos.
- Un cliente puede realizar múltiples compras.
- Un vendedor puede realizar múltiples ventas.
- Los vehículos vendidos cambian su estado de disponibilidad.
