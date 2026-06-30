# Procesamiento de imagen mediante Deep Learning

Repositorio asociado al Trabajo de Fin de Grado **“Procesamiento de imagen mediante Deep Learning”** / **“Image processing through Deep Learning”**.

**Autor:** Juan Antonio Coria Gómez
**Grado:** Grado en Física
**Universidad:** Universidad Complutense de Madrid
**Facultad:** Facultad de Ciencias Físicas
**Departamento:** Departamento de Óptica
**Código de TFG:** OPT09
**Supervisores:** Javier Vargas y Tatiana Alieva
**Curso académico:** 2025/26

## Descripción

Este repositorio contiene el código, los datos utilizados, los modelos guardados, las figuras y los resultados generados durante el desarrollo del caso práctico del TFG.

El trabajo estudia el uso de técnicas de aprendizaje profundo en procesamiento de imagen, con especial atención a las redes neuronales convolucionales y a la comparación entre redes densas, redes convolucionales entrenadas desde cero y transferencia de aprendizaje.

La aplicación práctica se divide en dos bloques:

1. **Clasificación categórica de imágenes preprocesadas y normalizadas**, usando Fashion-MNIST.
2. **Clasificación binaria de imágenes crudas y realistas**, usando Oxford-IIIT Pets reducido a la tarea gato/perro.

## Estructura del repositorio

```text
TFG_DL/
│
├── 01_fashion_mnist/
│   ├── data/
│   ├── figures/
│   ├── models/
│   ├── results/
│   └── 1_fashion_classification.mlx
│
├── 02_pets_oxford/
│   ├── data/
│   ├── figures/
│   ├── models/
│   ├── results/
│   └── pets_catdog_classification.mlx
│
└── figuras_memoria/
```

La carpeta `figuras_memoria/` contiene una selección de las figuras incorporadas directamente en la memoria del TFG.

## Experimento 1: Fashion-MNIST

El primer experimento utiliza Fashion-MNIST como banco de pruebas controlado. Las imágenes son de tamaño `28 × 28 × 1`, están centradas, normalizadas y distribuidas en diez clases de prendas.

Este bloque permite estudiar de forma ordenada:

* comparación entre MLP y CNN;
* efecto de la profundidad convolucional;
* regularización mediante dropout y normalización por lotes;
* aumento artificial de datos;
* ajuste de tasa de aprendizaje y tamaño del minilote;
* matrices de confusión;
* robustez frente a ruido y desplazamientos.

Modelos principales:

* **F3:** mejor MLP.
* **F6:** mejor CNN base.
* **F18:** mejor modelo final optimizado.

Resultado principal:

* La CNN base supera a la MLP con muchos menos parámetros.
* El modelo final F18 alcanza el mejor rendimiento en imágenes limpias.
* La robustez depende del tipo de perturbación: las CNN resisten mejor desplazamientos, mientras que la MLP resultó menos sensible al ruido gaussiano en este conjunto concreto.

## Experimento 2: Oxford-IIIT Pets gato/perro

El segundo experimento traslada el problema a imágenes naturales RGB. A diferencia de Fashion-MNIST, Oxford-IIIT Pets contiene variaciones de fondo, iluminación, pose, escala, textura y encuadre.

La tarea se reduce a clasificación binaria:

```text
gato / perro
```

El conjunto utilizado contiene:

* 7349 imágenes utilizables;
* 2371 gatos;
* 4978 perros;
* 5145 imágenes de entrenamiento;
* 1103 imágenes de validación;
* 1101 imágenes de prueba.

Como la clase perro es mayoritaria, además de la precisión global se calculan métricas más informativas:

* precisión balanceada;
* sensibilidad de gato;
* sensibilidad de perro;
* F1 macro;
* matrices de confusión;
* ejemplos representativos de error.

Modelos estudiados:

* MLP como baseline denso.
* CNN propias entrenadas desde cero con distintas resoluciones y profundidades.
* CNN con pesos de clase.
* ResNet-18 entrenada desde cero.
* ResNet-18 preentrenada con capas congeladas.

Modelos principales comparados:

* **CNN7_128:** mejor CNN propia equilibrada.
* **ResNet-18 preentrenada:** transferencia de aprendizaje con extractor congelado.
* **ResNet-18 desde cero:** misma arquitectura residual entrenada sin pesos previos.

Resultado principal:

* La MLP tiende a apoyarse en la clase mayoritaria y apenas reconoce gatos.
* Las CNN propias mejoran claramente la clasificación al conservar la estructura espacial.
* Aumentar resolución y profundidad ayuda hasta cierto punto, pero no garantiza una mejora equilibrada.
* La transferencia de aprendizaje con ResNet-18 preentrenada obtiene el mejor rendimiento final entrenando solo la capa clasificadora.
* ResNet-18 desde cero muestra sobreajuste: ajusta muy bien el entrenamiento, pero generaliza peor al conjunto de prueba.

## Contenido de las carpetas

### `data/`

Contiene los datos utilizados o descargados durante la ejecución de los scripts. En el caso de Oxford-IIIT Pets, la descarga puede tardar varios minutos debido al tamaño del conjunto y al número de imágenes naturales en color.

### `figures/`

Contiene las figuras generadas por MATLAB durante los experimentos:

* ejemplos de imágenes;
* curvas de entrenamiento;
* matrices de confusión;
* gráficos comparativos;
* ejemplos de errores;
* análisis de robustez.

### `models/`

Contiene modelos o variables guardadas durante los experimentos. Estos archivos permiten conservar redes entrenadas o resultados intermedios sin tener que repetir todos los entrenamientos.

### `results/`

Contiene tablas `.csv` y resultados exportados desde MATLAB:

* métricas de entrenamiento, validación y prueba;
* precisión global;
* precisión balanceada;
* sensibilidad por clase;
* F1 macro;
* tiempos de entrenamiento;
* comparaciones entre modelos.

## Requisitos

Los experimentos se han desarrollado en MATLAB / MATLAB Online.

Toolboxes recomendados:

* Deep Learning Toolbox.
* Image Processing Toolbox.
* Statistics and Machine Learning Toolbox, si está disponible.

Para la parte de transferencia de aprendizaje se requiere acceso a una red preentrenada compatible con MATLAB, en este caso ResNet-18.

## Reproducibilidad

Los Live Scripts contienen el flujo completo de trabajo: carga o descarga de datos, preprocesado, entrenamiento, evaluación, generación de figuras y exportación de resultados.

En los experimentos se recalculan las métricas evaluando los modelos sobre los conjuntos completos de entrenamiento, validación y prueba. No se usa el último minilote del entrenamiento como métrica definitiva, ya que puede fluctuar y no representa necesariamente el rendimiento total. 

## Nota sobre tamaño de archivos

Este repositorio incluye carpetas de datos y modelos guardados. Algunas de ellas pueden ocupar bastante espacio. Si GitHub no permite subir algún archivo por límite de tamaño, los Live Scripts permiten regenerar los resultados principales a partir del código y los datos.

## Estado del repositorio

Repositorio asociado a la redacción del TFG **“Procesamiento de imagen mediante Deep Learning”**.

