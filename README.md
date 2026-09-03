# Machine Learning — Ingeniería en IA y Ciencia de Datos

Material del curso: laboratorios, conjuntos de datos y series de ejercicios.
Universidad Católica del Uruguay.

> **Este repositorio es de solo lectura.** Acá se descarga el material.
> Las entregas se hacen por **Moodle**, no por GitHub.

---

## Qué hay acá

| Carpeta | Contenido |
|---|---|
| `notebooks/clase_N/` | El laboratorio de cada clase, con las celdas a completar |
| `data/` | Los conjuntos de datos que usan los laboratorios |
| `ejercicios/` | Series de ejercicios en PDF |
| `cronograma_clases.md` | Calendario del curso con fechas de controles y entregas |

---

## Puesta a punto (una sola vez)

### 1. Aceptá la invitación

El repositorio es privado. Vas a recibir un mail de GitHub invitándote a la
organización `ucudal`; hay que aceptarlo para poder clonar. Si no te llegó,
revisá spam y avisale al equipo docente **con tu usuario de GitHub**.

### 2. Cloná el repositorio

```bash
git clone https://github.com/ucudal/Machine-Learning---Ingenieria-en-IA-y-CD.git
cd Machine-Learning---Ingenieria-en-IA-y-CD
```

Si te pide usuario y contraseña, GitHub ya no acepta la contraseña de la cuenta:
usá un [Personal Access Token](https://github.com/settings/tokens) como contraseña,
o configurá [SSH](https://docs.github.com/es/authentication/connecting-to-github-with-ssh).

### 3. Creá el entorno de Python

```bash
python -m venv .venv
source .venv/bin/activate      # en Windows:  .venv\Scripts\activate
pip install numpy pandas matplotlib scikit-learn jupyterlab
```

---

## Cómo trabajar un laboratorio

**Regla única: no trabajes sobre los archivos de este repositorio.** Copiá el
notebook a tu carpeta de trabajo —donde vos quieras, dentro o fuera del
repositorio— y resolvelo ahí.

```bash
git pull                                    # traé lo último
cp notebooks/clase_7/lab_7.ipynb ~/mis-labs/   # o donde prefieras
jupyter lab ~/mis-labs/lab_7.ipynb
```

Los notebooks encuentran los datos solos: la celda `ruta_del_dato()` los busca
junto al notebook, en una carpeta `data/` cercana, y si no están los descarga.
Si preferís tenerlos a mano, copiá también la carpeta `data/` al lado de tu
notebook.

Trabajar sobre una copia es lo que hace que `git pull` nunca te dé un conflicto
ni te pise lo que resolviste.

---

## Actualizar el material

El material se publica clase a clase. Antes de cada laboratorio:

```bash
git pull
```

Si `git pull` te da un error de cambios locales, es porque editaste algún
archivo del repositorio. Para descartar esos cambios y quedarte con la versión
de la cátedra:

```bash
git checkout -- .
git pull
```

Tu trabajo no corre riesgo: está en tu carpeta, fuera del control de git.

---

## Entregas

Por **Moodle**, en la tarea correspondiente a cada laboratorio, subiendo tu
notebook **con las salidas ejecutadas**. Antes de subirlo:
`Kernel → Restart Kernel and Run All Cells`, y verificá que corra de punta a
punta sin errores.

---

## Preguntas

Dudas de contenido, en clase o por el foro de Moodle. Problemas de acceso a
este repositorio, al equipo docente por Moodle indicando tu usuario de GitHub.
