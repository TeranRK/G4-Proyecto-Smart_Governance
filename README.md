## Proyecto Smart Governance 

Smart Governance se refiere a la aplicación de tecnologías avanzadas y datos inteligentes para mejorar la eficiencia, transparencia y participación ciudadana en la gestión pública y toma de decisiones gubernamentales.

![](https://img.shields.io/github/stars/pandao/editor.md.svg) ![](https://img.shields.io/github/forks/pandao/editor.md.svg) ![](https://img.shields.io/github/tag/pandao/editor.md.svg) ![](https://img.shields.io/github/release/pandao/editor.md.svg) ![](https://img.shields.io/github/issues/pandao/editor.md.svg) ![](https://img.shields.io/bower/v/editor.md.svg)

## Tabla de Contenidos

- [1. Descripción del Dataset](#1-descripción-del-dataset)
  - [Origen de los datos](#origen-de-los-datos)
  - [Número de artículos](#número-de-artículos)

- [2. Pre-procesamiento](#2-pre-procesamiento)
  - [Combinación de BibTeX a CSV](#combinación-de-bibtex-a-csv)
  - [Unificación de Dataset](#unificación-de-dataset)
  - [Limpieza de datos](#limpieza-de-datos)
  - [Transformaciones aplicadas](#transformaciones-aplicadas)

- [3. Visualizaciones](#3-visualizaciones)
  - [Análisis Exploratorio de Datos (EDA)](#análisis-exploratorio-de-datos-eda)
  - [Frecuencia de palabras clave](#frecuencia-de-palabras-clave)

- [4. Modelos no supervisados empleados](#4-modelos-no-supervisados-empleados)
  - [Identificación de conceptos clave](#identificación-de-conceptos-clave)
  - [Agrupación por palabras clave](#agrupación-por-palabras-clave)

### Origen de los Datos
### 🔗 Links: <https://www.scopus.com/home.uri>
![](https://upload.wikimedia.org/wikipedia/commons/2/26/Scopus_logo.svg) 

**Número de artículos**
- Se seleccionaron 2,851 documentos de Scopus, que incluyen artículos de investigación y conference papers. Los datos, recopilados entre 2020 y 2024, están todos en inglés y se concentran en el tema de smart governance en el contexto de ciudades inteligentes. Estos documentos proporcionan una visión amplia y actualizada sobre los avances tecnológicos y la aplicación de la smart governance.

### 🔗 Links: <https://www.sciencedirect.com/>
![](https://upload.wikimedia.org/wikipedia/commons/3/35/ScienceDirect_logo_2020.svg)

**Número de artículos**
- Se descargaron 1,716 artículos de investigación, todos en inglés, que abarcan el período de 2020 a 2024. Estos artículos se enfocan en estudios detallados y análisis profundos sobre la implementación de tecnologías avanzadas en la gestión urbana, específicamente dentro del marco de la smart governance. Los datos de ScienceDirect se descargaron en formato BibTeX.

### Pre-procesamiento
### Combinación de Bibtex a CSV
- Los datos fueron inicialmente descargados en formato BibTeX. Para facilitar su análisis, se realizó una conversión de este formato a CSV utilizando Google Colab.

    ```bash
    !pip install bibtexparser
    ```
    

- Luego ejecutamos el siguiente script

    ```bash
    import bibtexparser
    with open("/content/drive/MyDrive/SCIENCE_DIRECT/ScienceDirect_citations_1721933036744.bib") as bibtex_file:
        bib_database = bibtexparser.load(bibtex_file)
        
	df = pd.DataFrame(bib_database.entries)
	selection = df[['doi', 'number','title','keywords','abstract','year']]
	selection.to_csv('/content/science1.csv', index=False)
    ```
- Así procedemos con cada uno de los archivos BibTeX de ScienceDirect. Después de esto, para unificar todos los archivos en uno solo, ejecutamos el siguiente código:

  ```bash
  # Lista de rutas a los archivos CSV
  csv_files = [
    "/content/science1.csv",
    "/content/science2.csv",
    "/content/science3.csv",
    "/content/science4.csv",
    "/content/science5.csv",
    "/content/science6.csv",
    "/content/science7.csv",
    "/content/science8.csv",
    "/content/science9.csv",
    "/content/science10.csv",
    # Agrega aquí las rutas a los otros archivos CSV]
    # Lista para almacenar los DataFrames 
    dataframes = []
    # Leer cada archivo CSV y agregarlo a la lista de DataFrames
    for file in csv_files:
        df = pd.read_csv(file)
        dataframes.append(df)
    # Combinar todos los DataFrames en uno solo
    combined_df = pd.concat(dataframes, ignore_index=True)
    # Guardar el DataFrame combinado en un nuevo archivo CSV
    combined_df.to_csv('/content/science_combined.csv', index=False)
    ```
