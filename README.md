# Perfiles de infraestructura digital en locales educativos del Perú

Proyecto del curso de Data Mining — Grupo **Mineros**.
Delgado Santana, Francisco Luis · Pando Cabezas, Nicole Rashel · Rua Pomahuacre, Brayan Anderson

## Descripción

Integra cuatro fuentes oficiales para caracterizar la infraestructura digital de los locales
educativos del país: recursos tecnológicos y líneas de internet (MINEDU–ESCALE), el padrón de
locales y la cobertura móvil por centro poblado (OSIPTEL).

Las tablas de recursos y de líneas traen varias filas por local, así que se pivotean a nivel
`CODLOCAL` antes de unirlas. La base final integra **18,483 locales × 93 columnas**, sin
faltantes por ausencia de fuente, y queda lista para el clustering de la segunda fase.

## Estructura

```
Proyecto_Mineros_Primera_Entrega.ipynb   Notebook principal
datos/                                   Archivos fuente
salidas/                                 Resultados que genera el notebook
```

## Cómo ejecutar

```bash
pip install -r requirements.txt
jupyter notebook Proyecto_Mineros_Primera_Entrega.ipynb
```

Las rutas son relativas (`datos/` y `salidas/`), así que hay que ejecutarlo desde la raíz del
repositorio. Correr todas las celdas en orden regenera los CSV y las figuras de `salidas/`.

## Limitaciones

- La unión usa `inner`, por lo que la base cubre el 26,5 % del padrón (18,483 de 69,642). Los
  excluidos son sobre todo locales rurales, así que los perfiles no representan al sistema
  educativo nacional.
- OSIPTEL no incluye a Movistar, solo a Bitel, Claro, Entel e Integratel.
- La cobertura móvil es del centro poblado, no del local.
