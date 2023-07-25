"# JAVA-STATE-PATTERN" 🚀
--------------------- 


![image](https://github.com/D-Perez85/JAVA-FACTORY-METHOD-PATTERN/assets/77124855/ed22828a-241f-4e7f-8125-bc296d7398dc)


State es un patrón de diseño de comportamiento que permite a un objeto alterar su comportamiento cuando su estado interno cambia. Parece como si el objeto cambiara su clase.




## Problema
El patrón State está estrechamente relacionado con el concepto de la Máquina de estados finitos.

![image](https://github.com/D-Perez85/JAVA-STATE-PATTERN/assets/77124855/3c1828af-b24c-4830-b2bf-0693d89586a7)


La idea principal es que, en cualquier momento dado, un programa puede encontrarse en un número finito de estados. 

Dentro de cada estado único, el programa se comporta de forma diferente y puede cambiar de un estado a otro instantáneamente. 

Sin embargo, dependiendo de un estado actual, el programa puede cambiar o no a otros estados. Estas normas de cambio llamadas transiciones también son finitas y predeterminadas. También puedes aplicar esta solución a los objetos. 

Imagina que tienes una clase Documento. Un documento puede encontrarse en uno de estos tres estados: Borrador, Moderación y Publicado. El método publicar del documento funciona de forma ligeramente distinta en cada estado:


       ○	  En Borrador, mueve el documento a moderación
       ○	  En Moderación, hace público el documento, pero sólo si el usuario actual es un administrador
       ○	  En Publicado, no hace nada en absoluto


![image](https://github.com/D-Perez85/JAVA-STATE-PATTERN/assets/77124855/90a9c74c-04f9-4a49-92ff-a3681695e70f)


Las máquinas de estado se implementan normalmente con muchos operadores condicionales (if o switch) que seleccionan el comportamiento adecuado dependiendo del estado actual del objeto. 

Normalmente, este “estado” es tan solo un grupo de valores de los campos del objeto y la mayor debilidad de una máquina de estado basada en condicionales se revela una vez que empezamos a añadir más y más estados y comportamientos dependientes de estados a la clase Documento. 

La mayoría de los métodos contendrán condicionales monstruosos que eligen el comportamiento adecuado de un método de acuerdo con el estado actual. 

Un código así es muy difícil de mantener, porque cualquier cambio en la lógica de transición puede requerir cambiar los condicionales de estado de cada método.
El problema tiende a empeorar con la evolución del proyecto y es bastante difícil predecir todos los estados y transiciones posibles en la etapa de diseño. 

Por ello, una máquina de estados esbelta, creada con un grupo limitado de condicionales, puede crecer hasta convertirse en un abotargado desastre con el tiempo.



## Solución

El patrón State sugiere que crees nuevas clases para todos los estados posibles de un objeto y extraigas todos los comportamientos específicos del estado para colocarlos dentro de esas clases.

En lugar de implementar todos los comportamientos por su cuenta, el objeto original, llamado contexto, almacena una referencia a uno de los objetos de estado que representa su estado actual y delega todo el trabajo relacionado con el estado a ese objeto.

![image](https://github.com/D-Perez85/JAVA-STATE-PATTERN/assets/77124855/4c2340bc-786f-4389-a1a3-be2aa1b964d0)

Para la transición del contexto a otro estado, sustituye el objeto de estado activo por otro objeto que represente ese nuevo estado. Esto sólo es posible si todas las clases de estado siguen la misma interfaz y el propio contexto funciona con esos objetos a través de esa interfaz.

Esta estructura puede resultar similar al patrón Strategy, pero hay una diferencia clave: en el patrón State, los estados particulares pueden conocerse entre sí e iniciar transiciones de un estado a otro, mientras que las estrategias casi nunca se conocen.


## Analogía en el mundo real

Los botones e interruptores de tu smartphone se comportan de forma diferente dependiendo del estado actual del dispositivo:

       ○    Cuando el teléfono está desbloqueado, al pulsar botones se ejecutan varias funciones.
       ○    Cuando el teléfono está bloqueado, pulsar un botón desbloquea la pantalla.
       ○    Cuando la batería del teléfono está baja, pulsar un botón muestra la pantalla de carga.




![image](https://github.com/D-Perez85/JAVA-STATE-PATTERN/assets/77124855/ac0fc78c-0e47-4c58-9e59-9564f4a6ec66)


##  Aplicabilidad
:pushpin: cuando tengas un objeto que se comporta de forma diferente dependiendo de su estado actual, el número de estados sea enorme y el código específico del estado cambie con frecuencia. El patrón sugiere que extraigas todo el código específico del estado y lo metas dentro de un grupo de clases específicas. 
Como resultado, puedes añadir nuevos estados o cambiar los existentes independientemente entre sí, reduciendo el costo de mantenimiento.


:pushpin: cuando tengas una clase contaminada con enormes condicionales que alteran el modo en que se comporta la clase de acuerdo con los valores actuales de los campos de la clase. El patrón State te permite extraer ramas de esos condicionales a métodos de las clases estado correspondientes. 
Al hacerlo, también puedes limpiar campos temporales y métodos de ayuda implicados en código específico del estado de fuera de tu clase principal.


:pushpin: cuando tengas mucho código duplicado por estados similares y transiciones de una máquina de estados basada en condiciones.
 El patrón State te permite componer jerarquías de clases de estado y reducir la duplicación, extrayendo el código común y metiéndolo en clases abstractas base.
 
 
## Cómo implementarlo
 
-	Decide qué clase actuará como contexto. Puede ser una clase existente que ya tiene el código dependiente del estado, o una nueva clase, si el código específico del estado está distribuido a lo largo de varias clases.
-	Declara la interfaz de estado. Aunque puede replicar todos los métodos declarados en el contexto, céntrate en los que pueden contener comportamientos específicos del estado.
-	Para cada estado actual, crea una clase derivada de la interfaz de estado. Repasa los métodos del contexto y extrae todo el código relacionado con ese estado para meterlo en tu clase recién creada. Al mover el código a la clase estado, puede que descubras que depende de miembros privados del contexto.
Hay varias soluciones alternativas:

 	    o	Haz públicos esos campos o métodos.
 	    o	Convierte el comportamiento que estás extrayendo para ponerlo en un método público en el contexto e invócalo desde la clase de estado. 
 	    o	Anida las clases de estado en la clase contexto, pero sólo si tu lenguaje de programación soporta clases anidadas.

  
-	En la clase contexto, añade un campo de referencia del tipo de interfaz de estado y un modificador (setter) público que permita sobrescribir el valor de ese campo.
-	Vuelve a repasar el método del contexto y sustituye los condicionales de estado vacíos por llamadas a métodos correspondientes del objeto de estado.
-	Para cambiar el estado del contexto, crea una instancia de una de las clases de estado y pásala a la clase contexto. Puedes hacer esto dentro de la propia clase contexto, en distintos estados, o en el cliente. Se haga de una forma u otra, la clase se vuelve dependiente de la clase de estado concreto que instancia.



## Pros y contras
•	 Principio de responsabilidad única. Organiza el código relacionado con estados particulares en clases separadas.

•	 Principio de abierto/cerrado. Introduce nuevos estados sin cambiar clases de estado existentes o la clase contexto.

•	 Simplifica el código del contexto eliminando voluminosos condicionales de máquina de estados.

•	 Aplicar el patrón puede resultar excesivo si una máquina de estados sólo tiene unos pocos estados o raramente cambia.



## Relaciones con otros patrones
:heavy_check_mark:	Bridge, State, Strategy (y, hasta cierto punto, Adapter) tienen estructuras muy similares. De hecho, todos estos patrones se basan en la composición, que consiste en delegar trabajo a otros objetos. Sin embargo, todos ellos solucionan problemas diferentes. Un patrón no es simplemente una receta para estructurar tu código de una forma específica. También puede comunicar a otros desarrolladores el problema que resuelve.

:heavy_check_mark:	State puede considerarse una extensión de Strategy. Ambos patrones se basan en la composición: cambian el comportamiento del contexto delegando parte del trabajo a objetos ayudantes. Strategy hace que estos objetos sean completamente independientes y no se conozcan entre sí. Sin embargo, State no restringe las dependencias entre estados concretos, permitiéndoles alterar el estado del contexto a voluntad.




## Comenzando 🚀

Estas instrucciones te permitirán obtener una copia del proyecto en funcionamiento en tu máquina local para propósitos de desarrollo y pruebas.


### Instalación 🔧

### `Clonar` 
Clonar el proyecto utilizando git clone  dentro de tu entorno local para poder obtener el codigo fuente. 
```
git clone https://github.com/D-Perez85/JAVA-STATE-PATTERN.git
```
### `Instalar Intellij IDE`
Necesitas este framework para poder compilar los archivos de prueba
```
https://www.jetbrains.com/es-es/idea/download/?section=windows
```
### `Run`
Una vez instalado el frame podras correr la App para ver este patron en funcionamiento.  

![image](https://github.com/D-Perez85/JAVA-STATE-PATTERN/assets/77124855/e6187447-0cc8-416f-9827-e2ffb8ccac1e)


Made with ❤️ by [Damian Perez](https://github.com/D-Perez85) 😊
