+++
title = "Gestionar Recetas"
description = "Ideas para solucionar el problema de gestionar recetas"
+++

# El problema

Mi novia gestionaba las recetas con krecipes, pero dejó de estar soportado hace muchos años y ya es muy
complicado ejecutarlo. La última vez conseguí hacerlo funcionar a través de una imagen docker de un
ubuntu viejuno, pero en el último ordenador esa fórmula no está funcionando correctamente. De todas formas,
antes o después, vamos a tener que dejar morir a krecipes y hacen falta alternativas.

# Brainstorming de soluciones

* Buscar otro gestor de recetas que funcione y que le guste :frowning_face:
* Hacer yo una alternativa a krecipes :hushed:
* Editar los html exportados de krecipes a mano :open_mouth:
* Usar un estatificador para gestionar las recetas :raised_eyebrow:
* Una base de datos y formularios en openoffice con la que se lo pueda gestionar ella

## Software existente

De todo lo que ha probado no le ha gustado ninguno. Muchos están orientados a un servicio en línea donde
compartir recetas y buscar las de otra gente, pero no es lo que ella quiere. Lo que ella quiere es un simple
gestor de recetas como era krecipes. Tampoco le apetece pasarse a algo que pueda dejarla tirada el día de mañana.

## Software hecho por mi

Hace mucho que no hago una aplicación GUI por lo que me costaría hacerla. Al menos hasta aprender algún framework
para hacer GUI actual. No quiero que me pase como con krecipes, que corre con qt3 y hay que portarlo para que
funcione en nuevos equipos. Dentro de todas las opciones para hacer GUIs no quiero optar por una que me convierta
en esclavo de la aplicación. Tampoco es una cosa rápida para hacer en un día y necesitaría hacer bastante toma de
requisitos. Otro tema complicado porque hace mucho que no hablo con putos usuarios y además voy a tener que dormir
que el usuario por la noche.

No me costaría hacer una interfaz web que desplegar en casa. Pero quiere poder usarla si se va de viaje, desde la tablet,
poder responsabilizarse ella de los backups para que no perder su trabajo,...

A ella tampoco le hace mucha ilusión depender de mí y de que tenga el tiempo libre para hacerlo. Tampoco quiere
forzarme a programar en mis ratos libres, porque desde que teletrabajo sabe que mayormente me paso la jornada
echando espumarajos por la boca.

Algunas de las opciones para hacer GUIs son:

* Python (lo conozco): wxwidgets, gtk, qt (qml o ui tradicional), tk,... difícil lo de la tablet.
* Java (lo conocía): swing ¿seguirá existiendo? Es con lo que más trabajé de GUIs. Miré en su día el desarrollo nativo
  en android y apesta bastante.
* Flutter: No lo conozco, pero funcionaría en linux y en su tablet. ¿Es estable? ¿Acabará en el cementerio Google?
* Javascript: Usar js para hacer una interfaz web y meterlo en una app electron. No me apetece, la verdad.
* C++: La mejor opción para trabajar con qt y se pueden hacer aplicaciones móviles. Pero ya estoy muy viejo para C++.
* Golang/Rust/cualquier otro lenguaje decente que me apetezca aprender: No hay opciones maduras.

Estuve haciendo algunas pruebas con QT. Trabajar con QML es muy cómodo y se puede hacer la lógica en javascript, por
lo que no tendría que tocar demasiado C++, usarlo con python lo descarté por carencias que le encontré.
Es una opción interesante, pero también es la opción en la que estaba krecipes hasta que dejó de funcionar.

GTK tiene los Blueprints, que son como el QML de Qt, pero ni genera apps en android ni se si va a ser mucho más estable
que Qt.

Flutter lo vi un poco por encima y el tooling es brutal. Dart no parece complicado de aprender. La parte visual es la
que tiene más miga: aprender los widgets, los nuevos patrones de diseño (MVVM o seguir con MVC), los truquis para
que funcione en linux y android (layout orientable, diferencias al acceder al sistema de ficheros,...). Creo que es la
opción.

