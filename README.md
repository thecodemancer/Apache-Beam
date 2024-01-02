# Apache-Beam

<img src="https://res.cloudinary.com/dxnufruex/image/upload/v1669761065/macrometa-web/images/6172fb248a6212d910d87b73_uLzn5MjM55jF0rj3YNgfPwakSo6-Vbng98ywy7mykWutqOhXP20PJsRfzFJlVg986fFWAjyzTErvoY5g32Vu60ui7Qgea2Qe1ReS3nlZt7czefTQ4QWLnpX1wDqYQznrffSW__08_s1600.png" />

¡Bienvenidos, data practitioners! Durante los próximos meses, nos embarcaremos en un emocionante viaje al mundo de Apache Beam, el framework open source líder para construir e implementar data pipelines tanto en modo batch como en modo streaming utilizando una sola API y ejecutándolo en cualquiera de los runners que dan soporte (Apache Spark, Apache Flink, Google Dataflow, Apache Apex, etc). Ya seas un principiante absoluto o tengas algo de experiencia en programación, encontrarás este contenido fascinante y práctico.

## Lo que aprenderás

- Aprender Apache Beam con su implementación en tiempo real.
- Crear canales de procesamiento de datos empresariales en tiempo real utilizando Apache Beam.
- Aprender un modelo de programación portátil que puedes ejecutar en Spark, Flink, GCP (Google Cloud Dataflow), etc.
- Comprende el funcionamiento de todos y cada uno de los componentes de Apache Beam con ejercicios prácticos.
- Desarrollar pipelines para Big Data del mundo real en diversos dominios comerciales.
- Carga de datos a tablas de Google BigQuery desde data pipelines hechos con Apache Beam.

## Estructura:

Se contará con sesiones semanales, cada una de una hora de duración. Nos centraremos en:

- Características y Métodos: Exploraremos las características y métodos principales del Framework, incluyendo ParDo, DoFn, etc.
- Conceptos: Cubriremos los conceptos clave en Apache Beam, como PCollection, PTransform.
- Aplicaciones: Nos adentraremos en las aplicaciones del mundo real de Apache Beam, con ejemplos para tiendas E-Commerce, banca, telecomunicaciones.

## Prerrequisitos

