# Retail Sales & Cost Analytics

### Business Intelligence Portfolio | Excel + Power Query

**Estado:** Versión demostrativa con datos sintéticos desarrollada, validada y preparada para publicación.

## 1. Descripción del proyecto

Retail Sales & Cost Analytics es un proyecto de análisis comercial orientado a empresas del sector retail que necesitan consolidar información de ventas y costos, automatizar transformaciones y visualizar indicadores relevantes para la gestión.

La solución se desarrolla mediante Microsoft Excel, Power Query, fórmulas y tablas dinámicas. La versión pública utiliza datos sintéticos con códigos `DEMO-0001` a `DEMO-0700`; no requiere los datos empresariales originales.

## 2. Problema de negocio

Muchas empresas administran su información comercial mediante reportes Excel independientes, lo que dificulta consolidar registros por producto, comparar ventas y costos, identificar productos que requieren revisión, actualizar indicadores y verificar la consistencia de los resultados.

## 3. Objetivo de la solución

Desarrollar un flujo reproducible para importar, limpiar, transformar, integrar y analizar información comercial, con controles de conciliación y un dashboard ejecutivo en Excel.

## 4. Tecnologías utilizadas

| Tecnología | Aplicación |
|---|---|
| Microsoft Excel | Análisis comercial y dashboard |
| Power Query (lenguaje M) | Limpieza, transformación e integración |
| Tablas dinámicas | Rankings y análisis por producto |
| Fórmulas de Excel | Indicadores y controles de calidad |
| GitHub | Publicación y documentación del portafolio |

## 5. Funcionalidades desarrolladas

- Preparación y normalización de dos fuentes sintéticas de datos.
- Consolidación de registros por código de producto.
- Integración de ventas y costos mediante Power Query.
- Conciliación de cantidades e importes.
- Cálculo de diferencia bruta (`Venta - Costo`) y margen estimado (`DiferenciaBruta / Venta`, cuando la venta es distinta de cero).
- Identificación de productos con diferencia bruta negativa, positiva o cero.
- Rankings Top 10 por ventas y por diferencia bruta.
- Dashboard ejecutivo con indicadores, gráficos y alertas.
- Actualización de consultas mediante un parámetro configurable.

## 6. Arquitectura general

`ventas_demo.xlsx` + `costos_demo.xlsx` → Power Query (limpieza y agregación) → integración por producto (`RevisionPQ`) → tablas de Excel → indicadores y tablas dinámicas → dashboard ejecutivo.

El parámetro **`pRutaDatosDemo`** identifica la carpeta que contiene ambas fuentes sintéticas.

## 7. Validación técnica y resultados DEMO

La versión demostrativa fue comprobada mediante controles de registros, coincidencia de códigos, conciliación de cantidades, conservación de importes y clasificación de diferencias comerciales.

| Indicador | Resultado validado |
|---|---:|
| Registros en ventas | 2.089 |
| Registros en costos | 2.089 |
| Productos (SKU) | 700 |
| Ventas totales | 26.305.478.127 |
| Costos totales | 20.067.403.143 |
| Diferencia bruta total | 6.238.074.984 |
| Margen estimado total | 23,71 % |
| Productos con diferencia negativa | 22 |
| Productos con diferencia positiva | 676 |
| Productos con diferencia cero | 2 |

**Nota:** Las magnitudes monetarias corresponden a datos sintéticos; no representan resultados empresariales reales. La diferencia bruta y el margen son métricas analíticas estimadas, no equivalentes necesariamente a utilidad contable. Su interpretación financiera depende de criterios contables, tributarios y temporales.

## 8. Dashboard ejecutivo

El libro incluye indicadores de ventas, costos, diferencia bruta, margen estimado, rankings Top 10 y alertas sobre productos que requieren revisión.

Las capturas públicas podrán incorporarse en `assets/screenshots/` después de verificar que muestran exclusivamente información sintética.

## 9. Organización del repositorio

```text
retail-sales-cost-analytics/
├── README.md
├── .gitignore
├── data-demo/
│   ├── ventas_demo.xlsx
│   └── costos_demo.xlsx
├── dashboard-demo/
│   └── Retail_Sales_Cost_Analytics_DEMO.xlsx
├── docs/
│   └── (documentación técnica del proyecto)
└── assets/
    └── screenshots/  (capturas públicas opcionales)
```

### Requisitos

- Microsoft Excel para Windows con Power Query y soporte para consultas y tablas dinámicas.
- Permisos de lectura sobre los archivos de `data-demo/`.

### Instalación y actualización

1. Descarga o clona este repositorio en una carpeta local.
2. Comprueba que `data-demo/` contiene `ventas_demo.xlsx` y `costos_demo.xlsx`.
3. Abre `dashboard-demo/Retail_Sales_Cost_Analytics_DEMO.xlsx` en Excel.
4. En **Datos → Consultas y conexiones**, abre el Editor de Power Query y selecciona el parámetro **`pRutaDatosDemo`**.
5. Cambia su **Valor actual** a la **ruta absoluta local** de la carpeta `data-demo`, incluyendo la barra invertida final (`\`). Por ejemplo, `D:\Proyectos\retail-sales-cost-analytics\data-demo\` (ruta ilustrativa; no tiene por qué existir en tu equipo).
6. Aplica los cambios mediante **Cerrar y cargar** y ejecuta **Datos → Actualizar todo**.
7. Verifica los indicadores y los controles de calidad del libro con los resultados de la tabla anterior.

**Importante:** El valor de distribución del parámetro (`C:\Retail-Sales-Cost-Analytics\data-demo\`) es un ejemplo genérico. La ubicación **no se detecta automáticamente**: cada usuario debe configurar `pRutaDatosDemo` antes de actualizar las consultas. Si Excel muestra `DataSource.NotFound`, comprueba que el parámetro apunta a `data-demo/` y no a `dashboard-demo/`.

## 10. Confidencialidad

Los datos empresariales originales y los archivos privados de trabajo quedan fuera del repositorio. Los archivos incluidos en `data-demo/` y el libro demostrativo están destinados exclusivamente a demostrar el funcionamiento de la solución mediante información sintética. Antes de incorporar nuevos archivos, deben revisarse sus contenidos, conexiones y metadatos.

## 11. Autor

**Cristian Vargas**

Portafolio profesional de análisis de datos, automatización de reportes y Business Intelligence.

*Proyecto desarrollado con fines de aprendizaje, demostración técnica y presentación de servicios profesionales.*
