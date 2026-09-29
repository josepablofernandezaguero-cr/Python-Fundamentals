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
