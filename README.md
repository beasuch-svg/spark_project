# Computacion Distribuida con PySpark

Material practico de la asignatura de Computacion Distribuida del Master en
Ingenieria de Datos, Arquitectura Cloud y MLOps de PontIA.

El objetivo no es memorizar la API de PySpark. En cada notebook seguimos la
misma dinamica:

> Predecir -> ejecutar -> observar -> explicar.

## Requisitos

- Visual Studio Code o JupyterLab.
- Java 17 o Java 21 disponible en el `PATH`.
- `uv` instalado.
- Conexion a Internet durante la primera instalacion.

Comprueba prerequisitos con:

```bash
java -version
uv --version
```

## Preparar el proyecto

Desde la raiz del proyecto:

```bash
uv init
uv venv --python 3.15 .venv
uv add pyspark jupyterlab ipykernel
uv lock
uv sync
```

Registrar kernel para Jupyter:

```bash
uv run python -m ipykernel install --user --name spark-project --display-name "Python (.venv) spark-project"
```

Para abrir notebooks:

```bash
uv run jupyter lab
```

Los notebooks estan en la carpeta `notebooks/` y los datos locales en `data/`.

En VS Code, abre la carpeta completa y selecciona como kernel el interprete de
`.venv` (o el kernel `Python (.venv) spark-project`).

## Diagnostico rapido

### Error: Java no encontrado

Sintoma:

```text
Unable to locate a Java Runtime
```

Comprobaciones:

```bash
java -version
echo $JAVA_HOME
```

Resolucion:
- Instala Java 17 o 21 (JDK recomendado).
- Si `java` existe pero Spark no lo detecta, exporta `JAVA_HOME` y reinicia terminal:

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH="$JAVA_HOME/bin:$PATH"
```

### Error: `ModuleNotFoundError: No module named 'pyspark'`

Comprobaciones:

```bash
uv run python -c "import pyspark; print(pyspark.__version__)"
```

Resolucion:

```bash
uv sync
```

### Error: notebook usa un kernel distinto

Comprobaciones:
- Verifica en Jupyter/VS Code que el kernel activo es el de `.venv`.
- Ejecuta:

```bash
uv run python -c "import sys; print(sys.executable)"
```

La ruta debe apuntar a `.venv/bin/python`.

## Recorrido de notebooks

1. `notebooks/00_entorno_y_primer_pipeline.ipynb`: SparkSession, modo local, DataFrames,
   particiones, transformaciones, acciones y lazy evaluation.
2. `notebooks/01_pyspark_ introduccion.ipynb`: fundamentos de DataFrames para perfil
   analista (seleccion, filtros, agregaciones, joins y transformaciones comunes).
3. `notebooks/02_modelo_de_ejecucion.ipynb`: plan logico, plan fisico, jobs, stages y
   tasks.
4. `notebooks/03_dataframes_esquemas_y_sql.ipynb`: esquemas explicitos, expresiones y SQL.
5. `notebooks/04_particiones_y_archivos.ipynb`: repartition, coalesce, CSV, Parquet y
   small files.
6. `notebooks/05_shuffle_joins_y_skew.ipynb`: operaciones wide, estrategias de join y
   datos desbalanceados.
7. `notebooks/06_diagnostico_y_optimizacion.ipynb`: explain, Spark UI y mejora de un
   pipeline deliberadamente ineficiente.
8. `notebooks/07_caso_final_integrado.ipynb`: caso integrado para consolidar conceptos.

Los notebooks se construiran de forma incremental utilizando un mismo caso de
datos para que cada concepto nuevo se apoye en el anterior.
