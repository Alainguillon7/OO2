4.1
```java
Publicacion.java

package redesocial;
public class Publicacion {
   private String texto;
   private int likes;
   public Publicacion(String texto) {
      this.texto = texto;
      this.likes = 0;
   }
   public void darLike() { likes++; }
   public void darDislike() { likes--; }
   private int procesar() {
       return likes * 3;
   }
   public int calcular() {
       return procesar() * 10;
   }
}

Perfil.java

package redesocial;
import java.util.ArrayList;
public class Perfil {
   private boolean verificado;
   private ArrayList<Publicacion> publicaciones;
   public Perfil(boolean verificado) {
        this.verificado = verificado;
        this.publicaciones = new ArrayList<>();
   }
   public void agregarPublicacion(Publicacion p) { publicaciones.add(p); }
   private int bonus() { return verificado ? 2 : 1; }
   private int alcanceDePublicaciones() {
       return publicaciones.stream().mapToInt(p -> p.calcular()).sum();
   }
   public int calcular() {
       return alcanceDePublicaciones() * bonus();
   }
}

```

Rename method: procesar (referenciado en línea 11 de Publicacion.java) por impacto
Rename method: calcular (referenciado en línea 14 de Publicacion.java) por alcance
Rename method: calcular (referenciado en línea 15 de Perfil.java) por alcance
Rename parameter: el parámetro “p” del método agregarPublicacion (línea 10 de Perfil.java) por “publicacion”

```java
Publicacion.java

package redesocial;
public class Publicacion {
   private String texto;
   private int likes;
   public Publicacion(String texto) {
      this.texto = texto;
      this.likes = 0;
   }
   public void darLike() { likes++; }
   public void darDislike() { likes--; }
   private int impacto() {
       return likes * 3;
   }
   public int alcance() {
       return impacto() * 10;
   }
}

Perfil.java

package redesocial;
import java.util.ArrayList;
public class Perfil {
   private boolean verificado;
   private ArrayList<Publicacion> publicaciones;
   public Perfil(boolean verificado) {
        this.verificado = verificado;
        this.publicaciones = new ArrayList<>();
   }
   public void agregarPublicacion(Publicacion publicacion) { publicaciones.add(p); }
   private int bonus() { return verificado ? 2 : 1; }
   private int alcanceDePublicaciones() {
       return publicaciones.stream().mapToInt(p -> p.calcular()).sum();
   }
   public int alcance() {
       return alcanceDePublicaciones() * bonus();
   }
}

```
