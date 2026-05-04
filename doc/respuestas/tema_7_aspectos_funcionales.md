<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

A continuación se presentan las preguntas **intactas** junto con sus respuestas, redactadas en **estilo impersonal**, adaptadas a los conocimientos indicados y cumpliendo la longitud solicitada (2–4 párrafos por respuesta, sin contar el código).

***

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a función en C es una variable que almacena la dirección de memoria de una función. Esto permite invocar funciones de manera indirecta, pasar funciones como parámetros o seleccionar dinámicamente qué función ejecutar en tiempo de ejecución. Conceptualmente, es similar a un puntero a datos, pero apuntando a código ejecutable.

Este mecanismo es muy utilizado en C para implementar tablas de funciones, callbacks o comportamientos configurables, ya que el lenguaje no dispone de orientación a objetos. La signatura del puntero debe coincidir exactamente con la de la función a la que apunta.

En el ejemplo, se define una función que transforma una cadena a mayúsculas y se crea un puntero local llamado `aMayusculas` que apunta a dicha función, el cual se usa posteriormente para invocarla.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirMayusculas(char* texto) {
    for (int i = 0; texto[i] != '\0'; i++) {
        texto[i] = toupper(texto[i]);
    }
    return texto;
}

int main() {
    char cadena[] = "hola mundo";

    char* (*aMayusculas)(char*);
    aMayusculas = convertirMayusculas;

    printf("%s\n", aMayusculas(cadena));
    return 0;
}
```

***

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima y generalmente corta, que puede definirse directamente en el lugar donde se usa. A diferencia de las funciones tradicionales, las lambdas no requieren nombre y suelen utilizarse para representar comportamientos simples que se pasan como valores.

En JavaScript, las funciones lambda (arrow functions) son ciudadanos de primera clase, lo que significa que pueden asignarse a variables, pasarse como parámetros o devolverse desde otras funciones. Su sintaxis es concisa y habitual en programación funcional.

En Java, a partir de Java 8, las funciones lambda se introducen junto con interfaces funcionales. En el ejemplo se utiliza `Function<String, String>` para almacenar la referencia a la función lambda.

```javascript
let aMayusculas = texto => texto.toUpperCase();
console.log(aMayusculas("hola mundo"));
```

```java
import java.util.function.Function;

Function<String, String> aMayusculas = texto -> texto.toUpperCase();
System.out.println(aMayusculas.apply("hola mundo"));
```

***

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación basado en el uso de funciones puras, evitando estados compartidos y efectos secundarios. Se centra en expresar la computación como la evaluación de funciones matemáticas y en el uso de composición de funciones.

Lenguajes como Java 8 se consideran multi-paradigma porque, aunque nacieron como orientados a objetos, han incorporado características funcionales como lambdas, streams y funciones como valores. Esto permite combinar distintos enfoques según el problema a resolver.

Decir que las funciones son ciudadanos de primera clase implica que las funciones pueden tratarse como cualquier otro valor: almacenarse en variables, pasarse como argumentos y devolverse como resultados. Esto es un pilar fundamental del paradigma funcional.

***

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una función lambda en Java se compone de tres partes: la lista de parámetros, el operador `->` y el cuerpo de la función. Esta forma permite definir implementaciones concisas de interfaces funcionales.

Si la lambda tiene un solo parámetro, se pueden omitir los paréntesis. Si el cuerpo es una única expresión, se pueden omitir las llaves y la palabra `return`. Esto favorece un estilo de programación más expresivo y legible.

El tipo de los parámetros suele inferirse automáticamente a partir del contexto, lo que reduce la verbosidad. Sin embargo, también se pueden declarar explícitamente si se desea mayor claridad.

```java
(x, y) -> x + y
texto -> texto.toUpperCase()
```

***

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro es una práctica habitual en programación funcional, ya que permite delegar parte del comportamiento a quien llama al método. Esto incrementa la reutilización y reduce el acoplamiento.

En JavaScript, al no existir restricciones de tipo, la función se recibe directamente y se invoca. En Java, se utiliza una interfaz funcional como `Function<String, String>` para definir el tipo del parámetro.

El método `transformar` aplica la función recibida al texto suministrado, demostrando cómo el comportamiento puede variar sin modificar el método.

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

let aMayusculas = t => t.toUpperCase();
console.log(transformar("hola", aMayusculas));
```

