# Documentación técnica

Esta carpeta reúne la arquitectura, el diccionario de datos, la metodología ETL y los controles de calidad de **Retail Sales & Cost Analytics**.

## Documentos

- [Arquitectura técnica](arquitectura.md): consultas, integración por SKU, modelo de análisis y dependencias.
- [Diccionario de datos](diccionario_datos.md): campos, tipos, fórmulas y significados comerciales.
- [Metodología ETL](metodologia.md): extracción, limpieza, agrupación, integración, actualización y resultados DEMO.
- [Controles de calidad](controles_calidad.md): integridad, conciliaciones, tolerancias y criterios de aceptación.

## Alcance de la versión pública

El proyecto utiliza dos fuentes sintéticas publicadas en `data-demo/` y un libro demostrativo ubicado en `dashboard-demo/`. Incluye 2.089 registros por fuente y 700 códigos de producto sintéticos.

Las cifras y los márgenes son indicadores analíticos demostrativos; no corresponden a resultados financieros reales ni sustituyen validaciones contables o tributarias.

Para instrucciones de instalación, consultar el [README principal](../README.md). El parámetro `pRutaDatosDemo` debe apuntar a la ubicación local de `data-demo/` antes de actualizar.
