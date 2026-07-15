# CNN Básica — Evaluación

Notebook de evaluación para la CNN convolucional simple entrenada desde cero (misma familia arquitectónica que el modelo **CNN Baseline**, ver [`src/cnn_baseline_model`](../cnn_baseline_model/README.md)).

## Contenido de esta carpeta

| Archivo | Descripción |
|---|---|
| `CNN_Basic_Birsd_COMPDES_2026_Testing.ipynb` | Notebook de **evaluación**: carga el modelo `.keras` entrenado (guardado en `Resultados/cnn_baseline_avesCR.keras`) y calcula métricas sobre el set de prueba. |

> ℹEsta carpeta solo contiene el notebook de evaluación. El entrenamiento del modelo correspondiente se documenta en [`src/cnn_baseline_model`](../cnn_baseline_model/README.md), donde también está disponible el archivo `.keras` generado.

## Qué hace el notebook

1. Carga el modelo entrenado (`tf.keras.models.load_model`).
2. Construye los conjuntos de prueba `Regular_photos` y `Flying_photos` con `image_dataset_from_directory` (224×224, `pad_to_aspect_ratio=True`).
3. Calcula *accuracy* y *loss* de evaluación (`model.evaluate`).
4. Genera matrices de confusión (conteo y normalizada) para ambos conjuntos.
5. Imprime el *classification report* (precisión, recall, F1) por clase.
6. Corre un análisis adicional de confianza de las predicciones por categoría.

## Resultados

| Conjunto de prueba | Accuracy | Loss |
|---|---|---|
| Fotos regulares (80 imágenes) | **81.25 %** | 0.4605 |
| Fotos en vuelo (20 imágenes) | **60.00 %** | 0.9119 |

*Classification report* — fotos regulares:

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| *Quiscalus mexicanus* (hembra) | 0.857 | 0.750 | 0.800 |
| *Turdus grayi* | 0.778 | 0.875 | 0.824 |

*Classification report* — fotos en vuelo:

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| *Quiscalus mexicanus* (hembra) | 0.571 | 0.800 | 0.667 |
| *Turdus grayi* | 0.667 | 0.400 | 0.500 |

## Cómo ejecutar

1. Abrir el notebook en Google Colab (badge "Open in Colab" incluido).
2. Montar Google Drive.
3. Ajustar `MODEL_PATH` y las rutas de `Testing_set` (`Regular_photos` / `Flying_photos`) a la ubicación real del dataset y del modelo `.keras`.
4. Ejecutar las celdas en orden.
