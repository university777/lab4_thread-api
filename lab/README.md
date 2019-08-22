# Thread API # 

En esta sección, escribiremos algunos programas multi-hilo y usaremos una herramienta especifica llamada ```helgrind``` para encontrar problemas en estos programas. 

## Actividades previas al laboratorio ##

Los siguientes ejemplos (tomados del siguiente [enlace](https://github.com/remzi-arpacidusseau/ostep-code/tree/master/threads-api)) muestran como usar POSIX thread API:
1. ```thread_create.c```: Programa simple para la creación de un hilo donde los argumentos son pasados al hilo desde linea e comandos.
2. ```thread_create_with_return_args.c```: programa que retorna valores dese un hilo a su padre.
3. ```thread_create_simple_args.c```: Simpler argument/return value passing for the lazy (Aun no traducido).

Ejecute el makefile; cada archivo llamado foo.c genera un ejecutable con foo el cual usted puede ejecutar; por ejemplo:

```bash
prompt> ./thread_create
```
## Questions ##

1. Primero codifique ```main-race.c```. Examine el código de manera que usted pueda ver (ojala de manera obvia) una data race en el código. Ahora ejecute ```helgrind``` (al teclear ```valgrind --tool=helgrind main-race```) y vea como este reporta la data race. ¿Se muestran las lineas de código involucradas?, ¿Que otra información da?
2. ¿Que pasa cuando usted se mueve una de las lineas que generan problemas en el código? Ahora agrege un lock alrededor de las actualizaciones de la variable compartida, y entonces alrededor de ambas. ¿Que reporta ```helgrind``` en cada uno de estos casos?
3. Ahora observe ```main-deadlock.c```. Examine el código. Este código tiene un problema conocido como deadlock. ¿Puede ver que problema podrá este tener?
4. Ahora ejecute ```helgrind``` en este código. ¿Que reporta helgrind?
5. Ahora ejecute ```helgrind``` en ```main-deadlock-global.c```. Examine el código. ¿Tiene este el mismo problema que ```main-deadlock.c```? ¿Muestra ```helgrind``` el mismo reporte de error? ¿Que dice esto a cerca de herramientas como ```helgrind```?
6. Ahora observe ```main-signal.c```. Este código usa una variable (```done```) para señalar que el hijo esta hecho y que el padre puede continuar. ¿Por que este códido es ineficiente? (En que termina el padre dedicando su tiempo, si el hijo toma una gran cantidad de tiempo en completarse)
7. Ahora ejecute ```helgrind``` en este programa. ¿Que reporta helgrind?, ¿Es correcto el código?
8. Ahora observe una versión levemente modificada del código, la cual es encontrada en ```main-signal-cv.c```. Esta versión usa una variable de condición para señalizar (y asociar un lock). ¿Por que este código es mejor que la versión previa? ¿Es la corrección, o el desempeño, o ambos?
9. Ejecute de nuevo ```helgrind``` en ```main-signal-cv``` ¿Reporta algunos errores?
