# Implementación de Pipelines de IA: Estabilidad en Seguimiento Focalizado (SOT)

Este repositorio contiene el pipeline experimental para evaluar el impacto del *Data Augmentation* espacial en la estabilidad del Seguimiento Focalizado de Objeto Único (SOT) bajo condiciones urbanas adversas. 

## Descripción del Proyecto
El proyecto aborda el reto de mantener un seguimiento confiable sobre un objeto específico dentro de un entorno urbano dinámico. Se evalúa el paradigma *Tracking-by-Detection* utilizando la arquitectura **YOLO11s** acoplada al rastreador cinemático **BoT-SORT**. 

El objetivo central de los experimentos es cuantificar el *trade-off* operativo: cómo las técnicas de aumentación espacial agresiva (*Mosaic*, *Copy-Paste*, *Erasing*) mejoran la precisión de detección en imágenes estáticas, pero introducen inestabilidad geométrica (*bounding box jittering*) que fragmenta las trayectorias durante el análisis de video continuo.

## Arquitectura y Tecnologías
* **Detector Base:** YOLO11s (Ultralytics)
* **Algoritmo de Rastreo:** BoT-SORT (Filtro de Kalman + Compensación de Movimiento de Cámara)
* **Dataset:** BDD100K (Dataset Élite curado a 9 clases vehiculares y peatonales)
* **Entorno de Cómputo:** Google Colab (NVIDIA L4 / A100)
* **Framework:** PyTorch, Python 3.12

## Estructura del Pipeline Experimental
El código está diseñado de forma modular para ejecutarse en entornos de libretas interactivas (Jupyter/Colab), garantizando tolerancia a desconexiones en la nube:

1. **Preparación y Curación:** Extracción del Dataset Élite y validación de integridad.
2. **Entrenamiento Línea Base (Configuración A):** Entrenamiento de YOLO11s con parámetros fotométricos estándar y *Early Stopping*.
3. **Entrenamiento con Aumentación (Configuración B):** Inyección de alteraciones espaciales extremas para simular oclusiones severas.
4. **Evaluación de Fragmentación (Fase 2):** Análisis de *ID Switching* en secuencias de video *raw* a 30 FPS estratificadas por clima, iluminación y tráfico.
5. **Renderizado Visual:** Generación de evidencia en video para contrastar el *Recall* vs. la estabilidad geométrica temporal.

## Resultados Destacados
* **Mejora en Detección Estática:** La Configuración B incrementa el mAP@0.5 frente a la línea base, evidenciando mayor robustez ante oclusiones y detección a larga distancia.
* **Degradación en Rastreo Continuo:** La hipersensibilidad adquirida por la red induce micro-oscilaciones espaciales, rompiendo la asociación del Filtro de Kalman y elevando la Fragmentación de Trayectorias de manera global (hasta un +20.9% en escenarios nocturnos).
