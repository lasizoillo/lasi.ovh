+++
title = "Inventario de cachivaches"
description = "Necesito gestionar el diogenes de cacharros y cachivaches que tengo montado"
+++

# Diogenes

Es lo que piensa cualquiera que entre a mi despacho. Cuando coleccionas hobbies acabas acumulando un montón de
cacharrería asociada:

* Con la CNC y cortadora/grabadora laser e impresora 3D
  * Diferentes brocas y fresas
  * Madera, corcho y otros materiales sobre los que trabajar
  * Gafas de protección, cepillos de limpieza de chips, lijadora, lijas, limas,...
  * Calibre, peines, plantillas de curvatura,... para tomar medidas de piezas a modelar
  * Tornillería variada
* Con la electrónica
  * Instrumental de medición: osciloscopio, polímetro, pirate bus, analizador lógico,...
  * Fuentes de alimentación, generador de señales,...
  * Estación de soldadura, recambios de puntas, estaño, tercera mano, pasta de soldar, desoldador,..,
  * Componentes electrónicos variados: resistencias, condensadores, transistores, microcontroladores,...
* Con el lockpicking
  * Diferentes juegos de ganzuas: normales, multipick, impresioning,...
  * Diferentes tipos de cerraduras, bombines, candados, bombines de entrenamiento repineables, pines de diferentes tipos,
  * Herramientas para desmontar bombines: follower, alfombrilla para pernos, pinzas,...
* Con la informática
  * El homelab: con el pfsense, el nas (ordenador y caja jbod), el cluster de kubernetes, los switches,...
  * La estantería llena de libros de varios temas
  * Las piezas de repuesto que has canibalizado de ordenadores antes de deshacerte de ellos al quedar obsoletos.
  * El cable de red, los conectores rj45, la crimpadora,...

Y al final, cuando te pones a hacer algo práctico, pasas más tiempo buscando donde tienes esa pieza que necesitas que
disfrutando de tu hobby. ¿Dónde andará el usb-ttl que necesito para cambiar el firmware de esta placa SC01 Plus (esp32
con pantalla ips)? Pues no lo sé, supongo que con el blue pill que tampoco encuentro. Voy a ver si encuentro
documentación para ver si puedo hacer un apaño con el bus pirate. Ale, otro rato perdido buscando info. Si, se puede,
voy a ver si me pongo con ello. Ups, tengo que hacer no sé que movida y ahora no puedo. Arrrggg!!!! El diogenes no es
por la cacharrería, es por la cantidad de tiempo basura que pierdo NO haciendo cosas.

# Requisitos

Quiero un programa para hacer inventario de mis cachivaches y chismes. No necesito funcionalidades de control de stock,
gestión automatizada de pedidos para reposición, interconexión con aplicaciones de contabilidad y otras mil funcionalidades
que implementan los softwares de gestión de inventarios al uso. Funcionalidades que estorban a la hora de inventariar lo
que tengo y que me van a hacer perder más tiempo que el que me van a terminar ahorrando.

Tampoco quiero el típico software para gestionar colecciones ultra-simplificado donde puedo meter inventariar todos los
chismes que tengo, pero no hay forma simple de relacionarlos con dónde están almacenados (estantería -> balda -> caja).

Tras evaluar alternativas creo que voy a tener que hacerme algo a medida que conste de lo siguiente:
* Gestión jerárquica de contenedores (almacenes, estanterías, caja, caja más pequeña, lo que sea), todos son contenedores
  unos dentro de otros.
* Gestión de elementos (componentes electrónicos, aparatos de medida, cables,...)
* Asociación de un elemento en qué contenedor se encuentra (con un path de todos sus padres) y poder consultar todos
  los elementos que tengo en una caja sin tener que abrirla para mirar.
* Taxonomías jerárquicas para agrupar lógicamente elementos. Si defino un elemento como condensador automáticamente
  queda definido también como componente electrónico pasivo y como componente electrónico. Así si veo una oferta de un
  paquete de componentes electrónicos puedo ver cuáles tengo ya y cuáles completarían opciones a la hora de hacer
  proyectos.
* Quiero poder dar un nombre y alias a los elementos, una descripción, imágenes. Y poder colgar contenido asociado, de un
  acelerómetro de 3 ejes quiero tener asociado:
  * el datasheet con la documentación y especificaciones
  * librerías de uso
  * links donde aparezca como componente de kikad, freecad,... lo que sea
