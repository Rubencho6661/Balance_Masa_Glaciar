# Balance de Masa Glaciar — Aplicación Shiny

> Aplicación web interactiva para el cálculo estandarizado de SMBs basada en el modelo clásico de Lliboutry.  
> Proyecto: *Revisiting Inter-Annual Glacier Mass Balance as a Response to the Recent Climate Variability Along the Andes Region* · Universidad Yachay Tech · 2025

---

## Contenido

1. [Descripción general](#descripción-general)
2. [Carga de archivos](#carga-de-archivos)
3. [Configuración de parámetros](#configuración-de-parámetros)
4. [Ejecución del modelo](#ejecución-del-modelo)
5. [Estructura del repositorio](#estructura-del-repositorio)
6. [Dependencias](#dependencias)

---

## Descripción general

Esta aplicación implementa el modelo no lineal de Lliboutry para calcular el Balance de Masa Superficial (SMB) de glaciares, con soporte para ajuste geodésico según la metodología de Zemp. Está desplegada en **Posit Connect Cloud** y el cuaderno asociado se encuentra disponible en **Kaggle**.

Los resultados se presentan en pestañas interactivas que permiten explorar:

- Balance de masa promedio anual
- Balance anual
- Relación altura – balance promedio
- Relación altura – balance por año

---

## Carga de archivos

Utilice el panel lateral izquierdo para cargar los archivos antes de ejecutar el modelo.

| Archivo | Formato | Descripción | Requerimiento |
|---|---|---|---|
| `balance.txt` | `.txt` / `.csv` | Columnas separadas por tabulación: `fecha`, `coord_X`, `coord_Y`, `altitud`, `balance`. Sin encabezados. | **Requerido** |
| `glaciar.shp` | `.shp` + `.dbf` `.shx` `.prj` | Polígono de la superficie glaciar. Cargar los 4 archivos simultáneamente. | **Requerido** |
| `dem.tif` | `.tif` | Raster georreferenciado que cubra el área completa del glaciar. | **Requerido** |
| `balance_geo.xlsx` | `.xlsx` | Columnas de período y balance geodésico. Mejora la precisión del modelo. | Opcional |

### Formato del archivo de balance puntual

Cada fila corresponde a una observación. Sin encabezados. Columnas separadas por tabulación:

```
2000-01-01    817916.836    9946615.416    5575    0.530
2000-01-01    817823.067    9946742.497    5475    0.750
2000-01-01    817707.090    9946870.812    5375    0.250
```

> ⚠️ **Importante:** Todos los archivos deben estar georreferenciados en la misma proyección cartográfica (mismo código EPSG). De lo contrario, el modelo producirá errores o resultados incorrectos.

---

## Configuración de parámetros

Verifique y ajuste los parámetros en el panel lateral antes de ejecutar el modelo.

| Parámetro | Valor por defecto | Descripción |
|---|---|---|
| `rango_calculo` | `50` | Rango en metros para el cálculo del modelo. |
| `resolucion_raster` | `100` | Resolución en metros del raster de salida. |
| `crs_epsg` | `32717` | Sistema de referencia de coordenadas. Debe coincidir con el de los archivos cargados. |
| `n_obs_min` | `6` | Mínimo de observaciones requeridas por celda para el cálculo. |
| `altitud_cima` | `5700` | Altitud aproximada de la cima del glaciar en metros. |

---

## Ejecución del modelo

1. Haga clic en **Ejecutar modelo** en la parte inferior del panel lateral, una vez cargados todos los archivos requeridos.
2. Monitoree el progreso en la sección de log. Mensajes en verde indican operaciones exitosas; en rojo, errores.
3. Al finalizar, navegue por los resultados en las pestañas: *Balance de masa promedio anual*, *Balance anual*, *Altura vs. Balance* y *Altura vs. Balance por año*.
4. Descargue los resultados completos en formato `.zip` haciendo clic en **Descargar resultados**.

> ℹ️ **Tiempo de ejecución:** Depende del tamaño del glaciar, la resolución del raster y la cantidad de datos de entrada. Para glaciares grandes o resoluciones finas, el proceso puede tardar varios minutos.

---

## Estructura del repositorio

```
balance-masa-glaciar/
├── app.R                        # Punto de entrada de la app Shiny
├── Libraries.R                  # Instalación de dependencias
├── Funcion_Geoglacier.R         # Función principal coordinadora
├── Lliboutry_NL_shiny.R         # Modelo no lineal de Lliboutry
├── BrickBuilder_shiny.R         # Construcción de bloques de datos
├── B_LMf_W_shiny.R              # Método ponderado área/altitud
├── B_LMf_Idw_shiny.R            # Interpolación IDW
├── B_LMf_Krg_shiny.R            # Interpolación por Kriging
├── Reanal_glac_geo_Zemp.R       # Error del balance glaciológico
├── ContourPlot.R                # Gráficos de líneas de contorno
├── RasterPlot.R                 # Gráficos raster
├── Graph_Adj.R                  # Gradiente altitudinal (ajustado)
├── Graph_NonAdj.R               # Gradiente altitudinal (no ajustado)
└── data/                        # Datos de prueba (Glaciar Antisana)
```

---

## Dependencias

Instale todas las dependencias ejecutando `source("Libraries.R")` desde la consola de R.

```r
# Interfaz y visualización
shiny, shinyjs, tmap, leaflet, plotly, ggpubr, ggprism, corrplot, DT, factoextra

# Datos espaciales
sf, terra, raster, sp, gdalUtilities, devtools

# Modelamiento y estadística
minpack.lm, gstat, pastecs, matrixStats, performance, Hmisc, circular

# Manipulación de datos
tidyverse, data.table, lubridate, janitor, stringr, zip

# Reportes y archivos
openxlsx, readxl, ncdf4, tiff

# Cómputo paralelo
parallel, doParallel, foreach

# Misceláneos
reticulate, here, splines, psych
```

> ℹ️ **Versión de R recomendada:** R ≥ 4.2. La aplicación fue validada en Posit Connect Cloud con un consumo de RAM inferior al 32% del límite de la plataforma (1 GB).

---

## Entregables

Los notebooks funcionales, datos de prueba y documentación adicional están disponibles en:

🔗 [Google Drive — Carpeta del proyecto](https://drive.google.com/drive/folders/1zzlrBigPaBVYG0INL5j1o3LK83jybY7i?usp=sharing)

---

*Autor: Jonathan Panimboza Deleg · Técnico Especialista en Ciencias de la Tierra*  
*Director: PhD. Rubén Basantes · Universidad de Investigación Tecnológica Experimental Yachay*
