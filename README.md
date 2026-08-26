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

## Alcance

La base reúne los locales con información en las cuatro fuentes: 18,483 de los 69,642 del padrón.
Los perfiles describen a ese conjunto.

- OSIPTEL cubre Bitel, Claro, Entel e Integratel; no incluye a Movistar.
- La cobertura móvil corresponde al centro poblado, no al local.



LInk del colab: https://colab.research.google.com/drive/1rqaA2bh1FbdHIvDvh5_Djn__6hPQkcua?usp=sharing