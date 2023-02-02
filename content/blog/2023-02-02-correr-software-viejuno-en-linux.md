+++
title = "Correr software viejuno en linux"
description = "Diferentes formas de correr software viejuno (o no tan viejuno) desde tu actualizado linux"

[taxonomies]
tags=["tecnología", "recetas", "retro"]
+++

# El problema

Hay un determinado software que no puedes ejecutar en tu nuevo ordenador, pero quieres seguir usando.

Este problema puede deberse a varios motivos. Voy a poner algunos ejemplos de ellos y luego veremos diferentes 
opciones existentes para solventarlo:

* Quiero volver a ejecutar mi primer programa que hice de niño en basic del spectrum porque me he puesto tierno.
* Quiero ejecutar el primer juego que hice de adolescente que corría en un pc con ms-dos.
* Quiero ver ese proyectito que hice en la uni con Electronic Workbench para unas prácticas de electrónica digital.
* Quiero ejecutar el krecipes en el ordenador de mi novia para poder probar esas recetas top de nuevo.

En algunos casos el hardware en el que el software corría era totalmente diferente y no tengo un dispositivo funcional
del mismo, como era el caso del spectrum. En otros casos el hardware era una versión antigua del que ahora tengo, pero
con sistemas operativos diferentes al que uso actualmente. Y otras veces es el mismo hardware y el mismo sistema operativo,
pero es alguna de las dependencias del programa las que quedaron obsoletas como en el caso de krecipes.

# Las alternativas

## Emulación

La wikipedia lo define de la siguiente forma:

> En informática, un emulador es un software que permite ejecutar programas o videojuegos en una plataforma 
> (sea una arquitectura de hardware o un sistema operativo) diferente de aquella para la cual fueron escritos
> originalmente. A diferencia de un simulador, que solo trata de reproducir el comportamiento del programa,
> un emulador trata de modelar de forma precisa el dispositivo de manera que este funcione como si estuviese 
> siendo usado en el aparato original. 

Para el caso de mi primer programa de spectrum me bastaría con un intérprete de basic compatible con el del spectrum o
un simulador porque era un programa muy tonto. 
Pero si quisiera jugar a aquellos juegos de mi infancia enseguida notaría que la velocidad no es la adecuada y
resultaría un problema. 
Por eso, aunque sean más complejos, es más fácil encontrar emuladores. Una búsqueda rápida de 
`spectrum emulator for linux` te da un montón de opciones. De hecho existen emuladores de spectrum que funcionan en un
navegador y no haría falta siquiera instalar nada.

