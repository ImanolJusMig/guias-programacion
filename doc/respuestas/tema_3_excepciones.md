# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
En lenguajes como C, al no existir un mecanismo automático de gestión de errores, se debe recurrir al valor de retorno de la función o a parámetros por referencia para indicar el fallo. La primera opción consiste en reservar un valor "especial" o imposible dentro del dominio de la función para señalar el error. Por ejemplo, dado que una raíz cuadrada siempre devuelve un valor positivo o cero, se podría devolver -1.0 para indicar que la entrada era inválida.

La segunda opción, más robusta cuando el rango de resultados válidos cubre todo el espectro de los tipos de datos, consiste en devolver un código de estado (o un booleano) indicando éxito o fracaso, y devolver el resultado real del cálculo a través de un puntero pasado como argumento. De esta forma, se separa el flujo de control del error del dato calculado.

```c
// Opcion 1: Valor de retorno especial
double raiz(double n) {
    if (n < 0) return -1.0; // Código de error
    return sqrt(n);
}
// Uso: if (raiz(x) == -1.0) { /* Error */ }

// Opcion 2: Paso por referencia y código de estado
int calcular_raiz(double n, double* resultado) {
    if (n < 0) return 0; // 0 indica Error
    *resultado = sqrt(n);
    return 1; // 1 indica Éxito
}
// Uso: if (calcular_raiz(x, &res) == 0) { /* Error */ }

```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un evento anómalo que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. A diferencia de los errores en C, una excepción es un mecanismo del lenguaje que permite "saltar" desde el punto donde se produce el error hasta un bloque de código diseñado específicamente para manejarlo, independientemente de la estructura de llamadas actual.

El objetivo principal de usar excepciones es separar la lógica del negocio (el "camino feliz" o happy path) del código de tratamiento de errores. Esto mejora la legibilidad, ya que evita tener que comprobar códigos de retorno (`if (error)`) después de cada llamada a función. Además, permite que los errores se propaguen automáticamente hacia niveles superiores de la aplicación donde se tenga suficiente contexto para decidir cómo gestionarlos.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, el método que detecta el problema no devuelve un código de error, sino que lanza (`throw`) una excepción. Para el caso de un argumento inválido, lo correcto es utilizar la excepción estándar `IllegalArgumentException`. Esto detiene la ejecución del método inmediatamente.

Desde el método `main`, se debe envolver la llamada al método problemático dentro de un bloque `try` (intentar). Si ocurre la excepción, el control salta inmediatamente al bloque `catch` (capturar), donde se procesa el error mostrando un mensaje, evitando que el programa se cierre abruptamente.

```java
class Calculadora {
    // Método que lanza la excepción si algo va mal
    public double raiz(double n) {
        if (n < 0) {
            throw new IllegalArgumentException("No se puede calcular raíz de negativo");
        }
        return Math.sqrt(n);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            // Intentamos ejecutar la lógica normal
            double resultado = calc.raiz(-5.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            // Este bloque solo se ejecuta si hay error
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}

```

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar (`throw`) una excepción es la acción de crear un objeto de error e interrumpir el flujo normal para señalar una anomalía. Controlar o capturar (`catch`) es la acción de interceptar esa excepción en un bloque específico para tratarla y evitar que el programa falle. La propagación es el proceso automático mediante el cual, si una función no captura la excepción, esta se pasa a la función que la llamó, y así sucesivamente hacia arriba.

Durante la propagación, las funciones que están en la pila de llamadas (*stack*) y que no capturan la excepción son terminadas abruptamente. Se dice que la pila se "desbobina" (*stack unwinding*); sus variables locales se destruyen y no se reanudan jamás; es decir, no continúan ejecutando las líneas posteriores a la llamada que falló. En el ejemplo anterior, si `raiz` lanza la excepción, la ejecución sale de `raiz` sin devolver nada, y si hubiera funciones intermedias antes del `main` que no usaran `try-catch`, también se cancelarían hasta llegar al `catch` del `main`.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La principal ventaja es la limpieza del código. En C, si una función A llama a B, y B llama a C, y ocurre un error en C que debe ser tratado en A, la función intermedia B está obligada a recibir el error de C y pasárselo manualmente a A (mediante `return` o punteros). Esto obliga a que todas las funciones intermedias conozcan y gestionen la fontanería del error, aunque no les incumba.

Con la propagación de excepciones en Java, las funciones intermedias pueden ignorar completamente el error si no saben cómo solucionarlo. La excepción "burbujea" automáticamente a través de ellas sin necesidad de escribir código adicional, llegando directamente al manejador superior. Esto reduce el acoplamiento y evita que errores sean ignorados accidentalmente por olvidar comprobar un código de retorno.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Sí, en lenguajes orientados a objetos como Java, las excepciones son objetos (instancias de clases que heredan de `Throwable` o `Exception`). Esto aprovecha la encapsulación, ya que el objeto excepción no es un simple código numérico, sino que contiene datos internos (mensaje de error, la pila de llamadas o *stack trace*, y posiblemente otros atributos personalizados) y métodos para acceder a esa información.

