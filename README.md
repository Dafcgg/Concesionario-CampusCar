# Diseño de Base de Datos Relacional — Concesionario de Vehículos "CampusCar"

> Modelo UML E-R diseñado
> **Autor:** Dilan Fonseca 

---

## Tabla de contenido

1. [Justificación del diseño](#1-justificación-del-diseño)
2. [Entidades y atributos](#2-entidades-y-atributos)
3. [Claves primarias y claves foráneas](#3-claves-primarias-y-claves-foráneas)
4. [Relaciones UML y cardinalidades](#4-relaciones-uml-y-cardinalidades)
5. [Restricciones y validaciones](#5-restricciones-y-validaciones)

---

## 1. Justificación del diseño

Se optó por un modelo relacional debido a que la información que maneja CampusCar tiene una naturaleza altamente estructurada y con relaciones bien definidas entre sus componentes: un vehículo pertenece a un inventario, una venta involucra a un cliente y a un vendedor, un detalle de venta conecta una venta con un vehículo específico, y un mantenimiento está asociado tanto a un vehículo como a un cliente. Este tipo de relaciones se representa de forma natural en un modelo relacional, donde las claves primarias y foráneas permiten mantener la integridad referencial entre las tablas.

PostgreSQL fue seleccionado como motor de base de datos por ser un sistema gestor robusto, de código abierto y ampliamente utilizado en el desarrollo de aplicaciones web y empresariales. Además, ofrece tipos de datos que se ajustan perfectamente a las necesidades del proyecto, como `SERIAL` para la generación automática de identificadores únicos, `DECIMAL` para el manejo exacto de valores monetarios sin errores de redondeo, y `TIMESTAMP` para el registro preciso de fechas y horas en eventos como ventas y mantenimientos.

Separar la venta general (tabla `Ventas`) del detalle de los vehículos vendidos (tabla `Detalle_Venta`) responde a una decisión de normalización: esta separación permite que el sistema conserve información adicional propia de cada vehículo vendido, como el precio final de venta, sin sobrecargar la tabla principal de ventas con datos que corresponden a otro nivel de detalle. Al mismo tiempo, `Detalle_Venta` funciona como tabla intermedia que permite que una misma venta agrupe uno o varios vehículos, tal como lo requiere el negocio.

El modelo está compuesto por seis entidades, cada una representando un concepto claramente diferenciado dentro del negocio del concesionario: **Vehículos**, entidad central del sistema que constituye el inventario; **Clientes**, que almacena la información de quienes compran vehículos o solicitan mantenimiento; **Vendedores**, encargados de gestionar las ventas; **Ventas**, que registra cada operación comercial; **Detalle_Venta**, que especifica qué vehículo fue vendido en cada transacción y a qué precio; y **Mantenimiento**, que registra los servicios técnicos prestados a los vehículos, tanto antes como después de su venta.

---

## 2. Entidades y atributos

### 2.1 Vehículos

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

### 2.2 Clientes

| Atributo  | Tipo de dato | Descripción                                      |
| --------- | ------------ | ------------------------------------------------ |
| ClienteID | SERIAL (PK)  | Identificador único autoincremental del cliente. |
| Nombre    | VARCHAR(100) | Nombre completo del cliente.                     |
| Teléfono  | VARCHAR(20)  | Número de contacto del cliente.                  |
| Correo    | VARCHAR(100) | Dirección de correo electrónico del cliente.     |
| Dirección | VARCHAR(200) | Dirección física de residencia del cliente.      |

### 2.3 Vendedores

| Atributo           | Tipo de dato | Descripción                                          |
| ------------------ | ------------ | ----------------------------------------------------- |
| VendedorID         | SERIAL (PK)  | Identificador único autoincremental del vendedor.    |
| Nombre             | VARCHAR(100) | Nombre completo del vendedor.                        |
| Número_Empleado    | VARCHAR(20)  | Código interno asignado al vendedor por la empresa.  |
| Fecha_Contratación | DATE         | Fecha en que el vendedor ingresó a laborar.          |

### 2.4 Ventas

| Atributo    | Tipo de dato  | Descripción                                                   |
| ----------- | ------------- | -------------------------------------------------------------- |
| VentaID     | SERIAL (PK)   | Identificador único autoincremental de la venta.              |
| Método_Pago | VARCHAR(30)   | Forma de pago utilizada, por ejemplo tarjeta o transferencia. |
| Total       | DECIMAL(12,2) | Valor total cobrado en la transacción.                         |
| Fecha_Venta | TIMESTAMP     | Fecha y hora exacta en que se registró la venta.               |
| VendedorID  | INT (FK)      | Referencia al vendedor que gestionó la venta.                  |
| ClienteID   | INT (FK)      | Referencia al cliente que realizó la compra.                   |

### 2.5 Detalle_Venta

| Atributo     | Tipo de dato  | Descripción                                          |
| ------------ | ------------- | ------------------------------------------------------ |
| DetalleID    | SERIAL (PK)   | Identificador único autoincremental del detalle.     |
| VentaID      | INT (FK)      | Referencia a la venta a la que pertenece el detalle. |
| VehiculoID   | INT (FK)      | Referencia al vehículo vendido en esa transacción.   |
| Precio_Venta | DECIMAL(12,2) | Precio final al que se vendió el vehículo.           |

### 2.6 Mantenimiento

| Atributo           | Tipo de dato  | Descripción                                                  |
| ------------------ | ------------- | --------------------------------------------------------------- |
| MantenimientoID    | SERIAL (PK)   | Identificador único autoincremental del mantenimiento.       |
| Tipo_Servicio      | VARCHAR(100)  | Tipo de servicio: preventivo, correctivo, etc.                |
| Costo              | DECIMAL(12,2) | Valor cobrado por el servicio de mantenimiento.               |
| Fecha_Del_Servicio | TIMESTAMP     | Fecha y hora en que se prestó el servicio.                    |
| VehiculoID         | INT (FK)      | Referencia al vehículo que recibió el mantenimiento.          |
| ClienteID          | INT (FK)      | Referencia al cliente que solicitó el servicio (si aplica).   |

---

## 3. Claves primarias y claves foráneas

Todas las entidades del modelo cuentan con una clave primaria definida como campo `SERIAL`, lo que significa que PostgreSQL genera automáticamente un valor entero único y consecutivo cada vez que se inserta un nuevo registro. Esta decisión simplifica la identificación de cada fila sin depender de que el usuario ingrese manualmente un identificador, reduciendo así el riesgo de duplicados o errores de digitación.

Las claves foráneas son las que materializan las relaciones entre las entidades:

- En la tabla `Ventas` se incluyen dos claves foráneas: `VendedorID`, que apunta hacia la entidad Vendedores, y `ClienteID`, que apunta hacia la entidad Clientes. Esto permite saber, para cada venta registrada, quién fue el vendedor responsable y qué cliente realizó la compra.
- La tabla `Detalle_Venta` utiliza también dos claves foráneas: `VentaID`, que la conecta con la venta general a la que pertenece, y `VehiculoID`, que identifica qué vehículo específico fue vendido.
- La tabla `Mantenimiento` incorpora `VehiculoID` y `ClienteID` como claves foráneas, ya que un mantenimiento siempre está asociado a un vehículo concreto y, cuando corresponde, a un cliente que lo solicita — incluso en los casos en que el mantenimiento se realiza antes de la venta, como parte del alistamiento del vehículo.

El uso consistente de claves primarias y foráneas en todo el modelo garantiza la integridad referencial de la base de datos: no es posible registrar una venta que haga referencia a un cliente o vendedor inexistente, ni un mantenimiento asociado a un vehículo que no está registrado en el sistema. Esto evita datos huérfanos y mantiene la coherencia general de la información.

---

## 4. Relaciones UML y cardinalidades

| Relación | Cardinalidad | Descripción |
|---|---|---|
| Clientes → Ventas | 1:N | Un cliente puede realizar varias compras a lo largo del tiempo, pero cada venta está asociada a un único cliente. |
| Vendedores → Ventas | 1:N | Cada vendedor puede gestionar múltiples ventas, pero cada venta fue gestionada por un único vendedor. |
| Ventas → Detalle_Venta | 1:N | Una venta puede contener uno o varios vehículos a través de la tabla Detalle_Venta, que actúa como tabla intermedia entre Ventas y Vehículos. |
| Vehículos → Detalle_Venta | 1:1 | Cada vehículo aparece en un único registro de Detalle_Venta, dado que una vez vendido deja de estar disponible para una nueva transacción. |
| Vehículos → Mantenimiento | 1:N | Un vehículo puede recibir múltiples servicios de mantenimiento a lo largo de su vida útil, con o sin haber sido vendido aún. |
| Clientes → Mantenimiento | 1:N | Un cliente puede solicitar varios servicios de mantenimiento; el campo es opcional cuando el mantenimiento se realiza sobre un vehículo aún no vendido. |

La tabla `Detalle_Venta` funciona como entidad intermedia entre `Ventas` y `Vehículos`, lo que permite que una misma venta agrupe uno o varios vehículos sin duplicar la información general de la transacción (método de pago, fecha, cliente, vendedor). Cada vehículo, sin embargo, solo puede aparecer una vez en `Detalle_Venta`, reforzando la regla de negocio según la cual un vehículo se vende una única vez.



---

## 5. Restricciones y validaciones

**Restricciones de integridad:**

- Todas las tablas cuentan con una clave primaria definida como campo `SERIAL`, lo que asegura que cada registro sea único e identificable dentro de su entidad.
- Todas las relaciones entre entidades se implementan mediante claves foráneas, impidiendo que se registren ventas, detalles de venta o mantenimientos que hagan referencia a clientes, vendedores o vehículos inexistentes.
- El campo `VIN` de la entidad Vehículos cuenta con la restricción `UNIQUE`, ya que corresponde al número de identificación vehicular, el cual por definición es único a nivel mundial y no puede repetirse entre dos vehículos distintos.
- Los campos que manejan valores monetarios (`Precio`, `Total`, `Precio_Venta`, `Costo`) utilizan el tipo de dato `DECIMAL(12,2)`, lo que garantiza precisión exacta en los cálculos financieros.
- Los campos de fecha se definieron diferenciando entre `DATE` y `TIMESTAMP` según la naturaleza de la información: `Fecha_Contratación` utiliza `DATE`, mientras que `Fecha_Venta` y `Fecha_Del_Servicio` utilizan `TIMESTAMP` porque es relevante conocer también la hora exacta del evento.

**Validaciones de negocio:**

- El atributo `Disponibilidad` de la entidad Vehículos controla si un vehículo puede o no ofrecerse para la venta en un momento determinado. Cuando un vehículo es vendido y se genera su correspondiente registro en `Detalle_Venta`, el valor de `Disponibilidad` debe actualizarse a `FALSE`, evitando así que ese mismo vehículo pueda ofrecerse nuevamente a otro cliente.
- La restricción `UNIQUE` sobre el campo `VIN` impide, desde el propio motor de base de datos, que se registre por error un vehículo duplicado o con un número de identificación vehicular ya existente en el sistema.
- El uso de claves foráneas en las tablas `Ventas`, `Detalle_Venta` y `Mantenimiento` actúa como una validación referencial constante: PostgreSQL rechaza automáticamente cualquier intento de insertar un registro que apunte a un cliente, vendedor o vehículo que no exista previamente en la base de datos.
