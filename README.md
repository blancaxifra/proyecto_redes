# Segmentación y clasificación de imágenes médicas para patologías pulmonares

## 1. Descripción del proyecto

Este proyecto pertenece a la asignatura de Redes Neuronales y tiene como objetivo desarrollar un sistema experimental de apoyo al diagnóstico para imágenes de tomografía computarizada (TC) de pulmón.

El trabajo se basa en el dataset IQ-OTH/NCCD Lung Cancer Dataset, compuesto por imágenes clasificadas en tres categorías diagnósticas:

- Normal
- Benigno
- Maligno

El objetivo principal es aplicar técnicas de redes neuronales convolucionales y transfer learning para clasificar imágenes médicas, incorporando además análisis exploratorio, preprocesamiento, evaluación rigurosa y técnicas de explicabilidad visual.

---

## 2. Objetivos del proyecto

### Objetivo general

Desarrollar un pipeline basado en redes neuronales convolucionales para clasificar imágenes de TC pulmonar en casos normales, benignos y malignos.

### Objetivos específicos

- Analizar la estructura y calidad del dataset.
- Estudiar la distribución de clases y posibles problemas de desbalanceo.
- Analizar el tamaño, formato y características técnicas de las imágenes.
- Crear particiones de entrenamiento, validación y prueba.
- Implementar un modelo baseline CNN entrenado desde cero.
- Aplicar transfer learning con modelos preentrenados.
- Comparar el rendimiento de distintas arquitecturas.
- Evaluar los modelos mediante métricas adecuadas para imagen médica.
- Aplicar técnicas de explicabilidad visual como Grad-CAM.
- Analizar limitaciones del dataset y del enfoque propuesto.

---

## 3. Dataset utilizado

Dataset: IQ-OTH/NCCD Lung Cancer Dataset  
Fuente: Kaggle

El dataset contiene imágenes de TC pulmonar organizadas en carpetas. Durante el análisis inicial se identificaron imágenes etiquetadas en tres clases principales:

- `Normal cases`
- `Bengin cases`
- `Malignant cases`

Además, existe una carpeta `Test cases`, que no se considera una clase diagnóstica supervisada porque no contiene etiquetas explícitas de normal, benigno o maligno.

Por tanto, el entrenamiento y la evaluación supervisada se realizarán únicamente sobre las imágenes etiquetadas.

---

## 4. Estado actual del proyecto

Hasta el momento se ha realizado:

- Descarga y carga del dataset.
- Exploración de la estructura de carpetas.
- Separación entre imágenes etiquetadas y carpeta `Test cases`.
- Normalización de nombres de clase.
- Conteo de imágenes por clase.
- Análisis del desbalanceo del dataset.
- Análisis de tamaño, resolución y modo de imagen.
- Identificación de imágenes con resolución atípica.
- Justificación del redimensionamiento común.
- Creación de particiones `train`, `validation` y `test`.

---

## 5. Resultados iniciales del análisis exploratorio

El conjunto etiquetado contiene 1097 imágenes distribuidas en tres clases:

- Malignant: 561 imágenes
- Normal: 416 imágenes
- Benign: 120 imágenes

Se observa un desbalanceo relevante, especialmente en la clase benigna.

Respecto a las resoluciones:

- Resolución dominante: 512x512 píxeles
- Imágenes 512x512: 1036
- Imágenes con resolución atípica: 61

La mayoría de imágenes atípicas pertenecen a la clase maligna, por lo que se aplicará un preprocesamiento homogéneo a todas las imágenes para evitar que el modelo aprenda diferencias técnicas asociadas a la clase.

---

## 6. Metodología propuesta

El pipeline general del proyecto será:

1. Carga del dataset.
2. Análisis exploratorio de datos.
3. Limpieza y organización de imágenes.
4. División en entrenamiento, validación y prueba.
5. Preprocesamiento:
   - Conversión a RGB.
   - Redimensionamiento a 224x224 píxeles.
   - Normalización de intensidades.
   - Aumentación de datos en entrenamiento.
6. Entrenamiento de una CNN baseline.
7. Entrenamiento de modelos mediante transfer learning.
8. Comparación de resultados.
9. Análisis de errores.
10. Aplicación de Grad-CAM para explicabilidad visual.
11. Discusión de resultados y limitaciones.

---

## 7. Modelos previstos

### Baseline

Se implementará una CNN sencilla entrenada desde cero para tener un punto de comparación inicial.

### Modelos preentrenados

Se probarán arquitecturas de transfer learning, previsiblemente:

- MobileNetV2
- ResNet50
- EfficientNetB0

Estas redes se adaptarán al problema de clasificación multiclase con tres salidas: normal, benigno y maligno.

---

## 8. Métricas de evaluación

Dado que se trata de un problema médico y existe desbalanceo entre clases, no se utilizará únicamente accuracy.

Las métricas previstas son:

- Accuracy
- Precision macro
- Recall macro
- F1-score macro
- Matriz de confusión
- Recall específico de la clase maligna
- Recall específico de la clase benigna

Se prestará especial atención a los falsos negativos en la clase maligna, ya que en un contexto clínico serían especialmente críticos.

---

## 9. Explicabilidad del modelo

Para mejorar la interpretación del sistema, se aplicará Grad-CAM sobre el mejor modelo obtenido.

El objetivo será visualizar qué regiones de la imagen influyen más en la predicción de la red neuronal y comprobar si el modelo se centra en zonas anatómicamente relevantes o si utiliza artefactos visuales no deseados.

---

## 10. Segmentación y limitaciones

Aunque el tema general del proyecto está relacionado con segmentación de imágenes médicas, el dataset utilizado no proporciona máscaras manuales de lesiones o tumores.

Por ello, no se planteará una segmentación supervisada de tumores con U-Net salvo que se incorpore un dataset adicional con máscaras.

En su lugar, el proyecto podrá incluir:

- Segmentación aproximada o preprocesamiento de región pulmonar.
- Recorte de regiones de interés.
- Explicabilidad visual mediante Grad-CAM.
- Discusión de U-Net como arquitectura relevante en el estado del arte.

Esta decisión evita plantear una segmentación tumoral no verificable con los datos disponibles.

---

## 11. Estructura provisional del proyecto

```text
proyecto_lung_cancer/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── 01_eda_dataset.ipynb
│   ├── 02_baseline_cnn.ipynb
│   ├── 03_mobilenet.ipynb
│   ├── 04_resnet50.ipynb
│   ├── 05_efficientnetb0.ipynb
│   ├── 06_models_comparison
│   ├── 07_gradcam_explainability.ipynb
│   └── 08_results_analysis.ipynb
│
├── models/
│
├── reports/
│   ├── figures/
│   └── memoria/
│
├── README.md
└── requirements.txt
