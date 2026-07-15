# Teachable Machine — Evaluación

Modelo entrenado con **[Google Teachable Machine](https://teachablemachine.withgoogle.com/)**, una herramienta de entrenamiento de modelos de imagen sin escribir código (no-code). Se incluye aquí únicamente el notebook que evalúa ese modelo exportado sobre el mismo set de prueba usado por los demás modelos, para poder comparar resultados de forma justa.

## Contenido de esta carpeta

| Archivo | Descripción |
|---|---|
| `Teachable_Machine_Birds_COMPDES_2026_Testing.ipynb` | Notebook de **evaluación**: carga el modelo `.h5` exportado desde Teachable Machine y lo evalúa sobre `Regular_photos` y `Flying_photos`. |

> El entrenamiento en sí **no** ocurre en este repositorio: se realizó directamente en la plataforma web de Teachable Machine, subiendo las mismas imágenes de entrenamiento usadas por los otros modelos. El resultado de ese entrenamiento (`keras_model.h5` + `labels.txt`) es lo que se evalúa aquí.

## Qué hace el notebook

1. Instala `tf_keras` (necesario para cargar modelos exportados por Teachable Machine).
2. Carga el modelo (`keras_model.h5`) y las etiquetas (`labels.txt`).
3. Empareja automáticamente las carpetas del dataset de prueba con las clases reconocidas por el modelo (comparando el nombre de la especie).
4. Preprocesa cada imagen igual que la app oficial de Teachable Machine: recorte centrado, redimensionado a 224×224 con LANCZOS y normalización a `[-1, 1]`.
5. Evalúa sobre `Regular_photos` y `Flying_photos`, generando matrices de confusión, *classification report* y análisis de confianza por categoría.

## Resultados

| Conjunto de prueba | Accuracy | Loss (cross-entropy) |
|---|---|---|
| Fotos regulares (80 imágenes) | **91.25 %** | 0.2692 |
| Fotos en vuelo (20 imágenes) | **70.00 %** | 1.2452 |

*Classification report* — fotos regulares:

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| *Q. mexicanus* | 0.902 | 0.925 | 0.914 |
| *T. grayi* | 0.923 | 0.900 | 0.911 |

*Classification report* — fotos en vuelo:

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| *Q. mexicanus* | 0.625 | 1.000 | 0.769 |
| *T. grayi* | 1.000 | 0.400 | 0.571 |

Teachable Machine ofrece un desempeño sorprendentemente competitivo para ser una solución sin código, quedando en segundo lugar general por debajo de MobileNetV2 y por encima de las CNN entrenadas desde cero.

## Cómo ejecutar

1. Entrenar un modelo en [teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com/) usando las mismas clases e imágenes de entrenamiento del dataset, y exportarlo en formato **Keras (TensorFlow)** (`keras_model.h5` + `labels.txt`).
2. Abrir `Teachable_Machine_Birds_COMPDES_2026_Testing.ipynb` en Google Colab.
3. Montar Google Drive y ajustar `MODEL_PATH`, `LABELS_PATH` y las rutas del `Testing_set`.
4. Ejecutar las celdas en orden.
