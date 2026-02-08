# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
Las cuatro características fundamentales de la Programación Orientada a Objetos (POO) son la **abstracción**, el **encapsulamiento**, la **herencia** y el **polimorfismo**. La **abstracción** consiste en aislar un elemento de su contexto y definir las características esenciales que lo distinguen, permitiendo modelar conceptos del mundo real en código ignorando los detalles innecesarios. Por otro lado, el **encapsulamiento** es el mecanismo que agrupa datos y métodos en una misma unidad (la clase) y protege la información interna, impidiendo el acceso directo a los datos desde fuera para asegurar la integridad del objeto.

La **herencia** es la capacidad de crear nuevas clases basadas en clases existentes, permitiendo reutilizar código y establecer jerarquías donde las clases hijas adquieren las propiedades y comportamientos de sus padres. Finalmente, el **polimorfismo** es la capacidad que tienen los objetos de diferentes clases de responder al mismo mensaje o método de formas distintas, permitiendo tratar objetos de tipos diferentes de manera uniforme.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Cuatro de los lenguajes más populares y ampliamente utilizados en la industria que soportan el paradigma orientado a objetos son:

1.  **Java**: Un lenguaje puramente orientado a objetos (casi en su totalidad) conocido por su portabilidad y uso masivo en entornos empresariales y desarrollo Android.
2.  **C++**: Una extensión del lenguaje C que introduce clases y objetos, permitiendo programación de alto rendimiento con gestión manual de memoria.
3.  **Python**: Un lenguaje de alto nivel y tipado dinámico que soporta múltiples paradigmas, donde "todo es un objeto", muy popular en ciencia de datos e IA.
4.  **C# (C Sharp)**: Desarrollado por Microsoft, es el lenguaje principal del ecosistema .NET, muy similar a Java en sintaxis y filosofía.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La **programación estructurada** es un paradigma que busca mejorar la claridad y calidad del código eliminando el uso descontrolado de saltos (como el `GOTO`). Se basa en el uso exclusivo de tres estructuras de control básicas: secuencia (ejecución paso a paso), selección (condicionales como `if` o `switch`) e iteración (bucles como `for` o `while`). Esto permite que el flujo del programa sea predecible y fácil de seguir.

Por su parte, la **programación modular** es una técnica de diseño que consiste en dividir un programa complejo en subprogramas o módulos más pequeños e independientes (similar a dividir código en varios archivos `.c` y `.h` en C). Cada módulo tiene una funcionalidad específica y una interfaz bien definida para comunicarse con los demás. Esto facilita el mantenimiento, la reutilización de código y el trabajo en equipo, ya que permite aislar funcionalidades y reducir la complejidad global del sistema.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Un objeto se define por tres elementos esenciales: **identidad**, **estado** y **comportamiento**. La **identidad** es la propiedad que permite distinguir a un objeto de todos los demás, incluso si sus contenidos son idénticos (suele corresponderse con su dirección de memoria o una referencia única).

El **estado** representa el conjunto de valores de todos los atributos (o variables) que tiene el objeto en un momento dado; es decir, "cómo está" el objeto ahora (por ejemplo, un objeto Coche puede tener velocidad=0 y color=rojo). El **comportamiento** está definido por los métodos (funciones) que el objeto puede ejecutar, determinando qué acciones puede realizar o cómo reacciona ante mensajes de otros objetos.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una **clase** es una plantilla, molde o "blueprint" que define la estructura y el comportamiento común de un conjunto de objetos. No es un objeto en sí mismo, sino la definición de qué atributos y métodos tendrán los objetos creados a partir de ella. Un **objeto** (o **instancia**) es la concreción de esa clase en la memoria del ordenador; es decir, es un elemento específico creado usando la clase como molde. Por tanto, no son lo mismo: la clase es el concepto abstracto (ej: "Plano de una casa") y el objeto es lo real (ej: "La casa construida en la calle X").

No todos los lenguajes orientados a objetos utilizan clases. Existen lenguajes basados en **prototipos**, como **JavaScript** (en sus versiones originales), donde no existen clases como tal. En estos lenguajes, los objetos se crean clonando otros objetos existentes (prototipos) y extendiendo su funcionalidad, en lugar de instanciarse desde una plantilla estática.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
Generalmente, en lenguajes como Java, los objetos se almacenan en una zona de memoria dinámica llamada **Heap** (montículo), mientras que las referencias (las variables que apuntan a esos objetos) se guardan en el **Stack** (pila) de ejecución. Esto difiere de lenguajes como C++, donde los objetos pueden crearse tanto en el Stack (variables locales automáticas) como en el Heap (usando `new` o `malloc`).

