# Sistema Automatizado Multi-etapa para la Verificación de Cheques Bancarios

Este repositorio contiene los recursos, cuadernos de código (`notebooks`) y algoritmos desarrollados para la implementación de un sistema de extremo a extremo basado en Aprendizaje Profundo (*Deep Learning*) diseñado para la automatización, lectura y detección de fraudes en cheques financieros.

El sistema aborda tres pilares críticos: segmentación inteligente de regiones de interés, digitalización de campos manuscritos mediante arquitecturas Transformer, y un pipeline biométrico en cascada para la detección de falsificación de firmas.

---

## 🚀 Arquitectura General del Sistema

El flujo de procesamiento se compone de tres fases secuenciales e interconectadas:

1. **Fase 1: Segmentación Inteligente (YOLO):** Localización y extracción de las Regiones de Interés (ROIs) en el cheque (fecha, beneficiario, montos y firma).
2. **Fase 2: Digitalización de Datos Transaccionales (TrOCR):** Modelos especializados basados en arquitectura de codificador-decodificador Transformer para transcribir texto manuscrito de campos financieros.
3. **Fase 3: Verificación Biométrica de Firma (En Cascada):** Pipeline de aislamiento no destructivo de tinta (SSMHED) acoplado a un filtro estructural (SGCIP-CNN) y un verificador de identidad final *Zero-Shot* (SigNet).

---

## 📁 Organización de los Notebooks

Los experimentos y el código de producción están divididos cronológicamente según las fases del proyecto:

* **`notebooks/01_segmentacion_yolo.ipynb`**: Entrenamiento y evaluación del detector de objetos enfocado en la delimitación precisa de las cajas contenedoras de datos del cheque.
* **`notebooks/02_ocr_monto_num_words.ipynb`**: Ajuste fino (*Fine-Tuning*) de `microsoft/trocr-small-handwritten` aplicado de forma independiente para el monto numérico y el monto en letras (incluyendo estrategias de remuestreo/concatenación para cheques de formato extendido).
* **`notebooks/03_ocr_payee_date.ipynb`**: Transcripción automatizada del nombre del beneficiario y la fecha de emisión, integrando un algoritmo de posprocesamiento basado en expresiones regulares para estandarizar el formato de salida temporal.
* **`notebooks/04_preprocesamiento_ssmhed.ipynb`**: Implementación del extractor de bordes *Sobel Sparse Marr-Hildreth Edge Detector* (SSMHED) y operaciones morfológicas de dilatación para aislar de forma limpia el trazo de la firma preservando la escala de grises.
* **`notebooks/05_verificacion_firmas.ipynb`**: Entrenamiento de la red estructural SGCIP-CNN y codificación siamesa mediante SigNet optimizada con *Triplet Margin Loss* para contrastar firmas genuinas contra falsificaciones maestras.

---

## 📊 Resumen de Resultados Metodológicos

### 1. Extracción y Detección de Campos (Detección de Objetos)
* **Métricas Globales**: El modelo base alcanza un equilibrio de desempeño global $F_1\text{-score}$ de **0.71** con un umbral de confianza de **0.325**, logrando una precisión perfecta (**1.00**) al elevar el umbral a **0.926**.
* Las clases estructuradas como `date` y `cheque_number` logran niveles de sensibilidad excepcionales superiores a **0.98** de $mAP@0.5$.

### 2. Reconocimiento Óptico Inteligente (Módulo OCR)
La especialización de modelos independientes por tipo de campo mitigó las discrepancias de longitud secuencial arrojando el siguiente comportamiento en el conjunto de prueba:

| Campo | Exact Match (EM) | Character Error Rate (CER) | Word Error Rate (WER) |
| :--- | :---: | :---: | :---: |
| **Monto Numérico** | 36.02% | 0.3831 | — |
| **Monto en Palabras** | 0.75% | 0.3629 | 0.5787 |
| **Beneficiario** | 25.75% | 0.2750 | 0.6485 |
| **Fecha (Con Posprocesamiento)** | 75.00% | 0.2500 | — |

### 3. Clasificación y Validación Biométrica de Firmas
* **Filtro Estructural (SGCIP-CNN)**: Consigue una exactitud del **93.62%** en el análisis de la naturalidad balística del trazo, reduciendo los falsos rechazos a únicamente 8 instancias sobre el conjunto ciego.
* **Verificador de Identidad (SigNet)**: Al calibrar un umbral de distancia Euclidiana adaptativo de **0.771** empleando el Índice de Youden, la tasa de verdaderos positivos ($TPR$) escaló a un **91.20%** bajo un esquema riguroso *Writer-Independent*.

---

## 🛠️ Instalación y Requisitos

Para clonar e instalar el entorno de ejecución local, ejecute los siguientes comandos en su terminal:

```bash
# Clonar el repositorio
git clone [https://github.com/tu-usuario/verificacion-cheques-deeplearning.git](https://github.com/tu-usuario/verificacion-cheques-deeplearning.git)
cd verificacion-cheques-deeplearning

# Instalar dependencias requeridas
pip install -r requirements.txt
