<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El objetivo es extender programas de forma más sencilla y segura, por el que las modificaciones vienen principalmente añadiendo nuevas clases, antes que tocando el código de las clases existentes.

En Java se logra con:
    - Sobrescritura de métodos en clases abstractas
    - Clases abstractas (con sus métodos abstractos)
    - Interfaces


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
Con la ligadura dinámica no se especifica el método al que se hace referencia hasta el tiempo de ejecución. En Java pasa siempre excepto en los métodos estáticos.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
```java
public class Main {

    public static void main(String[] args) {

        Soldado[] soldados = new Soldado[2];
        soldados[0] = new Zapador("Luis");
        soldados[1] = new Artillero("Carlos");

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}

// ----------------- Clase base -----------------

class Soldado {

    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println(
            "Soy el soldado " + nombre + " y saludo reglamentariamente."
        );
    }
}

// ----------------- Subclase Zapador -----------------

class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        System.out.println(
            "Soy el zapador " + nombre + " y me encargo de las explosiones."
        );
    }
}

// ----------------- Subclase Artillero -----------------

class Artillero extends Soldado {

    public Artillero(String nombre) {
        super(nombre);
    }
}
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, empleando la referencia super. En este contexto sería super.saludar().

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
- Al sobreescribir un método (overriding) en Java, la subclase debe usar **exactamente los mismos tipos y orden de parámetros** que el método de la superclase, y el **tipo de retorno debe ser el mismo o un subtipo compatible (retorno covariante)**.
Además, no se puede reducir la visibilidad del método ni lanzar excepciones más generales que las originales.

- La **sobrecarga (overloading)**, en cambio, consiste en definir varios métodos con el mismo nombre pero **distinta lista de parámetros** dentro de una misma clase (o por herencia), y se resuelve en **tiempo de compilación**, mientras que la sobrescritura se decide en **tiempo de ejecución** y es la base del polimorfismo.
La anotación **@Override** sirve para indicar explícitamente que un método pretende sobrescribir otro de la superclase, y es recomendable usarla siempre porque el compilador puede detectar errores (por ejemplo, un método mal escrito o con firma incorrecta) que de otro modo pasarían desapercibidos y romperían el polimorfismo.



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, ya que sobreescribimos un método.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta no se puede instanciar (sí se instancian las clases que heredan de esa clase).

Un método abstracto es un método sin implementar (solo está la firma, por lo que las clases que lo hereden deben implementar el método).

```java
abstract class Soldado {

    public void saludar() {
        System.out.println("Soy un soldado.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

class Artillero extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
En clases `final` prohibe la herencia. En los métodos prohibe su sobreescritura.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
- Las interfaces NO tienen atributos.
- Todos los métodos de las interfaces son abstractos.
- Una clase puede implementar varias interfaces.

A partir de Java 8 se permite dar código en los métodos de la interfaz empleando la palabra clave default.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
```java
public class Main {

    public static void main(String[] args) {

        Punto p1 = new Punto2D(0, 0);
        Punto p2 = new Punto2D(3, 4);

        Linea linea2D = new Linea(p1, p2);
        System.out.println("Longitud línea 2D: " + linea2D.longitud());

        Punto q1 = new Punto3D(0, 0, 0);
        Punto q2 = new Punto3D(1, 2, 2);

        Linea linea3D = new Linea(q1, q2);
        System.out.println("Longitud línea 3D: " + linea3D.longitud());
    }
}

// --------------------------------------------------
// Clase abstracta Punto
// --------------------------------------------------

abstract class Punto {

    public abstract double calcularDistanciaA(Punto otro);
}

// --------------------------------------------------
// Punto en 2D
// --------------------------------------------------

class Punto2D extends Punto {

    private double x;
    private double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre Punto2D y otro tipo"
            );
        }

        Punto2D p = (Punto2D) otro; // downcasting

        double dx = this.x - p.x;
        double dy = this.y - p.y;

        return Math.sqrt(dx * dx + dy * dy);
    }
}

// --------------------------------------------------
// Punto en 3D
// --------------------------------------------------

class Punto3D extends Punto {

    private double x;
    private double y;
    private double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
