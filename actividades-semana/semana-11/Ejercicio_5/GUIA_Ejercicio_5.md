# Guia del Ejercicio 5: manos y deteccion en video

Este ejercicio contiene dos aplicaciones locales de vision artificial:

1. Un tracker de esqueleto de la mano usando la camara integrada.
2. Un detector de objetos usando YOLO sobre un video seleccionado por el usuario.

## 1. Como ejecutar el notebook

Ejecuta las celdas en este orden:

1. Instala las dependencias.
2. Ejecuta la comprobacion rapida.
3. Ejecuta el tracker de manos si quieres usar la camara.
4. Ejecuta la celda de seleccion de video si quieres analizar un archivo.
5. Ejecuta la celda de YOLO para procesar el video seleccionado.

No ejecutes el tracker y el procesamiento de video al mismo tiempo.

## 2. Tracker de manos

El tracker abre la camara integrada y analiza sus cuadros en tiempo real. No guarda fotografias ni graba un video automaticamente. Cada cuadro se procesa temporalmente, se dibuja el esqueleto y luego se continua con el siguiente cuadro.

Al ejecutar la celda del tracker aparece una ventana llamada `Tracker de esqueleto de mano`. Coloca la mano frente a la camara y presiona `q` o `Esc` para terminar.

### Como detecta la mano, explicado de forma sencilla

Imagina que el programa recibe muchas fotografias por segundo. La red neuronal busca patrones que suelen aparecer en una mano, como la palma, los dedos y las articulaciones. Luego estima 21 puntos y conecta esos puntos para formar el esqueleto.

El modelo no entiende una mano como una persona. Aprendio a producir esta estimacion despues de observar muchas imagenes de manos con puntos marcados. Cuando ve una imagen nueva, calcula las posiciones mas probables.

### Como funciona tecnicamente

MediaPipe Tasks utiliza una red neuronal de vision para extraer caracteristicas de la imagen. Las primeras capas identifican bordes y texturas; las capas intermedias combinan esos patrones para reconocer partes de una mano; y las capas finales estiman las coordenadas de sus puntos.

Las coordenadas se expresan de forma normalizada. Por ejemplo, `x=0` representa el borde izquierdo y `x=1` el borde derecho. El programa multiplica esas coordenadas por el ancho y el alto del cuadro para obtener pixeles y dibujar los puntos.

El modo `VIDEO` usa la informacion temporal de cuadros anteriores para mantener el seguimiento. Esto evita tratar cada cuadro completamente desde cero y ayuda a que el movimiento sea mas estable.

## Configuracion del codigo

### `HAND_CONNECTIONS`

```python
HAND_CONNECTIONS = [
	(0, 1), (1, 2), (2, 3), (3, 4),
	(0, 5), (5, 6), (6, 7), (7, 8),
	(5, 9), (9, 10), (10, 11), (11, 12),
	(9, 13), (13, 14), (14, 15), (15, 16),
	(13, 17), (0, 17), (17, 18), (18, 19), (19, 20),
]
```

Esta no es una matriz aprendida por nuestro programa. Es una lista de conexiones basada en la topologia estandar de los 21 landmarks que entrega MediaPipe Hand Landmarker. Cada tupla `(a, b)` indica que se debe dibujar una linea entre el punto `a` y el punto `b`.

Los indices representan estas partes de la mano:

| Indices | Parte |
|---|---|
| `0` | Muneca |
| `1-4` | Pulgar, desde la base hasta la punta |
| `5-8` | Dedo indice |
| `9-12` | Dedo medio |
| `13-16` | Dedo anular |
| `17-20` | Dedo menique |

Por ejemplo, `(5, 6)` une dos articulaciones del dedo indice, mientras `(0, 5)` une la muneca con la base de ese dedo. Si se elimina una tupla, desaparece una linea; si se agrega una conexion incorrecta, aparecera una linea artificial. Los puntos detectados por la red no cambian, solo cambia la forma en que los visualizamos.

### Parametros del detector de manos

En `HandLandmarkerOptions` se configuran los siguientes valores:

| Parametro | Valor actual | Funcion y efecto de modificarlo |
|---|---:|---|
| `running_mode` | `VIDEO` | Indica que los cuadros llegan en orden temporal. Usar otro modo cambia como se realiza el seguimiento. |
| `num_hands` | `2` | Maximo de manos detectadas. Bajarlo a `1` puede acelerar el programa, pero ignora una segunda mano. |
| `min_hand_detection_confidence` | `0.60` | Confianza minima para aceptar que hay una mano. Subirla reduce falsos positivos, pero puede perder manos dificiles. |
| `min_hand_presence_confidence` | `0.60` | Confianza minima de que la mano sigue presente. Subirla produce detecciones mas estrictas. |
| `min_tracking_confidence` | `0.60` | Confianza minima para conservar el seguimiento entre cuadros. Subirla puede hacer que el tracker pierda manos durante movimientos rapidos. |

Los valores de confianza van normalmente de `0` a `1`. Un valor mayor no significa que la red aprenda mejor; significa que exigimos mas seguridad antes de mostrar una deteccion. Para una camara oscura puede ser util bajar temporalmente los umbrales, por ejemplo a `0.50`, aunque aumenten las detecciones incorrectas.

### Parametros de captura y dibujo

