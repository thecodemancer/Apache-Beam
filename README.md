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


### Beginners

#### Quickstarts

- [Java Quickstart](https://beam.apache.org/get-started/quickstart-java/) - How to set up and run a WordCount pipeline on the Java SDK.
- [Python Quickstart](https://beam.apache.org/get-started/quickstart-py/) - How to set up and run a WordCount pipeline on the Python SDK.
- [Go Quickstart](https://beam.apache.org/get-started/quickstart-go/) - How to set up and run a WordCount pipeline on the Go SDK.
- [Java Development Environment](https://medium.com/google-cloud/setting-up-a-java-development-environment-for-apache-beam-on-google-cloud-platform-ec0c6c9fbb39) - Setting up a Java development environment for Apache Beam using IntelliJ and Maven.
- [Python Development Environment](https://medium.com/google-cloud/python-development-environments-for-apache-beam-on-google-cloud-platform-b6f276b344df) - Setting up a Python development environment for Apache Beam using PyCharm.

#### Introduction

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

Este runner utiliza los servicios administrados de Cloud Dataflow. cuando corres tu data pipeline con el servicio de Cloud Dataflow, el runner sube tu código ejecutable y las dependencias a un bucket de Google Cloud Storage  y crea un job de Cloud Dataflow, el cual ejecuta tu pipeline con recursos administrados en Google Cloud Platform. El runner y el servicio de Cloud Dataflow son adecuados para jobs continuos a gran escala y proveen:

- un servicio totalmente administrado
- autoescalado del número de workers a través del tiempo de vida del job
- Rebalanceo dinámico de carga

#### Common Transforms

#### Core Transforms


#### Windowing


#### Triggers

#### IO Connectors

#### Splittable doFn

#### Cross-Language Transforms

#### Ejemplos

### Ejercicios

## Glosario de Términos


## Otros Notebooks


## Libros Recomendados


## Playlists Recomendadas


## Certificación
