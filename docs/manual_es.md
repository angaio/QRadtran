# QRadtran — Manual de instalación y uso

**Versión** 0.3.0　　**Fecha** 2026-09-19　　**Editor** Wuhan WaveFront Co., Ltd.

Este manual también está disponible en 简体中文, English, Русский, 日本語, 한국어, Français y Deutsch; tras la instalación, todas las versiones se encuentran en la carpeta `doc\` del directorio de instalación.

QRadtran es un software de cálculo de transmitancia y radiancia atmosféricas para ingenieros de sistemas infrarrojos y electroópticos. En Windows, mediante una interfaz gráfica, calcula la transmitancia espectral en trayectos inclinados y horizontales, la radiancia del trayecto y del cielo y la radiancia aparente de un objetivo. Admite la comparación de varias curvas, barridos de parámetros que generan tablas de consulta (CSV / HDF5), una herramienta de línea de comandos y una interfaz Python. El núcleo de cálculo es el software libre libRadtran, que se instala junto con el programa; no requiere configuración adicional.

---

## Índice

1. [Requisitos del sistema](#1-requisitos-del-sistema)
2. [Instalación](#2-instalación)
3. [Primera ejecución](#3-primera-ejecución)
4. [Vista general de la interfaz](#4-vista-general-de-la-interfaz)
5. [Panel de parámetros en detalle](#5-panel-de-parámetros-en-detalle)
6. [Flujos de trabajo típicos](#6-flujos-de-trabajo-típicos)
7. [Barridos de parámetros y tablas de consulta](#7-barridos-de-parámetros-y-tablas-de-consulta)
8. [Herramienta de línea de comandos](#8-herramienta-de-línea-de-comandos)
9. [Interfaz Python](#9-interfaz-python)
10. [Configuración](#10-configuración)
11. [Método de cálculo y precisión](#11-método-de-cálculo-y-precisión)
12. [Preguntas frecuentes](#12-preguntas-frecuentes)
13. [Desinstalación](#13-desinstalación)
14. [Archivos y carpetas](#14-archivos-y-carpetas)

---

## 1. Requisitos del sistema

| Elemento | Requisito |
|---|---|
| Sistema operativo | Windows 10 / 11, 64 bits |
| Procesador | 4 núcleos o más. El núcleo ejecuta varios procesos en paralelo; cuantos más núcleos, más rápidos son los cálculos por lotes |
| Memoria | 8 GB o más |
| Disco | Unos 1,2 GB tras la instalación (incluye las tablas espectrales de las tres resoluciones). Se recomienda SSD: los cálculos en resolución fina leen muchos datos |
| Pantalla | 1600 × 900 o superior |
| Otros | No se necesita Python, MSYS2 ni WSL; el runtime de VC++ está incluido |

La interfaz Python es opcional y requiere un Python 3.10 de 64 bits del sistema con numpy.

---

## 2. Instalación

1. Haga doble clic en `QRadtran-0.3.0-Setup-x64.exe`.
2. Elija el idioma del instalador (简体中文, English, Русский, 日本語, 한국어, Français, Deutsch, Español). El instalador registra el idioma elegido como idioma de la interfaz del programa; puede cambiarse después en Herramientas → Configuración.
3. Lea las notas sobre licencias de terceros y pulse Siguiente.
4. Elija la carpeta de instalación, por defecto `C:\Program Files\QRadtran`. La ruta puede contener acentos, pero no se recomienda una unidad de red.
5. Tareas adicionales:
   - **Crear un acceso directo en el escritorio**: marcado por defecto.
   - **Añadir la carpeta de instalación al PATH**: desmarcado por defecto. Si se marca, `qradtran-cli` puede escribirse en cualquier símbolo del sistema.
6. Pulse Instalar y espere a que se copien los archivos (de 1 a 3 minutos).
7. En la última página puede iniciar el programa o abrir este manual.

El instalador requiere derechos de administrador. Para instalar solo para el usuario actual, elija «Instalar solo para mí» en la solicitud de privilegios; la carpeta por defecto pasa a ser `%LOCALAPPDATA%\Programs\QRadtran`.

### Uso portátil

El instalador es opcional: copie la carpeta de distribución completa `QRadtran\` a cualquier lugar y ejecute `QRadtran.exe` desde ella. Todas las dependencias están dentro de la carpeta; borrarla desinstala el programa.

---

## 3. Primera ejecución

Tras iniciar, mire la barra de estado en la esquina inferior derecha de la ventana:

```
Núcleo libradtran 2.0.6 | 7 procesos
```

Esta línea indica que el núcleo de cálculo está listo; «procesos» es el número de procesos del núcleo que se ejecutan a la vez. Si aparece «Núcleo no disponible», abra Herramientas → Configuración y compruebe la ruta del núcleo, véase la sección 10.

**Idioma de la interfaz**: por defecto el programa sigue el idioma de visualización de Windows y admite chino simplificado, inglés, ruso, japonés, coreano, francés, alemán y español. Elija un idioma en Herramientas → Configuración → Idioma de la interfaz y reinicie; también puede forzarse desde la línea de comandos, por ejemplo `QRadtran.exe --lang es`.

Para la primera prueba, parta de un escenario predefinido:

1. Menú Archivo → Cargar escenario predefinido…, elija `mls_ground_to_air_3_5um.json` (atmósfera de latitud media en verano, observación desde tierra de un objetivo a 10 km de altitud, 3–5 µm).
2. Pulse Ejecutar en la barra de herramientas o presione F5.
3. Aparece una tarea en el panel Tareas y registro de la parte inferior; cuando la barra de progreso termina, las curvas se muestran en el centro: azul para la transmitancia (eje izquierdo), colores cálidos para las magnitudes radiométricas (eje derecho). Todo el proceso dura unos 7 segundos.

---

## 4. Vista general de la interfaz

```
┌─────────────────────────────────────────────────────────────────────┐
│ Archivo  Calcular  Ver  Herramientas  Ayuda                         │
│ [Nuevo][Abrir][Guardar] │ [▶Ejecutar][■Detener][Barrido] │ [Exportar][Zoom] │ Eje X  Densidad │
├──────────────┬───────────────────────────────────────┬──────────────┤
│  Parámetros  │  Espectros / Tabla de resultados      │  Curvas      │
│  ▸ Atmósfera │                                       │  ☑ Transmit. │
│  ▸ Geometría │        (gráfico de dos ejes)          │  ☑ Radiancia │
│  ▸ Aerosol   │                                       │  color       │
│  ▸ Suelo     │                                       │              │
│  ▸ Espectral │                                       │              │
│  ▸ Salidas   │                                       │              │
│  [Calcular]  │  x = 4,35 µm  Transmitancia 0,812 …   │              │
├──────────────┴───────────────────────────────────────┴──────────────┤
│  Tareas y registro: lista (estado / progreso / tiempo) │ registro   │
├─────────────────────────────────────────────────────────────────────┤
│ Barra de estado: lectura del cursor            Núcleo libradtran 2.0.6 │
└─────────────────────────────────────────────────────────────────────┘
```

Los tres paneles acoplables pueden arrastrarse, cerrarse y reorganizarse; un panel cerrado se restaura desde el menú Ver o arrastrando el borde.

### Barra de herramientas

| Botón | Función |
|---|---|
| Nuevo / Abrir / Guardar proyecto | Un proyecto es un archivo JSON con todos los parámetros y los resultados calculados |
| Cargar escenario predefinido | Tres escenarios de ejemplo incluidos; sus nombres siguen el idioma de la interfaz |
| Ejecutar (F5) | Calcula con los parámetros actuales; el resultado se añade al gráfico sin sustituir los anteriores |
| Detener todo | Cancela todas las tareas en ejecución |
| Barrido de parámetros / LUT | Abre el cuadro de diálogo de cálculo por lotes, véase la sección 7 |
| Exportar imagen | Guarda el gráfico actual como PNG / PDF / JPEG |
| Restablecer zoom (Inicio) | Devuelve el gráfico al rango completo |
| Eje X | Unidad del eje horizontal: µm / nm / cm⁻¹; efecto inmediato |
| Densidad espectral | Unidad de densidad espectral de las magnitudes radiométricas: por µm / por nm / por cm⁻¹ |

### Interacción con el gráfico

| Acción | Efecto |
|---|---|
| Rueda del ratón | Zoom centrado en el cursor |
| Arrastrar | Desplazar |
| Doble clic | Restablecer |
| Mover el ratón | La línea inferior y la barra de estado muestran el valor de cada curva bajo el cursor |
| Casilla del panel Curvas | Mostrar / ocultar una curva |
| Doble clic en la muestra de color | Cambiar el color de la curva |
| Clic derecho en un nombre de resultado | Exportar ese resultado como CSV o eliminarlo |

La transmitancia se dibuja en el eje izquierdo (0 a 1); la radiancia y la irradiancia en el eje derecho, de modo que pueden superponerse.

---

## 5. Panel de parámetros en detalle

### 5.1 Atmósfera

| Elemento | Descripción |
|---|---|
| Atmósfera estándar | Tropical, latitud media verano, latitud media invierno, subártico verano, subártico invierno, estándar EE. UU. 1976 (las seis atmósferas AFGL) o perfil personalizado |
| Importar perfil (CSV) | Importa un sondeo propio de presión/temperatura/humedad, véase abajo |
| Columna de vapor de agua | Si se marca, el agua precipitable total se ajusta a los milímetros indicados; si no, se usa el valor de la atmósfera estándar |
| Columna de ozono | Igual, en DU |
| Relación de mezcla de CO₂ | ppm, 420 por defecto |

**Formato del archivo de perfil personalizado**: CSV o texto separado por espacios con una línea de cabecera. Columnas obligatorias: altitud, presión, temperatura y una columna de humedad. Nombres de columna reconocidos (sin distinguir mayúsculas):

| Magnitud | Nombres de columna | Unidad |
|---|---|---|
| Altitud | z, alt, altitude, height | km (`z_m` para metros) |
| Presión | p, pres, pressure | hPa (`p_pa` para Pa) |
| Temperatura | t, temp, temperature | K (`t_c` para °C) |
| Humedad (una de ellas) | rh; q; h2o / h2o_vmr | humedad relativa %; humedad específica g/kg; relación de mezcla en volumen ppmv |
| Ozono (opcional) | o3, o3_vmr | ppmv |

Ejemplo:

```
z_km,p_hpa,t_k,rh
0.0,1013.0,295.0,70
1.0,900.0,289.0,65
2.0,795.0,283.0,55
5.0,540.0,262.0,40
10.0,265.0,223.0,20
```

Por encima del perfil importado, los niveles se completan con la atmósfera estándar seleccionada.

### 5.2 Geometría

| Tipo de trayecto | Significado | Parámetros |
|---|---|---|
| Trayecto inclinado (observador → objetivo) | Observador y objetivo a distintas altitudes | Altitud del observador, altitud del objetivo, ángulo cenital de observación |
| Trayecto horizontal | Observador y objetivo a la misma altitud | Altitud del observador, longitud del trayecto horizontal |
| Columna completa, vertical | Del suelo al tope de la atmósfera | Ninguno |
| Solo radiancia del cielo | Sin objetivo; radiancia del cielo en una dirección dada | Altitud del observador, ángulo cenital, acimut |

Convenciones angulares:

- **Ángulo cenital de observación**: 0° hacia arriba, 90° horizontal, 180° hacia abajo. Debe ser menor que 90° si el objetivo está por encima del observador y mayor que 90° en caso contrario; si no, el panel señala un error.
- **Acimut de observación**, **acimut solar**: 0° al norte, sentido horario.
- **Ángulo cenital solar**: solo afecta a las magnitudes radiométricas de la banda solar, no a la transmitancia.
- **Elevación del terreno**: altitud de la base del perfil atmosférico; las altitudes del observador y del objetivo son sobre el nivel del mar.

Para ángulos cenitales entre 85° y 95°, el programa pasa a una integración de la extinción por capas para trayectos casi horizontales y emite un aviso de precisión.

### 5.3 Aerosol y nube

| Elemento | Descripción |
|---|---|
| Tipo de aerosol | Ninguno, rural, urbano, marítimo, troposférico (modelos de Shettle) o mezcla OPAC |
| Visibilidad | Visibilidad horizontal en superficie en km; fija la carga de aerosol de la capa límite |
| Estación | Perfil primavera/verano u otoño/invierno |
| Aerosol estratosférico | Fondo / volcánico moderado / volcánico alto / volcánico extremo |
| Nombre de la mezcla OPAC | Se usa con OPAC, p. ej. continental_average, maritime_clean, urban, desert |
| Capa de nube única | Nube de agua o de hielo con base, techo, contenido de agua y radio efectivo |

### 5.4 Suelo

| Elemento | Descripción |
|---|---|
| Albedo del suelo | Banda solar, 0 a 1 |
| Temperatura del suelo | Temperatura de emisión del suelo en el infrarrojo térmico |
| Emisividad del suelo | Infrarrojo térmico, 0 a 1 |

### 5.5 Espectral

| Elemento | Descripción |
|---|---|
| Unidad espectral | µm / nm / cm⁻¹. Los límites se convierten al cambiar de unidad |
| Desde / hasta | Rango espectral. El programa envía por separado al núcleo la banda solar (≤ 5 µm) y la térmica (≥ 2,5 µm) y une los resultados |
| Paso de salida | Espaciado de la malla de salida; 0 usa directamente la malla de longitudes de onda representativas del núcleo |
| Resolución del modelo de banda | Grueso 15 cm⁻¹ / medio 5 cm⁻¹ / fino 1 cm⁻¹ / LOWTRAN 20 cm⁻¹. Más fino es más preciso y más lento |
| Solucionador | DISORT (dispersión múltiple, por defecto) o dos flujos. Cuando se necesita radiancia, el programa cambia a DISORT automáticamente |
| Flujos DISORT | 16 por defecto; aumente a 32 en casos con fuerte dispersión (aerosol denso, nube) |

Tiempos orientativos (PC de oficina de 4 núcleos): 3–5 µm tierra-aire, grueso ≈ 7 s, medio ≈ 30 s; 8–12 µm horizontal 1 km ≈ 2 s.

### 5.6 Salidas

| Elemento | Significado | Unidad |
|---|---|---|
| Transmitancia del trayecto | Transmitancia espectral del observador al objetivo | adimensional |
| Radiancia del trayecto | Radiancia emitida y dispersada hacia la línea de visión por la atmósfera a lo largo del trayecto | W/(m²·sr·cm⁻¹), conmutable |
| Radiancia del cielo | Radiancia del cielo vista desde el observador a lo largo de la línea de visión | ídem |
| Irradiancia directa / difusa descendente / ascendente | Irradiancias a la altitud del observador | W/(m²·cm⁻¹) |
| Radiancia aparente del objetivo | Para la temperatura y emisividad del objetivo dadas, radiancia vista por el observador tras la atenuación atmosférica más la radiancia del trayecto; también se produce la radiancia incidente en el objetivo | W/(m²·sr·cm⁻¹) |

---

## 6. Flujos de trabajo típicos

### 6.1 Cálculo único y comparación

1. Ajuste los parámetros y pulse Ejecutar.
2. Cambie uno o dos parámetros (por ejemplo la visibilidad de 23 km a 5 km) y vuelva a ejecutar. El nuevo resultado se añade al mismo gráfico con otro color y el nombre del escenario en la leyenda.
3. Marque o desmarque en el panel Curvas de la derecha las curvas que quiera comparar.
4. Pase a la pestaña Tabla de resultados para ver los valores; las unidades de eje y de densidad espectral siguen la barra de herramientas.

### 6.2 Guardar y restaurar

Archivo → Guardar proyecto almacena los parámetros actuales y todos los resultados en un archivo JSON. Al abrirlo después, las curvas se restauran sin recalcular.

### 6.3 Exportación

- Gráfico: Archivo → Exportar imagen o el botón de la barra de herramientas, PNG / PDF / JPEG.
- Datos: clic derecho en un resultado del panel Curvas → Exportar este resultado como CSV. El CSV usa las unidades de eje y de densidad espectral seleccionadas en la barra de herramientas; la cabecera indica las unidades.

### 6.4 Escenario de detección de objetivos

Para obtener la radiancia aparente de un objetivo en el detector:

1. Elija Trayecto inclinado en Geometría e introduzca las altitudes del observador y del objetivo y el ángulo cenital.
2. Marque Radiancia aparente del objetivo en Salidas e introduzca la temperatura y la emisividad del objetivo.
3. La ejecución produce cuatro curvas: transmitancia, radiancia del trayecto, radiancia aparente del objetivo y radiancia incidente en el objetivo.
4. Reste el resultado Solo radiancia del cielo de la misma geometría a la radiancia aparente del objetivo para obtener el contraste de radiancia objetivo/fondo.

---

## 7. Barridos de parámetros y tablas de consulta

Calcular → Barrido de parámetros / LUT… parte del panel de parámetros actual, calcula cada combinación (producto cartesiano) de los valores elegidos y escribe una tabla de consulta.

### Elementos del cuadro de diálogo

| Elemento | Descripción |
|---|---|
| Dimensiones del barrido | Un parámetro por fila. Los parámetros se escriben como punteros JSON; la lista desplegable ofrece los habituales. Los valores son una lista `1, 2, 5, 10` o un rango `0:10:2` (inicio:fin:paso); los parámetros de texto se escriben por su nombre, p. ej. `Rural, Urban` |
| Columna | Nombre de la coordenada de esa dimensión en el archivo de salida; por defecto el nombre del parámetro |
| Salidas | Si no se marca nada, se escriben todas las magnitudes del escenario |
| Promedios por banda | Opcional; promedio e integral por bandas en µm, escritos además |
| Archivo de salida | La extensión determina el formato: `.h5` para HDF5, `.csv` para una tabla larga |
| Reanudar | Omite las combinaciones ya calculadas al volver a ejecutar tras una interrupción |

El cuadro de diálogo muestra el número total de combinaciones y el número de ejecuciones del núcleo por combinación. El cálculo puede cancelarse; la parte terminada se conserva.

### Estructura del archivo HDF5

```
/<Magnitud>                   [dim1, …, dimN, espectral]  datos, unidad en el atributo "units"
/bands/<Magnitud>_average     [dim1, …, dimN, banda]      promedio por banda
/bands/<Magnitud>_integral    [dim1, …, dimN, banda]      integral por banda
/coords/wavenumber            [espectral]                 cm⁻¹
/coords/wavelength_um         [espectral]                 µm
/coords/<columna>             [valores]                   coordenada de cada dimensión (número o cadena)
/coords/band                  [banda]                     nombres de banda
/filled                       [dim1, …, dimN]             1 = combinación calculada
Atributos raíz: kernel, kernel_version, created_utc, base_scenario (JSON del escenario base), plan (JSON del plan de barrido)
```

Lectura en Python (requiere h5py):

```python
import h5py
with h5py.File("lut.h5") as f:
    T = f["Transmittance"][...]            # forma [dim1, …, espectral]
    nu = f["coords/wavenumber"][...]
    tgt = f["coords/target_km"][...]
