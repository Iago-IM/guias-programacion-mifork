<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
A continuación se presentan las respuestas solicitadas. Se mantienen un nivel acorde a conocimientos previos en C/C++ estructurado y Java básico (clases, encapsulación, excepciones y composición), y se redactan en forma impersonal.

***

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"? Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.


En orientación a objetos, la **herencia** es un mecanismo mediante el cual una clase (subclase) reutiliza y extiende el estado y el comportamiento definidos por otra clase (superclase). Conceptualmente, la herencia modela una relación **“A es-un B”**, lo que significa que un objeto de la subclase puede ser tratado como un objeto de la superclase. Por ejemplo, si `Artillero` hereda de `Soldado`, se puede afirmar que *un artillero es un soldado*.

La primera implicación importante es la **compatibilidad de tipos**. Esto permite que una referencia del tipo de la superclase (`Soldado`) apunte a objetos de cualquiera de sus subclases (`Artillero`, `Zapador`). Gracias a esto, se pueden escribir estructuras de código genéricas (arrays, métodos) que trabajen con el tipo base sin conocer los subtipos concretos, facilitando la reutilización y extensibilidad del programa.

La segunda implicación es la **herencia de estado y comportamiento**. Las subclases heredan los atributos y métodos accesibles (no privados de forma directa) de la superclase. En este caso, tanto `Artillero` como `Zapador` heredan el nombre del soldado y la capacidad de saludar, añadiendo además su propio estado y comportamiento específico.

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

Soldado[] ejercito = {
    new Artillero("Carlos", 10),
    new Zapador("Luis", 5),
    new Artillero("Ana", 7)
};

for (Soldado s : ejercito) {
    s.saludar();
}
```

***

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden?

Al crear una instancia de una subclase, se ejecutan **todos los constructores de la cadena de herencia**, comenzando por el de la superclase y continuando hacia abajo hasta llegar al de la subclase concreta. Por ejemplo, al crear un `Artillero`, primero se ejecuta el constructor de `Soldado` y después el de `Artillero`. Este orden garantiza que el estado heredado quede correctamente inicializado antes de añadir el estado específico.

La palabra clave `super` dentro de un constructor se utiliza para llamar explícitamente al constructor de la superclase. Esta llamada debe ser la primera instrucción del constructor de la subclase. Su función principal es inicializar la parte del objeto que corresponde a la superclase, especialmente cuando dicha superclase necesita parámetros para construirse.

Si la clase base **no tiene un constructor sin parámetros visible**, entonces es obligatorio llamar a `super` de forma explícita, pasando los argumentos adecuados. De no hacerse, el programa no compilará. En cambio, si existe un constructor sin parámetros accesible, Java inserta una llamada implícita a `super()`.

***

## 3. Atributos privados de la superclase y memoria

Desde el punto de vista de la memoria, **los atributos privados de la superclase sí forman parte de la instancia completa de la subclase**. Un objeto `Artillero` contiene en memoria tanto los atributos definidos en `Artillero` como los definidos en `Soldado`, incluyendo los privados. No existe una “separación física” de objetos; todo forma una única instancia.

Sin embargo, el hecho de que esos atributos existan en memoria **no implica que puedan usarse directamente desde el código de la subclase**. En Java, `private` significa acceso exclusivo desde la propia clase que declara el atributo. Por ello, `Artillero` no puede acceder directamente a `nombre` si este es privado en `Soldado`.

En el ejemplo, aunque `Artillero` hereda el estado `nombre`, solo puede interactuar con él mediante métodos públicos o protegidos definidos en `Soldado`, como `saludar()`. Esto permite mantener la encapsulación incluso en presencia de herencia.

***

## 4. Compatibilidad de tipos y extensibilidad

La compatibilidad de tipos entre superclases y subclases tiene un impacto directo en la **extensibilidad del código**. Permite añadir nuevas subclases sin modificar el código existente que opera sobre el tipo base. Esto es una aplicación directa del principio *abierto/cerrado*: el código está abierto a extensión, pero cerrado a modificación.

Cuando se escribe código que trabaja con `Soldado` en lugar de con tipos concretos, ese código no necesita cambiar al aparecer un nuevo tipo de soldado. Basta con que el nuevo tipo herede de `Soldado` y respete su contrato (métodos públicos).

Por ejemplo, se puede añadir un nuevo subtipo `Medico` y el bucle que pide el saludo a todos los soldados seguirá funcionando sin tocarlo.

```java
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}
```

El array de `Soldado` y el recorrido que invoca `saludar()` permanecen exactamente iguales, demostrando la extensibilidad lograda gracias a la herencia.

***

## 5. Referencias, upcasting y downcasting

En Java, es totalmente válido que una **referencia del supertipo apunte a un objeto real de un subtipo**. Esto es precisamente lo que permite el polimorfismo. Por ejemplo, una referencia `Soldado` puede apuntar a un `Artillero` o a un `Zapador`.

Con una referencia del supertipo solo se pueden invocar métodos que estén declarados en ese supertipo. Aunque el objeto real sea un `Artillero`, no es posible llamar directamente a `getCohetes()` usando una referencia `Soldado`. Esto protege el código de depender de detalles concretos del subtipo.

El **upcasting** consiste en tratar un objeto de una subclase como si fuera uno de la superclase; es seguro y suele ser implícito. El **downcasting** es el proceso inverso: convertir una referencia del supertipo en una del subtipo concreto; es potencialmente peligroso y requiere comprobación. Para ello se utiliza `instanceof`.

```java
for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