Esto permite crear excepciones personalizadas definiendo nuevas clases que hereden de `Exception`. Por ejemplo, se podría crear una clase `SaldoInsuficienteException` que, además del mensaje de error, encapsule en un atributo privado la cantidad de dinero que faltaba para completar la operación. Esto permite que el bloque `catch` reciba información rica y estructurada sobre el problema.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

A diferencia de C, donde un error suele ser un entero (como -1 o un `errno`), un objeto excepción en Java encapsula dos piezas de información vitales: un mensaje descriptivo (`String`) legible para humanos y, lo más importante, el *Stack Trace* (traza de la pila).

El *Stack Trace* es un informe detallado que indica exactamente en qué clase, método y número de línea se produjo el error, así como la cadena de llamadas que llevó hasta allí. Esto facilita enormemente la depuración, ya que el programador no solo sabe "qué" falló, sino la ruta exacta de ejecución que causó el fallo, algo que en C requeriría herramientas externas de depuración.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, se pueden y se suelen poner varios bloques `catch` seguidos después de un mismo `try`. Esto sirve para manejar de forma diferente distintos tipos de errores. Por ejemplo, uno puede querer mostrar un aviso al usuario si falla la conexión a internet (`IOException`), pero cerrar el programa si hay un error de lógica interna (`NullPointerException`).

Sin embargo, solo se ejecuta un único bloque `catch`: el primero que coincida con el tipo de excepción lanzada (o sea de una clase padre). Java evalúa los `catch` en orden secuencial, de arriba a abajo. Por ello, es importante colocar las excepciones más específicas primero y las más genéricas (como `Exception`) al final; de lo contrario, las genéricas capturarían todo y el código de las específicas sería inalcanzable.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Para garantizar la ejecución de código de limpieza, Java proporciona el bloque `finally`. Este bloque se coloca después del `try` (y de los `catch` si los hay) y su contenido se ejecuta siempre, tanto si todo fue bien, como si ocurrió una excepción (incluso si esta no se captura y sigue propagándose). Es el lugar idóneo para cerrar ficheros, bases de datos o *sockets*.

```java
// Ejemplo con catch: Se intenta, se captura el error y luego se limpia
try {
    abrirFichero();
    leerDatos(); // Puede fallar
} catch (IOException e) {
    System.out.println("Error leyendo");
} finally {
    cerrarFichero(); // Se ejecuta siempre, haya error o no
}

// Ejemplo sin catch: Se intenta, se limpia, y el error se propaga hacia arriba
try {
    abrirFichero();
    leerDatos();
} finally {
    cerrarFichero(); // Se ejecuta antes de que la excepción escape de este método
}

```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, un bloque `try` puede ir seguido solo por `finally`, sin ningún `catch`. En este caso, la excepción no se controla en el método actual, pero el bloque `finally` asegura que los recursos se limpien antes de que la excepción continúe propagándose hacia el método llamador.

El bloque `finally` se ejecuta siempre, incluso si hay una sentencia `return` dentro del `try`. Antes de que el método devuelva realmente el valor y finalice, el flujo salta al `finally`, ejecuta su código, y luego procede a completar el retorno. La única forma de evitar que un `finally` se ejecute es que la máquina virtual se detenga bruscamente (por ejemplo, con `System.exit(0)`).

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

