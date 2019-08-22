# Introducción al thread API #

## Parte teórica ##

Lea y haga un resumen de una hoja (ni mas ni menos) del capitulo  [Interlude: Thread API](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-api.pdf) del libro OSTEP de Remzi.

## Parte partica ##

Los siguientes ejemplos (tomados del siguiente [enlace](https://github.com/remzi-arpacidusseau/ostep-code/tree/master/threads-api)) muestran como usar POSIX thread API:
1. ```thread_create.c```: Programa simple para la creación de un hilo donde los argumentos son pasados al hilo desde linea e comandos.
2. ```thread_create_with_return_args.c```: programa que retorna valores dese un hilo a su padre.
3. ```thread_create_simple_args.c```: Simpler argument/return value passing for the lazy (Aun no traducido).

Ejecute el makefile; cada archivo llamado foo.c genera un ejecutable con foo el cual usted puede ejecutar; por ejemplo:

```bash
prompt> ./thread_create
```

**Nota**: Esta se analizará en clase.
