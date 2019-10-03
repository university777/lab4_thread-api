# Thread API # 

En esta sección escribiremos algunos programas multi-hilo y usaremos una herramienta específica llamada ```helgrind``` para encontrar problemas en estos programas. 

## Questions ##

1. Primero codifique ```main-race.c```. Examine el código de manera que usted pueda ver (ojalá de manera obvia) un data race en el código. Ahora ejecute ```helgrind``` (al teclear ```valgrind --tool=helgrind main-race```) y vea como este programa reporta los *data races*. ¿Se muestran las líneas de código involucradas?, ¿Qué otra información entrega este programa?

> Al compilar mediante gcc el archivo main-race.c, usando el comando gcc main-race.c -o main-race -lpthread, luego corrimos el valgrind mediante el comando en consola: valgrind --tool=helgrind main-race. Éste nos arrojó 2 errores en el código ya que se da una condición de carrera (algo esperado ya que teniamos las dos secciones criticas desprotegidas).Con valgrind podemos ver la dirección de memoria en la cual se da la condición de carrera, así como el nombre de los hilos que causan esta condición. Valgrind también especifica que no se está usando ningún lock mediante el mensaje ```locks held: none```

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

> En un principio helgrind nos muestra que hay inconvenientes que tienen que ser controlados relacionados a la obtención y liberación del lock; lo que podría desencadenarse en un deadlock circular. En la anterior captura podemos ver que en distintos segmentos del código nos alerta sobre ejecuciones luego de la acquisión del lock.


5. Ahora ejecute ```helgrind``` en ```main-deadlock-global.c```. Examine el código. ¿Tiene este el mismo problema que ```main-deadlock.c```? ¿Muestra ```helgrind``` el mismo reporte de error? ¿Qué dice esto a cerca de herramientas como ```helgrind```?  
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto5_Deadlock-Global.png)  

> Acá podemos ver el mismo problema que señalaba helgrind en el punto anterior, puede verse en la consola, donde vemos que un lock es primero observado por otra dirección en cierto caso de ejecución que podría darse con éste código.

6. Ahora observe ```main-signal.c```. Este código usa una variable (```done```) para señalar que el hijo esta hecho y que el padre puede continuar. ¿Por qué este códido es ineficiente? (En que termina el padre dedicando su tiempo, si el hijo toma una gran cantidad de tiempo en completarse).  

> Éste código es ineficiente ya que el hilo padre quedará esperando en un ciclo (sin hacer nada) mientras el hijo marca la variable done para que el padre pueda salir del ciclo y continuar con su ejecución. Si el hijo tiene interrupciones muy largas y tarda mucho que completarse, el padre seguirá el dicho ciclo desperdiciando recursos.

7. Ahora ejecute ```helgrind``` para este programa. ¿Qué reporta helgrind?, ¿Es correcto el código?  
> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto7_Hengrid_Deadlock.png)

> En este caso la respuesta que da helgrind es muy clara, básicamente nos señala que pueden haber condiciones de carrera en nuestro código, además señala que no hay locks para que sean tomados por un hilo en especifico.

> ![alt tag](https://github.com/university777/lab4_thread-api/blob/master/lab/Punto7_Hengrid_Deadlock2.png)
