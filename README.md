# Golden Iris — GoldenEye 007 nativo en PSP

**Golden Iris** es un port **no oficial** de aficionados de *GoldenEye 007* (Nintendo 64, Rare, 1997)
para ejecutarse de forma **nativa** en la PSP: el código del juego compilado
para el procesador de la PSP, sin emulación de N64.

**Versión 1.2** · por **fveralv** ·
[página del proyecto](https://fjveralv.github.io/Goldeniris007-psp/)

> **Aviso.** Es una versión temprana. Desde la 1.1 la GPU de la PSP
> dibuja los escenarios y los objetos; la 1.2 hace lo mismo con los
> guardias y corrige fallos gráficos. Aun así, en una PSP real todavía baja
> de fotogramas en las zonas con muchos enemigos. El trabajo de optimización
> sigue en marcha (ver [Estado](#estado)). Si encuentras un fallo, abre una
> *issue* y adjunta el `ge007.log` que el juego escribe en su carpeta.

Este repositorio **no incluye el juego**. Necesitas tu propia ROM de
GoldenEye 007 (USA).

## Agradecimientos

Este proyecto no existiría sin:

- **[jkdansereau/goldeneye-pc-port](https://github.com/jkdansereau/goldeneye-pc-port)**
  — el port nativo de PC en el que se basa este repositorio: capa de port,
  RSP por software (`fast3d`), conversores de datos y el trabajo de todos sus
  colaboradores. Licencia MIT (`LICENSE-MIT-goldeneye-pc-port`).
- **[z2442/oot-PSP](https://github.com/z2442/oot-PSP)** — el port de
  *Ocarina of Time* a PSP con la misma arquitectura (`fast3d` + `sceGu`), que
  sirvió de referencia de diseño. No se ha copiado código.
- **[n64decomp/007](https://github.com/n64decomp/007)** — la decompilación de
  GoldenEye 007 sobre la que se construye el port de PC.
- **[pspdev](https://github.com/pspdev/pspdev)** (toolchain de PSP) y
  **[PPSSPP](https://www.ppsspp.org)** (emulador usado para las pruebas).

## Contenido

- [Instalación](#instalación)
- [Controles](#controles)
- [Menú de opciones](#menú-de-opciones-select)
- [Estado](#estado)
- [Cómo se ha hecho](#cómo-se-ha-hecho)
- [Compilar](#compilar)
- [Estructura](#estructura)
- [Licencia](#licencia)

## Instalación

Descarga el paquete de la última versión en
[Releases](https://github.com/FJVERALV/Goldeniris007-psp/releases/latest).

**Requisitos**

- **PSP-2000 o superior** (modelos de 64 MB) con firmware personalizado.
  La PSP-1000 no tiene memoria suficiente.
- Tu ROM de **GoldenEye 007 (USA)** en formato `.z64`, SHA-1
  `abe01e4aeb033b6c0836819f549c791b26cfde83`.

**Pasos**

1. Descomprime el paquete.
2. Copia tu ROM `.z64` dentro de `PSP/GAME/GOLDENIRIS/data/` (el nombre da igual).
3. Copia `PSP/GAME/GOLDENIRIS` completa a `PSP/GAME/` de la Memory Stick.
4. Arranca **Golden Iris** desde el menú Juego.

La primera vez, el juego comprueba la ROM y prepara a partir de ella los
modelos y escenarios en el formato de la PSP, mostrando el progreso en
pantalla. Los guarda en `data/` y no lo vuelve a hacer. Esos datos salen de
la ROM, así que no se pueden distribuir: cada uno los genera con su copia. No
hace falta PC, Python ni ningún script.

## Controles

Dos esquemas, que se eligen en el menú de opciones (**SELECT** → Controls):

| Botón | Esquema "PSP" (por defecto) | Esquema "N64" |
|---|---|---|
| Stick analógico | Andar y desplazarse de lado | Andar y girar |
| △ / ✕ | Mirar arriba / abajo | Mirar arriba / abajo |
| □ / ○ | Girar a izquierda / derecha | Desplazarse de lado |
| R | Disparar | Disparar |
| L (mantener) | Apuntar: el stick mueve la mira; ✕ agacharse, △ levantarse, □/○ inclinarse | Igual |
| Cruceta abajo o arriba | Acción / recargar (puertas, objetos) | Igual |
| Cruceta derecha / izquierda | Arma siguiente / anterior | Igual |
| START | Reloj (pausa y opciones del juego) | Igual |
| SELECT | Menú de opciones del port | Igual |

En los menús del juego, el stick o la cruceta mueven el cursor, ✕ o □
aceptan y ○ o △ vuelven atrás.

Los esquemas usan los estilos de control originales del juego (1.2 Solitaire
y 1.1 Honey); el port solo traduce botones de PSP a botones de N64.

## Menú de opciones (SELECT)

Menú propio de la PSP (`port/psp/psp_menu.c`). Se maneja con la cruceta;
✕ acepta, ○ vuelve y SELECT lo cierra.

- **Controls:** esquema de control, invertir la mirada, mover con la
  cruceta, velocidad de giro con botones, zona muerta del stick y el botón de
  cada acción (disparar, apuntar, recargar, cambiar de arma...).
- **Video:** distancia de dibujado, de detalle y de niebla, campo de visión,
  tamaño del HUD, mira siempre visible, color de 32 bits y contador de FPS.
- **Audio:** volumen y mezcla en el Media Engine.
- **Game:** saltar la intro, sin destello al recibir daño, desbloquear todo,
  temblor de la pantalla y registro de rendimiento.
- **Cheats:** todos los trucos del juego, uno a uno o todos a la vez.
- **About** y **Exit Game**.

La configuración se guarda en `data/ge007.ini`, que solo contiene opciones
de PSP.

## Estado

| Tarea | Estado |
|---|---|
| Compilación nativa para PSP (EBOOT) | Completo |
| Todas las misiones cargan y se dibujan | Completo |
| Arranque en PSP real | Completo |
| Controles de PSP (dos esquemas) | Completo |
| Menú de pausa al estilo del port de Ocarina of Time (SELECT) | Completo |
| Sonido en el Media Engine de la PSP | Completo |
| Sonidos correctos y gráficos sin huecos en PSP real | Completo |
| Trucos del juego y botones configurables en el menú de pausa | Completo |
| El juego prepara sus datos desde la ROM la primera vez, sin PC | En pruebas |
| Escenarios y modelos dibujados por la GPU de la PSP | Completo |
| Rendimiento en PSP real (todavía baja con muchos enemigos) | En curso |
| Traducción al español con textos cargados de ficheros (ampliable por cualquiera) | Próximamente |
| Paquetes de texturas de sustitución | Próximamente |
| Multijugador sin pantalla partida (hasta 4 PSP, ad hoc o IP) | En estudio |

## Cómo se ha hecho

Resumen técnico del trabajo hasta la versión 1.2.

### Punto de partida

El port de PC original solo existía para sistemas de **64 bits**. La PSP es un
MIPS de **32 bits** (Allegrex a 333 MHz, 64 MB en los modelos 2000+), sin
memoria virtual, con una GPU de función fija (GE) y una FPU de precisión
simple. La estrategia fue:

1. Conseguir primero un **build de PC de 32 bits** (Linux i686), donde depurar
   es rápido (gdb, capturas, comparación con el build de 64 bits).
2. Pasar después a la PSP con el toolchain **pspdev** y probar en **PPSSPP**
   (con ventana y en modo sin ventana para pruebas automáticas).
3. Por último, probar en PSP real.

### Variante de 32 bits

Con punteros de 4 bytes, las estructuras del juego recuperan la disposición
de N64, así que la conversión de datos se reduce casi siempre a invertir el
orden de bytes. Cambios principales (marcados con `PSP-PORT` en el código):

- Condiciones `#if defined(__x86_64__)` generalizadas a `PORT`.
- Conversores de datos `tools_pc/d43_emit.py` (modelos), `d69_emit.py`
  (fondos y colisiones) y `d88_emit.py` + `d88_propdefs.py` (configuración de
  niveles), parametrizados por `GE_PTR_BYTES` (4 u 8) y `GE_DATA_ROOT`. Con
  `GE_PTR_BYTES=8` la salida es idéntica byte a byte a la original.
- Conversores internos de `port/src/romdata.c` (bancos de audio, fuentes)
  adaptados al tamaño de puntero.
- `sizepropdef()` usa los tamaños de N64 en destinos de 32 bits.
- Solapamiento `ModelSlot`/`AnimModelSlot` derivado de `struct Model`.
- `Gfx` de 8 bytes en 32 bits (`port/shim/PR/gbi.h`).

### Hilos

El núcleo de la PSP solo cambia de hilo por prioridad: los hilos de igual
prioridad no se turnan, y el juego espera en bucle activo como en N64. Cada
hilo recibe una prioridad distinta que conserva el orden original
(planificador > audio > principal), con el temporizador por encima de todos.
Las pilas bajan de 8 MB a 256 KB. Además, cada hilo **desactiva las
excepciones de coma flotante**: en PSP real están activas por defecto y una
división 0/0 mataba el hilo (los emuladores no lo reproducen).

### Renderizador (`port/psp/gfx_psp.cpp`)

`fast3d` transforma y recorta en la CPU y entrega triángulos en espacio de
recorte; el backend los dibuja con `sceGu`:

- Lotes con perspectiva: se envía `(x, y, w)` con una matriz que reconstruye
  la profundidad como `A·w + B` (deducidos de los vértices del lote), lo que
  mantiene la corrección de perspectiva de las texturas.
- Lotes ortográficos (HUD, menús): división en CPU y dibujo en modo 2D.
- Cada fórmula del combinador de color de N64 se asigna a la función de
  textura más parecida de la GE: modular, mezclar entre dos colores según la
  textura (texto, HUD) o color plano. En el modo de dos ciclos el color se
  toma del segundo ciclo.
- Texturas en el formato más pequeño que las guarda sin pérdida (5551, 4444
  u 8888), rellenadas hasta potencia de dos y en el orden "swizzled" que la
  GE lee cinco veces más rápido.

### Controles y menú

- `port/psp/psp_input.c` lee el mando con `sceCtrl` y produce botones de N64;
  `port/src/input.c` le cede el control en PSP.
- El menú de opciones del port de PC (`port/src/optionsoverlay.c`) se abre con
  SELECT, con páginas, textos de ayuda y botones propios de PSP.

### Fallos encontrados por el camino

- **Cuelgue tras el logo de Rare en PSP real:** el conversor de modelos, en
  modo de 32 bits, empaquetaba registros de 2 bytes sin relleno y dejaba
  datos en direcciones que no eran múltiplo de 4. La PSP real falla con una
  lectura desalineada; los emuladores no. Se encontró compilando el build de
  PC de 32 bits con `-fsanitize=alignment` y recorriendo los 21 niveles; el
  conversor ahora alinea cada objeto a 4 bytes (listas de dibujo a 8).
- **Cuelgue en la cámara de introducción** (`bondviewCalcIntroSwirlCamera`):
  desbordamiento de pila del código original, inofensivo con el compilador de
  N64 y fatal con GCC en MIPS. Corregido con `#ifdef PORT` sin cambiar la
  lógica.
- **Bloqueos al arrancar:** hilos de igual prioridad que nunca se turnaban.
- **Pantallas negras:** el camino de "framebuffer de juego" de `fast3d`
  borraba la imagen; en PSP se dibuja directamente al búfer de pantalla.
- **Colores incorrectos** (tinte azulado): en el modo de dos ciclos se tomaba
  como color la fracción de nivel de detalle de la textura.

### Rendimiento (en curso)

- La FPU de la PSP solo es de precisión simple: `sinf`/`cosf` de libultra
  pasan a precisión simple y el build usa
  `-fsingle-precision-constant -fno-math-errno -freciprocal-math`.
- Vsync adaptativo: un fotograma que llega tarde no espera al siguiente
  retrazado.
- **Perfilador integrado** (`port/psp/psp_prof.c`): cada ~2 s el juego escribe
  en `ge007.log` líneas `PROF` con fps, reparto del tiempo de dibujo por
  regiones de código, triángulos y comandos por fotograma y uso de CPU por
  hilo. Se desactiva en el menú (Game → Performance log).
- **Audio en el Media Engine**, el segundo procesador de la PSP, para que el
  sonido no le quite tiempo al juego.
- **Camino propio para la PSP en `fast3d`:** los triángulos van a la GE en
  lotes indexados con vértices ya en su formato; el estado de cada material
  se calcula una vez y se reutiliza (caché de los últimos 64 estados); el
  estado de la GE solo se envía cuando cambia; la GE se pone en marcha cada
  pocos lotes en lugar de con una llamada al sistema por dibujo.
- **Caché de geometría (1.1):** la primera vez que se dibuja una sala del
  nivel o una pieza de un modelo (brazo, pierna, puerta...), sus triángulos
  se guardan ya preparados para la GE con las coordenadas del modelo.
  Después la propia GE los transforma con la matriz de la cámara (y la de la
  pieza, que se vuelve a leer en cada fotograma), y el procesador solo
  comprueba qué trozos se ven. Los trozos pegados a la cámara se recortan en
  el procesador como antes, porque la GE descarta los triángulos que se
  salen demasiado de la pantalla. Una grabación solo se usa mientras el
  contenido de la lista y de sus vértices no cambie y el estado heredado sea
  el mismo, así que la imagen es idéntica a la de antes. En una PSP real las
  salas pasaron de 10–25 ms a 4–5 ms por fotograma.
- **Guardias y articulaciones (1.2):** cada guardia trae su propio color de
  luz, distinto en cada fotograma; ahora se aplica al dibujar, así que una
  sola grabación sirve para todos. Las piezas que unen dos partes del cuerpo
  (con dos matrices) también se guardan. En Dam los triángulos que aún
  preparaba el procesador bajaron de unos 1.400 a menos de 100 por fotograma
  (más el arma), y el peor momento con guardias pasó de unos 14 a unos 18 fps.
- **Fallos corregidos (1.2):** la GE guarda sus matrices con 24 bits, y con
  las coordenadas grandes de los niveles las paredes temblaban junto a la
  cámara al girar; ahora cada trozo se dibuja respecto a su centro. Sus
  coordenadas de pantalla dan la vuelta a los 1024 píxeles, lo que hacía
  aparecer formas sueltas (paredes rojas en Jungle). Además: calcomanías
  que se veían a través de otras superficies, un panel de ascensor que
  faltaba en Silo, colores que pasaban de una pieza a la siguiente y hojas
  tapadas en Jungle.
- Medido en PSP real: aún falta rendimiento en escenas con muchos enemigos;
  ahora el mayor coste es la propia caché y el arma en primera persona.

### Cómo se ha probado

- **Build de PC de 32 bits:** barrido de los 21 niveles con capturas.
- **PPSSPP sin ventana:** el juego guarda capturas de fotogramas concretos
  (`psp_dump.txt`) y admite pulsaciones guionizadas (`psp_input.txt`), así
  que menús, controles y misiones se prueban sin intervención.
- **Alineación:** build de 32 bits con `-fsanitize=alignment`, sin avisos en
  arranque, menús y los 21 niveles.
- **PSP real:** arranque, menús y misiones; rendimiento medido con el
  perfilador integrado.

**Datos del juego**: los crea el propio juego desde la ROM en el primer
arranque.

## Licencia

- Trabajo propio de este port: **Apache License 2.0** (`LICENSE`).
- Lo heredado del port de PC: **MIT** (`LICENSE-MIT-goldeneye-pc-port`).
- Detalle de componentes y de los archivos de la decompilación: `NOTICE`.

Proyecto de aficionados sin afiliación. GoldenEye 007 es marca de sus
respectivos propietarios. No se distribuye ninguna copia del juego ni datos
extraídos de él.
