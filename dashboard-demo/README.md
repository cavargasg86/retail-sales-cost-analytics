# Dashboard de demostración

Esta carpeta contiene la versión pública de **Retail Sales & Cost Analytics**, desarrollada en Microsoft Excel con Power Query, tablas dinámicas y datos sintéticos.

## Archivo principal

`Retail_Sales_Cost_Analytics_DEMO.xlsx`

El libro permite consolidar ventas y costos, analizar resultados por producto y visualizar indicadores comerciales mediante un dashboard ejecutivo.

## Hojas del libro

| Hoja | Función |
|---|---|
| `Dashboard` | Indicadores ejecutivos, gráficos y alertas comerciales |
| `Control` | Validaciones de integridad y conciliación |
| `Analisis` | Resultados y métricas por producto |
| `Ventas` | Datos de ventas procesados |
| `Costos` | Datos de costos procesados |
| `TablasDinamicas` | Rankings y análisis complementarios |

## Indicadores principales

- Ventas y costos totales.
- Diferencia bruta y margen estimado.
- Rankings Top 10 por ventas y diferencia bruta.
- Productos con diferencias comerciales negativas.
- Controles de cantidades y consistencia de datos.

La demostración incluye 700 productos sintéticos.

## Configuración de Power Query

El dashboard obtiene sus datos de los archivos `ventas_demo.xlsx` y `costos_demo.xlsx`, ubicados en la carpeta `data-demo` del repositorio.

Para actualizar las consultas:

1. Descarga o clona el repositorio completo.
2. Abre el archivo DEMO en Microsoft Excel para Windows.
3. Accede a **Datos → Consultas y conexiones** y abre el Editor de Power Query.
4. Selecciona el parámetro `pRutaDatosDemo`.
5. Cambia su valor por la ruta completa de la carpeta local `data-demo`, incluyendo la barra invertida final (`\`).
6. Aplica los cambios mediante **Cerrar y cargar**.
7. Ejecuta **Datos → Actualizar todo**.
8. Comprueba los resultados en las hojas `Control` y `Dashboard`.

**Importante:** la ruta de datos no se detecta automáticamente. Si aparece `DataSource.NotFound`, verifica que el parámetro apunte a `data-demo` y no a `dashboard-demo`.

## Datos y confidencialidad

El libro está preparado para trabajar con información sintética. Los resultados son demostrativos y no representan operaciones financieras reales.

La documentación general, los resultados validados y las instrucciones adicionales se encuentran en el `README.md` principal del repositorio.