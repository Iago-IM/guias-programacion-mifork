<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
- Abstracción; olvidarse de detalles para:
	* Manejar mejor temas complejos
	* Facilitar la modificación (el mantenimiento)

- Encapsulación: 
	* Unir información y funciones sobre esa información en un mismo artefacto (como en las clases de Python).
	* Ocultar partes al exterior.

- Armonía:
	* Crear jerarquías (como cuando una clase de Python parte de otra).
	* La herencia (o jerarquía de tipos) permite la reutilización de código aunque no es la mejor forma. La herencia es además un mecanismo de abstracción.

- Polimorfismo:
	* Misma función, distintas implementaciones en función del tipo (facilita la abstracción).


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

|Lenguajes				|Notas 																	|
|-----------------------|-----------------------------------------------------------------------|
|Python, JavaScript, PHP| LENGUAJES DINÁMICOS (se programa más rápido pero son menos eficientes)|
|Java, C#				| LENGUAJES COMPILADOS GC (Seguros en memoria)							|
|Rust?, C++...			| LENGUAJES COMPILADOS NO GC											|

Rust es un caso a parte, es seguro en memoria pero también tiene ventajas de los lenguajes no GC.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
Ensamblador: secuencia de instrucciones y saltos arbitrarios (similar a los .bat de Windows).

&darr;  

Programación estructurada: se quita el salto arbitrario (tenemos bifunciones como if y swith, iteración como los bucles for y while...).

&darr;  

Programación modular: tenemos "librerías", "paquetes", "interfaces"... para encapsular y reutilizar.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
- Identidad: todo objeto tiene una identidad única (piénsalo como su propia dirección en memoria).
- Estado: el valor de sus atributos (campos).
- Comportamiento: los métodos (funciones) del objeto.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
- Clase &rarr; Molde que define el estado y el comportamiento
- Objetos &rarr; entidad concreta creada a partir de una clase. Varios objetos de la misma clase pueden tener distintos estados.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En lenguajes como Java, los **objetos se almacenan normalmente en el *heap***, una zona de memoria diseñada para datos cuyo tamaño y tiempo de vida no se conocen de antemano. El *heap* permite crear objetos en tiempo de ejecución mediante `new`, y su uso es gestionado por la máquina virtual, no por el programador. En cambio, las variables que solo almacenan referencias a esos objetos suelen ubicarse en la **pila (*stack*)**, pero el objeto real permanece en el *heap*.

No todos los lenguajes gestionan la memoria de la misma manera. En C++, por ejemplo, los objetos pueden ir tanto al *stack* como al *heap*, dependiendo de si se crean de forma automática o con `new`. En cambio, lenguajes gestionados como Java o C# colocan casi todos los objetos en el *heap* y utilizan mecanismos automáticos para controlar su ciclo de vida. Otros lenguajes con modelos diferentes, como Python o JavaScript, también almacenan la mayoría de sus objetos en un *heap* gestionado, aunque su implementación interna pueda variar.

La **recolección de basura** (*garbage collection*) es un proceso automático que libera memoria ocupada por objetos que ya no son accesibles desde el programa. En lugar de que el programador deba liberar manualmente la memoria —como ocurre en C o C++ con `free` o `delete`—, el recolector analiza qué objetos ya no pueden ser usados y los elimina. Este mecanismo reduce errores típicos de gestión manual, como fugas de memoria o accesos a memoria liberada.

El recolector puede funcionar con distintos algoritmos, pero su objetivo siempre es mantener el *heap* limpio sin intervención directa del programador. Aunque facilita la programación, también implica que la liberación de memoria no es instantánea ni totalmente predecible, ya que depende del funcionamiento interno del recolector.



## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un método es cualquiera de las funciones definidas dentro de una clase.

La sobrecarga de métodos es la posibilidad de crear métodos dentro de una clase con el mismo nombre, pero cambiando el tipo y/o número de sus parámetros.

