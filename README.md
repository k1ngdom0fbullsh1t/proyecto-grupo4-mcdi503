# Proyecto Grupo 4 — MCDI503

**Exploración Inteligente para la Ciencia de Datos** · Magíster en Ciencia de Datos e Inteligencia Artificial · Universidad Andrés Bello (Online)

**Integrantes:** Marcelo Corro · Carolina Cortés · Pedro Espinoza · Juan Valdebenito
**Docente:** Sergio Paraíso
**Dataset:** Titanic (`seaborn.load_dataset('titanic')`)

## Estructura

```
data/
  external/puertos_embarque.csv   # Tabla auxiliar de puertos de embarque (integración, Fases 2-3)
notebooks/
  fase1/                          # Fase 1: EDA inicial
  fase2_3/                        # Fases 2 y 3: calidad, limpieza, integración, transformaciones y EDA
reports/
  figures/                        # Figuras exportadas por los notebooks
```

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
