<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

**Objetivo:** La función `raiz` **no imprime** nada; solo **indica** el error. El programa **llamador** es quien informa.

#### Opción A: **Valor centinela + `errno`** (estándar C)

*   La función devuelve `NAN` y pone `errno = EDOM` (“argumento fuera de dominio”).
*   El llamador **comprueba `errno`** y/o `isnan()` y decide qué hacer.

```c
#include <stdio.h>
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0.0) {
        errno = EDOM;        // Error de dominio matemático
        return NAN;          // Valor centinela
    }
    errno = 0;               // Limpia errno si todo va bien
    return sqrt(x);
}

int main(void) {
    double x = -9.0;
    errno = 0;
    double r = raiz(x);

    if (errno == EDOM || isnan(r)) {
        fprintf(stderr, "Error: no existe la raiz de %.2f en los reales.\n", x);
    } else {
        printf("raiz(%.2f) = %.4f\n", x, r);
    }
    return 0;
}
```

**Ventajas:** Simple y portable.  
**Inconvenientes:** `errno` es **global** (y potencialmente frágil), `NAN` puede confundirse si el dominio admite NaN.

#### Opción B: **Código de retorno (status) + parámetro de salida**

*   La función devuelve `int` (0 = OK, ≠0 = error).
*   El resultado real va por **parámetro de salida** (`double* out`).

```c
#include <stdio.h>
#include <math.h>

int raiz(double x, double* out) {
    if (x < 0.0) {
        return -1;           // Código de error
    }
    *out = sqrt(x);
    return 0;                // Éxito
}

int main(void) {
    double x = -9.0, r = 0.0;
    int status = raiz(x, &r);

    if (status != 0) {
        fprintf(stderr, "Error: no existe la raiz de %.2f en los reales.\n", x);
    } else {
        printf("raiz(%.2f) = %.4f\n", x, r);
    }
    return 0;
}
```

**Ventajas:** No usa globales; no confunde con `NAN`.  
**Inconvenientes:** Más verbosidad y manejo explícito de códigos.

***

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

**Resumen**: 
* Excepciones -> en situaciones atípicas
* Cuando las implementamos -> nos permite indicar más claramente el error.
* Cuando las llamamos -> facilita la lógica normal de la reacción o manejo de las situaciones complejas.

Una **excepción** es un objeto/evento que **interrumpe** el flujo normal cuando ocurre una condición anómala (error) y **transfiere el control** a un manejador (`catch`).  

**Objetivos:**
*   Separar la **lógica normal** de la **lógica de error**.
*   Permitir **propagación automática** hasta un lugar donde tenga sentido gestionar el error.
*   Facilitar **limpieza de recursos** (con `finally` o equivalentes) y aportar **información rica** (tipo, mensaje, causa, pila).

***

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

```java
public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No existe la raíz real de " + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        double x = -9.0;

        try {
            double r = Calculadora.raiz(x);
            System.out.printf("raiz(%.2f) = %.4f%n", x, r);
        } catch (IllegalArgumentException e) {
            System.err.println("Error: " + e.getMessage());
        }

        System.out.println("El programa continúa...");
    }
}
```

***

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

*   **Lanzar** = ejecutar `throw` para crear y emitir una excepción (`throw new ...`).
*   **Capturar/Controlar** = rodear con `try` y manejar en `catch`.
*   **Propagar** = si un método **no la captura**, la excepción **sube** por la pila buscando un `catch`.  
    Durante la propagación:
    *   Se **desenrolla la pila** (se “salen” frames).
    *   Se ejecutan los **`finally`** encontrados.
    *   Las funciones que no capturan **no se reanudan** después de la excepción; el control no vuelve a la instrucción siguiente.

**Ejemplo:**

```java
static double raiz(double x) {
    if (x < 0) throw new IllegalArgumentException("No existe la raíz de " + x);
    return Math.sqrt(x);
}

static void capaIntermedia(double x) {
    // No captura; si hay error, la excepción se propaga
    double r = raiz(x);
    System.out.println("Resultado: " + r); // No se ejecutará si hubo excepción
}

public static void main(String[] args) {
    try {
        capaIntermedia(-9);
        System.out.println("Esto no se imprime si hay excepción arriba");
    } catch (IllegalArgumentException e) {
        System.err.println("Manejada en main: " + e.getMessage());
    }
    System.out.println("Tras el catch, el programa sigue.");
}
```

***

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

