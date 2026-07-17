# CNN Models Comparison — Bird Classification (Costa Rica)

Comparación de cuatro enfoques de clasificación de imágenes para distinguir dos aves urbanas de Costa Rica: el **Yigüirro** (*Turdus grayi*) y la hembra del **Zanate grande** (*Quiscalus mexicanus*). El proyecto evalúa qué tan bien se comporta cada arquitectura tanto en fotografías "normales" (aves posadas) como en fotografías más difíciles (aves en vuelo), usando un dataset propio, curado y de-biasado.

Proyecto desarrollado para **COMPDES 2026**.

---

## Motivación

*Turdus grayi* y la hembra de *Quiscalus mexicanus* son fáciles de confundir a simple vista: tamaño similar, plumaje pardo/oscuro y hábitats compartidos en zonas urbanas costarricenses. Este repositorio compara distintas estrategias de Deep Learning para resolver ese problema de clasificación binaria, y analiza cómo se degrada el desempeño cuando las aves están en movimiento (vuelo) en vez de posadas.

## Modelos comparados

| Modelo | Enfoque | Accuracy (fotos regulares) | Accuracy (fotos en vuelo) | Documentación |
|---|---|---|---|---|
| **CNN Básica** | Red convolucional simple (`Sequential`, sin transfer learning) | 81.25 % | 60.00 % | [`src/cnn_basic_model`](src/cnn_basic_model/README.md) |
| **CNN Baseline** | Red convolucional propia (3 bloques Conv2D) entrenada desde cero, con aumento de datos | 81.25 % | 60.00 % | [`src/cnn_baseline_model`](src/cnn_baseline_model/README.md) |
| **MobileNetV2** | Transfer learning (ImageNet) con capa densa final | **96.25 %** | **75.00 %** | [`src/mobilenetv2_model`](src/mobilenetv2_model/README.md) |
| **Teachable Machine** | Modelo entrenado con Google Teachable Machine (sin código) | 91.25 % | 70.00 % | [`src/teachable_machine_model`](src/teachable_machine_model/README.md) |

> Los porcentajes provienen de la evaluación guardada en cada notebook de *testing*. Al re-entrenar con el dataset propio, los valores pueden variar levemente. Cada carpeta de `src/` documenta su propia metodología, arquitectura y resultados con más detalle.

**Conclusión general:** el transfer learning con **MobileNetV2** obtiene el mejor desempeño en ambos escenarios, seguido por **Teachable Machine**. Los modelos CNN entrenados desde cero, sin pesos preentrenados, son los que más sufren con las fotografías en vuelo (menos datos, mayor variabilidad de pose y fondo).

## Estructura del repositorio

```
CNN_Models_Comparison_Bird_Classification/
├── data/                                  # Metadata y atribuciones legales de las imágenes (CC)
│   ├── attribution_records.md
│   ├── regular_turdus_grayi_attributions.md
│   ├── regular_quiscalus_mexicanus_attributions.md
│   ├── flying_turdus_grayi_attributions.md
│   ├── flying_quiscalus_mexicanus_attributions.md
│   └── dataset_link.md
├── src/
│   ├── cnn_basic_model/                   # CNN simple — notebook de evaluación
│   ├── cnn_baseline_model/                # CNN propia — entrenamiento + evaluación
│   ├── mobilenetv2_model/                 # Transfer learning — entrenamiento + evaluación
│   └── teachable_machine_model/           # Modelo exportado de Teachable Machine — evaluación
├── LICENSE
└── README.md
```

## Dataset
###(Puede ser accedido y descargado a través de este link: [`dataset/`](https://github.com/MarioRojasV/CNN_Models_Comparison_Bird_Classification/releases/tag/v1.0-dataset)
- 250 imágenes de *Turdus grayi* + 250 imágenes de hembras de *Quiscalus mexicanus* (fotos "regulares", ave posada).
- 10 imágenes adicionales por especie de aves **en vuelo**, usadas como conjunto de prueba adicional para medir la robustez de cada modelo ante poses no convencionales.
- Todas las imágenes fueron seleccionadas bajo licencias **Creative Commons**, con su respectiva atribución documentada en [`data/`](data/attribution_records.md).
- División utilizada durante el entrenamiento: `Training_set`, `Validation_set` y `Testing_set` (con subcarpetas `Regular_photos` y `Flying_photos`).

> El dataset de imágenes en sí **no** se distribuye en este repositorio (por tamaño y por licenciamiento individual de cada foto). Lo que se documenta aquí es la trazabilidad legal de cada imagen (autor, fuente, licencia).

## Cómo reproducir los experimentos

1. Clonar el repositorio.
2. Reconstruir el dataset (`Training_set` / `Validation_set` / `Testing_set`) siguiendo los créditos y enlaces listados en `data/`, respetando la licencia de cada imagen.
3. Abrir el notebook del modelo deseado (todos están preparados para correr en **Google Colab** con Google Drive montado).
4. Ajustar las rutas (`BASE_PATH`, `TRAIN_PATH`, etc.) a la ubicación real del dataset.
5. Ejecutar las celdas en orden: carga de datos → aumento de datos → construcción del modelo → entrenamiento → evaluación (matriz de confusión, *classification report*, análisis de confianza).

### Requisitos

- Python 3.10+
- `tensorflow` / `tf-keras`
- `numpy`, `matplotlib`, `scikit-learn`, `Pillow`
- Entorno con GPU recomendado (Google Colab funciona sin configuración adicional)

## Licencia

El **código fuente** (notebooks, scripts y modelos entrenados dentro de `src/`) se distribuye bajo licencia **MIT** — ver [`LICENSE`](LICENSE).

El **dataset de imágenes** (`data/`) **no** está cubierto por la licencia MIT: cada imagen conserva su propia licencia Creative Commons individual, documentada con el formato TASL (Title, Author, Source, License) en los archivos de atribución correspondientes.

## Autoría

Proyecto académico desarrollado para **COMPDES 2026**.

Autores:

- Mario Andrés Rojas Varela
- María Laura Alpízar Rodríguez
- Marcos Eduardo Zamora Sánchez
