# CoinsEconomy + ArrowEffects

Documentación de los plugins **CoinsEconomy 1.0.0** y **ArrowEffects
1.0.0** para servidores Minecraft Java 1.8.x.

Estos dos plugins trabajan juntos: CoinsEconomy administra la moneda
virtual **Coins**, las recompensas y las compras; ArrowEffects añade
efectos cosméticos para las flechas que se compran y equipan usando esas
Coins.

> **Importante:** ArrowEffects declara una dependencia obligatoria de
> `CoinsEconomy`. Debes instalar ambos plugins. La información de esta
> guía se ha extraído de los archivos JAR y sus configuraciones
> incluidas.

------------------------------------------------------------------------

# 1. CoinsEconomy

## ¿Qué hace?

CoinsEconomy es un sistema de economía persistente. Guarda los saldos y
las compras de los jugadores en una base de datos SQLite, entrega
monedas por determinadas actividades y ofrece una tienda extensible para
que otros plugins puedan registrar categorías de productos.

### Funciones principales

-   Moneda virtual llamada **Coins**.
-   Saldo persistente entre reinicios.
-   Recompensas por eliminar jugadores.
-   Recompensas por derrotar determinados monstruos.
-   Recompensas por colocar bloques permitidos.
-   Metas diarias y logros de los primeros 100 registros.
-   Tienda gráfica que puede integrar categorías de otros plugins.
-   Visualización periódica del saldo en la barra de acción (ActionBar).

## Instalación

1.  Detén el servidor.
2.  Copia `CoinsEconomy-1.0.0.jar` en la carpeta `plugins`.
3.  Inicia el servidor.
4.  Comprueba en la consola que CoinsEconomy se haya habilitado.
5.  Revisa la carpeta `plugins/CoinsEconomy/` y su `config.yml`.

Al iniciar, el plugin crea la base de datos `economy.db` dentro de su
carpeta de datos. No borres este archivo: contiene los saldos, compras y
estadísticas guardadas.

## Comandos

  --------------------------------------------------------------------------------------
  Comando                                Alias                   Uso
  -------------------------------------- ----------------------- -----------------------
  `/coins`                               `/monedas`              Consulta tu saldo.

  `/coins balance <jugador>`             ---                     Consulta el saldo de un
                                                                 jugador. Para consultar
                                                                 a alguien desconectado
                                                                 se requiere permiso de
                                                                 administrador.

  `/coins give <jugador> <cantidad>`     ---                     Añade Coins al jugador.
                                                                 Requiere `coins.admin`.

  `/coins remove <jugador> <cantidad>`   ---                     Retira Coins al
                                                                 jugador. Requiere
                                                                 `coins.admin`.

  `/comprar`                             `/tienda`, `/shop`      Abre la tienda de Coins
                                                                 dentro del juego.
  --------------------------------------------------------------------------------------

### Ejemplos

Consultar tu saldo:

``` text
/coins
```

Consultar el saldo de otro jugador:

``` text
/coins balance Steve
```

Dar 100 Coins:

``` text
/coins give Steve 100
```

Retirar 25 Coins:

``` text
/coins remove Steve 25
```

Abrir la tienda:

``` text
/comprar
```

Las cantidades de `give` y `remove` deben ser números enteros positivos.

## Cómo se consiguen Coins

Las recompensas se entregan automáticamente cuando se cumplen las
condiciones configuradas.

### Eliminaciones PvP

-   Recompensa predeterminada: **3 Coins por eliminación**.
-   Límite diario de recompensas PvP: **150 Coins**.
-   Tiempo de espera para volver a recibir recompensa por eliminar a la
    misma víctima: **600 segundos (10 minutos)**.
-   El plugin también tiene una recompensa de logro para las primeras
    100 eliminaciones: **50 Coins**.

El límite diario se aplica a las recompensas PvP ordinarias. El logro de
las primeras 100 eliminaciones es una recompensa separada.

### Monstruos

Recompensas predeterminadas por derrotar monstruos:

  Monstruo                   Coins
  ------------------------ -------
  Zombi (`ZOMBIE`)               1
  Esqueleto (`SKELETON`)         1
  Araña (`SPIDER`)               1
  Creeper (`CREEPER`)            2
  Enderman (`ENDERMAN`)          3
  Blaze                          2
  Bruja (`WITCH`)                3

-   Límite diario de recompensas por monstruos: **60 Coins**.
-   Solo se consideran los tipos incluidos en la configuración.

### Colocación de bloques

El plugin registra la colocación de bloques permitidos y puede entregar
recompensas por metas.

Materiales permitidos de forma predeterminada:

-   `STONE`
-   `COBBLESTONE`
-   `DIRT`
-   `WOOD`
-   `SANDSTONE`
-   `BRICK`
-   `GLASS`

Configuración predeterminada:

-   Se exige estar en modo supervivencia (`require-survival: true`).
-   Meta diaria: **20 bloques válidos**.
-   Recompensa por completar la meta diaria: **8 Coins**.
-   Recompensa por los primeros 100 bloques válidos: **25 Coins**.