```java
import java.util.function.Function;

static String transformar(String texto, Function<String, String> transformador) {
    return transformador.apply(texto);
}
```

***

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Definir la función lambda directamente en la llamada permite expresar el comportamiento de forma localizada, evitando la creación de variables auxiliares. Esto es especialmente útil cuando la función solo se usa una vez.

Este estilo favorece un código más declarativo, donde se describe *qué* se quiere hacer en lugar de *cómo*. Además, mejora la legibilidad cuando la transformación es sencilla.

El ejemplo muestra cómo invertir una cadena utilizando una expresión lambda definida inline.

```javascript
console.log(transformar("hola", t => t.split("").reverse().join("")));
```

```java
System.out.println(transformar(
    "hola",
    t -> new StringBuilder(t).reverse().toString()
));
```

***

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un cierre o *closure* se produce cuando una función lambda captura variables de su entorno donde fue definida. Estas variables forman parte del contexto de la lambda, incluso cuando la función se ejecuta fuera de ese ámbito.

En Java, las variables locales capturadas deben ser *efectivamente finales*, es decir, no modificarse tras su inicialización. Esto garantiza seguridad y coherencia en la ejecución concurrente.

El ejemplo muestra una lambda que concatena una cadena externa al texto de entrada, demostrando el comportamiento de cierre.

```java
String sufijo = "!!!";

Function<String, String> transformar = t -> t + sufijo;
System.out.println(transformar.apply("hola"));
```

***

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Los punteros a funciones en C son direcciones de memoria sin contexto adicional. No capturan variables del entorno ni poseen información semántica sobre el comportamiento, más allá de su signatura.

Las funciones lambda, en cambio, pueden capturar variables y mantener estado inmutable mediante closures. Además, están integradas en un sistema de tipos más expresivo y seguro, como el de Java.

Otra diferencia clave es que las lambdas están orientadas a trabajar con interfaces funcionales y composición, mientras que los punteros a funciones son una herramienta de bajo nivel.

***

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver funciones permite crear comportamientos parametrizados. En este caso, se genera una función de descuento que encapsula un porcentaje concreto, devolviendo una función que aplica dicho descuento a una cantidad.

La variable `porcentaje` queda capturada por la lambda devuelta, formando un cierre. Cada función de descuento mantiene su propio porcentaje, aunque se haya creado a partir del mismo método.

Esto permite crear múltiples funciones especializadas a partir de una única función generadora.

```java
static Function<Double, Double> crearDescuento(double porcentaje) {
    return precio -> precio * (1 - porcentaje);
}

Function<Double, Double> dto10 = crearDescuento(0.10);
Function<Double, Double> dto20 = crearDescuento(0.20);

System.out.println(dto10.apply(100.0));
System.out.println(dto20.apply(100.0));
```

***

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional es una interfaz que define exactamente un método abstracto. Este método representa la operación que implementará la función lambda asociada al tipo.

Java utiliza interfaces funcionales como puente entre el sistema de tipos estático y las funciones lambda. El compilador infiere qué método implementar a partir de la signatura.

Pueden contener métodos `default` o `static`, pero solo un método abstracto. La anotación `@FunctionalInterface` es opcional, pero recomendable.

***

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Definir una interfaz funcional personalizada permite modelar un comportamiento específico del dominio del problema. En este caso, se define un transformador de cadenas.

La interfaz declara un único método abstracto que recibe un `String` y devuelve otro `String`. Esto permite usar lambdas o referencias a métodos con dicha interfaz.

La anotación `@FunctionalInterface` ayuda a garantizar que se cumple el contrato.

```java
@FunctionalInterface
interface Transformador {
    String transformar(String texto);
}
```

***

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Mediante generics se puede generalizar la interfaz `Transformador` para trabajar con distintos tipos de entrada y salida. Esto aumenta enormemente su reutilización.