Hay software de emulación para un montón de videoconsolas más o menos viejas. Una forma fácil y cómoda de probarlos es
instalar [retropie](https://retropie.org.uk/) en una raspberry pi.

Para el caso de ejecutar mi primer juego hecho en ms-dos un emulador completo como 
[DOSBox](https://www.dosbox.com/news.php?show_news=1). Esto sería necesario si quiero ejecutarlo en una raspberry-pi
que tiene una cpu arm que no es compatible con las instrucciones *x86* necesarias para correr ms-dos y el binario de
mi juego o del compilador privativo que usé. Sin la emulación no se podría utilizar la tarjeta de sonido de mis equipos
intel, ya que solo la tarjeta de sonido soundblaster estaba soportada en aquel juego.

## (Para)Virtualización

La paravirtualización es un tipo de emulación donde es posible el acceso al hardware del anfitrión por parte del
software emulado. Comúnmente se diferencia la virtualización de la emulación y por eso los cito al mismo nivel y no
como un sub apartado de la emulación. En linux tenemos opciones como [VirtualBox](https://www.virtualbox.org/),
[QEmu](https://www.qemu.org/) o [KVM](https://www.linux-kvm.org/page/Main_Page).

Hablando a lo bruto, los softwares de virtualización, te permiten crear un ordenador virtual en el que puedes
instalar un sistema operativo diferente del anfitrión. Las opciones de paravirtualización, el tema de compartir
recursos hardware directamente con la máquina virtualizada, permite saltarse pasos necesarios en la emulación
completa y mejorar el rendimiento de determinadas operaciones. Por ejemplo, puedes asignar direcciones de memoria o 
hardware como una unidad extraible que serán manejados directamente por el invitado. Según el hardware de tu equipo
anfitrión e invitado, soporte del mismo,... la emulación requerirá ser más o menos completa.
No tendría sentido para correr un juego de spectrum, donde la emulación completa es necesaria, pero es una alternativa
viable para el resto de las opciones (juegos de MS-DOS, aplicación de electrónica de windows, ejecutar krecipes).

### Krecipes en una máquina virtual

La forma sencilla, para usuarios, de hacer esto sería a través de una interfaz GUI para manejar máquinas virtuales
como VirtualBox o [Virtual Machine Manager](https://virt-manager.org/). Hay maneras más complicadas que pueden
suponer un atajo a algunos de los engorros que vamos a ver, pero no tienen sentido porque como veremos hay opciones
mejores.

Se crearía una máquina virtual nueva y se usaría una iso de Ubuntu 18.04 (o cualquier otra distro en la que krecipes
siga funcionando) para el primer arranque. Ejecutamos la instalación del sistema operativo. Reiniciamos. Instalamos
krecipes dentro de la máquina virtual (fuera no vas a poder) y ejecutamos la aplicación. Este proceso es engorroso
por varios motivos:

* Tenemos un ubuntu entero ocupando varios gigas de disco para una sola aplicación.
* La instalación es un engorro y se tarda bastante tiempo.
* Para ejecutar la aplicación tenemos que arrancar antes la máquina virtual (tarda un rato y chupa memoria) y luego
  buscar dentro de la máquina virtual la aplicación a ejecutar. ¡Pasamos a tener 2 escritorios!
* Para hacer backups de la aplicación hay que hacer unidades compartidas, montar el disco de backups en la máquina virtual
  o alguna otra opción para acceder a los datos que queremos guardar.
* Algo tan simple como copiar una receta del navegador al krecipes implican configuraciones extras en la máquina
  virtualizada y/o en la anfitriona.
* ... alguno que seguro que no me acuerdo, pero también es un dolor :shrug:

## Capa de emulación

Hemos visto con la paravirtualización que una forma de obtener más rendimiento es eliminar la cantidad de capas
necesarias a emular (cpu, memoria, dispositivos de audio, sistemas de ficheros,...). Y esto es lo que hace
[WINE](https://www.winehq.org/) (Wine Is Not Emulator), que en vez de emular una máquina en la que instalar windows
lo que hace es implementar el kernel de windows para que pueda funcionar en linux. Se limita a emular las capas
necesarias para que los programas funcionen.

La contrapartida para ejecutar programas de linux en windows se llama
[WSL](https://learn.microsoft.com/es-es/windows/wsl/about). FreeBSD también cuenta con una capa de compatibilidad para
poder [ejecutar binarios de linux](https://docs.freebsd.org/en/books/handbook/linuxemu/).

Pero volviendo al tema que nos ocupa: wine nos permite ejecutar un software de windows como lo era Electronic
Workbench sin la necesidad de emular hardware, hacer reservas de memoria,... Con eso podrías ver tus proyectos antiguos
para portarlos a un programa open source o rememorar esa época en la que tenías más pelo. Es también una opción ligera
para ejecutar software que no ha sido portado a Linux. Con [Bottles](https://usebottles.com/app/#fusion360) es muy
fácil instalar Autodesk Fusion 360 en unos pocos *clicks* y él se encarga de configurar una instancia de wine,
descargar el instalador y ejecutarlo para los que todavía no se atreven con FreeCAD ;-)

## Contenedores

Tras las ventajas vistas en el paso de emulación a paravirtualización minimizando las capas emuladas se dió otra
vuelta de tuerca con los contenedores. Si las aplicaciones no necesitan correr en un sistema operativo, hardware,...
lo único indispensable para emular serían los [namespaces](https://en.wikipedia.org/wiki/Linux_namespaces) para que
cada contenedor vea sus propios procesos, puntos de montaje, dispositivos de red,... aislado del resto. Así pensaría
que es una máquina independiente aunque ejecute directamente el kernel del anfitrión y acceda a su hardware. Por
conveniencia también son necesarios los [cgroups](https://en.wikipedia.org/wiki/Cgroups) o grupos de control, para
limitar el acceso a elementos como cpu, memoria,... De esta forma un contenedor podría reservar toda la memoria
disponible (no la de la máquina, sino la asignada para él), sin dejar sin memoria al anfitrión o los otros contenedores emulados.

La historia de esta tecnología es mucho más larga, empezando posiblemente por los 
[chroot](https://es.wikipedia.org/wiki/Chroot) que aislaban porciones al sistema de ficheros, las jaulas de freebsd,
el primitivo OpenVZ de linux hasta llegar las implementaciones que hacen uso de los namespaces y cgroups en linux como
[SystemD containers](https://wiki.archlinux.org/title/systemd-nspawn), [LXC](https://linuxcontainers.org/) o
[Docker](https://www.docker.com/).

Esta emulación mínima hace que surgieran dos estrategias principales para los contenedores:
* Emular una máquina con su propio initd o systemd,... como si fuese una máquina virtual tradicional.
* Emular aplicaciones haciendo del contenedor un chroot con esteroides. Haciendo del contenedor una herramienta
  para empaquetar aplicaciones con sus dependencias que se ejecuten de forma aislada (fácil administración de recursos,
  evitar *dependency hell*,...)

Para el tema de ejecutar krecipes esa emulación de aplicación es más que suficiente y no necesitamos la emulación de
máquina.

### Krepices en Docker

Con la emulación a nivel máquina de la máquina virtual hemos visto un montón de problemas con la instalación, (ab)uso
de recursos, problemas para compartir portapapeles, problemas para hacer backups, problemas, problemas, problemas...

Con la siguiente receta no notaremos diferencia respecto a tenerlo instalado nativamente.

#### Creación de la imagen Docker

Estos ficheros los he creado en un directorio llamado `~/opt/krecipes`, pero puedes usar cualquier otro.

Fichero `Dockerfile`:

```dockerfile
FROM ubuntu:18.04

RUN apt update && apt install -y krecipes

COPY run.sh /etc/init.d
ENTRYPOINT ["/etc/init.d/run.sh"]
```

Decimos que use una imagen base de ubuntu, versión 18.04, donde funcionaba krecipes. Actualizamos los paquetes e
instalamos desde los mismos krecipes (el sistema de paquetes se encargará de instalar las dependencias que ya
no son compatibles con nuestra nueva versión). Copiamos un script llamado `run.sh` a la carpeta `/etc/init.d` de la
imagen del contenedor. Decimos que ese script es el punto de entrada para cuando se arranque un contenedor que haga
empleo de esta imagen que estamos definiendo aquí.

Fichero `run.sh`:

```bash
#!/usr/bin/env bash

dbus-launch /usr/bin/krecipes --nofork
```

Este script es tan simple que directamente podríamos eliminarlo y cambiar al siguiente `Dockerfile`:

```dockerfile
FROM ubuntu:18.04

RUN apt update && apt install -y krecipes

ENTRYPOINT ["dbus-launch", "/usr/bin/krecipes", "--nofork"]
```

Para generar la imagen ejecutamos el siguiente comando: `docker build . -t krecipes:latest`. Esto se bajará la imagen
de ubuntu, instalará en un contenedor temporal krecipes y generará una imagen a partir de dicho contenedor llamada
`krecipes:latest` (nombre y versión). Los contenedores que hagan uso de esa imagen no necesitarán instalar krecipes,
porque partirán de una copia de los datos tras la instalación. También es posible subir esta imagen en un repositorio
de imágenes para que una instalación en un nuevo ordenado no necesite de estos pasos de generar la imagen.

#### Ejecutar el contenedor de docker

Para simplificar crearemos el siguiente script en la misma carpeta con nombre `launch.sh`:

```bash
#!/bin/bash

docker run --rm \
    --user=$(id -u $USER):$(id -g $USER) \
    --env="DISPLAY" \
    --workdir="/home/$USER" \
    --volume="/home/$USER:/home/$USER" \
    --volume="/etc/group:/etc/group:ro" \
    --volume="/etc/passwd:/etc/passwd:ro" \
    --volume="/etc/shadow:/etc/shadow:ro" \
    --volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --env="QT_X11_NO_MITSHM=1" \
    krecipes:latest
```

`docker run krecipes:latest` ejecutaría un contenedor con la imagen de krecipes. Fallaría porque no encontraría el
servidor X donde mostrar la aplicación. Con `--env="DISPLAY"` le comunicamos donde debe conectarse la aplicación al
servidor X. Con `--env="QT_X11_NO_MITSHM=1"` definimos un workaround, sin más. Con `--user=$(id -u $USER):$(id -g $USER)`
definimos que el contenedor se ejecutará con nuestro mismo uid y gid que en la máquina host. Con
`--volume="/tmp/.X11-unix:/tmp/.X11-unix:rw"` compartimos el unix socket del server X con el contenedor. El resto de
los `--volume=...` es para que el contenedor tenga configuradas nuestros mismos usuarios y tenga acceso a nuestro home.
Al arrancar krecipes creará una configuración persistente en nuestro propio host y podemos exportar los datos de backup
a una carpeta de nuestro home. `--rm` borrará el contenedor y todos los datos generados dentro de él (logs,...). Como
hemos compartido nuestro home la configuración queda escrita fuera del contenedor y estará disponible en las próximas
ejecuciones.

Haciendo ese script ejecutable `chmod u+x ./launch.sh` podemos lanzar krecipes con `./launch.sh`. Pero podemos mejorar
esto. Para eso buscaremos un icono que guardaremos en un fichero llamado, por ejemplo, `incon.png`. Lo podemos
bajar [del propio proyecto](https://invent.kde.org/unmaintained/krecipes/-/tree/master/icons) si no queremos complicarnos.
Luego crearemos el siguiente lanzador en `~/.local/share/applications/krecipes.desktop`:

```ini
[Desktop Entry]
Type=Application
Name=KRecipes
Comment=Las mejores recetas
Path=/home/tuusuario/opt/krecipes
Exec=/home/tuusuario/opt/krecipes/launch.sh
Icon=/home/tuusuario/opt/krecipes/icon.png
Terminal=false
X-Desktop-File-Install-Version=0.26
```

donde `home/tuusuario/opt/krecipes` dependerá de tu nombre de usuario y del directorio elegido para crear el script
`launch.sh` y hayas descargado el icono.

Con esto la aplicación quedará mejor integrada y podrá añadirse a tus aplicaciones favoritas o buscarse como cualquier
otra aplicación instalada en tu ordenador.

#### Notas finales

Aunque he puesto un ejemplo de usar docker para lanzar aplicaciones obsoletas es importante saber que se puede hacer
un ejemplo parecido con lxd o que variaciones de esta receta se pueden usar con objetivos diferentes:
* Tenemos una aplicación que de vez en cuando se vuelve loca y se come toda la ram de la máquina. Queremos usarla y que 
  en esa situación muera sin afectar al rendimiento de toda la máquina, poder trabajar con el resto de aplicaciones de
  forma normal.
* Queremos usar una aplicación para el trabajo que también usamos personalmente sin que sus configuraciones se mezclen
  en nuestro home. Sería tan fácil como montar otro directorio de home dentro del contenedor.
* Queremos probar que nuestra aplicación funcionará en diferentes distros sin tener que hacer instalaciones completas
  de todas las distros a probar.
* ...

# Ejemplos de emulación más allá de ejecutar cosas viejunas (o no)

## EDA (Electronic Design Automation)

Hay software que permite emular hardware (tanto componentes pasivos tipo condensadores y resistencias, 
como semiconductores). Por ejemplo se podría emular un DAC (con sus redes de resistencia y 
amplificador operacional), un amplificador a transistores y una etapa de potencia a válvulas para estimar la
distorsión del diseño sin gastar dinero en comprar los componentes y montar un prototipo. Esta emulación de
componentes analógicos puede ser usada por emuladores de consolas para una emulación más fidedigna de la tarjeta
de sonido de cierto dispositivo (con sus distorsiones al saturar amplificadores, sus limitaciones de ancho de banda,...).

Estos emuladores también permiten cosas que van más allá de hacer un prototipo virtual. Los componentes tienen
una tolerancia, por ejemplo, una resistencia de 330 ohmios con una tolerancia del 5% puede tener valores de resistencia
de 313.5 a 346.5 ohmios. Los componentes también tienen valores normalizados. Por ejemplo, si al calcular un circuito
vemos que necesitamos una resistencia de 323.7 ohmios sabemos que no vamos a encontrar esa resistencia en el mercado y
tenemos que aproximar a un valor normalizado, 330 ohmios en este caso. Los emuladores permiten probar el circuito con
diferentes valores de tolerancias y valores normalizados para garantizar que todo circuito que se construya funciona
conforme ciertos parámetros.

Los emuladores electrónicos también suelen tener la capacidad de probar los componentes ideales con sus características 
reales. Por ejemplo, un condensador ideal se comporta en la realidad como si tuviera en paralelo una resistencia grande
y en serie una resistencia pequeña, pero ninguna de ellas despreciables del todo. Estas características varían en
función del tipo de condensador, su calidad de construcción,... Estas variables repercutirán en el tamaño del componente,
su coste,... La emulación permite abaratar la prueba y error de diferentes componentes reales para que se comporten de
manera suficientemente aproximada a la ideal sin pasarse de restricciones como coste, tamaño o pérdidas energéticas.

Un ejemplo de simulador de circuitos open source es [Qucs-S](https://ra3xdh.github.io/).

## Emulación de máquinas recreativas

Las máquinas recreativas tenían un chasis bastante común con una pantalla crt, unos altavoces, sistema de monedas,
2 joysticks (digital, botones) y botones (por jugador, continues). Aunque había variaciones con trackballs 
(recuerdo un juego de minigolf), 4 joysticks como el gaunlet, joystick analógico para alguno de aviones, motorizados
para fuerza del volante y mover cabina de coche, tipo máquina de petacos o del millón, ... Esos chasis por dentro
tenían un conector jamma al que se conectaba una pcb para personalizarla con diferentes juegos.

Esas pcb con el juego, muchas veces eran [creaciones ad-hoc para un único juego](http://www.system16.com/hardware.php?id=756), 
otras veces [una placa genérica](http://www.system16.com/hardware.php?id=795)
que programadas con diferentes ROM (read only memories) se convertían en diferentes juegos.

[MAME](https://www.mamedev.org/) (Multi Arcade Machine Emulator) es un framework con el que se ha conseguido
crear las variaciones necesarias para correr más de
[10.000 juegos](http://adb.arcadeitalia.net/lista_mame.php). Para conseguir eso ha tenido que implementar la emulación
de varias cpus, chips de audio,... Solo esto es ya alucinante sin entrar en otros
requisitos necesarios para emular juegos.

## Emulación de consolas

Emular una consola suena a tarea más simple que emular cientos de placas diferentes. Para el 99% de los juegos basta
emular el hardware de la consola por un lado y tener el volcado de los cartuchos en ficheros separados por el otro.
El 99% de los cartuchos eran una ROM que se conectaba al bus de direcciones, datos y control de la cpu principal de la
consola. El otro 1% eran cartuchos con hardware específico para ese juego que complica sobre manera la emulación.

Uno de los mayores handicaps a la hora de emular consolas es el [timing](https://aarongiles.com/old/mamemem/part6.html).
Si la máquina emuladora es mucho más rápida que la
máquina emulada hay que meter retardos para que el juego no vaya demasiado rápido. Si la máquina emuladora no es capaz
de emular a la máquina emulada a tiempo la jugabilidad se verá afectada (eventos de entrada con retraso, distorsión
en los sonidos reproducidos, perdida de frames,...). Si hace falta emular varias cpus o chips de audio o varios sistemas
que se puedan ver afectados por la coordinación de los diferentes timings el resultado puede ser del todo erroneo y/o
confuso para el jugador.

## Emulación de ordenadores antiguos

Muchos tenían un hardware similar a una consola (también con la capacidad de ejecutar cartuchos), pero en el que los
programas solían cargarse desde una cinta de audio a memoria con unas rutinas de carga. Uno de los handicaps para
emular estos ordenadores es que no quieres tener que esperar 10 minutos a que cargue un juego, pero tampoco te puedes
limitar a cargar las roms como volcados de memoria porque algunos juegos requerían cargar nueva información de cinta
al pasar de fase. La mayor complejidad aquí es diseñar un formato compatible con esas rutinas de carga que pueda
emularse sin respetar la velocidad de carga. Esto hace que mientras puedes acceder a casi todas las roms de una 
consola nes con [un único formato de rom](https://www.nesdev.org/wiki/NES_2.0),
compatible con decenas de emuladores, existen [multitud de formatos para
emular ordenadores spectrum](https://worldofspectrum.org/faq/reference/formats.htm) de los que cada emulador de
spectrum implementa uno o más.

## Emulación hardware

Aparte de emular un hardware mediante un software, existen soluciones para emular hardware mediante hardware
configurable. [MiSTer FPGA](https://mister-devel.github.io/MkDocs_MiSTer/) es una plataforma open source para
recrear consolas mediante una [FPGA](https://en.wikipedia.org/wiki/Field-programmable_gate_array). Una FPGA, dicho a lo
bruto, son un cholón de puertas lógicas e incluso circuitos digitales más complejos en los que puedes configurar la
manera en la que se interconectan entre sí. Eso permite tanto emular hardware conocido como el caso de las consolas
emuladas en MiSTer FPGA, como tener una herramienta con la que prototipar sistemas complejos como una cpu antes mandar
a fabricar esa cpu a escala.

Otros ejemplos con los que simular circuitos lógicos a menor escala son las PLA (Programable Logic Array) como la [incluida
en el Commodore 64](https://www.c64-wiki.com/wiki/PLA_(C64_chip)) para ahorrar los varios chips con los que se podría
implementar la configuración de los bancos de memoria con puertas lógicas sin tener que fabricar/programar un chip específico para
ellos. La emulación de hardware en [dispositivos programables](https://madpcb.com/glossary/logic-device/) hardware
permiten un ahorro de costes en tiradas insuficientes para amortizar la fabricación de un chip específico.

Las FPGA tienen diferentes usos dentro de los dispositivos móviles. Algunos chips de acelerómetros/giróscopos implementan
lógica adicional para poder enviar interrupciones que despierten las cpus. Pero la lógica para despertar un móvil
puede implicar a más de un periférico (control de presencia, movimiento, tocar la pantalla, recepción de una llamada
del subsistema radio,...) y es aquí donde un dispositivo programable de bajo consumo puede despertar la cpu con
periféricos más simples, baratos y de menor consumo. Este dispositivo también puede hacer labores de DSP (digital
signal processing) con la ventaja de poder reprogramarse ante cambios de algoritmos o bugs, cosa que no se podría hacer
si ese hardware no estuviera emulado.

## Emulación dentro del microprocesador

### Instrucciones de virtualización

Para emular una cpu z80 en una cpu i486 era necesaria la emulación completa porque el conjunto de instrucciones no era
compatible. Pero para emular una cpu i486 en una máquina i486 también era necesaria una emulación completa, aunque era
un proceso más sencillo de realizar. ¿Pero por qué? Porque la arquitectura x86 tiene 4 niveles de privilegios a la hora
de ejecutar instrucciones. El kernel de la máquina anfitriona puede ejecutar las instrucciones de todos los niveles,
pero el kernel de la máquina invitada va a intentar acceder a unos niveles de ejecución que le son restringidos. Por eso
un i486 virtualizado sobre un i486 necesitaba emulación completa, donde cada instrucción necesitaba verificarse antes
de ser enviada directamente a la cpu o de ser transformada en un código que evitase las restricciones de privilegios.

Para acelerar la velocidad de virtualización se necesitaba una alternativa segura con la que saltarse los niveles de
privilegios para ejecutar instrucciones en la cpu. Esa alternativa se implementó mediante un nuevo conjunto de
instrucciones. 
[Cómo funcionan las instrucciones virtualización con el hypervisor y el virtualizador](https://binarydebt.wordpress.com/2018/10/14/intel-virtualisation-how-vt-x-kvm-and-qemu-work-together/)
para más info. Como comentan en el artículo enlazado existe otro conjunto de instrucciones de virtualización para
acceder a dispositivos de entrada/salida.

### Microcode

El [microcode](https://en.wikipedia.org/wiki/Microcode) es una capa de emulación dentro de la propia cpu. En
las arquitecturas que lo implementan existe una cpu simple dentro de la cpu (RISC).
Ese diseño interno simple permite tener un core
bien optimizado y más fácil de probar para garantizar que está libre de bugs. Pero la cpu hacia el exterior se comporta
como si tuviera un conjunto complejo de intrucciones (CISC), implementadas en esta capa de emulación que transforma los
opcodes en complejos en conjuntos de instrucciones más simples del cpu central RISC. Esta transformación realizada en el
microcode es programable en el caso de las cpu arquitectura x86, permitiendo corregir algunos bugs sin la necesidad de
cambiar el hardware como esta 
[actualización de microcode para mitigar la variante 2 de spectre](https://support.microsoft.com/es-es/topic/kb4090007-actualizaciones-del-microc%C3%B3digo-de-intel-3bdf784b-d4ad-d881-cfc1-658095b59638).

## Capas de emulación

Como hemos visto varias veces en este artículo hay varios niveles de emulación y diferentes subsistemas a emular. Una
capa de emulación se refiere a emulaciones específicas. 

Un ejemplo significativo es el [diseño de WINE](https://en.wikipedia.org/wiki/Wine_(software)#Design) para ejecutar
aplicaciones de windows dentro de linux. La idea principal es implementar el ABI (Application Binary Interface) de
windows en modo usuario de linux, haciendo llamadas al propio kernel de linux cuando sea necesario. En el enlace
comentan compatibilidad con la API de windows pero no con sus drivers. Cosa que es bastante normal si pensamos en que
es más fácil emular lo que se ejecuta en modo usuario, con niveles de privilegio de cpu reducidos, a temas en kernel
que implican privilegios mayores, cambios de contexto y demás complejidades. [Diagrama de la separación modo usuario
y kernel en windows](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode).
El cliente Steam de linux usa por debajo este tipo de tecnología para instalar las versiones de windows de juegos que
carecen de implementaciones nativas en linux. Esto permite a los usuarios de linux disfrutar de más juegos y a los
desarrolladores de videojuegos ahorrar costes no haciendo versiones de linux. De todas formas se agradecen las
versiones nativas de linux :wink:.

Otro ejemplo de capa de emulación es PipeWire que expone APIs compatibles con ALSA, PulseAudio y Jack para asegurar 
la retro-compatibilidad de las aplicaciones que dependen de esos sistemas de sonido anteriores.

[Shim](https://en.wikipedia.org/wiki/Shim_(computing)) es la técnica de interceptar llamadas a APIs para implementar
una capa de compatibilidad o cambiar cierto comportamiento. En el enlace se muestran varios ejemplos como el uso
de polyfils en javascript para implementar APIs nuevas en navegadores antiguos o la técnica del LD_PRELOAD en linux,
que usa [el gestor de memoria jemalloc](https://github.com/jemalloc/jemalloc/wiki/Getting-Started) 
para cambiar el comportamiento de malloc cuando es usado dinámicamente.

## Contenedores

Hemos visto el uso de contenedores para la distribución de una aplicación, con todas sus dependencias aisladas de
posibles conflictos con otras dependencias instaladas en el sistema base. ¿Pero qué pasa si extendemos la
metáfora de la aplicación a un sistema operativo que se ejecute sobre un conjunto de ordenadores? Necesitaríamos un
[scheduler](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/) que decidiera en que 
[nodo](https://kubernetes.io/docs/concepts/architecture/nodes/) de los que componen el cluster se ejecutan los 
[contenedores/aplicaciones](https://kubernetes.io/docs/concepts/workloads/pods/). Harían falta también 
[abstracciones de red para manejar las conexiones](https://kubernetes.io/docs/concepts/services-networking/)
dentro del cluster y hacia el exterior. También
[abstracciones de persistencia](https://kubernetes.io/docs/concepts/storage/) para almacenar los datos de las aplicaciones.
Y una [abstracción de configuración](https://kubernetes.io/docs/concepts/configuration/) para modificar sus
comportamientos. Acabaríamos teniendo un [sistema de paquetes](https://helm.sh/docs/helm/helm/), para ese hipotético
sistema operativo que funciona sobre varias máquinas,
capaz de manejar [dependencias](https://helm.sh/docs/helm/helm_dependency/) entre paquetes. Quizá suene muy loco o quizá
sea una tecnología que lleva varios años madurando :wink:

Los contenedores han cambiado la forma en la que se realizaban los despliegues y ha simplificado la gestión de configuración
de las máquinas:
* Se despliega instalando la nueva versión de un contenedor y borrando la previa si la hubiera. Ya no hay un flujo de
  instalación base y otro de actualizaciones.
* Se ha simplificado la gestión de la configuración: cada versión de imagen de contenedor nos abstrae la versión de cada
  una de sus dependencias. Ya no tenemos que preocuparnos de si dos aplicaciones dependen de librerías incompatibles entre sí.
* El mismo artefacto (singular!!!) de imagen que instalamos en un entorno se instala en otro inyectando una configuración diferente.
* Desplegar es más sencillo, por lo que escalar/desdescalar máquinas bajo demanda también.
* Y otra larga lista de ventajas que también se podían hacer con un uso adecuado de otras tecnologías, solo que ahora
  hacerlo mal es más difícil que hacerlo bien.

También se ha simplificado la generación de entornos de desarrollo, simplificado los sistemas de integración continua,...

Aunque no todo han sido ventajas. La simplificación también ha hecho que se menosprecie el trabajo de un buen administrador
de sistemas y muchos despliegues productivos se hacen como si fueran entornos de desarrollo :worried:

# Conclusiones

Gracias a la emulación podemos convertir nuestro ordenador en un sinfín de dispositivos diferentes (ordenadores,
consolas, microcontroladores,...). Y las diferentes capas de emulación (cpu, memoria, sistemas de ficheros, red,
dispositivos conectados,...) nos permiten hacerlo de formas más o menos
eficientes o aisladas.

Las emulaciones nos permiten ahorrar costes de prototipado, pruebas en diferentes entornos, mantenimiento de sistemas,...

Cada familia de emuladores es un pequeño mundo lleno de particularidades, posibilidades, tecnologías,... Este artículo
es una especie de bestiario sobre emulación que espero que sirva como breve introducción.