El sistema registra ubicaciones para evitar contar repetidamente el
mismo bloque como una nueva colocación válida.

## Configuración de CoinsEconomy

Archivo: `plugins/CoinsEconomy/config.yml`

Configuración incluida en el JAR:

``` yaml
economy:
  pvp:
    reward-per-kill: 3
    daily-cap: 150
    repeat-victim-cooldown-seconds: 600
  eliminations:
    first-100-reward: 50
  mobs:
    daily-cap: 60
    rewards:
      ZOMBIE: 1
      SKELETON: 1
      SPIDER: 1
      CREEPER: 2
      ENDERMAN: 3
      BLAZE: 2
      WITCH: 3
  blocks:
    allowed-materials:
      - STONE
      - COBBLESTONE
      - DIRT
      - WOOD
      - SANDSTONE
      - BRICK
      - GLASS
    daily-goal: 20
    daily-goal-reward: 8
    first-100-reward: 25
    require-survival: true
  daily-timezone: system
  actionbar-interval-ticks: 40
```

Puedes cambiar estos valores y reiniciar el servidor para aplicar la
configuración. `daily-timezone: system` utiliza la zona horaria del
sistema. `actionbar-interval-ticks: 40` establece el intervalo de
actualización de la barra de acción.

## Permisos

  -----------------------------------------------------------------------
  Permiso                 Función                 Predeterminado
  ----------------------- ----------------------- -----------------------
  `coins.admin`           Administrar saldos con  OP
                          los comandos de         
                          consulta                
                          administrativa, `give`  
                          y `remove`.             

  -----------------------------------------------------------------------

Los jugadores normales pueden consultar su propio saldo y abrir la
tienda sin ese permiso.

------------------------------------------------------------------------

# 2. ArrowEffects

## ¿Qué hace?

ArrowEffects añade efectos visuales y sonidos cosméticos a las flechas
disparadas por los jugadores.

-   Muestra partículas o efectos durante el vuelo de la flecha.
-   Puede reproducir sonidos durante el vuelo.
-   Puede generar un efecto y sonido al impactar contra un jugador,
    monstruo o bloque.
-   Permite comprar efectos con Coins.
-   Guarda los efectos comprados y el efecto seleccionado.
-   No modifica el daño de las flechas.

## Requisito obligatorio

ArrowEffects declara esta dependencia:

``` yaml
depend:
  - CoinsEconomy
```

Por tanto, **CoinsEconomy debe estar instalado y habilitado** para que
ArrowEffects pueda funcionar. Si no encuentra el servicio de economía de
CoinsEconomy, ArrowEffects se desactiva.

## Instalación

1.  Instala `CoinsEconomy-1.0.0.jar` en `plugins`.
2.  Instala `ArrowEffects-1.0.0.jar` en la misma carpeta.
3.  Inicia o reinicia el servidor.
4.  Comprueba que ambos plugins aparecen habilitados en la consola.
5.  Revisa `plugins/ArrowEffects/config.yml`.

## Comando

  -----------------------------------------------------------------------
  Comando                 Alias                   Función
  ----------------------- ----------------------- -----------------------
  `/flechas`              `/arroweffects`,        Abre el menú de efectos
                          `/eflechas`             de flecha.

  -----------------------------------------------------------------------

No necesita un comando separado para comprar o equipar: las acciones se
realizan desde el menú.

## Cómo usar el menú

1.  Consigue Coins mediante las actividades de CoinsEconomy o mediante
    un administrador.
2.  Ejecuta `/flechas`.
3.  Se abrirá el menú **Efectos de flecha**.
4.  Selecciona el efecto que quieras.
5.  Si todavía no lo tienes, el menú mostrará su precio y podrás
    comprarlo.
6.  Si ya lo compraste, podrás equiparlo.
7.  Dispara una flecha para ver el efecto seleccionado.

Los efectos comprados quedan guardados para el jugador. Puedes volver al
menú para cambiar el efecto equipado.

## Efectos disponibles y precios predeterminados

Los precios se leen de `plugins/ArrowEffects/config.yml`.

  Identificador   Nombre mostrado       Precio
  --------------- ----------------- ----------
  `heart`         Corazón             60 Coins
  `fire`          Llama               60 Coins
  `dirt`          Tierra rota         15 Coins
  `stone`         Piedra rota         20 Coins
  `barrier`       Barrera rota        35 Coins
  `poison`        Veneno              45 Coins
  `magic`         Magia               30 Coins
  `sparkle`       Chispa              25 Coins
  `cloud`         Nube                20 Coins
  `happy`         Feliz               25 Coins
  `portal`        Portal              35 Coins
  `lava`          Lava                30 Coins
  `water`         Agua                20 Coins
  `music`         Notas               40 Coins
  `slime`         Slime               25 Coins
  `smoke`         Humo                15 Coins
  `frost`         Escarcha            25 Coins
  `emerald`       Esmeralda           35 Coins

## Configuración de ArrowEffects

Archivo: `plugins/ArrowEffects/config.yml`

Configuración incluida en el JAR:

