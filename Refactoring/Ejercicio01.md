1.1
```java
/** 
* Retorna el límite de crédito del cliente
*/
public double lmtCrdt() {...

/** 
* Retorna el monto facturado al cliente desde la fecha f1 a la fecha f2
*/
protected double mtFcE(LocalDate f1, LocalDate f2) {...

/** 
* Retorna el monto cobrado al cliente desde la fecha f1 a la fecha f2
*/
private double mtCbE(LocalDate f1, LocalDate f2) {…
```

Bad smell :nombre poco explicativos en los metodos
Solucion: Rename method

```java
public double returnCreditLimit(){...

public double returnMontoFacturado(){...

public double returnMontoCobrado(){...
```
