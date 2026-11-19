# union-retailchain


# README – Módulo 5: UNION vs UNION ALL

## 1. ¿Cuántas filas devuelve cada consulta y por qué son distintas?
`UNION` devuelve 11 filas y `UNION ALL` devuelve 14. La diferencia es que `UNION`
elimina las filas idénticas en todas sus columnas. Se eliminaron 3:
- 103 Monitor 4K 27" (Computación)
- 104 Teclado Mecánico (Accesorios)
- 106 SSD Externo 1TB (Almacenamiento)

Están en ambas sucursales con los mismos valores. La Webcam HD 1080p no se eliminó
porque tiene un ID distinto en cada sucursal (107 y 111), así que sus filas no son idénticas.

## 2. ¿Por qué UNION ALL es más eficiente que UNION?
`UNION ALL` solo apila los resultados. `UNION` además elimina duplicados: para eso
tiene que ordenar o comparar todas las filas entre sí, lo que consume más memoria y
tiempo. En tablas grandes la diferencia es significativa.

## 3. ¿En qué casos de negocio usarías cada uno?
**UNION** (cuando necesito valores únicos):
- Lista de emails para una campaña combinando clientes online y de tienda física,
  para que nadie reciba el mail dos veces.
- Lista de ciudades donde hay clientes o proveedores, sin repetir.

**UNION ALL** (cuando cada registro debe contarse):
- Facturación total combinando las tablas de ventas de 2024 y 2025.
- Cierre contable consolidando movimientos de varias cajas: eliminar un movimiento
  "repetido" dejaría mal el saldo.

## 4. ¿Qué pasa si las columnas no coinciden en número o tipo?
- **Distinta cantidad de columnas:** SQL Server no ejecuta la consulta.
  Error: `Msg 205 – All queries combined using a UNION, INTERSECT or EXCEPT
  operator must have an equal number of expressions in their target lists.`
- **Tipos incompatibles:** SQL Server intenta convertir un tipo al otro y, si no
  puede, falla. Ejemplo uniendo `nombre_producto` (texto) con `id_producto` (número):
  `Msg 245 – Conversion failed when converting the varchar value 'Laptop Pro 15'
  to data type int.`
