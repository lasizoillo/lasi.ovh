+++
title = "Gestionar Recetas con Hugo"
description = "Implementación práctica y migración de krecipes a Hugo para gestionar recetas"
+++

# Recetas

* [Proyecto en Github](https://github.com/lasizoillo/recetas-belen)
* [Demo](http://recetas.lasi.ovh) (Tiene +500 recetas sin imágenes)
* Estado: parado

El proyecto incluye un importador de recetas desde krecipes. Tiene un tema de hugo hecho para manejar recetas bastante
tocado por los siguientes motivos:

* Mi novia quería poder navegar por categorías y lista de ingredientes
* La generación de los listados de categorías e ingredientes tardaban mucho y se ha modificado la forma de listar
* Cambios visuales a gusto de la consumidora
* Tocar el buscador (queda mucho por tocar aquí) para poder buscar por ingredientes
* Cambiar los enlaces rápidos a tags por enlaces rápidos a categorías e ingredientes en general
* Castellanizar un poco algunas entidades para que le sea más fácil la gestión de las recetas a la usuaria

## Temas pendientes

- [ ] Exportación consultable en diversos dispositivos, cualquiera de las siguientes:
  - [ ] Generación de html que funcione sin instalar un web server para ver en tablet
  - [ ] Generación de pdf, mobi u otro ebook para poder ver en tablet y en el ebook reader
- [ ] Simplificar la vida a usuarios con scripts que automaticen el server local y la generación de builds