```

`pyqradtran\read_lut_h5.py` en la carpeta de instalación es un script de visualización listo para usar.

### Tabla larga CSV

Una fila por (valores de dimensión…, número de onda, magnitud, valor); los promedios por banda van a `<archivo>.bands.csv`. Adecuada para tablas dinámicas de Excel o pandas.

---

## 8. Herramienta de línea de comandos

`qradtran-cli.exe` está en la carpeta de instalación; con «Añadir al PATH» marcado funciona desde cualquier lugar.

```
qradtran-cli example --out case.json         escribir un archivo de escenario de plantilla
qradtran-cli validate case.json              comprobar los parámetros
qradtran-cli plan case.json                  listar las ejecuciones del núcleo previstas, sin calcular
qradtran-cli run case.json --out out.csv     calcular y escribir CSV (--unit um|nm|cm-1  --density cm-1|nm|um)
qradtran-cli run case.json --out out.json    escribir JSON (con metadatos)
qradtran-cli batch-example --out plan.json   escribir una plantilla de plan de barrido
qradtran-cli batch plan.json --out lut.h5    cálculo por lotes (.h5 o .csv; --resume para reanudar)
qradtran-cli kernel-info                     mostrar el estado del núcleo y las resoluciones disponibles
```

Códigos de salida: 0 éxito, 1 error de argumentos, 2 validación fallida, 3 fallo del núcleo. Los archivos de escenario y los archivos de proyecto de la interfaz son intercambiables.

---

## 9. Interfaz Python

`pyqradtran\` en la carpeta de instalación es un paquete Python compilado para Python 3.10 de 64 bits. Añada la carpeta de instalación al `PYTHONPATH` antes de usarlo:

```bat
set PYTHONPATH=C:\Program Files\QRadtran
python
```

```python
import pyqradtran as qr

