# Proyecto de Optimización de Precios - Minería de Datos 📦

## 📅 **Equipo de Trabajo**

* Ana Laura Tschen Moscoso
* Cindy Gualim
* Luis Pedro Montenegro
* Josúe Say

## 🖥️ **Ambiente de Desarrollo**

* **Lenguaje:** Python 3.13.2
* **Notebooks:** Jupyter Notebook
* **Gestión de Entorno:** `.venv` (Entorno Virtual de Python)

## 🎯 **Objetivo del Proyecto**

Desarrollar un modelo predictivo capaz de **estimar precios óptimos de productos** en la plataforma Olist, considerando variables como características físicas de los productos, ubicación de clientes y vendedores, métodos de pago, comportamientos de compra y satisfacción del cliente.

La variable objetivo del proyecto fue **`price`**, enfocándose en entender y modelar los factores que afectan directamente su comportamiento para optimizar la estrategia de precios.

## 📚 **Estructura del Proyecto**

```bash
├── .venv/                  # Entorno virtual de Python
├── data/                   # Datasets originales descargados
├── data_clean/             # Datasets limpios y procesados
├── dataset_reports_raw/    # Reportes de calidad de datos (sin limpiar)
├── docs/                   # Documentación adicional
├── images/                 # Imágenes y gráficas generadas
├── scripts/                # Scripts utilitarios (descarga y limpieza)
├── semana1/                # Fase de exploración y limpieza
├── semana2/                # Experimentos adicionales
├── semana3/                # Modelos de regresión y clasificación iniciales
├── semana4/                # Implementación de modelos optimizados (Random Forest, XGBoost, MLP)
└── semana5/                # Reportes finales y documentación
```

## ⚙️ **Pasos para Reproducir el Proyecto**

### 1️⃣ **Instalación de Dependencias**

```bash
pip install -r requirements.txt
```

### 2️⃣ **Descarga del Dataset**

```bash
python .\scripts\download_dataset.py
```

Esto descargará los archivos CSV originales de la plataforma Olist en la carpeta `data/`.

### 3️⃣ **Ejecutar Fase de Limpieza y Preparación de Datos**

Abre y ejecuta el siguiente notebook para preparar y limpiar los datos:
📁 `semana1/fase1.ipynb`

### 4️⃣ **Ejecutar Modelos y Análisis Final**

Ejecuta el notebook de la fase final para aplicar los modelos de regresión y clasificación optimizados:
📁 `semana4/refactor_approach.ipynb`

## 📊 **Resultados Destacados**

* **Mejor Modelo de Regresión:** *Random Forest*

  * $R^2$ = 0.92
  * MAE = 15.22
  * Variables más importantes: `payment_value`, `freight_value`, `product_weight_g`.

* **Mejor Modelo de Clasificación:** *Random Forest Classifier*

  * Accuracy = 67% (Validación Cruzada)
  * AUC ROC = 0.85
