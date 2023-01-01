+++
title = "¿Qué tecnología usar para un blog?"
description = "Justificación de la tecnología utilizada para perpetrar este sitio"

[taxonomies]
tags=["tecnología", "web"]
+++

## Según una persona normal

Un amigo es periodista. De html y css sabe lo justo o menos. De programación, bases de datos, administración
de sistemas cero patatero. Eso si, su herramienta de trabajo es un ordenador y no le hace ascos a la tecnología:
el programa de maquetar las publicaciones, sus páginas web, usar Chat-GPT para quitarle trabajo repetitivo,...

Esto es lo que recomendaría Chat-GPT:

>Existen muchas opciones de tecnología que puedes utilizar para crear un blog. Algunas de las más populares son:
>
>    * WordPress: es un sistema de gestión de contenidos (CMS, por sus siglas en inglés) muy popular y fácil de usar. Tiene una gran cantidad de plantillas y plugins disponibles para personalizar tu blog.
>    * Ghost: es otro CMS que se enfoca en ofrecer una experiencia de usuario simple y rápida. Es ideal para bloggers que quieren publicar contenido con regularidad y sin demasiadas distracciones.
>    * Jekyll: es un generador de sitios estáticos que te permite crear un blog a partir de archivos de texto plano (como Markdown). Es muy rápido y seguro, pero requiere un poco más de conocimientos técnicos para usarlo.
>    * Squarespace: es una plataforma de construcción de sitios web que incluye herramientas de blogging. Es muy fácil de usar y ofrece una gran cantidad de plantillas y opciones de personalización, pero puede ser un poco más costosa que otras opciones.
>
> En resumen, la elección de la tecnología para tu blog depende de tus necesidades y habilidades técnicas. Si quieres una solución fácil de usar y altamente personalizable, WordPress o Squarespace pueden ser buenas opciones. Si prefieres una solución más ligera y rápida, Ghost o Jekyll pueden ser mejores opciones.

Y Wordpress es su primera opción también. Ha hecho un montón de webs a allegados comprando un dominio,
contratando un hosting que soporte Wordpress y configurándolo. En una tarde está listo. Si se pone exquisito
con los cambios te lo hace en una semana.

Alguna vez hemos hablado de colaborar y que yo haga la programación. Pero yo no quiero tocar Wordpress ni
con un palo largo del asquito que me da. No me gusta su código, ni lo que ofrece para extenderlo y hacer las
cosas que requerirían dicha colaboración. Ha tenido muchos problemas de seguridad (como todo código malo) y
muchos plugins son un despilfarro de recursos de los sistemas.

Es lo que le recomendaría a cualquier persona normal que quiera hacer un blog. Pero advertido está de ser
serio con las actualizaciones y no instalar cualquier plugin a lo loco. Configurándolo con cariño y mimando
las cachés se puede tener un sitio con millones de usuarios únicos en una máquina de 30 euros al mes. Doy fé ;-)

Usar servicios como blogspot u otros es una alternativa amigable para humanos a tener en cuenta. Pero siempre
teniendo en cuenta de no ser el cliente y no el producto de uno de esos servicios.

El stack quedaría así:

{% mermaid_diagram() %}
graph LR
web["Servidor Web<br/>(Apache o Nginx)"] --- wp["Wordpress"]
wp --- Database[(MySQL)]
{% end %}

## Según el arquitecto

