[README-Classification.md](https://github.com/user-attachments/files/32821149/README-Classification.md)
# Clasificación: KNN, árboles de decisión, regresión logística y SVM

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU-USUARIO/clasificacion-knn-arboles-svm/blob/main/notebooks/ML_Classification.ipynb)

Cuatro algoritmos de clasificación supervisada con **scikit-learn**, cada uno aplicado a un problema distinto: segmentación de clientes, asignación de medicamentos, fuga de clientes (churn) y diagnóstico de células.

## Objetivo

Entrenar, evaluar e interpretar modelos de clasificación con las métricas adecuadas para cada problema, y comparar su desempeño sobre datos de prueba.

## Contenido

| Técnica | Dataset | Datos | Métrica | Resultado |
|---|---|---|---|---|
| K vecinos más cercanos (KNN) | `teleCust1000t.csv` | 1000 clientes, 11 variables, 4 categorías de servicio | Accuracy en prueba | 0.32 con k = 4 |
| Árbol de decisión | `drug200.csv` | 200 pacientes (140 entrenamiento / 60 prueba), 5 variables | Accuracy en prueba | 0.983 |
| Regresión logística | `ChurnData.csv` | 200 clientes (160 / 40), 7 variables | Log loss | 0.61 |
| Máquina de soporte vectorial (SVM) | `cell_samples.csv` | 683 muestras tras la limpieza (546 / 137), 9 variables | F1 ponderado / Jaccard | 0.964 / 0.944 |

## Metodología

- **KNN:** variables estandarizadas, partición 80/20 y comparación de k entre 1 y 6 con accuracy e intervalo de ±1 y ±3 errores estándar.
- **Árbol de decisión:** entrenamiento, predicción, evaluación y visualización del árbol.
- **Regresión logística:** regularización `C = 0.01` con solver `liblinear`, matriz de confusión y pérdida logarítmica.
- **SVM:** limpieza de la variable `BareNuc` (conversión a numérica), reporte de clasificación y matriz de confusión.

## Hallazgos

- **KNN** tiene un desempeño bajo: con k = 4 alcanza 0.55 en entrenamiento y 0.32 en prueba. Para cuatro categorías, un clasificador al azar acertaría alrededor de 0.25 si estuvieran balanceadas, así que las variables disponibles separan mal los grupos y la brecha entre entrenamiento y prueba sugiere sobreajuste.
- **Árbol de decisión:** 0.983 de accuracy, aunque con solo 60 observaciones de prueba la estimación es poco precisa.
- **Regresión logística:** log loss de 0.61 sobre 40 clientes de prueba.
- **SVM:** con F1 ponderado de 0.964, la clase maligna (4) obtiene recall de 1.00 y precisión de 0.90; la benigna (2) obtiene precisión de 1.00 y recall de 0.94. En prueba no se escapó ningún caso maligno, a costa de 5 falsos positivos, lo cual es preferible cuando el costo de un falso negativo es mayor.

## Próximos pasos

- Validación cruzada en lugar de una sola partición, sobre todo para los conjuntos pequeños.
- Búsqueda de hiperparámetros (k, profundidad del árbol, `C`, kernel).
- Curvas ROC y matrices de confusión para todos los modelos.
- Comparar KNN con modelos basados en árboles para el problema de segmentación.

## Cómo reproducirlo

La forma más simple es abrirlo en Google Colab con el botón de arriba.