* La información tiene que estar indexada en FTS. Y bien indexada para que no se convierta en un diogenes digital.
* Facilidades para hacer la carga inicial de elementos:
  * Desde el detalle de un contenedor
    * se debe poder meter elementos que quedarán contenidos en él
    * los campos del elemento se auto-completarán priorizando las opciones de los elementos contenidos en él. Una caja
      con tornillos es probable que contenga más tornillos, una baterías más baterías
  * Desde el detalle una taxonomía
    * Se deben poder añadir elementos que quedarán relacionados con dicha taxonomía
    * Se priorizará la recomendación de contenedores que más elementos de esa taxonomía
  * Todos los campos que puedan ser auto-completados tendrán un auto-completado
  * Si hay plantillas de formularios (campo resistencia para resistencias, capacidad para condensadores, métrica para
    tornillos) los campos del formulario aparecerán mágicamente a través de una taxonomía que los asocie con esa
    plantilla de formulario.
  * Si se introducen fotos desde un móvil se podrá usar la galería o la cámara para subirlas, sin pasos intermedios.
  * Una serie de scrapers/agentes recomendarán contenido para relacionar a los elementos según su tipo y nombre:
    * De circuitos integrados buscará el dataset, librerías de software EDA,...
    * De materiales/consumibles como madera, tornillos, pla para la impresora 3D,... enlaces para comprar repuestos
  * Si se introduce una taxonomía nueva (no autocompletada) se creará automáticamente colgando de root o la
    taxonomía detalle desde la que se creó el elemento.
* Reordenado sencillo:
  * Facilidad de cambiar un elemento de contenedor
  * Facilidad de mover un contenedor (por ejemplo caja) a otro contenedor (por ejemplo otra balda).
  * Facilidad para mover taxonomías dentro de la jerarquía

# Faseado, iteraciones

Se usará versionado semántico y se desplegarán conjuntos de cambios que completen una o más épicas. Al ser una aplicación
intensiva de datos, que posiblemente requiera reindexado completo del buscador al cambiar los esquemas prefiero
mantener contenido el número de despliegues.

## Versión 0.1 (Modelo inicial)

- Versión inicial de las entidades `Containers`, `Items`, algunas taxonomías como `Kind` y `Tags`, algunas entidades para
agregar contenido relacionado a los `Items` como `ItemLinks` e `ItemFiles`.
- Carga inicial de entidades `Kind` y `Tags` desde un admin de django.
- Despliegue

## Versión 0.2 (Esqueleto de los formularios)

- Vistas de listado y/o detalle de las entidades `Containers`, `Items` y taxonomías:
  - `Containers`:
    - El detalle del elemento raiz es a la vez un listado de los elementos hijos. Así recursivamente.
    - Desde un detalle/listado se pueden crear/eliminar `Containers` o `Items` contenidos.
    - Un botón habilitará la edición del `Container` actual
    - Función mover `Container` bajo otro padre (None es el elemento raíz).
  - `Items`:
    - El listado de elementos contendrá una serie de filtros para reducir el número de elementos listados:
        - Por `Container` o `Container` e hijos.
        - Por taxonomía e hijos.
        - Por nombre
    - Desde el listado se pueden crear/eliminar `Items` y acceder al detalle
    - Un botón habilitará la edición del `Item` desde su vista de detalle.
  - Las taxonomías tendrán un comportamiento similar a categorías.

## Versión 0.3 (Mejoras front para UX y productividad de usuario desde front)

- Mejora de widgets usados para la ingesta de datos en los formularios:
  - Mejoras en los componentes jerárquicos
  - Mejora en los autocompletados (ajuste de tiempos de delay, etc)
  - Mejoras visuales del diseño responsive. Mejor ajuste para funcionar en móvil y uw screens.
  - Mejoras de estilos
  - Componentes de previsualización de contenido relacionado (evaluación y realización si corresponde)
  - Elementos de navegación mejorados:
    - Facilidad para cambiar de contexto a otras entidades para cambiar estrategia de uso
    - Acceso cercano a los contenidos relacionados con la vista actual

## Versión 0.4 (Mejoras de trazabilidad, monitorización y profiling para mejora de productividad desde back)

- Integrar un apm o similar (mínimo una forma sencilla de visualizar las queries realizadas por request)
- Métricas
  - De plataforma. Con alertas de query slows, uso de memoria,..,
  - De aplicación. Uso de vistas, tiempos medios y latencias por vista,...
- Alertas sobre las métricas
- Pruebas de carga

## Versión 0.5 (Business intelligence)

- Calidad de la categorización:
  - Reportes de entropía por contenedor respecto a las taxonomías de los elementos y por taxonomía respecto a los contenedores donde se hayan los items
  - Treemaps de cardinalidades de las taxonomías
