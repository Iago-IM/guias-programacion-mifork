<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

Encapsulación es como un escudo, añade protección a mi clase, la cual es un artefacto con estado y comportamiento, que permite ocultar miembros para:
-Garantizar que mi estado interno es siempre válido.
-Evitar que otro código dependa o acceda a partes que no quiero.
-Facilitamos poder cambiar partes sin afectar a otros.

La encapsulación busca agrupar los datos (estado) y las funciones que operan sobre esos datos (comportamiento) en una única entidad o unidad lógica, conocida como clase. Desde la perspectiva de C/C++ clásico, es similar a tomar un `struct` y empaquetar dentro de él también las funciones que lo manipulan, evitando tener los datos por un lado y las funciones globales por el otro. Por su parte, la ocultación de información busca restringir el acceso directo a los detalles internos de esa unidad desde el exterior, exponiendo únicamente lo estrictamente necesario.

La ocultación de información aporta numerosas ventajas fundamentales en el diseño de software. En primer lugar, previene la modificación accidental o indebida de los datos internos, garantizando que el estado del objeto siempre sea consistente. Además, reduce la complejidad del sistema, ya que el código externo no necesita conocer los detalles de implementación interna para interactuar con el objeto.

Finalmente, facilita enormemente el mantenimiento y la evolución del código. Al ocultar la implementación interna, es posible modificar la estructura de los datos (por ejemplo, cambiar una variable simple por una estructura de datos más compleja) sin que el código que utiliza dicha clase deba ser reescrito, siempre y cuando se mantenga inalterada la forma de interactuar con ella desde el exterior.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

Interfaz pública: los miembros que se ven desde otras clases, es decir, lo que no está oculto.

La interfaz pública de una clase es el conjunto de métodos (y ocasionalmente atributos, aunque no es recomendable) que están declarados con un nivel de acceso público y que, por tanto, pueden ser invocados desde cualquier otra parte del programa. Se puede entender como un "contrato" o un panel de control que la clase ofrece al mundo exterior para que interactúen con ella, definiendo qué puede hacer el objeto sin revelar cómo lo hace.

Esta interfaz pública es el mecanismo complementario a la ocultación de información. Mientras que la ocultación se encarga de esconder los atributos y la lógica interna para proteger el estado del objeto, la interfaz pública proporciona la única vía de acceso controlada y permitida hacia esos elementos ocultos.

Gracias a esta relación, el código cliente (quien usa la clase) pasa a depender exclusivamente de la interfaz pública y no de la implementación interna. Esto garantiza que cualquier cambio interno quede aislado gracias a la ocultación, mientras que el resto del programa sigue comunicándose fluidamente a través de la interfaz pública acordada.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública no es fácil de cambiar ya que tiene más consecuencias que si cambio partes ocultas.
"Cada método público que añado es un compromiso"

Hay que tener en cuenta la retrocompatibilidad.

Diseñar con cuidado la interfaz pública es crucial porque, una vez que una clase se integra en un sistema, otras partes del código (o incluso sistemas de terceros) comenzarán a depender de esos métodos públicos. Si la interfaz expone detalles innecesarios o está mal diseñada, se genera un alto grado de acoplamiento, haciendo que el sistema sea rígido y propenso a errores ante cualquier actualización.

Cambiar una interfaz pública no es fácil una vez que el código está en uso. Si se modifica la firma de un método público (su nombre, sus parámetros o su valor de retorno) o se elimina, será necesario modificar y recompilar todo el código externo que hacía llamadas a dicho método. Por ello, se recomienda aplicar el principio de exponer la menor cantidad de métodos posibles, garantizando que la interfaz sea estable y duradera.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Son condiciones que se cumplen durante toda la vida de los objetos de una clase. Se pueden expresar como expresiones booleanas y normalmente se refieren al estado interno.
El tipo de dato puede ser también una invariante de clase (aunque en Java esto es automático).

