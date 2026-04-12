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

```java
/**
* Retorna los últimos N posts que no pertenecen al usuario user
*/
public List<Post> ultimosPosts(Usuario user, int cantidad) {
        
    List<Post> postsOtrosUsuarios = new ArrayList<Post>();
    for (Post post : this.posts) {
        if (!post.getUsuario().equals(user)) {
            postsOtrosUsuarios.add(post);
        }
    }
        
   // ordena los posts por fecha
   for (int i = 0; i < postsOtrosUsuarios.size(); i++) {
       int masNuevo = i;
       for(int j= i +1; j < postsOtrosUsuarios.size(); j++) {
           if (postsOtrosUsuarios.get(j).getFecha().isAfter(
     postsOtrosUsuarios.get(masNuevo).getFecha())) {
              masNuevo = j;
           }    
       }
      Post unPost = postsOtrosUsuarios.set(i,postsOtrosUsuarios.get(masNuevo));
      postsOtrosUsuarios.set(masNuevo, unPost);    
   }
        
    List<Post> ultimosPosts = new ArrayList<Post>();
    int index = 0;
    Iterator<Post> postIterator = postsOtrosUsuarios.iterator();
    while (postIterator.hasNext() &&  index < cantidad) {
        ultimosPosts.add(postIterator.next());
    }
    return ultimosPosts;
}

```

Bad smell reinventa la rueda
Refcatoring reeplace loop with pipeline

```java
/**
* Retorna los últimos N posts que no pertenecen al usuario user
*/
public List<Post> ultimosPosts(Usuario user, int cantidad) {
    return this.posts.stream()
        .filter (post -> !post.getUsuario().equals(user))
        .sorted ((p1,p2) -> p2.getFecha().compareTo(p1.getFecha()))
        .limit (cantidad)
        .collect(Collectors.toList());
}
```

Punto 5

```java
public class Supermercado {
   public void notificarPedido(long nroPedido, Cliente cliente) {
     String notificacion = MessageFormat.format(“Estimado cliente, se le informa que hemos recibido su pedido         con número {0}, el cual será enviado a la dirección {1}”, new Object[] { nroPedido,                             cliente.getDireccionFormateada() });

     // lo imprimimos en pantalla, podría ser un mail, SMS, etc..
    System.out.println(notificacion);
  }
}

public class Cliente {
   public String getDireccionFormateada() {
	return 
		this.direccion.getLocalidad() + “, ” +
		this.direccion.getCalle() + “, ” +
		this.direccion.getNumero() + “, ” +
		this.direccion.getDepartamento()
      ;
}

```

Bad smell feature envy
REfactoring move method

```java
public class Supermercado {
    public void notificarPedido(long nroPedido, Cliente cliente) {
        String notificacion = MessageFormat.format("Estimado cliente, se le informa que hemos recibido su pedido con número {0}, el cual será enviado a la dirección {1}", new Object[] { nroPedido, cliente.getDireccionFormateada() });

        // lo imprimimos en pantalla, podría ser un mail, SMS, etc..
        System.out.println(notificacion);
  }
}

public class Cliente {
    private Direccion direccion;

    public String getDireccionFormateada() {
	    return this.direccion.toString();
    }
}

public class Direccion {
    public String localidad;
    public String calle;
    public int numero;
    public String departamento;

    @Override
    public String toString() {
        return this.localidad + ", " + this.calle  + ", " + this.numero + ", " + this.departamento;
    }
}

```

Bad smell se rompe el encapsulamiento
Refcatoring encapsulate fileds

```java
public class Supermercado {
    public void notificarPedido(long nroPedido, Cliente cliente) {
        String notificacion = MessageFormat.format("Estimado cliente, se le informa que hemos recibido su pedido con número {0}, el cual será enviado a la dirección {1}", new Object[] { nroPedido, cliente.getDireccionFormateada() });

        // lo imprimimos en pantalla, podría ser un mail, SMS, etc..
        System.out.println(notificacion);
  }
}

public class Cliente {
    private Direccion direccion;

    public String getDireccionFormateada() {
	    return this.direccion.toString();
    }
}

public class Direccion {
    private String localidad;
    private String calle;
    private int numero;
    private String departamento;

    @Override
    public String toString() {
        return this.localidad + ", " + this.calle  + ", " + this.numero + ", " + this.departamento;
    }
}

```

Bad smell Middle Man
Considero q si bien se presenta un bad smell es correspondiente que el middle man exista para una posible expansion del modelo, no me parece acertado qeu la clase Suoermecado interactue directamente con direccion sino que debe pasar por Cliente

Punto 4

