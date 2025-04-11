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

Se rellenaron latitudes y longitudes con el promedio por `customer_zip_code_prefix` y `seller_zip_code_prefix`. Esto preserva la coherencia espacial sin introducir valores arbitrarios.

### 2. **Imputación de fechas faltantes**  

- `order_approved_at`: con `order_purchase_timestamp`, ya que la aprobación ocurre después de la compra.
- `order_delivered_customer_date`: con `order_estimated_delivery_date`, si el pedido está marcado como `delivered`, asumiendo que se entregó en la fecha estimada.
- `order_delivered_carrier_date`: con `order_approved_at`, asumiendo que el paquete fue despachado justo después de la aprobación.

### 3. **Imputación de valores numéricos**

Se usaron medias globales para valores como precio, peso, dimensiones, etc., bajo la suposición de que siguen una distribución continua y su media representa una buena estimación.

### 4. **Valores categóricos y texto**

- `payment_type`: se imputó con la moda (más frecuente).
- `payment_installments`: con 1, como valor mínimo válido.
- `product_category_name` y `product_category_name_english`: con "unknown" para mantener la consistencia semántica.
- Campos de texto (`review_comment_title`, `review_comment_message`): con texto vacío `""`.

### 5. **Eliminación de filas incompletas**

Se eliminaron filas que no tienen:

- `seller_id`
- `product_id`
- `order_item_id`
- `order_delivered_customer_date`

Estas columnas son **críticas** para análisis como logística, ventas, entrega y satisfacción. Si no hay `seller_id` o `product_id`, no se puede trazar el pedido a un vendedor o producto. Si no hay `order_delivered_customer_date`, no se puede analizar el cumplimiento de entrega.

**Resultado:** Se eliminaron 3,413 filas, equivalente al **2.86 %** del dataset original (de 119,143 a 115,730 filas).

**Nota sobre `order_status`:**  
Como consecuencia de esta limpieza, el conjunto resultante conserva únicamente órdenes en estado `delivered` y `canceled`. Las órdenes en estados como `shipped`, `processing`, `created`, `approved`, `invoiced` y `unavailable` fueron eliminadas porque no cuentan con información completa para análisis logístico (por ejemplo, fechas de entrega o identificadores de producto/vendedor).

**Estados no finales eliminados:**

- `created` → creado
- `approved` → aprobado
- `invoiced` → facturado
- `processing` → procesando
- `shipped` → enviado

**Estados finales conservados:**

- `delivered` → entregado
- `canceled` → cancelado

Esto garantiza que el análisis logístico posterior se base únicamente en pedidos que completaron su ciclo o que fueron cancelados.

**Resultado:** Se eliminaron 3,413 filas, equivalente al **2.86 %** del dataset original (de 119,143 a 115,730 filas).

### 6. **Normalización de reseñas**

Fechas de reseñas (`review_creation_date`, `review_answer_timestamp`) fueron imputadas con `order_delivered_customer_date` si estaban ausentes, asegurando coherencia temporal.

### 7. **Normalización de `payment_sequential`**

Se imputó con 1 en caso de ausencia, representando el primer intento de pago.

### 8. **Limpieza para `geolocation`**

El archivo `olist_geolocation_dataset.csv` contiene datos geográficos por prefijo de código postal (`geolocation_zip_code_prefix`), incluyendo coordenadas (`lat`, `lng`), ciudad y estado. Aunque no contiene valores nulos, **presenta un número significativo de filas duplicadas**.

#### ¿Por qué hay duplicados?

Cada fila representa una geolocalización vinculada a un ZIP code. Los duplicados se deben a que múltiples clientes o pedidos pueden compartir el mismo ZIP, y por tanto, la misma latitud y longitud se registra varias veces.

#### ¿Representan información adicional?

No. Estos duplicados **no contienen información nueva**, ya que todos los campos (ZIP, coordenadas, ciudad y estado) son exactamente iguales. Su presencia **no aporta valor analítico** adicional y **no representa eventos distintos**, solo repeticiones.

#### ¿Afecta eliminarlos?

No. Para el análisis geoespacial y logístico que se desea realizar, lo que se necesita es una **asociación única entre cada ZIP y sus coordenadas**. Por tanto, eliminar duplicados:

- **No elimina información relevante**.
- **No afecta el cálculo de distancias ni la identificación de zonas**.
- **Reduce el tamaño del dataset y mejora la eficiencia de procesamiento**.

#### Resultado de la limpieza

- Se eliminaron **261,831 filas duplicadas**, lo cual representa un **26.18 %** del dataset original de `1,000,163` filas.
- El dataset limpio contiene **738,332 filas únicas**, sin nulos ni duplicados.

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
- `data_quality_report.txt`: Reporte previo a limpieza
- `data_quality_report_imputed.txt`: Reporte posterior a limpieza con imputaciones
