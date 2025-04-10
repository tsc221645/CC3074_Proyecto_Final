# Datasets Útiles

## Selección de Datasets y Columnas Relevantes

### Objetivo del Proyecto

El propósito de este análisis es desarrollar un modelo que permita predecir la **dificultad logística de una entrega** realizada a través de la plataforma de e-commerce Olist. Esta dificultad estará definida con base en la **diferencia entre la fecha estimada y la fecha real de entrega**, considerando factores como la **distancia entre el cliente y el vendedor**, y **patrones geográficos e históricos** de entrega.

El resultado esperado es poder **clasificar** las entregas como fáciles, moderadas o difíciles, o bien, **predecir el número de días de retraso**, con el fin de **optimizar la planificación logística, reducir costos** y **mejorar la experiencia del cliente**.

### Criterios de Selección de Datasets

Para lograr este objetivo, se seleccionaron únicamente aquellos datasets que contienen variables **directamente relacionadas** con:

- Fechas reales y estimadas de entrega
- Ubicación geográfica de clientes y vendedores
- Relación entre pedidos, productos y envíos
- Retroalimentación del cliente en caso de retrasos

### Criterios de Selección de Columnas

De cada dataset seleccionado, se extrajeron únicamente las columnas que:

- Permiten hacer uniones (merge) entre tablas: `order_id`, `customer_id`, `seller_id`, `product_id`, etc.
- Contienen **fechas clave** para evaluar tiempos de entrega
- Proveen **coordenadas** o proxies de ubicación (ZIP code, ciudad, estado)
- Permiten construir o validar la **variable objetivo** (por ejemplo, `order_delivered_customer_date` vs `order_estimated_delivery_date`)
- Aportan contexto logístico o validación cualitativa (como las reseñas de los clientes)

Los datasets y columnas no relacionados con **logística, ubicación, tiempos o satisfacción del cliente** fueron descartados por no aportar valor directo al modelo predictivo.

## Datasets Relevantes

### 1. `olist_orders_dataset.csv` - **CLAVE**

- **Por qué**: contiene fechas reales vs estimadas de entrega (variable objetivo).
- **Columnas clave**:
  - `order_purchase_timestamp`
  - `order_delivered_customer_date`
  - `order_estimated_delivery_date`
  - `customer_id`

### 2. `olist_customers_dataset.csv` - **CLAVE**

- **Por qué**: permite conocer la ubicación del cliente.
- **Columnas clave**:
  - `customer_id`
  - `customer_zip_code_prefix`
  - `customer_city`
  - `customer_state`

### 3. `olist_sellers_dataset.csv` - **CLAVE**

- **Por qué**: ubicación del vendedor, necesaria para calcular distancia.
- **Columnas clave**:
  - `seller_id`
  - `seller_zip_code_prefix`
  - `seller_city`
  - `seller_state`

### 4. `olist_order_items_dataset.csv` - **CLAVE**

- **Por qué**: vincula el pedido con el vendedor y fecha límite de envío.
- **Columnas clave**:
  - `order_id`
  - `seller_id`
  - `shipping_limit_date`
  - `freight_value`

### 5. `olist_geolocation_dataset.csv` - **ÚTIL**

- **Por qué**: permite obtener coordenadas geográficas para calcular distancia entre cliente y vendedor.
- **Columnas clave**:
  - `geolocation_zip_code_prefix`
  - `geolocation_lat`
  - `geolocation_lng`

### 6. `olist_order_reviews_dataset.csv` - **ÚTIL**

- **Por qué**: sirve para validar si los retrasos impactan en la percepción del cliente.
- **Columnas clave**:
  - `order_id`
  - `review_score`
  - `review_comment_message`

## Datasets Poca o Nula Utilidad en este problema

### 7. `olist_order_payments_dataset.csv`

- **Por qué**: se enfoca en métodos y montos de pago, no tiene impacto directo en la logística.

### 8. `olist_products_dataset.csv`

- **Por qué**: contiene info del producto, pero no tiene relación directa con tiempos de entrega o localización.

### 9. `product_category_name_translation.csv`

- **Por qué**: solo traduce nombres de categorías, no agrega valor al análisis logístico.

## Conclusión: Datasets necesarios

| Dataset                         | ¿Usar? | Uso principal |
|----------------------------------|--------|----------------|
| `olist_orders_dataset`           | ✅     | Fecha real vs estimada |
| `olist_customers_dataset`        | ✅     | Ubicación del cliente |
| `olist_sellers_dataset`          | ✅     | Ubicación del vendedor |
| `olist_order_items_dataset`      | ✅     | Vínculo con vendedores y shipping |
| `olist_geolocation_dataset`      | ✅     | Calcular distancia |
| `olist_order_reviews_dataset`    | ✅     | Satisfacción por retraso |
| `olist_order_payments_dataset`   | ❌     | No relevante |
| `olist_products_dataset`         | ❌     | No relevante |
| `product_category_name_translation` | ❌  | No relevante |