s = qr.Scenario.standard("MidlatitudeSummer")
s.geometry.mode = "SlantPath"
s.geometry.observer_alt_km = 0.0
s.geometry.target_alt_km = 10.0
s.geometry.view_zenith_deg = 30.0
s.aerosol.type = "Rural"
s.aerosol.visibility_km = 23.0
s.spectral.set_range(3, 5, unit="Micrometer", step=0.01, resolution="Coarse")
s.outputs.transmittance = True
s.outputs.path_radiance = True

r = qr.run(s)
nu = r.wavenumber                   # matriz numpy, cm⁻¹
T = r["Transmittance"]
L = r.series("PathRadiance", per="Micrometer")     # convertido a por µm
print(r.band_average("Transmittance", 3.0, 5.0))  # {'average':…, 'integral':…}
r.to_csv("out.csv", unit="Micrometer")

# barrido de parámetros
plan = qr.BatchPlan()
plan.base = s
plan.add_dimension("/geometry/target_alt_km", [1, 2, 5, 10], "tgt_km")
plan.add_band("MWIR", 3.0, 5.0)
qr.batch(plan, "lut.h5")
```

El ejemplo completo es `pyqradtran\python_quickstart.py`. Los enlaces comparten la biblioteca central y el núcleo con la interfaz gráfica, así que los resultados son idénticos.

---

## 10. Configuración

Herramientas → Configuración:

| Elemento | Descripción |
|---|---|
| uvspec.exe / directorio de datos de libRadtran | Ubicación del núcleo, por defecto `kernels\libradtran` bajo la carpeta de instalación. Normalmente no hay que cambiarlo |
| Detectar núcleo | Muestra la versión del núcleo y las resoluciones instaladas |
| Máx. procesos de núcleo simultáneos | 0 = automático (núcleos de CPU menos 1). Reduzca si falta memoria |
| Tiempo límite por ejecución del núcleo | Segundos. Resolución fina, bandas anchas o muchos flujos pueden necesitar un valor mayor |
| Conservar el directorio de trabajo si falla | Conserva los archivos de entrada y salida del núcleo para diagnóstico cuando falla; la ubicación se escribe en el registro |
| Conservar también el directorio de trabajo si tiene éxito | Solo para diagnóstico, normalmente desactivado |
| Idioma de la interfaz | Seguir el sistema o fijar uno de los ocho idiomas; se aplica tras reiniciar |

Los directorios de trabajo se crean en `%LOCALAPPDATA%\WaveFront\QRadtran\runs\`.

---

## 11. Método de cálculo y precisión

- **Núcleo**: uvspec de libRadtran 2.0.6, ejecutado como proceso independiente; absorción molecular con el modelo de banda de longitudes de onda representativas REPTRAN (basado en HITRAN 2004 y posteriores), dispersión múltiple con DISORT.
- **Transmitancia en trayecto inclinado**: en la banda solar, por el cociente de las irradiancias directas en las dos altitudes; en la banda térmica, por el cociente de las diferencias de radiancia de dos ejecuciones con la temperatura del suelo perturbada.
- **Trayectos horizontales y casi horizontales**: el coeficiente de extinción se deriva de la transmitancia vertical de la capa y se integra a lo largo del trayecto esférico.
- **Radiancia del trayecto**: radiancia en el observador menos la radiancia en el objetivo multiplicada por la transmitancia.
- **Precisión**: contrastada con MODTRAN4 en las ventanas atmosféricas de onda larga y onda media; en 6 de 8 casos la transmitancia media de la ventana difiere en menos de 0,04, el mejor caso en 0,003. Dentro de las bandas de absorción fuerte, las dos generaciones de datos espectroscópicos difieren sistemáticamente. Véase el informe de validación incluido con el programa.
- **Resoluciones REPTRAN**: la fina de 1 cm⁻¹ es de la misma clase que el modelo de banda de 1 cm⁻¹ de MODTRAN; la gruesa de 15 cm⁻¹ sirve para estimaciones rápidas.

---

## 12. Preguntas frecuentes

**La barra de estado muestra «Núcleo no disponible» al iniciar.**
Se movió la carpeta de instalación o un antivirus eliminó `kernels\libradtran`. Compruebe las rutas en Configuración; reinstale si es necesario.

**El botón Ejecutar está en gris.**
Bajo el panel de parámetros aparece un mensaje de validación en rojo; corrija lo que indica. Causas frecuentes: trayecto inclinado con el objetivo a la misma altitud que el observador; ángulo cenital contradictorio con la posición del objetivo arriba/abajo; límites espectrales iguales.

**El cálculo falla y el registro dice «Netcdf error … reptran_…_medium».**
Faltan las tablas de datos de la resolución elegida. El instalador completo contiene las tres; con un paquete reducido use Grueso o añada los datos.

**La transmitancia en el infrarrojo térmico es toda cero.**
Ocurría en versiones anteriores a 0.2.0 con el solucionador de dos flujos; corregido: se usa DISORT automáticamente cuando se requiere radiancia.

**El cálculo es lento.**
Resolución fina con banda ancha y 32 flujos es lo más lento. Ajuste los parámetros con resolución gruesa y genere el gráfico final con resolución fina; en los lotes use todos los procesos disponibles.

**Los valores del eje derecho son difíciles de interpretar.**
El eje derecho muestra magnitudes radiométricas; la unidad sigue el ajuste Densidad espectral de la barra de herramientas: por cm⁻¹, por nm o por µm. Las tablas y el CSV usan el mismo ajuste.

**La interfaz aparece en inglés.**
Cuando el idioma de visualización de Windows no está entre los admitidos, el programa vuelve al inglés. Elija el idioma en Herramientas → Configuración → Idioma de la interfaz y reinicie.

**Python informa «DLL load failed».**
`PYTHONPATH` debe apuntar a la propia carpeta de instalación (el nivel que contiene `qradtran_core.dll` y `Qt6Core.dll`), no a la subcarpeta `pyqradtran`; Python debe ser 3.10 de 64 bits.

**¿Puedo usar mi propio libRadtran?**
Sí. En Configuración, apunte uvspec.exe y el directorio de datos a su versión; debe ser 2.0.x.

---

## 13. Desinstalación

Desinstale QRadtran desde Configuración → Aplicaciones o ejecute Desinstalar QRadtran desde el menú Inicio. La desinstalación conserva los archivos de proyecto del usuario y los registros de ejecución en `%LOCALAPPDATA%\WaveFront\QRadtran`; bórrelos manualmente si lo desea.

En la variante portátil basta con borrar la carpeta.

---

## 14. Archivos y carpetas

```
QRadtran\
├── QRadtran.exe              interfaz gráfica
├── qradtran-cli.exe          línea de comandos
├── qradtran_core.dll         biblioteca central
├── Qt6*.dll, platforms\, styles\, translations\ …   runtime de Qt
├── hdf5.dll, msvcp140.dll, vcruntime140*.dll        HDF5 y runtime de VC++
├── kernels\libradtran\
│   ├── bin\uvspec.exe        núcleo de cálculo (compilación MinGW) y su runtime
│   ├── data\                 perfiles atmosféricos, aerosoles, nubes, espectros solares y tablas REPTRAN
│   ├── source\               archivo de fuentes de libRadtran, parches y script de compilación (requisito GPL)
│   └── VERSION
├── data\presets\             escenarios de ejemplo
├── pyqradtran\               enlaces Python y ejemplos
├── licenses\                 textos de licencias de terceros
└── doc\                      este manual (md y html en ocho idiomas)
```

Los archivos de proyecto (`.json`) y los CSV / HDF5 / imágenes exportados se guardan donde el usuario elija.

---

## Soporte

Wuhan WaveFront Co., Ltd.　　WeChat: angaio