La **recolección de basura** (Garbage Collection) es un mecanismo automático de gestión de memoria presente en lenguajes como Java, Python o C#. Su función es detectar y liberar la memoria ocupada por objetos que ya no son accesibles por el programa (no tienen ninguna referencia apuntando a ellos). Esto evita errores comunes en C como las fugas de memoria (*memory leaks*), ya que el programador no necesita liberar explícitamente la memoria (`free`) cuando deja de usar un objeto.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un **método** es una función que pertenece a una clase y define el comportamiento de los objetos de esa clase. A diferencia de las funciones globales en C, los métodos tienen acceso implícito a los datos internos (atributos) del objeto sobre el que se ejecutan. Son las acciones que un objeto puede realizar.

La **sobrecarga de métodos** es una característica que permite definir varios métodos con el **mismo nombre** dentro de la misma clase, siempre y cuando su lista de parámetros sea diferente (en cantidad o en tipo de datos). El compilador distingue a qué método llamar basándose en los argumentos que se le pasan. Esto permite, por ejemplo, tener un método `sumar(int a, int b)` y otro `sumar(double a, double b)` conviviendo sin conflicto.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```java
// Definición de la clase Punto
class Punto {
    // Atributos (visibilidad por defecto)
    int x;
    int y;

    // Método para calcular la distancia al origen (0,0)
    // Usa la fórmula: raíz cuadrada de (x² + y²)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Clase principal para probar el código
public class Main {
    public static void main(String[] args) {
        // Crear una instancia (objeto) de la clase Punto
        Punto miPunto = new Punto();
        
        // Asignar valores a los atributos
        miPunto.x = 3;
        miPunto.y = 4;
        
        // Llamar al método y mostrar el resultado
        double distancia = miPunto.calculaDistanciaAOrigen();
        System.out.println("La distancia al origen es: " + distancia);
    }
}

```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada en Java es el método `public static void main(String[] args)`. La palabra clave **`static`** indica que el método o atributo pertenece a la **clase** en sí misma y no a una instancia específica (objeto). Esto permite invocar al método `main` sin necesidad de crear un objeto previo de la clase, lo cual es imprescindible para arrancar el programa.

`static` no se usa solo para el `main`; se emplea para constantes globales, métodos utilitarios (como `Math.sqrt`) o contadores compartidos entre todos los objetos. Cuando se combina con **`final`** (que impide que el valor cambie), se utiliza para definir **constantes** reales (ej: `static final double PI = 3.1416`), similares a los `#define` de C, pero con tipo seguro.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para ejecutar Java desde la terminal, primero se debe **compilar** el código fuente (`.java`) usando el comando `javac NombreArchivo.java`. Esto genera un fichero intermedio llamado `.class` que contiene el **byte-code**. Posteriormente, se ejecuta el programa con el comando `java NombreClase` (sin la extensión), que arranca la Máquina Virtual.

Java es un lenguaje **híbrido**: se compila, pero no a código máquina nativo del procesador (como C), sino a un código intermedio llamado **byte-code**. La **Máquina Virtual de Java (JVM)** es el software encargado de interpretar o compilar "al vuelo" (JIT) este byte-code para que el procesador real lo entienda. Esto permite la portabilidad: "escribe una vez, ejecuta en cualquier lugar", ya que el mismo archivo `.class` funciona en Windows, Linux o Mac siempre que tengan una JVM instalada.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

La palabra clave **`new`** se utiliza para reservar memoria en el Heap y crear una nueva instancia de una clase. Es el equivalente (aunque más seguro y potente) al `malloc` de C. Inmediatamente después de reservar memoria, `new` llama al **constructor**. Un constructor es un método especial que tiene el mismo nombre que la clase y no devuelve ningún valor; su objetivo es **inicializar** el objeto, asignando valores iniciales a sus atributos.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Este es el Constructor
    public Empleado(String d, String n, String a) {
        dni = d;
        nombre = n;
        apellidos = a;
    }
}

