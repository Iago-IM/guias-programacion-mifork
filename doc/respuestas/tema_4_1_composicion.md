<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición

# 1. En C, podemos crear estructuras mayores componiendo unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se expresa incluyendo una estructura dentro de otra. En el caso de una línea formada por dos puntos, basta con que la estructura `Linea` contenga dos instancias de `Punto`, lo que refleja la relación “una línea tiene dos puntos”. Este mecanismo está disponible incluso sin orientación a objetos, ya que C permite agrupar datos mediante `struct`.

Para operar sobre estas estructuras pueden escribirse funciones externas. La distancia entre puntos se calcula a partir de sus coordenadas, y la longitud de la línea simplemente utiliza estos puntos internos. Este estilo de composición es directo, aunque no cuenta con las protecciones de encapsulación que sí están presentes en lenguajes orientados a objetos.

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}
```

***

# 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de composición en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

### Respuesta

En Java, la composición se implementa mediante atributos privados que representan instancias de otras clases. En este ejemplo, `Punto` se define como una clase inmutable cuyas coordenadas solo pueden asignarse en el constructor, evitando cualquier modificación posterior. Gracias a la encapsulación, se mantiene la integridad del estado interno sin necesidad de disciplina por parte del programador.

La clase `Linea` también es inmutable y contiene dos puntos, lo que refleja directamente la composición. Su longitud se obtiene delegando la operación en el método de distancia de `Punto`, manteniendo así un diseño limpio y modular. Esto permite representar relaciones “tiene-un” con seguridad y claridad, superando las limitaciones estructurales del lenguaje C.

```java
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x; 
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

***

# 3. ¿Qué significa la multiplicidad en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad indica cuántas instancias de un tipo se asocian con cuántas instancias de otro. Es una manera formal de describir relaciones estructurales entre clases en un modelo orientado a objetos. Gracias a la multiplicidad pueden expresarse restricciones como “uno”, “muchos”, “exactamente dos” o “cero o más”.

En el caso de `Linea` y `Punto`, una línea está constituida por exactamente dos puntos, por lo que la multiplicidad en la dirección `Linea → Punto` es **2**. En sentido contrario, un punto puede no pertenecer a ninguna línea o puede formar parte de varias, por lo que `Punto → Linea` tiene multiplicidad **0..**\*.

***

# 4. ¿Qué significa composición fuerte y composición débil? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como "asociación o agregación" y a cuál como "composición" propiamente.

### Respuesta

La composición débil, comúnmente llamada agregación, indica que un objeto contiene referencias a otros sin controlar su ciclo de vida. El objeto contenido puede existir de manera independiente fuera del contenedor y su destrucción no está ligada a la del objeto que lo agrupa. Este tipo de relación se usa cuando el vínculo entre las clases no implica dependencia total.

La composición fuerte, conocida simplemente como composición, indica una dependencia total: el ciclo de vida del objeto contenido está sujeto al del contenedor. Si el contenedor deja de existir, el contenido debe considerarse destruido también. Esto expresa una relación estructural más profunda y más restrictiva que la mera agregación.

***

# 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de "dependencia"?

### Respuesta

Cuando una clase utiliza otra únicamente como parámetro o valor de retorno de un método, o la crea de forma temporal dentro de un ámbito local, se habla de **dependencia**, no de composición. Esto significa que la relación no forma parte del estado interno del objeto y existe solo durante la ejecución de los métodos.

Este tipo de relación describe colaboración puntual entre clases. La composición, en cambio, implica almacenamiento permanente en los atributos de la clase, formando parte de su estructura y de su identidad. Por tanto, no toda interacción entre objetos constituye composición.

***

# 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una como composición fuerte, donde el ciclo de vida de los puntos está ligado al de Línea y otra como composición débil, donde no.

### Respuesta

En la composición fuerte, la línea crea internamente sus propios puntos, evitando que provengan del exterior. Esto implica que los puntos existen únicamente dentro de la línea y no tienen sentido fuera de ella. Su ciclo vital queda totalmente vinculado al ciclo de vida del objeto `Linea`.

En la composición débil, la línea recibe los puntos desde fuera y solo guarda referencias a ellos. Esto permite que los puntos existan de forma independiente y puedan usarse en más de una estructura. Su ciclo vital no depende del de la línea, lo que refleja una relación más flexible.

**Composición fuerte:**

```java
final class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}
```

**Composición débil:**

```java
final class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}
```

***

# 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, la destrucción explícita de objetos no existe porque el lenguaje confía en un recolector de basura para gestionar la memoria. Este mecanismo elimina automáticamente los objetos que ya no son accesibles desde ningún punto del programa, liberando los recursos asociados sin intervención del programador.

Por ello no aparece ningún código donde `Linea` destruya manualmente sus puntos, incluso en composición fuerte. Una vez que la línea deja de existir o no es accesible, también dejan de serlo sus puntos. El recolector se encarga del resto, respetando así el modelo de memoria gestionada característico de Java.

***