Ejemplo:
```java
class Calculadora {
	// sin estado

	int sumar(int a, int b) {
		return a + b;
	}

	double sumar(double a, double b) {
		return a + b;
	}
}

main() {
	Calculadora miCalculadora = new Calculadora();

	int suma1 = miCalculadora.sumar(4, 6);
	double suma2 = miCalculadora.sumar(4.3, 6.7);
}
```


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```java
class Punto {
    int x; // visibilidad por defecto (package-private)
    int y; // visibilidad por defecto (package-private)

    double calculaDistanciaAOrigen() {
        // Distancia de (x,y) a (0,0)
        return Math.sqrt(x * x + y * y);
    }
}

public class Ejercicio1 {
    public static void main(String[] args) {
        Punto miPunto = new Punto();
        miPunto.x = 5;
        miPunto.y = 3;

        double d = miPunto.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d);
    }
}
```	


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El **punto de entrada** de un programa en Java es siempre el método  
`public static void main(String[] args)`. La máquina virtual de Java comienza la ejecución buscando exactamente ese método dentro de alguna clase pública accesible. No se permite cambiar ni su nombre ni su firma, porque es la convención que utiliza la JVM para iniciar el programa.

La palabra clave **`static`** indica que un elemento pertenece a la **clase**, no a los objetos creados a partir de ella. En el caso de `main`, se utiliza porque la JVM necesita poder llamarlo sin crear primero una instancia de la clase. De esta manera, `main` existe “por sí mismo”, y puede ejecutarse sin depender de ningún objeto.

El modificador `static` no se emplea solo en `main`. También se usa en **métodos auxiliares**, **constantes**, **atributos compartidos** y funciones que no dependen del estado concreto de un objeto. Cuando algo es `static`, todas las instancias de la clase comparten ese mismo recurso.

Finalmente, combinar **`static` con `final`** es habitual para definir **constantes**, es decir, valores globales e inmutables accesibles sin crear objetos. Por ejemplo, `static final double PI = 3.14159;` permite declarar un valor que nunca cambia y que pertenece a la clase completa, no a cada instancia.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilar un programa Java desde la línea de comandos se emplea el compilador `javac`. Este comando toma un archivo `.java` y genera uno o varios ficheros `.class` que contienen el *bytecode*. Por ejemplo, si se tiene una clase pública llamada `Ejercicio1` en `Ejercicio1.java`<span style="color:purple;">\* </span>, se compila con `javac Ejercicio1.java`. Tras la compilación, el programa se ejecuta con el comando java, `java Ejercicio1`, indicando el nombre de la clase **sin** la extensión `.class`, ya que la herramienta `java` busca automáticamente ese archivo en el directorio actual.

<span style="color:purple;">\*Nota importante: un archivo .java solo puede tener **UNA** clase pública que se debe llamar como el archivo, por eso al usar el comando java no hace falta especificar el nombre del archivo, solo la clase.\*> </span>

Java se considera un lenguaje **compilado e interpretado al mismo tiempo**. Primero se compila a *bytecode*, un formato intermedio independiente del sistema operativo. Después, ese *bytecode* es ejecutado por la **Máquina Virtual de Java (JVM)**, que actúa como un intérprete optimizado. Este modelo permite que el mismo programa funcione en distintos sistemas sin cambiar el código fuente, siempre que exista una JVM disponible para esa plataforma.

La **máquina virtual** es un software que simula una computadora ideal diseñada para ejecutar *bytecode* Java. Se encarga de tareas como la gestión de memoria, la recolección de basura y la optimización en tiempo de ejecución. Este enfoque desacopla el código del hardware real, lo que facilita la portabilidad y añade una capa de seguridad y control sobre la ejecución del programa.

Los archivos `.class` contienen el **bytecode**, una representación binaria del programa que no es directamente ejecutable por el procesador físico, pero sí por la JVM. Este bytecode sirve como formato estándar para distribuir programas Java, ya que puede ejecutarse en cualquier sistema donde haya instalada una máquina virtual compatible. De este modo, el ciclo completo de Java combina portabilidad, optimización y control automático del entorno de ejecución.



## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
`new` es el operador que **crea un objeto** en Java: reserva memoria para él, construye la instancia y devuelve una **referencia** para poder usarla. En el ejemplo de `Punto`, `new Punto()` significa “crear un `Punto` nuevo”, y el resultado se guarda en una variable (la referencia), que permite acceder a sus atributos y métodos. A diferencia de C/C++, no se está “instanciando en la pila” como una variable automática típica, sino creando un objeto gestionado por el entorno de ejecución (normalmente en el *heap*).

Un **constructor** es un método “especial” que se ejecuta **justo al crear** un objeto con `new`. Sirve para **inicializar** el estado inicial del objeto (por ejemplo, asignar valores a sus atributos). Se reconoce porque **se llama igual que la clase** y **no tiene tipo de retorno** (ni siquiera `void`). Si no se define ninguno, Java proporciona un constructor por defecto sin parámetros, pero en cuanto se define uno propio, ese constructor por defecto deja de generarse automáticamente.

Ejemplo de clase `Empleado` con atributos `dni`, `nombre` y `apellidos`, y un constructor que inicializa esos campos:

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

// Ejemplo de uso
class PruebaEmpleado {
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "Pérez Gómez");
        System.out.println(e.dni + " - " + e.nombre + " " + e.apellidos);
    }
}
```


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
En el constructor de la pregunta anterior aparece `this`, que se utiliza para referirse al **objeto que se está construyendo** y distinguir sus atributos (`this.dni`) de los parámetros del constructor (`dni`). De esta forma, al ejecutar `new Empleado(...)` se garantiza que la instancia nace ya con un DNI, nombre y apellidos coherentes, en lugar de quedar con valores vacíos o `null`.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
```java
class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        int dx = otro.x - this.x;
        int dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Ejercicio1 {
    public static void main(String[] args) {
        Punto a = new Punto();
        a.x = 5;
        a.y = 3;

        Punto b = new Punto();
        b.x = 1;
        b.y = 6;

        System.out.println("Distancia de a al origen: " + a.calculaDistanciaAOrigen());
        System.out.println("Distancia de a a b: " + a.distanciaA(b));
    }
}
```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
En Java, el paso de parámetros es **siempre por valor (por copia)**, pero hay que distinguir **qué valor** se está copiando. Si el parámetro es un objeto como `Punto`, lo que se copia es la **referencia** al objeto (una “dirección”/identificador), no el objeto completo. Por eso, **dentro del método se puede modificar el estado del mismo objeto** y esos cambios se ven fuera: el método y el código que llama están apuntando al **mismo** objeto. En cambio, si dentro del método se reasigna el parámetro para que apunte a otro `Punto` nuevo, esa reasignación **no** afecta al de fuera, porque solo cambia la copia local de la referencia.

```java
static void cambiaCoordenadas(Punto p) {
    p.x = 100;          // Modifica el MISMO objeto -> se verá fuera
    p.y = 200;
}

static void reasigna(Punto p) {
    p = new Punto();    // Solo cambia la referencia local -> NO se verá fuera
    p.x = 7;
    p.y = 8;
}
```

Si en lugar de `Punto` se recibe un `int`, entonces el “valor” copiado es el **número** en sí (tipo primitivo). Al modificarlo dentro del método, solo se modifica la **copia local**, y **no** cambia la variable original fuera del método. Esto es lo que suele entenderse como “por copia” en C con tipos básicos.

```java
static void incrementa(int n) {
    n = n + 1;          // Solo cambia la copia local
}

