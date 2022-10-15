# Generar Deepfake con Faceswap

Last updated: Octubre 2022

## Introducción

Deepfake proviene de las palabras aprendizaje profundo y falsificación, es un medio que una persona en una imagen o video existente y reemplazada con la imagen de otra persona. Deepfakes hace uso de poderosas técnicas de aprendizaje automático e inteligencia artificial para manipular y generar contenido visual y de audio con un alto potencial de engaño. El mecanismo  para la creación de deepfakes son los modelos de aprendizaje profundo, como los codificadores automáticos y las redes generativas antagónicas (GAN). Estos modelos se utilizan para examinar las expresiones faciales y los movimientos de una persona y sintetizar imágenes faciales de otra persona realizando expresiones y movimientos análogos.

Deepfakes ha atraído una gran atención por su uso en videos pornográficos de celebridades, pornografía de venganza, noticias falsas, engaños y fraude financiero. Esto ha provocado respuestas tanto de la industria como del gobierno para detectar y limitar su uso. Este documento explorará qué son los deepfakes, qué algoritmos de aprendizaje automático se utilizan para crear una animación deepfake y cómo se pueden ejecutar.

### Redes Neuronales
Las redes neuronales son sistemas computacionales inspirados por la forma en que el cerebro procesa la información. Células especiales llamadas neuronas están conectadas entre sí en una red densa, lo que permite que la información sea procesada y transmitida. En informática, las redes neuronales artificiales están formadas por miles de nodos, conectados de forma específica. Estos nodos normalmente se organizan en capas; la forma en que están conectados determina el tipo de red y, en última instancia, su capacidad para realizar una determinada tarea computacional sobre otra.