Para correrlo localmente:

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook notebooks/ML_Classification.ipynb
```

El notebook descarga algunos datos con `wget`. Si trabajas en Windows, reemplaza esa celda por `pd.read_csv(url)` con la misma dirección.

## Créditos

Basado en los laboratorios del curso *Machine Learning with Python* de IBM Skills Network, traducidos al español y comentados. El dataset de células proviene de un conjunto público del repositorio UCI Machine Learning, distribuido en los materiales del curso.

## Autor

José Pablo Fernández Agüero, Estadístico · [LinkedIn](https://www.linkedin.com/in/TU-PERFIL)


# Clustering: jerárquico y DBSCAN

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU-USUARIO/clustering-jerarquico-dbscan/blob/main/notebooks/ML_Clustering.ipynb)

Agrupamiento no supervisado con **scikit-learn** y **SciPy**: clustering jerárquico aglomerativo aplicado a datos sintéticos y a un conjunto de vehículos, y DBSCAN aplicado a datos sintéticos y a estaciones meteorológicas.

## Objetivo

Descubrir grupos naturales en los datos sin etiquetas, comparar métodos de agrupamiento y visualizar los resultados con dendrogramas y mapas.

## Contenido

| Sección | Técnica | Datos |
|---|---|---|
| 1 | Agrupamiento jerárquico aglomerativo (enlace `average`) y dendrogramas (`complete` y `average`) | 50 puntos sintéticos generados con `make_blobs` en 4 centros |
| 2 | Agrupamiento jerárquico con SciPy y con scikit-learn | `cars_clus.csv`: 159 vehículos, 117 tras la limpieza |
| 3 | DBSCAN e identificación de valores atípicos, con comparación frente a K-medias | Datos sintéticos |
| 4 | DBSCAN por ubicación, y por ubicación y temperatura | Estaciones meteorológicas, año 2014 |

## Metodología

- **Limpieza:** conversión de las columnas a numéricas y eliminación de registros con valores faltantes (de 159 a 117 vehículos).
- **Variables de los vehículos:** `engine_s`, `horsepow`, `wheelbas`, `width`, `length`, `curb_wgt`, `fuel_cap` y `mpg`, normalizadas con `MinMaxScaler`.
- **Jerárquico:** matriz de distancias euclidianas, dendrogramas con enlaces `complete` y `average`, y corte en un número de clusters.
- **DBSCAN:** agrupamiento por densidad; los puntos que no pertenecen a ninguna región densa se marcan como ruido.
- **Estaciones meteorológicas:** DBSCAN por coordenadas y luego incorporando la temperatura, con visualización en mapa.

## Hallazgos

- El agrupamiento jerárquico recupera bien los cuatro grupos de los datos sintéticos, y el dendrograma permite decidir el número de clusters.
- DBSCAN no exige fijar el número de clusters de antemano y detecta valores atípicos, a diferencia de K-medias.
- En las estaciones meteorológicas se obtienen 9 clusters, con temperaturas medias que van de −16.3 °C a 6.8 °C, de modo que los grupos reflejan zonas climáticas distintas.

## Próximos pasos

- Justificar el número de clusters con métricas (silhouette, método del codo).
- Al llamar a `linkage` con una matriz de distancias, usar su forma condensada (`scipy.spatial.distance.squareform`); con la matriz cuadrada SciPy emite una advertencia.
- Interpretar los clusters de vehículos según su tipo y sus características.
- Reemplazar `basemap`, que está en desuso, por `cartopy` o `plotly` para los mapas.

## Cómo reproducirlo

La forma más simple es abrirlo en Google Colab con el botón de arriba.

Para correrlo localmente:

```bash
pip install numpy pandas matplotlib scipy scikit-learn jupyter basemap
jupyter notebook notebooks/ML_Clustering.ipynb
```

`basemap` puede dar problemas de instalación según la versión de Python; en ese caso usa Colab. El notebook descarga los datos con `wget`; en Windows reemplaza esas celdas por `pd.read_csv(url)` con la misma dirección.

> El notebook pesa unos 5 MB por los mapas incrustados, así que puede tardar en cargar en GitHub.

## Créditos

Basado en los laboratorios del curso *Machine Learning with Python* de IBM Skills Network, traducidos al español y comentados. Los datasets provienen de los materiales del curso.

## Autor

José Pablo Fernández Agüero, Estadístico · [LinkedIn](https://www.linkedin.com/in/TU-PERFIL)

[README-Clustering.md](https://github.com/user-attachments/files/32821179/README-Clustering.md)



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

[README-Regression.md](https://github.com/user-attachments/files/32821189/README-Regression.md)