El uso de parámetros de tipo permite definir transformaciones arbitrarias sin perder seguridad de tipos. El compilador verifica la coherencia entre tipos.

El ejemplo muestra un transformador que convierte un `Double` en un `Integer` redondeado.

```java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}

Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
```

***

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java proporciona un amplio conjunto de interfaces funcionales en el paquete `java.util.function`, diseñadas para cubrir los casos de uso más comunes.

Entre las más importantes se encuentran `Function<T,R>`, `Consumer<T>`, `Supplier<T>` y `Predicate<T>`. Estas cubren transformación, consumo, provisión y evaluación booleana, respectivamente.

También existen variantes especializadas para tipos primitivos, como `IntFunction`, `DoubleConsumer` o `LongPredicate`, que evitan el *boxing* innecesario.

***

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` permite recorrer colecciones aplicando una acción a cada elemento. Representa una alternativa funcional al bucle `for` tradicional.

Este enfoque separa la lógica de iteración de la lógica de procesamiento, haciendo el código más declarativo. Se apoya en la interfaz funcional `Consumer`.

El ejemplo muestra cómo imprimir solo los valores positivos de una lista.

```java
List<Integer> numeros = List.of(-2, 3, 0, 5);

numeros.forEach(n -> {
    if (n > 0) {
        System.out.println("Positivo: " + n);
    }
});
```

***

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma `Consumer<? super T>` sigue el principio **PECS**: *Producer Extends, Consumer Super*. Indica que si un parámetro consume valores de tipo `T`, se debe usar `? super T`.

Esto permite pasar consumidores que acepten tipos más generales que `T`, aumentando la flexibilidad del código sin perder seguridad de tipos.

Aplicado al método `transformar`, si la función consume un `String`, podría aceptarse un `Function<? super String, ? extends R>` para permitir mayor reutilización.

***

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Las referencias a métodos permiten reutilizar métodos existentes como funciones, sin necesidad de definir lambdas explícitas. Son una forma más concisa y legible cuando la firma coincide.

En JavaScript, los métodos pueden extraerse directamente, aunque hay que controlar el contexto (`this`). En Java, la referencia mantiene el vínculo con la instancia.

El ejemplo muestra cómo obtener y usar dicha referencia.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const p = new Persona("Ana");
const saludo = p.saludar.bind(p);
saludo();
```

```java
class Persona {
    private String nombre;
    Persona(String nombre) { this.nombre = nombre; }
    void saludar() { System.out.println("Hola, soy " + nombre); }
}

Persona p = new Persona("Ana");
Runnable saludo = p::saludar;
saludo.run();
```

***

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

Java permite cuatro tipos principales de referencias a métodos. Cada uno corresponde a una situación concreta donde la signatura coincide con una interfaz funcional.

Las referencias pueden ser a métodos estáticos, constructores, métodos de una instancia concreta o métodos de una clase aplicables a cualquier instancia.

Estos mecanismos reducen la verbosidad y mejoran la expresividad del código.

```java
// Método estático
Function<String, Integer> parsear = Integer::parseInt;

// Constructor
Supplier<List<String>> crearLista = ArrayList::new;

// Método de instancia concreta
Persona p = new Persona("Luis");
Runnable saludar = p::saludar;

// Método de instancia de cualquier objeto
Function<String, Integer> longitud = String::length;
```

***

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

La interfaz `Comparator` es un ejemplo claro de uso funcional en Java. Permite definir criterios de ordenación de forma concisa mediante expresiones lambda.

En la versión manual, se implementa explícitamente la lógica de comparación. En la versión moderna, se utilizan métodos auxiliares de `Comparator` para componer comparaciones.

Ambas versiones producen el mismo resultado, pero la segunda es más expresiva y mantenible.

```java
Collections.sort(personas, (p1, p2) -> {
    int cmp = Integer.compare(p1.getEdad(), p2.getEdad());
    if (cmp == 0) {
        return p1.getNombre().compareTo(p2.getNombre());
    }
    return cmp;
});
```

```java
Collections.sort(
    personas,
    Comparator.comparingInt(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```
