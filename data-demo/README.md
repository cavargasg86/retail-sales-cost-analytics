# Datos de demostración

Esta carpeta contiene los archivos de datos sintéticos utilizados para reproducir el análisis del proyecto **Retail Sales & Cost Analytics**.

Los datos no representan operaciones reales de una empresa ni contienen información comercial original.

## Archivos incluidos

| Archivo | Descripción | Registros |
|---|---|---:|
| `ventas_demo.xlsx` | Información sintética de ventas por producto | 2.089 |
| `costos_demo.xlsx` | Información sintética de costos por producto | 2.089 |

## Características de los datos

- **700 productos sintéticos**, identificados desde `DEMO-0001` hasta `DEMO-0700`.
- Registros preparados para su integración mediante Power Query.
- Códigos de producto compatibles entre ambas fuentes.
- Cantidades conciliadas por producto.
- Importes utilizados exclusivamente para demostración analítica.

## Uso de los archivos

Ambos archivos son las fuentes de información del libro ubicado en `dashboard-demo/`.

Para actualizar el análisis, es necesario configurar el parámetro `pRutaDatosDemo` en Power Query con la ruta local de esta carpeta.

Las instrucciones completas de instalación se encuentran en el `README.md` principal del repositorio.

## Confidencialidad

Todos los registros son sintéticos. No se incluyen archivos empresariales originales ni información comercial privada.