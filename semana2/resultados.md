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

## Random Forest

### Interpretación de los Resultados

- **$R^2$ Score: 0.25**
  - El modelo explica aproximadamente **el 25% de la variabilidad** en los días de retraso (`delivery_diff_days`).
  - **Conclusión**: **Mejora respecto al modelo lineal** y XGBoost, pero aún **lejos de ser un modelo óptimo** para predicción precisa.

- **MSE (Error cuadrático medio): 78.89**
  - En promedio, el **error de predicción** es **de aproximadamente 8.8 días** (ya que $\sqrt{78.89} \approx 8.88$).
  - **Conclusión**: Se reduce el error comparado con la regresión lineal (antes era ~9.8 días), mostrando un mejor ajuste.

### Justificación basada en las gráficas

1. **Dispersión Reales vs Predichos**
   - Hay **mayor concentración** de puntos cercanos a la línea roja ideal respecto a la regresión lineal.
   - Sin embargo, sigue existiendo **dispersión considerable**, indicando **que aún falta capturar mejor la variabilidad**.

2. **Histograma de Residuos**
   - **Distribución más concentrada alrededor del 0** que en los modelos anteriores.
   - Persisten **colas largas** (outliers), lo que sigue afectando la calidad global del modelo.

3. **Errores vs Predicción**
   - **Mejor distribución de residuos** comparado con regresión lineal.
   - Aun así, se observa **heterocedasticidad**: los errores no son completamente uniformes a lo largo de los valores predichos.

### Conclusión Global del Random Forest

- **Random Forest maneja mejor la no linealidad** de los datos comparado con la regresión lineal simple y XGBoost.
- Aún **no explica la mayoría de la variabilidad**, probablemente debido a:
  - Variables independientes limitadas.
  - Ruido inherente en los datos de retraso logístico.
  - Posibles variables relevantes no incluidas (clima, tráfico, estado del vendedor, etc.).
