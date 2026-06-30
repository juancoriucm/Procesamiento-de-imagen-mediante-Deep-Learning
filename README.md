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

Este repositorio contiene el material complementario generado durante el desarrollo del caso práctico del TFG: código MATLAB, datos utilizados, modelos guardados, tablas de resultados y figuras.

El trabajo estudia el uso de técnicas de aprendizaje profundo en procesamiento de imagen, con especial atención a las redes neuronales convolucionales. La aplicación práctica se organiza como una progresión entre dos escenarios de clasificación:

1. **Clasificación categórica de imágenes preprocesadas y normalizadas.**
2. **Clasificación binaria de imágenes crudas y realistas.**

El primer bloque utiliza un conjunto controlado, Fashion-MNIST, para estudiar de forma sistemática el efecto de la arquitectura, la regularización, el aumento artificial de datos y los parámetros de aprendizaje. El segundo bloque traslada la comparación a Oxford-IIIT Pets, donde las imágenes son RGB, tienen resolución variable y presentan mayor variabilidad de pose, escala, iluminación, textura y fondo.

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

La carpeta `figuras_memoria/` contiene una selección de las figuras utilizadas directamente en la memoria del TFG.

## 1. Clasificación categórica de imágenes preprocesadas y normalizadas

El primer bloque práctico se realizó sobre Fashion-MNIST. Este conjunto contiene imágenes en escala de grises de tamaño `28 × 28 × 1`, centradas y normalizadas, distribuidas en diez clases de prendas.

Este escenario se utilizó como sistema controlado para estudiar:

* comparación entre redes densas MLP y redes convolucionales CNN;
* efecto de la profundidad convolucional;
* número de parámetros entrenables;
* regularización mediante abandono aleatorio y normalización por lotes;
* aumento artificial de datos;
* ajuste de tasa de aprendizaje y tamaño del minilote;
* matrices de confusión y análisis de errores;
* robustez frente a ruido gaussiano y desplazamientos espaciales.

Modelos principales de este bloque:

* **F3:** mejor MLP.
* **F6:** mejor CNN base.
* **F18:** modelo final optimizado.

La conclusión principal de este bloque es que la CNN aprovecha mejor la estructura espacial de las imágenes que la MLP. La mejor CNN base supera a la mejor MLP con menos parámetros, y el modelo final optimizado alcanza el mejor rendimiento sobre imágenes limpias. El análisis de robustez muestra además que la ventaja de cada arquitectura depende del tipo de perturbación: las CNN toleran mejor los desplazamientos, mientras que la MLP resultó menos sensible al ruido gaussiano en este conjunto concreto.

## 2. Clasificación binaria de imágenes crudas y realistas

El segundo bloque práctico se realizó sobre Oxford-IIIT Pets, reducido a una tarea binaria:

```text
gato / perro
```

A diferencia de Fashion-MNIST, este conjunto contiene imágenes naturales RGB con fondo, escala, iluminación, pose y textura variables. El objetivo no fue clasificar razas, sino estudiar cómo cambian las conclusiones del bloque anterior cuando el problema deja de estar preprocesado y controlado.

El conjunto utilizado contiene:

* 7349 imágenes utilizables;
* 2371 gatos;
* 4978 perros;
* 5145 imágenes de entrenamiento;
* 1103 imágenes de validación;
* 1101 imágenes de prueba.

Debido al desbalance entre clases, no se usó únicamente la precisión global. También se calcularon:

* precisión balanceada;
* sensibilidad de gato;
* sensibilidad de perro;
* F1 macro;
* matrices de confusión;
* ejemplos representativos de error.

En este bloque se estudiaron:

* MLP como referencia densa;
* CNN propias entrenadas desde cero;
* efecto de la resolución de entrada;
* efecto de la profundidad convolucional;
* uso de pesos de clase;
* ResNet-18 entrenada desde cero;
* ResNet-18 preentrenada con capas congeladas.

Modelos principales de este bloque:

* **CNN7_128:** mejor CNN propia equilibrada.
* **ResNet-18 preentrenada:** transferencia de aprendizaje con extractor congelado.
* **ResNet-18 desde cero:** arquitectura residual entrenada sin pesos previos.

La conclusión principal es que las CNN propias mejoran claramente el comportamiento de la MLP, pero la transferencia de aprendizaje produce el mejor resultado global. ResNet-18 preentrenada aprovecha una representación visual previa y solo ajusta la capa final al problema gato/perro. En cambio, ResNet-18 entrenada desde cero tiene muchos más parámetros entrenables y muestra sobreajuste: ajusta bien el entrenamiento, pero generaliza peor al conjunto de prueba.

## Contenido de las carpetas

### `data/`

Contiene los datos utilizados o descargados durante la ejecución de los scripts. En Oxford-IIIT Pets la descarga puede tardar varios minutos debido al tamaño del conjunto y al número de imágenes naturales en color.

### `figures/`

Contiene las figuras generadas por MATLAB durante los bloques prácticos:

* ejemplos de imágenes;
* curvas de entrenamiento;
* gráficos comparativos;
* matrices de confusión;
* ejemplos de errores;
* análisis de robustez.

### `models/`

Contiene modelos o variables guardadas durante el desarrollo. Estos archivos permiten conservar redes entrenadas o resultados intermedios sin repetir todos los entrenamientos.

### `results/`

Contiene tablas `.csv` y resultados exportados desde MATLAB:

* métricas de entrenamiento, validación y prueba;
* precisión global;
* precisión balanceada;
* sensibilidad por clase;
* F1 macro;
* tiempos de entrenamiento;
* comparaciones entre modelos.

### `figuras_memoria/`

Contiene una selección de figuras ya preparadas para su incorporación en la memoria.

## Requisitos

Los scripts se desarrollaron en MATLAB / MATLAB Online.

Toolboxes recomendados:

* Deep Learning Toolbox.
* Image Processing Toolbox.
* Statistics and Machine Learning Toolbox, si está disponible.

Para la parte de transferencia de aprendizaje se requiere acceso a una red preentrenada compatible con MATLAB, en este caso ResNet-18.

## Reproducibilidad

Los Live Scripts contienen el flujo de trabajo empleado en el TFG: carga o descarga de datos, preprocesado, entrenamiento, evaluación, generación de figuras y exportación de resultados.

En los modelos se recalculan las métricas evaluando sobre los conjuntos completos de entrenamiento, validación y prueba. No se utiliza el último minilote registrado durante el entrenamiento como métrica definitiva, ya que puede fluctuar y no representar el rendimiento total del modelo.

## Nota sobre tamaño de archivos

Este repositorio incluye carpetas de datos y modelos guardados. Algunas de ellas pueden ocupar bastante espacio. Si GitHub no permite subir algún archivo por límite de tamaño, los Live Scripts permiten regenerar los resultados principales a partir del código y los datos.

## Estado del repositorio

Repositorio asociado a la redacción del TFG **“Procesamiento de imagen mediante Deep Learning”**.


