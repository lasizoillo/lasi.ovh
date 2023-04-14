+++
title = "GUI para tailscale que funcione en linux"
description = "Ideas para hacer un GUI de tailscale que funcione en linux y algunas posibles integraciones"
+++

# ¿Qué es tailscale?

[Tailscale](https://tailscale.com/) es una vpn. Realmente usa WireGuard para encargarse de la VPN y tailscale es un
conjunto de utilidades y servicios por encima que hacen cosas muy interesantes.

# ¿Por qué deberías usar tailscale?

Imagina que tienes un disco duro muy tocho con un cholón libros en casa y durante un viaje te has quedado sin lectura y
quieres descargarte otro libro para seguir leyendo. Alguna de las opciones que se te podrían ocurrir para solucionar
el problema son:

* Contratar un servicio de disco replicado en la nube. Llámalo nextcloud, dropbox, google drive o un montón de leuros
  al mes. Eso igual ayudaría a dejar de tener diógenes digital y coleccionar libros que jamás tendrás tiempo
  de leer.
* Contratar un hosting dedicado baratillo con mucho almacenamiento y montarte tu la nube con nextcloud. También
  necesitarías un dominio para acceder al hosting. Seguro que es más barato y engorroso que la opción anterior.
* Contratar un dominio que tenga servicio de dyn-dns, montar el servicio en tu casita con el disco duro tocho y
  configurar el router para redireccionar los puertos a esa máquina. Es mucho más barato y mucho más engorroso que
  las opciones anteriores.
* Instalar tailscale en la máquina con el disco duro muy tocho y no apagarla cuando sales de viaje con otro dispositivo
  que también tiene tailscale instalado. Tailscale se encarga de darte un [magic DNS](https://tailscale.com/kb/1081/magicdns/),
  hacer [hole punching](https://en.wikipedia.org/wiki/Hole_punching_(networking)) para que ambos dispositivos se vean
  sin que tengas que preocuparte de la config del router (aunque si le ayudas habilitando UPnP mucho mejor), cifrar el
  contenido de extremo a extremo para que usar una wifi pública no suponga un problema y darte una forma sencilla de 
  [servir](https://tailscale.com/kb/1242/tailscale-serve/) esa carpeta con libros que verá tu dispositivo de viaje y que
  no estará disponible para que la vea ningún cedro. Sin duda es la opción más barata y sencilla de todas. Pero no lo
  suficiente...

# ¿Por qué necesito un GUI para tailscale que funcione en linux?

Por temas de conciliación familiar. Yo no necesito una interfaz visual que me evite tener que lanzar 4 comandos
sencillitos en la consola. Pero podría ser una herramienta útil para compartir ficheros con mi novia incluso a través
de la red local. Y aquí es donde usar la consola me causa los problemas de conciliación familiar y tener que aguantar
chistes de matrix por abrir la terminal.

# ¿Qué debería tener el GUI ideal?

## GUIs actuales

[tailscale-status](https://github.com/maxgallup/tailscale-status) es una extensión para gnome shell que se pone en el
menú superior y te deja hacer con facilidad lo de enviar un fichero a otra máquina. Serviría para mandar un video a la
compu que está conectada al proyector, enviar un fichero al móvil para tener lectura en el trono, compartir entre
nosotros ficheros,... pero no para el caso de descargarte un libro cuando estás de viaje.

[trayscale](https://github.com/DeedleFake/trayscale) también crea un tray, permite configurar cosillas, pero no tiene
nada de enviar/recibir ficheros como el anterior. Tampoco la opción [tailscale serve](https://tailscale.com/kb/1242/tailscale-serve/)
para compartir a través de una url un directorio lleno de libros.

No he visto más, así que tocará extender estos proyectos o [hacer un tercero](https://xkcd.com/927/).

## Requisitos básicos

Implementar por completo las opciones del comando `tailscale` con una interfaz amigable. Así no hay otro como yo que se
inventa un cuarto proyecto también incompleto.

## Requisitos extras

Poder enviar con D&D ficheros arrastrando desde el nautilus a uno de los nodos listados por la app. O, en su defecto,
hacer un [script de nautilus](https://help.ubuntu.com/community/NautilusScriptsHowto) que permita enviar de forma fácil
archivos desde el propio explorador de ficheros.

Quizá algunos temas de la config tenían más sentidos integrados como extensión del network manager y dejar la app solo
para temas más de usuario:

* enviar ficheros
* configurar ruta de recepción o deshabilitar recepción
* servir ficheros o exponer servicios
* Usar el [PeerAPI](https://github.com/tailscale/tailscale/blob/64bbf1738e17dcc5cc74807a338606c9ebcfb687/ipn/ipnlocal/peerapi.go#L688)
  para extraer más info de los servers remotos, aunque por ahora no parece un api pública.

Opciones en nodos remotos:

* Usar servicios http(s)
* Lanzar consolas a server ssh
* ...