+++
title = "Homelabs, ¿Cómo evitar acabar montando uno?"
description = "Los homelabs son una cosa molona para aprender cosas, pero también un agujero negro de tiempo."

[taxonomies]
tags=["tecnología", "sysops"]
+++

# Qué es un homelab

Un homelab es un laboratorio de informática que te montas en casa. Empiezas con un ordenador para cacharrear y
acabas con un CPD entero.

No hay nada de malo en montar un homelab. Al contrario. Es una tarea muy recomendable para aprender y practicar un
montón de conceptos que te resultarán muy útiles si te dedicas a la informática. Lo malo es cuando te engañas a ti
mismo diciendo que lo necesitas para hacer algo de utilidad. Engañarse a sí mismo está mal.

Todo empieza de una forma inocente con alguna frase del estilo a:

* Estaría bien tener un media-center para poner al lado de la tele y ver las pelis/música/... que tengo
* No vendría mal un NAS para que cada miembro de la familia pueda hacer sus backups y de paso tener todo accesible por
  red.
* En vez de pagar un hosting carísimo podría aprovechar que me sobra ancho de banda para montar algún servicio chorra
  que solo van a ver 4 personas en una rpi casera.
* Voy a meter un firewall para que los ordenadores de casa estén seguros.

Da igual como de ingenua sea la propuesta inicial, porque vas a acabar con:

* Un ordenador que suplirá al router de tu proveedor de internet con cientos de opciones de configuración de redes
  disponibles.
* Un NAS con varios discos en RAID para almacenar datos.
* Otro ordenador conectado a la tele para ver películas, jugar a consolas viejunas,...
* Un ordenador tocho para montar un cluster en máquinas virtuales o varios ordenadores pequeños para montar el cluster
  directamente.
* Switches y cables de red a tutiplen.
* Otro ordenador para la domótica de la casa y más cables, esta vez de corriente, para alimentar los enchufes 
  inteligentes.
* ¿?

Y esto pasa porque los informáticos somos unos ansias y no podemos parar. Nada es suficiente porque siempre hay un
pequeño cambio de nada que mejoraría la situación. Si, esta guapa la colección de música del home cinema del salón,
pero quiero ponerme esa música para currar sin cargar la VPN del trabajo que va como el orto. Así que al home cinema
le metes streaming de música. Y mola un puñado, no tiene anuncios, lo quieres para meter música al móvil. Y luego
streaming remoto, para lo que necesitas abrir puertos y...

# Las piezas que acabarás montando

## Comunicaciones

En el mejor de los casos te vas a limitar a entrar en el router que te ha dado la operadora para configurar el cliente
de dyndns para que la ip dinámica no impida acceder a tu server, algún port forwarding a algún equipo local y habilitar
el UPnP para abrir de forma automática puertos.