***

## 6. Acceso protegido y herencia

El acceso **protegido (`protected`)** se sitúa a medio camino entre `private` y `public`. Un atributo o método protegido es accesible desde la propia clase, desde sus subclases y desde otras clases del mismo paquete. Su objetivo principal es permitir la reutilización controlada en un contexto de herencia.

En Java, se implementa usando la palabra clave `protected`. Esto es especialmente útil cuando una subclase necesita acceder a parte del estado interno de la superclase para implementar su lógica específica, sin exponer dicho estado al resto del programa.

Por ejemplo, si el nombre del soldado es protegido, el `Zapador` puede usarlo directamente en su método para poner bombas:

```java
class Soldado {
    protected String nombre;
    // ...
}

class Zapador extends Soldado {
    public void ponerMina() {
        System.out.println(nombre + " ha puesto una mina");
    }
}
```

***

## 7. Clase base común para todos los objetos

En muchos lenguajes orientados a objetos existe la noción de una **clase base común** para todos los objetos, que define comportamiento mínimo compartido. Sin embargo, esto no ocurre de forma uniforme en todos los lenguajes: algunos no lo imponen, otros lo hacen de manera implícita.

En Java, **todas las clases heredan directa o indirectamente de `java.lang.Object`**, incluso si no se indica explícitamente. Esta clase proporciona métodos fundamentales como `toString()`, `equals()` o `hashCode()`.

Esto permite que cualquier objeto en Java pueda ser tratado de forma genérica como `Object`, facilitando mecanismos como colecciones genéricas, reflexión y manipulación uniforme de objetos.

***

## 8. Herencia múltiple

La **herencia múltiple** es la capacidad de una clase para heredar directamente de más de una superclase. Esto permite combinar comportamientos y estados de varias jerarquías, pero introduce problemas de ambigüedad, como el famoso *problema del diamante*.

En Java **no existe herencia múltiple de clases**. Una clase solo puede extender de una única superclase concreta. Esta decisión simplifica el modelo de herencia y evita conflictos en la resolución de métodos y atributos.

No obstante, Java permite una forma limitada de herencia múltiple mediante **interfaces**, que definen contratos de métodos sin estado (o con estado muy restringido), combinando flexibilidad y seguridad.

***

## 9. Excepciones personalizadas no controladas

En Java, las excepciones son objetos y pueden formar parte de una jerarquía de herencia. Una excepción *no controlada* se consigue heredando de `RuntimeException`. Esto evita la obligación de declararla o capturarla explícitamente.

Es posible además **componer la excepción con otros objetos**, como un `Usuario`, para aportar contexto adicional sobre el error ocurrido. Asimismo, sobrecargar constructores permite incluir una causa subyacente (`Throwable cause`).

```java
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super(cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

***

## 10. Herencia vs. reutilización de código

No se recomienda usar herencia únicamente para reutilizar código porque la herencia establece una **relación semántica fuerte**. Decir que “A hereda de B” implica que *A es un B*, no solo que reutiliza su implementación. Si esta relación no es conceptualmente correcta, el diseño se vuelve confuso y frágil.

Además, la herencia crea un acoplamiento estrecho entre superclase y subclase. Cambios en la superclase pueden afectar de forma imprevista a todas las subclases, incluso aunque estas no necesiten dichos cambios.

Por ello, si el objetivo es solo compartir código sin una relación clara “es-un”, la herencia no suele ser la opción más adecuada.

***

## 11. Favorecer la composición frente a la herencia

Favorecer la **composición** frente a la herencia significa diseñar clases que **contienen** otras clases, en lugar de **ser** otras clases. La composición establece relaciones “tiene-un”, que son más flexibles y menos acopladas.

Con composición, los cambios en la clase reutilizada no afectan directamente a la jerarquía de tipos. Además, permite cambiar dinámicamente el comportamiento sustituyendo los objetos compuestos sin modificar la estructura de herencia.

Este enfoque conduce a diseños más modulares, fáciles de mantener y de extender, especialmente en sistemas grandes o en evolución constante.

***

## 12. “La herencia rompe la encapsulación”

Se dice que la herencia rompe la encapsulación porque las subclases **dependen de detalles internos de la superclase**, aunque no sean públicos. Incluso con `protected`, la superclase expone parte de su implementación interna a las subclases.

Esto implica que cambios internos en la superclase, aunque no afecten a su interfaz pública, pueden romper el comportamiento de las subclases. En otras palabras, la subclase queda acoplada no solo al qué hace la superclase, sino a cómo lo hace.

La composición, en cambio, interactúa principalmente a través de interfaces públicas, preservando mejor los límites de encapsulación.

***

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Una primera forma de modelar `Estudiante` y `Trabajador` es mediante herencia, usando una superclase `Persona` que contiene el DNI y el nombre. Ambas clases extienden de ella y reutilizan directamente esos atributos.

```java
class Persona {
    protected String dni;
    protected String nombre;
}

class Estudiante extends Persona {
}

class Trabajador extends Persona {
}
```

Una segunda alternativa es usar **composición**, creando una clase `DatosPersonales` y haciendo que `Estudiante` y `Trabajador` la contengan. En este caso, no se establece una relación “es-un”, sino “tiene-un”.

```java
class DatosPersonales {
    private String dni;
    private String nombre;
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

Ambos modelos son válidos, pero la elección depende de si conceptualmente `Estudiante` y `Trabajador` **son** personas, o simplemente **tienen** datos personales y podrían evolucionar de forma independiente.

