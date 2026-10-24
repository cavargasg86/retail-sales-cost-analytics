# Arquitectura técnica — Retail Sales & Cost Analytics

## 1. Propósito

Documentar la arquitectura de transformación, integración, validación y presentación de datos comerciales desarrollada mediante Microsoft Excel y Power Query.

La solución utiliza archivos de ventas y costos, consultas dependientes y tablas estructuradas para alimentar indicadores y visualizaciones.

## 2. Tecnologías

| Componente | Tecnología |
|---|---|
| Fuentes DEMO publicadas | Microsoft Excel (`ventas_demo.xlsx`, `costos_demo.xlsx`) |
| Transformación | Power Query (lenguaje M) |
| Integración | Combinación de consultas por código |
| Cálculos comerciales | Fórmulas estructuradas de Excel |
| Análisis | Tablas dinámicas |
| Visualización | Dashboard de Excel |

## 3. Flujo general de datos

```text
ARCHIVOS DE ORIGEN
│
├── Archivo de ventas
│   └── VentasLimpias
│       └── VentasPorProducto
│
└── Archivo de costos
    └── CostosLimpios
        └── CostosPorProducto

INTEGRACIÓN
│
├── VentasPorProducto
├── CostosPorProducto
│
└── RevisionPQ
    ├── DiferenciaCantidad
    └── ControlTotalesPQ

CAPA DE EXCEL
│
├── Ventas
├── Costos
├── Analisis
│   ├── DiferenciaBruta
│   ├── MargenEstimadoPct
│   └── EstadoMargen
│
├── Control
└── TablasDinamicas
    └── Dashboard
```

## 4. Descripción de consultas

### VentasLimpias

Importa y transforma información de ventas.

Operaciones principales:

- Eliminación de filas introductorias.
- Promoción de encabezados.
- Configuración de tipos de datos.
- Estandarización de códigos.
- Limpieza de campos de texto.

Destino: tabla estructurada en Excel.

### CostosLimpios

Importa y normaliza información de costos.

Operaciones principales:

- Preparación de encabezados.
- Conversión de tipos numéricos.
- Limpieza de identificadores.
- Conservación de clasificaciones de producto.
- Revisión de campos vacíos.

Destino: tabla estructurada en Excel.

### VentasPorProducto

Consulta de referencia de VentasLimpias.

Agrupa los registros por Codigo y calcula:

- Suma de CantidadVenta.
- Suma de Venta.

Destino: solo conexión.

### CostosPorProducto

Consulta de referencia de CostosLimpios.

Agrupa los registros por Codigo y calcula:

- Suma de CantidadCosto.
- Suma de Costo.

Destino: solo conexión.

### RevisionPQ

Consulta de referencia de VentasPorProducto.

Combina los resultados agrupados con CostosPorProducto mediante una combinación externa izquierda utilizando Codigo.

Incorpora las columnas de costos y calcula DiferenciaCantidad.

Destino: tabla estructurada en Excel.

### ControlTotalesPQ

Consulta de referencia de RevisionPQ.

Calcula:

- TotalVentas.
- TotalCostos.
- TotalCantidadVentas.
- TotalCantidadCostos.
- DiferenciaVentas.
- DiferenciaCostos.

Destino: solo conexión.

## 5. Modelo de integración

La clave utilizada para integrar los reportes es Codigo.

Las fuentes se agrupan antes de combinarse, para evitar multiplicaciones de registros cuando un mismo producto aparece varias veces.

La integración conserva todos los códigos de ventas mediante una combinación externa izquierda.

La ausencia de coincidencias debe tratarse como una incidencia de calidad de datos y no reemplazarse automáticamente por cero.

## 6. Cálculos de análisis comercial

La hoja Analisis utiliza tres cálculos principales:

**Diferencia bruta**

DiferenciaBruta = Venta - Costo

**Margen estimado**

MargenEstimadoPct = DiferenciaBruta / Venta

Este indicador no está definido cuando Venta es igual a cero. En el libro se utiliza temporalmente una convención de visualización que devuelve cero y estos casos requieren revisión.

**Clasificación comercial**

- NEGATIVO: diferencia bruta menor que cero.
- POSITIVO: diferencia bruta mayor que cero.
- CERO: diferencia bruta igual a cero.

## 7. Controles de calidad

Se implementan controles de:

- Número de registros.
- Tipos de datos.
- Valores nulos y errores.
- Concordancia de códigos.
- Conciliación de cantidades.
- Conciliación monetaria.
- Clasificación de resultados.
- Actualización de consultas.

## 8. Limitaciones

La solución trabaja sobre reportes comerciales de un período específico.

No se han confirmado formalmente los criterios de IVA, reconocimiento contable, devoluciones y comparabilidad temporal.

Por lo tanto, la diferencia bruta y el margen estimado deben interpretarse como indicadores analíticos sujetos a validación, no como utilidad financiera auditada.

## 9. Reproducibilidad

El proceso está organizado mediante consultas de Power Query y referencias estructuradas de Excel.

Las actualizaciones deben ejecutarse desde Datos → Actualizar todo y posteriormente verificarse mediante los controles de calidad.

La versión pública utiliza datos sintéticos de 700 SKU (`DEMO-0001` a `DEMO-0700`). Antes de actualizar, debe configurarse `pRutaDatosDemo` con la ruta local de `data-demo/`, incluyendo la barra invertida final (`\`).

## 10. Seguridad y confidencialidad

Los archivos de origen, las consultas conectadas a rutas privadas y los resultados comerciales reales permanecen fuera del repositorio público.

En el repositorio público se distribuyen únicamente las fuentes sintéticas, el dashboard DEMO y documentación técnica revisada.