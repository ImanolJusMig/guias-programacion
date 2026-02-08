
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
La **encapsulación** es el proceso de agrupar datos (atributos) y los métodos que operan sobre ellos en una única unidad lógica (la clase). Por su parte, la **ocultación de información** es el principio de diseño que consiste en restringir el acceso a los detalles internos de esa clase, exponiendo solo lo necesario para su uso. El objetivo es reducir el acoplamiento entre distintas partes del programa, de modo que un cambio en la estructura interna de una clase no obligue a reescribir el código que la utiliza.

Entre las ventajas principales de la ocultación de información destacan:
1.  **Mantenibilidad**: Se puede cambiar la implementación interna (por ejemplo, cambiar una variable de tipo `int` a `double`) sin afectar al resto del sistema.
2.  **Integridad de los datos**: Al impedir el acceso directo a los atributos, se evita que se asignen valores inválidos o incoherentes.
3.  **Abstracción**: Permite al programador usar la clase pensando en "qué hace" y no en "cómo lo hace", simplificando la complejidad mental del desarrollo.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La **interfaz pública** de una clase es el conjunto de métodos y constantes que se han definido como accesibles para cualquier otra parte del programa. Es el "contrato" o manual de uso que la clase ofrece al exterior. En C, esto sería comparable a lo que se define en un archivo de cabecera (`.h`), mientras que la implementación oculta estaría en el `.c`.

Se relaciona directamente con la ocultación de información porque la interfaz pública actúa como una barrera o filtro. Todo lo que no está en la interfaz pública se considera privado y oculto. La ocultación asegura que el exterior solo interactúe con el objeto a través de esta interfaz controlada, protegiendo así el estado interno del objeto y permitiendo que la "caja negra" funcione correctamente.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
Es crucial diseñar la interfaz pública con sumo cuidado porque, una vez que otros programadores (o partes del propio código) empiezan a utilizarla, se crea una dependencia. La interfaz pública es una promesa de comportamiento; si se diseña mal o de forma incompleta, obligará a realizar "parches" externos o usos incorrectos de la clase.

Cambiar la interfaz pública **no es fácil**; de hecho, suele ser costoso y problemático. Si se modifica un método público (se cambia su nombre, sus parámetros o se elimina), todo el código que dependía de ese método dejará de funcionar (se "romperá") y tendrá que ser refactorizado. Por eso, la regla general es: "haz público solo lo estrictamente necesario", ya que añadir funciones más tarde es fácil, pero quitarlas es muy difícil.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las **invariantes de clase** son condiciones o reglas lógicas que deben ser siempre verdaderas para que un objeto sea considerado válido y consistente durante toda su vida. Por ejemplo, en una clase `Hora`, una invariante sería que los minutos deben estar siempre entre 0 y 59. Si en algún momento los minutos valen 90, el objeto ha roto su invariante y está en un estado corrupto.

La ocultación de información es la herramienta fundamental para preservar estas invariantes. Si los atributos fueran públicos, cualquier parte del código podría escribir `hora.minutos = 90`, rompiendo la regla. Al hacer los atributos privados, se obliga a pasar por métodos (setters) donde el programador de la clase puede insertar código de validación (`if (min < 60)...`), garantizando que nadie pueda poner el objeto en un estado inválido.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
```java
public class Punto {
    // Atributos ocultos (privados)
    private double x;
    private double y;

    // Constructor (público para poder crear objetos)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método público (parte de la interfaz)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

```

En este ejemplo, la **interfaz pública** está formada por el constructor `Punto(double, double)` y el método `calcularDistanciaAOrigen()`. Los modificadores significan lo siguiente:

* **`private`**: El miembro solo es accesible desde dentro de la propia clase `Punto`. Es invisible para el resto del programa.
* **`public`**: El miembro es accesible desde cualquier parte del programa, sin restricciones.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores de acceso se pueden aplicar a varios elementos, aunque con matices. Se aplican principalmente a los **miembros de una clase**: atributos (variables), métodos y constructores. Esto permite definir qué partes del objeto son visibles y cuáles son internas.

También se pueden aplicar a las **clases** en sí mismas, pero con restricciones. Una clase de nivel superior (la que coincide con el nombre del archivo) solo puede ser `public` o tener visibilidad de paquete (sin modificador), pero no puede ser `private` (salvo que sea una "clase interna" o *inner class*, definida dentro de otra clase).

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí, la visibilidad no es binaria; existe un espectro. En Java, además de `public` y `private`, existen dos niveles intermedios: **`protected`** (visible para la propia clase, sus subclases y clases del mismo paquete) y la visibilidad por defecto o **"package-private"** (cuando no se escribe nada), que hace visible el elemento para todas las clases del mismo paquete (carpeta), pero no para las subclases de otros paquetes.

En otros lenguajes varía. **C++** utiliza `public`, `private` y `protected`, y añade el concepto de `friend` (amigo), que permite a una clase externa específica acceder a los privados de otra. **Python**, por el contrario, no tiene privacidad estricta forzada por el compilador; usa convenciones, como poner un guion bajo `_variable` para indicar "esto debería tratarse como privado", pero técnicamente sigue siendo accesible.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

