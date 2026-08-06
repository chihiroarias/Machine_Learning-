# Especificación — Laboratorios de la clase 2

> Documento de cátedra. No se distribuye.

## Los dos notebooks

| Archivo | Qué es | Duración estimada |
|---|---|---|
| `lab_2_carga_datos.ipynb` | Piso de herramienta: cargar un archivo y manejar un DataFrame | 25–35 min |
| `lab_2.ipynb` | **La práctica de la clase**, una sola, de punta a punta | 90–110 min |

**No hay laboratorio domiciliario separado.** La práctica es una: se empieza en clase y
**lo que no se complete se termina fuera de ella**. El entregable —el informe de hallazgos de la
sección 10— se entrega igual. El notebook lo dice en la introducción y en el cierre, sin usar la
palabra «domiciliario» ni fijar bloques de tiempo.

Los dos son de **experimentación metodológica**: el entregable es un informe justificado con
evidencia, no una función que pasa un test. Aun así se implementan doce celdas con NumPy y
pandas, porque son la base del criterio.

## Decisiones del docente

**1. Los datos se cargan de un archivo, no se generan en el notebook.** El CSV lo produce
`src/generar_dataset_sucio.py` con semilla fija; el notebook solo lo lee. La celda de carga
busca el archivo en tres ubicaciones y, si no lo encuentra y está en Colab, abre el diálogo de
subida. Ya no hay celda oculta con el generador embebido.

**2. El notebook de carga usa un dataset real.** `heart.csv` (Heart Failure Prediction, 918
pacientes, 12 variables), descargado desde
`https://raw.githubusercontent.com/gustavovazquez/datasets/main/heart.csv`. Es el mismo que usa
el notebook `01 carga_dataframes_pandas...` del material previo. Sin credencialización y sin
subir archivos: abre en Colab tal cual.

**3. Sin MAD ni z-score modificado.** Se explican en notas y slides; acá no se implementan. La
detección univariada se hace con la regla $1{,}5\,$IQR y con el z-score clásico, y el contraste
entre ambos es lo que muestra el enmascaramiento.

**4. Sin detección multivariada.** Mahalanobis queda en notas y slides. Isolation Forest y LOF
ya no forman parte de la clase.

**5. Sin emojis y en registro formal e impersonal.** Marcadores en texto plano
(`# VERIFICACIÓN`, `# DECIDE:`, `### Para analizar`).

## Datasets

| Notebook | Dataset | Origen | Filas |
|---|---|---|---|
| `lab_2_carga_datos` | `heart.csv` | URL pública (repositorio del docente) | 918 × 12 |
| `lab_2` | `mantenimiento_industrial.csv` | `data/`, generado por `src/generar_dataset_sucio.py` | 4237 × 14 |

`data/entregas_logistica.csv` y `src/generar_dataset_logistica.py` quedan en el repositorio sin
uso asignado: eran del laboratorio domiciliario, que se eliminó.

## `lab_2_carga_datos.ipynb` — estructura

| # | Sección | Qué escribe el estudiante |
|---|---|---|
| 0 | Preparación | — |
| 1 | Cargar un CSV | — (tabla de argumentos de `read_csv`; el identificador se lee como texto) |
| 2 | Primer vistazo | Contar `Cholesterol == 0` y su efecto sobre la media |
| 3 | Selección de columnas y filas | — (`loc` contra `iloc`, rango cerrado contra abierto) |
| 4 | Filtrado por condición | `filtrar(df, columna, minimo, maximo)` |
| 5 | Columnas derivadas | — (`pd.cut`, `SettingWithCopyWarning` y el `.copy()`) |
| 6 | Agrupar y resumir | `resumen_por_grupo(df, grupo, variable)` |
| 7 | Tablas cruzadas | — |
| 8 | Ordenar, contar, únicos | — |
| 9 | Faltantes y tipos | DECIDE: qué hacer con los 172 ceros de `Cholesterol` |
| 10 | Guardar el resultado | — |

**Dos funciones y una decisión.** El hallazgo que engancha con la práctica siguiente: un mínimo
de 0 en una variable fisiológica es un código de error, no una medición.

## `lab_2.ipynb` — estructura

