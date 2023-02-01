+++
title = "hugo como gestor de recetas"
description = "Detalles sobre cómo ha sido la experiencia de usar hugo como gestor de recetas"

[taxonomies]
tags=["tecnología", "recetas"]
projects=["recetas_hugo"]
+++

# Por qué

La idea viene descrita en más detalle en [aquí](@/ideas/recetas.md). Básicamente, se llega a esta idea por descarte de
otras opciones:

* Mi novia era feliz usando [KRecipes](https://apps.kde.org/es/krecipes/) hasta que en una actualización de ubuntu dejó
  de funcionarle.
* A pesar de que conseguí hacerlo andar con una imagen de ubuntu viejuna dentro de docker le entró la angustia de que
  en algún momento pudiera perder el trabajo de recopilación de recetas. Es mucho trabajo y algo que usa bastante.
* Ningún programa que evaluó terminó de convencerla.
* Que yo le hiciese un programa o me pusiera a mantener krecipes (hacer que dependa de QT5 o QT6 en vez de QT3) no
  resolvía el problema de que ella no era dueña de sus datos.
* Se había puesto a editar html a mano con los conocimientos que adquirió en un curso del paro en el siglo pasado.
  Labor que aparte de titánica me escandalizó: demasiado trabajo y no hay separación de contenido y presentación.

Usar un estatificador en el que el contenido (las recetas) y la presentación (plantillas, css,...) estuvieran separadas
me pareció obvio. Aparte todas las páginas de listados (total, por categorías, por ingredientes) se hacían de forma
automática quitando un montón de trabajo repetitivo y propenso a errores. También era capaz de generar datos
que un js podía utilizar para hacer un buscador.

Al haber [un tema en hugo para recetas](https://github.com/deranjer/hugo-cookbook) me pareció que no implicaba demasiado
trabajo.

# Por qué no

Para resolver el problema tuvimos que hablar bastante del tema y llegamos a una alternativa que podría ser mejor para
ella: Un documento donde cada receta es un sub apartado dentro de un capítulo por categoría, con un índice de referencia
por ingredientes es más sencillo para ella. No calló en su momento en la opción de hacer referencias cruzadas y pensó
en html como única tecnología para hacer hipervínculos, pero a veces hay una opción más simple. 
Escribir sus datos como un documento word o libreoffice le permite cumplir con los siguientes requisitos:

- [x] Es dueña de sus datos
- [x] Tiene una forma rápida de buscar por categorías (el índice de capítulos)
- [x] Tiene una forma rápida de buscar por ingredientes (siempre que haga correctamente el sumario)
- [x] Es fácil de exportar para consultar las recetas en la tablet o ebook reader sin conexión a internet
- [x] Puede organizar los datos como le plazca y no como un desarrollador haya programado (no usa muchas funcionalidades 
  de krecipes)
- [ ] Un editor que le guíe con la edición de cada receta es algo que no sé si soportan las herramientas ofimáticas,
  por lo que esta funcionalidad de krecipes la dejó aquí sin marcar.

Otro de los problemas es que viene con la necesidad de instalar un editor con soporte de markdown, tiene que ejecutar
comandos para construir la publicación,... Demasiadas complejidades para una persona que piensa que una terminal es
ver matrix.

El HTML generado funciona fetén en un web server, pero abierto como `file:///home/luser/recetas/public/index.html` ya
no tanto. Eso hace que hagan falta pasos extras para exportar a dispositivos externos.

# La experiencia de desarrollo

## El esqueleto inicial

Instalar hugo, descargar la plantilla para manejar recetas y configurar el IDE para trabajar con el contenido es algo
que se puede hacer muy rápido. La plantilla no estaba pensada para organizar el contenido como a ella le gustaba así
que es algo que iba a tener que tocar. Pero para eso es mejor hacerlo con contenido.

## Importando datos reales

Krecipes guarda los datos en una base de datos, ella usaba sqlite que es una base de datos embebida de SQL. Realmente
ella exportaba un fichero para hacerse los backups y no tenía la menor idea de esto, pero enseguida vi que lo era y que
iba a ser fácil hacer un exportador de datos de krecipes al contenido en hugo.

El modelo de datos no es demasiado complejo y al haber varias funcionalidades que no utilizaba el trabajo era todavía
más simple. Sabiendo un poco de sql y algún lenguaje de programación es fácil hacer la exportación sin requerir de un
complejo análisis de ingeniería inversa de los datos.

Enseguida tenía montado un script para sacar las más de 500 recetas que tenía en krecipes y generar los markdowns
de contenido y rellenados su contenido de [Front Matter](https://gohugo.io/content-management/front-matter/).

Estaba listo para hacer las correspondientes adaptaciones de plantillas.

## Trabajar con las plantillas

Partir de algo que ya funciona y es casi lo que quieres ahorra un montón de trabajo. Pero como en todos los trabajos
se cumple la ley de pareto: en el mismo día estaba hecho el 90% del trabajo, durante todo el siguiente día puede avanzar
otro 9%, el 1% restante será cosa de meses :sweat_smile:

El sistema de plantillas de go no me gusta mucho, pero por suerte o por desgracia he tenido que usarlo para hacer
helms de kubernetes, por lo que no tuve que aprender tecnologías nuevas para esto. Me costó un poquito hacerme con la
documentación de referencia de hugo para ver los tipos de datos que tenía en cada página (listados, detalle de cada
taxonomía,...) y las funciones añadidas al sistema de plantillas por hugo. Algunos de los cambios más duros a realizar
fueron en los listados de categorías, que con la nueva organización tardaban mucho en generar el contenido. Tras los
cambios el proceso se agilizó x20 y paso de tardar segundos a décimas de segundo. La verdad es que trabajar en
desarrollo con un volumen no trivial de datos ayuda a encontrar y solucionar pronto este tipo de problemas de
rendimiento.

Para hacer los estilos la plantilla que mutilé usaba bulma.css, que resultó conveniente para agilizar los cambios
visuales solicitados. Apenas hubo que hacer cambios directamente en css.

A pesar de lo oxidado que tenía el tema de front la cosa quedó aparente (no profesional, pero si resultona) con muy
poco esfuerzo.

## Despliegue

El hosting que contraté antes ofrecía acceso ssh y ftp; pero ahora, al menos la versión barata, solo ofrece acceso
ftp. No contrates nunca para nada serio este tipo de hosting. Sin ssh olvídate de hacer despliegues rápidos con rsync o
actualizar un git y cómete despliegues completos que con miles de ficheros tardan lo suyo.

Para agilizar esto me va a tocar pagar un hosting que no sea tan chustero o hacer un pipeline con soporte de caché para
hacer lo siguiente:

* Leer de caché la ref de git del último despliegue. Si no hay entrada en caché subir todo y guardar ref en caché.
* Actualizar los ficheros modificados en `git diff <version> --stat --name-only` solamente.
* Guardar ref en caché para siguientes despliegues.

Pero es una solución que me parece muy guarra porque no quería meter los ficheros generados en el git, pero sin meterlos
se complica la detección de cambios. Otra opción es guardar en caché lo último que se subió y hacer una comparación con
la generación de páginas de la última ejecución. Más complejo, más limpio, pero no deja de ser ñapeo porque el hosting
es una chusta.

# Resultado final

Al enseñarle la forma de uso del [proyecto](@/proyectos/recetas_hugo.md) a mi novia,
la usuaria final, me dijo algo así como "eso de ejecutar
comandos desde una terminal no lo voy a hacer ni de coña". Fallaron los tests de aceptación, cosas que pasan. El
resultado final le gustaba, la apariencia del contenido de las recetas era mucho más limpio que el html que ella
estaba escribiendo a mano, pero ejecutar comandos en una terminal no resultó ser lo suficiente user friendly.

También falló el test de aceptación que abriendo la url como `file:///home/luser/recetas/public/index.html` fallasen
tantos enlaces.

Mientras hablaba con ella de la lista de tareas que me apuntaba para terminar con ello:

- [ ] Hacer una entrada en systemd para ejecutar `hugo server`
- [ ] Montar un web server redirigiendo el dominio `recetas.lasi.ovh` al hugo
- [ ] Tocar el `/etc/hosts` para que resolviese localmente el dominio
- [ ] Bla, bla, bla cosas frikis de como resolver el tema de los enlaces abriendo con `file:///...`

... surgió otra vez el tema de por qué no funcionaba krecipes en su ordenador con el apaño que hice para el otro.

Le comenté que no sabía, le mostré que conseguía arrancarlo desde la terminal, pero que no sabía por qué fallaba el
lanzador de escritorio, que estaba atascado con el problema y que... Un *click* sonó en mi cabeza, encontré el problema y
ahora vuelve a ser una feliz usuaria de krecipes. Eso si, sin la preocupación de que en algún momento vaya a perder su
trabajo porque ha visto una alternativa casi funcionar. MISIÓN CUMPLIDA.