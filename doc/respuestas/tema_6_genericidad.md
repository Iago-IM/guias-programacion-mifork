<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
A continuación se desarrollan las respuestas solicitadas, adaptadas a los conocimientos indicados y manteniendo estilo impersonal, con una longitud de entre 2 y 4 párrafos por respuesta (sin contar los fragmentos de código).

***

# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En C, al no existir mecanismos de orientación a objetos ni genéricos, una forma habitual de conseguir estructuras de datos “genéricas” consiste en utilizar punteros de tipo `void*`. Un `void*` es un puntero sin tipo concreto, capaz de apuntar a cualquier zona de memoria, independientemente del tipo real del dato almacenado. Esto permite definir, por ejemplo, un array de `void*` que pueda almacenar direcciones de memoria de datos heterogéneos.

La estructura de datos realmente no almacena los valores, sino punteros a ellos, delegando en el programador la responsabilidad de conocer el tipo real en cada acceso. Esto suele combinarse con convenciones externas o información adicional para saber cómo interpretar cada elemento. Aunque es flexible, esta solución no proporciona seguridad de tipos en tiempo de compilación.

```c
// Array genérico en C usando void*
void* datos[10];

int a = 5;
double b = 3.14;

datos[0] = &a;
datos[1] = &b;
```

En Java, antes de la introducción de los genéricos (Java 5), se empleaba un enfoque similar usando la clase base `Object`. Como todas las clases heredan de `Object`, un array de `Object` puede almacenar referencias a cualquier objeto. Sin embargo, ocurre algo parecido a C: al recuperar los elementos es necesario hacer *downcasting* explícito.

```java
Object[] datos = new Object[10];

datos[0] = "Hola";
datos[1] = Integer.valueOf(42);
```

***

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta

La programación genérica es un paradigma que permite definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos con los que trabajan. La idea central es escribir el código una sola vez y reutilizarlo con distintos tipos, manteniendo la corrección y coherencia del comportamiento. Esto favorece la reutilización, la claridad y la reducción de duplicación de código.

En lenguajes modernos, la programación genérica se apoya en mecanismos del lenguaje (como *generics* en Java o *templates* en C++) que permiten expresar explícitamente los tipos como parámetros. De esta forma, el compilador puede verificar que el uso de los tipos es correcto y coherente en todo el programa.

Los ejemplos anteriores con `void*` y `Object` pueden considerarse ejemplos muy básicos y rudimentarios de genericidad, ya que permiten almacenar “cualquier cosa”. Sin embargo, no son genéricos en sentido estricto moderno, porque no proporcionan seguridad de tipos ni expresan claramente las restricciones sobre los datos. Son más bien soluciones de compromiso ante la ausencia de mecanismos genéricos en el lenguaje.

***

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta

El principal problema de emplear `void*` en C es la ausencia total de chequeo de tipos por parte del compilador. Al no conocer el tipo real del dato apuntado, el compilador no puede detectar accesos incorrectos, conversiones erróneas o usos indebidos de la memoria. Cualquier error de interpretación del tipo se manifiesta en tiempo de ejecución, normalmente como fallos difíciles de depurar.

En Java, aunque existe un sistema de tipos más seguro, el uso de `Object` presenta problemas similares. Al recuperar un elemento almacenado como `Object`, es necesario realizar un *downcasting* explícito al tipo deseado. Este *casting* no se comprueba completamente en tiempo de compilación y puede provocar una excepción `ClassCastException` en tiempo de ejecución si el tipo no es el esperado.

Además, ambos enfoques permiten mezclar tipos incompatibles en la misma estructura sin que el compilador lo detecte. Esto incrementa la posibilidad de errores lógicos y obliga al programador a llevar un control manual de los tipos, lo que va en contra de la robustez y mantenibilidad del código.

***

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta

Los parámetros de tipo son una abstracción que permite definir clases, interfaces o métodos indicando que trabajarán con uno o varios tipos aún desconocidos. Estos tipos se representan mediante identificadores (como `T`, `E`, `K`, `V`) y se especifican en el momento de instanciar o invocar la entidad genérica. De esta forma, el mismo código se adapta a diferentes tipos concretos.

A diferencia del uso de `Object` o `void*`, los parámetros de tipo forman parte del sistema de tipos del lenguaje. El compilador conoce qué tipo concreto se está utilizando en cada instanciación y puede verificar que todas las operaciones sobre esos datos son válidas. Esto elimina la necesidad de *downcasting* inseguro y evita una gran clase de errores en tiempo de ejecución.

En esencia, los parámetros de tipo permiten expresar con claridad la intención del programador: qué partes del código dependen de un tipo genérico y cuáles no. Esto mejora la legibilidad, la seguridad y la reutilización del software, manteniendo una sintaxis relativamente sencilla.

