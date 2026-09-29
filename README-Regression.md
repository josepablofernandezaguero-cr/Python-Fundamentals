# Regresión: emisiones de CO₂ de vehículos

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU-USUARIO/regresion-emisiones-co2/blob/main/notebooks/ML_Regression.ipynb)

Modelos de regresión lineal múltiple, polinomial y no lineal con **scikit-learn** para estimar las emisiones de CO₂ de vehículos a partir de sus características, más un ejercicio de ajuste de curvas no lineales sobre el PIB de China.

## Objetivo

Comparar distintas formas de regresión, evaluar qué tan bien explican la variable respuesta en datos que el modelo no vio y entender cuándo conviene una relación lineal, polinomial o no lineal.

## Datos

| Dataset | Descripción | Uso |
|---|---|---|
| `FuelConsumptionCo2.csv` | Consumo de combustible y emisiones estimadas de CO₂ de vehículos ligeros nuevos vendidos en Canadá | Regresión lineal múltiple y polinomial |
| `china_gdp.csv` | PIB de China por año | Regresión no lineal (curva sigmoide) |

Los archivos se descargan desde el propio notebook (ver [Cómo reproducirlo](#cómo-reproducirlo)).

## Metodología

1. **Exploración:** gráfico de dispersión de `ENGINESIZE` contra `CO2EMISSIONS` para inspeccionar la tendencia.
2. **Partición:** 80 % entrenamiento / 20 % prueba.
3. **Regresión lineal múltiple:** `CO2EMISSIONS` explicada por `ENGINESIZE`, `CYLINDERS`, `FUELCONSUMPTION_CITY` y `FUELCONSUMPTION_HWY`.
4. **Regresión polinomial** de grado 2 y 3 usando solo `ENGINESIZE`.
5. **Formas de regresión** sobre datos sintéticos con ruido: lineal, cúbica, cuadrática, exponencial, logarítmica y sigmoide.
6. **Regresión no lineal:** ajuste de una curva sigmoide al PIB de China.

## Resultados (conjunto de prueba)

| Modelo | Variables | MAE | MSE | R² |
|---|---|---|---|---|
| Lineal múltiple | 4 predictores | — | 629.60 | 0.85 |
| Polinomial grado 2 | `ENGINESIZE` | 24.92 | 1106.48 | 0.74 |
| Polinomial grado 3 | `ENGINESIZE` | 25.00 | 1105.29 | 0.74 |
| Sigmoide (PIB de China, datos normalizados) | año | 0.03 | 0.00 | 0.98 |

> Las métricas provienen de una partición aleatoria; pueden variar ligeramente entre ejecuciones.

## Hallazgos

- El modelo lineal múltiple, con cuatro predictores, explica cerca del 85 % de la variabilidad de las emisiones en prueba, frente al 74 % del modelo que usa solo el tamaño del motor.
- Pasar de grado 2 a grado 3 no mejora el ajuste (R² igual y MSE prácticamente idéntico), por lo que el grado 2 es suficiente por parsimonia.
- Para el PIB de China, la curva sigmoide ajusta muy bien la serie (R² de 0.98).

## Próximos pasos

- Fijar una semilla aleatoria para que los resultados sean reproducibles.
- Revisar multicolinealidad entre los predictores (por ejemplo, con VIF), en especial entre consumo en ciudad y en carretera.
- Verificar los supuestos del modelo lineal: normalidad, homocedasticidad e independencia de los residuos.
- Comparar con validación cruzada y con modelos regularizados (Ridge, Lasso).

## Cómo reproducirlo

La forma más simple es abrirlo en Google Colab con el botón de arriba.

Para correrlo localmente:

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook notebooks/ML_Regression.ipynb
```

El notebook descarga los datos con `wget`. Si trabajas en Windows, reemplaza esa celda por `pd.read_csv(url)` con la misma dirección.

## Créditos

Basado en los laboratorios del curso *Machine Learning with Python* de IBM Skills Network, traducidos al español y comentados. Los datasets provienen de los materiales del curso.

## Autor

José Pablo Fernández Agüero, Estadístico · [LinkedIn](https://www.linkedin.com/in/TU-PERFIL)
