+++
title = "Sketch2shiba"
date = 2018-11-09T11:18:37-07:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Ricardo Holguin Esquer"]

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["Deep learning", "CNN", "Machine learning", "dogs", "IA", "Artificial inteligence"]
categories = ["Deep learning"]
summary = "Red GAN que a partir de un bosquejo de un perro, esta genera una imagen de un perro shiba inu."
# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
[image]
  # Caption (optional)
  caption = "Prueba"

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = "Smart"
+++
## Antesedenctes

Actualmente curso la materia de redes neuronales impartida por el profesor [Julio Waissman](http://mat.uson.mx/~juliowaissman/), donde para proyecto de mitad de curso teníamos que hacer alguna red neuronal convolucional (CNN). El profesor nos dio la idea de basarnos en algún proyecto o red ya hecha y aplicarla a otra idea que tengamos. También nos mostró algunos artículo o proyectos de [Github](https://github.com/) que nos pudieran ayudar de inspiración, pero ninguno de los ejemplo que nos mostró me llamó mucho la atención (porque me parecían aburridos o algún otro compañero tomó algún proyecto que también yo quería). Entonces decidí buscar otros proyectos en internet, y fue un poco después de cuando decidí buscar que me encontré con una página de un [curso de Stanford de Redes convolucionales para reconocimiento visual](http://cs231n.stanford.edu/project.html) donde en dicho curso el profesor les dejó como proyecto algo similar a lo que nos habían dejado a nosotros, y al igual que nuestro maestro, también dejó material que podría servir de inspiración y con ello habían proyectos de generaciones pasadas del curso, articulos, proyectos de github, entre otros. La cantidad de diferentes proyectos que había en esta página eran demasiados, pero entre ellos encontré un proyecto de [coloración automática de bosquejos](http://cs231n.stanford.edu/reports/2017/pdfs/403.pdf) donde, en resumen, su red lo que hacía era que a base de un bosquejo o dibujo de algún paisaje, la red colorea el dibujo. Este proyecto se basó en un modelo de red neuronal llamado [pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) donde tiene su propio artículo llamado [Image-to-Image Translation with Conditional Adversarial Networks](https://arxiv.org/pdf/1611.07004.pdf) el cual tiene su propio artículo llamado [Image-to-Image Translation with Conditional Adversarial Networks](https://arxiv.org/pdf/1611.07004.pdf).

## Modelo pix2pix

El modelo **_pix2pix_** es una red generativa antagónica ([GAN](https://en.wikipedia.org/wiki/Generative_adversarial_network)) el cual tiene como intención ser una red generadora de imágenes de propósito general, donde dependiendo de qué conjunto de imágenes se use para entrenar la red, esta puede producir una imagen de salida a partir de una imagen de entrada y esta imagen de salida es parecida al conjunto de imágenes de salida con el que se entrenó. Y la manera en que funciona es que en realidad se usan dos redes neuronales, una red **_discriminativa_** y una red **_generativa_**, donde la red generativa trata de producir imágenes par a engañar a la red discriminativa pero el objetivo de esta última red es de aprender a distinguir entre una imagen real y una imagen falsa que haya hecho la red generativa. Entonces el proceso de aprendizaje del modelo pix2pix es básicamente hacer que la red generativa genere imágenes lo más cercano posible a las imágenes reales y la red discriminativa se vuelva mejor diferenciando entre las imágenes reales y las que genere la red generativa. Cuando se termine de entrenar la red del modelo pix2pix solo se usará la red generativa, pues el punto de todo esto es hacer una red que genere imágenes a partir de otra imagen.

Las arquitecturas que usan las dos redes de pix2pix son:

* Red generativa: El modelo propone usar dos tipos de arquitecturas: [**_encoder-decoder_**](https://machinelearningmastery.com/encoder-decoder-recurrent-neural-network-models-neural-machine-translation/) y [**_U-Net_**](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/). En una red convolucional consiste en hacer una convolución (downsampling o submuestreo) tras convolución a la imagen usando métodos como max pooling para encontrar los valores más importantes en un grupo de pixeles de la imagen y asi poder generalizar la información que esta en una imagen no importando en que parte de la imagen este dicha información. Entonces, lo que propone pix2pix es que una vez submuestreado la imagen de entrada lo más que se desea, se hace lo contrario al convolución y se hace una convolucion para atras (_convolución transpuesta_ o _deconvolución_ (este último termino se recomienda no usarlo ya que es usada en otras áreas de las matemáticas o visión de computadoras)) hasta llegar a las dimensiones de la imagen original de entrada. Pero esta idea se ha mejorado usando una modificación llamada [_encoder-decoder_](https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf) que es una arquitectura relativamente nueva del 2014 (no tanto para el área del aprendizaje profundo que es un área que se desarrolla muy rápido) el cual propone traer información de la imagen en un "punto del tiempo" de cuando estabamos convolucionando la imagen a otro "punto del tiempo" de cuando estamos convolucionando para atras cuando este tiene la misma dimensión que la imagen de donde estamos trayendo información, esto se puede ver algo así:

{{< figure src="encoder_decoder.png" title="Diagrama de la arquitectura encoder_decoder" >}}

## Mi modelo sketch2shiba
