# Proceso de Unificación y Limpieza del Dataset de Olist

## Objetivo

El propósito de este proceso es consolidar y limpiar todos los archivos del dataset público de Olist para:

- Unificar toda la información relacional en un solo DataFrame
- Detectar y corregir valores faltantes (nulos)
- Eliminar filas inválidas o incompletas
- Generar un dataset limpio y usable para análisis predictivo y visualizaciones
- Restaurar los archivos originales desde el dataset unificado limpio

## Unificación de Datasets

Se parte de múltiples archivos CSV con información sobre pedidos, clientes, productos, pagos, reseñas y ubicación. Estos se unifican paso a paso con `merge`, conservando las relaciones por claves (`order_id`, `customer_id`, `product_id`, etc.).

```python
# Ejemplo: unión de pedidos con clientes
df = orders.merge(customers, on='customer_id', how='left')
```

Además, se realiza una agregación sobre geolocalización por ZIP Code para obtener las coordenadas promedio de clientes y vendedores.

## Revisión de Calidad de Datos

Se genera un reporte de calidad con:

- Cantidad total de valores nulos por columna
- Número de filas con al menos un nulo
- Número total de duplicados
- Vista previa del DataFrame
- Nombres de columnas

Esto ayuda a decidir qué valores se imputan y cuáles se eliminan.

## Limpieza de Datos

Se aplicaron las siguientes estrategias:

### 1. **Imputación de coordenadas**  

Por promedio de latitud y longitud agrupado por `customer_zip_code_prefix` y `seller_zip_code_prefix`.

### 2. **Imputación de fechas faltantes**  

- `order_approved_at` con `order_purchase_timestamp`
- `order_delivered_customer_date` con `order_estimated_delivery_date` (si `status` es `delivered`)
- `order_delivered_carrier_date` con `order_approved_at`

### 3. **Imputación de valores numéricos**

Se utilizaron medias globales para columnas como precio, peso y dimensiones del producto.

### 4. **Valores categóricos y texto**

- Categorías vacías se llenaron con `"unknown"`
- Texto de reseñas (`review_comment_title`, `review_comment_message`) con `""` vacío

### 5. **Eliminación de filas incompletas**

Filas sin `seller_id`, `product_id`, `order_item_id`, o sin `order_delivered_customer_date` fueron eliminadas por ser críticas para el análisis.

### 6. **Columnas auxiliares eliminadas**

Las columnas `geolocation_zip_code_prefix_x` y `geolocation_zip_code_prefix_y` fueron usadas para imputar coordenadas, y luego eliminadas por ser técnicas.

### 7. **Normalización de reseñas**

Fechas de reseña (`review_creation_date`, `review_answer_timestamp`) fueron imputadas usando `order_delivered_customer_date` si no existían.

## Generación del Dataset Limpio

Se guardó el resultado final en:

```bash
/data_clean/olist_dataset_unificado_clean.csv
```

## Restauración de Datasets Originales desde el Dataset Limpio

Usando un diccionario llamado `columnas_originales`, se reconstruyeron los datasets originales (`orders`, `customers`, etc.) extrayendo solo las columnas correspondientes y eliminando duplicados.

```python
df_filtrado = df[columnas_presentes].drop_duplicates()
df_filtrado.to_csv(f"olist_{nombre}_dataset_clean.csv")
```

Cada uno se guarda en la carpeta `../data_clean/`.

## Archivos Generados

- `olist_dataset_unificado_clean.csv`: Dataset completo limpio
- `olist_orders_dataset_clean.csv`
- `olist_customers_dataset_clean.csv`
- `olist_order_items_dataset_clean.csv`
- `olist_sellers_dataset_clean.csv`
- `olist_geolocation_dataset_clean.csv`
- `olist_order_reviews_dataset_clean.csv`
- `olist_order_payments_dataset_clean.csv`
- `olist_products_dataset_clean.csv`
- `product_category_name_translation_clean.csv`
- `data_quality_report.txt`: Reporte previo
- `data_quality_report_imputed.txt`: Reporte después de limpiar
