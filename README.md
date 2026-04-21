==============================================================================

MANUAL DE USO: ANALIZADOR DE IMÁGENES TERMOCRÓMICAS (PYTHON)

==============================================================================

DESCRIPCIÓN:

Este script (Jupyter Notebook) automatiza el análisis de reflectancia de muestras
termocrómicas. Realiza las siguientes tareas:

1. Carga una secuencia de imágenes.
2. Alinea las imágenes automáticamente usando una referencia fija (Template Matching)
   para corregir vibraciones.
3. Extrae la intensidad media (reflectancia) de dos zonas de interés (Tinte 1 y 2).
4. Convierte el tiempo (número de foto) a temperatura usando ecuaciones de calibración.
5. Grafica las curvas de histéresis.

REQUISITOS:

- Python 3.x
- Librerías: opencv-python, numpy, matplotlib
- Entorno: Google Colab (recomendado) o Jupyter Notebook local.

==============================================================================

GUÍA DE CONFIGURACIÓN PASO A PASO (VALORES A CAMBIAR)

==============================================================================

Para usar este programa con tus propios datos, debes modificar las siguientes
secciones en el código (busque los comentarios marcados como # --- CONFIGURACION ---):

1. CARPETA DE IMÁGENES
----------------------
Busca la variable `base_dir`. Debes cambiar la ruta por la dirección donde
tengas tus fotos.

   - Si usas Google Colab: Sube tus fotos a Drive y copia la ruta.
   - Si usas local: Pon la ruta de tu disco duro (ej: "C:/Datos/Ciclo1").

   EJEMPLO:
   base_dir = '/content/drive/MyDrive/Mis_Datos/4seg_Ciclo1/*.jpg' #IMPORTANTE poner *.jpg al terminar la ruta

2. Selección didáctica de la Región de Interés (ROI)
El script incluye una interfaz interactiva con controles deslizantes (sliders) que hace el proceso más didáctico y visual. En lugar de ingresar coordenadas a ciegas, puedes modificar en tiempo real la posición (Pos X, Pos Y) y el tamaño (Ancho, Alto) de los recuadros.

Esto permite ubicar con precisión las zonas de "ANCLA" y "TINTE" sobre el material termocrómico, asegurando que el análisis se realice exactamente sobre la muestra deseada tal como se muestra en la previsualización.

3. Configuración de variables del experimento
Para procesar correctamente los datos físicos, es necesario actualizar las variables globales al inicio del código con los valores específicos de la medición realizada. Se deben ingresar los siguientes datos:
TOTAL_IMAGENES =   # Número total de fotogramas capturados 
NUM_CICLOS =         # Cantidad de ciclos de calentamiento/enfriamiento 
T_INICIAL =      # Temperatura base (°C) 
T_FINAL =          # Temperatura máxima alcanzada (°C) 
FOTOS_EN_MESETA =     # Imágenes tomadas durante la estabilización térmica 
-----------------------------

El código convierte el tiempo en temperatura usando la ecuación de una recta:
T = m * t + b

Debes cambiar los valores de la pendiente (m) y la ordenada (b) con los datos
que obtuviste de tu caracterización del Peltier.

CÓDIGO A MODIFICAR (Sección de cálculo de temperaturas):

Para la subida (calentamiento):
temperatura_subida.append(0.034 * i + 21.297)  <-- Cambia 0.034 y 21.297

Para la bajada (enfriamiento):
temperatura_bajada.append(-0.0339 * i + 65.778) <-- Cambia -0.0339 y 65.778 

==============================================================================

SOLUCIÓN DE PROBLEMAS COMUNES

==============================================================================

- ERROR "OpenCV(4.8.0) ... (-215:Assertion failed)":
  Significa que la ruta de las imágenes está mal o la carpeta está vacía.
  Verifica `base_dir`.

- LA GRÁFICA SALE CON PICOS GIGANTES:
  La alineación falló. Probablemente el `template` que elegiste cambió de color
  o se salió de la imagen. Elige una marca que NUNCA cambie (el metal o un borde).

- LAS CURVAS NO SE CIERRAN O SE VEN DESFASADAS:
  Revisa tus ecuaciones de temperatura (paso 4). Si la ecuación está mal,
  el eje X (Temperatura) será incorrecto.

==============================================================================

AUTOR DEL CÓDIGO ORIGINAL: Brian Eduardo Fuentes Hernandez y Salma Anette Rodriguez Muñoz

LABORATORIO DE FÍSICA CONTEMPORÁNEA - FACULTAD DE CIENCIAS, UNAM

==============================================================================
