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

### Model version
175 billion parameter model

### Model version
175 billion parameter model

### Paper & samples
[Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) 

[Release repository containing unconditional, unfiltered samples](https://github.com/openai/gpt-3/blob/master/175b_samples.jsonl) (CONTENT WARNING: GPT-3 was trained on arbitrary data from the web, so samples may contain offensive content and language.)

## Model Use
The intended direct users of GPT-3 are developers who access its capabilities via the OpenAI API. Through the OpenAI API, the model can be used by those who may not have AI development experience to build and explore language modeling systems across a wide range of functions. We also anticipate that the model will continue to be used by researchers to better understand the behaviors, capabilities, biases, and constraints of large-scale language models. 

Given GPT-3’s limitations (described below), and the breadth and open-ended nature of GPT-3’s capabilities, we currently only support controlled access to and use of the model via the OpenAI API. Access and use are subject to OpenAI’s access approval process, API Usage Guidelines, and API Terms of Use, which are designed to prohibit the use of the API in a way that causes societal harm. 

We review all use cases prior to onboarding to the API, review them again before customers move into production, and have systems in place to revoke access if necessary after moving  to production. Additionally, we provide guidance to users on some of the potential safety risks they should attend to and related mitigations.  

## Data, Performance, and Limitations

### Data 
The GPT-3 training dataset is composed of text posted to the internet, or of text uploaded to the internet (e.g., books). The internet data that it has been trained on and evaluated against to date includes: (1) a version of the [CommonCrawl dataset](https://commoncrawl.org/the-data/), filtered based on similarity to high-quality reference corpora, (2) [an expanded version of the Webtext dataset](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf), (3) two internet-based book corpora, and (4) [English-language Wikipedia](https://en.wikipedia.org/wiki/Main_Page). 

Given its training data, GPT-3’s outputs and performance are more representative of internet-connected populations than those steeped in verbal, non-digital culture. The internet-connected population is more representative of developed countries, wealthy, younger, and male views, and is mostly U.S.-centric. Wealthier nations and populations in developed countries show higher internet penetration.<sup>[[1]](#fn1)</sup> The digital gender divide also shows fewer women represented online worldwide.<sup>[[2]](#fn2)</sup> Additionally, because different parts of the world have different levels of internet penetration and access, the dataset underrepresents less connected communities.<sup>[[3]](#fn3)</sup>

### Performance 
GPT-3’s performance has been evaluated on a wide range of datasets in the task categories listed below, with each task evaluated in the few-shot, one-shot, and zero-shot settings. Results on each can be found in the [paper](https://arxiv.org/abs/2005.14165).

* Language Modeling, Cloze, and Completion Tasks
* Closed Book Question Answering
* Translation
* Winograd-Style Tasks
* Common Sense Reasoning Tasks
* Reading Comprehension
* SuperGLUE
* Natural Language Inference
* Synthetic and Qualitative Tasks

Such measures of performance depend on details of the benchmark and therefore won’t be the same as the performance of the model in a deployed system. Ultimately, performance of a deployed system depends on a number of factors, including the technology and how it is configured, the use case for the system, the context in which it is used, how people interact with the system, and how people interpret the system’s output. 

## Limitations
GPT-3 and our analysis of it have a number of limitations. Some of these limitations are inherent to any model with machine learning (ML) components that can have high-bandwidth, open-ended interactions with people (e.g. via natural language): ML components have limited robustness; ML components are biased; open-ended systems have large surface areas for risk; and safety is a moving target for ML systems. GPT-3 has the propensity to generate text that contains falsehoods and expresses them confidently, and, like any model with ML components, it can only be expected to provide reasonable outputs when given inputs similar to the ones present in its training data. In addition to these fundamental limitations, we outline some of the technical limitations evaluated in the [paper](https://arxiv.org/abs/2005.14165) below.

Repetition: GPT-3 samples sometimes repeat themselves semantically at the document level, and can lose coherence over sufficiently long passages, contradict themselves, and occasionally contain non-sequitur sentences or paragraphs. Our [release repository](https://github.com/openai/gpt-3/blob/master/175b_samples.jsonl) contains 500 unconditional, unfiltered 2048 token samples (CONTENT WARNING: GPT-3 was trained on arbitrary data from the web, so samples may contain offensive content and language). 

Lack of world grounding: GPT-3, like other large pretrained language models, is not grounded in other modalities of experience, such as video, real-world physical interaction, or human feedback, and thus lacks a large amount of context about the world.<sup>[[4]](#fn4)</sup> 

Predominantly English: GPT-3 is trained largely on text in the English language, and is best suited for classifying, searching, summarizing, or generating such text. GPT-3 will by default perform worse on inputs that are different from the data distribution it is trained on, including non-English languages as well as specific dialects of English that are not as well-represented in  training data. 

Interpretability & predictability: the capacity to interpret or predict how GPT-3 will behave is very limited, a limitation common to most deep learning systems, especially in models of this scale.

High variance on novel inputs: GPT-3 is not necessarily well-calibrated in its predictions on novel inputs. This can be observed in the much higher variance in its performance as compared to that of humans on standard benchmarks. 

Creation date of training corpora: The May 2020 version of GPT-3 was trained on a dataset created in November 2019, so has not been trained on any data more recent than that. The September 2020 version of the model was retrained to reflect data up to August 2020.

Biases: GPT-3, like all large language models trained on internet corpora, will generate stereotyped or prejudiced content. The model has the propensity to retain and magnify biases it inherited from any part of its training, from the datasets we selected to the training techniques we chose. This is concerning, since model bias could harm people in the relevant groups in different ways by entrenching existing stereotypes and producing demeaning portrayals amongst other potential harms.<sup>[[5]](#fn5)</sup> This issue is of special concern from a societal perspective, and is discussed along with other issues in the [paper](https://arxiv.org/abs/2005.14165) section on Broader Impacts.

## Where to send questions or comments about the model
Please use this [Google Form](https://forms.gle/5R5xU99kAzcpBf1Y9).

-------
 <a name="fn1">[1]</a>  International Telecommunication Union ( ITU ) World Telecommunication/ICT Indicators Database. "Individuals using the Internet (% of population)" https://data.worldbank.org/indicator/IT.NET.USER.ZS?end=2018&start=2002. 
 
 <a name="fn2">[2]</a> Organisation for Economic Co-operation and Development. "Bridging the Digital Divide." http://www.oecd.org/internet/bridging-the-digital-gender-divide.pdf.
 
 <a name="fn3">[3]</a> Telecommunication Development Bureau. "Manual for Measuring ICT Access and Use by Households and Individuals." https://www.itu.int/pub/D-IND-ITCMEAS-2014.
  
 <a name="fn4">[4]</a> Bisk, Yonatan, et al. Experience Grounds Language. arXiv preprint [arXiv:2004.10151](https://arxiv.org/abs/2004.10151), 2020.

 <a name="fn5">[5]</a> Crawford, Kate. [The Trouble with Bias](https://www.youtube.com/watch?v=fMym_BKWQzk). NeurIPS 2017 Keynote, 2017. 