Ejemplos:
·Persona debe tener edad >= 0.
·Usuario debe tener contraseña de más de 5 caracteres.

La ocultación del estado interno permite asegurar la integridad de las invariantes de clase ya que los datos solo se modifican por métodos públicos que pueden comprobar la validez de los nuevos valores.

Las invariantes de clase son condiciones, reglas o restricciones sobre el estado de un objeto que deben cumplirse siempre para que dicho objeto sea válido durante toda su vida útil. Por ejemplo, en una clase que representa una fracción matemática, una invariante sería que el denominador nunca puede ser igual a cero; o en una clase que represente una fecha, que el día no puede ser mayor a 31.

La ocultación de información es la única manera de garantizar que estas invariantes no se rompan. Si los atributos fuesen de acceso libre (como en un `struct` básico de C), cualquier parte del programa podría asignar un cero al denominador de forma directa. Al ocultar los datos y obligar a que cualquier modificación pase por un método de la interfaz pública, ese método puede evaluar las reglas de negocio y rechazar o corregir cualquier intento de asignar un valor que viole la invariante.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt((this.x * this.x) + (this.y * this.y));
    }
}

/*
Interfaz pública:
-Punto
-Distancia al origen
*/
```

Private: miembros solo accesibles desde el código de la propia clase.

Public: miembros accesibles desde cualquier código de otras clases.

En este ejemplo, la interfaz pública de la clase `Punto` está compuesta únicamente por su constructor (`Punto(double x, double y)`) y por el método `calcularDistanciaAOrigen()`. Estos son los únicos elementos a los que se puede acceder libremente desde otras clases para crear y manipular objetos de este tipo.

El modificador `public` significa que el elemento es accesible desde cualquier otra clase del programa, sin restricciones. Por otro lado, el modificador `private` significa que el elemento está restringido y solo puede ser leído o modificado directamente desde dentro del código de la propia clase donde ha sido declarado, impidiendo su acceso desde el exterior.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

Private: clases internas (no las veremos) y miembros

Public: clases y miembros


Los modificadores de visibilidad `public` y `private` se aplican principalmente a los miembros de una clase: los atributos (las variables que guardan el estado) y los métodos (las funciones que definen el comportamiento). También es muy común y necesario aplicarlos a los constructores, determinando desde dónde se puede instanciar el objeto.

Además de los miembros internos, el modificador `public` se puede aplicar a la declaración de la clase en sí misma (haciéndola accesible desde cualquier paquete del proyecto). Cabe destacar que una clase de nivel superior (la clase principal en un archivo) no puede ser declarada como `private`, pero las clases anidadas (clases definidas dentro de otras clases) sí pueden llevar el modificador `private` para uso exclusivo de la clase contenedora.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Protected: miembros accesibles desde clases.

Sin modificador o "package private": miembros accesibles desde otras clases del mismo paquete.



Sí, existen más tipos de visibilidad diseñados para gestionar la herencia y la organización en módulos. En Java existen cuatro niveles de acceso: público (`public`), privado (`private`), protegido (`protected`) y un nivel por defecto conocido como "package-private". El nivel `protected` permite el acceso dentro de la misma clase, clases del mismo paquete y subclases que heredan de ella. El nivel por defecto (que se aplica cuando no se escribe ningún modificador) restringe el acceso estrictamente a las clases que se encuentran dentro del mismo paquete.

En otros lenguajes de programación orientada a objetos existen variaciones de estos conceptos. En C++, por ejemplo, existen explícitamente `public`, `private` y `protected`. Sin embargo, en lenguajes como Python no existen palabras clave para bloquear el acceso a nivel de compilador; en su lugar, se utilizan convenciones de nomenclatura (como anteponer uno o dos guiones bajos al nombre de la variable, ej. `_variable`) para indicar a los programadores que ese miembro debe tratarse como privado, apelando a la responsabilidad del desarrollador.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

Los miembros de instancia privados están ocultos para **(a) otras clases**. Un error muy común al empezar con la POO es pensar que un objeto no puede acceder a los datos privados de otro objeto distinto. Sin embargo, en lenguajes como Java o C++, el control de acceso se evalúa a nivel de *clase*, no a nivel de *objeto*. Esto significa que un objeto puede acceder libremente a los atributos privados de otro objeto, siempre y cuando ambos sean instancias de la misma clase.

```java
    // Método a añadir dentro de la clase Punto

	//private double x;
	//private double y;
    public double calcularDistanciaAPunto(Punto otro) {
        double difX = this.x - otro.x; // Se accede directamente al atributo privado 'x' del objeto 'otro'
        double difY = this.y - otro.y; // Se accede directamente al atributo privado 'y' del objeto 'otro'
        return Math.sqrt((difX * difX) + (difY * difY));
    }

