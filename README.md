# Limpieza y validación de ventas retail con Apache Spark en Databricks

Flujo completo de ingeniería de datos en un entorno distribuido: integración de las ventas de tres sucursales de una cadena de retail, depuración de sus problemas de calidad, validación del resultado con reglas e indicadores objetivos y análisis del conjunto limpio con Spark SQL. Cada operación que altera el número de registros queda registrada en una bitácora de trazabilidad, de modo que toda diferencia entre el volumen inicial y el final está cuantificada y justificada.

**Autor:** Jorge Álvarez Ossandón

---

## Problema

Las ventas del cuarto trimestre de 2024 de las sucursales de Viña del Mar, Santiago y Concepción llegan en tres archivos CSV independientes con registros duplicados, categorías faltantes, fechas en cuatro formatos, precios con símbolos monetarios, nombres de productos inconsistentes y cantidades no válidas. ¿Cómo obtener un conjunto confiable para la toma de decisiones y cuánto cambian las conclusiones del negocio cuando se controla la calidad de los datos?

## Hallazgos principales

| Tema | Resultado |
|---|---|
| Trazabilidad | 105 + 125 + 85 = 315 registros cargados → 15 duplicados → 26 cantidades no válidas → 274 ventas válidas (87,0 % del total). Cada descarte tiene una causa documentada y verificada con `assert`. |
| Calidad antes y después | Fechas no estandarizadas: 226 → 0. Precios no numéricos: 203 → 0. Nulos en `categoria`: 46 → 0. Variantes de nombre de producto: 31 → 10. |
| Valores atípicos | 7 registros (2,6 %) concentran el 60,3 % de los montos. Sin marcarlos, 7 de las 10 mayores ventas del trimestre serían datos erróneos. |
| Ranking de sucursales | Con todos los registros, Concepción aparece segunda (24,4 %); excluidos los marcados, es la de menor desempeño (22,1 %) y Viña del Mar sube al segundo lugar (35,1 %). Santiago lidera en ambos escenarios por volumen de transacciones. |
| Datos de origen | Cada producto aparece en 3 a 5 categorías y en 8 a 10 precios distintos: la limpieza técnica no puede corregir un dato incorrecto en origen. Se recomienda un maestro de productos contra el cual validar las transacciones. |

## Competencias y métodos

| Área | Métodos y herramientas |
|---|---|
| Ingesta e integración | Lectura de CSV como texto para no perder valores mal formateados, `unionByName`, control de esquemas y conteos. |
| Limpieza | Tratamiento justificado de nulos, eliminación de duplicados, normalización de texto (minúsculas, tildes, diccionario de sinónimos), unificación de fechas por patrón (`when` + `rlike`, compatible con modo ANSI), conversión de precios con expresiones regulares. |
| Transformación | Columnas derivadas (`precio_total`, `dia_semana`), filtrado de registros no válidos, valores distintos por dimensión. |
| Validación y calidad | Reglas de negocio por fila, detección de atípicos por IQR (Tukey) marcados y no eliminados, regla complementaria para precios anormalmente bajos, informe de calidad antes y después (volumen, unicidad, completitud, validez, consistencia). |
| Análisis | Vista temporal y consultas Spark SQL, cada una contrastada con y sin registros marcados. |
| Plataforma | Databricks Free Edition, *volumes* de Unity Catalog, PySpark, Git y GitHub. |

## Contenido del notebook

| Sección | Contenido |
|---|---|
| 0 | Configuración del entorno y bitácora de trazabilidad |
| 1 | Carga e integración de los tres archivos |
| 2 | Limpieza y depuración |
| 3 | Transformación y análisis con DataFrames |
| 4 | Validación y control de calidad |
| 5 | Análisis con Spark SQL |
| 6 | Conclusiones, decisiones técnicas y recomendaciones |

## Estructura del repositorio

```
data/        ventas_vina_del_mar.csv, ventas_santiago.csv, ventas_concepcion.csv
notebooks/   limpieza_ventas_retail.ipynb (exportado desde Databricks con sus salidas)
```

## Cómo reproducir

1. Crear una cuenta en Databricks Free Edition.
2. Crear un *volume* en Unity Catalog (por ejemplo `/Volumes/workspace/default/ventas`) y subir los tres archivos de `data/`.
3. Importar `notebooks/limpieza_ventas_retail.ipynb` en el workspace, ajustar la ruta del *volume* en la primera celda de código si es distinta y ejecutar todas las celdas.