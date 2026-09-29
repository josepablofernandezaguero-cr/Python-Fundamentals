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
