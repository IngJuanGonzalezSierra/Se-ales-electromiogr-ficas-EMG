# Se-ales-electromiogr-ficas-EMG
Toma de Señales electromiográficas EMG 

# Introducción:
El electromiograma (EMG) es una grabación de la actividad eléctrica de los músculos, también llamada actividad mioeléctrica. Existen dos tipos de EMG, el
de superficie y el intramuscular o de aguja. Para poder realizar la captura de las señales mioeléctricas se utilizan dos
electrodos activos y un electrodo de tierra. En el caso de los electrodos de superficie, deben ser ubicados en la piel sobre el músculo a estudiar, mientras
que el electrodo de tierra se conecta a una parte del cuerpo eléctricamente activa. La señal EMG será la diferencia entre las señales medidas por los
electrodos activos. La ecuación  muestra la forma de calcular la respuesta impulsiva del potencial de acción medido con electrodos de
superficie.

[![Sin-t-tulo.png](https://i.postimg.cc/Dzm1F3ZD/Sin-t-tulo.png)](https://postimg.cc/rdXDCb5C) 

# Objetivo de la práctica
Analizar la señal electromiográfica (EMG) durante una contracción muscular continua hasta la fatiga, utilizando técnicas de adquisición de datos y procesamiento de señales digitales.

# Descripción de la configuración experimental
Electrodos de superficie
Se colocaron electrodos de superficie sobre el músculo objetivo para captar la actividad eléctrica generada durante la contracción muscular. Estos electrodos son esenciales para registrar señales EMG debido a su capacidad para detectar potenciales eléctricos generados por la actividad muscular.

# Sistema de adquisición de datos (DAQ NI USB 6001/6002/6003)
El sistema DAQ (Data Acquisition) NI USB 6001/6002/6003 es un dispositivo portátil de National Instruments diseñado para capturar señales analógicas y digitales. Tiene las siguientes características principales:

Función: Convierte señales analógicas (como la EMG) en digitales para su análisis posterior en una computadora.
Conexión: Se conecta mediante USB a un ordenador, lo que facilita su uso en laboratorios.
Aplicaciones: Se emplea en medición de señales biológicas, control en tiempo real y experimentos de laboratorio que requieren alta precisión.
Análisis de la Señal EMG:
Este laboratorio tiene como objetivo analizar la señal electromiográfica (EMG) de un músculo durante la fatiga muscular. El proceso se divide en varias etapas, desde la preparación del sujeto hasta el análisis espectral de la señal.

## Procedimientos:
•	Pedir al sujeto que realice una contracción muscular continua hasta llegar a la fatiga.
•	Registrar la señal EMG en tiempo real durante todo el proceso.

# Adquisición de la señal

El paciente realizó una contracción muscular continua hasta llegar a la fatiga, registrando la señal EMG durante todo el proceso. El análisis se dividió en tres fases:

Inicio de la señal:
La señal EMG tiene baja amplitud porque las unidades motoras apenas comienzan a activarse.
Durante el esfuerzo:
Se observa un incremento en la amplitud y la densidad espectral de la señal debido a la activación de más unidades motoras.
La frecuencia dominante se mantiene relativamente alta.
Fatiga muscular:
Disminuyen las frecuencias altas debido al agotamiento de las fibras musculares rápidas.
Aumentan las componentes de baja frecuencia porque las fibras lentas dominan la contracción.

# Procesamiento de la señal
Filtrado de la señal
Se emplearon dos filtros para mejorar la calidad de la señal:

Filtro pasa-alta:

Objetivo: Eliminar componentes de baja frecuencia, como ruido debido al movimiento del electrodo o artefactos respiratorios.
Rango de corte: Aproximadamente 20-30 Hz.
Justificación: La actividad EMG se concentra en frecuencias superiores a 30 Hz, por lo que el ruido de baja frecuencia no es de interés.
Filtro pasa-baja:

Objetivo: Reducir ruido de alta frecuencia, como el ruido electromagnético del entorno (por ejemplo, ruido de 50-60 Hz de la red eléctrica).
Rango de corte: Aproximadamente 500 Hz.
Justificación: Las señales EMG típicas tienen una banda de frecuencia útil entre 30 y 500 Hz.

#Segmentación de la señal
La señal procesada se dividió en 48 ventanas de tiempo utilizando una ventana de Hanning.

## Ventana Hanning:
Suaviza la señal en los bordes, reduciendo las distorsiones espectrales causadas por discontinuidades en los límites.
Ventaja sobre Hamming: La Hanning tiene un decrecimiento más pronunciado hacia los extremos, lo que reduce aún más las componentes espurias en el análisis espectral.
Duración de cada ventana: Dependió de la longitud total de la señal y la frecuencia de muestreo, garantizando una resolución adecuada.

# Análisis espectral
Técnica: Se utilizó la Transformada Rápida de Fourier (FFT) para convertir la señal de dominio temporal a dominio frecuencial.
Resultados esperados:
Durante el esfuerzo, la señal muestra un espectro centrado en frecuencias medias-altas (80-150 Hz).
En la fatiga, el espectro se desplaza hacia frecuencias bajas (<80 Hz), debido a la disminución en la capacidad contráctil de las fibras rápidas.

# Resultados esperados
Inicio: Señal de baja amplitud y espectro centrado en frecuencias bajas.
Esfuerzo: Incremento en la amplitud y ensanchamiento del espectro.
Fatiga: Disminución de la amplitud total, desplazamiento del espectro hacia frecuencias bajas.
Este análisis permite identificar patrones de activación muscular, niveles de esfuerzo y fatiga, proporcionando información valiosa para aplicaciones médicas y deportivas.
#### Código:
	  import numpy as np
	  import matplotlib.pyplot as plt
	  from scipy.signal import butter, filtfilt

# Función para leer una señal biológica desde un archivo txt
	def leer_senal_desde_txt(nombre_archivo):
	senal = []
	with open(nombre_archivo, 'r') as archivo:
	for linea in archivo:
	try:
	valores = linea.split(',')
	for valor in valores:
	senal.append(float(valor.strip()))
	except ValueError:
	print(f"Línea no numérica ignorada: {linea.strip()}")
	continue
	return np.array(senal)

# Leer la señal desde el archivo

	nombre_archivo = 'C:/Users/herna/OneDrive/Escritorio/lab señales/senalO.txt'
	senal = leer_senal_desde_txt(nombre_archivo)
	3. Filtrado de la Señal
	Procedimientos:
	•	Aplicar un filtro pasa altas para eliminar componentes de baja frecuencia (ruido asociado a la línea base o al movimiento).
	•	Utilizar un filtro pasa bajas para eliminar frecuencias altas no deseadas, como el ruido electromagnético o de interferencia de alta frecuencia.
 


[![Figure-2024-11-15-101438.png](https://i.postimg.cc/LsYQHkk5/Figure-2024-11-15-101438.png)](https://postimg.cc/7C4n92Wr)


# Función para aplicar un filtro Butterworth
	def aplicar_filtro(senal, tipo, fc, fs, orden=5):
	   nyquist = 0.5 * fs
	  normal_fc = fc / nyquist if isinstance(fc, (int, float)) else [f / nyquist for f in fc]
	   b, a = butter(orden, normal_fc, btype=tipo, analog=False)
	   senal_filtrada = filtfilt(b, a, senal)
	   return senal_filtrada

# Parámetros de la señal
frecuencia_muestreo = 250  # Frecuencia de muestreo en Hz

# Aplicar filtros pasa altos y pasa bajos en serie
	fc_pasa_bajos = 50  # Frecuencia de corte del filtro pasa bajos en Hz
	fc_pasa_altos = 1   # Frecuencia de corte del filtro pasa altos en Hz

	senal_filtrada = aplicar_filtro(senal, 'high', fc_pasa_altos, frecuencia_muestreo)
	senal_filtrada = aplicar_filtro(senal_filtrada, 'low', fc_pasa_bajos, frecuencia_muestreo)
[![Figure-2024-11-15-101503.png](https://i.postimg.cc/KY6q4KQN/Figure-2024-11-15-101503.png)](https://postimg.cc/Y4xfsC5G)
4. Aventanamiento
Procedimientos:
•	Dividir la señal registrada en ventanas de tiempo, usando una técnica de aventanamiento como la ventana de Hamming o Hanning.
•	Realizar el análisis espectral de cada ventana utilizando la Transformada de Fourier (FFT) para obtener el espectro de frecuencias en intervalos específicos de la señal EMG.

# Aplicar ventana de Hanning a la señal filtrada
ventana_hanning = np.hanning(len(senal_filtrada))
senal_filtrada_hanning = senal_filtrada * ventana_hanning

[![Figure-2024-11-15-101511.png](https://i.postimg.cc/C1bv8FQV/Figure-2024-11-15-101511.png)](https://postimg.cc/G8hJw1RM)

# Dividir la señal en ventanas
duracion_ventana = 1  # Duración de cada ventana en segundos
muestras_por_ventana = duracion_ventana * frecuencia_muestreo
num_ventanas = len(senal) // muestras_por_ventana

# Función para realizar la Transformada de Fourier (FFT)
	def realizar_fft(senal, fs):
	   n = len(senal)
	   k = np.arange(n)
	   T = n / fs
	   frq = k / T  # Dos lados del rango de frecuencia
	   frq = frq[range(n // 2)]  # Un lado del rango de frecuencia
	   Y = np.fft.fft(senal) / n  # FFT normalizada
	   Y = Y[range(n // 2)]
	   return frq, abs(Y)
    [![Figure-2024-11-15-134033.png](https://i.postimg.cc/VLqp8dJX/Figure-2024-11-15-134033.png)](https://postimg.cc/zVf0wDSf)}
    
[![Figure-2024-11-15-133959.png](https://i.postimg.cc/vZjBwqg1/Figure-2024-11-15-133959.png)](https://postimg.cc/xXvYKPPY)

[![Figure-2024-11-15-134152.png](https://i.postimg.cc/vTtDVk7P/Figure-2024-11-15-134152.png)](https://postimg.cc/K34ZX9HL)

[![Figure-2024-11-15-134203.png](https://i.postimg.cc/L6g8Rdfz/Figure-2024-11-15-134203.png)](https://postimg.cc/dLvYmx51)

[![Figure-2024-11-15-134213.png](https://i.postimg.cc/B6JjQhsJ/Figure-2024-11-15-134213.png)](https://postimg.cc/PLFXSQP7)

5. Análisis Espectral

### Procedimientos:
•	Observar cómo cambia el espectro de la señal en cada ventana conforme se acerca la fatiga muscular.
•	Evaluar la disminución de la frecuencia mediana en cada ventana como indicador de la fatiga.
•	Implementar una prueba de hipótesis para verificar si el cambio en la mediana es significativo estadísticamente.
# Función para calcular la frecuencia mediana
	def frecuencia_mediana(espectro, frq):
	   espectro_acumulado = np.cumsum(espectro)
	   mediana_idx = np.where(espectro_acumulado >= espect







