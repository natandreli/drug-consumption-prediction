# Clasificación del Consumo de Drogas Basado en Rasgos de Personalidad y Datos Demográficos

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Status](https://img.shields.io/badge/Status-Completed-green)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Focus](https://img.shields.io/badge/Focus-Machine%20Learning%20%7C%20Psychometrics-red)

> **Predicción de riesgo multi-salida basada en el modelo de los "Cinco Grandes" (Big Five) utilizando el dataset de la UCI.**

## Descripción del Proyecto

Este proyecto aborda el desafío de predecir el nivel de consumo de **6 sustancias psicoactivas** (Cannabis, Cocaína, Heroína, Éxtasis, Benzodiacepinas y LSD) basándose exclusivamente en perfiles psicológicos y datos demográficos.

A diferencia de enfoques binarios tradicionales, este modelo implementa una **Regresión Multi-Salida (Multi-Output)** para predecir un índice de riesgo en una escala ordinal de 7 niveles (0-6), permitiendo una granularidad mucho mayor en la estratificación del riesgo.

### Fuente de Datos
El conjunto de datos utilizado es el **Drug Consumption (Quantified)** del Repositorio de Machine Learning de la UCI: **[Enlace al Dataset Oficial](https://archive.ics.uci.edu/dataset/373/drug+consumption+quantified)**

### Objetivos Principales
1. **Modelado Predictivo:** Comparar el desempeño de modelos lineales, ensambles y redes neuronales en un problema de alta dimensionalidad y desbalance de clases.
2. **Reducción de Dimensión:** Evaluar si el cuestionario psicométrico puede simplificarse mediante PCA o UMAP sin perder capacidad predictiva.
3. **Interpretabilidad:** Identificar qué rasgos de personalidad (ej. *Sensation Seeking*) son los detonantes principales del consumo.

## Tecnologías y Herramientas

El proyecto fue desarrollado en Python utilizando las siguientes librerías:

* **Procesamiento de Datos:** `pandas`, `numpy`, `imbalanced-learn` (RandomOverSampler).
* **Machine Learning:** `scikit-learn` (Ridge, KNN, RF, SVR, PCA), `umap-learn`.
* **Deep Learning:** `torch` (PyTorch) para la Red Neuronal Multi-Task.
* **Optimización:** `optuna` para la búsqueda bayesiana de hiperparámetros.
* **Visualización:** `matplotlib`, `seaborn`, `plotly`.

## Metodología

### 1. Preprocesamiento
* **Limpieza:** Transformación de variables categóricas y codificación ordinal.
* **Balanceo:** Aplicación de **Random OverSampling (ROS)** para mitigar el desbalance severo en clases minoritarias (usuarios de heroína/LSD).
* **Escalado:** Estandarización de rasgos numéricos (StandardScaler).

### 2. Modelos Evaluados
Se entrenaron y optimizaron 5 familias de algoritmos:
* **Ridge Regression:** Baseline lineal con regularización L2.
* **K-Nearest Neighbors (KNN):** Regresión no paramétrica basada en instancias.
* **Multi-Task Neural Network:** MLP con *backbone* compartido y cabezas específicas por droga.
* **Random Forest Regressor:** Ensamble de árboles (Bagging).
* **Support Vector Regression (SVR):** Modelado con kernel RBF.

### 3. Reducción de Dimensión
Se analizó la redundancia de las 33 variables de entrada comparando **PCA** (lineal) vs **UMAP** (manifold learning).

## Resultados Clave

| Modelo | RMSE Global | RMSLE | Conclusión |
| :--- | :---: | :---: | :--- |
| **Random Forest** | **1.48** | **0.620** | 🏆 **Mejor Desempeño.** Robusto a ruido y no-linealidades. |
| Red Neuronal | 1.51 | 0.667 | Competitivo, pero requiere más datos para generalizar. |
| SVR | 1.62 | 0.669 | Dificultad para modelar la granularidad fina (7 clases). |
| Ridge (Base) | 1.77 | 0.787 | Insuficiente para capturar la complejidad del problema. |

### Hallazgos Importantes
* **Sensation Seeking & Openness:** Son los predictores más potentes, superando a cualquier variable demográfica.
* **Eficacia de PCA:** Se logró reducir el espacio de entrada de **33 a 11 variables** (66% de reducción) manteniendo el error del Random Forest estable (RMSLE 0.626). Esto sugiere que los tests psicológicos pueden acortarse significativamente en producción.
* **Falla de UMAP:** Las proyecciones de UMAP degradaron el rendimiento de regresión (+22% de error), demostrando que la preservación de estructura local no favorece la predicción de magnitudes globales.
