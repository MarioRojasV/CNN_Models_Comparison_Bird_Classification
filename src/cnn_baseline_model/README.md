# CNN Baseline — Clasificación de Aves

Red convolucional propia, entrenada **desde cero** (sin transfer learning), usada como línea base ("baseline") para comparar contra MobileNetV2 y Teachable Machine.

## Contenido de esta carpeta

| Archivo | Descripción |
|---|---|
| `CNN_Baseline_AvesCR.ipynb` | Notebook de **entrenamiento**: carga el dataset, aplica aumento de datos, define y entrena la CNN, y evalúa sobre validación, testing (fotos regulares) y fotos en vuelo. |
| `CNN_Baseline_Birds_COMPDES_2026_Testing.ipynb` | Notebook de **evaluación** independiente: carga el modelo ya entrenado (`.keras`) y genera matrices de confusión, *classification report* y análisis de confianza sobre el set de prueba. |

> **Nota:** El modelo entrenado (`cnn_baseline_avesCR.keras`) no se incluye en este repositorio debido al límite de tamaño de GitHub. Puede generarse ejecutando el notebook `CNN_Baseline_AvesCR.ipynb`.


## Arquitectura

Red `Sequential` de Keras/TensorFlow, con aumento de datos incorporado como primeras capas del modelo:

```
Input (224x224x3)
 └─ RandomFlip("horizontal") + RandomRotation(0.2) + RandomZoom(0.2)   ← aumento de datos
 └─ Rescaling(1./255)                                                  ← normalización
 └─ Conv2D(32, 3x3, relu)  → MaxPooling2D
 └─ Conv2D(64, 3x3, relu)  → MaxPooling2D
 └─ Conv2D(128, 3x3, relu) → MaxPooling2D
 └─ Flatten
 └─ Dense(128, relu)
 └─ Dropout(0.5)
 └─ Dense(2, softmax)                                                  ← salida binaria
```

- **Total de parámetros:** ~11.17 M (todos entrenables, no hay pesos preentrenados).
- **Optimizador:** Adam
- **Función de pérdida:** `sparse_categorical_crossentropy`
- **Épocas:** 20
- **Batch size:** 32
- **Tamaño de imagen:** 224×224, con `pad_to_aspect_ratio=True` para no deformar las aves.
- **Semilla (reproducibilidad):** 42

## Resultados

Entrenamiento (última época registrada, sobre 340 imágenes de entrenamiento):

| Métrica | Valor |
|---|---|
| Accuracy de entrenamiento (época 20) | ≈ 78.2 % |
| Loss de entrenamiento (época 20) | ≈ 0.45 |

Evaluación sobre el set de prueba (`CNN_Baseline_Birds_COMPDES_2026_Testing.ipynb`):

| Conjunto de prueba | Accuracy | Loss |
|---|---|---|
| Fotos regulares (80 imágenes) | **81.25 %** | 0.4605 |
| Fotos en vuelo (20 imágenes) | **60.00 %** | 0.9119 |

*Classification report* — fotos regulares:

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| *Quiscalus mexicanus* (hembra) | 0.857 | 0.750 | 0.800 |
| *Turdus grayi* | 0.778 | 0.875 | 0.824 |

El modelo funciona razonablemente bien con aves posadas, pero su desempeño cae de forma notable con aves en vuelo — esperable en una red sin pesos preentrenados y con un dataset relativamente pequeño (340 imágenes de entrenamiento).

Las matrices de confusión (conteo y normalizada en porcentaje) se generan y guardan automáticamente al ejecutar el notebook de evaluación.

## Cómo ejecutar

1. Abrir el notebook en **Google Colab**.
2. Montar Google Drive.
3. Ajustar las rutas (`BASE_PATH`, `MODEL_PATH`, etc.) según la ubicación del dataset.
4. Ejecutar las celdas en orden.

Si se desea volver a entrenar el modelo, ejecutar `CNN_Baseline_AvesCR.ipynb`.

Si únicamente se desea evaluar un modelo previamente entrenado, utilizar `CNN_Baseline_Birds_COMPDES_2026_Testing.ipynb`, indicando la ubicación del archivo del modelo generado durante el entrenamiento.