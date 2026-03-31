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

---

1.2  

Bad smell: future envy  
Refactoring: move method  

---

1.3   
```java
public void imprimirValores() {
	int totalEdades = 0;
	double promedioEdades = 0;
	double totalSalarios = 0;
	
	for (Empleado empleado : personal) {
		totalEdades = totalEdades + empleado.getEdad();
		totalSalarios = totalSalarios + empleado.getSalario();
	}
	promedioEdades = totalEdades / personal.size();
		
	String message = String.format("El promedio de las edades es %s y el total de salarios es %s",        promedioEdades, totalSalarios);
	
	System.out.println(message);
}
```

Bad smells:  
long method  
reinventa la rueda  
Temporary Variable 

Bad smell: reinventa la rueda  
Refactoring: reeplace loop with pipeline  

```java
public void imprimirValores() {
	int totalEdades = 0;
	double promedioEdades = 0;
	double totalSalarios = 0;
	
	for (Empleado empleado : personal) {
		totalSalarios = totalSalarios + empleado.getSalario();
	}
    totalEdades = this.personal.stream().mapToInt(p->p.getEdad()).average().orElse(0);
		
	String message = String.format("El promedio de las edades es %s y el total de salarios es %s",        promedioEdades, totalSalarios);
	
	System.out.println(message);
}
```

Bad smell: reinventa la rueda  
Refactoring: reeplace loop with pipeline  

```java
public void imprimirValores() {
	int totalEdades = 0;
	double promedioEdades = 0;
	double totalSalarios = 0;
	
  totalSalarios = this.personal.stream().mapToDouble(p->p.getSalario()).sum();
  promedioEdades = this.personal.stream().mapToInt(p->p.getEdad()).average().orElse(0);
		
	String message = String.format("El promedio de las edades es %s y el total de salarios es %s",        promedioEdades, totalSalarios);
	
	System.out.println(message);
}
```

Bad smell: Temporary Variable
Refactoring: eliminar variables no usadas o mal declaradas

```java
public void imprimirValores() {	
  totalSalarios = this.personal.stream().mapToDouble(p->p.getSalario()).sum()
  promedioEdades = this.personal.stream().mapToInt(p->p.getEdad()).sum().average().orElse(0);
		
	String message = String.format("El promedio de las edades es %s y el total de salarios es %s",        promedioEdades, totalSalarios);
	
	System.out.println(message);
}
```

Bad smell: Long Method
Refactoring: Extract Method
> Duda: Se pueden quitar ambos a la vez?
>       Metodos nuevos privados?

```java
public void imprimirValores() {	
  totalSalarios = this.calcularTotalSalarios
  promedioEdades = this.calcularPromedioEdades
		
	String message = String.format("El promedio de las edades es %s y el total de salarios es %s",        promedioEdades, totalSalarios);
	
	System.out.println(message);
}

private double calcularPromedioEdades(){
return  this.personal.stream().mapToInt(p->p.getEdad()).average().orElse(0);
}

private double calcularTotalSalarios(){
return this.personal.stream().mapToDouble(p->p.getSalario()).sum()
}
```

Bad smell: Long Method
Refactoring: Extract Method

```java
public void imprimirValores() {	
	System.out.println(String.format("El promedio de las edades es %s y el total de salarios es %s",this.calcularPromedioEdades, this.calcularTotalSalarios););
}

private double calcularPromedioEdades(){
return  this.personal.stream().mapToInt(p->p.getEdad()).average().orElse(0);
}

private double calcularTotalSalarios(){
return this.personal.stream().mapToDouble(p->p.getSalario()).sum()
}
```
