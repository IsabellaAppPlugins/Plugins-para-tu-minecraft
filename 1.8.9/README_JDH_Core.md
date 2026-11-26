# JDH Core --- Hunger Games

Plugin de Juegos del Hambre para **Minecraft Java 1.8.9 con Paper**. JDH
Core permite registrar arenas, configurar sus límites y puntos de
aparición, gestionar partidas y añadir sistemas de cofres, monstruos y
desafíos.

> **Versión:** 0.1.0\
> **Nombre del plugin:** JDH\
> **Autor indicado en el plugin:** JDH\
> **Plataforma objetivo:** Paper 1.8.9

## Características

-   Registro y selección de mundos como arenas.
-   Configuración del centro y del tamaño del borde.
-   Registro de puntos de aparición mediante una herramienta especial.
-   Aldeano selector de partidas.
-   Sistema de entrada, salida y gestión de partidas.
-   Cofres normales, especiales y centrales.
-   Cierre progresivo de la zona.
-   Monstruos con dificultad progresiva.
-   Desafíos de altura y de Creepers.
-   Herramientas para guardar la construcción y los cofres de una arena.
-   Restauración del mapa después de utilizarlo.

## Requisitos

-   Servidor Minecraft Java Edition **1.8.9**.
-   Software de servidor **Paper 1.8.9**.
-   Acceso a la consola o permisos de operador para instalar y
    configurar el plugin.

La compatibilidad con versiones posteriores de Minecraft o con otros
programas de servidor no está garantizada.

## Instalación

1.  Detén el servidor.
2.  Copia `JDH-Core-Paper-1.8.9.jar` en la carpeta `plugins`.
3.  Inicia el servidor.
4.  Comprueba que el plugin `JDH` aparece habilitado.
5.  Abre `plugins/JDH/` y revisa los archivos de configuración
    generados.
6.  Configura la arena antes de permitir que los jugadores entren.

Haz una copia de seguridad del mundo antes de probar las funciones de
guardado y restauración.

## Configurar una arena paso a paso

### 1. Preparar el mundo

Construye el mapa de Juegos del Hambre en un mundo que esté disponible
en el servidor. El plugin no construye automáticamente la arena.

### 2. Registrar o seleccionar el mundo

``` text
/map Arena1
```

Sustituye `Arena1` por el nombre exacto del mundo que quieras utilizar.
El comando registra y selecciona ese mundo como arena.

### 3. Establecer el centro

``` text
/jdh coordenadas 0 100 0
```

Cambia `0 100 0` por las coordenadas X, Y y Z del centro de tu arena.

### 4. Configurar el borde

``` text
/jdh borde 500
```

Cambia `500` por el tamaño que quieras establecer para el borde de la
arena.

### 5. Registrar los puntos de aparición

``` text
/surgimiento cantidad 12
```

El comando entrega un hacha especial con la cantidad de usos indicada.
Utilízala haciendo clic derecho en los bloques donde quieras registrar
los puntos de aparición.

Registra suficientes puntos para la capacidad de jugadores de la arena.
Los puntos deben tener espacio libre para que los jugadores puedan
aparecer.

### 6. Revisar los puntos registrados

``` text
/surgimiento lista
```

Muestra la lista de puntos del mapa seleccionado, sus coordenadas y si
están marcados como válidos.

### 7. Revisar la arena

``` text
/jdh info
```

Consulta la información y el estado de configuración de la arena
seleccionada.

### 8. Crear el aldeano selector

``` text
/aldeanomap
```

Crea el aldeano selector en la ubicación del administrador que ejecuta
el comando. Colócalo en el lobby para que los jugadores puedan
interactuar con él.

## Comandos

Los comandos de administración requieren el permiso `jdh.admin`, salvo
donde se indique lo contrario.

  -----------------------------------------------------------------------
  Comando                             Uso
  ----------------------------------- -----------------------------------
  `/map <mundo>`                      Registra y selecciona un mundo como
                                      arena.

  `/jdh coordenadas <x> <y> <z>`      Establece el centro de la arena
                                      seleccionada.

  `/jdh borde <tamaño>`               Configura el tamaño del borde.

  `/jdh info`                         Muestra información de la arena
                                      seleccionada.

  `/jdh forzar`                       Fuerza el inicio de una partida
                                      para pruebas.

  `/surgimiento cantidad <n>`         Entrega el hacha para registrar `n`
                                      puntos de aparición.

  `/surgimiento lista`                Muestra los puntos de aparición
                                      registrados.

  `/surgimiento borrar <n>`           Elimina el punto indicado por su
                                      número en la lista.

  `/surgimiento reiniciar`            Elimina todos los puntos de
                                      aparición de la arena seleccionada.

  `/aldeanomap`                       Crea el aldeano selector de
                                      partidas.

  `/salirhmb`                         Permite al jugador abandonar la
                                      partida.

  `/juegoshmb guardar`                Guarda la construcción de la arena
                                      seleccionada.
  -----------------------------------------------------------------------

**Nota:** `/salirhmb` debe ejecutarlo un jugador dentro de una partida.
Los comandos que modifican una arena deben utilizarse después de
seleccionar el mundo con `/map`.

## Sistema de partidas

### Aldeano selector

Ejecuta `/aldeanomap` en el lobby. Los jugadores pueden interactuar con
el aldeano para acceder al sistema de selección de partidas.