En los últimos trabajos de desarrollador de software que he tenido algo así simple como un blog supongo que
hubiera supuesto para ello el siguiente stack tecnológico:

 * Proveedor en la nube tipo AWS, Azure o Google. Y a ser posible usar tecnologías que te casen con él.
 * Kubernetes y varias opciones de la [Cloud Native Foundation](https://landscape.cncf.io/) 
 * Funciones lambda a cascoporro. Para el que no lo sepa, es para conseguir lo que hacía la gente subiendo
   en php el siglo pasado pero en moderno.
 * Micro-servicios:
    * Publicación
    * Comentarios
    * Detección de spam en los comentarios
    * ... completar la lista hasta tener 30 o 40 micro-servicios y la mitad de las funcionalidades
      que una persona normal lograría en una tarde.

El stack simplificado podría ser algo parecido a esto (creado con ayuda de CHAT-GPT):

{% mermaid_diagram() %}
graph LR
    usuarios[Usuarios] --> cargador[Cargador de balanceo de carga]
    cargador --> servidor[WAF]
    servidor --> api[API Gateway]
    api -->|GET| servidor_posts[Servicio de posts]
    api -->|POST| servidor_posts
    api -->|GET| servidor_comentarios[Servicio de comentarios]
    api -->|POST| servidor_comentarios
    api -->|GET| servidor_perfiles[Servicio de perfiles]
    api -->|POST| servidor_perfiles
    api -->|GET| servidor_categorías[Servicio de categorías]
    api -->|POST| servidor_categorías
    api -->|GET| servidor_etiquetas[Servicio de etiquetas]
    api -->|POST| servidor_etiquetas
    api -->|GET| servidor_buscador[Servicio de búsqueda]
    api -->|GET| servidor_suscripciones[Servicio de suscripciones]
    api -->|POST| servidor_suscripciones
    api -->|GET| servidor_notificaciones[Servicio de notificaciones]
    api -->|POST| servidor_notificaciones
    api -->|GET| servidor_análisis[Servicio de análisis]
    api -->|GET| servidor_autenticación[Servicio de autenticación]
    api -->|POST| servidor_autenticación
    api -->|GET| servidor_publicidad[Servicio de publicidad]
    api -->|POST| servidor_publicidad
    servidor_posts --> base_datos_posts[(Base de datos de posts)]
    servidor_posts --> redis_posts[Redis caché de posts]
    servidor_comentarios --> base_datos_comentarios[(Base de datos de comentarios)]
    servidor_perfiles --> base_datos_perfiles[(Base de datos de perfiles)]
    servidor_categorías --> base_datos_categorías[(Base de datos de categorías)]
    servidor_etiquetas --> base_datos_etiquetas[(Base de datos de etiquetas)]
    servidor_suscripciones --> base_datos_suscripciones[(Base de datos de suscripciones)]
    servidor_notificaciones --> base_datos_notificaciones[(Base de datos de notificaciones)]
    servidor_análisis --> base_datos_análisis[(Base de datos de análisis)]
    servidor_autenticación --> base_datos_autenticación[(Base de datos de autenticación)]
    servidor_publicidad --> base_datos_publicidad[(Base de datos de publicidad)]
{% end %}

Por supuesto le faltaría un kafka para enviar datos entre micro-servicios, toda la infraestructura de
k8s donde se desplegaría, con su sistema de monitorización, alertas y logging, su...

La verdad es que como pet project está muy bien para mantenerse al día con las tecnologías empresariales
más usadas hoy en día, pero desde cualquier otro punto de vista es un auténtico disparate.

Pero la mayor desventaja de todo esto es la cantidad de side projects que salen de todo esto. Y no hablo solo
de los helms para desplegar los micro-servicios, la integración continua y demás parafernalia para mantener
semejante cipote. El primer side project sería atracar bancos o algo parecido para poder costearme vivir
mientras dedico semejante esfuerzo en esta nimiedad.

Te extrañe o no, el mundo empresarial actualmente funciona así. Supongo uno no se hace arquitecto pintando
diagramas como el del stack que usaría y podría entender una persona normal. Tampoco los arquitectos tienen
que programar lo que planean. Aunque la verdad es que en algunos trabajos tampoco los desarrolladores programan
lo que el arquitecto proyecta. En mi último trabajo los arquitectos diseñaron la sagrada familia y los
desarrolladores estaban haciendo unas chabolas en los mismos plazos. En el anterior a ese el arquitecto
proyectaba brutalismo con aluminosis que se derrumbaba solo cuando algún programador le hacía caso (se ponía
muy canso para conseguirlo).

No he trabajado en ningún caso de éxito con este tipo de enfoque.

Sí que he trabajado en gestores de contenido que manejaban las webs de 40.000 clientes o en sitios con una
barbaridad de visitas al día. Ambos monolitos con unos pocos servicios de apoyo para tareas concretas:
redimensionado de imágenes, motor de búsqueda, filtro anti-spam,... y poco más.

Si estás pensando hacer el nuevo blogger o medium te recomendaría empezar con un monolito modular y cuando
tengas la velocidad del equipo comprometida pasar algunos de los módulos a micro-servicios para dividir
el trabajo en equipos más pequeños. Pero recuerda:
define bien las boundaries (fronteras entre los módulos) y recuerda que los sistemas distribuidos son una
cosa peligrosa que requieren el mismo cuidado que tienen en su trabajo los expertos en voladuras con
explosivos.

## Lo que haría alguien minimalista

Si el usuario va a ver HTML guapeado con un poco de css escribe el blog en HTML y CSS. Cero tonterías. En
informática hay un principio de desarrollo llamado KISS (Keep It Simple Stupid, mantenlo simple estúpido).

El código más seguro es el que no se ejecuta y te evitas esos problemas existentes con Wordpress
en el que hace falta código para procesar las solicitudes, leer datos de la base de datos y
renderizar el contenido en HTML.

El código más rápido es el que no necesita ser ejecutado. Tu página será tan rápida o más que lo que pueda
conseguir un sistema dinámico con el contenido en caché. El que te veas obligado a escribir el DOM del HTML
a la hora de escribir el contenido también te obligará a hacer algo simple, que normalmente se renderizará
más rápido en el navegador del usuario. La experiencia del usuario puede ser genial.

Por desgracia no todo es jauja. El día que quieras cambiar la presentación del contenido es posible que la
labor sea titánica. Eso ha pasado muchas veces en el pasado: contenido que ha de verse bien en un móvil,
pantallas retina en el que las imágenes se ven mal si no las tienes reescaladas pensando en los puntos por
pulgada de las pantallas del cliente,...

Un blog así será por definición estático. Si quieres tener contenido dinámico: comentarios de usuarios,
un buscador, listados (por tags, categorías,...). Va a ser un trabajo que va a necesitar de partes dinámicas:
si tiras mucho de php igual acabas haciendo un Wordpress, uso de servicios que se integran con javascript
en el cliente, scripts que te ayude a crear esos HTML repetitivos de forma automática...

Esta opción es inviable, pero los pros son demasiado buenos como para no darle una vuelta.

## Lo que he montado

Hay otro tipo de tecnologías para tener todo lo bueno de las versiones minimalistas con el mínimo de sus
problemas: los estatificadores. Y hay [muchas opciones](https://jamstack.org/generators/).

Un antiguo compañero escogería [next.js](https://nextjs.org/) porque es el que más estrellitas tiene en github. 
En mi caso no lo conozco, ni quiero tocar nada que tenga que ver con javascript. La gente que trabaja con
esa tecnología está loca y me ha dado muchos quebraderos de cabeza y ardores de estómago en el pasado.
No quiero andar con actualizaciones de node, dependency hell en cada actualización, diseños pesados,...

Uno con el que he trabajado y es una maravilla es [gohugo](https://gohugo.io/).
Está hecho en golang, muy rápido y la instalación es un simple ejecutable. Es completísimo y tiene de todo, a
pesar de eso puedes leer la documentación completa en un día. Tiene miles
de temas hechos por la comunidad disponibles, por lo que si no quieres maquetar vas a poder buscar el
diseño que más se ajuste a lo que quieres conseguir. Es la opción que hubiera elegido si no quisiera usar
este blog como excusa para volver a tocar temas de maquetación (HTML, CSS y algo de javascript tal vez),
pero usa las plantillas de golang que me dan cancer de sida.

Al final me he decantado por [zola](https://www.getzola.org/) por ser parecido a gohugo y tener un sistema
de plantillas más amigable. Es más simple que gohugo, por lo que para leer la documentación completa basta
una tarde. Espero no arrepentirme al echar de menos alguna funcionalidad. Pero de ser ese el caso prefiero
comerme una migración a gohugo cuando tenga el diseño estable que tener que hacer cambios con el sistema
de plantillas de gohugo.

Mi elección ha priorizado la sencillez a las funcionalidades. He decidido que no necesito comentarios: si
alguien quiere comentarme algo podrá hacerlo a través de una forja de código. Por desgracia casi todos los
comentarios son spam. Tampoco quiero nada que requiera contenido dinámico o tener que poner avisos de
aceptación de cookies. Llegado el caso podría programar un servicio para los comentarios y una maquetación
para la aceptación de cookies y demás burocracia. Así que tampoco es un problema insalvable. Por ejemplo,
los diagramas que aparecen en esta página están generados en texto plano y he hecho unos cambios al tema
para que si hay algún diagrama en la página cargue dinámicamente mermaid (un módulo en javascript) para
renderizarlos.

Estuve barajando [gemini](https://gemini.circumlunar.space/), pero la generación de contenido es demasiado
espartana y limita mucho. Tampoco me acabó de convencer el que en cada petición se corte el canal seguro y
haya que hacer un nuevo handshake en cada petición, es necesario para garantizar la privacidad, pero tampoco
creo que sea suficiente. Le tengo un poco de amor odio, pero recomiendo su lectura.