```

Como se observa en el código, el método pertenece a la clase `Punto` (ejecutándose sobre el objeto `this`), pero recibe como parámetro otro objeto `Punto` llamado `otro`. Dentro del método es totalmente legal escribir `otro.x`, accediendo directamente al atributo privado de esa segunda instancia. Esto es posible y compila correctamente porque el código se encuentra físicamente escrito dentro de la definición de la clase `Punto`, la cual tiene visibilidad total sobre todos sus propios miembros privados, sin importar a qué instancia concreta pertenezcan.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los getter acceden a un dato privado, los setter lo modifican.

Siguiendo con el ejemplo del punto:
```java
double getX() {
	return this.x;
}

void setX(double x) {
	this.x = x
}

double getY() {
	return this.y;
}

void setY(double y) {
	this.y = y;
}
```

No se pueden poner porque sí, ya que modifican la interfaz pública.

Los métodos "getter" (accesores) y "setter" (mutadores) son funciones de la interfaz pública cuyo único propósito es permitir la lectura o escritura, respectivamente, de un atributo privado específico. Por convención, sus nombres se forman con los prefijos `get` o `set` seguidos del nombre del atributo (por ejemplo, `getX()` o `setX(double nuevoValor)`).

Su existencia permite mantener la estricta ocultación de la información (dejando los atributos privados) mientras se ofrece una vía controlada para interactuar con esos datos. La principal ventaja sobre el acceso directo a una variable radica en que, dentro de un método "setter", el programador puede incluir lógica de validación (para mantener las invariantes de clase) o desencadenar acciones adicionales cuando un valor cambia, algo imposible en estructuras de datos simples de lenguajes procedimentales.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, cuando se habla de "seguridad" en el contexto de la encapsulación y la ocultación de información, no se hace referencia a la seguridad informática contra atacantes o hackers. Se trata de una seguridad arquitectónica y de ingeniería de software. Significa que el código es seguro frente a usos accidentales, errores de programación o interferencias imprevistas provocadas por otros desarrolladores (o por uno mismo en el futuro) que utilizan la clase.

El objetivo es prevenir estados inválidos o caídas del programa (bugs) asegurando que el estado interno se modifica únicamente por canales definidos y controlados. De hecho, en lenguajes como Java o C++, un programador con conocimientos avanzados puede evadir fácilmente los modificadores de acceso `private` utilizando técnicas como la "Reflexión" (Reflection) en Java o manipulando punteros de memoria directamente en C++. Por tanto, la visibilidad es una herramienta de diseño para organizar el código, no una medida criptográfica o de ciberseguridad.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un miembro de instancia (ya sea atributo o método) pertenece a un objeto concreto. Cada vez que se crea un nuevo objeto usando `new`, este recibe su propia copia independiente de los atributos de instancia (como la `x` y la `y` de cada objeto `Punto`). Por el contrario, un miembro de clase pertenece a la clase en su totalidad, independientemente de cuántos objetos se hayan instanciado. Es análogo a una variable o función global en C, pero contenida conceptualmente dentro del espacio de nombres de la clase, y su valor es compartido por absolutamente todas las instancias.

Los miembros de clase también se pueden y, por lo general, se deben ocultar aplicando el modificador `private`. Las reglas de encapsulación se aplican exactamente igual: si un miembro de clase guarda un estado global interno que no debe ser manipulado libremente desde fuera (como un contador de objetos creados), debe declararse como privado y exponerse solo mediante métodos de clase públicos si fuera estrictamente necesario.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene mucho sentido y es una práctica común en ciertos patrones de diseño. Cuando un constructor es declarado como `private`, se impide que cualquier otra clase exterior pueda utilizar la palabra clave `new` para instanciar objetos de esa clase.

Esto se utiliza en escenarios como el patrón "Singleton", donde se desea garantizar que solo exista una única instancia de la clase en toda la aplicación; en "clases de utilidad" (como `java.lang.Math`), que agrupan funciones y constantes globales y no tienen sentido ser instanciadas; o cuando se obliga al programador a crear objetos indirectamente a través de métodos "factoría" (Factory Methods) que controlan la inicialización.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican añadiendo la palabra reservada `static` a la declaración del atributo o método. Esto le indica al compilador que ese miembro no está ligado a ningún objeto particular, sino a la definición de la clase misma.

```java
public class Punto {
    private double x;
    private double y;
    