Puede que vayas un pasito más allá y te conformes con instalar un router compatible con [OpenWRT](https://openwrt.org/).
Dado lo limitado en recursos de la mayoría de esos chismes tampoco vas a volverte demasiado loco con las configuraciones.
Tal vez un chilispot, un dns interno, una VPN,... poca cosa.

Pero se te acabará quedando pequeño y comprando un ordenador con al menos 2 tarjetas de red. Puede que sigas con el
OpenWRT o, ya que tienes hardware más potente, por qué no probar [pfsense](https://www.pfsense.org/) u 
[opensense](https://opnsense.org/). Porque quien no quiere tener un IDS/IPS, dns que resuelve nombres de la red
interna, un dhcp que te puede fijar ips en función de la mac, un ha-proxy para hacer de reverse proxy a cada uno de los
servicios que tienes en ordenadores dispersos por la casa, configuración sencilla de Lets-encrypt, monitorización de
red,...

Al final vas a tener también un switch (o varios) para conectar con los equipos de casa. Por qué no probar un
switch manejado.

## Almacenamiento

Tener un disco para hacer backup es lo mínimo a tener. Pero ya sabes que los discos acaban fallando y hacer dos copias
manuales es un coñazo. Lo suyo es que esos discos estén en RAID. Y si ya de paso tienes un sistema de ficheros con 
opción de snapshots para volver a un estado anterior te evitas perder datos cuando borras por error. 
Y por qué no tener esa información accesible en todo momento en la red local. Y más cuando hay distros como 
[TrueNAS](https://www.truenas.com/) para montarlo y manejarlo de forma sencilla. Sobre todo cuando a golpe de click
puedes tener montado un [NextCloud](https://nextcloud.com/es/).

## Media server

Igual con un cromecast crees que es suficiente para enviar a la tele algún video. Y puede que hayas leído que emby
permite hacer streaming a este de forma fácil, aparte de que lo puedes instalar en el NAS a golpe de click. Pero luego
te das cuenta de que esos servicios corren dentro de un contenedor, que el protocolo dlna hace multidifusión pero esta
no llega a dentro de los contenedores y empieza la guerra: configurar la red o ya puestos montar un media-server con
todo. Todo implica ver pelis, escuchar música, ver el tiempo que va a hacer, retro-gaming,...

Y para eso lo mejor es montar un equipo dedicado que obtenga los datos del NAS. Tampoco tienen que ser muy tocho, ni
consumir mucho, ni estar todo el día enchufado. Y si es pequeño como una raspberry pi y te quitas de ventiladores
mejor que mejor. Y si la raspberri pi se queda pequeña, pues un ordenador formato nuc.

## Cluster

Al final has visto que 10 o 12 servicios que molan mucho. Tienes para organizar la música, las películas, las fotos,
los libros, para descargar de forma automatizada, para... Y todos esos servicios no caben en el media-server
(recuerda que tiene que ser liviano para no hacer ruido que moleste), ni en el router/firewall/..., ni en el NAS
porque aunque creías que iba sobrado de RAM la mitad se la está comiendo el caché de ZFS. Tampoco los vas a montar
en tu ordenador personal. Y ya que estamos por qué no montar un cluster y probar el kubernetes ese que se usa en todos
los lados y aprenderlo.

Una opción es comprar un ordenador muy tocho y dividirlo con máquinas virtuales, contendores,... Otra opción es montar
varios [pequeñitos](https://www.amazon.es/Fanless-Computer-Ethernet-AWOW-NY41S/dp/B09F3BJNPH/ref=sr_1_8?keywords=mini+pc+stick+windows+10&qid=1682212822&sr=8-8)
y así pagas la inversión a plazos.

Lo peor de entrar en esto es que tener 10 o 12 servicios molones corriendo no te hace feliz y entras en los siguientes
agujeros negros de tiempo:

* Tener 10 o 12 servicios con sus 10 o 12 registros del mismo usuario te parece un dolor. Eso es porque no has empezado
  todavía a configurar un sistema de autenticación única para esos 10 o 12 servicios.
* Que los servicios estén levantados cuando el 99% del tiempo están ociosos te parece un desperdicio. Pero eso es porque
  no has mirado el equivalente de k8s de tener un servicio systemd que se levanta con la primera petición y un script
  que lo mata si su access log no ha crecido en, por ejemplo, media hora.
* Como guardar la config de todo ese jaleo en un git o similar porque como el cluster se vaya a la mierda no vas a
  querer repetir la paliza de configuración.
* Que usar helms que alguien hizo es muy rápido, pero tener una bb.dd. levantada por servicio es villano y complica los
  backups. Ver como parametrizar esos helms o hacer los tuyos propios ya no es tan rápido.
* Cómo agilizar tu ciclo de desarrollo para que termine corriendo en ese cluster del averno.

## Domótica

Y ya que estás qué más te da 8 que 80. Por que no meter un cliente wifi en cada enchufe y un 
[home-assistant](https://www.home-assistant.io/) para dominarlos a todos. Total, solo es un servicio más en tu cluster.
Así puedes monitorizar también todo el mineral de luz que estás gastando en todas esas máquinas que tienes enchufadas.
Y por qué no, automatizar subir y bajar las persianas para ahorrar energía, que con el tiempo que dedicas al homelab
ya no tienes tiempo de hacerlo tu mismo.

## Extras

Un dominio con opción de dynDNS para poder acceder desde el exterior a tu homelab.

Guarda toda la config en repos en la nube para poder volver a montar todo si te va al garete.

Guarda los datos también en la nube, porque fijo que no has dimensionado correctamente la acometida energética y vas
a terminar quemando tu casa y los datos del NAS con ella.

# Alternativas

## Tailscale

Te ahorras el necesitar de un dominio propio. Si quieres compartir una carpeta o un servicio de tu máquina es solo un
comando. También te da una forma fácil de copiar ficheros entre máquinas. Lo configuras en una tarde y te resuelve
varios de los problemas que se complican con otras alternativas:

* Abrir puertos. Él se encarga de hacer [hole punching](https://en.wikipedia.org/wiki/Hole_punching_(networking)).
* Proxy inverso. Accedes directamente al dispositivo que provee el servicio.
* Securizar la red. Es mayormente una VPN.
* ...

## Simplifica y racionaliza

Monta un servicio por aprender, pero piensa dos veces si realmente lo necesitas a la hora de mantenerlo levantado. Eso
implica una inversión hardware, costes de energía, esfuerzo personal para mantenerlo,...

Tener cosas a instalar a golpe de clic y espacio abundante suele llevar al Diógenes digital. Y da igual lo grande que
sean los discos, porque el Diógenes digital puede llenarlos todos.

Si no estás dando un servicio productivo no necesitas auto-escalado, monitorización, alta disponibilidad,... Con que
hagas backup de lo que realmente importa es suficiente. Si un docker-compose te sirve no montes un cluster de k8s solo
porque en empresas con personal dedicado a mantenerlo lo usan.

## Centraliza en un solo trasto

Con tailscale te puedes ahorrar la necesidad de configurar comunicaciones. Pero si quieres montar un NAS y un cluster
para montar servicios locales comprar una torre potente es una buena opción. Un ordenador con bastante RAM y discos
duros corriendo [proxmox](https://www.proxmox.com/en/) te proporciona opciones ilimitadas de cacharreo. Te quitas de
un montón de enchufes (de ordenadores peques, cajas de discos, switches) y cables (no hacen falta cables para
interconectar máquinas virtuales o contenedores Linux). Te sale más barato y reinstalar el software de una máquina
virtual es mucho más rápido. Aparte de que tienes snapshots para dar una marcha atrás fácil cuando te equivocas con algo.

Si te da por cacharrear con cosas de deep learning y necesitas una gráfica brutal es fácil ampliarlo.

# Conclusiones finales

Un homelab es una plataforma de aprendizaje brutal, pero por mucho que te flipes (o a no ser que te flipes mucho
montándolo) no es un entorno decente para correr servicios. Antes de montar uno piensa si lo que quieres es aprender u
obligarte a hacer un montón de tareas de mantenimiento.