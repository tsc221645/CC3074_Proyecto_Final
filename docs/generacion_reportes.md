# Módulo de Generación de Reportes de Análisis Exploratorio de Datos (EDA)

## Objetivo

Este módulo tiene como propósito automatizar el análisis exploratorio de múltiples archivos CSV y generar reportes detallados en formato `.txt`, facilitando así la comprensión de cada dataset de manera individual.

Esto es útil como paso previo a la limpieza o modelado, ya que permite:

- Identificar valores nulos
- Revisar tipos de datos
- Observar estadísticas descriptivas
- Detectar valores frecuentes y cardinalidad

## Funcionalidades del Módulo

A continuación se describen las funciones incluidas:

### `get_null_count(df)`

Devuelve el número total y el porcentaje de valores nulos por columna.

```python
nulls = df.isnull().sum()
percent = (nulls / len(df)) * 100
```

### `get_types(df)`

Devuelve el tipo de dato (`int`, `float`, `object`, etc.) de cada columna.

### `get_row_and_col_count(df)`

Devuelve el número total de filas y columnas del DataFrame.

### `get_descriptive_stats(df)`

Devuelve estadísticas básicas de columnas numéricas: media, desviación estándar, mínimo, máximo, etc.

### `get_unique(df)`

Muestra cuántos valores únicos hay por columna.

### `get_top_frequent(df, top_n=3)`

Muestra los valores más frecuentes (por defecto, top 3) de cada columna categórica (`object`).

## 📝 `write_report()`

Genera un archivo `.txt` con los resultados de las funciones anteriores, separadas por secciones con encabezados legibles.

### Parámetros

- `df`: DataFrame a analizar
- `df_name`: nombre del DataFrame (usado como título en el reporte)
- `path`: ruta donde guardar el `.txt`
- `summary_functions`: lista de funciones de análisis a ejecutar

## `generate_report_folder()`

Función automatizada para aplicar el análisis a **todos los archivos `.csv` en una carpeta**.

### Parámetros

- `csv_folder`: carpeta que contiene los `.csv`
- `report_folder`: carpeta destino de los reportes `.txt`
- `summary_functions`: lista de funciones que se aplicarán a cada archivo

## Ejemplo de uso

```python
from analysis_report_module import *  # suponiendo que guardaste el módulo

generate_report_folder(
    csv_folder="../data_clean",
    report_folder="../dataset_reports_clean",
    summary_functions=[
        get_null_count,
        get_types,
        get_row_and_col_count,
        get_descriptive_stats,
        get_top_frequent,
        get_unique
    ]
)
```

Esto generará un archivo `.txt` por cada CSV en la carpeta `../data_clean`, con nombre tipo:

- `olist_orders_dataset_clean_raw_report.txt`
- `olist_customers_dataset_clean_raw_report.txt`
- ...
