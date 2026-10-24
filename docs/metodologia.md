# Metodología ETL — Retail Sales & Cost Analytics

## 1. Objetivo

Documentar el procedimiento de extracción, transformación, integración y carga de información comercial utilizado en la solución.

El proyecto implementa un flujo ETL mediante Microsoft Excel y Power Query, orientado al análisis de ventas y costos por código de producto.

## 2. Herramientas

- Microsoft Excel.
- Power Query.
- Lenguaje M.
- Tablas estructuradas de Excel.
- Tablas dinámicas.

## 3. Extracción de datos

Se utilizan dos fuentes principales:

**Fuente de ventas:** reporte Excel con registros comerciales por producto y sucursal.

**Fuente de costos:** reporte Excel con cantidades, importes y clasificaciones de productos.

En la versión DEMO, `ventas_demo.xlsx` y `costos_demo.xlsx` se publican en `data-demo/`. Los archivos empresariales originales permanecen fuera del repositorio público.

Las fuentes se importan desde:

Datos → Obtener datos → Desde archivo → Desde libro de Excel.

Se selecciona Transformar datos para realizar las operaciones dentro de Power Query.

## 4. Transformación de ventas

Consulta: `VentasLimpias`.

Procedimiento:

1. Importar la hoja correspondiente.
2. Identificar el encabezado real.
3. Eliminar las filas introductorias.
4. Promover los encabezados.
5. Asignar tipos de datos.
6. Renombrar los campos.
7. Recortar y limpiar identificadores.
8. Convertir códigos a mayúsculas.
9. Verificar nulos, errores y cantidades de registros.

Los registros no se eliminan únicamente por presentar valores negativos o repetidos.

## 5. Transformación de costos

Consulta: `CostosLimpios`.

Procedimiento:

1. Importar el reporte.
2. Identificar encabezados.
3. Eliminar filas introductorias.
4. Configurar tipos de datos.
5. Estandarizar nombres de columnas.
6. Limpiar códigos y textos.
7. Conservar las clasificaciones disponibles.
8. Identificar campos incompletos.
9. Revisar integridad de cantidades e importes.

Los campos opcionales sin información se conservan para mantener el esquema original.

## 6. Agrupación por producto

Se crean dos consultas auxiliares:

### VentasPorProducto

Agrupa por `Codigo` y calcula:

- Suma de `CantidadVenta`.
- Suma de `Venta`.

### CostosPorProducto

Agrupa por `Codigo` y calcula:

- Suma de `CantidadCosto`.
- Suma de `Costo`.

La agrupación se realiza antes de combinar los reportes, para evitar multiplicar registros y montos en una relación muchos a muchos.

## 7. Integración

Consulta: `RevisionPQ`.

Se utiliza una combinación externa izquierda mediante el campo `Codigo`.

La consulta conserva los productos presentes en ventas e incorpora:

- CantidadCosto.
- Costo.

La integridad de los resultados se comprueba posteriormente.

## 8. Control de cantidades

Se incorpora la columna `DiferenciaCantidad`:

```powerquery
if [CantidadVenta] = null or [CantidadCosto] = null
then null
else Number.Round([CantidadVenta] - [CantidadCosto], 6)
```

La fórmula permite controlar pequeñas diferencias de precisión decimal.

Los registros con diferencias significativas deben analizarse sin modificar automáticamente los valores originales.

## 9. Conciliación monetaria

La consulta `ControlTotalesPQ` calcula:

- TotalVentas.
- TotalCostos.
- TotalCantidadVentas.
- TotalCantidadCostos.

Posteriormente compara los importes con referencias previamente validadas.

Las diferencias monetarias se redondean a dos decimales para aplicar una tolerancia de control.

Las referencias de conciliación deben corresponder al conjunto de datos utilizado y no reutilizarse automáticamente al cambiar las fuentes.

## 10. Carga en Excel

| Consulta | Destino |
|---|---|
| VentasLimpias | Tabla `tblVentas` |
| CostosLimpios | Tabla `tblCostos` |
| VentasPorProducto | Solo conexión |
| CostosPorProducto | Solo conexión |
| RevisionPQ | Tabla `tblAnalisis` |
| ControlTotalesPQ | Solo conexión |

## 11. Cálculos comerciales

En `tblAnalisis` se agregan:

**DiferenciaBruta**

Venta menos Costo.

**MargenEstimadoPct**

DiferenciaBruta dividida por Venta.

**EstadoMargen**

Clasificación POSITIVO, NEGATIVO o CERO según DiferenciaBruta.

Estos indicadores representan resultados analíticos sujetos a validación de criterios tributarios, contables y temporales.

## 12. Presentación de resultados

Se construyen tres tablas dinámicas:

1. Top 10 productos por ventas.
2. Top 10 productos por diferencia bruta.
3. Productos con diferencia bruta negativa.

Finalmente se genera un dashboard ejecutivo con indicadores, gráficos y alertas comerciales.

## 13. Actualización

Para actualizar la solución:

1. Descargar o clonar el repositorio completo.
2. Comprobar que `data-demo/` contiene `ventas_demo.xlsx` y `costos_demo.xlsx`.
3. Abrir `dashboard-demo/Retail_Sales_Cost_Analytics_DEMO.xlsx` con Microsoft Excel para Windows.
4. Abrir el Editor de Power Query desde **Datos → Consultas y conexiones** y seleccionar `pRutaDatosDemo`.
5. Configurar su valor con la ruta absoluta local de `data-demo/`, terminada en `\`.
6. Aplicar con **Cerrar y cargar** y ejecutar **Datos → Actualizar todo**.
7. Revisar las hojas `Control`, `Analisis`, `TablasDinamicas` y `Dashboard` para comprobar recuentos, conciliaciones y visualizaciones.

La ubicación de las fuentes no se detecta automáticamente. Si aparece `DataSource.NotFound`, comprobar que el parámetro apunta a `data-demo/` y no a `dashboard-demo/`.

## 14. Resultados de validación DEMO

Para la versión sintética comprobada se obtuvieron 2.089 registros por fuente, 700 SKU, ventas totales de 26.305.478.127, costos de 20.067.403.143 y una diferencia bruta de 6.238.074.984. El margen estimado total fue 23,71 %, con 22 SKU negativos, 676 positivos y 2 con diferencia cero.

Estas cifras corresponden únicamente al conjunto demostrativo, no a resultados financieros empresariales reales.