# 8. Pon un ejemplo de composición débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java con máximo 50, pero no rompas la encapsulación: permite añadir y eliminar profesores y obtenerlos sin exponer el array.

### Respuesta

En una composición débil, el departamento almacena profesores pero no controla su ciclo de vida. Los profesores pueden existir fuera del departamento y ser compartidos entre distintas estructuras. Sin embargo, se mantiene una invariante importante: el director debe ser siempre un profesor incluido en la lista, y nunca es posible eliminar al director sin antes sustituirlo. Cualquier intento de romper esta regla debe conducir a una excepción.

El uso de arrays obliga a gestionar manualmente el número de profesores y su movimiento interno. La encapsulación se preserva ofreciendo métodos seguros para añadir, consultar y eliminar profesores sin devolver el array real. Asimismo, la comprobación de la invariante debe realizarse en cada operación sensible para garantizar la coherencia del modelo.

```java
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }
}

class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int num = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores[num++] = directorInicial;
        director = directorInicial;
    }

    public int getNumProfesores() {
        return num;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= num) throw new IllegalArgumentException();
        return profesores[pos];
    }

    public void addProfesor(Profesor p) {
        if (num == 50) throw new IllegalStateException();
        profesores[num++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= num) throw new IllegalArgumentException();
        if (profesores[pos] == director)
            throw new IllegalStateException("No se puede eliminar al director");
        profesores[pos] = profesores[num - 1];
        profesores[num - 1] = null;
        num--;
    }

    public void setDirector(Profesor p) {
        boolean encontrado = false;
        for (int i = 0; i < num; i++) {
            if (profesores[i] == p) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) throw new IllegalArgumentException("Debe ser profesor del departamento");
        director = p;
    }
}
```

***

# 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Al usar `List`, desaparecen gran parte de las tareas manuales: ya no es necesario controlar el tamaño máximo, mover elementos al eliminar, ni gestionar posiciones vacías. La colección ofrece métodos listos para añadir, eliminar y consultar profesores, lo que simplifica considerablemente la implementación. Esto se traduce en un código más conciso y menos susceptible a errores.

Sin embargo, si se devolviera directamente la lista interna, se rompería la encapsulación, ya que el cliente podría modificarla sin restricciones, cambiando el estado interno del departamento. Para evitarlo se puede devolver una copia o una vista inmodificable con `Collections.unmodifiableList()`. Este enfoque preserva la integridad interna del objeto y evita modificaciones externas no controladas.

```java
class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores.add(directorInicial);
        director = directorInicial;
    }

    public int getNumProfesores() { return profesores.size(); }

    public Profesor getProfesor(int pos) { return profesores.get(pos); }

    public void addProfesor(Profesor p) { profesores.add(p); }

    public void removeProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
    }

    public void setDirector(Profesor p) {
        if (!profesores.contains(p))
            throw new IllegalArgumentException("Debe ser profesor del departamento");
        director = p;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```

***

# 10. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Las excepciones recursivas son un ejemplo de composición recursiva (una excepción tiene una causa, que a su vez es otra excepción con otra causa...)

Una composición recursiva ocurre cuando una clase contiene elementos de su mismo tipo, permitiendo construir estructuras jerárquicas como árboles familiares. La inmutabilidad ayuda a mantener la consistencia del modelo, ya que una vez creada la estructura, no puede modificarse de forma accidental. Así se evita la introducción de ciclos extraños o inconsistencias en la genealogía.

Este tipo de composición aparece también en estructuras habituales como árboles de directorios, expresiones matemáticas representadas como nodos o listas enlazadas. En todos estos casos existe un elemento que contiene referencias a elementos de su propio tipo, formando cadenas o árboles. Es una técnica fundamental para modelar información jerárquica.

```java
final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Bea", abuela);
        Persona hijo = new Persona("Carlos", madre);

        System.out.println(hijo.getNombre() + ", madre: " +
            hijo.getMadre().getNombre() + ", abuela: " +
            hijo.getMadre().getMadre().getNombre());
    }
}
```

***

# 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una relación de composición bidireccional ocurre cuando ambos objetos participantes mantienen referencias mutuas, es decir, cada uno conoce al otro. Esto obliga a garantizar la coherencia en ambas direcciones, y requiere un diseño cuidadoso para evitar estados incoherentes o referencias que no se correspondan entre sí. Además, deben definirse métodos que aseguren la actualización simultánea de ambas partes.

En el ejemplo de `Profesor` y `Departamento`, habría que añadir un atributo `Departamento` en la clase `Profesor`. Los métodos del departamento que añaden o eliminan profesores deberían actualizar también la referencia inversa en `Profesor`. De igual modo, si un profesor cambiara de departamento, debería notificarse al departamento anterior para eliminarlo de su lista. Este tipo de relación exige coordinación bidireccional para mantenerse consistente.

Las composiciones bidireccionales exigen tener cuidado para mantener la consistencia. Soluciones:
- Que cada método modificador llame al otro (cuidando los ciclos para evitar un bucle infinito)
- Solo dejar visible a terceros uno de los métodos modificadores.

***