React native para móvil y react con electron para escritorio lo usa mucha gente. Pero no quiero tocar nada de esa
tecnología ni con un palo largo. Solo con ver la ejecución de los pipelines de la gente del curro que usaba esas
tecnologías me quitan las ganas de volver a tocar nada de lo que se ha convertido la comunidad de javascript.

## Editar HTML a mano

Aunque parezca muy loco es lo que a día de hoy le parece más factible. Solo depende de un editor, cualquier editor, para
no perder años de trabajo recopilando recetas. No depende de que la comunidad o yo mismo le mantengamos un software que
pudiera dejar de funcionar en las nuevas versiones de Linux o Android. Puede copiarse los ficheros html y leerlos en la
tablet. Lo mismo que puede leer un pdf. Por lo que ya no tendría la necesidad de llevarse el ordenador a la cocina o
hincharse a hacer viajes para consultar las recetas mientras las lleva a la práctica.

Este método solo tiene una pega: es un dolor.

## HTML generado en un estatificador

Volvemos un poco al paso de requerir un software, el estatificador para manejar las recetas. Pero confío que el texto
plano va a ser lo suficientemente claro como para que perder el programa que renderiza el html sea un problema menor.

A la hora de editar se centraría en rellenar contenido: texto de la receta en markdown y metadatos de título,
ingredientes, categoría y demás en yaml o toml. Luego una plantilla se encargaría de generar el html a partir de ese
contenido.

[hugo-cookbook](https://github.com/deranjer/hugo-cookbook) es un tema para mantener una web de recetas, por lo que la
idea no es algo descabellado que no haya más gente haciendo.

Algunas funcionalidades que perdería respecto a krecipes serían:
* autocompletado de ingredientes mientras edita una nueva receta
* búsquedas dinámicas por ingredientes. Algo de magia se puede hacer con javascript.
* Por limitaciones en las taxonomías la [edición de ingredientes se duplica](https://discourse.gohugo.io/t/per-post-taxonomy-metadata/21580)

## Base de datos openoffice

Lo utiliza para gestionar sus libros. Los que ha leído o le gustaría leer. Los que tiene en propiedad (colección
permanente o no) o no dispone de un ejemplar. Con sus búsquedas por título o autor o...

Por desgracia no ha conseguido fluidez modelizando los datos (SQL) y se ha encontrado con algunos problemas para
diseñar formularios que hagan lo que ella tenía en mente.

Que no funcione en la tablet no es mayor problema porque siempre tiene la opción de hacer un informe con el detalle
de una receta que exportar para ver en dicha tablet.

Sería curioso ver nuevas formas de estos programas, más accesibles, con la que los usuarios pudieran modelizar la
gestión de sus datos de una forma sencilla pero potente. Se supone que vivimos en la era de los datos y las aplicaciones
de este tipo son iguales que las del siglo pasado o han ganado en usabilidad a base de sacrificar funcionalidad.

# Requisitos del gestor de recetas

Las recetas contienen una lista de ingredientes (según el número de raciones a preparar) y las instrucciones detalladas
de su preparación. Opcionalmente, pueden contener imágenes para mostrar el resultado final o pasos de la preparación.
Las recetas pueden estar agrupadas por características más o menos ortogonales [(entrante, primero, segundo, postre), 
(vegetariano, apto celiacos, según alérgenos)].

Los ingredientes también podrían estar categorizados (frutas, verduras, salsas). Tener su propia receta asociada:
la mahonesa puede usarse como ingrediente o hacerse a partir de aceite y huevo. Estar disponible en una determinada
temporada (naranjas en invierno, tomates en verano) o estar disponible todo el año. Aunque cada ingrediente podría
tener asociado su valor nutricional no es requisito aquí.

Todo debería ser hipertexto: desde una receta se podría pinchar en un ingrediente, desde el ingrediente ver todas las
recetas en las que participa, en recetas o ingredientes pinchar en las categorías que los organizan, listados de
recetas e ingredientes por categorías,...

Las recetas deben ser fáciles de buscar (por ingrediente, categoría,...) y editar (autocompletado de ingredientes,
categorías,...).