``` yaml
menu-title: "§8Efectos de flecha"
flight:
  sound-enabled: true
  sound-interval-ticks: 10
  sound-volume: 0.25
impact:
  sound-volume: 0.8
  player-sound: ORB_PICKUP
  mob-sound: HURT_FLESH
  block-sound: ARROW_HIT
prices:
  heart: 60
  fire: 60
  dirt: 15
  stone: 20
  barrier: 35
  poison: 45
  magic: 30
  sparkle: 25
  cloud: 20
  happy: 25
  portal: 35
  lava: 30
  water: 20
  music: 40
  slime: 25
  smoke: 15
  frost: 25
  emerald: 35
```

### Qué significa cada opción

-   `menu-title`: título del menú de efectos.
-   `flight.sound-enabled`: activa o desactiva los sonidos durante el
    vuelo.
-   `flight.sound-interval-ticks`: intervalo entre sonidos durante el
    vuelo, en ticks.
-   `flight.sound-volume`: volumen de los sonidos durante el vuelo.
-   `impact.sound-volume`: volumen del sonido al impactar.
-   `impact.player-sound`: sonido al impactar contra un jugador.
-   `impact.mob-sound`: sonido al impactar contra un monstruo.
-   `impact.block-sound`: sonido al impactar contra un bloque.
-   `prices`: precio de cada efecto en Coins.

Puedes modificar los precios para adaptar la tienda a la economía de tu
servidor. Mantén los identificadores (`heart`, `fire`, `dirt`, etc.) tal
como aparecen; cambia únicamente sus valores si quieres ajustar los
precios.

------------------------------------------------------------------------

# 3. Cómo funcionan juntos

CoinsEconomy es el plugin que administra los saldos y las compras.
ArrowEffects utiliza el servicio de CoinsEconomy para cobrar los efectos
y guardar qué efectos tiene cada jugador.

**Flujo de uso:**

1.  El jugador entra al servidor.
2.  Consigue Coins jugando o las recibe de un administrador.
3.  Consulta su saldo con `/coins`.
4.  Abre la tienda de efectos con `/flechas`.
5.  Compra un efecto.
6.  Lo equipa desde el mismo menú.
7.  Dispara una flecha y se muestra el efecto cosmético.

## Prueba rápida para administradores

Con ambos plugins instalados y habilitados:

``` text
/coins
/coins give <jugador> 200
/flechas
```

Sustituye `<jugador>` por el nombre real del jugador. Después abre
`/flechas`, compra un efecto que cueste 200 Coins o menos y pruébalo
disparando una flecha.

------------------------------------------------------------------------

# 4. Solución de problemas

### ArrowEffects no se habilita

-   Comprueba que `CoinsEconomy` está instalado.
-   Comprueba que CoinsEconomy se habilitó correctamente.
-   Revisa los errores de la consola al iniciar el servidor.
-   Verifica que ambos JAR sean las versiones esperadas.

### `/flechas` no abre el menú

-   Comprueba que ArrowEffects está habilitado.
-   Ejecuta el comando dentro del juego.
-   Revisa la consola para detectar errores.

### No puedo comprar un efecto

-   Consulta tu saldo con `/coins`.
-   Comprueba que tienes suficientes Coins.
-   Revisa el precio del efecto en `plugins/ArrowEffects/config.yml`.

### No veo el efecto en las flechas

-   Comprueba que compraste y equipaste un efecto.
-   Dispara una flecha después de equiparlo.
-   Revisa que ArrowEffects siga habilitado y que la consola no muestre
    errores.

### No recibo monedas por una actividad

-   Comprueba que la actividad y el tipo de entidad o bloque están
    incluidos en `config.yml`.
-   Revisa si alcanzaste el límite diario.
-   Para PvP, comprueba el tiempo de espera para recompensar de nuevo
    por la misma víctima.
-   Para bloques, comprueba el modo de juego y que el material esté
    permitido.

------------------------------------------------------------------------

# 5. Compatibilidad y notas

  Plugin         Versión   Plataforma indicada
  -------------- --------- ---------------------
  CoinsEconomy   1.0.0     Minecraft 1.8.x
  ArrowEffects   1.0.0     Minecraft 1.8.x

-   ArrowEffects necesita CoinsEconomy.
-   CoinsEconomy almacena los datos en SQLite.
-   No borres `economy.db` si quieres conservar los saldos y las
    compras.
-   ArrowEffects es cosmético: su descripción indica que no modifica el
    daño de las flechas.
-   No se garantiza compatibilidad con versiones de Minecraft
    posteriores a las indicadas.

## Archivos del plugin

Después de iniciar el servidor, revisa las carpetas de datos de cada
plugin para encontrar sus configuraciones y datos persistentes.

**CoinsEconomy** - `config.yml`: recompensas, límites y materiales
válidos. - `economy.db`: base de datos SQLite con saldos, compras y
estadísticas.

**ArrowEffects** - `config.yml`: precios, sonidos y título del menú. -
Los efectos comprados y seleccionados se guardan mediante el sistema de
persistencia del plugin.

------------------------------------------------------------------------

**Documentación de CoinsEconomy 1.0.0 y ArrowEffects 1.0.0.**
