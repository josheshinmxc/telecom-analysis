# Análisis de Usuarios LATAM — Proyecto de Análisis de Datos

## Objetivo del Proyecto

El objetivo de este proyecto es realizar un análisis exploratorio de datos sobre una base de usuarios de una empresa de telecomunicaciones en Latinoamérica. A través del análisis se busca:

- Identificar y tratar problemas de calidad en los datos (nulos, outliers, valores inválidos).
- Entender el comportamiento de uso de los usuarios (llamadas, mensajes, minutos).
- Segmentar a los usuarios por edad y nivel de uso.
- Generar insights accionables para la toma de decisiones de negocio.

---

## Datasets 

`users_latam.csv` -  Información demográfica y de plan de cada usuario
`plans.csv` - Detalle de los planes disponibles (Básico y Premium)
`usage.csv` - Registros de uso por usuario (llamadas y mensajes)

---

## Etapas del Análisis

### 1. Exploración Inicial
- Revisión de tipos de datos, dimensiones y primeras filas de cada dataset.
- Identificación de columnas numéricas, categóricas y de fechas.

### 2. Calidad de Datos y Limpieza
- Detección y tratamiento de valores nulos por columna.
- Reemplazo del valor centinela `-999` en `age` por la mediana.
- Reemplazo de `?` por `NaN` en la columna `city`.
- Marcado de fechas futuras (año > 2024) en `reg_date` como `NaT`.
- Verificación de nulos estructurales en `duration` y `length` (correlacionados con la columna `type`).
- Verificación de valores inválidos en columnas categóricas (`plan`, `city`).

### 3. Detección de Outliers
- Análisis de outliers con el método IQR en columnas numéricas: `age`, `cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`.
- Visualización mediante boxplots.
- Aplicación de cap (techo) en `cant_minutos_llamada` en el límite superior de 61.86 minutos.

### 4. Agregación y Combinación de Datos
- Construcción de tabla agregada de `usage` por `user_id` con:
  - `cant_mensajes`: total de mensajes enviados.
  - `cant_llamadas`: total de llamadas realizadas.
  - `cant_minutos_llamada`: total de minutos de llamadas.
- Combinación con `users_latam` mediante `merge` por `user_id`.

### 5. Visualización
- Histogramas por plan (`hue='plan'`) para: `age`, `cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`.
- Boxplots para detección visual de outliers.
- Countplots para distribución de segmentos.

### 6. Segmentación de Usuarios
- **Segmento por edad** (`grupo_edad`):
  - `Joven`: age < 30
  - `Adulto`: age < 60
  - `Adulto Mayor`: resto
- **Segmento por nivel de uso** (`grupo_uso`):
  - `Bajo uso`: llamadas < 5 y mensajes < 5
  - `Uso medio`: llamadas < 10 y mensajes < 10
  - `Alto uso`: resto

---

## Cómo Ejecutar el Notebook

### Opción 1 — Google Colab (recomendado)

1. Abre [Google Colab](https://colab.research.google.com/).
2. Ve a **Archivo > Subir notebook** y selecciona el archivo `.ipynb` del proyecto.
3. Sube los archivos de datos (`users_latam.csv`, `plans.csv`, `usage.csv`) desde **Archivo > Subir al entorno de ejecución**, o colócalos en tu Google Drive y móntalo con:


from google.colab import drive
drive.mount('/content/drive')


4. Ejecuta las celdas en orden con `Shift + Enter` o desde el menú **Entorno de ejecución > Ejecutar todo**.

### Opción 2 — Local con Jupyter Notebook

1. Clona el repositorio:
```bash
git clone https://github.com/tu_usuario/tu_repositorio.git
cd tu_repositorio
```

2. Instala las dependencias:
```bash
pip install pandas matplotlib seaborn
```

3. Abre el notebook:
```bash
jupyter notebook
```

---

## 🔁 Guía de Reproducción

Para reproducir el análisis de principio a fin, sigue este orden:

1. **Cargar los datos**: importar los 3 archivos CSV con `pd.read_csv()`.
2. **Exploración inicial**: revisar `.dtypes`, `.shape`, `.head()` y `.describe()` de cada dataset.
3. **Limpieza**: ejecutar las celdas de tratamiento de nulos, valores centinela y fechas inválidas.
4. **Outliers**: correr el análisis IQR y los boxplots.
5. **Agregación**: construir la tabla `usage_agg` y hacer el `merge` con `users`.
6. **Visualizaciones**: ejecutar los histogramas y countplots.
7. **Segmentación**: crear las columnas `grupo_edad` y `grupo_uso`.
8. **Insights**: revisar el resumen ejecutivo al final del notebook.

**Asegúrate de ejecutar las celdas **en orden**, ya que algunas dependen de transformaciones anteriores.**

