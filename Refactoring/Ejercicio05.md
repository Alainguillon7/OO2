5.1

```java
HotelStay.java

public class HotelStay extends Product {
    public double cost;
    private TimePeriod timePeriod;
    private Hotel hotel;

    public HotelStay(double cost, TimePeriod timePeriod, Hotel hotel) {
        this.cost = cost;
        this.timePeriod = timePeriod;
        this.hotel = hotel;
    }

    public LocalDate startDate() {
        return this.timePeriod.start();
    }

    public LocalDate endDate() {
        return this.timePeriod.end();
    }

    public double priceFactor() {
        return this.cost / this.price();
   }

    public double price() {
        return this.timePeriod.duration() * this.hotel.nightPrice() * this.hotel.discountRate();
    }
}

CarRental.java

public class CarRental extends Product {
    public double cost;
    private TimePeriod timePeriod;
    private Company company;

  public CarRental(double cost, TimePeriod timePeriod, Company company) {
        this.cost = cost;
        this.timePeriod = timePeriod;
        this.company = company;
    }

    public LocalDate startDate() {
        return this.timePeriod.start();
    }

    public LocalDate endDate() {
        return this.timePeriod.end();
    }

    public double price() {
        return this.company.price() * this.company.promotionRate();
    }

    public double cost() {
        return this.cost;
    }
}
```

1 La variable “cost” está declarada como pública, lo que rompe el encapsulamiento de la clase. Utilice el refactoring Encapsulate Field y describa brevemente los pasos llevados a cabo. Verifique que los tests provistos sigan funcionando. Discuta con un ayudante: 
¿Es correcto modificar alguno de los tests para que el código refactorizado funcione?
 En caso de qué el test falle, ¿qué situación está representando este test?

```java
package ar.edu.info.unlp.refactoring.ejercicio1;

import java.time.LocalDate;

public class CarRental extends Product {
    private double cost;
    private TimePeriod timePeriod;
    private Company company;

    public CarRental(double cost, TimePeriod timePeriod, Company company) {
        this.cost = cost;
        this.timePeriod = timePeriod;
        this.company = company;
    }
    
    public double getCost() {
		return cost;
	}

    public LocalDate startDate() {
        return this.timePeriod.start();
    }

    public LocalDate endDate() {
        return this.timePeriod.end();
    }

    public double price() {
        return this.company.price() * this.company.promotionRate();
    }
}

package ar.edu.info.unlp.refactoring.ejercicio1;

import java.time.LocalDate;

public class HotelStay extends Product {
    private double cost;
    private TimePeriod timePeriod;
    private Hotel hotel;

    public HotelStay(double cost, TimePeriod timePeriod, Hotel hotel) {
        this.cost = cost;
        this.timePeriod = timePeriod;
        this.hotel = hotel;
    }
    
    

    public double getCost() {
		return cost;
	}



	public LocalDate startDate() {
        return this.timePeriod.start();
    }

    public LocalDate endDate() {
        return this.timePeriod.end();
    }

    public double priceFactor() {
        return this.cost / this.price();
    }

    public double price() {
        return this.timePeriod.duration() * this.hotel.nightPrice() * this.hotel.discountRate();
    }
}
```

2 Utilice el refactoring Rename Field en el método priceFactor(), para que la variable “cost” se pase a llamar 
“quote”. Verifique que los tests provistos sigan funcionando. Discuta con un ayudante: ¿en este caso, 
es necesario modificar alguno de los tests para que el código refactorizado funcione?

```java
package ar.edu.info.unlp.refactoring.ejercicio1;

import java.time.LocalDate;

public class HotelStay extends Product {
    private double quote;
    private TimePeriod timePeriod;
    private Hotel hotel;

    public HotelStay(double cost, TimePeriod timePeriod, Hotel hotel) {
        this.quote = cost;
        this.timePeriod = timePeriod;
        this.hotel = hotel;
    }
    
    

    public double getCost() {
		return this.quote;
	}


	public LocalDate startDate() {
        return this.timePeriod.start();
    }

    public LocalDate endDate() {
        return this.timePeriod.end();
    }

    public double priceFactor() {
        return this.quote / this.price();
    }

    public double price() {
        return this.timePeriod.duration() * this.hotel.nightPrice() * this.hotel.discountRate();
    }
}
```

