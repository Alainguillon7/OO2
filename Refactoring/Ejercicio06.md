6.1

```java
public class EmpleadoTemporario {
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    public double horasTrabajadas = 0;
    public int cantidadHijos = 0;
    // ......
    
public double sueldo() {
return this.sueldoBasico
(this.horasTrabajadas * 500) 
(this.cantidadHijos * 1000) 
(this.sueldoBasico * 0.13);
}
}


public class EmpleadoPlanta {
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
        return this.sueldoBasico 
+ (this.cantidadHijos * 2000)
- (this.sueldoBasico * 0.13);
    }
}

public class EmpleadoPasante {
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    // ......
    
    public double sueldo() {
        return this.sueldoBasico - (this.sueldoBasico * 0.13);
    }
}
```

Bad smells
Duplicated Code
Refactoring
Extract superclass

```java
public abstract class Empleado {}

public class EmpleadoTemporario extends Empleado{
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    public double horasTrabajadas = 0;
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
    return this.sueldoBasico 
    + (this.horasTrabajadas * 500) 
    - (this.cantidadHijos * 1000) 
    - (this.sueldoBasico * 0.13);
    }
}


public class EmpleadoPlanta extends Empleado{
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
        return this.sueldoBasico 
            + (this.cantidadHijos * 2000)
            - (this.sueldoBasico * 0.13);
    }
}

public class EmpleadoPasante extends Empleado{
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    // ......
    
    public double sueldo() {
        return this.sueldoBasico - (this.sueldoBasico * 0.13);
    }
}
```

Bad smell Duplicated Code
Refactoring extract method

```java
public abstract class Empleado {}

public class EmpleadoTemporario extends Empleado{
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    public double horasTrabajadas = 0;
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
    return this.calcularBasico()
    + (this.horasTrabajadas * 500) 
    - (this.cantidadHijos * 1000);
    }

publlic double calcularBasico(){
    return this.sueldoBasico - (this.sueldoBasico * 0.13);
}


public class EmpleadoPlanta extends Empleado{
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
        return this.calcularBasico() 
            + (this.cantidadHijos * 2000)
    }

    publlic double calcularBasico(){
        return this.sueldoBasico - (this.sueldoBasico * 0.13);
    }
}

public class EmpleadoPasante extends Empleado{
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;
    // ......

    public sueldo(){
        return this.calcularBasico();
    }
    public double calcularBasico() {
        return this.sueldoBasico - (this.sueldoBasico * 0.13);
    }
}
```
Bad smell Duplicated COde
Refactoring Pull up method y pull up Field

```java
public abstract class Empleado {
    public String nombre;
    public String apellido;
    public double sueldoBasico = 0;

    public double calcularBasico() {
        return this.sueldoBasico - (this.sueldoBasico * 0.13);
    }

    public  abstract double sueldo(); 
}

public class EmpleadoTemporario extends Empleado{
    public double horasTrabajadas = 0;
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
    return this.calcularBasico()
    + (this.horasTrabajadas * 500) 
    - (this.cantidadHijos * 1000);
    }

public class EmpleadoPlanta extends Empleado{
    public int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
        return this.calcularBasico() 
            + (this.cantidadHijos * 2000)
    }
}

public class EmpleadoPasante extends Empleado{
    // ......

    public sueldo(){
        return this.calcularBasico();
    }

}
```

Bad smell romper el encapuslamiento
Refactoring Encapsulate Field

```java
public abstract class Empleado {
    private String nombre;
    private String apellido;
    private double sueldoBasico = 0;

    public double calcularBasico() {
        return this.sueldoBasico - (this.sueldoBasico * 0.13);
    }

    public  abstract double sueldo(); 
}

public class EmpleadoTemporario extends Empleado{
    private double horasTrabajadas = 0;
    private int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
    return this.calcularBasico()
    + (this.horasTrabajadas * 500) 
    - (this.cantidadHijos * 1000);
    }

public class EmpleadoPlanta extends Empleado{
    private int cantidadHijos = 0;
    // ......
    
    public double sueldo() {
        return this.calcularBasico() 
            + (this.cantidadHijos * 2000)
    }
}

public class EmpleadoPasante extends Empleado{
    // ......

    public sueldo(){
        return this.calcularBasico();
    }

}
```

Punto 2

```java
public class Juego {
    // ......
    public void incrementar(Jugador j) {
        j.puntuacion = j.puntuacion + 100;
    }
    public void decrementar(Jugador j) {
        j.puntuacion = j.puntuacion - 50;
    }

public class Jugador {
    public String nombre;
    public String apellido;
    public int puntuacion = 0;
}
}
```
Bad smell feature envy
Refactoring move method

```java
public class Juego {
    // ......
    public void incrementar(Jugador j) {
        j.incrementar();
    }
    public void decrementar(Jugador j) {
        j.decrmentar();
    }
}
public class Jugador {
    public String nombre;
    public String apellido;
    public int puntuacion = 0;

    public void incrementar(Jugador j) {
        j.puntuacion = j.puntuacion + 100;
    }
    public void decrementar(Jugador j) {
        j.puntuacion = j.puntuacion - 50;
    }
}

```

Bad smell nombrs poco expliactivos
Refcatoring rename method

```java
public class Juego {
    // ......
    public void incrementarPuntaje(Jugador j) {
        j.incrementarPuntaje();
    }
    public void incrementarPuntaje(Jugador j) {
        j.incrementarPuntaje();
    }
}
public class Jugador {
    public String nombre;
    public String apellido;
    public int puntuacion = 0;

    public void incrementarPuntaje(Jugador j) {
        j.puntuacion = j.puntuacion + 100;
    }
    public void incrementarPuntaje(Jugador j) {
        j.puntuacion = j.puntuacion - 50;
    }
}

```
Bad smell rompe el encapsulamiento
Refactoring Encapsulate fields

```java
public class Juego {
    // ......
    public void incrementarPuntaje(Jugador j) {
        j.incrementarPuntaje();
    }
    public void incrementarPuntaje(Jugador j) {
        j.incrementarPuntaje();
    }
}
public class Jugador {
    private String nombre;
    private String apellido;
    private int puntuacion = 0;


    public String getNombre() {
        return this.nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getApellido() {
        return this.apellido;
    }

    public void setApellido(String apellido) {
        this.apellido = apellido;
    }

    public int getPuntuacion() {
        return this.puntuacion;
    }

    public void incrementarPuntaje(Jugador j) {
        j.puntuacion = j.puntuacion + 100;
    }
    public void incrementarPuntaje(Jugador j) {
        j.puntuacion = j.puntuacion - 50;
    }
}

```

Punto 3