```java
public class Producto {
    private String nombre;
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}

public class ItemCarrito {
    private Producto producto;
    private int cantidad;
        
    public Producto getProducto() {
        return this.producto;
    }
    
    public int getCantidad() {
        return this.cantidad;
    }

}

public class Carrito {
    private List<ItemCarrito> items;
    
    public double total() {
return this.items.stream()
.mapToDouble(item -> 
item.getProducto().getPrecio() * item.getCantidad())
.sum();
    }
}
```
Bad smell feature envy
Refactoring move method

```java
public class Producto {
    private String nombre;
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}

public class ItemCarrito {
    private Producto producto;
    private int cantidad;
        
    public Producto getProducto() {
        return this.producto;
    }
    
    public int getCantidad() {
        return this.cantidad;
    }

    public double calcularMonto(){
        return this.producto.getPrecio() * this.cantidad;

}

public class Carrito {
    private List<ItemCarrito> items;
    
    public double total() {
return this.items.stream()
.mapToDouble(item -> item.calcularMonto())
.sum();
    }
}
```

Punto 6

```java
public class Usuario {
    String tipoSubscripcion;
    // ...

    public void setTipoSubscripcion(String unTipo) {
   	 this.tipoSubscripcion = unTipo;
    }
    
    public double calcularCostoPelicula(Pelicula pelicula) {
   	 double costo = 0;
   	 if (tipoSubscripcion=="Basico") {
   		 costo = pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno();
   	 }
   	 else if (tipoSubscripcion== "Familia") {
   		 costo = (pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno()) * 0.90;
   	 }
   	 else if (tipoSubscripcion=="Plus") {
   		 costo = pelicula.getCosto();
   	 }
   	 else if (tipoSubscripcion=="Premium") {
   		 costo = pelicula.getCosto() * 0.75;
   	 }
   	 return costo;
    }
}

public class Pelicula {
    LocalDate fechaEstreno;
    // ...

    public double getCosto() {
   	 return this.costo;
    }
    
    public double calcularCargoExtraPorEstreno(){
	// Si la Película se estrenó 30 días antes de la fecha actual, retorna un cargo de 0$, caso contrario, retorna un cargo extra de 300$
   	return (ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now()) ) > 30 ? 0 : 300;
    }
}

```

bad smell switch statments
Refactoring Replace Conditional with Polymorphism

```java
public interfcate Suscripcion{
    public double calcularCostoPelicula(Pelicula pelicula);
}

public SuscripcionBasica implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno();
   	}
public SuscripcionFamilia implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno() * 0.9;
   	}
public SuscripcionPlus implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.getCosto();

public SuscripcionPremium implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.getCosto() * 0.75;
   	}
public class Usuario {
    Suscipcion tipoSubscripcion;
    // ...

    public void setTipoSubscripcion(Suscipcion s) {
   	 this.tipoSubscripcion = s
    }
}

public class Pelicula {
    LocalDate fechaEstreno;
    // ...

    public double getCosto() {
   	 return this.costo;
    }
    
    public double calcularCargoExtraPorEstreno(){
	// Si la Película se estrenó 30 días antes de la fecha actual, retorna un cargo de 0$, caso contrario, retorna un cargo extra de 300$
   	return (ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now()) ) > 30 ? 0 : 300;
    }
}
```  

Bad smell feature envy
refactoring move method

```java
public interfcate Suscripcion{
    public double calcularCostoPelicula(Pelicula pelicula);
}

public SuscripcionBasica implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.calcularTotal()
   	}

public SuscripcionFamilia implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.calcularTotal() * 0.9;
   	}
public SuscripcionPlus implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.getCosto();

public SuscripcionPremium implements Suscipcion{
    public double calcularCostoPelicula(Pelicula pelicula) {
   	    return pelicula.getCosto() * 0.75;
   	}
public class Usuario {
    Suscipcion tipoSubscripcion;
    // ...

    public void setTipoSubscripcion(Suscipcion s) {
   	 this.tipoSubscripcion = s
    }


}

public class Pelicula {
    LocalDate fechaEstreno;
    // ...

    public double getCosto() {
   	 return this.costo;
    }
    
    public double calcularCargoExtraPorEstreno(){
	// Si la Película se estrenó 30 días antes de la fecha actual, retorna un cargo de 0$, caso contrario, retorna un cargo extra de 300$
   	return (ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now()) ) > 30 ? 0 : 300;
    }

    public double calcularTotal(){
        return this.costo + this.calcularCargoExtraPorEstreno();
    }
}
```
Bad smell codigo rpetido
Refactoring Replace Conditional with Polymorphism


