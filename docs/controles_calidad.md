# Controles de calidad — Retail Sales & Cost Analytics

## 1. Objetivo

Describir las validaciones necesarias para garantizar que la preparación, agrupación e integración de datos comerciales preserve la información de las fuentes.

## 2. Principios de control

- Conservar los datos originales.
- Evitar modificaciones manuales de importes.
- Mantener trazabilidad de las transformaciones.
- Validar antes de presentar indicadores.
- Investigar valores negativos y anomalías.
- Documentar las limitaciones del modelo.

## 3. Control de integridad

Se comprueba:

- Número de registros importados.
- Número de productos identificados.
- Tipos de datos.
- Valores nulos.
- Errores de transformación.
- Duplicados según la granularidad del reporte.

Los códigos repetidos no se eliminan automáticamente porque pueden representar registros comerciales válidos.

## 4. Control de agrupación

Las consultas por producto deben conservar las sumas de las fuentes originales.

La agrupación se realiza utilizando `Codigo`.

La cantidad de registros resultante corresponde al número de códigos distintos presentes en cada fuente.

## 5. Control de integración

La combinación entre ventas y costos debe comprobar:

- Coincidencia de identificadores.
- Cantidad de filas resultantes.
- Valores de costos ausentes.
- Preservación de importes.
- Diferencias entre cantidades.

La combinación externa izquierda conserva los códigos presentes en ventas.

## 6. Control de precisión numérica

Los cálculos con números decimales pueden presentar residuos de precisión.

Se utiliza redondeo a seis decimales para comparar cantidades.

Los importes monetarios se comparan con una tolerancia definida de 0,01 unidades monetarias.

Esta tolerancia sirve para conciliación técnica y no sustituye una validación contable.

## 7. Control de indicadores comerciales

Se verifica que:

- DiferenciaBruta sea Venta menos Costo.
- El margen se calcule sobre ventas.
- Los productos se clasifiquen de acuerdo con su diferencia.
- Los recuentos de las categorías coincidan con el número de SKU.
- Las tablas dinámicas utilicen las agregaciones correctas.
- Los gráficos reflejen los rankings.

## 8. Casos especiales

Los registros con ventas cero, ventas negativas, costos negativos o diferencias negativas requieren revisión.

No se clasifican automáticamente como pérdidas financieras.

Las posibles causas incluyen ajustes, devoluciones, reclasificaciones o particularidades de las fuentes, que deben verificarse con información adicional.

## 9. Actualización y mantenimiento

Después de cada actualización se comprueba:

1. Carga exitosa de las consultas.
2. Conservación de los campos.
3. Integridad de las fórmulas.
4. Conciliación de los nuevos totales.
5. Actualización de tablas dinámicas.
6. Funcionamiento del dashboard.

Las referencias monetarias de los controles deben actualizarse cuando cambie el conjunto de datos.

## 10. Criterios de aceptación

El proceso puede considerarse técnicamente validado cuando:

- No existen errores de carga sin resolver.
- Los registros y las claves cumplen los controles esperados.
- Las sumas se conservan dentro de la tolerancia establecida.
- Las excepciones están identificadas.
- Los indicadores se actualizan correctamente.
- Las limitaciones y supuestos están documentados.

La aceptación técnica no equivale a una auditoría financiera.

## 11. Versión pública

Los controles de la demostración se ejecutaron con datos sintéticos. En el conjunto validado se registraron 2.089 filas por fuente, 700 SKU y diferencias de cantidades iguales a cero para los 700 SKU. La prueba de actualización desde otra carpeta también se completó correctamente.

Los resultados públicos corresponden exclusivamente a registros e importes sintéticos, no a resultados confidenciales de una empresa real.