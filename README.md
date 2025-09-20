Prácticas de aula 3 (PA03). Procesa datos GNSS y obtén soluciones
fijas<small><br>Geomorfología (GEO-114)<br>Universidad Autónoma de Santo
Domingo (UASD)</small>
================
El Tali
2025-09-20

Versión HTML (quizá más legible),
[aquí](https://geomorfologia-master.github.io/datos-gnss-soluciones-fijas/README.html)

# Fecha/hora de entrega

**Ver portal de la asignatura**

# Objetivos

1.  **Familiarizarse con el procesamiento de datos GNSS**: Los y las
    estudiantes aprenderán a descargar, convertir y analizar datos GNSS
    recolectados en el campo utilizando diferentes herramientas, con el
    fin de obtener soluciones precisas de posición.

2.  **Evaluar la calidad de datos GNSS crudos**: A través de la
    inspección visual de los archivos en formato RINEX y el uso de
    herramientas especializadas como RTKLib, los estudiantes
    identificarán posibles problemas en las observaciones, como saltos
    de ciclo o interrupciones en las señales.

3.  **Convertir coordenadas a WGS84**: Los estudiantes utilizarán el
    servicio HTDP del NGS (NOAA) para convertir las coordenadas
    obtenidas del procesamiento GNSS a WGS84, ajustándose a las normas
    de referencia geodésica internacional.

4.  **Generar soluciones fijas mediante posprocesamiento GNSS**: A
    través de herramientas de posprocesamiento como RTKPOST, los
    estudiantes utilizarán las coordenadas de la base para obtener
    soluciones de alta precisión para las coordenadas del rover,
    consolidando el ciclo de procesamiento de datos GNSS.

# Recursos

- Vídeo

  - [Vídeo de apoyo para obtener soluciones fijas usando observables del
    rover y observables de ocupación estática de la
    base](https://drive.google.com/file/d/1j4YXlBTxb_ouYsBzqIQgPyhpCdn09WoN/view?usp=drive_link).
    El vídeo muestra el proceso completo de obtener soluciones fijas por
    posproceso. Lo grabé hace unos dos años en el aula FC-203 para
    estudiantes que estaban, en ese momento, usando algunas de las PC en
    dicho espacio (escucharán personas preguntando sobre el proceso). La
    versión mencionada de RTKLib en el vídeo es vieja, así que descarga
    la más reciente (ver abajo enlace para descargar RTKLib-Ex). Otro
    detalle importante: dado que en dicha práctica colocamos una base
    móvil, en el vídeo me verán obtener las coordenadas precisas de
    dicha base móvil. **En este caso, es decir, en la práctica actual,
    la base es fija, y sus coordenadas precisas ya fueron obtenidas por
    medio de solución PPP (ver más abajo), por lo que no tendrás que
    obtener las coordenadas de la base por solución PPP**. Eso sí,
    tendrás que convertir la coordenada que se encuentra actualmente en
    ITRF a WGS84, pues RTKLib-Ex sólo trabaja con coordenadas en WGS84.

  - [Vídeo de apoyo para convertir coordenada ITRF a WGS84
    clase](https://drive.google.com/file/d/1j4YXlBTxb_ouYsBzqIQgPyhpCdn09WoN/view?usp=drive_link)

-Software:

- Converter de Unicore. Para convertir archivos Unicore (normalmente son
  de extensión `.log`, pero a veces vienen en extensión `.unc`), usar
  Converter, que se encuentra
  [aquí](https://drive.google.com/drive/folders/1uh69yyfTBoJakwA3yxUPJAyA2rRIuzEO?usp=drive_link)

- [RTKLib-EX, antiguamente
  “Demo5”](https://github.com/rtklibexplorer/RTKLIB/releases). Descarga
  la versión más reciente de “RTKLIB-EX”. A la fecha de elaborar esta
  práctica, la más reciente era “RTKLIB-EX 2.5.0”. Con independencia de
  cuál sea la versión, si estás usando como sistema operativo Windows,
  te convendrá descargar el binario compilado, el cual se encuentra en
  el archivo de extensión `.zip` (por ejemplo
  [v2.5.0.zip](https://github.com/rtklibexplorer/RTKLIB/archive/refs/tags/v2.5.0.zip)).

- [u-center](https://www.u-blox.com/en/product/u-center). Sirve para
  convertir archivos de formato u-blox. ¡¡Importante, descarga la
  versión 25.06 o la que se encuentre vigente para trabajar con archivos
  generados por hardware F9; no descargues ni instales u-center 2 para
  esta práctica!!

# Ejercicio 1. Descarga tus datos, conviértelos, evalúa su calidad, descríbelos

- Visita [esta carpeta de
  Drive](https://drive.google.com/drive/folders/1i5vjg74UvPlk5JYNqnjek-2YNaWiKiFm?usp=drive_link).
  Aquí alojé las colectas de terreno (rovers) y las sincrónicas de la
  base. Normalmente, los datos que hayas obtenido con el o los rovers,
  llevarán tu nombre como prefijo (al inicio); los de las bases no
  llevan prefijos, porque aplican para todos.

## Convierte a RINEX

Convierte tus archivos de observaciones crudas (*raw*) a formato RINEX
v3.04. Las observaciones crudas contienen las mediciones GNSS
(pseudorango, fase portadoras, doppler, SNR, etc.) y las efemérides
(órbitas de los satélites). Los archivos de observaciones crudas pueden
estar en distintos formatos, dependiendo del hardware usado. En este
caso, tienes dos tipos de hardware.

> Tengo por costumbre guardar las soluciones RTK (las que se tomaron en
> terreno) junto con las observaciones crudas (me gusta quedarme con
> todo para tener más elementos de comparación). Por lo tanto, los
> archivos `.ubx` y `.log` contienen también soluciones RTK en formato
> NMEA, las cuales puedes visualizar en RTKPLOT (de RTKLib-Ex) o con
> cualquier otro software capaz de “parsear” sentencias NMEA (investiga
> qué son sentencias NMEA). Sin embargo, en esta práctica lo que te pido
> es que generes soluciones fijas por posproceso usando las
> observaciones crudas, y las efemérides contenidas en los archivos
> `.ubx` y `.log` y usando dos bases. Por lo tanto, no usarás las
> soluciones RTK que ya vienen en los archivos `.ubx` y `.log`, pero las
> tienes en los archivos por si quieres verlas y contrastar.

Los archivos Unicore (casi seguramente tienen extensión `.log`, pero
también podrían ser `.unc`), los podrás convertir con la herramienta
`Converter`; ahora también RTKLIB-EX los convierte, pero había algunos
problemillas meses atrás (desconozco si se resolvieron; en las últimas
releases se asegura que sí, pero habría que hacer pruebas y no me fío).
Te recomiendo que conviertas tus archivos Unicore (extensión `.log`), de
base y de rover, a formato RINEX (v 3.04) usando la herramienta
`Converter`; desmarca la opción Obs2Range, y desmarca teqc. Documéntate
sobre el formato RINEX.

En el caso de los archivos u-blox (extensión `.ubx`), con cualquiera de
las herramientas de RTKLib, denominadas, RTKCONV (interfaz gráfica) o
`convbin` (terminal), convierte los archivos de formato U-blox
(extensión `.ubx`) a RINEX v3.04.

En ambos casos, además de los observables, obtendrás las efemérides
(órbitas de los satélites) en formato RINEX, las cuales necesitarás en
el posproceso. Las efemérides tienen extensión .`.rnx`, `.nav`, o
también `.##N`, `##.G`, `.##L`, `.##C` o `.##P` (donde `##` son dos
dígitos que indican el año). Cuando RTKCONV convierte un archivo `.ubx`
a RINEX, las efemérides se almacenan en un archivo `.nav` (todas las
constelaciones mezcladas, GPS, GLONASS, Galileo, BeiDou). Por otro lado,
`Converter` genera las efemérides en archivos separados por
constelación, pero también produce un archivo mezclado de extensión
`.##P` (en tu caso seguramente será `.25P`); dicho archivo es más cómodo
de usar en RTKPOST, pues al igual que el `.nav`, contiene las efemérides
de todas las constelaciones.

## Evalúa la calidad

- Simple inspección visual de los RINEX, tanto los de las bases como los
  de los rovers

- Con RTKLib, identificando si hay saltos de ciclo (*cycle slips*),
  observaciones interrumpidas, \*DOP. Tendrás que documentarte al
  respecto.

# Ejercicio 2. Convierte la coordenada la base a WGS84

## Coordenadas de las bases

Para obtener soluciones precisas (de error centimétrico y
subcentimétrico) por posicionamiento diferencial (usando, como mínimo,
un equipo base y otro móvil o “rover”) es necesario conocer las
coordenadas precisas de la base. Dichas coordenadas pueden calcularse
usando observaciones de larga duración; una semana de observaciones
reduce el error al milímetro. Para esto, se usa software de escritorio
(e.g. GAMIT/GLOBK) o servicios en línea. En esta práctica, te facilitaré
las coordenadas de las bases que opero ya calculadas por un servicio en
línea, por lo que no tendrás que obtenerlas, PEEEEERO, tendrás que
transformarlas de ITRF a WGS.

Aclarar que, para fines de reproducibilidad, necesito dejar documentado
cómo obtuve las coordenadas de la base (algunos detalles no te servirán
para la práctica, pero debes conocerlos). Igualmente, te explico a
continuación cómo las bases que opero transmiten mensajes de corrección
en tiempo real para RTK.

Para hacer RTK, que es el mecanismo por el cual obtuvimos soluciones
precisas en el terreno, usamos mensajes RTCM, los cuales son
transmitidos por un caster NTRIP (busca y aprende sobre estos
protocolos). Como cualquier *streaming* NTRIP, la coordenada de la base
se transmite en uno de los mensajes RTCM (aprende sobre los
identificadores numéricos de los mensajes RTCM). El servicio de
transmisión que uso es rtk2go.com, un caster NTRIP basado en SNIP (busca
SNIP y aprende sobre dicho software).

### Base u-blox

La “base u-blox” usa un módulo u-blox ZED-F9P (archivo extensión `.ubx`
en la carpeta “bases”). Estos son los detalles de la solución obtenida
por PPP:

- Si algún día necesitaras hacer RTK, la base transmite en rtk2go.com.
  Estos son los detalles:
  - Usuario: pon una dirección de correo válida y a la que puedas
    acceder
  - Password: no requiere
  - URL: rtk2go.com
  - Puerto: 2101
  - Punto de montaje: `geofis_ovni`
- Si visitas esta ruta, podrás ver la coordenada en distintos formatos:
  <http://rtk2go.com:2101/SNIP>::MOUNTPT?NAME=geofis_ovni
- Esta base se encuentra en el Distrito Nacional de Santo Domingo, y ha
  estado transmitiendo mensajes RTCM desde 2021. La coordenada de la
  base transmitida a partir de 08/11/2021 (desde aprox 11.02 am hora
  local SD, de ese día) es la siguiente: **18.4682098638889
  -69.9101351555556 10.764** (ver otros formatos abajo).
- Esta coordenada fue generada como una solución PPP usando 7 días de
  observaciones brutas mediante el Sistema Canadiense de Referencia
  Espacial Posicionamiento Puntual Preciso (*Canadian Spatial Reference
  System Precise Point Positioning*, CSRS-PPP) de *Natural Resources
  Canada* (NRCAN) (en lo adelante “CSRS-PPP-NRCAN”) EN MODO FINAL
  (efemérides precisas) (investiga que es CSRS-PPP de NRCAN).
- Dejo a continuación los detalles de la solución obtenida, contenida en
  el archivo “observations.sum”:
  - Referencia: ITRF14 (2021.8)
  - Errores: Sigmas(95%): Latitude=0.0007 m, Longitude=0.0008 m, Ell.
    Height=0.0030 m
  - Coordenada provista por CSRS-PPP-NRCAN en Latitude, Longitude, Ell.
    Height: 18° 28’ 5.55551”, -69° 54’ 36.48656”, 10.7640 m
  - Coordenada provista por CSRS-PPP-NRCAN en XYZ (m): 2078721.2081,
    -5683487.6649, 2007608.4186
- IMPORTANTE: ¡¡¡¡¡NO USAR ESTA COORDENADA PARA POSPROCESAR EN RTKLIB,
  PUES RTKLIB LA NECESITA EN WGS84. PARTE DE LA PRÁCTICA CONSISTE EN
  APRENDER A TRANSFORMAR ESTA COORDENADA A WGS84!!!!!

### Base Unicore

La “base Unicore” usa un módulo UM980 (archivo extensión `.log` en la
carpeta “bases”). Estos son los detalles de la solución obtenida:

- Si algún día necesitaras hacer RTK, la base transmite en rtk2go.com.
  Estos son los detalles:
  - Usuario: pon una dirección de correo válida y a la que puedas
    acceder
  - Password: no requiere
  - URL: rtk2go.com
  - Puerto: 2101
  - Punto de montaje: `geofis_mbase`
- Si visitas esta ruta, podrás ver la coordenada en distintos formatos:
  <http://rtk2go.com:2101/SNIP>::MOUNTPT?NAME=geofis_mbase
- Esta base se encuentra en el Distrito Nacional de Santo Domingo, y ha
  estado transmitiendo mensajes RTCM desde finales de 2024. La
  coordenada de la base transmitida a partir de 19/11/2024 es la
  siguiente: **18.45964924° -69.91668732° -11.582** (ver otros formatos
  abajo).
- Esta coordenada fue generada como una solución PPP usando 2 días de
  observaciones brutas mediante el Sistema Canadiense de Referencia
  Espacial Posicionamiento Puntual Preciso (*Canadian Spatial Reference
  System Precise Point Positioning*, CSRS-PPP) de *Natural Resources
  Canada* (NRCAN) (en lo adelante “CSRS-PPP-NRCAN”) EN MODO Ultra-Rapid
  (efemérides no precisas) (investiga los tipos de soluciones de
  CSRS-PPP-NRCAN).
- Dejo a continuación los detalles de la solución obtenida, contenida en
  el archivo “SALIDA.24O”:
  - Referencia: ITRF20 (2024.9)
  - Errores: Sigmas(95%): Latitude=0.002 m, Longitude=0.002 m, Ell.
    Height=0.006 m
  - Coordenada provista por CSRS-PPP-NRCAN en Latitude, Longitude, Ell.
    Height: 18° 27’ 34.73725” -69° 55’ 0.07435”, -11.582 m
  - Coordenada provista por CSRS-PPP-NRCAN en XYZ (m): 2078167.0183,
    -5683987.2824, 2006702.5783
- IMPORTANTE: ¡¡¡¡¡NO USAR ESTA COORDENADA PARA POSPROCESAR EN RTKLIB,
  PUES RTKLIB LA NECESITA EN WGS84. PARTE DE LA PRÁCTICA CONSISTE EN
  APRENDER A TRANSFORMAR ESTA COORDENADA A WGS84!!!!!

## Transforma

- Usa el servicio de Posicionamiento Horizontal Dependiente de la Fecha
  (Horizontal Time-Dependent Positioning, HTDP) del Servicio Geodésico
  Nacional de Estados Unidos (NGS, NOAA) para convertir la coordenada de
  la base a WGS84.

Conserva dicha coordenada, pues es la que deberás usar en RTKLib-Ex para
generar las soluciones fijas por posproceso.

# Ejercicio 3. Genera soluciones fijas, evalúa el resultado

## Genera soluciones fijas

Usa RTKLib-Ex para hacer posproceso. Si tienes otro software, mientras
sea de código abierto, úsalo; de lo contrario, si es de código
privativo, úsalo sólo para tus propios fines, pero no lo uses para
reportar tus resultados en la práctica.

En RTKLib-Ex puedes usar la herramienta RTKPOST (interfaz gráfica) o
rnx2rtkp (línea de comandos). Necesitarás las coordenadas de las bases
**en WGS84** para esto. Para facilitar las cosas, obtén las soluciones
respecto del punto de referencia de la antena (Antenna Reference Point,
ARP), por lo que tendrás que restar los 2 metros de jalón. Esto se lo
debes indicar a RTKPOST (o a rnx2rtkp si usas línea comandos) en el
argumento “Delta-E/N/U (m)”.

Deberás generar cuatro soluciones. Dado que tienes dos colectas de rover
(u-blox y Unicore) y dos bases (u-blox y Unicore), deberás generar las
siguientes soluciones:

- Rover u-blox corregido con base u-blox
- Rover Unicore corregido con base u-blox
- Rover u-blox corregido con base Unicore
- Rover Unicore corregido con base Unicore

**En cada caso deberás realizar dos procesamientos, por lo que te
saldrán dos archivos por cada solución: uno para obtener la solución
fija promediada, y otro para obtener las soluciones de cada época
(“época por época”, *epoch-by-epoch*). En total, generarás 8 archivos**.

## Representa tabular y cartográficamente, compara

### Representación tabular

Muestra los resultados de tus soluciones promediadas en una tabla como
ésta:

<div class="vlines" style="max-width: 560px;">

| Rover   | Base    | Resultado (LLH o XYZ) | Observaciones |
|---------|---------|-----------------------|---------------|
| u-blox  | u-blox  |                       |               |
| Unicore | u-blox  |                       |               |
| u-blox  | Unicore |                       |               |
| Unicore | Unicore |                       |               |

</div>

<br>

O si lo prefieres, en una tabla como ésta (en cada celda, colocarías tus
soluciones promediadas en formato LLH o XYZ):

<div class="vlines" style="max-width: 560px;">

| Rover \\ Base | u-blox | Unicore |
|:-------------:|--------|---------|
|  **u-blox**   |        |         |
|  **Unicore**  |        |         |

</div>

Para gráficar las soluciones, ve a tu cuenta en el servidor RStudio,
sube tus archivos de soluciones `.pos`, carga la función personalizada
`pos_read_violin` usando el comando siguiente …

``` r
devtools::source_url(
paste0('https://raw.githubusercontent.com/geomorfologia-master/',
       'datos-gnss-soluciones-fijas/refs/heads/main/R/funciones.R'))
```

… y evalúa dicha función con este comando …

``` r
archivos_pos <- list.files(pattern = '*.pos')
res <- pos_read_violin(archivos_pos, q_filter=1, export='gpkg',
                       export_path = "ppk_fix.gpkg",
                       export_layer = "ppk_fix",
                       plot_stat = "raw", stat_by = "archivo")
```

… y finalmente imprime el resultado en consola con esto …

``` r
res
```

… lo cual imprimirá un extracto de la tabla y un gráfico de violín. Si
notas mucha dispersión, o si quieres ver cuál de las soluciones presenta
mayor dispersión respecto de la media, podrías correr esto:

``` r
res <- pos_read_violin(archivos_pos, q_filter=1, export='gpkg',
                       export_path = "ppk_fix.gpkg",
                       export_layer = "ppk_fix",
                       plot_stat = "z", stat_by = "archivo")
res
```

El argumento `export='gpkg'` genera un archivo GeoPackage (ppk_fix.gpkg)
que puedes descargar y abrir en QGIS para representar cartográficamente
las soluciones. El argumento `export_layer` permite definir el nombre de
la capa dentro del GeoPackage.

### Compara los resultados

Compara los resultados obtenidos. Dado que obtendrás las soluciones
época por época, con la prueba t de Student no emparejada, compara los
siguientes pares de muestras en cada una de las componentes de
coordenadas (X, Y, Z o Lat, Lon, H) para un nivel de significancia de
0.05:

- Misma base, distintos rovers:
  - Rover u-blox corregido con base u-blox vs rover Unicore corregido
    con base u-blox
  - Rover u-blox corregido con base Unicore vs rover Unicore corregido
    con base Unicore
- Mismo rover, distintas bases:
  - Rover u-blox corregido con base u-blox vs rover u-blox corregido con
    base Unicore
  - Rover Unicore corregido con base u-blox vs rover Unicore corregido
    con base Unicore

#### u-blox con base u-blox vs Unicore con base u-blox

<div class="vlines" style="max-width: 600px;">

| Componente | Media de las diferencias | p   | Sign. o no sign. |
|------------|--------------------------|-----|------------------|
| Lat / X    |                          |     |                  |
| Lon / Y    |                          |     |                  |
| H / Z      |                          |     |                  |

</div>

#### u-blox con base Unicore vs Unicore con base Unicore

<div class="vlines" style="max-width: 600px;">

| Componente | Media de las diferencias | p   | Sign. o no sign. |
|------------|--------------------------|-----|------------------|
| Lat / X    |                          |     |                  |
| Lon / Y    |                          |     |                  |
| H / Z      |                          |     |                  |

</div>

#### u-blox con base u-blox vs u-blox con base Unicore

<div class="vlines" style="max-width: 600px;">

| Componente | Media de las diferencias | p   | Sign. o no sign. |
|------------|--------------------------|-----|------------------|
| Lat / X    |                          |     |                  |
| Lon / Y    |                          |     |                  |
| H / Z      |                          |     |                  |

</div>

#### Unicore con base u-blox vs Unicore con base Unicore

<div class="vlines" style="max-width: 600px;">

| Componente | Media de las diferencias | p   | Sign. o no sign. |
|------------|--------------------------|-----|------------------|
| Lat / X    |                          |     |                  |
| Lon / Y    |                          |     |                  |
| H / Z      |                          |     |                  |

</div>

### Representación cartográfica

Representa tus soluciones promediadas o época por época en RTKPLOT, el
cual te permite mostrar dos archivos de soluciones a la vez. Si lo haces
con QGIS, asigna simbología que permita distinguir qué soluciones estás
mostrando.

# Redacta un informe integrado (IMRaD abreviado)

Prepara un **informe breve con estructura IMRaD** (Introducción,
Métodos, Resultados, Discusión), **usando IA como apoyo si lo deseas**
(para bosquejar texto, tablas o figuras). **Todo debe ser conciso**:
**cada sección debe tener entre 3 y 4 párrafos cortos**. Añade **lista
de referencias** al final y **citas en el texto** (mínimo **3**
fuentes).

> **Uso de IA**: puedes apoyarte en herramientas de IA para redactar,
> resumir o formatear, pero **debes verificar** los contenidos, números
> y citas; **no** entregues texto sin revisar. Añade al final una breve
> nota: *“Se utilizó IA para apoyo en redacción/edición/formato;
> verificación humana realizada.”*

------------------------------------------------------------------------

## 1) Introducción (3–4 párrafos)

- **Contexto y relevancia**: sitúa el problema (posicionamiento GNSS,
  necesidad de soluciones fijas/PPK/RTK) y por qué importa en
  geomorfología.
- **Estado breve del arte**: menciona 1–2 ideas clave de la literatura
  (p. ej., PPP vs. RTK, calidad de observaciones, conversión a WGS84),
  **con citas en el texto**.
- **Objetivo(s) y pregunta(s)**: formula con precisión qué comparas
  (p. ej., “cuatro combinaciones rover–base”) y qué esperas observar.
- **Hipótesis (opcional, breve)**: qué combinación esperas que tenga
  menor dispersión y por qué.

## 2) Métodos (3–4 párrafos)

- **Datos y equipos**: resume rovers/bases, formatos (UBX/LOG → RINEX
  3.04), ventanas temporales, y criterio de calidad (p. ej., filtro
  **Q=1** FIX).
- **Procesamiento**: software (RTKLIB-EX/RTKPOST o rnx2rtkp), parámetros
  esenciales (máscara de elevación, constelaciones, modelo troposférico,
  ARP/altura de jalón, “Delta E/N/U”), y **conversión de coordenadas**
  de ITRF → **WGS84** (HTDP/NGS).
- **Salidas**: especifica que generaste **solución promediada** y
  **época por época** para cada combinación (4 pares rover–base → 8
  archivos).
- **Estadística**: cómo resumiste (medias/dispersiones), cómo
  normalizaste (opcional: **z-score** o **\|x−μ\|**), y **prueba t no
  emparejada** por componente (Lat/X, Lon/Y, H/Z; α=0.05).

## 3) Resultados (3–4 párrafos)

- **Tabla(s) y figura(s) numeradas**: incluye al menos **una tabla** de
  promedios por combinación y **una o dos figuras** (p. ej., **gráficos
  de violín** de E/N/H; **mapa** opcional).

  - **Cita en el texto**: *“…(ver **Fig. 1** y **Tabla 1**)”*.
  - **Leyendas completas** bajo cada figura/tabla: *“**Figura 1.**
    Distribución E/N/H por combinación…”* / *“**Tabla 1.** Medias y
    dispersión…”*.

- **Hallazgos principales**: reporta tendencias concisas (p. ej., menor
  varianza en Unicore@Unicore; sesgo vertical en u-blox@u-blox).

- **Pruebas de hipótesis**: resume **p-valores** por componente y la
  **significancia** (p\<0.05), referenciando una tabla de contraste
  (*ver **Tabla 2***).

- **Calidad y anomalías**: menciona FIX/float, outliers y cualquier
  incidencia (saltos de ciclo, pérdida de solución).

## 4) Discusión (3–4 párrafos)

- **Interpretación**: explica por qué ciertas combinaciones rinden mejor
  (geometría satelital, multipath, configuración, base–rover).
- **Comparación con literatura**: relaciona tus resultados con **al
  menos 2–3** trabajos/fuentes citados.
- **Limitaciones**: ventana temporal, entorno, parámetros de RTKLIB,
  supuestos (WGS84, modelos).
- **Implicaciones y trabajo futuro**: cómo mejorar (más datos, PPP
  final, máscaras, antenas, filtros), y aplicabilidad en geomorfología.

## Referencias

- Incluye **mínimo 3** fuentes y **cítalas en el texto** (elige un
  estilo y sé consistente: **autor–año** o **numérico**).

- Sugerencias típicas (elige las reales que uses):

  - Documentación **RTKLIB-EX** / manual de usuario.
  - **NGS/NOAA HTDP** (conversión o marcos de referencia).
  - Artículos/revisiones sobre **PPP/RTK/PPK**, calidad de observaciones
    GNSS, tratamiento de **RINEX**.

- Formato de ejemplo (autor–año):

  - *Autor, A. (2023).* Título del recurso. Editorial/Revista. DOI/URL.
  - *Autor, B. & Autor, C. (2021).* Título… DOI/URL.
  - *Organismo (2024).* Título… URL.

------------------------------------------------------------------------

### Requisitos de forma

- **Extensión breve**: 4 secciones × **3–4 párrafos** cada una (frases
  compactas; evita relleno).
- **Numeración de figuras y tablas** (Fig. 1, Fig. 2…; Tabla 1, 2…),
  **mención en el texto** y **leyenda clara**.
- **Transparencia**: nota de **uso de IA** (si aplicó) y
  **repositorio/archivos** usados (ruta o enlace).
- **Entregable**: PDF/HTML; incluye tablas/figuras incrustadas y **lista
  de referencias**.

# Referencias
