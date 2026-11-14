# MCDI502 · Sumativa 4 (Taller III): Preprocesamiento y calidad de datos con Apache Spark en Databricks

**Universidad Andrés Bello** · **Curso:** MCDI502 Gestión de Datos y Tecnologías Big Data · **Grupo 6**

**Integrantes:** Fernanda Ovalle Román, Sebastián Cajales Cid, César Lorca Bacián, Jorge Álvarez Ossandón.

## Caso de estudio

Consolidación y análisis de las ventas del cuarto trimestre de 2024 de una cadena de retail con tres sucursales (Viña del Mar, Santiago y Concepción). Los datos de cada sucursal provienen de archivos CSV independientes con problemas de calidad: registros duplicados, categorías faltantes, fechas en múltiples formatos, precios con símbolos monetarios, nombres de productos inconsistentes y cantidades no válidas.

## Contenido del repositorio

| Ruta | Descripción |
|---|---|
| `data/` | Archivos fuente: `ventas_vina_del_mar.csv`, `ventas_santiago.csv`, `ventas_concepcion.csv` |
| `notebooks/mcdi502_s4_grupo6.ipynb` | Notebook de Databricks exportado con sus salidas |

## Estructura del notebook

1. Carga e integración de datos (ID3.1)
2. Limpieza y depuración (ID4.2)
3. Transformación y análisis con DataFrames (ID4.1)
4. Validación y control de calidad (ID4.3)
5. Análisis con Spark SQL (ID3.3)
6. Conclusiones

## Ejecución

El notebook se ejecuta íntegramente en Databricks. Los tres archivos de `data/` se cargan al almacenamiento del workspace y la ruta se define en la primera celda de código.
