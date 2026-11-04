# Perfiles multidimensionales de brecha tecnológica en los locales educativos del Perú

Proyecto del curso de Data Mining (Universidad del Pacífico, docente Soledad Espezúa Llerena) — Grupo **Mineros**.
Delgado Santana, Francisco Luis · Pando Cabezas, Nicole Rashel · Rua Pomahuacre, Brayan Anderson

## Descripción

Identifica perfiles de brecha tecnológica en los locales educativos del Perú combinando su acceso a
internet, la disponibilidad y el estado de sus recursos tecnológicos y la cobertura móvil de su
entorno distrital. Está dirigido al MINEDU, las DRE y las UGEL.

**Pregunta de análisis:** ¿qué perfiles de brecha tecnológica se distinguen entre los locales
educativos del Perú al combinar estas tres dimensiones, y cómo se distribuyen en el territorio?

La unidad de análisis es el local educativo. Las tablas de recursos y de líneas traen varias filas por
local, así que se agregan a nivel `CODLOCAL` antes de unirlas. La base integrada tiene
**42 221 locales × 54 columnas**; la matriz analítica, 42 221 × 17 variables.

## Fuentes de datos

| Fuente | Institución | Archivo |
|---|---|---|
| Ficha lineal del local (Censo Educativo 2025) | MINEDU – ESCALE | `loc_lineal_1.dbf` (en `datos/loc_lineal_1.zip`) |
| Recursos tecnológicos (P91) | MINEDU – ESCALE | `loc_p91_rectec.dbf` |
| Líneas de internet (P3230) | MINEDU – ESCALE | `Loc_p3230_inter.dbf` |
| Padrón de locales (Padlocal) | MINEDU – ESCALE | `Padlocal.dbf` |
| Cobertura móvil por centro poblado, operadora y tecnología (2025-IV) | OSIPTEL (datos abiertos) | `osiptel_cobertura_2025IV_parte1.xlsx` y `parte2.xlsx` |
| Límites de provincias del Perú (196), capa `per_admbnda_adm2_ign_20200714` | IGN vía OCHA, HDX (COD-AB) | shapefile, en `datos/peru_shapes/` |

Portal ESCALE: <https://escale.minedu.gob.pe> · Cobertura móvil: <https://www.datosabiertos.gob.pe/dataset/porcentaje-de-cobertura-m%C3%B3vil-por-centro-poblado-empresa-operadora-y-tecnolog%C3%ADa>

### Shapefile de provincias (análisis geográfico)

Capa de provincias `per_admbnda_adm2_ign_20200714` (IGN vía OCHA, *Peru - Subnational Administrative Boundaries*,
COD-AB, versión 14/07/2020, 196 provincias; fuente: <https://data.humdata.org/dataset/cod-ab-per>).
Ya está incluida en `datos/peru_shapes/` (archivos `.shp`, `.shx`, `.dbf`, `.prj`, `.cpg`, `.sbn`, `.sbx`
y `.shp.xml`), por lo que no hay que descargarla ni cambiar rutas: el cuaderno la lee con la ruta relativa
`datos/peru_shapes/per_admbnda_adm2_ign_20200714.shp`.

## Técnica seleccionada

Clustering sobre componentes principales:

1. Se eliminan los 6 locales con faltantes (42 215 × 17).
2. `log1p` en `cantidad_total_recursos` y `cantidad_lineas_internet`; `StandardScaler` en las 11 variables
   no binarias; las 6 binarias quedan en 0/1.
3. PCA con 5 componentes (Z1–Z5, 84,4 % de la varianza), matriz `Z_df`.
4. **K-means con k = 5** (k de 2 a 8 evaluado con inercia, silueta y Davies-Bouldin): silueta 0,331,
   Davies-Bouldin 1,073.
5. Comparación: Ward sobre una muestra de 5 000 locales, corte en 5 grupos (0,289; 1,139), y DBSCAN
   (`eps` 0,3–0,7, `min_samples` = 10), que no produce perfiles útiles.
6. Análisis geográfico: unión espacial con las provincias, perfiles por departamento, índice de
   concentración, mapa del perfil predominante y DBSCAN espacial (haversine, 10 km, `min_samples` = 10)
   sobre el perfil G3 (pequeños y desconectados): 124 focos.

| Grupo | % locales | Perfil |
|---|---|---|
| G0 | 16,8 | Conectados con cobertura 4G/5G |
| G1 | 15,9 | Pequeños, antiguos y en reparación |
| G2 | 26,2 | Bien equipados y conectados |
| G3 | 31,4 | Pequeños y desconectados |
| G4 | 9,7 | Inventario grande pero inoperativo |

## Estructura

```
Hito3_Entrega.ipynb         Notebook principal (limpieza, integración, PCA, clustering y análisis geográfico)
informe_hito3/              Informe del Hito 3 (formato IEEE conference)
    Mineros_Hito3.pdf         informe compilado
    Mineros_Hito3.tex         fuente LaTeX
    figuras/                  mapas usados en el informe
datos/                      Archivos fuente
    peru_shapes/              shapefile de provincias (IGN/OCHA)
salidas/                    Resultados de entregas anteriores
requirements.txt            Dependencias
Hito2_Entrega.ipynb, Hito2_PCA.ipynb, Proyecto_Mineros_Primera_Entrega.ipynb   Cuadernos de entregas anteriores
```

Informe del Hito 3: [`informe_hito3/Mineros_Hito3.pdf`](informe_hito3/Mineros_Hito3.pdf).

## Cómo ejecutar

`datos/loc_lineal_1.dbf` pesa 163 MB y supera el límite de GitHub, por eso se sube comprimido.
Hay que descomprimirlo antes de ejecutar el notebook:

```bash
unzip datos/loc_lineal_1.zip -d datos/
pip install -r requirements.txt
jupyter notebook Hito3_Entrega.ipynb
```

Todas las rutas (datos y shapefile) son relativas a `datos/`, así que hay que ejecutarlo desde la raíz del repositorio.
Las celdas se ejecutan en orden. El informe se compila con
`pdflatex Mineros_Hito3.tex` (dos veces) dentro de `informe_hito3/` (clase `IEEEtran`).

## Alcance y límites

- La base reúne los 42 221 locales del censo (67 145) que tienen registro de recursos tecnológicos;
  los otros 24 924 quedan fuera. Los perfiles describen a ese conjunto, y 42 215 reciben perfil.
- La cobertura móvil es distrital: todos los locales de un distrito comparten el valor, y no asegura
  señal en el local. OSIPTEL cubre Bitel, Claro, Entel e Integratel; no incluye a Movistar.
- Pendiente para el Hito 4: validación con área (urbano/rural) y gestión, detección de anomalías y
  discusión global.

Link del colab: https://colab.research.google.com/drive/1NROPlQCj0G6-5YIp2ktx0qwg78kIsYNR?usp=sharing
Link de la actividad de PCA y NMF: https://colab.research.google.com/drive/1bvi3vbLQaz1fLOgoblBoiafOcyUTBP9_?usp=sharing
