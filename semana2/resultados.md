# Regresiones

## Lineal

### Interpretación de los Resultados

- **$R^2$ Score: 0.08**
  - Solo **el 8% de la variabilidad** en los días de retraso (`delivery_diff_days`) es explicada por nuestro modelo lineal.
  - **Conclusión**: El modelo **no capta bien la relación** entre las variables independientes y la variable objetivo.

- **MSE (Error cuadrático medio): 96.76**
  - En promedio, el **error de predicción** de los días de retraso es **de casi 10 días** (ya que $\sqrt{96} \approx 9.8$).
  - **Conclusión**: Los errores son considerables para el problema que buscamos resolver.

### Justificación basada en las gráficas

1. **Dispersión Reales vs Predichos**
   - Los puntos **no siguen la línea roja ideal** (donde predicción = realidad).
   - **Mucho ruido y dispersión**, lo que indica que **la relación no es lineal**.

2. **Histograma de Residuos**
   - Aunque los errores están **centrados en 0** (lo cual es bueno), hay una **larga cola** (outliers).
   - **Conclusión**: Hay **errores grandes** en algunas predicciones, afectando la calidad del modelo.

3. **Errores vs Predicciones**
   - No hay **patrón sistemático claro** (lo cual es bueno), pero **se nota una varianza heterogénea**: conforme cambia el valor predicho, **los errores se dispersan más**.
   - **Conclusión**: **Violamos el supuesto de homocedasticidad** requerido para una regresión lineal.

## XGBoost

### Interpretación de los Resultados

- **$R^2$ Score: 0.165**
  - XGBoost explica **el 16.5% de la variabilidad** en `delivery_diff_days`.
  - **Conclusión**: Aunque el resultado es **mejor que la regresión lineal**, **sigue siendo bajo** para un modelo útil en producción.

- **MSE: 87.89**
  - El **error cuadrático medio** disminuyó ligeramente respecto a la regresión lineal.
  - **Conclusión**: XGBoost **mejora el ajuste**, pero no resuelve el problema de raíz.

### Justificación basada en las gráficas

1. **Dispersión Reales vs Predichos**
   - Aún se observa **dispersión** importante, aunque los predichos **se agrupan ligeramente mejor** cerca de la línea ideal.

2. **Histograma de Residuos**
   - Distribución **más centrada** y **con menos dispersión extrema** comparado con la regresión lineal.
   - **Conclusión**: **XGBoost maneja mejor los errores grandes**, pero no elimina por completo los outliers.

3. **Errores vs Predicción**
   - La dispersión de errores **sigue existiendo** pero es **un poco más controlada** en comparación a la regresión lineal.

### Resumen final

- **Problema principal**: El retraso de entregas **no sigue una relación lineal** simple con las variables elegidas.
- **Siguiente paso**: Probar modelos **no lineales** como:
  - **Random Forest Regressor**
  - **SVR (Support Vector Regression)**
  - **Árboles de decisión**
  - **Regresión polinomial**

Estos modelos pueden capturar relaciones **más complejas** o **no lineales** que la regresión lineal no puede.