- [Python-StudyClub Data Engineering Latam 🐍](https://github.com/DataEngineering-LATAM/Python-StudyClub)

## Contenido

### Quickstarts

- [Java Quickstart](https://beam.apache.org/get-started/quickstart-java/) - How to set up and run a WordCount pipeline on the Java SDK.
- [Python Quickstart](https://beam.apache.org/get-started/quickstart-py/) - How to set up and run a WordCount pipeline on the Python SDK.
- [Go Quickstart](https://beam.apache.org/get-started/quickstart-go/) - How to set up and run a WordCount pipeline on the Go SDK.
- [Java Development Environment](https://medium.com/google-cloud/setting-up-a-java-development-environment-for-apache-beam-on-google-cloud-platform-ec0c6c9fbb39) - Setting up a Java development environment for Apache Beam using IntelliJ and Maven.
- [Python Development Environment](https://medium.com/google-cloud/python-development-environments-for-apache-beam-on-google-cloud-platform-b6f276b344df) - Setting up a Python development environment for Apache Beam using PyCharm.

### Introduction

#### Conceptos

##### Runners

###### Overview

Apache Beam proporciona una capa API portátil para crear sofisticados data pipelines de datos en paralelo que pueden ejecutarse en una diversidad de motores de ejecución o ejecutores. Los conceptos centrales de esta capa se basan en el modelo Beam (anteriormente denominado modelo Dataflow) y se implementan en distintos grados en cada runner de Beam.

###### Direct runner

Direct Runner ejecuta data pipelines en tu máquina y está diseñado para validar que los data pipelines se adhieran al modelo Apache Beam lo más fielmente posible. En lugar de centrarse en la ejecución eficiente del data pipeline, Direct Runner realiza comprobaciones adicionales para garantizar que los usuarios no dependan de una semántica que no esté garantizada por el modelo. Algunas de estas comprobaciones incluyen: 

- hacer cumplir la inmutabilidad de los elementos 
- hacer cumplir la codificabilidad de los elementos 
- los elementos se procesan en un orden arbitrario en todos los puntos 
- serialización de las funciones del usuario (DoFn, CombineFn, etc.) 

El uso de Direct Runner para pruebas y desarrollo ayuda a garantizar que los data pipelines sean robustos en diferentes runners de Beam. Además, la depuración de ejecuciones fallidas puede ser una tarea no trivial cuando un data pipeline se ejecuta en un clúster remoto. En cambio, suele ser más rápido y sencillo realizar pruebas unitarias locales en el código de tu data pipeline. La prueba unitaria local de tu data pipeline también permite usar tus herramientas de depuración locales preferidas. En el SDK de Python, el valor predeterminado para un runner es DirectRunner.

**Ejemplo**

```
python -m apache_beam.examples.wordcount --input YOUR_INPUT_FILE --output counts
```

###### Google Cloud Dataflow runner

Este runner utiliza los servicios administrados de Cloud Dataflow. cuando corres tu data pipeline con el servicio de Cloud Dataflow, el runner sube tu código ejecutable y las dependencias a un bucket de Google Cloud Storage  y crea un job de Cloud Dataflow, el cual ejecuta tu pipeline con recursos administrados en Google Cloud Platform. El runner y el servicio de Cloud Dataflow son adecuados para jobs continuos a gran escala y proporcionan:

- un servicio totalmente administrado
- autoescalado del número de workers a través del tiempo de vida del job
- Rebalanceo dinámico de carga

**Ejemplo**

```
# Como parte de la configuración inicial, se instalan los componentes específicos extra de Google Cloud Platform.

pip install apache-beam[gcp]
python -m apache_beam.examples.wordcount --input gs://dataflow-samples/shakespeare/kinglear.txt \
                                         --output gs://YOUR_GCS_BUCKET/counts \
                                         --runner DataflowRunner \
                                         --project YOUR_GCP_PROJECT \
                                         --region YOUR_GCP_REGION \
                                         --temp_location gs://YOUR_GCS_BUCKET/tmp/
```

###### Apache Flink runner

El runner de Apache Flink se puede utilizar para ejecutar data pipelines de Beam utilizando Apache Flink. Para la ejecución, puedes elegir entre un modo de ejecución en clúster (por ejemplo, Yarn/Kubernetes/Mesos) o un modo de ejecución integrado local que es útil para hacer pruebas. El runner de Flink y Apache Flink son adecuados para job continuos a gran escala y proporcionan:

- Un runtime centrado en streaming que admite programas de procesamiento por lotes y en streaming de datos
- Un runtime que admite un rendimiento muy alto y una baja latencia de eventos al mismo tiempo
- Tolerancia a fallos con garantías de procesamiento exactamente una vez
- Contrapresión natural en programas de streaming.
- Gestión de memoria personalizada para una conmutación eficiente y sólida entre algoritmos de procesamiento de datos en memoria y fuera del núcleo
- Integración con YARN y otros componentes del ecosistema Apache Hadoop

**Ejemplo**

1. A partir de Beam 2.18.0, imágenes de Docker preconstruidas del servicio Flink están disponibles en Docker Hub: `Flink 1.10, Flink 1.11, Flink 1.12, Flink 1.13, Flink 1.14.2.`
2. Inicializa el endpoint JobService: `docker run --net=host apache/beam_flink1.10_job_server:latest3`
3. Envía el pipeline al endpoint de arriba utilizando el PortableRunner, configurando el job_endpoint para localhost:8099 (esta es la dirección por defecto del JobService). Opcionalmente setea el environment_type a LOOPBACK. Ejemplo:
   
```
import apache_beam as beam
from apache_beam.options.pipeline_options import PipelineOptions

options = PipelineOptions([
    "--runner=PortableRunner",
    "--job_endpoint=localhost:8099",
    "--environment_type=LOOPBACK"
])
with beam.Pipeline(options) as p:
    ...
```

###### Apache Spark runner

El runner de Apache Spark se puede utilizar para ejecutar data pipelines de Beam usando Apache Spark. El runner de Spark puede ejecutar pipelines de Spark como una aplicación nativa de Spark; desplegando una aplicación autónoma para modo local, corriendo en Spark Standalone, o utilizando YARN o Mesos. El runner de Spark ejecuta pipelines de Beam en Apache Spark, proporcionando:

- Pipelines por lotes y streaming (y combinados).
- Las mismas garantías de tolerancia a fallos que ofrecen los RDD y DStreams.
- Las mismas características de seguridad que proporciona Spark.
- Informes de métricas integradas utilizando el sistema de métricas de Spark, que también informa sobre Beam Aggregators.
- Soporte nativo para side-inputs de Beam a través de las variables Broadcast de Spark.

**Ejemplo**

1. Inicializa el endpoint JobService:
   - con Docker (de preferencia): `docker run --net=host apache/beam_spark_job_server:latest`
   - o del código fuente de Beam: `./gradlew :runners:spark:3:job-server:runShadow`
3. Ejecuta el pipeline en el endpoint de arriba utilizando el PortableRunner, configurando el job_endpoint para localhost:8099 (esta es la dirección por defecto del JobService), y el environment_type seteado en LOOPBACK. Ejemplo:
  
```
import apache_beam as beam
from apache_beam.options.pipeline_options import PipelineOptions

options = PipelineOptions([
    "--runner=PortableRunner",
    "--job_endpoint=localhost:8099",
    "--environment_type=LOOPBACK"
])
with beam.Pipeline(options) as p:
    ...
```

Consola:

```
python -m apache_beam.examples.wordcount --input /path/to/inputfile \
                                         --output /path/to/write/counts \
                                         --runner SparkRunner
```
    
#### Common Transforms

#### Core Transforms

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Windowing

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Triggers

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### IO Connectors

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Splittable doFn

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Cross-Language Transforms

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### [How Beam executes a pipeline](https://beam.apache.org/documentation/runtime/model)

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Pipeline development lifecycle

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Common pipeline patterns

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### AI/ML pipelines

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Runtime systems

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Container environments

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Resource hints

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### SDK Harness Configuration

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### [Beam SQL overview](https://beam.apache.org/documentation/dsls/sql/overview/)

#### Walkthrough

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Shell

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Apache Calcite dialect

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### ZetaSQL dialect

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Beam SQL extensions

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Beam DataFrames overview

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### What is a DataFrame?

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Pre-requisites

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Using DataFrames

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### Embedding DataFrames in a pipeline

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Ejemplos

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Ejercicios

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Preguntas de entrevistas

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Glosario de Términos

Apache Beam is a unified model for defining both batch and streaming data-parallel processing pipelines. To get started with Beam, you’ll need to understand an important set of core concepts:

- Pipeline - A pipeline is a user-constructed graph of transformations that defines the desired data processing operations.
- PCollection - A PCollection is a data set or data stream. The data that a pipeline processes is part of a PCollection.
- PTransform - A PTransform (or transform) represents a data processing operation, or a step, in your pipeline. A transform is applied to zero or more PCollection objects, and produces zero or more PCollection objects.
- Aggregation - Aggregation is computing a value from multiple (1 or more) input elements.
- User-defined function (UDF) - Some Beam operations allow you to run user-defined code as a way to configure the transform.
- Schema - A schema is a language-independent type definition for a PCollection. The schema for a PCollection defines elements of that PCollection as an ordered list of named fields.
- SDK - A language-specific library that lets pipeline authors build transforms, construct their pipelines, and submit them to a runner.
- Runner - A runner runs a Beam pipeline using the capabilities of your chosen data processing engine.
- Window - A PCollection can be subdivided into windows based on the timestamps of the individual elements. Windows enable grouping operations over collections that grow over time by dividing the collection into windows of finite collections.
- Watermark - A watermark is a guess as to when all data in a certain window is expected to have arrived. This is needed because data isn’t always guaranteed to arrive in a pipeline in time order, or to always arrive at predictable intervals.
- Trigger - A trigger determines when to aggregate the results of each window.
- State and timers - Per-key state and timer callbacks are lower level primitives that give you full control over aggregating input collections that grow over time.
- Splittable DoFn - Splittable DoFns let you process elements in a non-monolithic way. You can checkpoint the processing of an element, and the runner can split the remaining work to yield additional parallelism.


## Otros Notebooks


## Libros Recomendados


## Playlists Recomendadas


## Certificación
