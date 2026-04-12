```java
public class Document {
    List<String> words;
  
    public long characterCount() {
 	long count = this.words
.stream()
.mapToLong(w -> w.length())
.sum();
    	return count;
	}
    public long calculateAvg() {
    	long avgLength = this.words
.stream()
.mapToLong(w -> w.length())
.sum() / this.words.size();
 	return avgLength;
	}
// Resto del código que no importa
}

```
Bad smell codigo duplicado
Refactoring extract method

```java
public class Document {
    List<String> words;
  
    public long characterCount() {
 	long count = this.words
.stream()
.mapToLong(w -> w.length())
.sum();
    	return count;
	}
    public long calculateAvg() {
    	return this.characterCount() / this.words.size();
	}
// Resto del código que no importa
}

```
