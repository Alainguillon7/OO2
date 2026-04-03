3.1
```java
public class CharRing {
    private char[] source;
    private int idx;

    public CharRing(String src) {
        source = src.toCharArray();
        idx = 0;
    }

    public char next() {
        if (idx >= source.length)
            idx = 0;
        return source[idx++];
    }
}

public class IntRing {
    private int[] source;
    private int idx;

    public IntRing(int[] src) {
        source = src;
        idx = 0;
    }

    public int next() {
        if (idx >= source.length)
            idx = 0;
        return source[idx++];
    }
}
```
TESTS
```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import org.junit.jupiter.api.Test;

public class CharRingTest {

    @Test
    public void testNextRecorreCircularmenteElArreglo() {
        CharRing ring = new CharRing("abc");

        assertEquals('a', ring.next());
        assertEquals('b', ring.next());
        assertEquals('c', ring.next());
        assertEquals('a', ring.next());
        assertEquals('b', ring.next());
        assertEquals('c', ring.next());
    }
}
```
```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import org.junit.jupiter.api.Test;

public class IntRingTest {

    @Test
    public void testNextRecorreCircularmenteElArreglo() {
        IntRing ring = new IntRing(new int[]{1, 2, 3});

        assertEquals(1, ring.next());
        assertEquals(2, ring.next());
        assertEquals(3, ring.next());
        assertEquals(1, ring.next());
        assertEquals(2, ring.next());
        assertEquals(3, ring.next());
    }
}
```
---
3.2
REFACTORINGS
TIPO T???
```java

public abstract class Ring<T>{
    protected T[] source;
    protected int[] idx;

    public Ring(T[] source) {
        this.source = source;
        this.idx = 0;
    }

    public T next() {
        if (idx >= source.length)
            idx = 0;
        return source[idx++];
    }
}
