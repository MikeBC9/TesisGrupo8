# TesisGrupo8

# 🦓 HerdNet — Detección y Conteo de Fauna en Imágenes Aéreas con Deep Learning

Este repositorio contiene el código, notebooks y recursos desarrollados como parte del proyecto de tesis de los autores **Juan Pablo Cediel, Juan Felipe Campozano, Mijail Bolaños, Sebastián Laguna y Adrián Martínez**.

El objetivo es automatizar el **conteo y clasificación de animales** en imágenes aéreas mediante técnicas de deep learning, abordando desafíos como **oclusiones**, **clases desbalanceadas** y **fondos naturales complejos**.

## 🧠 Resumen Técnico

Se propone **HerdNet**, una arquitectura *encoder–decoder* basada en **Deep Layer Aggregation (DLA)** con salidas *multi-cabeza*, que permite:
- Detectar la presencia de animales en imágenes aéreas densas.
- Contar individuos por especie.
- Clasificar las especies con segmentación semántica.

El modelo fue entrenado sobre un dataset real de 1.297 imágenes aéreas anotadas (6 especies) con más de 10.000 individuos. Los resultados demuestran mejoras sustanciales frente a modelos como Faster R-CNN y mapas de densidad.

## 🏗️ Arquitectura HerdNet

- **Backbone:** DLA-34 con extracción jerárquica de características
- **Decoder:** módulos de upsampling con `dla_up`
- **Salidas multi-cabeza:**
  - `loc_head`: mapa de calor para localización puntual
  - `cls_head`: segmentación por especie
- **Loss function:** combinación modular de Focal Loss, FIDT y clasificación por pixel

## 📈 Resultados Clave

| Modelo             | F1-score | AP    | RMSE conteo |
|--------------------|----------|-------|-------------|
| Faster R-CNN       | 0.48     | 0.60  | 9.3         |
| Mapas de densidad  | 0.51     | 0.63  | 8.5         |
| HerdNet (sin HNP)  | 0.859    | 0.744 | 4.38        |
| **HerdNet + HNP**  | **0.873**| **0.856** | **3.19**  |

## 📂 Estructura del Repositorio

    ├── 010_MuestraDatos/
    ├── 020_Exploration/
    ├── 030_Notebooks/
    ├── 040_Modelo/
    ├── 050_HerdNet/
    ├── api/
    ├── data_classes_names.ipynb
    ├── transforms.py
    ├── README.md

## 🧰 Tecnologías y Librerías

- Python 3.8
- PyTorch 1.12
- CUDA 11.4
- Albumentations, OpenCV, SciPy
- FiftyOne, Weights & Biases
- FastAPI (para despliegue)
- AWS (EC2 + S3 + CloudWatch)

## ⚙️ Instalación y Uso

1. Clona el repositorio:

       git clone https://github.com/MikeBC9/TesisGrupo8.git
       cd TesisGrupo8

2. Instala dependencias:

       pip install -r requirements.txt

3. Corre los notebooks desde Jupyter:

       jupyter notebook

4. Usa la API desplegada para inferencia:
   - Swagger: http://3.19.40.171:8000/docs
   - Interfaz para predecir: http://3.19.40.171:8000
   - Dashboard: http://3.19.40.171:8000/dashboard

      Instrucciones Básicas del API

      1. Abrir el navegador: http://3.19.40.171:8000
      2. Seleccionar imagen: Haz clic en el botón Choose File y selecciona una imagen desde tu dispositivo (idealmente JPG o PNG).
      3. Ejecutar la inferencia: Presiona el botón verde Predecir.
      4. Ver resultados: Se mostrará la imagen original. Aparecerán mapas por clase (zonas destacadas según la detección). Abajo verás las especies detectadas.

## 🐘 Dataset

- Fuente: Dataverse (CC BY-NC-SA 4.0)
- Resolución: 6000x4000 px
- Especies: Búfalo, Elefante, Kob, Topi, Jabalí verrugoso, Cobo de agua
- Total: 10.239 individuos anotados

## 🔬 Métricas de Evaluación

- F1-score binario
- Average Precision (AP)
- RMSE de conteo
- Matriz de confusión

## 📌 Hallazgos Clave

- HerdNet mejora la precisión y robustez en escenarios con oclusiones
- La arquitectura multi-cabeza permite detección y clasificación conjunta
- Clases minoritarias siguen siendo un reto (F1 < 0.53)
- Requiere GPU potente para entrenamiento e inferencia

## 🧪 Futuro del Proyecto

- Aplicación a entornos más complejos (selvas, sabanas)
- Inferencia en tiempo real sobre drones (*edge computing*)
- Integración de módulos de atención y aprendizaje auto-supervisado
- Balanceo activo de clases con muestreo focalizado

## 📝 Licencia

Este proyecto se distribuye bajo la Licencia MIT. Ver el archivo `LICENSE` para más información.