- Calidad del buscador:
  - Histogramas de ordinales mínimos en los que se ha hecho click tras una búsqueda
  - Ranking de búsquedas con mayor ordinal mínimo en los que se ha hecho click tras una búsqueda
- Calidad de `Items`
  - Ranking de la cantidad de los detalles de `Items` más y menos visitados en un rango de tiempo.
  - Ranking de cantidad de impresiones de `Items` en listados y búsquedas
  - Ranking de ratios de visitas a detalles de `Items` por número de impresiones en listados y búsquedas.
- Definir actuables a partir de los datos obtenidos. Por ejemplo:
  - Recategorizar taxonomías para obtener distribuciones de cardinalidades más homogeneas.
  - Mejorar funciones de boosting de los resultados del buscador para obtener más conversión en los primeros elementos 
    de los listados.
  - Tirar elementos o almacenar en sitios menos accesibles elementos que no se usan nunca
  - Evaluar compra de elementos sobre los que las búsquedas no arrojan resultados buenos o poner en lugares más
    accesibles elementos que se usan más a menudo.
  - Añadir un peso a cada elemento que actue como t-norma (lógica borrosa) en el boosting de búsqueda para que elementos
    birriosos aparezcan menos.

## Versión 0.6 (????)

Seguramente haya que redefinir las épicas anteriores, así que ya vale de inventar.

# Propuestas de tecnologías a evaluar o decididas sin evaluación
## Tecnología para la interfaz de usuario
Web. Es lo que mejor conozco y no quiero poner en riesgo este proyecto por monear con interfaces nativas

## Tecnologías front-back
### HTMX
Pros:
  * Old school, zona de confort
  * La lógica de negocio está centralizada en el back. Front se centra en la lógica de presentación para UX.
  * Simple, fácil de integrar, se lee la documentación completa en una tarde.

Contras:
  * Poca comunidad
  * Mayor consumo de red. La respuesta renderizada html tiende a pesar más que los datos serializados en json.
  * Se acaba haciendo lógica de presentación en back

### API REST y SPA en cliente
Pros:
  * Menor consumo de red
  * Gran comunidad en varias de las alternativas
  * Es posible que existan componentes visuales ya creados que puedan reutilizarse (revisar) (no se yo)

Contras:
  * Sobre-ingeniería. Demasiados pasos para obtener el js final (ts, jsx, bla, bla, bla)
  * Lógica en cliente
  * Al final el REST expuesto no representa solo estado y necesita de un pseudo-rpc para mover `Containers`, Autocompletado de ...

### API GraphQL
Pros:
  * Es fácil hacer a medida la proyección de las consultas

Contras:
  * Todo entra por el mismo endpoint
  * Añade capa absurda de parseo de su lenguaje
  * No es fácil evitar el problema N+1 en muchas implementaciones
  * Lógica en cliente

## Frameworks CSS
Evaluar en función de los siguientes requisitos:
* Que mediante Saas, Less, Sccs o lo que sea se pueda ocultar la morralla overhead en el html. Sobre todo sí uso htmx.
* Que sea fácil hacer una página responsive
* Que sirva de base, provea, de un componente de autocompletado decente.
* Sencillo.
* Que dependa lo mínimo de javascript.
* Que se haya mantenido activo los últimos años.

[Listado de frameworks css para revisar](https://github.com/awesome-css-group/awesome-css#frameworks-art)

Tras leer varias comparativas consistentes con [esta](https://ritza.co/articles/tailwind-css-vs-bootstrap-vs-material-ui-vs-styled-components-vs-bulma-vs-sass/)
está claro que gana bulma.css.

## Tecnología Back
Si uso htmx me quedo con django:
* integración con htmx (incluidos middlewares para temas de eventos y demás)
* sistema de plantillas molón
* Formularios ~~acoplados~~ integrados con los modelos, las plantillas y las vistas para simplificar el CRUD.
* ORM decente para simplificar filtros dinámicos (necesarios para el listado de `Items`) y con buen soporte para migraciones.

Usando api rests me da un poco igual que usar. Aunque Django sigue teniendo buen ORM y hay alternativas a DRF (no me gusta nada).

Seguramente que use Django con Postgresql. Y si las consultas de búsqueda flojean añado un solr (index transient is good) o
un elasticsearch (si es que sigue siendo OSS).

## Valoración conjunta

Me llama usar Django, PostgreSQL, HTMX, algún framework css con un buen default para hacerme la vida front sencilla y
vanilla js para todo lo demás. Algo simple, old-school, fácil de desarrollar y mantener son mis preferencias para este
proyecto.