- `cv2.VideoCapture(0)` usa la camara predeterminada del computador. Si hay varias camaras, se puede probar `1` o `2`.
- `cv2.flip(frame, 1)` crea una vista tipo espejo. Si se elimina, la imagen deja de invertirse horizontalmente.
- `cv2.cvtColor(..., cv2.COLOR_BGR2RGB)` transforma el formato de color de OpenCV al formato que espera MediaPipe.
- `timestamp_ms` debe aumentar en cada cuadro cuando se usa el modo `VIDEO`. Si no aumenta, el seguimiento temporal puede fallar.
- El tamano de los circulos y lineas se controla con los ultimos argumentos de `cv2.circle` y `cv2.line`. Cambiarlos solo modifica la apariencia.

### Configuracion de YOLO para video

En la celda de deteccion se usa:

```python
video_model = YOLO("yolov8n.pt")
conf=0.35
imgsz=640
device="cpu"
stream=True
```

- `yolov8n.pt` es un modelo pequeno de deteccion, rapido pero menos preciso que modelos mas grandes.
- `conf=0.35` es la confianza minima para conservar una deteccion. Subirla elimina detecciones dudosas; bajarla muestra mas objetos, pero puede aumentar errores.
- `imgsz=640` es el tamano de entrada. Un valor mayor puede detectar objetos pequenos con mas detalle, pero aumenta el tiempo y la memoria usados.
- `device="cpu"` fuerza el procesamiento en el procesador. En un equipo con GPU compatible se puede usar `device=0` para acelerar.
- `stream=True` procesa el video cuadro por cuadro en lugar de cargarlo completo en memoria.

Cambiar estos parametros no reentrena la red. Solo cambia la velocidad, el consumo de recursos o el nivel de filtrado durante la inferencia. Para que YOLO aprenda clases nuevas se necesita un dataset etiquetado y un proceso de fine-tuning.

### Cuando usarlo y limitaciones

Usalo para estudiar posiciones de dedos, movimientos de la mano o interacciones en vivo. Puede fallar si la mano esta oscura, muy lejos, parcialmente tapada, fuera del cuadro o se mueve demasiado rapido.

## 3. Deteccion de objetos en video

Este ejercicio no activa la camara. Abre un explorador de archivos para que selecciones un video que ya exista en el computador. Puedes grabarlo previamente con la aplicacion de camara del computador o del celular, o usar un video permitido para la actividad.

Formatos aceptados por el selector:

- `.mp4`
- `.avi`
- `.mov`
- `.mkv`
- `.webm`

El programa no modifica el video original. Genera una copia con cajas, nombres de clase y confianza sobre los objetos detectados. El resultado se guarda en:

```text
resultados_video/detecciones/
```

### Como funciona de forma sencilla

YOLO revisa el video como una secuencia de fotografias. En cada cuadro busca objetos conocidos y coloca una caja alrededor de cada uno. Tambien escribe una etiqueta, como `person`, `car`, `dog` o `backpack`, junto con una confianza.

Despues de procesar todos los cuadros, el programa guarda los cuadros anotados como un nuevo video.

### Como funciona tecnicamente

YOLO es una red neuronal convolucional que analiza una imagen completa en una sola pasada. Sus capas convolucionales convierten los pixeles en caracteristicas: primero detectan bordes y texturas, despues formas y finalmente combinaciones que pueden corresponder a objetos.

La red devuelve:

- Coordenadas de las cajas delimitadoras.
- Clase estimada del objeto.
- Confianza de la prediccion.

El codigo usa `stream=True`, por lo que procesa los cuadros uno a uno sin cargar el video completo en memoria. Esto reduce el uso de memoria, aunque el procesamiento puede tardar dependiendo de la duracion, resolucion y cantidad de cuadros.

### Cuando usarlo y limitaciones

Usalo para localizar personas, animales, vehiculos y otros objetos generales en un video. El modelo `yolov8n.pt` es ligero y rapido, pero solo reconoce las clases aprendidas durante su entrenamiento.

No esperes que identifique automaticamente un juego tipo Souls, un personaje especifico, una emocion o la identidad de una persona. Para clases personalizadas se necesita crear un dataset, etiquetarlo y hacer fine-tuning.

## 4. Diferencia entre camara y video

| Funcion | Entrada | Resultado | Graba automaticamente |
|---|---|---|---|
| Tracker de manos | Camara integrada | Ventana en tiempo real con esqueleto | No |
| YOLO en video | Archivo seleccionado | Nuevo video con detecciones | No |

Para usar un video de tu camara, primero grabalo con otra aplicacion, guardalo y despues selecciona el archivo en el notebook.

## 5. Problemas frecuentes

### La ventana de la camara parece bloqueada

Es normal: el tracker tiene un ciclo continuo. Selecciona la ventana de OpenCV y presiona `q` o `Esc`.

### No se abre la camara

Cierra Zoom, Teams, la aplicacion Camara u otro programa que pueda estar usandola. Verifica tambien los permisos de camara de Windows.

### El video tarda mucho

YOLO analiza cada cuadro. Los videos largos, de alta resolucion o con muchos cuadros tardan mas. `yolov8n.pt` es la version ligera; una GPU puede acelerar el proceso.

### No encuentra objetos

El objeto puede no pertenecer a las clases del modelo, ser muy pequeno, estar oculto o tener poca iluminacion. Reducir demasiado el umbral de confianza puede aumentar detecciones incorrectas.
