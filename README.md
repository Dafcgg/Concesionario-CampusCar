# CampusCar — Base de Datos

Resumen de las tablas del modelo relacional para el concesionario de vehículos CampusCar.

---

## Vehículos

| Atributo         | Tipo de dato       | Descripción                                                |
| ---------------- | ------------------ | ---------------------------------------------------------- |
| VehiculoID       | SERIAL (PK)        | Identificador único autoincremental del vehículo.          |
| Marca            | VARCHAR(50)        | Fabricante del vehículo, por ejemplo Toyota o Mazda.       |
| Modelo           | VARCHAR(50)        | Nombre comercial del vehículo dentro de la marca.          |
| Año              | INT                | Año de fabricación del vehículo.                           |
| VIN              | VARCHAR(17) UNIQUE | Número de identificación vehicular, único a nivel mundial. |
| Precio           | DECIMAL(12,2)      | Precio base de venta del vehículo.                         |
| Color            | VARCHAR(30)        | Color exterior del vehículo.                               |
| Tipo_Combustible | VARCHAR(30)        | Tipo de combustible que utiliza el vehículo.               |
| Tipo_Transmision | VARCHAR(30)        | Tipo de transmisión, manual o automática.                  |
| Estado           | VARCHAR(20)        | Condición general del vehículo, nuevo o usado.             |
| Disponibilidad   | BOOLEAN            | Indica si el vehículo puede venderse actualmente.          |

---

## Clientes

| Atributo  | Tipo de dato | Descripción                                      |
| --------- | ------------ | ------------------------------------------------ |
| ClienteID | SERIAL (PK)  | Identificador único autoincremental del cliente. |
| Nombre    | VARCHAR(100) | Nombre completo del cliente.                     |
| Teléfono  | VARCHAR(20)  | Número de contacto del cliente.                  |
| Correo    | VARCHAR(100) | Dirección de correo electrónico del cliente.     |
| Dirección | VARCHAR(200) | Dirección física de residencia del cliente.      |

---

## Vendedores

| Atributo           | Tipo de dato | Descripción                                          |
| ------------------ | ------------ | ----------------------------------------------------- |
| VendedorID         | SERIAL (PK)  | Identificador único autoincremental del vendedor.    |
| Nombre             | VARCHAR(100) | Nombre completo del vendedor.                        |
| Número_Empleado    | VARCHAR(20)  | Código interno asignado al vendedor por la empresa.  |
| Fecha_Contratación | DATE         | Fecha en que el vendedor ingresó a laborar.          |

---

## Ventas

| Atributo    | Tipo de dato  | Descripción                                                   |
| ----------- | ------------- | -------------------------------------------------------------- |
| VentaID     | SERIAL (PK)   | Identificador único autoincremental de la venta.              |
| Método_Pago | VARCHAR(30)   | Forma de pago utilizada, por ejemplo tarjeta o transferencia. |
| Total       | DECIMAL(12,2) | Valor total cobrado en la transacción.                         |
| Fecha_Venta | TIMESTAMP     | Fecha y hora exacta en que se registró la venta.               |
| VendedorID  | INT (FK)      | Referencia al vendedor que gestionó la venta.                  |
| ClienteID   | INT (FK)      | Referencia al cliente que realizó la compra.                   |

---

## Detalle_Venta

| Atributo     | Tipo de dato  | Descripción                                          |
| ------------ | ------------- | ------------------------------------------------------ |
| DetalleID    | SERIAL (PK)   | Identificador único autoincremental del detalle.     |
| VentaID      | INT (FK)      | Referencia a la venta a la que pertenece el detalle. |
| VehiculoID   | INT (FK)      | Referencia al vehículo vendido en esa transacción.   |
| Precio_Venta | DECIMAL(12,2) | Precio final al que se vendió el vehículo.           |

---

## Mantenimiento

| Atributo           | Tipo de dato  | Descripción                                                  |
| ------------------ | ------------- | --------------------------------------------------------------- |
| MantenimientoID    | SERIAL (PK)   | Identificador único autoincremental del mantenimiento.       |
| Tipo_Servicio      | VARCHAR(100)  | Tipo de servicio: preventivo, correctivo, etc.                |
| Costo              | DECIMAL(12,2) | Valor cobrado por el servicio de mantenimiento.               |
| Fecha_Del_Servicio | TIMESTAMP     | Fecha y hora en que se prestó el servicio.                    |
| VehiculoID         | INT (FK)      | Referencia al vehículo que recibió el mantenimiento.          |
| ClienteID          | INT (FK)      | Referencia al cliente que solicitó el servicio (si aplica).   |
