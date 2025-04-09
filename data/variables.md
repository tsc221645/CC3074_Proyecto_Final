# Estructura de los Datasets de Olist

Fuente original del dataset:  

- [Kaggle - Brazilian E-commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
- [Olist Website](https://www.olist.com/)

## 1. `olist_customers_dataset`

Contiene información de los clientes.

| Campo                   | Descripción                   |
|------------------------|-------------------------------|
| customer_id            | ID único del cliente (anónimo) |
| customer_unique_id     | ID único por cliente real     |
| customer_zip_code_prefix | Prefijo del código postal    |
| customer_city          | Ciudad del cliente            |
| customer_state         | Estado del cliente            |

**Ejemplo:**

```csv
"customer_id","customer_unique_id","customer_zip_code_prefix","customer_city","customer_state"
"06b8999e2fba1a1fbc88172c00ba8bc7","861eff4711a542e4b93843c6dd7febb0","14409",franca,SP
```

## 2. `olist_geolocation_dataset`

Datos geográficos por prefijo de código postal.

| Campo                 | Descripción                  |
|----------------------|------------------------------|
| geolocation_zip_code_prefix | Prefijo de código postal |
| geolocation_lat       | Latitud                      |
| geolocation_lng       | Longitud                     |
| geolocation_city      | Ciudad                       |
| geolocation_state     | Estado                       |

**Ejemplo:**

```csv
"geolocation_zip_code_prefix","geolocation_lat","geolocation_lng","geolocation_city","geolocation_state"
"01037",-23.54562128115268,-46.63929204800168,sao paulo,SP
```

## 3. `olist_order_items_dataset`

Contiene los artículos asociados a cada pedido.

| Campo                | Descripción                         |
|---------------------|-------------------------------------|
| order_id            | ID del pedido                       |
| order_item_id       | Número de ítem en el pedido         |
| product_id          | ID del producto                     |
| seller_id           | ID del vendedor                     |
| shipping_limit_date | Fecha límite de envío               |
| price               | Precio del producto                 |
| freight_value       | Costo de envío                      |

**Ejemplo:**

```csv
"order_id","order_item_id","product_id","seller_id","shipping_limit_date","price","freight_value"
"00010242fe8c5a6d1ba2dd792cb16214",1,"4244733e06e7ecb4970a6e2683c13e61","48436dade18ac8b2bce089ec2a041202",2017-09-19 09:45:35,58.90,13.29
```

## 4. `olist_order_payments_dataset`

Información sobre los pagos realizados.

| Campo                | Descripción              |
|---------------------|--------------------------|
| order_id            | ID del pedido            |
| payment_sequential  | Secuencia del pago       |
| payment_type        | Tipo de pago             |
| payment_installments | Cuotas del pago         |
| payment_value       | Monto pagado             |

**Ejemplo:**

```csv
"order_id","payment_sequential","payment_type","payment_installments","payment_value"
b81ef226f3fe1789b1e8b2acac839d17,1,credit_card,8,99.33
```

## 5. `olist_orders_reviews_dataset`

Contiene las reseñas y puntuaciones de los pedidos.

| Campo                   | Descripción                  |
|------------------------|------------------------------|
| review_id              | ID de la reseña              |
| order_id               | ID del pedido asociado       |
| review_score           | Puntuación (1 a 5)           |
| review_comment_title   | Título del comentario        |
| review_comment_message | Mensaje de la reseña         |
| review_creation_date   | Fecha de creación            |
| review_answer_timestamp| Fecha de respuesta           |

**Ejemplo:**

```csv
"review_id","order_id","review_score","review_comment_title","review_comment_message","review_creation_date","review_answer_timestamp"
"7bc2406110b926393aa56f80a40eba40","73fc7af87114b39712e6da79b0a377eb",4,,,2018-01-18 00:00:00,2018-01-18 21:46:59
```

## 6. `olist_orders_dataset`

Información general de los pedidos.

| Campo                       | Descripción                            |
|----------------------------|----------------------------------------|
| order_id                   | ID del pedido                          |
| customer_id                | ID del cliente                         |
| order_status               | Estado del pedido                      |
| order_purchase_timestamp   | Fecha de compra                        |
| order_approved_at          | Fecha de aprobación                   |
| order_delivered_carrier_date | Fecha de envío al transportista     |
| order_delivered_customer_date | Fecha de entrega al cliente       |
| order_estimated_delivery_date | Fecha estimada de entrega          |

**Ejemplo:**

```csv
"order_id","customer_id","order_status","order_purchase_timestamp","order_approved_at","order_delivered_carrier_date","order_delivered_customer_date","order_estimated_delivery_date"
e481f51cbdc54678b7cc49136f2d6af7,"9ef432eb6251297304e76186b10a928d",delivered,2017-10-02 10:56:33,2017-10-02 11:07:15,2017-10-04 19:55:00,2017-10-10 21:25:13,2017-10-18 00:00:00
```

## 7. `olist_sellers_dataset`

Información sobre los vendedores.

| Campo                 | Descripción           |
|----------------------|-----------------------|
| seller_id            | ID del vendedor       |
| seller_zip_code_prefix | Código postal       |
| seller_city          | Ciudad del vendedor   |
| seller_state         | Estado del vendedor   |

**Ejemplo:**

```csv
"seller_id","seller_zip_code_prefix","seller_city","seller_state"
"3442f8959a84dea7ee197c632cb2df15","13023",campinas,SP
```

## 8. `product_category_name_translation`

Traducción de nombres de categorías de productos.

| Campo                    | Descripción                       |
|-------------------------|-----------------------------------|
| product_category_name    | Nombre original (portugués)       |
| product_category_name_english | Nombre traducido al inglés  |

**Ejemplo:**

```csv
product_category_name,product_category_name_english
beleza_saude,health_beauty
```

## 9. `olist_products_dataset`

Detalles de los productos disponibles en la tienda.

| Campo                    | Descripción                              |
|-------------------------|------------------------------------------|
| product_id              | ID del producto                          |
| product_category_name   | Categoría del producto                   |
| product_name_lenght     | Longitud del nombre del producto         |
| product_description_lenght | Longitud de la descripción            |
| product_photos_qty      | Cantidad de fotos del producto           |
| product_weight_g        | Peso del producto en gramos              |
| product_length_cm       | Longitud del empaque (cm)                |
| product_height_cm       | Altura del empaque (cm)                  |
| product_width_cm        | Ancho del empaque (cm)                   |

**Ejemplo:**

```csv
"product_id","product_category_name","product_name_lenght","product_description_lenght","product_photos_qty","product_weight_g","product_length_cm","product_height_cm","product_width_cm"
"1e9e8ef04dbcff4541ed26657ea517e5",perfumaria,40,287,1,225,16,10,14
```

## Puntos Importantes

- **Un pedido puede tener varios artículos.**
- **Cada artículo puede ser enviado por un vendedor distinto.**
- **Los nombres de tiendas y socios han sido anonimizados usando nombres de casas de *Game of Thrones*.**