***

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, los *generics* permiten indicar explícitamente el tipo de los elementos que almacena una colección. Por ejemplo, una `ArrayList<String>` garantiza que solo se puedan introducir objetos de tipo `String`. El compilador verifica esta restricción y, además, asegura que los elementos recuperados ya son del tipo correcto, sin necesidad de casting.

```java
import java.util.ArrayList;

ArrayList<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");

for (String s : lista) {
    System.out.println(s.length());
}
```

En C++, los *templates* permiten un enfoque similar, pero resuelto de forma diferente por el compilador. Un `std::vector<std::string>` es una instancia concreta del template `vector` para el tipo `std::string`. El compilador genera código específico para ese tipo, garantizando seguridad y eficiencia.

```cpp
#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> v;
v.push_back("uno");
v.push_back("dos");

for (const std::string& s : v) {
    std::cout << s.size() << std::endl;
}
```

En ambos casos, el recorrido demuestra que cada elemento es del tipo concreto `String` (`std::string` en C++) con total seguridad, sin comprobaciones dinámicas ni conversiones explícitas.

***

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase genérica, el compilador utiliza la información de los parámetros de tipo para comprobar que el uso de la clase es correcto. Sin embargo, Java y C++ implementan este proceso de forma muy distinta. Ambos lenguajes persiguen seguridad y reutilización, pero con estrategias internas opuestas.

En Java, los genéricos se implementan mediante *type erasure*. Tras la compilación, la información sobre los parámetros de tipo se elimina y se reemplaza por tipos más generales (normalmente `Object` o el límite superior indicado). Esto significa que todas las instancias genéricas comparten el mismo bytecode y que la información de tipo no está disponible en tiempo de ejecución.

En C++, en cambio, los *templates* se instancian en tiempo de compilación. Para cada tipo concreto utilizado, el compilador genera una versión específica del código. Este proceso se denomina instanciación de plantillas y produce código distinto para cada tipo, lo que aumenta el tamaño del binario pero permite mayor optimización y conservación completa de la información de tipo.

***

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`.

### Respuesta

Una clase genérica con dos parámetros de tipo permite modelar una estructura que agrupa dos valores potencialmente heterogéneos. En Java, esto se define indicando dos parámetros de tipo en la declaración de la clase. El compilador garantiza que se usan de manera coherente en atributos, constructores y métodos.

```java
public class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() { return primero; }
    public B getSegundo() { return segundo; }
}
```

Un uso típico de esta clase es como tipo de retorno múltiple. Por ejemplo, una función que calcula la media y la desviación típica de un array de `double` puede devolver ambos valores agrupados en un `Par<Double, Double>`.

```java
public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;

    double var = 0.0;
    for (double d : datos) var += (d - media) * (d - media);
    double desviacion = Math.sqrt(var / datos.length);

    return new Par<>(media, desviacion);
}
```

***

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo.

### Respuesta

Si se define un método que recibe dos `Object` y devuelve un `Object`, el compilador no puede garantizar que ambos parámetros sean del mismo tipo ni que el valor devuelto sea el esperado. El programador debe realizar *downcasting* explícito al usar el resultado, con el consiguiente riesgo de errores en tiempo de ejecución.

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```

Con un método genérico, se introduce un parámetro de tipo a nivel de método. Esto permite expresar que ambos argumentos y el resultado comparten exactamente el mismo tipo. El compilador impide llamadas inconsistentes y elimina la necesidad de casting.

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

En términos de ventajas, el enfoque genérico (i) evita completamente el *downcasting*, ya que el tipo del retorno es conocido en compilación, y (ii) fuerza que ambos argumentos sean del mismo tipo, algo que no puede garantizarse usando simplemente `Object`.

***

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

En Java, es posible establecer restricciones en los parámetros de tipo mediante *bounded type parameters*. Esto permite indicar que un tipo genérico debe heredar de una clase o implementar una interfaz concreta. Por ejemplo, `<T extends Number>` fuerza a que `T` sea algún subtipo de `Number`, habilitando el uso de métodos comunes como `doubleValue()`.

Una primera solución sin genéricos consiste en definir las coordenadas directamente como `Number`. Esto permite usar `Integer`, `Double`, etc., pero pierde precisión sobre el tipo concreto con el que se trabaja.