Se quiere aplicar el refactoring Pull Up Method para subir los métodos startDate() y endDate() a la superclase
Product. ¿Es posible hacerlo en el código anterior? Justifique su respuesta basándose en las precondiciones
del refactoring vistas en la teoría y en el libro de Refactoring de Martin Fowler.

Existe el bad smell de codigo repetido en ambas subclases de Prodcut asi q se decide aplicar el refactoring 
de Pull up method e implicitamnete se hace pull up field para tener a disposicion los atrtibutos que utiliza
el metodo del primer refactoring

```java

public abstract class Product {
	protected TimePeriod timePeriod;
	
	public LocalDate startDate() {
        return this.timePeriod.start();
    }

    public LocalDate endDate() {
        return this.timePeriod.end();
    }
}

public class HotelStay extends Product {
    private double quote;
    private Hotel hotel;

    public HotelStay(double cost, TimePeriod timePeriod, Hotel hotel) {
        this.quote = cost;
        this.timePeriod = timePeriod;
        this.hotel = hotel;
    }
    
    

    public double getCost() {
		return this.quote;
	}


    public double priceFactor() {
        return this.quote / this.price();
    }

    public double price() {
        return this.timePeriod.duration() * this.hotel.nightPrice() * this.hotel.discountRate();
    }
}

public class CarRental extends Product {
    private double cost;
    private Company company;

    public CarRental(double cost, TimePeriod timePeriod, Company company) {
        this.cost = cost;
        this.timePeriod = timePeriod;
        this.company = company;
    }
    
    public double getCost() {
		return cost;
	}

    public double price() {
        return this.company.price() * this.company.promotionRate();
    }

}
```
6
Observe los métodos price() en CarRental.java (línea 21) y en HotelStay.java (línea 25).
Identifique los Code Smell presentes. Luego aplique los refactoring correspondientes para eliminarlos.
Verifique que los tests provistos sigan funcionando.

En price() existe el bad smell feature envy y se debe aplicar el refctoring move method

```java
public class Company {
    private final double price;
    private final double promotionRate;

    public Company(double price, double promotionRate) {
        this.price = price;
        this.promotionRate = promotionRate;
    }

    public double price() {
        return this.price;
    }

    public double promotionRate() {
        return this.promotionRate;
    }
    
    public double calculatePrice() {
    	return (this.price * this.promotionRate());
    }
}


public class CarRental extends Product {
    private double cost;
    private Company company;

    public CarRental(double cost, TimePeriod timePeriod, Company company) {
        this.cost = cost;
        this.timePeriod = timePeriod;
        this.company = company;
    }
    
    public double getCost() {
		return cost;
	}

    public double price() {
        return this.company.calculatePrice();

    }
}
```

En HotelStay.java se ve feature envy y se aplica el Refactoring move method y luego dentro
de la clase hotel se genera el bad smell de metodo no usado asi que eaplica el elimnar metodo.

```java

package ar.edu.info.unlp.refactoring.ejercicio1;

public class Hotel {
    private double nightPrice;
    private double discountRate;

    public Hotel(double nightPrice, double discountRate) {
        this.nightPrice = nightPrice;
        this.discountRate = discountRate;
    }

    public double getPrecio() {
        return (this.discountRate * this.nightPrice);
    }
}


public class HotelStay extends Product {
    private double quote;
    private Hotel hotel;

    public HotelStay(double cost, TimePeriod timePeriod, Hotel hotel) {
        this.quote = cost;
        this.timePeriod = timePeriod;
        this.hotel = hotel;
    }

    public double getCost() {
		return this.quote;
	}


    public double priceFactor() {
        return this.quote / this.price();
    }

    public double price() {
        return this.timePeriod.duration() * this.hotel.getPrecio();
    }
}
```
