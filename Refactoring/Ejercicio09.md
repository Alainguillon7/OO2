```java
public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private String formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, String formaPago) {
        if (!"efectivo".equals(formaPago)
                && !"6 cuotas".equals(formaPago)
                && !"12 cuotas".equals(formaPago)) {
            throw new Error("Forma de pago incorrecta");
        }
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = 0;
        for (Producto producto : this.productos) {
            costoProductos += producto.getPrecio();
        }

        double extraFormaPago = 0;
        if ("efectivo".equals(this.formaPago)) {
            extraFormaPago = 0;
        } else if ("6 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.2;
        } else if ("12 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.5;
        }

        int aniosDesdeFechaAlta = Period.between(this.cliente.getFechaAlta(), LocalDate.now()).getYears();

        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        if (aniosDesdeFechaAlta > 5) {
            return (costoProductos + extraFormaPago) * 0.9;
        }

        return costoProductos + extraFormaPago;
    }
}

public class Cliente {
    private LocalDate fechaAlta;

    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
}

public class Producto {
    private double precio;

    public double getPrecio() {
        return this.precio;
    }
}
```

1-Dado e	l código anterior, aplique únicamente los siguientes refactoring:
Replace Loop with Pipeline (líneas 16 a 19)
Replace Conditional with Polymorphism (líneas 21 a 27)
Extract method y move method (línea 28)
Extract method y replace temp with query (líneas 28 a 33)

Replace Loop with Pipeline (líneas 16 a 19)
```java
public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private FormaPago formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, FormaPago formaPago) {
        ithis.formaPago = formaPago
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = this.productos.stream()
          .mapToDouble (p -> p.getPrecio())
          .sum();

        double extraFormaPago = 0;
        if ("efectivo".equals(this.formaPago)) {
            extraFormaPago = 0;
        } else if ("6 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.2;
        } else if ("12 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.5;
        }

        int aniosDesdeFechaAlta = Period.between(this.cliente.getFechaAlta(), LocalDate.now()).getYears();

        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        if (aniosDesdeFechaAlta > 5) {
            return (costoProductos + extraFormaPago) * 0.9;
        }

        return costoProductos + extraFormaPago;
    }
}

public class Cliente {
    private LocalDate fechaAlta;

    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
}

public class Producto {
    private double precio;

    public double getPrecio() {
        return this.precio;
    }
}
```
Replace Conditional with Polymorphism (líneas 21 a 27)

```java
public interface FormaPago{
        public double calcularExtra(double costoProductos);
    }

    public Efectivo implements FormaPago{
        public double clacularExtra(double costoProductos){
            return 0;
        }
    }

    public SeisCuotas implements FormaPago{
        public double clacularExtra(double costoProductos){
            return costoProductos * 0.2;
         }
     }

    public DoceCuotas implements FormaPago{
        public double clacularExtra(double costoProductos){
            return costoProductos * 0.5;
         }
     } 

public class Pedido {
   private Cliente cliente;
    private List<Producto> productos;
    private FormaPago formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, FormaPago formaPago) {
        ithis.formaPago = formaPago
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }
        public double getCostoTotal() {
          double costoProductos = this.productos.stream()
            .mapToDouble (p -> p.getPrecio())
            .sum();
  
          double extraFormaPago = formaPago.clacularExtra(costoProductos);
  
          int aniosDesdeFechaAlta = Period.between(this.cliente.getFechaAlta(), LocalDate.now()).getYears();
  
          // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
          if (aniosDesdeFechaAlta > 5) {
              return (costoProductos + extraFormaPago) * 0.9;
          }
  
          return costoProductos + extraFormaPago;
      }
}

public class Cliente {
    private LocalDate fechaAlta;

    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
}

public class Producto {
    private double precio;

    public double getPrecio() {
        return this.precio;
    }
}
```

Extract method y move method (línea 28)


```java
public interface FormaPago{
        public double calcularExtra(double costoProductos);
    }

    public Efectivo implements FormaPago{
        public double clacularExtra(double costoProductos){
            return 0;
        }
    }

    public SeisCuotas implements FormaPago{
        public double clacularExtra(double costoProductos){
            return costoProductos * 0.2;
         }
     }

    public DoceCuotas implements FormaPago{
        public double clacularExtra(double costoProductos){
            return costoProductos * 0.5;
         }
     } 

public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private FormaPago formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, FormaPago formaPago) {
        ithis.formaPago = formaPago
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

        public double getCostoTotal() {
            double costoProductos = this.productos.stream()
              .mapToDouble (p -> p.getPrecio())
              .sum();
  
            double extraFormaPago = formaPago.clacularExtra(costoProductos);
            double total = costoProductos + extraFormaPago;
            return this.cliente.aplicarDescuento(total);
        }
}

public class Cliente {
    private LocalDate fechaAlta;

    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }

    public int anioDesdeAlta(){
        return Period.between(this.fechaAlta, LocalDate.now()).getYears();
    }

    public double aplicarDescuento(double monto) {
        if (this.anioDesdeAlta() > 5) {
            return monto * 0.9;
        }
        return monto;
    }
}

public class Producto {
    private double precio;

    public double getPrecio() {
        return this.precio;
    }
}
```
