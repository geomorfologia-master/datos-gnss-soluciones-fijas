Prácticas de aula 6 (PA06). Procesa datos GNSS y obtén soluciones
fijas<small><br>Geomorfología (GEO-114)<br>Universidad Autónoma de Santo
Domingo (UASD)<br>Semestre 2024-02</small>
================
El Tali
2024-10-11

Versión HTML (quizá más legible),
[aquí](https://geomorfologia-master.github.io/datos-gnss-soluciones-fijas/README.html)

# Fecha/hora de entrega

**14 de octubre de 2024, 7:59 pm.**

# Objetivos

1.  **Familiarizarse con el procesamiento de datos GNSS**: Los
    estudiantes aprenderán a descargar, convertir y analizar datos GNSS
    recolectados en el campo utilizando diferentes herramientas, con el
    fin de obtener soluciones precisas de posición.

2.  **Evaluar la calidad de datos GNSS crudos**: A través de la
    inspección visual de los archivos en formato RINEX y el uso de
    herramientas especializadas como RTKLib, los estudiantes
    identificarán posibles problemas en las observaciones, como saltos
    de ciclo o interrupciones en las señales.

3.  **Obtener coordenadas precisas de la base GNSS mediante el servicio
    CSRS-PPP**: Los estudiantes aprenderán a procesar observaciones de
    la base a través del servicio canadiense de posicionamiento puntual
    preciso (CSRS-PPP) para generar coordenadas precisas.

4.  **Convertir coordenadas a WGS84**: Los estudiantes utilizarán el
    servicio HTDP del NGS (NOAA) para convertir las coordenadas
    obtenidas del procesamiento GNSS a WGS84, ajustándose a las normas
    de referencia geodésica internacional.

5.  **Generar soluciones fijas mediante posprocesamiento GNSS**: A
    través de herramientas de posprocesamiento como RTKPOST, los
    estudiantes utilizarán las coordenadas de la base para obtener
    soluciones de alta precisión para las coordenadas del rover,
    consolidando el ciclo de procesamiento de datos GNSS.

# Recursos

- Vídeo

  - [Vídeo de apoyo para obtener soluciones fijas usando observables del
    rover y observables de ocupación estática de la
    base](https://drive.google.com/file/d/1j4YXlBTxb_ouYsBzqIQgPyhpCdn09WoN/view?usp=drive_link)

  - [Vídeo de apoyo para convertir coordenada ITRF a WGS84
    clase](https://drive.google.com/file/d/1j4YXlBTxb_ouYsBzqIQgPyhpCdn09WoN/view?usp=drive_link)

Notar que en el vídeo sugerido, se conocen las coordenadas precisas de
la base. En este caso, las coordenadas de la base deben generarse a
partir de las observaciones brutas realizadas en el terreno.

-Software:

- Converter de Unicore. Para convertir archivo Unicore (de la base),
  usar Converter, que se encuentra
  [aquí](https://drive.google.com/drive/folders/1uh69yyfTBoJakwA3yxUPJAyA2rRIuzEO?usp=drive_link)

- [RTKLib Demo5](https://github.com/rtklibexplorer/RTKLIB/releases)

  - Descarga la versión más reciente “RTKLib Demo5”. A la fecha de
    elaborar esta práctica, la más reciente era “demo5 b34k”. Con
    independencia de cuál sea la versión, si estás usando como sistema
    operativo Windows, te convendrá descargar el binario compildo, el
    cual se encuentra en el archivo de extensión `.zip` (por ejemplo
    `demo5_b34k.zip`).

# Ejercicio 1. Descarga tus datos, conviértelos, evalúa su calidad, descríbelos

- Visita [esta carpeta de
  Drive](https://drive.google.com/drive/folders/1bEyVLVSQo7-bkQr5q4Tt_-ookPERQKBH?usp=drive_link)
  y descarga tus datos, según el número que te tocó (estudiante-##).

- Con la herramienta `Converter`, convierte tus archivos extensión .log
  (de base y de rover) a formato RINEX (elige el formato 3.04, marca y
  desmarca la opción Obs2Range, y desmarca teqc). Documéntate sobre el
  formato RINEX.

- Con cualquiera de las herramientas de RTKLib, denominadas, RTKCONV
  (interfaz gráfica) o `convbin` (terminal), convierte el archivo de la
  base de extensión .ubx a RINEX.

- Evalúa su calidad:

  - Simple inspección visual del RINEX.

  - Con RTKLib, identificando si hay saltos de ciclo (*cycle slips*),
    observaciones interrumpidas, \*DOP. Tendrás que documentarte al
    respecto.

Redacta un párrafo que sintetice la justificación de este paso, el
método empleado, qué obtuviste (describe los datos obtenidos) y qué
limitaciones encontraste.

# Ejercicio 2. Obtén la coordenada precisa de la base

- Visita la web del Sistema Canadiense de Referencia Espacial
  Posicionamiento Puntual Preciso (*Canadian Spatial Reference System
  Precise Point Positioning*, CSRS-PPP). Esta es la [ruta del
  servicio](https://webapp.csrs-scrs.nrcan-rncan.gc.ca/geod/tools-outils/ppp.php),
  y la página para iniciar sesión o crear cuenta está
  [aquí](https://webapp.csrs-scrs.nrcan-rncan.gc.ca/geod/account-compte/login.php).

- Crea una cuenta si no la tienes aún. Debes usar un correo válido,
  porque a dicha dirección te enviarán un enlace de confirmación de
  verificación de tu registro. Además, los datos que pidas que te
  procesen, te los enviarán a dicha cuenta.

- Una vez dentro del servicio, sube tus RINEX observables de la base,
  tanto los originados a partir de los .log, que son los Unicore tomados
  en terreno (archivos. \*.??O, donde normalmente ?? son los dos últimos
  dígitos del año, y la letra “O” significa “observables”), como el o
  los RINEX originados a partir del binario del receptor U-blox (si tus
  RINEX observables aún no tienen extensión ??O, este es un buen momento
  para que les cambies las extensión). Puedes juntar todos tus RINEX en
  un único comprimido .zip, .gzip, .Z o .tar, y posteriormente subir el
  comprimido (recibirás tantas soluciones PPP como RINEX envíes). Si son
  muy grandes los archivos RINEX, puedes usar RTKCONV para generar un
  RINEX de tasa de muestreo de 30 segundos, por ejemplo.

- Recibirás un mensaje de correo con un .zip. Abre el PDF. El error
  (“Sigmas(95%)”) dependerá de múltiples factores, especialmente de la
  duración de la colecta de la base (menos de 1 hora suele ser poco), de
  las condiciones intrínsecas del sitio (e.g. poca obstrucción) y de la
  calidad de los equipos.

Redacta un párrafo que sintetice la justificación de este paso, el
método empleado, qué obtuviste (describe los datos obtenidos) y qué
limitaciones encontraste.

# Ejercicio 3. Convierte la coordenada la base a WGS84

- Usa el servicio de Posicionamiento Horizontal Dependiente de la Fecha
  (Horizontal Time-Dependent Positioning, HTDP) del Servicio Geodésico
  Nacional de Estados Unidos (NGS, NOAA) para convertir la coordenada de
  la base a WGS84.

Redacta un párrafo que sintetice la justificación de este paso, el
método empleado, qué obtuviste (describe los datos obtenidos) y qué
limitaciones encontraste.

# Ejercicio 4. Genera soluciones fijas

- Usa cualquier de las herramientas de RTKLib para hacer posproceso.
  Puedes usar la herramienta RTKPOST (interfaz gráfica) o rnx2rtkp
  (línea de comandos). Necesitarás las coordenadas de la base en WGS84
  para esto.

Redacta un párrafo que sintetice la justificación de este paso, el
método empleado, qué obtuviste (describe los datos obtenidos) y qué
limitaciones encontraste.

# Referencias