// Uso:
// Empleado e = new Empleado("12345678Z", "Pepe", "Pérez");

```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La palabra clave **`this`** es una referencia que apunta al **objeto actual** que está ejecutando el método. Se utiliza dentro de la clase para referirse a sus propios atributos o métodos. Es útil para evitar ambigüedades, por ejemplo, cuando un parámetro del método se llama igual que un atributo de la clase (fenómeno conocido como *shadowing*).

No se llama igual en todos los lenguajes; por ejemplo, en **Python** se utiliza explícitamente `self` como primer parámetro de los métodos, y en Visual Basic se usaba `Me`.

```java
class Punto {
    int x;
    int y;

    // Constructor usando 'this' para distinguir 
    // entre el atributo 'x' y el parámetro 'x'
    public Punto(int x, int y) {
        this.x = x; // Asigna el parámetro x al atributo x del objeto
        this.y = y; // Asigna el parámetro y al atributo y del objeto
    }
}

```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

```java
class Punto {
    int x, y;

    // ... (constructor y otros métodos) ...

    // Método que calcula la distancia a otro punto recibido por parámetro
    double distanciaA(Punto otroPunto) {
        // Restamos las coordenadas de 'este' objeto (this) con el 'otro'
        int dx = this.x - otroPunto.x;
        int dy = this.y - otroPunto.y;
        
        return Math.sqrt(dx * dx + dy * dy);
    }
}

```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función?

### Respuesta

En Java, estrictamente hablando, todo se pasa **por valor** (por copia). Sin embargo, hay una diferencia crucial: cuando pasamos un objeto (como `Punto`), lo que se copia es la **referencia** (la dirección de memoria). Por tanto, al tener una copia de la "llave" de acceso al objeto, si modificamos sus atributos internos dentro del método, los cambios **sí afectan** al objeto original fuera del método.

Por el contrario, con los tipos primitivos (como `int`, `double`, `boolean`), se pasa una copia literal del valor. Si modificamos ese entero dentro de la función, solo estamos cambiando la copia local; el valor original fuera de la función **permanece intacto**. Esto es similar a pasar un valor `int` en C frente a pasar un puntero `int*`.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método `toString()` es un método especial heredado por todas las clases en Java (proviene de la clase base `Object`). Su propósito es devolver una representación en forma de texto (`String`) del objeto. Si no se redefine, devuelve un código hash poco útil, pero se suele sobrescribir para que devuelva información legible sobre el estado del objeto. Es lo que se llama automáticamente cuando hacemos `System.out.println(miObjeto)`.

Existe en muchos otros lenguajes con nombres diferentes. Por ejemplo, en **Python** es el método mágico `__str__`, y en C# es también `ToString()`.

```java
class Punto {
    int x, y;
    
    // Sobreescritura del método toString
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta

Una clase es conceptualmente una evolución del `struct` de C, pero con esteroides. Un `struct` en C es simplemente un contenedor de datos agrupados; define una estructura de memoria, pero es pasivo. Para que un `struct` fuese como una clase, le faltan dos cosas fundamentales: la **unión de los datos con el código** (métodos) y el **control de acceso** (visibilidad).

En C, las funciones están separadas de los datos. Para convertir un `struct` en una clase, las funciones deberían ser parte intrínseca de la definición, operando automáticamente sobre los datos internos. Además, un `struct` es siempre público (cualquiera puede tocar sus datos); una clase añade mecanismos (`private`, `protected`) para proteger esos datos, garantizando la encapsulación.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Para emular una clase en C, definimos un `struct` con los datos y creamos funciones separadas que reciban obligatoriamente un **puntero** a ese `struct` como primer parámetro. Ese puntero actúa explícitamente como la referencia `this`, que en Java es implícita (mágica).

```c
#include <math.h>

// Definición de datos (Atributos)
typedef struct {
    int x;
    int y;
} Punto;

// Simulación del método (Comportamiento)
// Recibe un puntero 'self' que actúa como 'this'
double punto_calculaDistanciaAOrigen(Punto* self) {
    // Accedemos a los datos usando el puntero (flecha)
    return sqrt(self->x * self->x + self->y * self->y);
}

// Uso:
// Punto p = {3, 4};
// double d = punto_calculaDistanciaAOrigen(&p); 

