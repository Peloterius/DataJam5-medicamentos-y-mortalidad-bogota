# Desabastecimiento de Medicamentos Prioritarios y Mortalidad Prematura por Enfermedades Crónicas en Adultos Mayores (Bogotá)
 
**Proyecto desarrollado para el Bogotá DataJam (Edición 5, 2026)**
Alcaldía Mayor de Bogotá (DIPEA) y Fundación Universitaria Compensar · **Equipo: TINTOS**
 
*Análisis integrado de la disponibilidad de medicamentos prioritarios y la mortalidad prematura por enfermedades crónicas en adultos de 60 a 69 años en Bogotá, D.C.*
 
---
 
## Tabla de Contenidos
 
* [Acerca del Proyecto](#acerca-del-proyecto)
* [Visualización Rápida (Sin Instalaciones)](#visualización-rápida-sin-instalaciones)
* [Estructura del Repositorio](#estructura-del-repositorio)
* [Fuentes de Datos](#fuentes-de-datos)
* [Metodología y Procesamiento](#metodología-y-procesamiento)
* [Ejecución en la Nube con Google Colab](#ejecución-en-la-nube-con-google-colab)
* [Ejecución Local (Opcional)](#ejecución-local-opcional)
* [Instrucciones de Despliegue](#instrucciones-de-despliegue)
* [Visualizaciones y Resultados](#visualizaciones-y-resultados)
* [Limitaciones](#limitaciones)
* [Documentos de Entrega](#documentos-de-entrega)
* [Equipo de Trabajo](#equipo-de-trabajo)
---
 
## Acerca del Proyecto
 
Los medicamentos prioritarios para enfermedades crónicas pueden escasear justo en los grupos donde más personas mueren antes de tiempo. Este proyecto revisa, para adultos de **60 a 69 años** en Bogotá, si los grupos de enfermedad con más mortalidad prematura son también los que enfrentan más desabastecimiento de medicamentos.
 
**Pregunta:** ¿los grupos de enfermedad crónica con mayor mortalidad prematura en Bogotá son también los que enfrentan más desabastecimiento de medicamentos prioritarios, y cómo ha evolucionado esa relación en el tiempo?
 
**Grupos de enfermedad analizados:** cardiocerebrovascular, neoplasias, respiratorias crónicas y diabetes.
 
### Hipótesis
 
* **H1:** los grupos con más mortalidad prematura concentran más medicamentos prioritarios desabastecidos.
* **H2:** los desabastecimientos de medicamentos prioritarios duran más que los de los no prioritarios.
* **H3:** las localidades con más mortalidad prematura son prioritarias para el monitoreo.
### Objetivos
 
1. **Identificar** los medicamentos prioritarios desabastecidos en Bogotá y asignarlos a su grupo de enfermedad crónica.
2. **Comparar** la mortalidad prematura (60 a 69 años) con el desabastecimiento de medicamentos por grupo de enfermedad, por localidad y por sexo.
3. **Ofrecer herramientas interactivas** (dashboard y nota técnica) para apoyar la toma de decisiones en políticas públicas de salud.
---
 
## Visualización Rápida (Sin Instalaciones)
 
Para revisar los resultados y la documentación final **no se requiere instalar ningún programa o librería**:
 
* **Dashboard Interactivo:** [abrir el dashboard](https://peloterius.github.io/DataJam5-medicamentos-y-mortalidad-bogota/DataJam/outputs/dashboard.html). El archivo editable (`dashboard.html`) está en la carpeta `DataJam/outputs`.
* **Nota Técnica:** [consulta el documento explicativo final (PDF)](DataJam/Docs/NOTA%20T%C3%89CNICA%20TINTOS%20DATAJAM5.pdf).
* **Formulario de caracterización y formulación del problema:** [descargar el formulario (.docx)](DataJam/Docs/Formulario%20DataJam%20TINTOS.docx).
---
 
## Estructura del Repositorio
 
```
DataJam5-medicamentos-y-mortalidad-bogota/
├── README.md                # Documentación principal del proyecto
└── DataJam/
    ├── data/
    │   ├── raw/             # CSV originales del Portal de Datos Abiertos Bogotá
    │   └── processed/       # Datos limpios y clasificados
    ├── notebooks/
    │   └── Notebook.ipynb   # Limpieza, clasificación, integración e indicadores
    ├── outputs/             # Indicadores en CSV y dashboard (archivo editable)
    ├── Docs/
    │   ├── NOTA TÉCNICA TINTOS DATAJAM5.pdf      # Nota técnica (1 página)
    │   └── Formulario DataJam TINTOS.docx        # Formulario de caracterización y formulación del problema
    └── requirements.txt     # Librerías necesarias
```
 
---
 
## Fuentes de Datos
 
Todas provienen del [Portal de Datos Abiertos Bogotá](https://datosabiertos.bogota.gov.co). Hay una copia local en `data/raw/`.
 
| Archivo en `data/raw/` | Fuente |
|---|---|
| `medicamentos.csv` | [Desabastecimiento de medicamentos](https://datosabiertos.bogota.gov.co/dataset/a5908994-8f12-461d-b6a1-a1b5e4497951) (Secretaría Distrital de Salud) |
| `cardio.csv` | [Mortalidad prematura por enfermedad cardiocerebrovascular](https://datosabiertos.bogota.gov.co/dataset/mortalidad-prematura-por-enfermedad-cardiocerebrovascular-en-bogota) |
| `neoplasias.csv` | [Mortalidad prematura por neoplasias](https://datosabiertos.bogota.gov.co/dataset/mortalidad-prematura-por-neoplasias-en-bogota-d-c-30-a-69-anos) |
| `respiratorias.csv` | [Mortalidad prematura por enfermedades crónicas respiratorias bajas](https://datosabiertos.bogota.gov.co/dataset/mortalidad-prematura-por-enfermedades-cronicas-respiratorias-bajas-en-bogota) |
| `diabetes.csv` | [Mortalidad por diabetes mellitus](https://datosabiertos.bogota.gov.co/dataset/tasa-de-mortalidad-por-diabetes-mellitus-bogota-d-c) |
 
---
 
## Metodología y Procesamiento
 
1. **Carga y limpieza:** se normalizan nombres de columnas y textos y se eliminan duplicados.
2. **Medicamentos:** el estado se simplifica en desabastecido, en riesgo, en monitorización y no desabastecido. Solo "desabastecido" cuenta como desabastecimiento efectivo. Se consideran **prioritarios** los de prioridad en salud "enfermedades crónicas" y "cáncer".
3. **Clasificación:** cada medicamento prioritario se asigna a uno de los 4 grupos de enfermedad, por la columna de prioridad (cáncer) y por palabras clave en la indicación y el nombre. Hay excepciones manuales documentadas en el notebook.
4. **Mortalidad:** se filtran los grupos de edad 60-64 y 65-69. Se suman los casos por grupo, año, localidad, sexo, nacionalidad y CIE-10.
5. **Integración:** se cruzan medicamentos y mortalidad por grupo de enfermedad, en el último año común a los 4 grupos.
6. **Indicadores y gráficas:** brecha entre mortalidad y desabastecimiento, estado y causas de los medicamentos, evolución en el tiempo, lectura por localidad y por sexo.
---
 
## Ejecución en la Nube con Google Colab
 
Hay dos formas de ejecutar el notebook en Colab sin instalar nada en tu equipo.
 
### Opción 1: desde GitHub
 
1. Haz clic en el botón: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Peloterius/DataJam5-medicamentos-y-mortalidad-bogota/blob/main/DataJam/notebooks/Notebook.ipynb)
2. En la barra superior, selecciona **Entorno de ejecución > Ejecutar todas**.
También puedes ver una vista previa del notebook directamente en el repositorio: https://github.com/Peloterius/DataJam5-medicamentos-y-mortalidad-bogota
 
### Opción 2: desde Google Drive
 
1. Sube la carpeta `DataJam` del repositorio a *Mi unidad*.
2. Abre `notebooks/Notebook.ipynb` en Colab y autoriza el acceso a Drive.
3. Selecciona **Entorno de ejecución > Reiniciar y ejecutar todo**.
El notebook genera los archivos de `data/processed/` y `outputs/`.
 
---
 
## Ejecución Local (Opcional)
 
Requiere **Python 3.10 o superior**. Si prefieres ejecutar el código en tu entorno de desarrollo habitual:
 
1. **Clonar el repositorio:**
```bash
   git clone https://github.com/Peloterius/DataJam5-medicamentos-y-mortalidad-bogota.git
   cd DataJam5-medicamentos-y-mortalidad-bogota/DataJam
```
2. **Crear y activar un entorno virtual:**
```bash
   python -m venv venv
   source venv/bin/activate    # En macOS/Linux
   # venv\Scripts\activate     # En Windows
```
3. **Instalar dependencias:**
```bash
   pip install -r requirements.txt
```
4. **Abrir Jupyter Notebook:**
```bash
   jupyter notebook notebooks/Notebook.ipynb
```
5. En el menú, selecciona *Kernel → Restart & Run All*. El notebook encuentra la carpeta `data/` por sí solo.
---
 
## Instrucciones de Despliegue
 
El dashboard es un único archivo HTML (`DataJam/outputs/dashboard.html`) que no necesita servidor ni dependencias, y se publica con **GitHub Pages**:
 
1. En el repositorio de GitHub, entra a **Settings → Pages**.
2. En *Build and deployment*, elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`, y guarda.
3. Tras uno o dos minutos, el dashboard queda disponible en: https://peloterius.github.io/DataJam5-medicamentos-y-mortalidad-bogota/DataJam/outputs/dashboard.html
Para regenerarlo, ejecuta el notebook (`DataJam/notebooks/Notebook.ipynb`), que actualiza `DataJam/outputs/`, y sube los cambios a `main`. GitHub Pages lo republica solo.
 
Para usarlo sin internet, descarga `dashboard.html` y ábrelo en cualquier navegador (Chrome, Edge, Firefox).
 
---
 
## Visualizaciones y Resultados
 
El proyecto cuenta con un dashboard interactivo para explorar las métricas por grupo de enfermedad, localidad y sexo:
 
* **Dashboard:** [abrir el dashboard](https://peloterius.github.io/DataJam5-medicamentos-y-mortalidad-bogota/DataJam/outputs/dashboard.html) · archivo editable en `DataJam/outputs/dashboard.html`.
* **Nota Técnica:** [disponible en `DataJam/Docs`](DataJam/Docs/NOTA%20T%C3%89CNICA%20TINTOS%20DATAJAM5.pdf) (PDF).
### Resultados del último año común (2026, parcial)
 
Las muertes son casos de 60 a 69 años, no tasas.
 
| Grupo | Muertes prematuras | Medicamentos prioritarios desabastecidos |
|---|---|---|
| Cardiocerebrovascular | 744 | 4 |
| Neoplasias | 697 | 3 |
| Diabetes | 123 | 2 |
| Respiratorias | 108 | 0 |
 
El orden por mortalidad coincide con el orden por desabastecimiento. Con solo 4 grupos la comparación es descriptiva y no estadística.
 
---
 
## Limitaciones
 
* La relación es descriptiva, no causal.
* Los datos de mortalidad vienen en casos y no en tasas.
* 2026 es un año parcial, y el grupo cardiocerebrovascular solo trae ese año.
* El listado de medicamentos es de un solo mes y no trae fecha de cierre, por eso H2 y la evolución de medicamentos no se pueden calcular.
* La clasificación de medicamentos es manual (palabras clave) y los medicamentos no tienen edad ni territorio.
---
 
## Documentos de Entrega
 
Están en la carpeta `DataJam/Docs`:
 
| Documento | Descripción |
|---|---|
| [Nota técnica (PDF)](DataJam/Docs/NOTA%20T%C3%89CNICA%20TINTOS%20DATAJAM5.pdf) | Dificultades de acceso y calidad de los datos, retos de integración, limitaciones y recomendaciones (1 página). |
| [Formulario (.docx)](DataJam/Docs/Formulario%20DataJam%20TINTOS.docx) | Formulario de caracterización y formulación del problema, con la descripción del dashboard, los hallazgos y las capturas de pantalla. |
 
---
 
## Equipo de Trabajo
 
**Equipo TINTOS**
 
| Integrante | Rol | Contacto |
|---|---|---|
| **Jean Phillipe Carreño Riaño** | Análisis de Datos / Modelado | jphillipecarreno@ucompensar.edu.co |
| **Santiago Salazar Lizarazo** | Preprocesamiento / ETL | ssalazarl@ucompensar.edu.co |
| **Jhoan Stit Fragua Avila** | Desarrollo / Visualización | [@Peloterius](https://github.com/Peloterius) · jfragua@ucompensar.edu.co |
