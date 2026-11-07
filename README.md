# TRABAJO PRÁCTICO Nº 4: MODELADO LÓGICO Y FÍSICO 

Este repositorio contiene la resolución integral del **Trabajo Práctico Nº 4**, abordando la transición desde el Modelo Entidad-Relación Conceptual hacia el **Modelo Lógico** (diseñado en Draw.io) y el **Modelo Físico Relacional** (diseñado con dbdiagram.io y tipado estático según los lineamientos de la cátedra).

---

## 📌 Guía Teórica y Reglas de Transformación

### 1. Transformaciones al Modelo Lógico
- **Atributos Polivalentes (Multivaluados):** No existen en el modelo lógico; se convierten en una tabla independiente con relación `(1,1)` a `(1,n)` respecto a la entidad origen.
- **Atributos Compuestos:** No existen en el modelo lógico; se descomponen en sus atributos atómicos simples dentro de la misma tabla (o se crean como tabla independiente según convenga).
- **Jerarquías de Especialización/Generalización:** No existen en el modelo lógico; se transforman en relaciones binarias con cardinalidad `(0,1)` en la superentidad y `(1,1)` en las subentidades.
- **Atributos Derivados:** Se excluyen del modelo lógico relacional, ya que se calculan en tiempo de ejecución.

### 2. Reglas del Modelo Físico y Tipado Estático
- **Tipado estático aplicado:** `VARCHAR`, `INTEGER`, `LONG`, `FLOAT`, `BIGFLOAT`, `BOOLEAN`, `DATE`, `TIMESTAMP`.
- **Relaciones Uno a Muchos (1:N):** La clave primaria (`PK`) del lado "1" se propaga como clave foránea (`FK`) en la tabla del lado "N".
- **Relaciones Muchos a Muchos (N:M):** Se resuelven creando una tabla intermedia (tabla puente) con claves foráneas apuntando a ambas entidades principales, más sus atributos propios.
- **Relaciones Uno a Uno (1:1) y Jerarquías:** Se modelan mediante tablas separadas donde la `PK` de la entidad padre pasa como `[PK, FK]` a las entidades hijas especializadas.

---

## 🚀 Problema 1: Clientes, Pedidos, Artículos y Fábricas

### 1. Modelo Lógico
- **Multivaluado `Direcciones de envío` de Cliente:** Pasa a la tabla `DIRECCIONES_CLIENTE` vinculada `(1,1)` a `(1,n)` con `CLIENTE`.
- **Compuesto `Dirección de envío` en Pedido:** Se aplana en `calle_envio`, `nro_envio`, `comuna_envio`, `ciudad_envio`.
- **Derivado `Nº artículos provistos`:** Se elimina del modelo lógico.

![Respuesta 1 - Modelo Lógico](respuesta%201.png)

### 2. Modelo Físico (dbdiagram.io)
```sql
CLIENTE = ( id_cliente (PK): LONG, saldo: BIGFLOAT, limite_credito: BIGFLOAT, descuento: FLOAT )
DIRECCIONES_CLIENTE = ( id_dir (PK): LONG, cliente_id (FK): LONG, calle: VARCHAR(255), numero: VARCHAR(20), comuna: VARCHAR(100), ciudad: VARCHAR(100) )
PEDIDO = ( id_pedido (PK): LONG, cliente_id (FK): LONG, fecha: TIMESTAMP, calle_envio: VARCHAR(255), nro_envio: VARCHAR(20), comuna_envio: VARCHAR(100), ciudad_envio: VARCHAR(100) )
ARTICULO = ( id_articulo (PK): LONG, descripcion: VARCHAR(255) )
PEDIDO_ARTICULO = ( id (PK): LONG, pedido_id (FK): LONG, articulo_id (FK): LONG, cantidad: INTEGER )
FABRICA = ( id_fabrica (PK): LONG, telefono: VARCHAR(50) )
ARTICULO_FABRICA = ( id (PK): LONG, articulo_id (FK): LONG, fabrica_id (FK): LONG, existencias: INTEGER )
```

![Respuesta 1 - Modelo Físico](respuesta%201%20modelado%20fisico.png)

---

## 🚀 Problema 2: Proveedores, Categorías, Productos, Ventas y Clientes

### 1. Modelo Lógico
- **Multivaluado `Teléfonos` de Cliente:** Pasa a la tabla `TELEFONO_CLIENTE` con relación `(1,1)` a `(1,n)` con `CLIENTE`.
- **Compuestos `Dirección` en Proveedor y Cliente:** Aplanados a columnas atómicas (`calle`, `numero`, `comuna`, `ciudad`).
- **Derivado `Monto Final` en Venta:** Descartado del modelo relacional.

![Respuesta 2 - Modelo Lógico](respuesta%202.png)