La respuesta correcta es la **(a)**: están ocultos para otras clases. La privacidad en Java (y en C++ o C#) se define a nivel de **Clase**, no a nivel de Objeto. Esto significa que un objeto de la clase `Punto` puede acceder a los atributos privados de *otro* objeto de la clase `Punto`.

```java
public double calcularDistanciaAPunto(Punto otro) {
    // Podemos acceder directamente a 'otro.x' aunque sea private
    // porque 'this' y 'otro' son de la misma clase.
    double dx = this.x - otro.x; 
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}

```

Si la privacidad fuera por instancia, el código anterior daría error, y tendríamos que usar getters públicos. Sin embargo, como ambos son `Punto`, tienen "confianza" total entre ellos.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos "getter" (observadores) y "setter" (modificadores) son métodos públicos estandarizados cuya única función es leer o escribir, respectivamente, el valor de un atributo privado. El **getter** devuelve el valor del atributo y el **setter** recibe un nuevo valor como parámetro y lo asigna al atributo.

Su propósito es mantener el control sobre el acceso a los datos. Aunque a veces parezca que hacen lo mismo que poner la variable pública, el uso de estos métodos permite añadir lógica en el futuro (como validaciones en el setter o conversiones de formato en el getter) sin romper la interfaz pública del código que usa la clase.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, ese es un error común. La ocultación de información (poner atributos como `private`) no es una medida de seguridad informática o ciberseguridad; no cifra los datos en memoria ni evita que un hacker con un depurador o editor de memoria los lea.

La "seguridad" a la que se refiere la POO es la **seguridad contra errores de programación** (robustez). Se refiere a proteger el código de usos incorrectos, evitando que un programador despistado modifique un dato interno que no debería tocar y rompa la lógica del programa (inconsistencia de estado). Es seguridad estructural, no criptográfica.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un **miembro de instancia** pertenece a un objeto concreto; cada vez que haces un `new`, se crea una nueva copia de esa variable (por ejemplo, cada `Punto` tiene su propia `x`). Un **miembro de clase** (marcado con `static`) es único y compartido por todos los objetos de esa clase; existe una sola copia en memoria, independientemente de cuántos objetos se creen.

Sí, los miembros de clase también pueden (y a menudo deben) ocultarse usando `private`. Por ejemplo, si una clase tiene un contador interno para generar IDs únicos, esa variable debería ser `private static` para que nadie pueda alterar la secuencia de generación de IDs desde fuera.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido en patrones de diseño específicos. Si un constructor es privado, nadie puede hacer `new Clase()` desde fuera. Esto se utiliza, por ejemplo, en el patrón **Singleton**, donde se desea garantizar que solo exista una única instancia de la clase en todo el programa, o en clases de utilidad (como `java.lang.Math`), que solo tienen métodos estáticos y no tiene sentido instanciarlas.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

Los miembros de clase se indican con la palabra reservada **`static`**. Estos atributos se inicializan una sola vez y mantienen su valor entre diferentes llamadas al constructor.

```java
public class Punto {
    private double x, y;
    
    // Miembros de clase (static) para guardar los máximos históricos
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Actualizamos los estáticos si el nuevo punto es mayor
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}

```

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`?

### Respuesta

```java
// Método de fábrica (Factory method)
public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeada = Math.round(x);
    double yRedondeada = Math.round(y);
    // Llamamos al constructor privado o público desde aquí
    return new Punto(xRedondeada, yRedondeada);
}

```

Sí, es obligatorio usar **`static`**. Al ser un método que "fabrica" objetos, se debe poder llamar antes de tener un objeto creado (es decir, se llama como `Punto.crearPuntoRedondeado(...)`), por lo que no puede depender de una instancia (`this`) existente.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

```java
public class Punto {
    // Cambio radical de la implementación interna (oculta)
    private double[] coordenadas; 