public static void main(String[] args) {
    int a = 10;
    incrementa(a);
    System.out.println(a); // Imprime 10
}
```

En resumen: con **objetos** se copia la **referencia**, así que cambiar atributos (`p.x = ...`) sí afecta fuera; con **primitivos** como `int`, se copia el **valor numérico**, así que cambiarlo dentro no afecta fuera.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
En Java, `toString()` es un método definido en la clase base `Object`, por lo que **todo objeto lo tiene**. Su finalidad es devolver una **representación textual** del objeto (un `String`) para mostrarlo de forma legible, por ejemplo al imprimirlo con `System.out.println(objeto)`. Si no se redefine, la versión por defecto suele ser poco útil (típicamente algo como `NombreClase@hash`), así que lo normal es **sobrescribirlo** para que describa el estado relevante del objeto (sus atributos principales).

Este concepto existe en muchos otros lenguajes, aunque no siempre con el mismo nombre exacto. En Python se usa `__str__` (y a veces `__repr__`), en C++ suele lograrse con la sobrecarga del operador `<<` para poder enviar el objeto a un `ostream`, y en JavaScript también existe `toString()` aunque su personalización depende del modelo de prototipos. En todos los casos se persigue lo mismo: facilitar la conversión del objeto a texto para depuración, trazas o salida por consola.

A continuación se muestra un ejemplo de `toString()` para una clase `Punto` que devuelva el formato `(x, y)`. Se recomienda usar `@Override` para que el compilador detecte errores si la firma no coincide con la del método heredado.

```java
class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

public class Ejemplo {
    public static void main(String[] args) {
        Punto p = new Punto(5, 3);
        System.out.println(p);            // llama implícitamente a p.toString()
        System.out.println(p.toString()); // llamada explícita
    }
}
```


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta
Si bien un `struct` de C tiene **atributos** (permite agrupar datos bajo un mismo tipo como las clases), **no** puede tener **métodos asociados**.

Además, un `struct` carece de mecanismos fundamentales de las clases: no tiene **constructores**, no permite **ocultación de datos** mediante niveles de visibilidad, no admite **herencia** ni **polimorfismo**, ni dispone de un sistema automático de creación de instancias como `new`. Las variables de tipo `struct` en C son simples bloques de memoria con una plantilla fija.

En resumen, un `struct` de C puede verse como la “mitad de una clase”: aporta los **datos**, pero no el **comportamiento**, ni las herramientas que permiten tratar esos datos como **objetos** completos dentro de un modelo orientado a objetos.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
Para “emular” en C la clase `Punto` con su método `calculaDistanciaAOrigen`, se define un `struct` para agrupar `x` e `y` y se escribe una función “externa” que reciba un `Punto` y calcule la distancia. En C, como no existen métodos asociados al tipo, la función se nombra de forma convencional (por ejemplo `Punto_calculaDistanciaAOrigen`) y se le pasa el dato explícitamente. Si se quiere evitar copiar el `struct`, se pasa un **puntero**; si el `struct` es pequeño, pasarlo por valor también sería válido, pero el puntero se parece más al estilo “orientado a objetos”.

Lo que ha pasado con `this` es que deja de ser implícito: en Java, `this` es una referencia automática al objeto sobre el que se invoca el método; en C, esa referencia se convierte en un parámetro explícito, normalmente llamado `self`, `p` o `this`. Es decir, el `this` de Java se corresponde con el puntero que se pasa a la función en C (`const struct Punto* self`). Por eso, dentro de la función se accede con `self->x` y `self->y` (operador `->`), que equivale conceptualmente a `this.x` y `this.y` en Java.

```c
#include <stdio.h>
#include <math.h>

struct Punto {
    int x;
    int y;
};

/* "Método" emulado: recibe el objeto explícitamente */
double Punto_calculaDistanciaAOrigen(const struct Punto* self) {
    return sqrt((double)self->x * self->x + (double)self->y * self->y);
}

int main(void) {
    struct Punto miPunto;
    miPunto.x = 5;
    miPunto.y = 3;

    double d = Punto_calculaDistanciaAOrigen(&miPunto);
    printf("Distancia al origen: %f\n", d);
    return 0;
}
```

En resumen, la “magia” de un método en una clase consiste, en gran parte, en que el lenguaje **inyecta** ese primer parámetro oculto (`this`) y permite escribir la llamada como `miPunto.calculaDistanciaAOrigen()` en lugar de `Punto_calculaDistanciaAOrigen(&miPunto)`. La diferencia clave no es la fórmula, sino la **sintaxis** y el hecho de que Java liga de forma natural datos y operaciones, mientras que en C hay que mantener esa asociación por convención.
