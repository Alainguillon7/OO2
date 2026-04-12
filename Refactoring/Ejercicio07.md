```java
abstract class Etiqueta {
    protected String nombreProducto;
    protected double precio;

    public Etiqueta(String nombre, double precio) {
        this.nombreProducto = nombre;
        this.precio = precio;
    }
}

class EtiquetaSimple extends Etiqueta {
    public EtiquetaSimple(String nombre, double precio) {
        super(nombre, precio);
    }

    public void generar() {
        System.out.println("--- ETIQUETA BÁSICA ---");
        System.out.println("Producto: " + nombreProducto);
        System.out.println("Precio: $" + precio);
        System.out.println("-----------------------");
    }
}

class EtiquetaDetalle extends Etiqueta {
    public EtiquetaDetalle(String nombre, double precio) {
        super(nombre, precio);
    }

    public void generar() {
        System.out.println("--- ETIQUETA DETALLE ---");
        System.out.println("Producto: " + nombreProducto);
        System.out.println("Precio sin imp.: $" + (precio * 0.79));
        System.out.println("Precio final: $" + precio);
        System.out.println("-----------------------");
    }
}
```

1-¿Hay código duplicado? Indique claramente en qué líneas se encuentra.
si, en lineas 16-20 y 30-34

Refcatoring pull up method -> primero hago extract method

```java
abstract class Etiqueta {
    protected String nombreProducto;
    protected double precio;

    public Etiqueta(String nombre, double precio) {
        this.nombreProducto = nombre;
        this.precio = precio;
    }
}

class EtiquetaSimple extends Etiqueta {
    public EtiquetaSimple(String nombre, double precio) {
        super(nombre, precio);
    }

    public void generarProducto(){
        System.out.println("Producto: " + nombreProducto);
    }

    public void generarLinea(){
        System.out.println("-----------------------");
    }

    public void generar() {
        System.out.println("--- ETIQUETA BÁSICA ---");
        this.generarProducto();
        System.out.println("Precio: $" + precio);
         this.generarLinea();
    }
}

class EtiquetaDetalle extends Etiqueta {
    public EtiquetaDetalle(String nombre, double precio) {
        super(nombre, precio);
    }

    public void generarProducto(){
            System.out.println("Producto: " + nombreProducto);
        }

    public void generarLinea(){
        System.out.println("-----------------------");
    }

    public void generar() {
        System.out.println("--- ETIQUETA DETALLE ---");
        this.generarProducto();
        System.out.println("Precio sin imp.: $" + (precio * 0.79));
        System.out.println("Precio final: $" + precio);
        this.generarLinea();
    }
}
```

```java
abstract class Etiqueta {
    protected String nombreProducto;
    protected double precio;

    public Etiqueta(String nombre, double precio) {
        this.nombreProducto = nombre;
        this.precio = precio;
    }

    public void generarProducto(){
            System.out.println("Producto: " + nombreProducto);
        }

    public void generarLinea(){
        System.out.println("-----------------------");
    }
}

class EtiquetaSimple extends Etiqueta {
    public EtiquetaSimple(String nombre, double precio) {
        super(nombre, precio);
    }
    public void generar() {
        System.out.println("--- ETIQUETA BÁSICA ---");
        this.generarProducto();
        System.out.println("Precio: $" + precio);
         this.generarLinea();
    }
}

class EtiquetaDetalle extends Etiqueta {
    public EtiquetaDetalle(String nombre, double precio) {
        super(nombre, precio);
    }
    public void generar() {
        System.out.println("--- ETIQUETA DETALLE ---");
        this.generarProducto();
        System.out.println("Precio sin imp.: $" + (precio * 0.79));
        System.out.println("Precio final: $" + precio);
        this.generarLinea();
    }
}
```
