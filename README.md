# dataset-incendios-pichincha-goes19

# Dataset GOES-19 — Incendios Forestales en Pichincha, Ecuador (2025)

## 1. Descripción del proyecto

Este repositorio contiene imágenes satelitales multiespectrales del satélite **NOAA GOES-19**, recopiladas para el proyecto de investigación sobre detección automática de incendios forestales mediante visión por computador y aprendizaje profundo.

El área de estudio corresponde a la provincia de **Pichincha, Ecuador**, con especial interés en zonas de bosques, páramos y montañas.

Los datos se utilizarán para desarrollar y evaluar un modelo híbrido **CNN-GRU**, capaz de analizar información espacial y temporal de imágenes satelitales.

## 2. Datasets disponibles

### Dataset 1: Imágenes de fechas asociadas a incendios forestales

**Archivo:** `DATASET_GOES19_PICHINCHA_2025.zip`

Contiene 151 escenas satelitales multiespectrales recopiladas en fechas seleccionadas de 2025, incluyendo el período del 6 al 7 de julio, asociado a un incendio reportado en el sector del Sincholagua.

Este conjunto también incluye escenas descargadas previamente de otras fechas.

**Importante:** las imágenes no están etiquetadas y no se ha confirmado que todas contengan incendios activos.

### Dataset 2: Imágenes candidatas sin incendios

**Archivo:** `DATASET_CANDIDATO_SIN_INCENDIOS_2025.zip`

Contiene imágenes satelitales correspondientes al 12 de mayo de 2025, entre las 15:00 y las 17:59 UTC.

Se seleccionaron como muestras candidatas de condiciones sin incendios forestales. Su condición negativa todavía requiere verificación con NASA FIRMS VIIRS.

## 3. Características técnicas

| Característica | Descripción |
|---|---|
| Satélite | NOAA GOES-19 |
| Producto | ABI Level 2 MCMIPF |
| Región | Pichincha, Ecuador |
| Año | 2025 |
| Formato principal | NumPy comprimido (.npz) |
| Visualizaciones | PNG |
| Bandas | 7 |
| Dimensiones observadas | 79 × 104 × 7 |
| Etiquetado | Pendiente |
| Modelo objetivo | CNN-GRU |

### Bandas espectrales

- C02: visible, aproximadamente 0,64 µm.
- C03: infrarrojo cercano, aproximadamente 0,86 µm.
- C06: infrarrojo de onda corta, aproximadamente 2,2 µm.
- C07: infrarrojo medio, aproximadamente 3,9 µm.
- C13: infrarrojo térmico, aproximadamente 10,3 µm.
- C14: infrarrojo térmico, aproximadamente 11,2 µm.
- C15: infrarrojo térmico, aproximadamente 12,3 µm.

Las bandas térmicas permiten analizar anomalías de temperatura potencialmente relacionadas con incendios forestales.

## 4. Estructura del repositorio

```text
dataset-incendios-pichincha-goes19/
├── README.md
├── DATASET_GOES19_PICHINCHA_2025.zip
└── DATASET_CANDIDATO_SIN_INCENDIOS_2025.zip
```

Los archivos ZIP contienen imágenes procesadas para su posterior análisis en Python.

## 5. Cómo utilizar las imágenes en Python

Primero, instalar NumPy:

```bash
pip install numpy
```

Después, abrir un archivo NPZ:

```python
import numpy as np

ruta = "G19_20250706_150021.npz"

with np.load(ruta, allow_pickle=False) as datos:
    print("Variables disponibles:", datos.files)

    for nombre in datos.files:
        print(nombre, datos[nombre].shape)
```

Este código permite inspeccionar las variables y dimensiones del archivo antes de seleccionar las bandas para su procesamiento.

## 6. Procesamiento pendiente

Para obtener el dataset definitivo del modelo CNN-GRU será necesario:

1. Verificar los metadatos y la calibración espectral.
2. Comprobar la correspondencia espacial y temporal entre escenas.
3. Crear recortes de 15 × 15 píxeles.
4. Agrupar seis observaciones temporales consecutivas.
5. Calcular tres canales derivados.
6. Verificar incendios y ausencia de incendios mediante NASA FIRMS VIIRS.
7. Construir las etiquetas y los conjuntos de entrenamiento, validación y prueba.

La estructura objetivo de entrada al modelo será:

**6 × 15 × 15 × 10**

Seis instantes temporales, 15 × 15 píxeles y diez canales por instante.

## 7. Fuentes de información

- NOAA GOES: https://www.ncei.noaa.gov/products/satellites/geostationary-operational-environmental-satellites
- NOAA GOES Open Data: https://registry.opendata.aws/noaa-goes/
- NASA FIRMS: https://firms.modaps.eosdis.nasa.gov/

## 8. Estado del proyecto

**Dataset preliminar de imágenes multiespectrales, sin etiquetar.**

Los archivos publicados permiten iniciar el análisis exploratorio y la preparación de secuencias temporales, pero todavía no constituyen un dataset validado para entrenamiento supervisado.

Proyecto académico de investigación — Pichincha, Ecuador, 2025.