![Redes Neuronales](https://drive.google.com/uc?id=1nhDO_BYIsyT163bpALey0DeUkU3gs5BL)

Cada nodo o neurona en la *capa de entrada* contiene un valor numérico que codifica la entrada que se enviará a la red.

Por ejemplo, si la red intenta predecir el clima, los nodos de entrada pueden contener la presión, la temperatura, la humedad y la velocidad del viento codificados como números en el rango [-1, +1]. Estos valores luego se transmiten a la siguiente capa; cada arista amortigua o amplifica los valores que transmite. Cada nodo suma todos los valores que recibe y genera uno nuevo basado en su propia función. El resultado del cálculo se puede recuperar de la capa de salida; en este caso, sólo se produce un valor.

#### Deep Learning con Imagenes

Cuando las imágenes son la entrada (o salida) de una red neuronal, normalmente la red tiene tres nodos de entrada para cada píxel, inicializados con la cantidad de rojo, verde y azul que contiene. La arquitectura más efectiva para aplicaciones basadas en imágenes hasta el momento son las redes neuronales convolucionales (CNN), que es exactamente lo que aprovechan los modelos deepfake.

Entrenar una red neuronal significa encontrar un conjunto de pesos para todos los bordes, de modo que la capa de salida produzca el resultado deseado. Una de las técnicas más utilizadas para lograr esto se llama backpropagation, y funciona reajustando los pesos cada vez que la red comete un error.

La idea básica detrás de la detección de rostros y la generación de imágenes es que cada capa representará progresivamente funciones complejas centrales. En el caso de una cara, por ejemplo, la primera capa podría detectar bordes, las características de la segunda cara, que la tercera capa puede usar para detectar imágenes.

![Deep Learning](https://drive.google.com/uc?id=1Jem-FWW-yD7NR7gRWc_eyJwdlhaCDAu7)


## Funcionamiento Faceswap

### Extracción
Deepfakes aprovecha las redes neuronales para transformar rostros y requiere grandes cantidades de datos (imágenes) para que todo funcione sin problemas y sea creíble. El proceso de extracción se refiere al paso de extraer todos los fotogramas de los videoclips, identificar las caras y alinearlas para optimizar el rendimiento.

![Extraccion](https://drive.google.com/uc?id=1-PR60_fm-gF58AIkB3cUb-rH2osgiVxa)

En alto nivel, la extracción consta de tres fases: detección, alineación y generación de máscaras:

- Detección: Proceso de encontrar caras dentro de un marco. El detector escanea una imagen y selecciona áreas de la imagen que cree que son rostros.
- Alineación: Encontrar los "puntos de referencia" dentro de una cara y orientar las caras de manera consistente. Esto toma a los candidatos del detector e intenta determinar dónde están las características clave (ojos, nariz, etc.) en la cara potencial. Luego intenta usar esta información para alinear la cara.
- Generación de máscaras: Identifica las partes de una cara alineada que contiene una cara y bloquea aquellas áreas que contienen fondo/obstrucciones.

El perfilamiento es un paso crítico ya que la red neuronal que realiza el intercambio de caras requiere que todas las caras tengan el mismo tamaño (generalmente 256 x 256) y características alineadas. La detección y alineación de rostros es un problema que se considera resuelto en su mayoría, y la mayoría de las aplicaciones lo realizan de manera muy eficiente (detección de rostros).

El proceso de perfilamiento extrae toda información sobre todas las caras que encuentra en cada cuadro, específicamente dónde está la cara:
![Perfilamiento](https://miro.medium.com/max/828/1*96UT-D8uSXjlnyvs9DZTog.png)


### Entrenamiento

El proceso de entrenamiento permite que la red neuronal convierta una cara en otra. El entrenamiento puede durar varias horas o incluso días, según el tamaño del conjunto de entrenamiento y el dispositivo en el que se entrena el modelo. Al igual que el entrenamiento de la mayoría de las otras redes neuronales, el entrenamiento solo debe completarse una vez. Una vez que el modelo esté entrenado, podrá convertir una cara de la persona A la B.

![trainface](https://drive.google.com/uc?id=1VekvM6foF0Xjzk4LP7uCV5hGlVYUj5Zs)

La mayoría de los modelos se componen en gran parte de 2 partes:

- Codificador: Tiene la función de tomar un montón de rostros como entrada y "codificarlos" en una representación en forma de "vector". Es importante tener en cuenta que no está aprendiendo una representación exacta de cada rostro que ingresa, sino que está tratando de crear un algoritmo que pueda usarse para reconstruir rostros más tarde lo más cerca posible de las imágenes de entrada.

- Decodificador: Tiene el trabajo de tomar los vectores creados por el codificador e intentar convertir esta representación nuevamente en caras, lo más cerca posible de las imágenes de entrada.
 
![train](https://forum.faceswap.dev/download/file.php?id=138)

La red necesita saber qué tan bien está codificando y decodificando rostros. Utiliza 2 herramientas principales para hacer esto:

- Pérdida: Por cada lote de caras ingresadas en el modelo, la red observará la cara que ha intentado recrear mediante su algoritmo de codificación y decodificación actual y la comparará con la cara real que se introdujo. Basado en lo bien que lo hizo otorgará a sí mismo una puntuación (el valor de pérdida) y actualizará sus ponderaciones en consecuencia.

- Pesos: Una vez que el modelo ha evaluado qué tan bien ha recreado una cara, actualiza sus pesos. Estos alimentan los algoritmos del codificador/descodificador. Si ha ajustado sus pesos en una dirección, pero siente que ha hecho un peor trabajo de reconstrucción de la cara que antes, entonces sabe que los pesos se están moviendo en la dirección equivocada, por lo que los ajustará en la otra dirección. Si siente que ha mejorado, entonces sabe que debe seguir ajustando los pesos en la dirección en la que va.

![perdida](https://forum.faceswap.dev/download/file.php?id=139)

Luego, el modelo repite esta acción muchas, muchas veces, actualizando constantemente sus pesos en función de sus valores de pérdida, mejorando teóricamente con el tiempo, hasta que llega a un punto en el que siente que ha aprendido lo suficiente como para recrear una cara de manera efectiva, o los valores de pérdida dejan de caer.

Para reconstruir la cara se hace lo siguiente:
- Compartir el codificador para el conjunto A y B. De esta forma, nuestro codificador está aprendiendo un solo algoritmo para 2 personas diferentes.
- Cuando se trata de intercambiar finalmente las caras, cambian los decodificadores, por lo tanto se alimenta el modelo de la Cara A, pero páselo a través del decodificador B

![decodificador](https://forum.faceswap.dev/download/file.php?id=141)


### Creación Video Deepfake

Una vez que se entrena el modelo, se puede crear un deepfake. A partir de un video, se extraen fotogramas y se alinean todas las caras. Luego, cada cuadro se convierte utilizando la red neuronal entrenada. El paso final es fusionar la cara convertida de nuevo al marco original.

![convert1](https://drive.google.com/uc?id=17l_Q1w3mDqmqfmBDDVOVA4PkYs2UwSAK)

El proceso de creación no utiliza ningún algoritmo de aprendizaje automático. El proceso consiste en volver a pegar una cara en una imagen que está codificado y, por lo tanto, carece de la capacidad de detectar errores.

![fusion](https://drive.google.com/uc?id=1asptwgEiDOWxclpsQgwQf2bb5w6qKPDy)


## Datos Entrenamiento
Para la extración de las caras se usaron 2 videos con rostos de Tony Stark y Elon Musk, los videos se encuentran disponibles en:

[Repositorio Videos Extracción](https://github.com/christiancgil/DFLWorkspace)

## Extración de Caras
Después de realizar el proceso de extración, se obtuvo:
- Video Stark - 654 Caras
- Video Musk - 1467 Caras

## Entrenamiento del Modelo
El proceso de entrenamiento se realizó con las siguientes condiciones :
- [Algoritmo Original](https://github.com/deepfakes/faceswap/blob/master/plugins/train/model/original.py)
- 20 horas de Entrenamiento
- 240.000 repeticiones

## Resultados del Entrenamiento
Tras las 20 horas de entrenamiento se identificaron los siguientes resultados:
- Una perdida de 0.01920 para la cara de Tony Stark
- Una perdida de 0.00708 para la cara de Elon Musk

## Generación de Deepfake con faceswap en Colab
### Demostración
Por medio de este archivo se puede generar el video deepfake usando el modelo ya entrenado

[Faceswap Demo Deepfake](https://colab.research.google.com/drive/1axquw4TSbWnpMscUUmmk-tQcgo17rl-x?usp=sharing)

### Mediante Entrenamiento del Modelo
Por medio de este archivo se puede generar el video deepfake usando los modulos de faceswap para entrenar el modelo

[Faceswap Entrenamiento Deepfake](https://colab.research.google.com/drive/102ET6rsimMa-5QcCpetZNtU1aFlDftxf?usp=sharing)

### Video Deepfake Generado
El video resultante del proceso se puede descargar del siguiente enlace:

[facewap Video Deepfake](https://drive.google.com/file/d/1f-b6Yvq2-XGPJYhLTwnGg2bYKjXqBK39/view?usp=sharing)

## Generación del Model Card para Modelo Faceswap
[Model Card](https://colab.research.google.com/drive/1fOiEoYVYQSJ9RL9tEDCxrUqO75t89jI8?usp=sharing)

## Referencias

-------
 <a name="fn1">[1]</a>  Deep Learning for Deepfakes Creation and Detection."https://www.researchgate.net/profile/Thanh-Nguyen-267/publication/336058980_Deep_Learning_for_Deepfakes_Creation_and_Detection_A_Survey/links/5d8c932092851c33e93cc598/Deep-Learning-for-Deepfakes-Creation-and-Detection-A-Survey.pdf. 
 
 <a name="fn2">[2]</a> Model Cards for Model Reporting." https://universepg.com/public/storage/journal-pdf/A%20qualitative%20survey%20on%20deep%20learning%20based%20deep%20fake%20video%20creation.pdf.
   
 <a name="fn3">[3]</a> Main Page faceswap."https://faceswap.dev/#content.
  
 <a name="fn4">[4]</a> First Order Motion Model for Image Animation and Deep Fake Detection: Using Deep Learning. https://ieeexplore.ieee.org/document/9740969/references#references
