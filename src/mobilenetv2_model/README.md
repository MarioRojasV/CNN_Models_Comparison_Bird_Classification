# MobileNetV2 — Transfer Learning

Modelo de clasificación basado en **transfer learning** sobre `MobileNetV2` preentrenada en ImageNet. Es el modelo con mejor desempeño de toda la comparación.

## Contenido de esta carpeta

| Archivo | Descripción |
|---|---|
| `MobileNetV2_Birds_COMPDES_2026.ipynb` | Notebook de **entrenamiento**: carga el dataset, congela la base de MobileNetV2, entrena la cabeza de clasificación y guarda el modelo junto con el historial de entrenamiento. |
| `MobileNetV2_Birds_COMPDES_2026_Testing.ipynb` | Notebook de **evaluación** independiente: carga el modelo guardado y calcula métricas sobre el set de prueba (fotos regulares y en vuelo). |

## Arquitectura

```
Input (224x224x3)
 └─ preprocess_input (mobilenet_v2)             ← normalización específica de MobileNetV2
 └─ RandomFlip + RandomRotation(0.05) + RandomZoom(0.05)   ← aumento de datos (leve)
 └─ MobileNetV2(weights="imagenet", include_top=False)     ← base congelada (trainable=False)
 └─ GlobalAveragePooling2D
 └─ Dropout(0.5)
 └─ Dense(2, softmax)                            ← cabeza de clasificación entrenable
```

- **Total de parámetros:** ~2.26 M — de los cuales solo **2,562 son entrenables** (la base preentrenada permanece congelada).
- **Optimizador:** Adam
- **Función de pérdida:** `sparse_categorical_crossentropy`
- **Épocas:** 20
- **Batch size:** 32
- **Tamaño de imagen:** 224×224, con `pad_to_aspect_ratio=True`
- El preprocesamiento de imágenes usa `tf.keras.applications.mobilenet_v2.preprocess_input`, requisito para aprovechar correctamente los pesos de ImageNet.

## Resultados

Entrenamiento (última época registrada):

| Métrica | Valor |
|---|---|
| Accuracy de entrenamiento | 89.4 % |
| Accuracy de validación | **93.75 %** |
| Loss de entrenamiento | 0.217 |
| Loss de validación | 0.201 |

Evaluación sobre el set de prueba (`MobileNetV2_Birds_COMPDES_2026_Testing.ipynb`):

| Conjunto de prueba | Accuracy | Loss |
|---|---|---|
| Fotos regulares (80 imágenes) | **96.25 %** | 0.1502 |
| Fotos en vuelo (20 imágenes) | **75.00 %** | 0.5267 |

*Classification report* — fotos regulares:

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| *Quiscalus mexicanus* (hembra) | 1.000 | 0.925 | 0.961 |
| *Turdus grayi* | 0.930 | 1.000 | 0.964 |

Este es el modelo con **mejor generalización** de los cuatro comparados en este repositorio, tanto en fotos regulares como en fotos en vuelo, gracias a las características visuales aprendidas previamente en ImageNet.

## Cómo ejecutar

1. Abrir `MobileNetV2_Birds_COMPDES_2026.ipynb` en Google Colab para entrenar desde cero, o `MobileNetV2_Birds_COMPDES_2026_Testing.ipynb` para solo evaluar un modelo ya guardado.
2. Montar Google Drive y ajustar `BASE_PATH` / `MODEL_PATH`.
3. Ejecutar las celdas en orden.