*   Menos **código de pegamento** (no hay que pasar y comprobar códigos en cada salto).
*   **Separación de preocupaciones**: el que sabe manejar el error lo hace; los demás no se ensucian.
*   **Información rica** (tipo, mensaje, **pila**, **causa**).
*   **Limpieza garantizada** con `finally`/try-with-resources durante el desenrollado.
*   **Mantenimiento** y **legibilidad** mejores.

***

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Sí: en Java heredan de `Throwable`.  
**Ventajas (encapsulación):** pueden llevar **mensaje**, **causa**, **datos extra**, tipo específico; se pueden **organizar por jerarquías**.  
Sí, podemos crear **excepciones personalizadas**:

```java
class RaizNegativaException extends IllegalArgumentException {
    public RaizNegativaException(double x) {
        super("No existe la raíz real de " + x);
    }
}
```

***

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Cualquier objeto excepción lleva:
1) Un mensaje
2) La traza de pila (muy útil para depurar)
3) Opcionalmente, una "causa", que es otra excepción


*   **Tipo** (clase): naturaleza del problema.
*   **Mensaje** descriptivo.
*   **Causa** encadenada (otra excepción que originó esta).
*   **Stack trace** (ruta exacta por el código hasta el fallo).
*   Opcional: **datos de contexto** (campos propios en personalizadas).

En C solemos tener **códigos** (`errno`) y menos contexto; en Java llega al manejador **todo** lo necesario.

***

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

*   Puede haber más de un catch
*   Solo se ejecuta UNO:
    +   Se va comprobando por orden de declaración del cath --> IMPORTA EL ORDEN
    +   Orden obligatorio: **más específico → más general**.
    +   El primero que encaje es el que se ejecuta

*   Existe el **multi-catch**: `catch (IOException | SQLException ex)`.


Nota: las excepciones más específicas son un tipo de las más generales

***

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

El `finally` se ejecuta SIEMPRE que se haya entrado en el try (haya excepciones o no)

```java
// Con catch
try {
    usarRecurso();
} catch (IllegalArgumentException e) {
    System.err.println("Error de argumentos: " + e.getMessage());
} finally {
    liberarRecurso(); // Se ejecuta siempre (salvo casos extremos)
}

// Sin catch (se sigue propagando)
try {
    usarRecurso();
} finally {
    liberarRecurso(); // Se ejecuta, y luego se propaga la excepción
}
```

***

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

*   Sí, `try` puede ir con **solo `finally`** (sin `catch`).
*   `finally` se ejecuta **tanto si hay** como si **no hay** excepción e **incluso si hay `return`** dentro del `try`.
*   No se ejecuta en casos **extremos** (p. ej., `System.exit()`, apagado abrupto, error fatal del JVM).

```java
static int demo() {
    try {
        return 42; // finally aún se ejecutará
    } finally {
        System.out.println("Siempre paso por aquí antes de devolver.");
    }
}
```

***

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

La gestión de las excepciones es diferente dependiendo del lenguaje, pero la clasificación en el fondo es siempre la misma. En Java:

* No controladas (unchecked): típicamente errores de programación que una vez solventados no vuelven a ocurrir. **No** estamos obligados a controlarlas con try-catch/throws.

* Excepciones controladas (checked): típicamente errores por causas externas y que siempre pueden llegar a ocurrir (que no dependen de nosotros) como, por ejemplo, errores de entrada/salida. **Estamos obligados** a controlarlas con try-catch/throws


**Ejemplos típicos:**

*   **Checked:** `IOException`, `SQLException`, `FileNotFoundException`.
*   **Unchecked:** `IllegalArgumentException`, `NullPointerException`, `IndexOutOfBoundsException`, `ArithmeticException`.

Esquema con algunas excepciones:
*   **RuntimeException (No controladas)**:
    +   IlegalArgumentException
    +   NullPointerException
    +   ArrayIndexOutOfBoundsException
*   **IOException (Controladas)**:
    +   AccessDeniedException

**Preferir controladas cuando:**

1.  Fallos de **E/S** o **red** que el llamador puede **reintentar** o **informar** (archivo no encontrado, timeout).
2.  Acceso a **BD** donde el llamador decide **transacción alternativa**.
3.  **Carga de configuración** externa que el usuario puede **corregir**.
4.  **Protocolos** remotos con **timeouts** recuperables.

**Preferir no controladas cuando:**

1.  **Precondiciones inválidas** del API (argumentos fuera de rango).
2.  **Bugs lógicos**: NPE, índice fuera de rango.
3.  **Estados imposibles** por contrato: `IllegalStateException`.
4.  Fallos que **no** deben intentarse recuperar localmente.


***

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

