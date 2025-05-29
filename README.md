# Intramove: Automatización del flujo de datos operativos con AWS Glue DataBrew
## Beneficios estratégicos de implementar un pipeline en Intramove
En este proyecto diseñé y puse en marcha un flujo automatizado de trabajo con datos, también conocido como 'pipeline'. De forma sencilla, se trata de una secuencia de pasos que permite recoger datos brutos, limpiarlos, organizarlos y dejarlos listos para ser analizados sin tener que hacerlo manualmente cada vez.

Este proceso, que funciona en la nube, simula lo que una empresa real de logística como Intramove haría a diario con sus datos de ventas y entregas: recibir archivos, revisarlos, corregirlos si es necesario, calcular métricas importantes y guardarlos preparados para sacar conclusiones y tomar decisiones.

A continuación, resumo los beneficios concretos que este tipo de sistema puede aportar a una empresa de este sector:

| 🧩 **Beneficio clave** | 📈 **Impacto empresarial** |
|-------------------------|-----------------------------|
| Optimización del control logístico diario. Automatiza la limpieza y transformación de los datos de entregas y ventas. | Seguimiento preciso del rendimiento logístico y de la demanda. |
| Reducción de tareas manuales repetitivas. Se programa para ejecutarse automáticamente cuando se suben nuevos datos. | Menos tareas manuales, más tiempo para análisis estratégico. |
| Mejora de la calidad y la fiabilidad del dato gracias a procesos de limpieza y transformación estandarizados, lo que minimiza los errores humanos. | Decisiones basadas en datos íntegros, coherentes y actualizados. |
| Datos siempre listos para el equipo de análisis. Los archivos limpios se almacenan automáticamente en S3 y pueden ser consumidos desde Power BI, Tableau o Python. | Mayor autonomía del área de análisis y reportes más veloces. |
| Ahorro de costes con tecnología cloud escalable. AWS Glue DataBrew y S3 ofrecen un modelo de pago por uso con capa gratuita, ideal para empresas que quieren escalar sin inversiones iniciales elevadas. | Solución profesional sin necesidad de infraestructura propia ni desarrolladores. |
| Integración con toda la arquitectura AWS. Se apoya en servicios como: S3 (almacenamiento), Glue (catálogo), IAM (seguridad). Es fácil integrarlo luego con Redshift, Athena o SageMaker. | Base sólida para una futura analítica avanzada o inteligencia artificial logística. |

## Habilidades demostradas en este proyecto
- Conciencia del impacto en negocio: Foco constante en la automatización y el ahorro de tiempo, reduciendo tareas manuales repetitivas mediante un flujo de trabajo visual, reproducible y escalable. 

- Fundamentos de ingeniería de datos y flujo ETL: Aplicación de un proceso ETL completo en entorno cloud. Comprensión y diseño del flujo: ingesta (Extract), limpieza y transformación (Transform), y exportación estructurada para análisis (Load).

- Automatización y eficiencia de procesos: Configuración de flujos que simulan automatizaciones reales: recepción diaria de archivos brutos, ejecución de trabajos de transformación y generación de salidas listas para análisis.

- Calidad, validación y gobernanza del dato: Aplicación de buenas prácticas en la limpieza y validación: detección de inconsistencias, eliminación de duplicados y transformaciones estandarizadas, asegurando la integridad del dato.​​​​​

- Uso de tecnologías cloud y herramientas visuales: Diseño e implementación de un pipeline completo en AWS, utilizando: Amazon S3 para almacenamiento estructurado, AWS Glue para catalogación automatizada, AWS Glue DataBrew para limpieza y transformación. 

## Propósito y contexto del proyecto

Este proyecto tiene como objetivo diseñar y poner en marcha un flujo automatizado de tratamiento de datos en la nube para IntraMove, una empresa logística ficticia que gestiona entregas y ventas diarias.

En su actividad habitual, esta empresa recibe archivos con información sobre pedidos y entregas, normalmente sin procesar, con formatos inconsistentes o errores. Procesar estos datos manualmente supone un consumo elevado de tiempo y recursos, además de un mayor riesgo de errores humanos.

Para abordar este desafío, se construyó un pipeline de datos visual en AWS que permite:

- Recoger los archivos brutos automáticamente desde almacenamiento en la nube.

- Aplicar transformaciones y limpieza estructurada: cálculo de métricas, corrección de formatos, creación de nuevas variables.

- Generar archivos limpios y organizados, listos para análisis con herramientas de BI o lenguajes como Python.