    // Miembros de clase (compartidos)
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    private static void actualizarMaximos(double nuevaX, double nuevaY) {
        if (nuevaX > maxX) maxX = nuevaX;
        if (nuevaY > maxY) maxY = nuevaY;
    }
    
    // Getters públicos estáticos para consultar los máximos
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt((this.x * this.x) + (this.y * this.y));
    }
}

```

En este ejemplo, `maxX` y `maxY` son atributos privados de la clase. Cada vez que se invoca al constructor para crear un nuevo `Punto`, se comprueba si sus coordenadas superan a los máximos históricos registrados en las variables estáticas compartidas. Para consultar estos valores desde el exterior, se proveen métodos de clase públicos (`getMaxX()`), los cuales se invocan sobre el nombre de la clase (por ejemplo, `Punto.getMaxX()`), sin necesidad de tener un objeto creado.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

```java
    public static Punto crearPuntoRedondeado(double x, double y) {
        double xRedondeada = Math.round(x);
        double yRedondeada = Math.round(y);
        return new Punto(xRedondeada, yRedondeada);
    }

```

Sí, se ha utilizado el modificador `static`. Un método factoría debe ser forzosamente un miembro de clase (estático) porque su propósito principal es construir y devolver un nuevo objeto. Si fuera un método de instancia normal, obligaría al programador a tener ya un objeto `Punto` previamente creado para poder invocar al método, lo cual contradice el propósito de usar el método para fabricar el objeto en primer lugar.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

```java
public class Punto {
    // La implementación interna cambia a un array
    private double[] coordenadas;

    // El constructor mantiene su firma pública intacta
    public Punto(double x, double y) {
        this.coordenadas = new double[2];
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
    }

    // El método mantiene su firma y funcionalidad intactas
    public double calcularDistanciaAOrigen() {
        return Math.sqrt((this.coordenadas[0] * this.coordenadas[0]) + 
                         (this.coordenadas[1] * this.coordenadas[1]));
    }
}

