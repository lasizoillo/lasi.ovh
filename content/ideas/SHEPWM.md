+++
title = "Selective Harmonics Elimination PWM"
description = "Usar Selective Harmonics Elimination PWM para diferentes proyectos como inversores o amplificadores de audio"
+++

# Selective Harmonics Elimination PWM

## Eficiencia energética de los PWM para la generación de ondas

### El interruptor ideal

Idealmente (y la práctica no se aleja apenas) un interruptor no consume energía. Cuando el circuito está cerrado
su resistencia es prácticamente cero: $$  R \approx 0 \implies P = R I^2 \approx 0 $$
Mientras que con el circuito abierto la resistencia tiende a infinito y la circulación de intensidad es prácticamente
cero. Despejando en la fórmula anterior vemos que la potencia consumida es prácticamente nula.

### El consumo en conmutación

Pero si la CPU de un ordenador son un montón de transistores que están en corte o conducción deberían consumir
una potencia despreciable y carecer de disipador. ¿Por qué no es así? Porque la mayoría del consumo se produce en la
conmutación y millones de transistores conmutan miles de millones de veces por segundo. Es como cuando cierras el
circuito de un interruptor y ves saltar una chispa. Ese fogonazo requiere energía que es consumida en la conmutación
del interruptor. Un circuito con un pwm no necesita tantos transistores ni conmutar a esa velocidad, por lo que el
consumo en conmutación es moderado.

### El demonio está en los armónicos

Vamos a suponer que estamos implementando un inversor para obtener una onda alterna senoidal de 230V a 50Hz a partir
de unas baterías de 12V continua que cargamos con unas placas. Como primera aproximación usamos una onda cuadrada y
fardamos de ella diciendo que es una "onda senoidal de 8 bits".

Durante unos meses todo es muy bonito hasta que se rompe el compresor del frigo. Mala suerte. Lo cambias y a los meses
se vuelve a romper, algo no funciona. Esa "onda senoidal de 8 bits" es la suma de la onda senoidal deseada más un
montón de armónicos que son disipadas en el motor del compresor causando las averías. Una primera solución es añadir
un filtro pasabanda de 50 Hz que deje pasar una onda senoidal y disipe en forma de pérdidas los armónicos no deseados.
No notamos que las baterías duren menos porque esa disipación ya la hacía el compresor, solo que ahora no se hace allí
y no se va a estropear.

¿Pero no hay una forma de generar un tren de pulsos que no venga acompañado de esos armónicos no deseados? Si, la hay.
Desde hace mucho tiempo había trucos para mejorar la respuesta sumando diferentes trenes de pulsos. Pero su obtención
era a prueba de ensayo y error. Desde hace muchos años tenía la mosca detrás de la oreja tratando de encontrar unas
matemáticas que sirvieran para resolver ese problema, pero no encontré respuesta hasta hoy. Esas matemáticas no se
conocen, pero [han mejorado los métodos](https://www.astesj.com/publications/ASTESJ_030322.pdf)
[para conseguir aproximaciones](http://www.ijeijournal.com/papers/Vol.4-Iss.12/G04123844.pdf).

Si mediante la mejora del tren de pulsos generados consigues quitarte 30 armónicos son 30 armónicos que no tienes
que disipar en la etapa de filtrado. Las baterías deberían durar más, el inversor podría ser más pequeño y requerir
menores sistemas de disipación, los aparatos deberían durar como si estuvieran conectados a la red eléctrica.

Pero no solo se generan armónicos por cómo se genera la onda en el inversor, también las cargas puede añadir armónicos
a la corriente y generar problemas. Por eso hay
[variaciones en lazo cerrado para adaptar el inversor a las cargas](https://ijeecs.iaescore.com/index.php/IJEECS/article/viewFile/26784/16362).

## Posibles usos

### Inversor eficiente de onda cuasi senoidal

Ver los ejemplos anteriores

### Mejora de calidad/rendimiento de amplificadores de clase D

El problema de las señales de audio es que trabajan a frecuencias mayores y la generación de la onda de salida
depende de la entrada, no vamos a generar la misma onda senoidal como era el caso del inversor. De todas formas
[las propuestas existen](https://www.researchgate.net/publication/336977485_A_Class_D_Power_Amplifier_for_Multi-Frequency_Eddy_Current_Testing_Based_on_Multi-Simultaneous-Frequency_Selective_Harmonic_Elimination_Pulse_Width_Modulation).
¿Merece la pena?

### Generador de señales

¿Tiene sentido? Posiblemente no, pero quizá puedan hacerse sintetizadores electrónicos si resulta más barato que
aplicar el algoritmo a un amplificador clase D.