Los valores de jugadores mínimos y máximos se configuran en `config.yml`
y pueden sobrescribirse por arena.

### Inicio de prueba

``` text
/jdh forzar
```

Este comando fuerza el inicio de una partida y está pensado para pruebas
administrativas. Úsalo en un entorno de prueba antes de abrir la arena
al público.

### Salir de una partida

``` text
/salirhmb
```

El jugador abandona la partida. El plugin gestiona la salida y la
restauración del estado del jugador según el sistema de partida.

## Cofres y botín

El plugin dispone de botín para cofres normales, cofres especiales y
cofres centrales.

-   **Normales:** pueden contener comida, armas, armaduras, pociones y
    materiales.
-   **Especiales:** se relacionan con las fases de cierre y utilizan
    grupos de recompensas configurables.
-   **Centrales:** utilizan una configuración de botín independiente.

Las probabilidades, cantidades y recompensas se ajustan en `config.yml`.

## Zona y cierres

La configuración incluye cinco fases de cierre de zona y un cierre
extraordinario al alcanzar el tiempo máximo de partida. También permite
configurar el daño fuera de la zona segura, el intervalo de daño y el
tamaño final.

## Monstruos progresivos

Los monstruos empiezan a aparecer después del tiempo configurado. La
configuración incluye esqueletos, lobos y conejos, además de niveles
progresivos que modifican sus atributos y los efectos que pueden
aplicar.

El ajuste `require-forest` permite exigir los biomas indicados en
`forest-biomes` para la aparición de monstruos.

## Desafíos

El plugin incluye dos tipos de desafío configurables:

-   **Altura:** establece un intervalo de altura que los jugadores deben
    alcanzar durante el evento.
-   **Creepers:** genera Creepers para un reto de supervivencia.

La probabilidad, ventana de activación, preparación y duración se
configuran en `config.yml`.

## Guardar la arena

Selecciona primero el mundo:

``` text
/map Arena1
```

Después ejecuta:

``` text
/juegoshmb guardar
```

El comando utiliza el sistema de guardado del plugin para registrar la
construcción de la arena y los cofres detectados. Realiza una copia de
seguridad antes de usarlo y prueba la restauración en un mundo de
prueba.

## Configuración

Al iniciar por primera vez, el plugin genera su carpeta de datos, que
incluye `config.yml` y `messages.yml`.

-   `plugins/JDH/config.yml`: configuración general.
-   `plugins/JDH/messages.yml`: mensajes del plugin.
-   `plugins/JDH/maps/<mundo>.yml`: configuración específica de cada
    mapa, cuando se genera.

La configuración específica de un mapa puede sobrescribir valores
generales dentro de la sección `settings:`.

### Valores predeterminados principales

  Ajuste                                        Valor predeterminado
  ------------------------------------- ----------------------------
  Jugadores mínimos                                                6
  Jugadores máximos                                               12
  Cuenta atrás larga                                     25 segundos
  Cuenta atrás corta                                     10 segundos
  Duración de partida                     1200 segundos (20 minutos)
  Celebración final                                      10 segundos
  Tamaño final de zona                                            20
  Máximo de monstruos vivos                                       24
  Monstruos por oleada                                             3
  Intervalo de aparición de monstruos                    15 segundos
  Inicio de aparición de monstruos                      180 segundos

Los tiempos de cierre, daño de zona, botín, niveles de monstruos y
parámetros de desafíos también se pueden modificar en `config.yml`.

## Permisos

  -----------------------------------------------------------------------
  Permiso                 Descripción             Valor predeterminado
  ----------------------- ----------------------- -----------------------
  `jdh.admin`             Administrar arenas y    Operadores (`op`)
                          partidas. Incluye       
                          `jdh.builder`.          

  `jdh.builder`           Utilizar las            Operadores (`op`)
                          herramientas de         
                          construcción y          
                          guardado.               
  -----------------------------------------------------------------------

## Solución de problemas

-   **El comando indica que no hay arena seleccionada:** ejecuta
    `/map <mundo>` primero.
-   **No aparecen suficientes puntos:** utiliza `/surgimiento lista` y
    registra los puntos que falten.
-   **El aldeano no aparece:** comprueba que ejecutas `/aldeanomap`
    dentro del juego y tienes `jdh.admin`.
-   **No puedes guardar:** selecciona la arena correcta y comprueba que
    tienes `jdh.builder`.
-   **El plugin no se habilita:** revisa la consola del servidor y
    confirma que estás utilizando Paper 1.8.9.

## Recomendaciones

-   Prepara y prueba cada arena antes de abrirla a los jugadores.
-   Registra suficientes puntos de aparición.
-   Comprueba el centro y el borde.
-   Guarda copias de seguridad de los mundos.
-   Prueba las partidas con varios jugadores.
-   No actualices el servidor a otra versión sin comprobar antes la
    compatibilidad del plugin.

## Información del proyecto

-   **Nombre:** JDH Core
-   **Identificador del plugin:** JDH
-   **Versión:** 0.1.0
-   **Autor indicado:** JDH
-   **Minecraft:** Java Edition 1.8.9
-   **Servidor:** Paper

------------------------------------------------------------------------

*Documentación preparada a partir de los comandos, permisos y
configuración incluidos en JDH-Core-Paper-1.8.9.jar. Algunas funciones
internas pueden depender de la configuración de la arena y del estado de
la partida.*
