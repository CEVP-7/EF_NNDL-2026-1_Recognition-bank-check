# Verificación de Cheques con SGCIP-CNN y SigNet

Este repositorio contiene la implementación y los experimentos para la verificación de autenticidad de cheques utilizando dos arquitecturas de aprendizaje profundo de vanguardia: SGCIP-CNN (convoluciones escasas y agrupamiento espacial) y SigNet (redes siamesas/tripleta). El objetivo es distinguir entre cheques genuinos y falsificados analizando características visuales clave.

## Arquitecturas Utilizadas

* **SGCIP-CNN:** Esta red se enfoca en capturar eficientemente características discriminatorias en los cheques, aprovechando operaciones escasas para reducir la redundancia y agrupamiento espacial para preservar la información estructural de los elementos del cheque.
* **SigNet:** Esta arquitectura emplea redes siamesas o tripleta para aprender representaciones de características profundas que maximizan la distinción entre cheques genuinos y falsificados al comparar pares o tripletas de imágenes durante el entrenamiento.

## Requisitos de Instalación

Asegúrese de tener instaladas las siguientes dependencias principales:

* Python >= 3.8
* TensorFlow >= 2.x o PyTorch >= 1.x (dependiendo de la implementación específica)
* NumPy
* OpenCV-python
* Matplotlib
* Scikit-learn

Para instalar las dependencias principales usando `pip`:

```bash
pip install tensorflow keras torch numpy opencv-python matplotlib scikit-learn