Cuando tenemos una excepción controlada tenemos dos opciones, manejar la excepción con un try catch o desentendernos del error con el throw mandándolo a quién llame la función (el llamador). Si manejamos la excepción y la función devuelve un valor (por ejemplo, un string en una función para leer un archivo) debemos usar el finally.

`throws` en la **firma** declara que un método **puede lanzar** ciertas **checked exceptions**.  
Sirve para **delegar** la gestión al **llamador**, que decidirá capturar o volver a declarar. Es una alternativa a capturar localmente y forma parte del **contrato** del método.

***

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa manejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

REVISAR: Cuando editamos un método público, añadir un throws puede romper la retrocompatibilidad por lo que podemos no poner el catch en la función y así delegar el error.

```java
import java.io.*;

public class Lector {

    // Declaramos que no manejamos aquí: delegamos (checked)
    public static String leePrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // puede lanzar FileNotFoundException
            return br.readLine();                           // puede lanzar IOException
        } finally {
            if (br != null) {
                try { br.close(); } 
                catch (IOException e) { /* registrar y continuar */ }
            }
        }
    }

    public static void main(String[] args) {
        try {
            String linea = leePrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            System.err.println("No se pudo leer el archivo: " + e.getMessage());
        }
    }
}
```

> Nota: **try-with-resources** (`try (BufferedReader br = ... ) { ... }`) es preferible hoy; cierra automáticamente sin `finally`.

***

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Se hace por propósitos de documentación, pero no es lo habitual.

*   **Se puede** declarar `RuntimeException` en `throws`, pero **no es necesario**.
*   El llamador **no está obligado** a capturarla; lo hará **solo** si quiere **interceptar** para registrar, transformar o continuar.
*   Tiene **sentido documental**: explicitar en la API que un método puede lanzar p. ej. `IllegalArgumentException` o `UnsupportedOperationException`.

***

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

No hay ambas opciones en todos los lenguajes. La más típica es la de "no controladas" (Rust, por ejemplo, tiene `panic` para no controladas y emplea un tipo de retorno especial para controlar excepciones controladas). Para Java las excepciones son situaciones atípicas que siempre pueden ocurrir. Para Rust, en cambio, la filosofía es radicalmente opuesta.

*   **Usa checked** para condiciones **externas y recuperables**: E/S, red, permisos, recursos externos.
*   **Usa unchecked** para **errores del programador** o **violaciones de contrato**: argumentos inválidos, estado ilegal.

**Lenguajes:**

*   **Java**: checked y unchecked.
*   **C#**, **C++**, **Python**, **Kotlin**, **Scala**: **todas** son unchecked (no verificación en compilación).
*   **Go**/**Rust**: no usan excepciones al estilo Java; emplean valores `error`/`Result`.

Donde solo hay una opción, lo habitual es el modelo **unchecked** (o valores de error), priorizando contratos y testing.

***

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, dos patrones útiles:

**a) Envolver (“traducir”) una excepción:**

```java
try {
    dao.guardar(obj);
} catch (SQLException e) {
    throw new DataAccessException("Fallo al guardar", e); // causa encadenada
}
```

*Sentido:* elevar el nivel semántico y no filtrar detalles de bajo nivel.

**b) Relanzar la misma excepción (preservando contexto):**

```java
try {
    procesoDelicado();
} catch (IOException e) {
    registrar(e);
    throw e; // se propaga; mantiene el stack original
}
```

*Sentido:* añadir logging/telemetría/limpieza y **no** ocultar el problema.

*(Si hicieses `throw new IOException(e)` crearías otra excepción; conserva la causa pero cambia el punto de creación.)*

***

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Cuando generamos un tipo de excepción propia para tener nuestra propia semántica podemos incluír en ella la excepción original (la de toda la vida). Así conservamos la traza entera.

Una excepción puede **envolver** otra como su **causa** (chaining). Preserva la raíz del problema y la contextualiza.

```java
// Excepción personalizada de alto nivel
class ServicioReglaNegocioException extends RuntimeException {
    public ServicioReglaNegocioException(String msg, Throwable cause) {
        super(msg, cause);
    }
}

void servicio() {
    try {
        repositorio.cargar(); // podría lanzar IOException
    } catch (IOException e) {
        throw new ServicioReglaNegocioException("No se pudo cargar datos para la operación", e);
    }
}
```

**Salida típica al imprimir la traza:**

    Exception in thread "main" ServicioReglaNegocioException: No se pudo cargar datos para la operación
        at com.acme.Servicio.servicio(Servicio.java:10)
        ...
    Caused by: java.io.IOException: Archivo no encontrado
        at java.base/...
        ...

Sí: cuando una excepción tiene **causa**, Java la muestra con `Caused by:`.

