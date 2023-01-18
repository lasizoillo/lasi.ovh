+++
title = "97 cosas que todo programador debería saber"
description = "Apuntes de la lectura del libro 97 cosas que todo programador debería saber"

[taxonomies]
tags=["tecnología", "libro"]
+++

# Sobre el libro

Un libro que se titula "97 cosas que todo programador debería saber" tiene una pinta de click-bait que tira para atrás y
he de reconocer que en eso no defrauda. De las 97 "cosas", varias están repetidas hasta la saciedad. Algunas son
tratadas de formas contradictorias en diferentes apartados. Reconozco que no me arrepiento de haber comprado el libro
porque en el [Humble Bunde](https://www.humblebundle.com/books) que lo compré había otros títulos interesantes.

No recomendaría la lectura a alguien que quiera aprender a programar, porque aunque se dicen cosas muy interesantes,
el nivel de detalle con el que se tratan los temas es superfluo. En algunos apartados aparecen referencias a
algunos temas de los que se hablan, pero en general faltan referencias que ayuden a los programadores a
profundizar sobre el tema expuesto. Se habla mucho de ser positivo, jugador de equipo,... pero me da la impresión
de que hay más comentarios hablando de que errores suelen cometer los juniors, que información útil para que eviten
seguir cometiéndolos. Tampoco creo que muchos de los errores sean exclusivos de los juniors. Hay de hecho un tema no
tratado que ningún junior echará en falta, pero que estoy seguro forma parte del top 97 cosas a saber: ¿cómo lidiar
con el burnout?

Quizá sea más provechoso como inspiración a sacar un tema con el que conversar con un colega programador
a lo ilustre ignorante. Aprovechar las anécdotas del libro para soltar las propias, matizar ideas, crear polémica sobre
alguno de los temas,... creo que puede dar mucho juego entre cerveza y cerveza o entre café y café.

# Cada capítulo por partes

A veces me limito a resumir lo que he entendido que el autor quería decir. La mayoría de las veces dejo que el texto
me inspire a soltar mi pedrada de ilustre ignorante. No juzguen a ninguno de los colaboradores del libro por mis
exabruptos, incorrecciones, malinterpretaciones,... o cualquier otro tipo de falta. Tampoco me juzgues a mí,
sé proactivo y crea un pull request o ticket en
[https://github.com/lasizoillo/lasi.ovh](https://github.com/lasizoillo/lasi.ovh) con lo que creas que puede ser
mejorado.

## Act with Prudence

Que no te hagas trampas al solitario metiendo deuda técnica para llegar a tiempo con los plazos,
pero si lo haces que lo arregles cuanto antes porque es como una deuda con intereses. Estoy de
acuerdo. Interesante la distinción de deuda técnica metida a idea o sin querer queriendo.

## Apply Functional Programming Principles

Habla de lo chupi guay que es la programación funcional (PR) porque la 
[transparencia referencial](https://es.wikipedia.org/wiki/Transparencia_referencial) es lo más,
que lo es. Termina diciendo que la PF y la POO son como el ying y el yang, aquí he echado en
falta una referencia al [expression problem](https://wiki.c2.com/?ExpressionProblem)

## Ask, “What Would the User Do?” (You Are Not the User)

Piensa en el usuario, pero como tú no eres como el usuario debes juntarte con él para ver lo que quiere y una vez
hecho para ver si es usable para él. Condición necesaria para hacer código production ready, que no
tiene nada que ver con el que habitualmente se sube a producción.

## Automate Your Coding Standard

Que automatices el uso de formateadores de código, linters y otros analizadores estáticos de código.
No se moja en cómo conseguirlo y las políticas de este estilo mal implementadas son un desastre para
el DX (Developer eXperience), la productividad y la unidad del equipo. Mucha suerte si te toca en el
equipo dos personas con TOC, una que quiere las cadenas con comillas simples y otra con dobles :facepalm:.

## Beauty Is in Simplicity

La belleza es algo subjetivo, revisa código de otros (sobre todo de eminencias) y enseguida descubrirás
lo que es la belleza y su relación con la simplicidad. Estoy de acuerdo, si algo caracteriza el
código de los programadores novatos y los malos programadores es lo que se complican para hacer
cualquier cosa.

## Before You Refactor

Ante todo mucha calma. Trata de evitar ser Nerón y quemarlo todo para hacerlo de cero. Sé humilde y
apóyate en los tests para garantizar que lo tuyo sigue funcionando. Es fácil cometer errores y crear
regresiones de ese código que lleva años evolucionando. Estoy bastante de acuerdo. Mi heurística es:
si puedes rehacerlo todo en el tiempo que tardarías en modificar lo que necesitas hacer es la hora
de hacer el Nerón. En el resto de los casos trata de ser conservador y medido con los cambios.

## Beware the Share

El autor cuenta una batallita de como refactorizó un código para hacerlo reutilizable aplicando
todas las buenas prácticas aprendidas en la carrera y como ese código fué brutalmente rechazado en la
review. Él achaca el tema al contexto de aplicación de las buenas prácticas, pero sin dar una guía
clara para que nuevos juniors puedan aprender de su error. Su enemigo era el acoplamiento y en
[https://connascence.io/](https://connascence.io/) puedes encontrar un bestiario para reconocerlo
y aprender a evitarlo. Por desgracia en el libro se queda en la anécdota.

## The Boy Scout Rule

Deja las cosas mejor que como te las has encontrado. Por desgracia no ayuda a saber qué es lo está
bien y lo que está mal. Igual el simil de ver a un boy scout pasando la cortadora de cesped al lugar
de acampada te parece muy tonto, pero es lo que hace mucha gente bien intencionada con el código
cuando trata de arreglarlo, como la anécdota del apartado anterior y el rechazo brutal que recibió.

## Check Your Code First Before Looking to Blame Others

Si encuentras un error al compilar tu código lo más probable, que no seguro, es que esté en tu
código y no en el compilador. Así que lo más barato es centrarte en encontrar el problema en lo
que has hecho antes de echar balones fuera. A mí una vez me llamaron la atención por llamar
"inútiles payasos malcriados" a unos programadores que echaban las culpas a negocio por especificar mal,
a QA por no encontrar los errores, al cliente por no saber usar la aplicación, a los devops por
no auto-escalar debidamente las máquinas para ejecutar ese código O(n^2), a las librerías que
usaban,... porque ellos eran perfectos :facepalm:.

## Choose Your Tools with Care

Otro que se da cuenta de que las dependencias pueden convertirse en un problema y describe sin darse
cuenta lo que es el acoplamiento. Os recuerdo esto [https://connascence.io/](https://connascence.io/).

## Code in the Language of the Domain

Si tu ves:
```java
if (portfolioIdsByTraderId.get(trader.getId()).containsKey(portfolio.getId())) {...}
```
te tienes que estrujar la cabeza para entender la intención de ese fragmento de código. Pero si ves:
```java
if (trader.canView(portfolio)) {...}
```
todo queda claro cristalino. Mi recomendación es que aprendas DDD del 
[libro de Evans](https://www.amazon.es/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
y seas muy crítico con todos los libros sobre DDD que llevan la palabra "microservicios" en el título.

## Code Is Design

Hacer código es diseñar y eso implica un proceso creativo difícil de estimar. En otras ingenierías el
diseño y su construcción están separadas y es fácil estimar cuanto te cuesta construir una pieza a
partir de su diseño. Hay una presión por sacar a producción diseños inacabados e incorrectos para
conseguir una ventaja competitiva, pero implica una serie de costes asociados a la baja calidad de
despliegues que no son *production ready*. Estoy de acuerdo y tener esto en cuenta a la hora de
programar no es poner la venda antes de tener la herida. Los tests, por ejemplo, ayudan a que los
posteriores arreglos no estropeen más el producto.

## Code Layout Matters

Pasas más tiempo leyendo código que escribiendo. Por lo que la legibilidad del código redundará en la
productividad de todo el equipo de desarrollo. Estructuras claras (somos buenos detectando patrones),
formateo expresivo (los formateadores automáticos de código no conocen de intención y te limitan
en esto) y código compacto (que quepa en la pantalla sin necesitar de scroll o saltar a otros ficheros)
son convenientes. En [Code in the Language of the Domain](@/blog/2023-01-01-97-cosas-que-todo-programador-deberia-saber.md#code-in-the-language-of-the-domain)
dan pistas de como conseguirlo.


## Code Reviews

Evita convertir las code reviews en tribunales o ser sarcástico. Trata de usarlas como herramienta
para compartir conocimiento, unificar guías de estilo y aprender unos de otros. Este apartado
es muy importante porque muchas veces se hace por cargo cultismo sin aprovechar su utilidad.

## Coding with Reason

Da un conjunto de heurísticas, más o menos discutibles, sobre cómo hacer código sensato. Comenta que varias
pueden comprobarse con una herramienta de análisis estático de código. Que sean más o menos
discutibles creo que las hace adecuadas para discusiones meta-físicas con otros programadores,
no tanto para ayudarte a hacer mejor código.

## A Comment on Comments

Me quedo con esta frase:

> Your header comments should give any programmer enough information to
> use your code without having to read it, while your inline comments should
> assist the next developer in fixing or extending it.

No voy a comentar nada más porque un código legible es mejor a una explicación farragosa :wink:

## Comment Only What the Code Cannot Say

Comentar es una habilidad en sí misma y la parte más difícil es saber que no comentar. Los
comentarios erroneos no hacen que el compilador o la aplicación falle, o que no pasen los
tests, pero son dañinos para la mantenibilidad del código. Trata los comentarios como si
fueran código, lee y corrige cuando corresponda para mantenerlos correctos.

Lee también el sub-texto. Un comentario te puede marcar un refactor. Una función larga con
comentarios de diferentes pasos pueden refactorizarizarse en una función corta que llama
a los diferentes pasos, que son funciones con un nombre descriptivo.

## Continuous Learning

Heurísticas para mantener al día aprendiendo cosas.

## Convenience Is Not an -ility

Lo que te resulte conveniente no es un requisito no funcional para hacer mejor código.
No te dejes llevar por la vagancia (por ejemplo, meter un flag en una función para hacer cosas
semánticamente diferentes) para ahorrar unas líneas de código. Guíate mejor por la consistencia,
eficiencia y elegancia (o por cualquier otro requisito no funcional de los de verdad).

## Deploy Early and Often

Dice que un despliegue simple y correcto es necesario (estoy de acuerdo), pero no da ninguna
pista de cómo conseguirlo. Aquí entro yo: Lo mismo que los alcoholicos tienen los 12 pasos
para dejar de beber, los programadores deberían conocer sobre [Twelve-factor](https://12factor.net/es/)
para evitar que el responsable de las releases y la gente de sistemas no caigan en las drogas.

## Distinguish Business Exceptions from Technical

:clap: :clap: :clap: :clap: Gran capítulo que todo programador debería leer y conocer. Habla de las
diferencias entre excepciones de la capa de negocio (validaciones, cambios de estado no permitidos, ...)
de los errores en la infraestructura (no poder conectar a la base de datos, te quedastes sin memoría,...).
Y deja entrever la correcta jerarquía de errores y como defenders de errores que no tenías contemplados
para asegurar la estabilidad de los sistemas. Trataré de leerme su libro "Secure by Design" porque este
señor aquí a ido a lo concreto dando información muy útil.

## Do Lots of Deliberate Practice

Busca marrones prácticos con el objetivo de aprender. No con el objetivo de tener las cosas hechas
que es el objetivo en los trabajos. Aprender. Y para eso hay que hacer ese ejercicio en el que
aprendes algo una y otra vez. No hacer lo mismo una y otra vez, sino diferentes variaciones que
anticipen cualquier cosa que te puedas encontrar en ese dominio. Después de unas 10.000 horas
serás un experto. Aprende cosas que te cambien a ti, que cambien tu comportamiento.

## Domain-Specific Languages

Pues sí, estoy de acuerdo, los DSL's es algo que todo programador debería conocer. Hay dos tipos de
programadores, los que buscan la herramienta óptima para un fin y los que tratan de engañarse
a sí mismos con que su lenguaje favorito es la mejor herramienta para todo. Me los suelo imaginar
cortando la carne con un martillo o haciendo un código de 20 líneas en donde otros lo resolverían
con una regex de 10 caracteres.

## Don’t Be Afraid to Break Things

No dice que seas un sudas, dice que para hacer una tortilla hay que romper algunos huevos. Y es verdad,
muchas veces es mejor pedir perdón que pedir permiso. Profesionalmente he encontrado que cuando
hay un código que te dicen que no hay que tocar porque es muy complejo y podría romperse todo es por
donde hay que empezar a refactorizar para arreglar las cosas. Si es complejo e inmantenible lo más
probable es que esté mal hecho.

## Don’t Be Cute with Your Test Data

Que escribas tu código privativo como si fuese software libre porque nunca sabes cuando va a haber
una filtración. Vale, estoy de acuerdo, pero no el puritanismo de algunos ejemplos. Para mí
"// TERRIBLE HORRIBLE NO GOOD VERY BAD HACK" es transparencia, el que debería sonrojarse es el
manager que no dedicó recursos a pagar deuda técnica cuando se filtró el código fuente de Windows.

## Don’t Ignore That Error!

Por vagancia sé estricto tratando los errores, si eres vago a la hora de tratarlos tendrás que
trabajar mucho más en el futuro. Totalmente de acuerdo. He visto perder millones en contratos por el
enfado de un cliente producido por un mal tratamiento de errores, días de depuración para encontrar
errores por no estar controlados, semanas de trabajo identificando y arreglando millones de registros
erroneos en base de datos,... Se vago, se estricto tratando los errores. Nunca los enmascares.

## Don’t Just Learn the Language, Understand Its Culture

Un lenguaje es algo más que su sintaxis. Puedes escribir fortran en C# limitándote a escribir
funciones dentro del main, pero eso no es aprender C#. Lo mismo con el resto de los lenguajes.
Se recomienda aprender lenguajes porque te abren ideas de cómo atacar problemas desde un prisma
diferente. Y eso requiere de una inmersión más profunda en ellos.

## Don’t Nail Your Program into the Upright Position

Se pone literario y ahora sabemos por qué hay novela policiaca, pero no hay novela programadora :shrug:.

## Don’t Rely on “Magic Happens Here”

Cuando ves a cualquier experto en cualquier disciplina hacer las cosas parece fácil, pero no
lo es si no tienes ese nivel. Viene a decir que no te marques un Elon Musk y despidas a la
mitad de la plantilla porque no es necesaria, porque la magia desaparecerá y necesitarás alguna
forma de hacer que fluya otra vez.

## Don’t Repeat Yourself

DRY (Don't Repeat Yourself) aquí es tratado más como una forma de vida que como una heurística
para reducir el número de código duplicado. Estoy de acuerdo con el principio y con su
planteamiento, pero soy más estricto que él en los marcos de su aplicación.

Si usas scaffolding para generar tus proyectos vas
a tener un montón de código duplicado que habrá que mantener y un fallo en esa plantilla implicará
tener que repetir la solución en todas los proyectos generados así. Si utilizas una librería que abstrae
de ese copy&paste tendrás que actualizar la versión de la librería en todos los códigos que la utilicen.
Sí, el escenario mejora, pero no es jauja. Sigue habiendo repetición, pero no pongas paranoico tratando
de arreglarlo si solo tienes un puñado de proyectos, a veces un poco de repetición es un mal menor.

Si tienes un procedimiento repetitivo que te consume tiempo y lo automatizas dejarás de repetirte
a ti mismo haciendo ese mismo proceso o [tal vez no](https://xkcd.com/1319/)

Si te repites mucho haciendo algo probablemente te hace falta una abstracción. Si eliges las abstracciones
incorrectas te encontrarás en el trabajo repetitivo de agrupar código parecido, ver como diverge en
el futuro y repetirte a ti mismo separando ese código cuando reconozcas que tu abstracción era incorrecta
y estás haciendo algo infumable.

Ten siempre en cuenta DRY, pero sé muy crítico en su marco de aplicación. No te conviertas en el protagonista
de una viñeta de xkcd.

## Don’t Touch That Code!

Que los desarrolladores desarrollen en sus entornos y dejen el resto de los entornos tranquilos es más o menos
lo que viene a decir. Me vale a la hora de no tocar código a lo salvaje oeste, la receta para el desastre.
Nada de tocar a pelo en staging porque QA encontró un error o conectarse a la máquina de producción para hacer
un hotfix en caliente. Estoy totalmente de acuerdo con eso.

Me inquieta cuando entras en un trabajo y hay compañeros hablando de como optimizar algo, y que cuando preguntas
¿qué cardinalidad tienen esas tablas?, ¿cuántas requests por minuto de pico tienes?, ¿cuál es uso de memoria
con datos productivos de X proceso?,... Nadie sabe nada porque hasta las estadísticas de producción las tienen
capadas y es imposible que sepan si un join que están escribiendo va a generar una tabla temporal intermedia
de 2.000 millones de registros o de 500 :disappointed:

## Encapsulate Behavior, Not Just State

Habla de un error muy común diseñando software, se usa la orientación a objetos para encapsular el estado y
se olvida encapsular también su comportamiento. Luego ese comportamiento se implementa en métodos enormes
y acoplados con todo dentro de clases cuyo nombre termina en *Service*, *Handler* o *Manager*. Lección
importante para todo programador.

## Floating-Point Numbers Aren’t Real

Si no sabes lo que es el exponente y la mantisa de un número real deberías leer este apartado. No te vendría
de más leerte un manual sobre métodos numéricos en informática. :clap: :clap: :clap: este es uno de los pocos
apartados que enseñan de una forma objetiva y no es click-bait.

## Fulfill Your Ambitions with Open Source

Si te mola el desarrollo de software embarcándote en algún proyecto de software libre vas a aprender
un montón y te lo vas a pasar como un gorrino en una charca. Lee código de otros, escribe tests, encuentra bugs,
socializa con otros desarrolladores,...

## The Golden Rule of API Design

Mucho cuidadito con los API's porque lo que los cambios que les hagan pueden romper miles de códigos a los
que no tienes acceso para arreglarlos. Con los API's hay que pensar bien las cosas y hacer muchos tests.
Personalmente encuentro una irresponsabilidad el típico pretexto del MVP para hacer una cagada que vas a
tener que mantener hasta los restos.

## The Guru Myth

Excelente planteamiento. Si piensas que hay Gurus que te pueden responder los problemas sin darles el más
mínimo concepto surgen varios problemas:
* Las respuestas que recibirán podrán ser más o menos inteligentes, pero difícilmente óptimas.
* Estás poniendo una barrera mitológica a lo que tú, como programador podrás llegar a desarrollarte.

Hay gente que sabe más que ti. Pregúntales dándole el contexto necesario y aprende de ellos para mejorar.

## Hard Work Does Not Pay Off

No te flipes con sobre esfuerzos y jornadas extenuantes. Se rinde más respetando los descansos y
dosificándote para mantenerte al día durante los descansos.

## How to Use a Bug Tracker

Una manera positiva y razonable de como usar herramientas de seguimiento de errores. "Bugs are like a conversation"
dicen en el libro, por desgracia muchas veces es una pelea a gritos :pensive:.

## Improve Code by Removing It

Cuenta una anécdota de un código que eliminó porque iba muy lento y no hacía falta. El programador que lo
desarrolló lo hizo porque quiso, *porsiaca*, como guinda a un pastel que nadie le pidió, porque se inventó
requisitos que no existían en ninguna parte y además los inventó incorrectos o vete tú a saber por qué.

Conozco a pintores que me han reconocido que en más de un trabajo, con tanto disolvente, han salido pedos.
Ninguno de ellos me contó jamás una historia de alguno que se pusiera pintar cosas que no les hubiera
pedido el cliente. Con algunos programadores esto es bastante normal.

## Install me

Comenta lo ocupadito que está y como es importante que un software vaya directo al grano para conseguir ser instalado.
La verdad es que hay veces que visito la página de un software/librería, se me queda cara de vaca mirando al tren y
cierro la pestaña sin saber a qué se debían todos esos *WOW!!!* o qué es siquiera en lo que consistía. Se nota que
hay herramientas que no las paga el usuario final :laughing:.

## Interprocess Communication Affects Application Response Time

El autor explica que muchas veces el problema de rendimiento no está en las estructuras de datos o en los algoritmos
sino en el número de llamadas a sistemas externos y las latencias de estos y pone allí el foco a mirar primero.
Aunque en muchos casos tenga razón, sobre todo con la moda de los micro-servicios, comete un fallo garrafal a la
hora de optimizar: lo primero siempre es medir. Haz un profiling y luego ya vas a tiro hecho al algoritmo, a la
estructura de datos o a las llamadas a sistemas remotos que se realizan.

## Keep the Build Clean

No ignores los warnings que te lanza el compilador (o el intérprete al ejecutar los tests) y trátalos debidamente.

## Know How to Use Command-Line Tools

Los IDEs hacen mucha magia con un botón y mucha gente es dependiente de ellos, no sabe lo que ocurre por debajo,
no es capaz de hacer un build automatizado en un pipeline del CI porque no tiene ese botón mágico al que pulsar.
Usa un IDE si te facilita la tarea, pero sé consciente siempre de qué es lo que hace.

## Know Well More Than Two Programming Languages

Al conocer más de un lenguaje de programación y sobre todo más de un paradigma (estructurado, funcional,
orientado a objetos, lógico,...) te ayudará a amoldar la cabeza para solucionar los problemas de más de una
forma. Incluso cuando estés con un lenguaje orientado a objetos te será de utilidad lo aprendido en programación
funcional para hacer un código más declarativo. Puede que tu lenguaje no distinga entre variables mutables e
inmutables, pero haber trabajado con ella te amoldará el cerebro a tenerla en cuenta y hacer mejor código.

## Know Your IDE

Conocer tu herramienta de trabajo te hace más productivo y la vida más cómoda.

## Know Your Limits

Habla de por qué conocer los límites físicos es importante. Por un lado habla de órdenes de ejecución (O(n), O(log n),
...) y por otro de velocidades de acceso a diferentes memorias (registros de cpu, diferentes caches de cpu, ram,
disco, red). Y pone el ejemplo de como los árboles de van Emde Boas le paten el culito a otras estructuras de datos
por lo predecible que son los accesos a sus datos incluso teniendo la misma complejidad computacional (orden de ejecución).

Más y mejor en esta línea [este artículo](https://dl.acm.org/doi/10.1145/1810226.1814327) del creador de Varnish.

## Know Your Next Commit

Habla de enfocar tus tareas en unidades entregables (el commit como entrega) con un claro principio y fin *sabiendo*
lo que vas a hacer y tardar. Si pensabas que iba a ser una hora y luego son dos tampoco pasa nada. Y si después de dos
te pones a hacer un trabajo especulativo, menos enfocado en ese *se lo que tengo que hacer* también está bien
porque te va a ayudar a aprender. Y si te das cuenta después que te estás metiendo en algo que no te está llevando a
nada productivo no pasa nada por tirarlo y enfocarte en otra tarea más concreta y específica. Nunca se hace commits de
conjeturas, solo de trabajo que se sabe correcto. El contraejemplo es otro programador que decía que estaba trabajando
en una historia de usuario (muy bien por esto) y que iba a tardar un día o dos en hacerlo porque eso es trabajo 
especulativo (no tenía claro como conseguir ese objetivo).

Me quedo un poco de aquella manera digiriendo este texto. Por un lado, saber dividir el trabajo en pequeños hitos
más centrados te hace más productivo. Por otro lado, cuando ese entregable implica crear un ticket, asignarle puntos,
esperar su aprobación en un sprint, asignarlo, hacerlo, *commitearlo*, comentarlo, cambiar estado, esperar review,
hablar con QA sobre como probarlo y otras cere**moñeces** agile<sup>tm</sup> caemos en las mismas ineficiencias de las
latencias al llamar a sistemas remotos pero en el campo del *management*. Creo que este texto sin su debido contexto
pierde todo el sentido.

## Large, Interconnected Data Belongs to a Database

Un poco de cordura sobre las bondades de las bases de datos. Una tecnología madura, eficaz con grandes volúmenes
de datos, barata, con opciones embebidas, una DSL (el SQL) para procesar/analizar los datos sin tener que escribir
código, que te permite centrarte en las funcionalidades de tu código delegando el acceso de datos eficiente y concurrente,
datos que se mantendrán relacionalmente integros,...

## Learn Foreign Languages

Habla de la parte social del desarrollo de software, que no solo te tienes que comunicar con gente técnica, de
otras habilidades comunicativas como el saber escuchar y la comunicación no verbal. También habla que la vida
va más allá de lo que es la programación y otros idiomas te ayudan. Creo que viene a decir algo así como
"se menos ROBOT".

## Learn to Estimate

Habla de la importancia de saber estimar y para eso hace la siguiente distinción:
* Estimar es una aproximación tentativa de cuánto podría llevar hacer una cierta labor. No es algo preciso.
* Objetivo es un deseo marcado por gerencia o el cliente sobre lo que es esperado.
* Compromiso es una promesa de entregar una funcionalidad específica en un determinado
  nivel de calidad en una determinada fecha o evento.

La estimación debe ser una herramienta para deducir la viabilidad de invertir en una cierta funcionalidad.
Habla del problema de que los desarrolladores hablen de estimaciones y los managers lo interpreten como
compromisos. Por desgracia tiene toda la razón del mundo. Es por eso que cuando determinada gente me pide
fechas les arrojo un calendario de bolsillo y les digo "toma, ahí las tienes todas".

## Learn to Say, “Hello, World”

La moraleja que he entendido, si es que tiene alguna, es que dejes de fliparte con tu IDE y sus funcionalidades
y aprendas a pensar. Para ello probó a hacer un hola mundo con un editor de texto espartano. Lo que me
ha recordado a una batallita mía. Una persona a mi cargo estaba haciendo un uso ilógico la palabra reservada
*static* y le dije que escribiese un hola mundo en mi ordenador (en Java el *main* es *static*). Me
sorprendió que sin el Wizard de su IDE fuese incapaz de conseguirlo, pero entendí el por qué de sus
problemas. Quizá la historia no sea tan birriosa después de todo.

## Let Your Project Speak for Itself

Recomienda usar gadgets para dar un feedback físico de métricas sacadas en la integración continua. No voy a
hacer ningún comentario porque hace ya más de 30 años que dejé de tener 12 años.

## The Linker Is Not a Magical Program

Un caso concreto de: conoce qué es lo que pasa debajo de tu IDE. En este caso habla de las tareas que ejecuta
el linker. Dicho de forma chabacana, arrejunta los códigos objetos generados por el compilador para generar
el ejecutable. Eso no es magia oscura, es simple y te ayuda a entender los siguientes errores:
* "The linker says def is defined more than once" una definición está en más de un fichero objeto.
* "The linker says abc is an unresolved symbol" No ha encontrado el fichero objeto o librería para una decaración
  *extern*.

## The Longevity of Interim Solutions

Muchas veces las soluciones temporales son eternas. Luego eso degrada la mantenibilidad, causa molestias,
pero no siempre vas a conseguir que te dejen arreglarlo porque "si funciona no lo toques". Aprende que
batallas luchar y cuales debes dejar estar.

## Make Interfaces Easy to Use Correctly and Hard to Use Incorrectly

Algunas ideas para hacer las interfaces de usuario más ergonómicas. Cosas de UX. Para un conocimiento más profundo
recomiendo el libro [No me hagas pensar](https://www.amazon.es/hagas-pensar-Actualizaci%C3%B3n-T%C3%ADtulos-Especiales/dp/8441537275/ref=as_li_ss_tl?s=books&ie=UTF8&qid=1539538658&sr=1-2&keywords=ux&linkCode=sl1&tag=uifrommars-21&linkId=c804d0d5e6be7122058e322df42500b5&language=es_ES)

## Make the Invisible More Visible

El código fuente no huele, no se ve en el producto final,... es invisible. Si a dos semanas de entrega tienes
un problema en una cosa invisible que va a provocar seis meses de retraso y mucha más pasta de inversión
tienes un problema. Habla de como dar visibilidad al estado de los proyectos para evitar sorpresitas y disgustos.

## Message Passing Leads to Better Scalability in Parallel Systems

Los sistemas tienen cada vez más microprocesadores y la programación paralela es cada vez más importante.
Correcto. Compartir memoria (como cualquier otro recurso) está lleno de problemas. Cierto. Y plantea el
paso de mensajes como única alternativa al problema de compartir recursos y los bloqueos necesarios. Mmmmm.
Todo tiene pros, contras y marcos de aplicación. Las soluciones categóricas y mágicas es algo que ningún
programador debería escuchar. Documéntate sobre shared-memory, compartir mediante paso de mensajes y 
soluciones shared-nothing (nada compartido) entre otras y usa la mejor opción en cada caso.

## A Message to the Future

Una tierna historia de por qué es importante hacer el código mantenible y legible. Yo conocía la mucho
menos tierna frase de "programa siempre como un psicópata que sabe donde vives tuviera que mantener tu código".

## Missing Opportunities for Polymorphism

Ejemplo de como el polimorfismo puede ayudar a dejar el código limpio evitando construcciones *if/switch*.
El autor tiene todo mi reconocimiento por decir de forma explícita que no es una solución mágica para aplicar siempre,
sino solo cuando conviene.

## News of the Weird: Testers Are Your Friends

La gente de QA es tu aliada, no tu adversaria. No puedo estar más de acuerdo con eso. Prefiero que los errores
que cometo los detecte el tipo de QA en pre-producción a tener que arreglarlos a prisa y corriendo mientras el
cliente me chilla "me has hecho perder un negocio de un millón de dólares" porque el error ha llegado a producción.

Y recuerda que QA no es tu chacha que hace el proceso de pruebas. Es tu responsabilidad pasarles un código
*production ready* que funciona a la primera. El karma te recompensará cuando los gerentes escuchen lo
contentos que está QA con tu trabajo.

## One Binary

Algunos consejos más o menos acertados sobre gestión de la configuración. 
La [gestión de la configuración](https://es.wikipedia.org/wiki/Gesti%C3%B3n_de_la_configuraci%C3%B3n) es
muy importante. Estudia a fondo el tema.

## Only the Code Tells the Truth

Si quieres saber qué es lo que sucede en la aplicación mira el código fuente. La documentación puede ser
obsoleta, los requisitos pueden haber cambiado desde su definición inicial o haberse implementado de forma
errónea, los comentarios en el código no se ejecutan podrían contarte cualquier milonga. Haz código legible
porque el código es tu fuente de verdad. Si el código necesita comentarios porque es farragoso refactoriza
para deje de serlo. :clap: :clap: :clap: :clap:.

## Own (and Refactor) the Build

Los scripts de construcción de tu código son tu responsabilidad. No lo delegues porque no quieres aprender un
lenguaje de scripting o porque solo sabes apretar botones en un IDE. Es importante para mantener la gestión
de la configuración y la reproducibilidad de las construcciones, así se evita el típico "en mi máquina funciona".
Tú eres el más indicado para garantizar que se optimiza la ejecución de los tests y que cada miembro del equipo
tiene una forma sencilla de ejecutarlos localmente. Hazle caso a este buen hombre, que a la larga todo son
ventajas.

## Pair Program and Feel the Flow

Contra intuitivamente a lo que se pudiera esperar, compartir un mismo código entre dos personas hace más fácil
que el código fluya entre vuestros dedos. Cuando te atascas en una chorrada el otro te desatasca y vice versa,
todo fluye. Tiene otras ventajas extras: mejor transferencia de conocimiento (al menos dos personas saben
de qué va ese código), cuando te interrumpen el otro puede mantener el flow y te cuesta menos retomarlo,
aprendes del otro,...

## Prefer Domain-Specific Types to Primitive Types

Habla de como evita errores, mejora la legibilidad y en general es una mejor opción. No os podeis imaginar
cuanto odio ver una función que acepta un parámetro *timeout* de tipo *int* o *float* y no sé si se lo
tengo que dar en segundos o milisegundos. Si me pidiese un *timedelta* lo tendría claro al nanosegundo de verlo :wink:.

## Prevent Errors

Imagina que a un usuario le muestras un calendario para pedirle la fecha para reservar las vacaciones, si no le
habilitas la opción de marcar fechas en el pasado o en un futuro lejano nunca va a introducir errores en esas
fechas. Si implementas una opción undo puedes hacer una estadística de qué funcionalidad confunde más a tus usuarios
para arreglarla. Truquitos para hacerle la vida mejor a la gente.

## The Professional Programmer 

¿Qué es un programador?

Un tio con responsabilidad personal:
* Es responsable de su carrera y se mantiene a la última el solito.
* Se responsabilizan del código que escriben. Por ejemplo lo prueban y cuando están seguros de que está bien se lo
  pasan a QA.
* Son jugadores de equipo.
* No toleran grandes listas de bugs en el bug-tracker.
* No hacen chapuzas. La excusa del deadline es una gilipollez, deadline es un transplante de corazón y tiene que
hacerse para que dure toda la vida (del paciente al menos) y antes de que mueran demasiadas células.

:clap: :clap: :clap:

## Put Everything Under Version Control

Cuando tienes todo en control de versiones puedes ver no solo el código actual sino cada cambio en la historia.
Habla de otras bondades menores y algunos consejos de uso. Totalmente de acuerdo.

## Put the Mouse Down and Step Away from the Keyboard

Cuando estás ofuscado con un problema te bloqueas y entras en bucle de colapso. Tómate un respiro, descansa y
es muy probable que la solución aparezca sola.

## Read Code

Lee código de otros. Si el código es malo y difícil de leer aprende de sus errores para no cometerlos tú.
Si el código es bueno aprende de como lo ha hecho, busca patrones que no conozcas o formas de abordar
problemas que no conocías. Si quieres hacer mejor código no leas un libro, lee código. :clap: :clap: :clap:

## Read the Humanities

Es importante entender a los humanos, su forma de pensar y como comunicarse con ellos.

## Reinvent the Wheel Often

Reinventar la rueda como forma de experimentación para darte cuenta de las complejidades de un sistema. Me parece
muy razonable si eres lo bastante humilde como para tirar tu trabajo si no está a la altura del estado del arte. 
Por desgracia mucho programador reinventa la rueda, la hace cuadrada y obliga a otros a mantener su despropósito.

## Resist the Temptation of the Singleton Pattern

Que te pienses seriamente si realmente necesitas un singleton porque puede esconder muchos problemas:

* Realmente no lo necesitas
* Añade acoplamiento
* La persistencia complica las pruebas unitarias (el acoplamiento también)
* Dificulta la concurrencia (hay que sincronizar ese estado compartido)

Y luego quitárselo de en medio es complicado.

Totalmente de acuerdo. Hace 30 años creía que el monitor, el teclado y el ratón tenían cardinalidad uno y se podían
modelar con un singleton. Ahora tengo un portátil que trae uno de cada y al que le enchufo otro monitor, teclado y ratón.
Por suerte mi escritorio no los modela como singleton.

## The Road to Performance Is Littered with Dirty Code Bombs

El código acoplado es difícil de refactorizar, por ejemplo para hacerlo más eficiente. Las métricas de calidad
te pueden ayudar a eliminar esas bombas de código que suponen el código acoplado. Así que refactoriza primero en
mantenibilidad y luego en eficiencia. De otra forma acabarás empantanado y no tendrás ni una ni otra.

## Simplicity Comes from Reduction

El código debe ser simple y mantenible. Muchas veces la solución no es añadir más código, es cortar por lo sano y dejar
el código sin cancer, gangrena o esas líneas apestosas que requieren tanto esfuerzo para conseguir tan poco.

## The Single Responsibility Principle

El autor habla sobre la S de SOLID y yo hablaré de por qué no me gusta como principio de código. Primero hay que
definir qué significa "principio de responsabilidad única", que define muy bien como "mantén juntas las cosas
que cambian por el mismo motivo y separa aquellas que cambian por motivos diferentes". Para hablar de ello pone como
ejemplo una clase *Empleado* que aparte de modelizar un empleado se encarga de su persistencia y de hacer ciertos
reportes sobre el empleado. Y la separa en la clase *Empleado*, *EmpleadoReporte* y *EmpleadoRepositorio* donde
estás dos últimas están acopladas con *Empleado*. El autor reconoce el problema y cita alguna alternativa para
evitar que un cambio en *Empleado* te obligue a tocar en tres sitios. El autor hace un ejemplo correcto, pero la
justificación del principio de responsabilidad única es un poco débil. Ese es mi problema, prefiero las heurísticas
de "alta cohesión y bajo acoplamiento" del principio GRASP. En este caso se cambia de sitio un acoplamiento que ya
existía (reportes y persistencia estaban inicialmente acoplados con empleado), lo hace más explícito y mejora la
cohesión. Con GRASP no hay que hacer justificaciones, porque GRASP es un principio de desarrollo que funciona.

## Start from Yes

En el trabajo trata de ser empático en vez de enfrentarte con tus compañeros/subordinados.

## Step Back and Automate, Automate, Automate

Por si no quedó claro con el título: Automatiza!!!

## Take Advantage of Code Analysis Tools

Usa analizadores de código y que no te dé miedo hacerte tus propias herramientas si alguna cojea. Totalmente de
acuerdo. Algún refactor imposible se me ha tornado laborioso haciendo alguna mini herramienta de este tipo. Y en
el proceso descubierto y arreglado varios bugs que todavía no estaban reportados o nadie había conseguido encontrar
su origen.

## Test for Required Behavior, Not Incidental Behavior

Que hagas tests para probar los requisitos y no la implementación correcta actual porque los tests dejan de ser
confiables: un test correcto (que cumple con los requisitos) puede dar un falso positivo de error en los tests
porque la implementación a la que estaban acoplados ha cambiado.

Muy de acuerdo. No puedo verbalizar el odio y asco que me generan esos programadores que hacen un código, lo
suponen correcto y hacen el test con los datos que devuelve su implementación a una entrada dada. Cuando corriges
un bug tienes que arreglar su código y su test, cuando cometes un error y falla un test tienes que repasar si el test
es correcto o si tu cambio es lo correcto. Todo el mundo trabaja el doble, pero en las métricas de cobertura ellos lo
petan. Centrarse en métricas de cobertura ayuda a provocar este tipo de errores.

## Test Precisely and Concretely

El punto anterior no es excusa para ser ambiguo. Si quieres probar una función de ordenación puedes suponer las
siguientes reglas:

* Los elementos del resultado están ordenados (si la función devuelve siempre "1, 2, 3" el test pasa)
* El número de elementos pasados es igual a los elementos devueltos (la función de antes pasaría si la pruebas con "4, 5, 6")

Mejor ser concreto (y buscar corner cases):

* "1, 5, 1, 2" devuelve "1, 1, 2, 5". Es concreto, engloba los dos anteriores, el 1 repetido sirve para ver que no borras
  duplicados.
* "NaN, 1, 5, 1, 2" devuelve ¿?. NaN es un valor válido para un float, pero ¿cómo debería tu función ordenarlo? Enhorabuena,
  pensando en un *corner case* has encontrado un comportamiento indefinido. ¿Hay que definirlo o dejar que pase con
  diferentes implementaciones?

## Test While You Sleep (and over Weekends)

Hay muchos tipos de tests y tardaría mucho si los ejecutas todos en cada commit. Ejecuta los rápidos y deja los lentos
que se ejecuten en tareas programadas por la noche o los fines de semana para que no consuman workers de los tests que
quieres que se ejecuten rápidos en cada commit. Un ejemplo mio puede ser:
* En cada commit ejecutar todos los tests unitarios y alguno de integración a modo de prueba de humo. Debería ser rápido.
* Por la noche ejecutar la suite de integración completa, el analizador estático de código, los unitarios con cobertura,
  ...
* El fin de semana ejecutar tests de mutación, fuzzers, de rendimiento y una matriz entera de los tests unitarios y de
  integración con la versión actual de la bbdd y la siguiente a desplegar, con la versión actual del framework y la
  siguiente, en las diferentes arquitecturas que tu código debería correr,...

No penalices el ciclo de desarrollo con una batería automatizada de tests eterna. No fomentes que la gente agrupe
cambios para evitar esos tests lentos.

## Testing Is the Engineering Rigor of Software Development

En otras ingenierías el proceso de construcción es lento y caro. No es admisible fallar y volver a hacer. En software
construir es barato, pero los errores en producción pueden costar caro. Los tests dan el rigor al software que en
otras ingenierías dan las matemáticas, procesos, normativas,... Se estricto y riguroso con los tests. Y hazlos confiables.

## Thinking in States

Usa máquinas de estados finitas para modelizar y clarificar el comportamiento. Muy de acuerdo, con un dibujo de una
máquina de estado en una pizarra, papel o documento se puede discutir de una forma clara los diferentes flujos de un
proceso. Eso evita muchos errores. A mí me gusta integrar en la documentación del código diagramas que se generan con
texto (por ejemplo plantuml o mermaid) para que esa información sea más fácil de mantener. Así de importante la
considero ;-)

## Two Heads Are Often Better Than One

Colaborar es bueno. Creo que esto es duplicado y ya se ha hablado de los beneficios del pair programming antes. Lo
mismo del trabajo en equipo.

## Two Wrongs Can Make a Right (and Are Difficult to Fix)

El autor comenta casos en los que un error puede enmascarar otro error. Esto genera problemas (a la hora de arreglar
un error puedes tener regresiones al hacer visible el error que este error enmascaraba, vaya frasecita :sweat_smile:)
y plantea que no hay estrategia válida y que hay que valorar diferentes alternativas.

Algunas heurísticas que propongo yo:
* Que algo funcione no implica que sea correcto. Define contratos y válida que se cumplen. Haz tests unitarios con
  los contratos definidos y de integración para evitar que otros errores te causen regresiones (reduccionismo y 
  holismo al mismo tiempo).
* Ratifica tus creencias con negocio. Tú le dices qué es lo que hace el código y les preguntas si es lo que debería
  hacer. Un error puede estar en la implementación o ser una implementación correcta de un malentendido. Si hace
  falta preguntar para descartarlo se hace.
* Aplica el principio de falsabilidad. Haz todo tipo de hipótesis por estúpidas que puedan parecer, imagina un
  experimento para falsarla (normalmente un test), descártala o grita "Eureka" si has encontrado el error. No te deprimas
  demasiado si al arreglar ese error aparece otro, he llegado a encadenar 6 o 7 bugs bastante complejos
  en "una cosita pequeña que no estaba bien especificada que se resuelve en un par de horas".
* Retrasar la subida de un hotfix porque has encontrado más errores puede ser un problema para el usuario, subir algo
  que arregla una cosa y rompe otras también. Si tienes dudas de qué hacer pregunta, la colaboración es buena.
* Ser metódico con el código y los tests previenen errores. Tener una buena trazabilidad y manejo de errores hace más
  rápida su solución. Los errores difíciles de arreglar es mejor prevenirlos, si no los puedes prevenir es mejor que las
  herramientas para depurarlos funcionen bien. No enmascares los errores.
* Ten un patito de goma, un peluche de shin chan,... y pregúntale si te atascas. Verbalizando el problema suele
  desbloquearse la solución. Si te atascas *molesta* a un compañero.

## Ubuntu Coding for Your Friends

Hay un momento Zen, solitario, de máxima concentración en el que el código fluye. Pero no hay que olvidar que ese
código que haces no lo haces tú solo, ni lo haces para tu disfrute personal,
lo haces dentro de un equipo y repercutirá en ellos. Haz código legible,
mantenible, que sea un regalo para tus compañeros y no un acto onanista. Con ubuntu quiere decir: "una persona es
persona a través de otras personas".

## The Unix Tools Are Your Friends

Aprende comanditos de Unix que te van a servir para muchas cosas. Estoy de acuerdo. Recuerdo un proyecto que se estaba
presupuestando en decenas de miles de euros y me dijo mi jefe: "esto yo lo haría con 4 comandos de unix, pero estoy
oxidado y tardaría 2 días. Mira a ver qué te lleva hacerlo". Un rato después 6 líneas de *awk* enterraron ese
despropósito de proyecto.

## Use the Right Algorithm and Data Structure

Usar el algoritmo o la estructura de datos correcta puede acelerar mucho la ejecución de un programa, reducir su
consumo de recursos,... Y es cierto. Pídele a cualquier junior que calcule el centésimo número de la serie de Fibonacci:

```python
def fibo(n):
  if n < 1:
    return 0
  elif n == 1:
    return 1
  retrun fibo(n-1) + fibo(n-2)
```

Alguien menos junior se dará cuenta de que para calcular `fibo(100)` estás calculando `fibo(99)` y `fibo(98)`, pero
para calcular `fibo(99)` estás volviendo a calcular `fibo(98)`. Eso es <i>O(2<sup>n</sup>)</i>, una barbaridad y te ofrecerá 
cualquiera de estas alternativas:

* Hacer el código iterativo
* Hacer que el código haga recursividad de cola y convertirlo a cython porque C, a diferencia de python, optimiza
  este tipo de recursividad.
* Decorar la función `fibo` con una función que recuerde los resultados para reducir el número de llamadas.

Todos ellas son *O(n)* lo cual es una mejora importante. De no terminar nunca a ser casi inmediato. Habrá un debate
de si hacerlo en C (cython) es lo más rápido, si el iterativo es lo más simple y mantenible,... Pero solo alguien
más experto en matemáticas y análisis numérico reparará en el siguiente código *O(1)*:

```python
from math import *

def fibo(n):
  """
  See https://en.wikipedia.org/wiki/Fibonacci_number for a complete analysis
  """
  sqrt_5 = sqrt(5);
  p = (1 + sqrt_5) / 2;
  q = 1/p;
  return int( (p**n + q**n) / sqrt_5 + 0.5 )
```

Todo esto da de sí un problema trivial. Uno complejo puede implicar estructuras de datos caché friendly, algoritmos
de programación dinámica,... Evita la optimización prematura porque haré que tu código no sea mantenible y dificultarás
futuras (y necesarias) optimizaciones. Otras veces las cardinalidades reales harán que un algoritmo aparentemente más
simple y peor sea más eficiente. Por ejemplo, hay motores de bases de datos que prefieren recorrer una tabla con pocos
elementos a filtrar por un índice y luego ir a leer los registros de la tabla ya filtrados, esto es porque una lectura
secuencial puede ser más rápida que varios accesos aleatorios y el aumento de complejidad de la implementación.
¡¡¡Aprende todos los algoritmos y estructuras de datos que puedas, pero no los pongas en práctica hasta haber usado antes
un profiler!!!

## Verbose Logging Will Disturb Your Sleep

El autor comenta que una aplicación que en producción lanza muchos logs es siempre el primer síntoma de problemas.
Y cualquiera que conozca la fábula de "Pedro y el lobo" debería estar de acuerdo. Los niveles de logs están para que
en producción no lleguen los de nivel *INFO* o *DEBUG*. Y si por algún motivo, de esos que solo pasan cada 100 años,
hay que habilitar los logs en producción en modo debug debería poder hacerse solo de un módulo o paquete de la aplicación
y no de la aplicación entera. He visto tirar servidores de producción porque algún developer pensó que podía llegar a
ser conveniente llenar los discos con gigas y gigas de información absurda de debug. Si eres administrador de sistemas
te recomiendo repasar cómo funciona logrotate para que veas que defenderse de un mal developer no es trivial.

## WET Dilutes Performance Bottlenecks

WET es Write Every Time y lo usa como antítesis de DRY (Don't Repeat Yourself). Expone que si tienes una función que
es utilizada en 10 sitios y consume el 30% de la cpu, enseguida canta haciendo profiling, pero que si esa función la has
repetido 10 veces, de media solo consumirá el 3% de cpu y será más difícil de encontrar el problema de rendimiento.
Y si llegaras a encontrarlo aplicar la optimización te llevaría 10 veces más porque tendrías que arreglarla en 10
sitios. Irrebatible.

## When Programmers and Testers Collaborate

No tengo palabras. Pone un ejemplo de [herramienta mágica](https://en.wikipedia.org/wiki/Framework_for_integrated_test)
que permite que negocio, unos testers (¿será QA?) y los desarrolladores colaboren y hagan cosas que me parecen utopías.
Como el tener una batería de tests hecha por negocio y los testers para que cuando los desarrolladores implementen el
código puedan probarlo ¿sin escribir un solo test?. Tengo que investigar el tema y si es verdad postular por alguna
empresa que lo implemente (no que diga que lo hace, que lo haga). Una vez intenté una magia parecida con BDD, pero
negocio se dedicaba a cambiar palabras por sus sinónimos, poner signos de puntuación, convertir varias líneas de
Gherkin reutilizable en una sola frase compuesta, quitar las comillas dobles que indicaban el valor concreto de una
variable,... y otras formas ingeniosas de romper los tests.

## Write Code As If You Had to Support It for the Rest of Your Life

Escribe código como si tuvieras que mantenerlo el resto de tu vida. Me parece muy bonico y muy jipi todo eso que dice
el autor aquí sobre la actitud, pero voy a cambiarle el
sentido totalmente a la frase: "Huye rápido de los sitios que no te dejan escribir el código que tienes que mantener".
Eso si, si te dejan escribir el código hazlo como si tuvieras que mantenerlo por el resto de tu vida.

## Write Small Functions Using Examples

No entiendo por qué está titulado así. Habla sobre explosión combinatoria de todas las pruebas a realizar
a una función que recibe como parámetro un entero y devuelve un booleano. Eso hace que los tests solo sean viables
para garantizar el correcto funcionamiento de ese conjunto y no que el código está libre de bugs. Luego cambia el tipo
nativo entero por un tipo de dato del dominio que solo puede tomar los valores `1..4` haciendo que probar todas las
opciones sea viable :shrug:.

## Write Tests for People

El objetivo de los tests no es solo detectar errores para solventarlos antes de desplegar a producción. Ni solamente
evitar tener que probar manualmente lo que puedes afectar con un cambio, evitar regresiones. Los tests deben enfocarse
también a las personas que pueden leerlos para entender el comportamiento en diferentes escenarios. Por eso recomienda
la siguiente estructura (similar a Gherkin):
* Establece el estado inicial del escenario, su contexto (Given)
* Ilustra cómo la funcionalidad es invocada (When)
* Describe las post condiciones o resultados esperados a comprobar (Then)

Y haz que eso sea legible. BDD es llevar este consejo al extremo, deberías echarle un ojo si todavía no lo conoces.

También habla de una forma manual de mutation tests para hacer tests de los test. La idea es inyectar errores en el
código y esperar que algún test falle. Recomiendo echar un ojo a alguna herramienta de mutation tests y fiarse más de
sus resultados que de las métricas de cobertura de tests.

## You Gotta Care About the Code

Grandes capacidades técnicas y hacer código del averno no son incompatibles. Da una serie de consejos para cuidar el
código:
* Hacer código elegante y claramente correcto. Esta corrección está apoyada con tests.
* Haces código que otros puedan entender, mantener y es correcto (no solo que parezca funcionar, que puedas garantizar
  que lo hace).
* Haces código para el equipo, no para satisfacer tu ego mostrando lo listo que eres.
* Cuando tocas un código tratas de dejarlo mejor que como te lo encontraste (más tests, mejor estructura, más comprensible,...)
* Aprendes nuevos lenguajes, técnicas, idioms, pero solo lás aplicas cuando tienen sentido.

Y ahora mis consejos:
* Solo escribes para la máquina en ensamblador. En el resto de lenguajes escribes historias amenas a otros programadores
  en un lenguaje que un compilador o intérprete puede hacer entender a la máquina. La presentación de los personajes
  (variables descriptivas), un lenguaje preciso (en los buenos libros una risa puede ser sardónica, histriónica,... en
  los malos códigos todo es un handler o un manager o data), un ritmo adecuado (frases cortas, parrafos no muy largos,
  funciones concisas,... excepto cuando hay que ponerse intensito), cohesión (cada capítulo centra una subtrama,
  cada clase modela una entidad, cada paquete es un libro dentro de una saga), bajo acoplamiento (aunque los libros
  formen parte de una saga deben ser lo más autocontenidos posible, los módulos no pueden estar haciendo constantes
  referencias a otros módulos,...), consistentes (aunque el libro lo firme un autor y lo escriban 10 *negros* debe
  aparentar un único estilo, los programadores tratan de adaptar/consensuar un estilo de código en el proyecto),...
* Hacer código que cumpla con la especificación es medio fácil. Trata de hacer amigos con el handicap de implementar
  tantas [-ilities](https://en.wikipedia.org/wiki/List_of_system_quality_attributes) como te sea posible: Con la
  legibilidad, la mantenibilidad y la testeabilidad te ganarás el respeto de tus compañeros. Con la recuperabilidad,
  escalabilidad, trazabilidad y manejabilidad conseguiras que la gente de sistemas no haga muñecos voodoo con tu cara
  durante sus guardias. Con la corrección la gente de QA que no se sentirá la chacha a la que unos developers
  marqueses le pasan código sin probar. Con la eficiencia el corazón de los usuarios más humildes que no pueden
  pagarse una supercomputadora y con la usabilidad el de todos los usuarios. Con...
* Haz TDD, BDD, DDD y/o lo que te apetezca, pero no hagas Resume Driven Development. No uses tecnologías solo para
  engordar curriculum haciendo la existencia de tus compañeros un infierno, peligrar la inversión de tu cliente, minando
  la tranquilidad de los usuarios,... Lo honesto es aprender las tecnologías con un pet project y utilizarlas cuando 
  tenga sentido hacerlo. Porque cuando hay deadlines de por medio no vas a tener tiempo de aprender la tecnología y
  con los retrasos por usar una tecnología que ni dominas ni apenas conoces vas a tener unos deadlines muy duros.
* No seas clasista. Muestra el mismo respeto a la señora de la limpieza que al CTO. Valora a la gente por su actitud y
  no por su aptitud, con buena actitud la aptitud acabará llegando. Es normal que los juniors anden escasos de aptitudes,
  mentorizalos. No proyectes tus gustos sobre otra gente, puede que a ti los retos te hagan sentir vivo, pero a un
  compañero le cause estrés y angustia o al contrario. Sé empático y trabaja en equipo.
* Es normal que allá donde caigas encuentres un código con un montón de problemas. Trata de conocer cuáles han sido las
  circunstancias que han llevado a esa situación (falta de formación, requisitos cambiantes, inicios precarios que
  obligaban a soluciones efectistas para conseguir financiación, problemas de comunicación con cliente,...) para
  encontrar una estrategia que no vaya a tropezarse en los mismos errores. He visto
  varias reimplementaciones agresivas que acababan en fracaso estrepitoso. También he vivido pequeños cambios, uno tras
  otro, poco a poco, que convertían en virtuosos círculos viciosos. Implantar una cultura de tests (de cero a miles)
  cuesta al principio porque se ve como una sobrecarga de trabajo, cuando los tests empiezan a evitar regresiones los 
  desarrolladores reacios empiezan a aceptarlos y hacerlos con gusto, con mejores tests se dedica menos esfuerzo a 
  corregir bugs y más a desarrollar nuevas funcionalidades, las nuevas funcionalidades atraen clientes e inversores, 
  esa inversión provoca una ampliación del equipo y la opción de ir pagando algo de deuda técnica. No es el cuento de la lechera,
  lo he vivido personalmente, aunque en algún momento la cosa se torció y acabó igual que el cuento,
  creo que fué por ese refactor agresivo que dejó las cosas peores de lo que estaban y enrareció la dinámica de equipo.
  También he visto refactors agresivos salir bien.
* No te encariñes con tu código. Piensa que es como el pescado o las visitas, después de unos días empieza oler mal.
  Puede que a ese diseño tan perfecto con las especificaciones de ayer se le vean las costuras con las especificaciones
  de hoy. Puede que ese MVP hecho a prisa y corriendo, cogido con pinzas, para que un cliente firme es lo que quieren 
  todos los clientes y que esa funcionalidad estratégica, trabajada durante meses, solo la use uno. Puede que todo ese 
  diseño de hace años, hecho para aprovechar las lecturas secuenciales de los discos magnéticos empezase a oler con la 
  llegada de los discos SSD.
  Prioriza el código mantenible, cohesionado, poco acoplado, testeable y mantén actualizada la documentación de cuáles
  son los requisitos actuales, porque lo más probable es que antes o después haya que tirar el pescado podrido.
* Evita el [culto al cargamento](https://es.wikipedia.org/wiki/Culto_cargo). Se tiende a imitar lo que hacen las
  [FAANG](https://es.wikipedia.org/wiki/Gigantes_tecnol%C3%B3gicos#FAANG) sin entender nada. La organización de un
  gigante con decenas de miles de empleados casi seguro que no es el mejor ejemplo para una start-up de media docena.
  200 microservicios para 1.000 empleados toca a 5 empleados por microservicio, 20 microservicios para 5 ingenieros
  de una start-up tocan a 4 microservicios por ingeniero (8 si quieres tener un
  [bus factor](https://es.wikipedia.org/wiki/Bus_factor) mayor que uno). La gente que desplegaba clusters hadoop para
  hacer map-reduce como google con datos que cabían en un excel. La gente que instala un cluster k8s para una aplicación
  LAMP básica... No seas como ellos.

## Your Customers Do Not Mean What They Say

Muchas veces el cliente no sabe lo que quiere. Aquí se comentan algunas técnicas/dinámicas para la toma de requisitos,
pero creo que es un tema lo bastante complejo para ser tratado en dos páginas de un libro. Busca documentación de verdad.