### 2. Modelo Físico (dbdiagram.io)
```sql
CATEGORIA = ( id_categoria (PK): LONG, nombre: VARCHAR(100), descripcion: VARCHAR(255) )
PROVEEDOR = ( rut (PK): LONG, nombre: VARCHAR(150), web: VARCHAR(255), telefono: VARCHAR(50), calle: VARCHAR(255), numero: VARCHAR(20), comuna: VARCHAR(100), ciudad: VARCHAR(100) )
PRODUCTO = ( id_producto (PK): LONG, categoria_id (FK): LONG, proveedor_rut (FK): LONG, nombre: VARCHAR(150), precio: BIGFLOAT, stock: INTEGER )
CLIENTE = ( rut (PK): LONG, nombre: VARCHAR(150), calle: VARCHAR(255), numero: VARCHAR(20), comuna: VARCHAR(100), ciudad: VARCHAR(100) )
TELEFONO_CLIENTE = ( id_tel (PK): LONG, cliente_rut (FK): LONG, telefono: VARCHAR(50) )
VENTA = ( id_venta (PK): LONG, cliente_rut (FK): LONG, fecha: TIMESTAMP, descuento: FLOAT )
DETALLE_VENTA = ( id (PK): LONG, venta_id (FK): LONG, producto_id (FK): LONG, cantidad: INTEGER )
```

![Respuesta 2 - Modelo Físico](respuesta%202%20modelado%20fisico.png)

---

## 🚀 Problema 3: Sedes, Complejos (Jerarquía), Eventos y Comisarios (Jerarquía)

### 1. Modelo Lógico
- **Jerarquías:** Transformadas en relaciones binarias `(0,1)` a `(1,1)`:
  - `Complejo` con `Unideportivo` y `Polideportivo`.
  - `Comisario` con `Juez` y `Observador`.
- **Multivaluado `Equipamiento` en Evento:** Pasa a la tabla `EQUIPAMIENTO_EVENTO` vinculada `(1,n)` a `(1,1)`.
- **Derivados eliminados:** `#complejos` en Sede, `Area total` en Complejo y `#comisarios` en Evento.

![Respuesta 3 - Modelo Lógico](respuesta%203.png)

### 2. Modelo Físico (dbdiagram.io)
```sql
SEDE = ( id_sede (PK): LONG, presupuesto: BIGFLOAT )
COMPLEJO = ( id_complejo (PK): LONG, sede_id (FK): LONG, localizacion: VARCHAR(255), jefe: VARCHAR(150) )
UNIDEPORTIVO = ( id_complejo (PK, FK): LONG )
POLIDEPORTIVO = ( id_complejo (PK, FK): LONG )
AREA = ( id_area (PK): LONG, polideportivo_id (FK): LONG, localizacion: VARCHAR(255) )
EVENTO = ( id_evento (PK): LONG, complejo_id (FK): LONG, fecha: TIMESTAMP, duracion: INTEGER, num_participantes: INTEGER )
EQUIPAMIENTO_EVENTO = ( id_equip (PK): LONG, evento_id (FK): LONG, descripcion: VARCHAR(255) )
COMISARIO = ( id_comisario (PK): LONG, nombre: VARCHAR(150) )
EVENTO_COMISARIO = ( id (PK): LONG, evento_id (FK): LONG, comisario_id (FK): LONG )
JUEZ = ( id_comisario (PK, FK): LONG )
OBSERVADOR = ( id_comisario (PK, FK): LONG )
```

![Respuesta 3 - Modelo Físico](respuesta%203%20modelado%20fisico.png)

---

## 🚀 Problema 4: Libros, Autores, Ediciones, Copias y Usuarios

### 1. Modelo Lógico
- **Entidad débil `Copia`:** Identificador primario y relación identificadora estricta `(1,1)` a `(1,n)` con `EDICION`.
- **Relaciones N:M:** `autorea` conecta Autor y Libro, y `prestamo` conecta Copia y Usuario manteniendo los atributos `fecha_prestamo` y `fecha_devolucion`.

![Respuesta 4 - Modelo Lógico](respuesta%204.png)

### 2. Modelo Físico (dbdiagram.io)
```sql
AUTOR = ( id_autor (PK): LONG, nombre: VARCHAR(150) )
LIBRO = ( id_libro (PK): LONG, titulo: VARCHAR(255) )
AUTOR_LIBRO = ( id (PK): LONG, autor_id (FK): LONG, libro_id (FK): LONG )
EDICION = ( isbn (PK): LONG, libro_id (FK): LONG, año: INTEGER, idioma: VARCHAR(50) )
COPIA = ( id_copia (PK): LONG, edicion_isbn (FK): LONG, numero: INTEGER )
USUARIO = ( rut (PK): LONG, nombre: VARCHAR(150) )
PRESTAMO = ( id_prestamo (PK): LONG, copia_id (FK): LONG, usuario_rut (FK): LONG, fecha_prestamo: TIMESTAMP, fecha_devolucion: TIMESTAMP )
```

![Respuesta 4 - Modelo Físico](respuesta%204%20modelado%20fisico.png)

---

## 💻 Código DBML para dbdiagram.io
El archivo [dbdiagram_models.dbml](dbdiagram_models.dbml) contiene el código fuente en sintaxis DBML listo para ser copiado y pegado en [dbdiagram.io](https://dbdiagram.io/).
