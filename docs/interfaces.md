# Interfaces

*Interfaces* sind auch abstrakte Klassen. Interfaces enthalten ausschließlich abstrakte Methoden (keine Methode darf implementiert sein). Interfaces beschreiben **Schnittstellen**. Für Interfaces wird nicht das Schlüsselwort `class`, sondern `interface` verwendet. Klassen erben nicht von Interfaces, sondern **implementieren** sie. Deshalb wird auch nicht das Schlüsselwort `extends`, sondern das Schlüsselwort `implements` verwendet. Während in Java nur von genau einer Klasse geerbt werden kann (also auch nur von genau einer abstrakten Klasse), kann eine Klasse kann beliebig viele Interfaces implementieren. 

Interfaces sind automatisch `abstract`, d.h. das Schlüsselwort `abstract` muss nicht angegeben werden. Auch die Methoden in Interfaces müssen nicht als abstrakt gekennzeichnet werden. Interfaces können, wie abstrakte Klassen auch, als Typen verwendet werden. 

| Abtrakte Klasse | Interface |
|-----------------|-----------|
|können abstrakte und nicht-abstrakte (also implementierte) Methoden haben |können nur abstrakte Methoden beinhalten |
|es kann nur von einer (abstrakten) Klasse geerbt werden (Schlüsselwort `extends`) |es können beliebig viele Interfaces implementiert werden (Schlüsselwort `implemenets`), mehrere Interfaces durch Komma getrennt |
|abstrakte Klassen können selbst Interfaces implementieren |Interfaces können keine abstrakten Klassen implementieren (alle Methoden müssen ja abstrakt sein) |
|das Schlüsselwort `abstract` deklariert eine abstrakte Klasse (und eine abstrakte Methode) |das Schlüsselwort `interface` deklariert ein Interface |
|eine abstrakte Klasse kann von einer anderen abstrakten Klasse erben und mehrere Interfaces implementieren |ein Interface kann nur von *einem* anderen Interface erben |
|abtrakte Klassen können `final` Variablen (Konstanten), nicht-finale Variablen, statische und nicht-statische Variablen als Eigenschaften beinhalten |Interfaces können nur statische Konstanten (`static final`) als Eigenschaften beinhalten |
|die Eigenschaften einer abstrakten Klasse können `private`, `protected`, *default* und `public` sein |in Interfaces sind alle Eigenschaften `public` |
|Bsp.: `public abstract class Shape{ public abstract void draw(); }` |Bsp.: `public interface Drawable{ void draw(); }` | 


## Das Interface `Comparable`