```java
public class Punto {
    private Number x, y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

La solución genérica refuerza el chequeo de tipos y garantiza que ambas coordenadas son del mismo tipo numérico.

```java
public class Punto<T extends Number> {
    private T x, y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

Debido al *type erasure*, tras la compilación el tipo real de `T` se convierte en `Number`, que es su límite superior.

***

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

La solución basada únicamente en `Number` permite crear un `Punto` con coordenadas de tipos distintos, por ejemplo una `Integer` y una `Double`. Esto es posible porque ambas son subclases de `Number`, pero introduce incoherencias semánticas y obliga a tratar siempre los valores como `Number`, perdiendo información del tipo original.

En la solución con genéricos, no es posible mezclar tipos de coordenadas dentro del mismo `Punto`. Si se instancia un `Punto<Integer>`, ambas coordenadas deben ser `Integer`. Esto refuerza el chequeo de tipos en compilación y evita combinaciones inconsistentes, haciendo el diseño más robusto y predecible.

Respecto a los tipos devueltos, en la solución sin genéricos el método `getX` devuelve `Number`, independientemente del tipo real almacenado. En cambio, en la solución con genéricos, `getX` devuelve `T`, es decir, el tipo concreto utilizado al instanciar el `Punto`, proporcionando mayor precisión y seguridad de tipos para el código cliente.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

### Respuesta

El problema del diseño original es que la interfaz `Punto` no expresa, a nivel de tipos, que la distancia solo tiene sentido entre puntos del mismo tipo concreto. Al usar un parámetro `Punto` genérico, el compilador permite comparar un `Punto2D` con un `Punto3D`, obligando a introducir comprobaciones dinámicas con `instanceof` y conversiones explícitas, lo cual degrada el diseño y la seguridad de tipos.

La solución consiste en hacer la interfaz genérica usando un patrón conocido como *F-bounded polymorphism*. Se introduce un parámetro de tipo que representa el propio subtipo concreto. Así, el método `distanciaA` queda tipado de forma que solo acepta puntos del mismo tipo que la implementación concreta.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Cada implementación concreta fija el parámetro de tipo a sí misma. De este modo, la firma del método obliga al compilador a garantizar que los tipos coinciden, eliminando por completo la necesidad de `instanceof` y *downcasting*.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)
        );
    }
}
```

Una implementación `Punto3D` seguiría exactamente el mismo patrón con tres coordenadas. El compilador impide ahora comparar puntos de dimensiones distintas, reforzando el diseño en tiempo de compilación y evitando errores en tiempo de ejecución.

***

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque `String` es subtipo de `Object`, en Java **no** se cumple que `List<String>` sea subtipo de `List<Object>`. Los tipos genéricos son invariantes respecto a su parámetro de tipo. Permitir lo contrario rompería la seguridad de tipos, ya que se podría insertar en una `List<String>` un objeto que no fuera `String` a través de una referencia `List<Object>`.

En cambio, los arrays en Java **sí** son covariantes, por lo que `String[]` es subtipo de `Object[]`. Esto se permite por razones históricas del lenguaje, pero introduce un problema: el compilador no puede impedir que se intente almacenar un objeto incompatible en tiempo de ejecución. En ese caso, la JVM lanza una `ArrayStoreException`.

```java
Object[] arr = new String[10];
arr[0] = Integer.valueOf(5); // Error en tiempo de ejecución
```

Un tipo genérico es **covariante** si `G<String>` es subtipo de `G<Object>`, **contravariante** si ocurre al revés, e **invariante** si no existe relación de subtipo entre `G<String>` y `G<Object>`. En Java, los genéricos normales son invariantes, mientras que los arrays son covariantes, lo que explica la diferencia de comportamiento y los posibles fallos en ejecución.

***

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un *wildcard* (`?`) es una forma de expresar un tipo genérico desconocido, permitiendo relajar la invariancia de los genéricos de manera segura. Los wildcards siempre se usan en puntos de uso (*use-site variance*), no en la definición de la clase, y permiten declarar rangos de tipos aceptables.

`List<? extends T>` expresa **covarianza**: la lista puede ser de `T` o de cualquier subtipo de `T`. Es apropiada cuando la colección solo se va a leer, ya que el compilador no permite insertar elementos (salvo `null`), porque no conoce el subtipo concreto. Es el enfoque típico para producir valores.

```java
public static double suma(List<? extends Number> lista) {
    double resultado = 0.0;
    for (Number n : lista) {
        resultado += n.doubleValue();
    }
    return resultado;
}
```

Por el contrario, `List<? super T>` expresa **contravarianza**: la lista puede ser de `T` o de cualquier supertipo de `T`. Es adecuada cuando se van a insertar elementos, ya que cualquier `T` es compatible con un supertipo. A cambio, al leer elementos solo se garantiza que son de tipo `Object`.

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

En resumen, `? extends` se usa cuando se necesita flexibilidad para **leer** datos, y `? super` cuando se necesita flexibilidad para **escribir** datos, expresado habitualmente con la regla: *PECS* (*Producer Extends, Consumer Super*).

