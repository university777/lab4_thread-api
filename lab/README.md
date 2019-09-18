# Thread API # 

En esta sección escribiremos algunos programas multi-hilo y usaremos una herramienta específica llamada ```helgrind``` para encontrar problemas en estos programas. 

## Questions ##

1. Primero codifique ```main-race.c```. Examine el código de manera que usted pueda ver (ojalá de manera obvia) un data race en el código. Ahora ejecute ```helgrind``` (al teclear ```valgrind --tool=helgrind main-race```) y vea como este programa reporta los *data races*. ¿Se muestran las líneas de código involucradas?, ¿Qué otra información entrega este programa?

> (VOLVER A HACER)   Al compilar mediante gcc el archivo main-race.c, usando el comando gcc main-race.c -o main-race lpthread, luego podemos ejecutar el archivo. En nuestro caso agregamos un printf que mostrara el valor del contador, el cual como se evidenció en el código estaba en una condición de carrera protegida por un lock ya que el contador era accedido por el hilo principal y el hilo que creamos. Luego corrimos el valgrind mediante el comando en consola: valgrind --tool=helgrind main-race. Éste nos arrojó 0 errores en el código (algo esperado ya que teniamos las secciones criticas protegidas con el lock) luego comentamos, tanto la inicialización del lock, como las funciones que lo tomaban y soltaban y volvimos a correr el comando de valgrind; esta vez se evidencia que hay dos errores. Con valgrind podemos ver la dirección de memoria que esta en la condición de carrera, así como el nombre de los hilos que intentan acceder a esta.

2. ¿Qué ocurre cuando usted elimina una de las líneas que generan problemas en el código? Ahora agrege un lock alrededor de las actualizaciones de la variable compartida, y entonces alrededor de ambas. ¿Qué reporta ```helgrind``` en cada uno de estos casos?  
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto2_Con1Lock.png)  
![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto2_Con2Locks.png)  
![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto2_ConLineaComentada.png)
![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto2_SinLocks.png)
 

> Al comentar una de las líneas con la race condición y correr nuevamente el comando de valgrind, tenemos que ahora solamente hay un error en nuestro código. Por otro lado, si rodeamos la sección crítica del hilo principal que accede a la variable 'balance' con un lock que previamente habiamos inicializado y corremos helgrind, este nos muestra que nuestro código tiene 3 errores.


3. Ahora observe ```main-deadlock.c```. Examine el código. Este código tiene un problema conocido como deadlock. ¿Puede ver que problema podrá este tener?

> En este código se da la condición hold and wait que es uno de las condiciones para que exista un deadlock.

4. Ahora ejecute ```helgrind``` en este código. ¿Qué reporta helgrind?  
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto4_Deadlock.png)
![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto4_Deadlock2.png)  

5. Ahora ejecute ```helgrind``` en ```main-deadlock-global.c```. Examine el código. ¿Tiene este el mismo problema que ```main-deadlock.c```? ¿Muestra ```helgrind``` el mismo reporte de error? ¿Qué dice esto a cerca de herramientas como ```helgrind```?  
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto5_Deadlock-Global.png)  

6. Ahora observe ```main-signal.c```. Este código usa una variable (```done```) para señalar que el hijo esta hecho y que el padre puede continuar. ¿Por qué este códido es ineficiente? (En que termina el padre dedicando su tiempo, si el hijo toma una gran cantidad de tiempo en completarse).  

7. Ahora ejecute ```helgrind``` para este programa. ¿Qué reporta helgrind?, ¿Es correcto el código?  
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto7_Hengrid_Deadlock.png)
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto7_Hengrid_Deadlock2.png)
