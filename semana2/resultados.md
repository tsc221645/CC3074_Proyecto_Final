# Regresiones

## 1. Regresión Lineal

### Interpretación de Resultados

- **$R^2$ Score: 0.08**
  - Solo **el 8%** de la variabilidad de los días de retraso (`delivery_diff_days`) es explicada.
  - **Conclusión**: **Muy mala capacidad predictiva**; la relación no es lineal.

- **MSE: 96.76**
  - El **error promedio** es de casi **10 días** ($\sqrt{96} \approx 9.8$ días).
  - **Conclusión**: El error es **muy grande** para ser útil en predicción real.

### Análisis Gráfico

- **Dispersión real vs predicho**: Alta dispersión y ruido.  
- **Histograma de residuos**: Centrado en 0 pero colas largas.
- **Errores vs predicciones**: Heterocedasticidad (errores dispersos a medida que cambia el valor predicho).

## 2. XGBoost

### Interpretación de Resultados

- **$R^2$ Score: 0.165**
  - Explica **solo 16.5%** de la variabilidad.  
- **MSE: 87.89**
  - Error medio aún **alto** (~9.4 días).

### Análisis Gráfico

- **Mejor que la regresión lineal**, pero **la dispersión de errores persiste**.
- **Menos outliers extremos**, pero **no logra capturar la dinámica real** de los retrasos.

## 3. Random Forest

### Interpretación de Resultados

- **$R^2$ Score: 0.25**
  - **Mejor** de los modelos probados, pero **apenas explica el 25%** de la variabilidad.
- **MSE: 78.89**
  - Error promedio reducido (~8.8 días), **pero todavía considerable**.

### Análisis Gráfico

- **Residuos más concentrados** cerca de cero.
- **Persisten colas y dispersión**.
- Mejor manejo de no-linealidades, **pero sigue faltando información clave**.

## **Conclusión Global**

- Los **modelos de regresión no son suficientes** para capturar el fenómeno del retraso en entregas.
- Aunque Random Forest mejora los resultados, **todos los modelos tienen bajo desempeño** (R² muy bajo y MSE relativamente alto).
- **Las variables actuales no explican adecuadamente el fenómeno**: falta información externa relevante (clima, tráfico, días feriados, condiciones del vendedor, problemas logísticos, etc.).

## **¿Por qué no tenemos buen resultado?**

- **Alta varianza natural** en los retrasos de entrega (mucho ruido).
- **Faltan variables explicativas importantes**.
- **Relación compleja** y posiblemente **no continua** entre las variables y el retraso.
- **Outliers** y datos extremos mal controlados.
- **Problema posiblemente mal formulado como regresión**.

## **¿Qué otro enfoque podríamos intentar?**

### 1. **Clasificación en lugar de regresión**

En vez de predecir "cuántos días de retraso", podrías **predecir categorías**:

- Ejemplo de etiquetas:
  - Entrega a tiempo (retraso <= 0 días)
  - Entrega ligeramente retrasada (1-5 días)
  - Entrega muy retrasada (>5 días)
  
Modelo sugerido: **RandomForestClassifier, XGBoostClassifier** o **LightGBMClassifier**.

Ventajas:

- La clasificación **tolera mejor la variabilidad y el ruido**.
- Puedes interpretar el problema como **riesgo de retraso** en lugar de cantidad exacta de días.

### 2. **Regresión robusta**

Si quieres seguir con regresión:

- Usar **modelos robustos** a outliers como **HuberRegressor** o **RANSACRegressor** de `sklearn`.
- Estos son menos sensibles a valores extremos y dispersión.

### 3. **Enriquecer el dataset**

- Incorporar **variables externas** como:
  - Estado del tiempo.
  - Congestión en la región de entrega.
  - Capacidad logística del vendedor.
  - Temporada del año (Navidad, Black Friday).

Esto **podría mejorar sustancialmente** los resultados sin cambiar el tipo de modelo.

## Resumen Final

| Alternativa | Justificación |
|:------------|:---------------|
| Clasificación | Permite tolerar el ruido y manejar el retraso como categorías. |
| Regresión robusta | Mejora tolerancia a outliers y heterocedasticidad. |
| Enriquecimiento de variables | Fundamental para aumentar la capacidad predictiva de cualquier modelo. |
