# Escolaridad y condiciones de vida en Chile
### Análisis de los microdatos del Censo 2024 (INE)

¿Cómo se asocia el nivel de escolaridad de las personas con sus condiciones de vida (trabajo, vivienda y acceso a internet), una vez controlados factores como la edad y el sexo?

Proyecto de análisis de datos con Python que usa los microdatos del Censo de Población y Vivienda 2024 de Chile: 12,4 millones de personas de 25 años o más que viven en viviendas particulares.

## Resultado principal

Un mayor nivel educativo se asocia con mejores condiciones de vida en las tres dimensiones estudiadas, y la asociación se mantiene al controlar por edad, sexo, región y nacionalidad.

| Dimensión | Básica o menos | Superior | Brecha ajustada |
|---|---|---|---|
| Ocupación (25 a 64 años) | 56,4 % | 80,4 % | +24,0 puntos |
| Hacinamiento | 12,2 % | 4,4 % | −7,8 puntos |
| Acceso a internet fija en el hogar | 48,3 % | 81,7 % | +33,4 puntos |

*Porcentajes ajustados por edad, sexo, región y nacionalidad.*

Un hallazgo metodológico: la edad cumple papeles distintos según la dimensión. En hacinamiento, sin controlarla, los niveles intermedios parecían tener *más* hacinamiento que básica o menos (paradoja de Simpson); al controlarla, el orden se corrige.

Los resultados describen asociaciones y no demuestran causalidad.

## Contenido del repositorio

```text
censo2024-escolaridad-y-condiciones-de-vida/
├── notebooks/
│   ├── 01_preparacion_datos.ipynb   # carga, validación y construcción de la base
│   └── 02_analisis.ipynb            # exploración, asociaciones, modelos y conclusiones
├── requirements.txt
├── .gitignore
└── README.md
```

Las carpetas `data/raw/` y `data/processed/` no se incluyen porque los datos son pesados; se crean siguiendo los pasos de la sección siguiente.

- **Notebook 01:** carga las tres tablas del censo, las valida contra las cifras oficiales del INE y construye la base analítica.
- **Notebook 02:** el análisis principal. Se recomienda leer primero este.

## Cómo reproducir el análisis

1. Instala Python 3.12 (el proyecto se desarrolló con la versión 3.12.7) y las dependencias:
   ```bash
   pip install -r requirements.txt
   ```
   Para ejecutar los notebooks también necesitas Jupyter (`pip install jupyterlab`).
2. Crea la carpeta `data/raw/viv_hog_per_censo2024/` en la raíz del proyecto, descarga los microdatos del Censo 2024 (formato Parquet: personas, hogares y viviendas) desde el portal del INE, https://censo2024.ine.gob.cl/resultados/, y guárdalos ahí con los nombres `personas_censo2024.parquet`, `hogares_censo2024.parquet` y `viviendas_censo2024.parquet`.
3. Ejecuta `notebooks/01_preparacion_datos.ipynb`, que crea la carpeta `data/processed/` y genera `data/processed/base_analitica_censo2024.parquet` (unos 115 MB).
4. Ejecuta `notebooks/02_analisis.ipynb`.

## Decisiones metodológicas principales

- **Universo:** personas de 25 años o más en viviendas particulares con escolaridad válida (67,2 % de la población censada).
- **Validación:** los totales de registros y el promedio de escolaridad se contrastaron con las cifras publicadas por el INE.
- **Modelos:** regresión logística (scikit-learn, sin regularización) sobre una muestra reproducible del 4 %, con controles sucesivos.

## Limitaciones

Se detallan en la sección 5.2 del notebook 02. Las principales: es una asociación y no causalidad; el análisis cubre a personas de 25 años o más en viviendas particulares; el hacinamiento y el acceso a internet son características del hogar; y los modelos no incluyen intervalos de confianza.

## Herramientas

Python, DuckDB, pandas, NumPy, Matplotlib y scikit-learn.

## Datos y fuente

Instituto Nacional de Estadísticas (INE), Censo de Población y Vivienda 2024, microdatos y *Manual de Uso de Microdatos Censales* (versión del 12-12-2025). Los datos no se incluyen en este repositorio.

## Autor

Nelson Hernández Becerra  
[LinkedIn](https://www.linkedin.com/in/nnhb)