| # | Sección | Qué escribe el estudiante | Verificación |
|---|---|---|---|
| 0 | Preparación y carga | — | `df.shape == (4237, 14)` |
| 1 | Estructura y duplicados | `contar_duplicados_reales` | `== 37` |
| 2 | Códigos numéricos de error | `detectar_codigos_error` | `[999.0]` y `[-1.0]`, sin falsos positivos |
| 3 | **Media y mediana** | `media`, `mediana` (sin `np.mean`/`np.median`) y `tabla_posicion` | Contra NumPy; casos par e impar |
| 4 | **Categóricas y gráficos de barras** | `tabla_frecuencias` + DECIDE sobre cardinalidad y codificación inconsistente | Porcentajes suman 100; orden descendente |
| 5 | **Histograma y KDE** | `ancho_freedman_diaconis` + DECIDE sobre bimodalidad y asimetría | Contra `np.histogram_bin_edges(bins="fd")` |
| 6 | **Diagrama de caja desde cero** | `cinco_numeros` (caja, límites, bigotes reales, atípicos) | Contra `matplotlib.cbook.boxplot_stats` |
| 7 | Atípicos y enmascaramiento | `marcar_iqr`, `marcar_zscore` + aplicar IQR al caso del 500 | 199 marcados (5,2 %); cota $(n-1)/\sqrt{n}$ |
| 8 | Correlaciones | `divergencia_pearson_spearman` | `horas_operacion` / `antiguedad_meses`: 0,159 contra 0,863 |
| 9 | Faltantes | DECIDE: mecanismo de cada variable y la fuga de `costo_reparacion_usd` | Tasa por grupo, dada |
| 10 | Informe de hallazgos | La tabla evidencia · criterio · decisión · justificación | — |

**Doce celdas a completar, cinco decisiones metodológicas y un informe.**

### Los gráficos que produce

1. Barras de `planta`, `turno` y `modelo_equipo`, con la cardinalidad de cada una.
2. Histograma de `temperatura_c` con tres anchos de barra (5, Freedman–Diaconis, 200).
3. Densidad por núcleos con tres anchos de banda sobre el histograma FD.
4. **Cuatro diagramas de caja con los puntos superpuestos**, coloreando en naranja los que caen
   fuera de los bigotes. Es la figura central: muestra a la vez qué marca el criterio y qué
   esconde la caja.
5. Diagramas de caja por grupo: vibración por turno y vibración según `fallo`.
6. Dispersión de `horas_operacion` contra `antiguedad_meses`, con y sin los extremos.

### Cierre esperado

Una tabla con, por cada hallazgo: **evidencia · criterio · decisión · justificación**, y tres
consecuencias concretas para el modelado. Es el formato que pide la sección 11 de las notas.

## Dónde se traban (previsto)

| Paso | Error previsible | Cómo se destraba |
|---|---|---|
| 0 | El CSV no está junto al notebook en Colab | La celda de carga abre el diálogo de subida; el docente distribuye el archivo con el notebook |
| 1 | Usar `df.duplicated()` sin excluir el identificador → devuelve 0 | El identificador es único por construcción |
| 2 | Buscar solo `999` porque se mencionó en clase | La función debe **descubrir** el valor, no recibirlo |
| 2 | Usar solo la repetición → falsos positivos | Falta la segunda condición: repetirse **y** estar lejos |
| 3 | Implementar la mediana sin ordenar, o sin distinguir el caso par | La verificación incluye los dos casos de borde |
| 6 | Devolver el límite calculado como bigote | La verificación contra `boxplot_stats` falla exactamente ahí |
| 7 | Comparar `x > lim` con `NaN` presentes | El `assert` de la firma lo detecta |
| 8 | Correr sobre `df` en lugar de `limpio` | Limpiar primero es parte del ejercicio |

## Plan B

`lab_2` no depende de internet: el CSV se distribuye con el notebook. `lab_2_carga_datos` sí
descarga el archivo; si no hay conexión, el docente proyecta la versión solución, que está
ejecutada y con salidas guardadas.

## Sincronización

Cada notebook y su solución se mantienen sincronizados: **si cambia uno, cambia el otro en el
mismo movimiento**. Los dos se generan con los scripts de construcción usados en su creación y
la solución se ejecuta de punta a punta antes de darla por terminada.