Ehe wir uns ein eigenes Interface schreiben, schauen wir uns zunächst die Verwendung eines bereits existierenden Interfaces an. Es handelt sich um das Interface [Comparable](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Comparable.html) aus dem `java.lang`-Paket. Wenn Sie sich die Java-Dokumentation dieses Interfaces einmal anschauen, dann sehen Sie, dass es von sehr vielen Klassen implementiert wird. Dieses Interface enthält genau eine (natürlich abstrakte) Methode `compareTo()`. Diese Methode kennen wir auch schon, denn wir haben sie betrachtet, als wir [in Prog1 Strings](https://freiheit.f4.htw-berlin.de/prog1/hilfsklassen/#die-klasse-string) kennengelernt haben. 

Die Methode `this.compareTo(Object obj)` wird verwendet, um zu vergleichen, ob `this` größer, kleiner oder gleich `obj` ist. Das bedeutet, dass wir `compareTo()` in unserer Klasse implementieren sollten, wenn wir die Objekte unserer Klasse der Größe nach ordnen wollen, wenn wir also ermöglichen wollen, dass die Objekte der Klasse sortiert werden können. 

Die Methode `this.compareTo(Object obj)` gibt ein `int` zurück, für dessen Wert Folgendes gelten soll:

- ist der zurückgegebene `int`-Wert positiv (`> 0`), dann ist `this` **größer** als `obj`,
- ist der zurückgegebene `int`-Wert negativ (`< 0`), dann ist `this` **kleiner** als `obj`,
- ist der zurückgegebene `int`-Wert `0`, dann ist `this` **gleich** `obj`.

Angenommen, wir wollen für die folgende Klasse `Rectangle` (aus dem Abschnitt [Abstrakte Klassen](../abstrakt/#rectangle-erbt-von-shape)) festlegen, dass die Rechtecke der Größe nach geordnet werden können. Gegeben ist also zunächst folgende Klasse (wir verwenden hier auch `Shape` aus [Abstrakte Klassen](../abstrakt/#ein-beispiel-die-abstrakte-klasse-shape)):

```java linenums="1"
public class Rectangle extends Shape
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}
}
```

Die Klasse `Rectangle` erbt also von der abstrakten Klasse `Shape` und `Rectangle` **muss** deshalb die Methoden `perimeter()` und `area()` implementieren. Nun geben wir an, dass `Rectangle` auch das Interface `Comparable` implementieren soll. Dazu ergänzen wir die erste Zeile um `implements Comparable`, d.h. die Klassendeklaration sieht jetzt so aus:

```java linenums="1"
public class Rectangle extends Shape implements Comparable
{
```

Wenn Sie das hinzufügen, stellen wir fest, dass ein **Fehler** erzeugt wird (die Klasse lässt sich nicht compilieren). Die Fehlerausgabe besagt: `The type Rectangle must implement the inherited abstract method Comparable.compareTo(Object)`. Es werden zwei `QuickFixes` angeboten, 

- entweder `Add unimplemented methods` 
- oder `Make type Rectangle abstract`. 

Letzteres wollen wir aber nicht (`Rectangle` soll nicht zu einer abstrakten Klasse gemacht werden). Also wählen wir `Add unimplemented methods`. Eclipse fügt uns die `compareTo()`-Methode in den Code ein:

```java linenums="1" hl_lines="23-27"
public class Rectangle extends Shape implements Comparable
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}

	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

Jetzt lässt sich der Code bereits compilieren, wir erhalten aber noch eine Warnung:

```bash
Comparable is a raw type. References to generic type Comparable<T> should be parameterized
``` 

Diese Warnung besagt, dass wir, wie wir das von Collections bereits kennen, auch das Interface `Comparable` **typisieren** sollen. Das wollen wir auch tun, denn wir implementieren dieses Interface hier für unsere Klasse `Rectangle`. Wir typisieren deshalb `Comparable` mit `Rectangle`: 


```java linenums="1" hl_lines="1"
public class Rectangle extends Shape implements Comparable<Rectangle>
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}

	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

Interssanterweise ist nun zwar unsere Warnung weg, aber dafür erhalten wir erneut einen Fehler:

```bash
The type Rectangle must implement the inherited abstract method Comparable<Rectangle>.compareTo(Rectangle)
```

Dadurch, dass wir `Comparable` mit `Rectangle` typisieren (was korrekt ist), wird nun verlangt, dass wir nicht mehr die Methode 

```java 
	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		return 0;
	}
```

implementieren, sondern die Methode 

```java 
	@Override
	public int compareTo(Rectangle o) {
		// TODO Auto-generated method stub
		return 0;
	}
```

Der Typ des Parameters hat sich durch unsere Typisierung also geändert. Das ist gut, denn dann müssen wir nicht mehr, wie z.B. bei `equals(Object o)`, prüfen, ob es sich bei dem übergebenen Objekt tatsächlich um ein `Rectangle` handelt. Wir ändern also den Parametertyp in `compareTo()`:

```java linenums="1" hl_lines="24"
public class Rectangle extends Shape implements Comparable<Rectangle>
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}

	@Override
	public int compareTo(Rectangle o) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

In Zukunft typisieren wir das `Comparable`-Interface noch bevor wir `Add unimplemented methods` wählen. Wir typisieren es stets mit der Klasse, in der wir das Interface implementieren. 

Für die Implementierung müssen wir uns nun überlegen, wann ein `Rectangle`-Objekt größer (kleiner/gleich) sein soll, als ein anderes. Da `compareTo()` ein `int` zurückgibt, könnten wir z.B. die Summen von `height` und `width` verwenden:

```java linenums="23" 
	@Override
	public int compareTo(Rectangle o) {
		int diff = (this.height+this.width) - (o.height+o.width);
		return diff;
	}
```

Wenn die Summe von `height` und `width` von `this` größer ist, als von `o`, dann geben wir eine positive `int`-Zahl zurück, wenn sie kleiner ist, dann eine negative `int`-Zahl und wenn sie gleich sind, dann `0`. Damit entsprechen wir den Vorgaben von `compareTo()`. 






