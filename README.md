# Proyecto Grupo 4 — MCDI503

**Exploración Inteligente para la Ciencia de Datos** · Magíster en Ciencia de Datos e Inteligencia Artificial · Universidad Andrés Bello (Online)

**Integrantes:** Marcelo Corro · Carolina Cortés · Pedro Espinoza · Juan Valdebenito
**Docente:** Sergio Paraíso
**Dataset:** Titanic (`seaborn.load_dataset('titanic')`)

## Estructura

```
data/
  external/puertos_embarque.csv        # Tabla auxiliar de puertos de embarque (integración, Fases 2-3)
  processed/titanic_fase4_modelo.csv   # Dataset preparado para modelamiento (salida de la Fase 4)
notebooks/
  fase1/                               # Fase 1: EDA inicial
  fase2_3/                             # Fases 2 y 3: calidad, limpieza, integración, transformaciones y EDA
  fase4/                               # Fase 4: pipeline de ingeniería y selección de variables
reports/
  figures/                             # Figuras exportadas por los notebooks
  mcdi503_f4_sumativo_grupo4.pdf       # Informe final del proyecto (Fase 4)
```

## Fase 4: pipeline exploratorio de ingeniería y selección de variables

El notebook `notebooks/fase4/mcdi503_f4_sumativo_grupo4.ipynb` es autocontenido: parte de las fuentes originales (`seaborn` y la tabla de puertos) y reaplica la limpieza de las Fases 2 y 3 con la función `preparar_base()`. Luego:

1. **Transforma y normaliza** variables según los hallazgos del EDA (`log1p` en `fare`, *z-score* en `age` y `fare_log`, codificación de categóricas y tramos de `family_size`).
2. **Crea variables derivadas**: `sexo_clase`, `es_nino`, `tarifa_pp_log`, `deck_conocido` y `edad_imputada`.
3. **Selecciona características** con criterios explícitos (relevancia, redundancia, interpretabilidad, calidad y fuga de información).
4. **Documenta** cada decisión en un registro (`log_decisiones`) y encapsula las transformaciones en un `Pipeline` de scikit-learn ajustable solo con entrenamiento.

### Salida: `data/processed/titanic_fase4_modelo.csv`

Dataset de 891 filas × 16 columnas (15 características + `survived`), completamente numérico y sin valores nulos:

| Grupo | Columnas |
|---|---|
| Continuas estandarizadas | `fare_log_z`, `age_z` |
| Tamaño familiar (*one-hot*) | `family_cat_solo`, `family_cat_pequena`, `family_cat_grande` |
| Interacción sexo × clase (*one-hot*) | `sexo_clase_mujer_1` … `sexo_clase_hombre_3` |
| Binarias y ordinales | `sex_male`, `pclass`, `es_nino`, `deck_conocido` |
| Objetivo | `survived` |

> El escalado de este archivo se ajustó con el dataset completo (uso exploratorio). Para modelar, se debe usar el pipeline del notebook ajustado solo con el conjunto de entrenamiento. Al ejecutar el notebook, el CSV se genera en `data/processed/` relativo a la carpeta de ejecución.

## Fuente auxiliar: `data/external/puertos_embarque.csv`

Metadatos de los tres puertos de embarque del Titanic, usados para integrar con la clave `embarked` del dataset.

| Columna | Descripción |
|---|---|
| `embarked` | Código del puerto en el dataset Titanic (C, Q, S). Clave de integración (única). |
| `puerto` | Nombre del puerto en 1912 (Queenstown corresponde a la actual Cobh). |
| `pais` | País según la ubicación geográfica actual del puerto. |
| `lat`, `lon` | Coordenadas aproximadas del centro de la ciudad-puerto (grados decimales, WGS84). |

Fuente de las coordenadas: artículos de Wikipedia *Cherburgo*, *Cobh* y *Southampton* (consultados en 2026).

El notebook de las Fases 2 y 3 lee este archivo directamente desde GitHub:

```python
URL_PUERTOS = 'https://raw.githubusercontent.com/k1ngdom0fbullsh1t/proyecto-grupo4-mcdi503/main/data/external/puertos_embarque.csv'
puertos = pd.read_csv(URL_PUERTOS)
```
