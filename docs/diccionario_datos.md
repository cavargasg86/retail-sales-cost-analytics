# Diccionario de datos — Retail Sales & Cost Analytics

## 1. Objetivo

Documentar la estructura de las tablas utilizadas en el proyecto, incluyendo nombres de campos, tipos de datos, reglas de transformación y significado de los indicadores comerciales.

La documentación corresponde al modelo técnico desarrollado. Los datos empresariales originales no forman parte del repositorio público.

## 2. Convenciones utilizadas

- Los identificadores de productos se almacenan como texto.
- Los nombres de columnas utilizan el formato PascalCase.
- Las cantidades e importes se almacenan como números decimales.
- Los códigos de producto se normalizan eliminando espacios y caracteres de control, y convirtiéndolos a mayúsculas.
- Los valores nulos se conservan cuando representan información ausente.
- No se eliminan registros duplicados sin estudiar su significado comercial.

## 3. Tabla de ventas — tblVentas

**Consulta:** VentasLimpias

**Destino local:** Hoja Ventas

**Granularidad:** registro de ventas reportado por producto y sucursal, según la estructura de la fuente.

| Campo | Tipo | Descripción |
|---|---|---|
| SucursalCodigo | Texto | Código de la sucursal |
| Sucursal | Texto | Nombre de la sucursal |
| Codigo | Texto | Identificador del producto |
| Producto | Texto | Descripción comercial |
| CantidadVenta | Decimal | Cantidad reportada como vendida |
| CantidadVenta2 | Decimal | Segunda cantidad conservada para trazabilidad |
| Venta | Decimal | Importe de ventas reportado |

### Transformaciones

1. Eliminación de filas introductorias.
2. Promoción de encabezados.
3. Asignación de tipos de datos.
4. Renombrado de columnas.
5. Limpieza de identificadores y descripciones.

## 4. Tabla de costos — tblCostos

**Consulta:** CostosLimpios

**Destino local:** Hoja Costos

**Granularidad:** registro de costo reportado por producto, según la fuente.

| Campo | Tipo | Descripción |
|---|---|---|
| Codigo | Texto | Identificador del producto |
| Producto | Texto | Descripción comercial |
| CantidadCosto | Decimal | Cantidad asociada al reporte de costos |
| CantidadCosto2 | Decimal | Segunda cantidad conservada para trazabilidad |
| Costo | Decimal | Importe de costo reportado |
| Superfamilia | Texto | Clasificación comercial superior |
| Familia | Texto | Familia del producto |
| Subfamilia | Texto | Subfamilia del producto |
| Rubro | Texto | Campo opcional de clasificación |
| Marca | Texto | Campo opcional de marca |

### Consideraciones

Rubro y Marca pueden encontrarse completamente vacíos en los archivos de origen.

Se conservan en el modelo de datos para mantener el esquema original.

## 5. Consulta VentasPorProducto

**Tipo:** consulta auxiliar.

**Destino:** solo conexión.

**Clave:** Codigo.

| Campo | Tipo | Regla |
|---|---|---|
| Codigo | Texto | Agrupación |
| CantidadVenta | Decimal | Suma por código |
| Venta | Decimal | Suma por código |

Su finalidad es obtener una fila por producto antes de la integración.

## 6. Consulta CostosPorProducto

**Tipo:** consulta auxiliar.

**Destino:** solo conexión.

**Clave:** Codigo.

| Campo | Tipo | Regla |
|---|---|---|
| Codigo | Texto | Agrupación |
| CantidadCosto | Decimal | Suma por código |
| Costo | Decimal | Suma por código |

## 7. Tabla integrada — tblAnalisis

**Consulta de origen:** RevisionPQ

**Destino local:** Hoja Analisis

**Granularidad:** una fila por código de producto.

| Campo | Tipo | Descripción |
|---|---|---|
| Codigo | Texto | Identificador del producto |
| CantidadVenta | Decimal | Cantidad total reportada de ventas |
| Venta | Decimal | Importe total reportado de ventas |
| CantidadCosto | Decimal | Cantidad total reportada de costos |
| Costo | Decimal | Importe total reportado de costos |
| DiferenciaCantidad | Decimal | Diferencia de cantidades, redondeada a seis decimales |
| DiferenciaBruta | Decimal | Venta menos Costo |
| MargenEstimadoPct | Porcentaje | DiferenciaBruta dividida por Venta |
| EstadoMargen | Texto | Clasificación según DiferenciaBruta |

### Columnas calculadas en Excel

**DiferenciaBruta**

```excel
=[@Venta]-[@Costo]
```

**MargenEstimadoPct**

```excel
=SI.ERROR([@DiferenciaBruta]/[@Venta];0)
```

**EstadoMargen**

```excel
=SI([@DiferenciaBruta]<0;"NEGATIVO";SI([@DiferenciaBruta]>0;"POSITIVO";"CERO"))
```

## 8. Consulta ControlTotalesPQ

**Tipo:** control técnico.

**Destino:** solo conexión.

| Campo | Tipo | Descripción |
|---|---|---|
| TotalVentas | Decimal | Suma de ventas integradas |
| TotalCostos | Decimal | Suma de costos integrados |
| TotalCantidadVentas | Decimal | Suma de cantidades de ventas |
| TotalCantidadCostos | Decimal | Suma de cantidades de costos |
| DiferenciaVentas | Decimal | Diferencia frente al total de referencia |
| DiferenciaCostos | Decimal | Diferencia frente al total de referencia |

Los controles de diferencias utilizan totales de referencia previamente conciliados. Para la versión DEMO se emplean referencias correspondientes al conjunto sintético validado; si se sustituyen las fuentes, es necesario revisar y actualizar dichas referencias.

## 9. Indicadores comerciales

| Indicador | Cálculo | Interpretación |
|---|---|---|
| Ventas totales | SUMA(Venta) | Ventas reportadas |
| Costos totales | SUMA(Costo) | Costos reportados |
| Diferencia bruta | Ventas - Costos | Diferencia comercial |
| Margen estimado | Diferencia bruta / Ventas | Indicador relativo sujeto a validación |
| SKU analizados | Recuento de códigos | Productos del modelo |
| SKU negativos | DiferenciaBruta menor que 0 | Productos que requieren revisión |
| SKU positivos | DiferenciaBruta mayor que 0 | Productos con diferencia positiva |
| SKU en cero | DiferenciaBruta igual a 0 | Productos sin diferencia |

## 10. Reglas de calidad

- Los campos Codigo deben permitir la identificación de los productos.
- Los importes deben conservar los decimales de origen.
- La agrupación no debe modificar los totales monetarios.
- La integración debe revisarse por coincidencias y valores nulos.
- Las diferencias de cantidades deben considerar tolerancias numéricas.
- Las cantidades y los importes negativos no se eliminan automáticamente.
- Los indicadores deben actualizarse junto con los datos.

## 11. Limitaciones del modelo

El diccionario documenta la estructura técnica, no certifica la naturaleza contable de los importes.

No se ha comprobado formalmente si las ventas y costos incluyen impuestos, ni la interpretación exacta de los campos secundarios de cantidad.

El margen no está definido para ventas iguales a cero. La fórmula implementada devuelve cero como convención provisional de visualización.

Las cifras deben interpretarse dentro del alcance del reporte y no como resultados financieros auditados.

## 12. Privacidad

Este documento describe la estructura y las transformaciones sin incorporar registros comerciales privados.

La versión pública del proyecto utiliza datos sintéticos. Los archivos originales privados no están incluidos.