    public Punto(double x, double y) {
        coordenadas = new double[2];
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    // La interfaz pública (getters) NO cambia, aunque el código interno sí
    public double getX() {
        return coordenadas[0];
    }
    
    public double getY() {
        return coordenadas[1];
    }
}

```

Al haber usado encapsulación (getters), el código externo que llamaba a `p.getX()` seguirá funcionando sin enterarse de que internamente ahora hay un array en lugar de dos variables sueltas.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque parezca redundante, la convención habitual es declarar los atributos **siempre privados**. Declararlo público cierra la puerta a cambios futuros. Si hoy es público, mañana no puedes añadir validación (invariantes) sin romper el código de quien lo usa.

Con un setter, puedes proteger las **invariantes de clase**. Por ejemplo, si tienes `public int edad`, cualquiera puede poner `-5`. Si tienes `setEdad(int e)`, puedes poner un `if (e < 0)` y lanzar un error. Si hubieras hecho la variable pública desde el principio, ya no podrías añadir ese control de seguridad sin obligar a cambiar todos los sitios donde se accedía a la variable directamente.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es **inmutable** si el estado de sus objetos no puede cambiar una vez han sido creados (construidos). No tienen métodos "setters". Si se quiere modificar algo, en lugar de cambiar el objeto actual, se devuelve un **nuevo objeto** con el valor modificado.

Un método modificador es cualquiera que altere el estado interno. Normalmente son "setters" (`setX`), pero pueden ser otros como `mover()`, `resetear()`, etc. La ventaja de la inmutabilidad es la seguridad en entornos concurrentes (varios hilos/procesos): si el objeto no cambia, no hay riesgo de que un hilo lea datos a medias mientras otro escribe. Además, son más fáciles de entender y cachear.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No necesariamente. Existe una mala costumbre en Java de generar automáticamente getters y setters para todo (Java Beans). Sin embargo, desde el punto de vista del diseño puro, un objeto debería exponer **comportamiento**, no datos.

Incluir setters indiscriminadamente rompe la encapsulación, convirtiendo al objeto en una simple estructura de datos tonta. Es mejor proporcionar métodos con significado de negocio (ej: `empleado.subirSueldo(5%)` en lugar de `empleado.setSueldo(nuevoCalculo)`). Además, evitar setters favorece la inmutabilidad, que suele hacer los sistemas más robustos.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase `String` en Java es **inmutable**. Cuando haces `String s = "Hola" + " Mundo";`, no se modifica la cadena original "Hola"; se crea un tercer objeto nuevo en memoria que contiene "Hola Mundo".

Si se realizan muchas concatenaciones (por ejemplo, dentro de un bucle), se crean y destruyen miles de objetos intermedios, lo que es desastroso para el rendimiento y la memoria. Para esos casos, Java ofrece las clases **`StringBuilder`** (o `StringBuffer`), que sí son mutables. Permiten ir añadiendo caracteres a un buffer interno sin crear objetos nuevos constantemente, y al final se convierte a String.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?

### Respuesta

En Java, el operador `==` compara **identidad** (referencias de memoria), es decir, si dos variables apuntan exactamente al mismo objeto en el Heap (similar a comparar punteros en C). Para comparar **contenido** (si dos Puntos tienen la misma x e y, aunque sean objetos distintos), se debe usar el método.

El método **`equals(Object o)`** es el estándar para comparación lógica. Por defecto (heredado de la clase base Object), `equals` hace lo mismo que `==`. Sin embargo, las clases bien diseñadas (como `String`, `Date`, etc.) sobrescriben este método para comparar sus datos internos. Por eso, las cadenas en Java **siempre** deben compararse con `cadena1.equals(cadena2)`, nunca con `==`.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?

### Respuesta

Las clases **wrapper** (envoltorios) son clases que encapsulan un tipo primitivo (`int`, `double`, `boolean`) dentro de un objeto (`Integer`, `Double`, `Boolean`). Esto es necesario porque en Java los primitivos no son objetos y ciertas estructuras (como las listas `ArrayList` o Colecciones) solo pueden almacenar objetos.

La conversión suele ser automática gracias al **autoboxing** (Java convierte `int` a `Integer` solo) y **unboxing**. La ventaja es que permiten tratar números como objetos (con métodos útiles y valor `null`). No todos los lenguajes tienen esto: en lenguajes "puros" como Smalltalk o Ruby, o modernos como Python, todo es un objeto desde el principio (`1.toString()` funciona), por lo que no necesitan wrappers explícitos.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un **enumerado** (`enum`) es un tipo de dato especial que define un conjunto fijo y limitado de constantes con nombre (como Días de la Semana, Palos de la Baraja). Sirve para representar variables que solo pueden tomar uno de esos valores predefinidos.

En Java, a diferencia de C donde son simples enteros (`0, 1, 2...`), los `enum` son **clases completas**. Pueden tener atributos, constructores y métodos. Su ventaja en encapsulación es la **seguridad de tipos**: si un método pide un `DiaSemana`, el compilador garantiza que solo se le puede pasar un valor válido del enum (LUNES, MARTES...), haciendo imposible pasar un valor inválido (como un "8" o "JUEVESS").

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`).

### Respuesta

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    // Atributos privados (encapsulación)
    private final int dias;
    private final int orden;

    // Constructor privado (solo lo usa el propio enum)
    private Mes(int dias, int orden) {
        this.dias = dias;
        this.orden = orden;
    }

    public int getDias() { return dias; }
    public int getOrden() { return orden; }

    // Lógica simplificada de estaciones (solapamientos aproximados)
    public boolean esDeInvierno(boolean norte) {
        return norte ? (orden <= 3 || orden == 12) : (orden >= 6 && orden <= 9);
    }

    public boolean esDePrimavera(boolean norte) {
        return norte ? (orden >= 3 && orden <= 6) : (orden >= 9 && orden <= 12);
    }

    public boolean esDeVerano(boolean norte) {
        return norte ? (orden >= 6 && orden <= 9) : (orden <= 3 || orden == 12);
    }

    public boolean esDeOtono(boolean norte) {
        return norte ? (orden >= 9 && orden <= 12) : (orden >= 3 && orden <= 6);
    }
}