Esta solución está orientada a la mejora operativa, permitiendo a IntraMove disponer de datos fiables y actualizados sin depender de tareas manuales, y ofreciendo una base sólida para futuros procesos de analítica avanzada.

 ## Desarrollo técnico del pipeline de datos
 A continuación, se detallan las fases desarrolladas durante el proyecto, desde la preparación del entorno hasta la automatización del flujo de datos.
 ### Fases del análisis
1. **Preparación de los datos y entorno cloud**


Simulación de archivos


Se generaron dos archivos .csv representando datos operativos típicos de una empresa logística:
- ventas.csv: contiene información de las ventas realizadas, incluyendo columnas como fecha de venta, producto, cantidad, precio unitario, cliente y almacén de origen.
- logistica.csv: recoge los detalles logísticos asociados a esas ventas, incluyendo la fecha de envío y entrega, el estado de la entrega y el nombre del repartidor.

Subida a Amazon S3


Después, se creó un bucket en Amazon S3, el sistema de almacenamiento en la nube de AWS, para alojar los archivos y organizar el flujo de trabajo:

- Carpeta /raw/: para almacenar los datos en bruto sin transformar.

- Carpeta /processed/: para guardar los archivos finales ya limpios y listos para análisis.

Esta estructura permite separar claramente los datos originales de los resultados del pipeline, siguiendo buenas prácticas de almacenamiento en entornos cloud.