```

Este cambio demuestra el inmenso poder de la encapsulación y la ocultación de información. Toda la estructura de datos interna se ha rediseñado (pasando de variables separadas a un array), pero la interfaz pública (`Punto(double, double)` y `calcularDistanciaAOrigen()`) no ha sufrido ninguna modificación desde la perspectiva externa. Cualquier programa, función o módulo que utilizara la versión anterior de la clase `Punto` seguirá funcionando con esta nueva versión sin requerir ni un solo cambio en su código.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque pueda parecer redundante crear métodos para simplemente leer o escribir una variable, declarar el atributo como público rompe la encapsulación. La convención casi absoluta en la programación orientada a objetos es que todos los atributos deben ser privados. Al usar "getters" y "setters", se establece una capa de separación entre el dato y el mundo exterior, lo que permite cambiar el comportamiento interno en el futuro sin modificar los contratos externos.

Esta práctica está íntimamente ligada a las invariantes de clase. Si un atributo es público, es imposible evitar que se le asigne un valor que rompa la lógica del objeto. Sin embargo, si el acceso se realiza a través de un "setter", aunque hoy solo consista en una asignación simple, en el futuro se pueden añadir sin problema condicionales dentro del método para rechazar valores inválidos, lanzar excepciones o recalcular datos derivados, manteniendo siempre el estado del objeto en condiciones válidas.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase inmutable es aquella cuyas instancias no pueden ver modificado su estado (sus datos internos) una vez que el objeto ha sido construido. Todos sus datos se inicializan en el constructor de forma definitiva. Un método modificador es cualquier función en la clase que altera uno o más atributos del estado interno del objeto; por tanto, los "setters" son métodos modificadores por definición, pero no los únicos (por ejemplo, un método `trasladar(double dx, double dy)` también modificaría las coordenadas sin ser un "setter" estricto).

El diseño de clases inmutables presenta notables ventajas. Elimina una gran cantidad de errores relacionados con el paso por referencia (donde una función modifica un objeto de forma inesperada afectando al resto del programa). Además, los objetos inmutables son inherentemente seguros en entornos de programación concurrente (multihilo), ya que múltiples partes del programa pueden leer el objeto simultáneamente sin riesgo de que los datos cambien en medio de la lectura.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, incluir métodos "setter" por defecto para todos los atributos es considerado un antipatrón en el diseño orientado a objetos. Aunque formalmente se mantiene la sintaxis de la encapsulación al tener variables privadas, lógicamente se expone todo el estado del objeto a manipulación externa, lo que destruye gran parte de los beneficios de la ocultación de información.

Lo más recomendable es crear únicamente los métodos "setter" que el comportamiento del dominio del problema exija de manera estricta. Siempre se debe preferir la inmutabilidad o la modificación a través de métodos de negocio con significado lógico (por ejemplo, `retirarFondos(cantidad)` en lugar de `setSaldo(nuevoSaldo)`), reduciendo las vías por las cuales el estado de un objeto puede ser alterado.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

En Java, la clase `String` es estrictamente inmutable. Una vez que se crea un objeto de tipo texto en memoria, su secuencia de caracteres jamás puede cambiar. Por lo tanto, cuando se realiza una operación de concatenación de dos cadenas, el lenguaje crea un tercer objeto `String` completamente nuevo en una ubicación de memoria distinta, el cual contiene el resultado de la unión de las dos cadenas originales.

Si se requiere realizar una operación que implique concatenar texto múltiples veces de forma iterativa (como construir una cadena larga dentro de un bucle `for`), utilizar objetos `String` provocará la creación de cientos de objetos temporales, saturando la memoria y consumiendo ciclos de procesamiento en su recolección. En estos casos, se debe utilizar la clase `StringBuilder` (o `StringBuffer`), las cuales proporcionan una estructura mutable diseñada específicamente para añadir o modificar caracteres de forma eficiente antes de extraer la cadena `String` definitiva.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, la comparación de objetos puede realizarse bajo dos criterios. El primero es por su "identidad" (usando el operador `==`), que verifica si dos variables apuntan exactamente al mismo espacio de memoria (al mismo objeto físico). El segundo es por su "contenido" o valor, que verifica si dos objetos diferentes contienen internamente información equivalente, aunque habiten espacios de memoria distintos.

En Java, la comparación por contenido se delega al método `equals()`. Sin embargo, la implementación por defecto de `equals()` (heredada de la clase base global `Object`) compara estrictamente por identidad, actuando igual que `==`. Para que se pueda comparar por contenido, cada clase debe sobrescribir (redefinir) el método `equals()` especificando qué atributos deben ser comparados para considerar que dos objetos son equivalentes.

En el caso específico de las cadenas de texto (`String`), se debe utilizar absolutamente siempre el método `equals()` para compararlas (ej. `cadena1.equals(cadena2)`), ya que la clase `String` ya tiene implementado internamente el código para comparar carácter por carácter el contenido de ambas variables. Usar `==` con cadenas suele derivar en errores lógicos muy graves y difíciles de detectar.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases "wrapper" (envoltorios) son clases especiales diseñadas para encapsular los tipos de datos primitivos clásicos (como el `int`, `double` o `char` heredados de C) dentro de un objeto real. En Java, cada primitivo tiene su correspondiente clase envoltorio (`Integer` para `int`, `Double` para `double`, etc.), lo que permite tratar números y caracteres básicos como verdaderos objetos dentro del paradigma.

Actualmente, este proceso suele ser automático en lenguajes modernos mediante mecanismos conocidos como "Autoboxing" (convertir un primitivo a su clase wrapper automáticamente) y "Unboxing" (extraer el primitivo del objeto). La ventaja principal es que estas clases wrapper permiten usar números en colecciones y estructuras de datos que requieren estrictamente objetos (como las listas dinámicas `ArrayList`), además de proporcionar métodos estáticos útiles para la conversión de formatos (como transformar una cadena a número).

No todos los lenguajes necesitan clases wrapper. En lenguajes de programación que son orientados a objetos de forma pura (como Ruby o Smalltalk), no existe el concepto de tipo primitivo; todo, hasta un simple número `1`, es inherentemente un objeto desde el principio, por lo que las clases de envoltura no tienen ninguna razón de ser.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo de dato enumerado (`enum`) es una construcción que permite definir un conjunto limitado y predeterminado de valores constantes posibles para una variable, como los días de la semana, las estaciones del año o los estados de un proceso. En lugar de usar números enteros o cadenas mágicas sueltas, se restringe el dominio a opciones fijas.

En Java, un `enum` no es simplemente una etiqueta numérica como en C/C++; es internamente una clase completa y muy potente. Puede poseer sus propios atributos privados, constructores y métodos de comportamiento. Esto eleva considerablemente las ventajas de encapsulación, ya que toda la información y la lógica de negocio vinculada a esa constante se encapsula dentro del propio enumerado, en lugar de estar dispersa en múltiples bloques `switch` a lo largo de todo el programa, garantizando además una absoluta seguridad de tipos en tiempo de compilación.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), 
    ABRIL(30, 4), MAYO(31, 5), JUNIO(30, 6), 
    JULIO(31, 7), AGOSTO(31, 8), SEPTIEMBRE(30, 9), 
    OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
        return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
        return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        if (enHemisferioNorte) return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
        return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
    }
}

```

En este código se puede observar cómo el tipo enumerado actúa como una clase robusta. Cada instancia (desde `ENERO` hasta `DICIEMBRE`) llama implícitamente al constructor privado con sus datos característicos. Los atributos internos han sido declarados como `private final`, lo que garantiza que una vez creado el mes, nadie pueda alterar sus características (inmutabilidad).

Los métodos relativos a las estaciones utilizan la propia identidad del objeto (`this`) para evaluar la lógica en función del parámetro provisto para el hemisferio. Cabe destacar que, al haber meses de transición donde se produce el solsticio o equinoccio (como marzo, junio, septiembre y diciembre), se ha considerado de forma lógica que dichos meses abarcan días de ambas estaciones concurrentes para devolver el valor `true` correspondientemente.

---

Para continuar de forma interactiva, ¿desea que se le proporcionen algunos ejercicios prácticos con código para afianzar estos conceptos de clases, interfaces y envoltorios en Java?