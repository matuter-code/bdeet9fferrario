# TRABAJO PRÁCTICO Nº 4: MODELADO LÓGICO Y FÍSICO 

## Problema 1:
- **Transformación de Multivaluado:** `Direcciones de envío` de Cliente pasa a la tabla `DIRECCIONES_CLIENTE` relacionada con `CLIENTE`.
- **Transformación de Compuesto:** `Dirección de envío` en Pedido se aplana en `calle_envio`, `nro_envio`, `comuna_envio`, `ciudad_envio`.
- **Atributo Derivado:** Se descarta `Nº artículos provistos` en Fábrica.

![Respuesta 1](respuesta%201.png)

---

## Problema 2: Proveedores, Categorías, Productos, Ventas y Clientes
- **Transformación de Multivaluado:** `Teléfonos` de Cliente pasa a la tabla `TELEFONO_CLIENTE` vinculada a `CLIENTE`.
- **Transformación de Compuestos:** Las direcciones de Proveedor y Cliente se aplanan en columnas simples (`calle`, `numero`, `comuna`, `ciudad`).
- **Atributo Derivado:** Se descarta `Monto Final` en Venta.

![Respuesta 2](respuesta%202.png)

---

## Problema 3: Sedes, Complejos (Jerarquía), Eventos y Comisarios (Jerarquía)
- **Transformación de Jerarquías:** 
  - `Complejo` se vincula binariamente con `Unideportivo` y `Polideportivo` mediante relaciones `(0,1)` y `(1,1)`.
  - `Comisario` se vincula binariamente con `Juez` y `Observador` mediante relaciones `(0,1)` y `(1,1)`.
- **Transformación de Multivaluado:** `Equipamiento` en Evento pasa a la entidad `EQUIPAMIENTO_EVENTO`.
- **Atributos Derivados:** Se eliminan `#complejos`, `Area total` y `#comisarios`.

![Respuesta 3](respuesta%203.png)

---

## Problema 4: Libros, Autores, Ediciones, Copias y Usuarios
- **Entidad Débil:** `Copia` se modela con clave identificadora primaria y vinculación `(1,1)` a `Edición`.
- **Relaciones N:M:** `autorea` conecta Autor y Libro, y `prestamo` conecta Copia y Usuario manteniendo los atributos `fecha_prestamo` y `fecha_devolucion`.

![Respuesta 4](respuesta%204.png)