![1 (2)](https://github.com/user-attachments/assets/9fa637ac-3d54-4521-97c3-e255a5c3b38a)


**2. Creación del catálogo de datos con AWS Glue Crawler**

Una vez almacenados los archivos en Amazon S3, el siguiente paso fue automatizar la detección de su estructura para poder trabajar con ellos como si fueran tablas en una base de datos. Para ello, se utilizó AWS Glue Crawler, una herramienta que permite escanear datos en S3 y catalogarlos automáticamente.


Configuración del crawler


Se creó un crawler que apunta a la ruta /raw/ del bucket, donde se encuentran los archivos originales. Este crawler:

- Detecta de forma automática las columnas, tipos de datos y formato de los archivos CSV.

- Genera tablas estructuradas en el Glue Data Catalog, el catálogo central de datos de AWS.

- Permite que herramientas como AWS DataBrew o Amazon Athena trabajen con los datos sin necesidad de prepararlos manualmente.

​

Resultado: tablas estructuradas listas para usar


​
Tras ejecutar el crawler, se generaron dos tablas en una base de datos Glue llamada intramove_db:

- ventas_csv: representa la tabla estructurada del archivo de ventas.

- logistica_csv: representa la tabla de entregas logísticas.

Gracias a este paso, los archivos ya no son simplemente documentos sueltos en un sistema de archivos, sino datasets estructurados y gestionables desde el ecosistema cloud de AWS, preparados para ser transformados y analizados.

​![2](https://github.com/user-attachments/assets/934a1688-7749-483f-adb6-55e2d0cbaa2c)


**3. Limpieza y transformación visual con AWS Glue DataBrew**

Una vez catalogados los datos, se procedió a la fase central del pipeline: la limpieza, validación y transformación visual de los datasets usando AWS Glue DataBrew. Esta herramienta permite aplicar reglas de calidad y transformaciones complejas de forma visual, sin necesidad de escribir código, ideal para flujos ETL accesibles y replicables.

Proyecto 1: Transformación de datos de ventas
​
Se creó un proyecto en DataBrew basado en la tabla ventas_csv. A partir de esta fuente, se aplicaron varias transformaciones clave para preparar los datos para análisis comercial:

- Revisión y corrección de tipos de datos (fechas, numéricos, texto).

- Creación de la columna total_venta: resultado de multiplicar la cantidad por el precio unitario.

- Extracción de componentes de la fecha (año, mes) para permitir análisis temporales.

- Verificación de calidad: detección de duplicados y validación de consistencia en las columnas clave.

Estas transformaciones permiten construir KPIs como ingresos mensuales, ventas por producto o rendimiento por almacén.

Proyecto 2: Transformación de datos logísticos
​
En paralelo, se creó un segundo proyecto sobre la tabla logistica_csv, centrado en el análisis de la eficiencia de las entregas. Transformaciones aplicadas:

- Cálculo del tiempo de entrega (tiempo_entrega_dias) mediante la diferencia entre fecha_entrega y fecha_envio.

- Creación de la columna entrega_clasificada, que indica si la entrega fue “A tiempo” o “Retrasada” según el tiempo calculado.

- Verificación de formatos y limpieza de inconsistencias.

Gracias a estas transformaciones, se pueden analizar métricas como la puntualidad logística o la eficiencia por repartidor o ruta.

![3](https://github.com/user-attachments/assets/7495db43-d45b-43b1-ac56-10beadcf03a0)


**4: Exportación del dataset limpio a S3**

Una vez aplicadas todas las transformaciones en AWS Glue DataBrew, se configuraron trabajos (jobs) para ejecutar las recetas completas sobre los datasets originales y exportar los resultados transformados al almacenamiento en la nube.
​
Creación de los trabajos en DataBrew
​
Se crearon dos trabajos independientes, uno para cada proyecto:

- Job de ventas: aplicó todas las transformaciones del proyecto ventas_csv y generó un archivo limpio con métricas clave como total_venta, año, mes.

- Job de logística: ejecutó la receta con el cálculo de tiempo_entrega_dias y la clasificación de entregas como “A tiempo” o “Retrasadas”.

Cada job tomó los datos originales desde /raw/, aplicó las transformaciones configuradas y generó automáticamente un nuevo archivo limpio.

![4](https://github.com/user-attachments/assets/0bb95e84-cb8f-4afa-bb65-032fa611eb76)


**5. Automatización del pipeline**

Una vez validadas y exportadas las transformaciones de los datos, se completó el desarrollo del pipeline configurando su automatización en la nube, lo que permite a IntraMove mantener un flujo continuo y sin intervención manual para procesar sus datos operativos.

Automatización implementada
​
El flujo fue automatizado mediante la configuración de tareas programadas en AWS, lo que garantiza que el sistema procese nuevos archivos de forma periódica y ordenada. El proceso automatizado sigue los siguientes pasos:

1. Carga periódica de nuevos archivos a S3
Los nuevos datos de ventas y entregas se cargan regularmente en la carpeta /raw/ del bucket de Amazon S3. Esta ubicación funciona como punto de entrada del pipeline, centralizando todos los datos sin procesar.

2. Ejecución programada del crawler
​
Se configuró el crawler de AWS Glue para ejecutarse automáticamente. Este se encarga de:

- Escanear la carpeta /raw/.

- Detectar nuevos archivos incorporados.

- Actualizar las tablas del Glue Data Catalog para mantenerlas sincronizadas con los datos más recientes.


3. Lanzamiento automático del job de transformación
Tras la ejecución del crawler, se activa automáticamente el job de AWS Glue DataBrew que aplica las recetas de limpieza y transformación a los nuevos datos detectados.

4. Generación y almacenamiento del dataset limpio
​
Una vez transformados, los datos limpios se exportan automáticamente a la carpeta /processed/ del bucket S3, manteniendo la estructura organizada y separada por área (ventas y logística). Estos archivos están listos para ser consumidos por herramientas de análisis como Power BI o integrarse en otros sistemas de reporting.

![ultsi](https://github.com/user-attachments/assets/ace384cd-13ca-4a21-a04a-4bd3387bd969)


**6. Preparado para escalar**

​El pipeline está diseñado para crecer y adaptarse a nuevas necesidades. Su arquitectura permite:
​
- Añadir nuevas fuentes de datos.

- Incorporar otras capas de validación o modelos predictivos.

- Integrarse con consultas SQL en la nube mediante Amazon Athena.

## Reflexiones finales y aprendizajes

Este proyecto ha sido una oportunidad para simular una solución de datos completa y aplicable a un entorno empresarial real. La experiencia ha demostrado cómo un flujo de datos bien estructurado y automatizado puede convertirse en un activo estratégico para cualquier empresa.

Desde el primer paso —la preparación y catalogación de los datos— hasta la automatización del pipeline, el enfoque se ha mantenido firme en resolver una necesidad operativa concreta: transformar de manera automatizada datos dispersos y en bruto en información confiable, actualizada y lista para análisis.

Valor aportado al negocio:
- Reducción drástica del trabajo manual, al eliminar tareas repetitivas de limpieza y validación.

- Agilidad y rapidez en la toma de decisiones, al contar con datos actualizados automáticamente.

- Mayor control logístico, gracias a la capacidad de medir el tiempo de entrega, la puntualidad y el rendimiento de las operaciones.

- Sostenibilidad técnica, al construir una arquitectura escalable en la nube que no depende de programación compleja ni recursos técnicos avanzados.​

​

Aprendizajes personales

​
Como analista de datos en formación, este proyecto me ha permitido afianzar conocimientos clave relacionados con la ingeniería de datos y la analítica aplicada al negocio, entre ellos:

- Diseñar un flujo ETL completo de forma visual y estructurada.

- Aplicar buenas prácticas de calidad y validación del dato.

- Utilizar servicios cloud como S3, Glue y DataBrew de forma coordinada.

- Pensar como analista pero también como empresa: cómo ahorrar tiempo, minimizar errores y aumentar el valor de los datos.


## Contacto









​​