Las excepciones controladas (*checked*) son aquellas que el compilador obliga a gestionar (capturar o declarar). Representan fallos externos recuperables (como fichero no encontrado). Las no controladas (*unchecked*) son aquellas que el compilador no obliga a tratar; suelen derivar de la clase `RuntimeException` y representan errores de programación o bugs (como acceso a puntero nulo).
----
·No controladas(no obliga a try/catch//throws): runtimeExceptionnnnnnn, ilegalArgumentException,      ArrayIndexofBoundException
·Controladas(si obliga a try/catch//throws): IOException, AccessDeniedException
----
* **Excepciones Controladas (Checked):** Se usan cuando se espera que la aplicación pueda recuperarse o deba informar al usuario obligatoriamente.
* Conectar a una base de datos caída (`SQLException`).
* Leer un archivo que ha sido borrado (`FileNotFoundException`).
* Analizar un formato de fecha incorrecto introducido por usuario (`ParseException`).


* **Excepciones No Controladas (Unchecked):** Se usan para errores irrecuperables causados por fallos del programador.
* Acceder a una posición fuera de rango en un array (`ArrayIndexOutOfBoundsException`).
* Llamar a un método sobre una referencia nula (`NullPointerException`).
* Pasar un argumento inválido a un método lógico (`IllegalArgumentException`).



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra clave `throws` se coloca en la firma (declaración) de un método para indicar que dicho método puede lanzar una excepción de un tipo específico y que no la va a gestionar internamente. Forma parte del "contrato" o interfaz pública del método, avisando a quien lo llame de los riesgos.

Es la alternativa a capturar (`try-catch`) porque, ante una excepción controlada, Java te da dos opciones: o la solucionas tú mismo (capturas), o te lavas las manos y avisas de que tú puedes fallar, pasando la responsabilidad al método que te llamó (declaras con `throws`). Si eliges `throws`, la excepción se propaga hacia arriba en la pila de llamadas.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

En este ejemplo, el método `leerPrimerByte` declara que puede lanzar una `IOException`. No usa `catch` porque no sabe cómo solucionar el problema si el fichero no existe, pero usa `try-finally` para asegurar que, si el fichero se llegó a abrir, se cierre correctamente antes de propagar el error.

```java
import java.io.*;

// Declaramos 'throws IOException' en la firma
public int leerPrimerByte(String ruta) throws IOException {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream(ruta); // Puede fallar aquí
        return fis.read();               // O aquí
    } finally {
        // Garantizamos cierre, tanto si hubo éxito como si hubo error
        if (fis != null) {
            fis.close();
        }
    }
}

```

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, técnicamente se puede declarar `throws RuntimeException` o cualquiera de sus hijas en la firma del método, pero no es obligatorio ni el compilador lo exige. El método llamador tampoco está obligado a poner un `try-catch`, ya que siguen siendo excepciones no controladas.

El sentido de hacer esto es puramente documental. Sirve para avisar al programador que usa el método (a través de la documentación generada o Javadoc) de que existe la posibilidad de ese error específico (por ejemplo, "este método lanza `IllegalArgumentException` si el parámetro es nulo"). Sin embargo, no fuerza ningún control en tiempo de compilación.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

La recomendación general en Java (citando a Joshua Bloch) es: usa excepciones controladas para condiciones de las que el llamador pueda razonablemente recuperarse (fallos de red, ficheros, entrada de usuario) y no controladas para errores de programación (precondiciones violadas, lógica incorrecta) de los que no hay recuperación posible, salvo corregir el código.

No, la distinción entre excepciones controladas y no controladas es una característica casi exclusiva de Java. La mayoría de los otros lenguajes modernos (C++, C#, Python, Ruby, Kotlin) utilizan únicamente excepciones no controladas. En estos lenguajes, nunca estás obligado por el compilador a capturar una excepción, quedando a criterio del programador decidir qué errores documentar y manejar.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene sentido. Se puede capturar una excepción para hacer una gestión parcial (como escribir un log del error) y luego relanzar la misma excepción (`throw e;`) para que el nivel superior también se entere y tome medidas (como mostrar el error al usuario o detener el proceso).

También es común capturar una excepción de bajo nivel y lanzar una nueva excepción diferente (traducción de excepciones). Esto se hace para no exponer detalles de implementación. Por ejemplo, capturar una `SQLException` (detalle técnico) y lanzar una `ErrorDeNegocioException` (más abstracta) que tenga más sentido para la capa superior de la aplicación.

```java
// Caso 1: Log y Relanzar
try {
    procesar();
} catch (IOException e) {
    Logger.log("Falló el proceso: " + e.getMessage());
    throw e; // Se relanza para que quien llamó también sepa que falló
}

// Caso 2: Traducción de excepción
try {
    accederBaseDatos();
} catch (SQLException e) {
    // Encapsulamos el error técnico en uno de negocio
    throw new NoSePudoCompletarPedidoException("Error en sistema"); 
}

```

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

El concepto de "causa" (o *exception chaining*) permite que, al crear una nueva excepción para relanzarla (como en la traducción explicada antes), guardemos dentro de ella la excepción original que provocó el fallo. Esto es fundamental para no perder el rastro del error original (el *Stack Trace* real de la raíz del problema).

En Java, casi todos los constructores de excepciones aceptan un parámetro `Throwable cause`. Al imprimir la excepción por pantalla, Java muestra la cadena completa: primero el error de alto nivel y luego, anidado con un "Caused by...", el error original, permitiendo ver toda la historia del fallo.

```java
try {
    leerFicheroConfiguracion();
} catch (IOException eOriginal) {
    // Creamos la nueva excepción pasando la original como segundo argumento
    throw new ConfiguracionException("No se puede iniciar la app", eOriginal);
}
```
Examen teorico tipo test 4 respuestas, solo una verdadera.
cada 3 mal resta una bien. 
hay fragmento de codigo para entender y responder el test.