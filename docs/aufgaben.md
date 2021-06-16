# Aufgaben

##### Aufgabe 1 (Würfelspiel)

??? "Aufgabe 1"

	**Vorbereitung (Selbstudium)**

	1. Informieren Sie sich über die Klasse `JOptionPane` aus dem Paket `javax.swing` (z.B. [hier](https://docs.oracle.com/javase/10/docs/api/javax/swing/JOptionPane.html) oder [hier](https://www.java-tutorial.org/joptionpane.html) oder [hier](https://de.wikibooks.org/wiki/Java_Standard:_Grafische_Oberfl%C3%A4chen_mit_Swing:_Top_Level_Container:_javax_swing_JOptionPane)) <br/>
	*Sollten Sie mit dem Java-Modulsystem arbeiten, d.h. sollten Sie in Ihrem Java-Projekt eine Datei `module-info.java` haben, dann müssen Sie in diese Datei (in den Anweisungsblock) die Anweisung `requires java.desktop;` einfügen - das ist das Modul, in dem sich das Paket `javax.swing` befindet.* 
	2. Rufen Sie jeweils die statischen Methoden 
		- `showConfirmDialog()`,
		- `showInputDialog()`,
		- `showMessageDialog()` und
		- `showOptionDialog()` 
		auf und werten Sie jeweils die Nutzereingabe aus (Bei `showInputDialog()` ist die Rückgabe ein `String`, ansonsten ist die Rückgabe ein `int`, der mit folgenden Optionen verglichen werden kann:
		- `JOptionPane.YES_OPTION`,
		- `JOptionPane.NO_OPTION`,
		- `JOptionPane.CANCEL_OPTION`,
		- `JOptionPane.OK_OPTION`,
		- `JOptionPane.CLOSED_OPTION`
	3. Erstellen Sie insbesondere folgenden Dialog und prüfen Sie, ob der `Nein`- oder der `Ja`-Button gedrückt wurde (im Beispiel steht `A` für den Namen eines Spielers – siehe Aufgabe unten):
		![aufgabe1](./files/19_aufgabe1.png)

	**Aufgabe**

	1. Implementieren Sie folgendes Würfelspiel:
		- An dem Spiel können beliebig viele Spieler teilnehmen.
		- Die Spieler sind nacheinander an der Reihe.
		- Wenn ein Spieler an der Reihe ist, dann befindet er sich in einem *Versuch*.
		- In einem *Versuch* kann der Spieler so lange würfeln, bis er entweder
			- eine 6 würfelt oder er
			- den Versuch freiwillig beendet.  
		- Hat der Spieler eine 6 gewürfelt, wird der gesamte *Versuch* mit `0` Punkten bewertet.
		- Hat der Spieler den Versuch freiwillig beendet, wird die in dem *Versuch* erzielte Summe aus sein Punktekonto addiert (gespeichert).
	2. Der Spieler, der zuerst eine bestimmte Punktzahl (z.B. `20`) erreicht hat, hat gewonnen. <br/>
		Beispiel mit zwei Spielern `A` und `B` bis Gesamtpunktzahl `20`:
		![aufgabe1](./files/20_aufgabe1.png)
	3. Committen und pushen Sie Ihre Lösung nach GitHub (oder Ihrem Git-Diensteanbieter).


??? question "eine mögliche Lösung für Aufgabe 1"
	
	=== "Spiel.java"
		```java linenums="1"
		package aufgaben.aufgabe1;

		import javax.swing.JOptionPane;

		public class Spiel {
			public static int siegPunkte;
			private Spieler[] spieler;
			
			Spiel()
			{
				int anzSpieler = 0;
				int siegPunkte = 0;
				boolean inputOk = false;
				String input = "";
				
				while(!inputOk)
				{
					try
					{
						input = JOptionPane.showInputDialog("Anzahl Spieler :");
						inputOk = true;
						anzSpieler = Integer.parseInt(input);
					}
					catch(NumberFormatException e)
					{
						inputOk = false;
					}
				}
				
				inputOk = false;
				
				while(!inputOk)
				{
					try
					{
						input = JOptionPane.showInputDialog("Siegpunkte :");
						inputOk = true;
						siegPunkte = Integer.parseInt(input);
					}
					catch(NumberFormatException e)
					{
						inputOk = false;
					}
				}
				
				Spiel.siegPunkte = siegPunkte;
				spieler = new Spieler[anzSpieler];
				
				namenErmitteln();
			}
			
			Spiel(int siegPunkte, int anzSpieler)
			{
				Spiel.siegPunkte = siegPunkte;
				spieler = new Spieler[anzSpieler];
				
				/* Spielernamen sind A, B, C ...
				char name = 'A';
				for(int i=0; i<spieler.length; i++)
				{
					spieler[i] = new Spieler(String.valueOf(name));
					name ++;
				}
				*/
				
				/* Spielernamen werden eingegeben
				 * wenn man namenErmitteln() auskommentiert,
				 * kann man die obige for-Schleife nehmen, damit
				 * man nicht immer die Spielernamen eingeben muss
				 * - zum Testen
				 */
				namenErmitteln();
			}
			
			private void namenErmitteln()
			{
				String name = "";
				for(int i=0; i<spieler.length; i++)
				{
					name = JOptionPane.showInputDialog("Name des Spielers " + (i+1)+" :");
					spieler[i] = new Spieler(name);
				}
			}
			
			public void spielen()
			{
				int index = 0;
				boolean gewonnen = false;
				while(!gewonnen)
				{
					gewonnen = spieler[index].wuerfeln();
					index = (index<spieler.length-1) ? index+1 : 0;
				}
			}
			
			public static void main(String[] args)
			{
				//Spiel spiel = new Spiel(30, 3);
				Spiel spiel = new Spiel();
				spiel.spielen();
			}
			
		}
		```
	
	=== "Spieler.java"
		```java linenums="1"
		package aufgaben.aufgabe1;

		import java.util.Random;

		import javax.swing.JOptionPane;

		public class Spieler {
			private String name;
			private int aktStand;
			private Random r;
			
			Spieler(String name)
			{
				aktStand=0;
				this.name = name;
				r = new Random();
			}
			
			public int getAktStand()
			{
				return this.aktStand;
			}
			
			public boolean wuerfeln()
			{
				
				System.out.printf("%n%nSpieler %s ist an der Reihe (bisher %d Punkte) %n ----------------------------------------- %n" , this.name, this.aktStand);
				int reihe = 0;
				int wurf = 0;
				boolean ende = false;
				while(!ende)
				{
					wurf = r.nextInt(6)+1;
					System.out.printf("%s hat eine %d gewuerfelt %n" , this.name, wurf);
					if(wurf==6)
					{
						System.out.printf("Versuch zu Ende %n Aktueller Spielstand von %s : %d Punkte %n Der naechste Spieler ist dran%n", this.name, this.aktStand);
						ende = true;
					}
					else if(aktStand+reihe+wurf >= Spiel.siegPunkte)
					{
						System.out.printf("%s hat insgesamt %d Punkte und somit gewonnen !!!", this.name, (aktStand+reihe+wurf));
						return true;
					}
					else	 // eine 1..5 gewuerfelt und noch nicht gewonnen
					{
						reihe += wurf;
						System.out.printf("in diesem Versuch bisher %d Punkte -- insgesamt %d Punkte %n", reihe, (aktStand+reihe));
						int dialogResult = JOptionPane.showConfirmDialog (null, this.name+ ", wollen Sie weiter wuerfeln? ", "Weiter wuerfeln?", JOptionPane.YES_NO_OPTION);
						ende = !(dialogResult==JOptionPane.YES_OPTION);
						if(ende)
						{
							aktStand += reihe;
						}
					}
				}
				return false;
			}
			
		}
		```


##### Aufgabe 2 (MyInteger)

??? "Aufgabe 2"

	**Vorbereitung (Selbstudium)**

	1. Eine *statische* Variable wird mit dem Schlüsselwort `static` deklariert, also z.B. `static int myVariable = 0;`. Der Zugriff auf eine solche statische Variable erfolgt nicht über eine Referenzvariable, sondern über den Klassennamen. Angenommen, die Variable `myVariable` wurde in der Klasse `MyClass` deklariert, dann erfolgt der Zugriff auf die Variable über `MyClass.myVariable`. Für Objektvariablen gilt, dass jedes Objekt seine eigene(n) Objektvariable(n) hat. Statische Variablen gibt es in der Klasse genau einmal. Alle Objekte "teilen" sich eine statische Variable. Eine statische Variable heißt deshalb auch *Klassenvariable*.
	2. Eine *statische* Methode wird ebenfalls mit dem Schlüsselwort `static` deklariert, also z.B. `public static void myMethod() {}`. Der Zugriff auf eine solche statische Methode erfolgt nicht über eine Referenzvariable, sondern über den Klassennamen. Angenommen, die Methode `myMethod()` wurde in der Klasse `MyClass` deklariert, dann erfolgt der Zugriff auf die Methode über `MyClass.myMethod()`. Wir kennen solche Methoden bereits, z.B. sind alle Methoden aus der Klasse `Math` statisch und wir können sie z.B. mit `Math.sqrt(value)` oder `Math.abs(value)` aufrufen. 
	3. siehe z.B. [hier](https://javabeginners.de/Grundlagen/Modifikatoren/static.php) oder [hier](https://www.programmierenlernenhq.de/statische-variablen-konstanten-und-methoden-in-java/) oder [hier](http://www.gailer-net.de/tutorials/java/Notes/chap25/ch25_12.html)

	**Info**: Die Klasse `MyInteger` ist eine sogenannte *Wrapper*-Klasse. Die Idee ist, dass `MyInteger` eine objektorientierte Hülle um den `int`-Typ darstellt. Implementieren Sie die Klasse `MyInteger`. Diese Klasse hat folgende Eigenschaften:

	1.	Statische Konstanten vom Typ `int` `MAX_VALUE` und `MIN_VALUE`, welche als Wert die größte bzw. kleinste `int`-Zahl enthalten.
	2.	Eine private Objektvariable `value` vom Typ `int`. (Dieser `value` hat jetzt eine "objektorientierte" Hülle: `MyInteger`).
	3.	Eine statische Methode `parseInt(String s)`, die den übergebenen `String s` als `int`-Zahl zurückgibt, wenn `s` einer Zahl entspricht. Wenn nicht, wirft die Methode eine `IllegalArgumentException`. Beachten Sie:
		a.	`s` kann mit `+` oder `–` beginnen,
		b.	`s` kann führende Nullen aufweisen,
		c.	die Länge von `s` kann mit `s.length()` ermittelt und jedes einzelne Zeichen aus `s` kann mit `s.charAt(index)` betrachtet werden. 
		d.	Ist `s` leer, wird eine `IllegalArgumentException` geworfen und wenn `s` keiner Zahl entspricht auch.
		e.	Die Exception wird nur weitergereicht, nicht hier behandelt.
	4.	Zwei Konstruktoren `MyInteger(int value)` und `MyInteger(String s)`, die jeweils die Objektvariable `value` initialisieren. Der zweite Konstruktor verwendet `parseInt(String)` und kann ebenfalls eine `IllegalArgumentException` werfen (reicht die Exception von `parseInt(String)` weiter). 
	5.	Eine Objektmethode `intValue()`, die den Wert von `value` zurückgibt.
	6.	Eine Objektmethode `doubleValue()`, die den Wert von `value` als `double` zurückgibt.
	7.	Eine statische Methode `valueOf(String s)`, die ein Objekt von `MyInteger` erzeugt und zurückgibt (und evtl. eine `IllegalArgumentException` wirft).
	8.	Eine statische Methode `valueOf(int value)`, die ein Objekt von `MyInteger` erzeugt und zurückgibt.
	9.	Überschreiben Sie außerdem die Methoden `equals()` und `toString()` (*Zusatz:*  auch `hashCode()` überschreiben).
	10. Testen Sie Ihre Klasse ausführlich in einer `Testklasse` mit `main()`-Methode.
	11. Committen und pushen Sie Ihre Lösung nach GitHub (oder Ihrem Git-Diensteanbieter).


??? question "eine mögliche Lösung für Aufgabe 2"
	
	=== "MyInteger.java"
		```java linenums="1"
		package aufgaben.aufgabe2;

		public class MyInteger
		{
			public static final int MAX_VALUE = 2147483647;
			public static final int MIN_VALUE = -2147483648;
			
			private int value;
			
			public MyInteger(int value)
			{
				this.value=value;
			}
			
			public MyInteger(String s) throws IllegalArgumentException
			{
				this.value = parseInt(s);
			}
			
			private static boolean isDigit(char c)
			{
				return (c=='0' || c=='1' || c=='2' || c=='3' || c=='4' || c=='5' ||
						c=='6' || c=='7' || c=='8' || c=='9');
			}
			
			private static int charToInt(char c)
			{
				int asciivalue = c;
				int intvalue = asciivalue-48; // 0 ist 048 bis 9 ist 057
				return intvalue;
			}
			
			public static int parseInt(String s) throws IllegalArgumentException
			{
				boolean negativ = false;
				if(s.length()==0) throw new IllegalArgumentException("leer");
				// pruefe, ob erstes Zeichen + oder -
				// merken und weiter mit Rest
				if(s.charAt(0)=='+') s = s.substring(1);
				else if(s.charAt(0)=='-')
				{
					s = s.substring(1);
					negativ = true;
				}
				if(s.length()==0) throw new IllegalArgumentException("nur '+' bzw. '-' --> keine Zahl");
				// entferne führende Nullen
				while(s.charAt(0)=='0')
				{
					s = s.substring(1);
				}
				for(int i=0; i<s.length(); i++)
				{
					if(!isDigit(s.charAt(i))) throw new IllegalArgumentException("keine Zahl!");
				}
				int exponent = 0;
				int zahl = 0;
				for(int i = s.length()-1; i>=0; i--)
				{
					zahl = zahl + charToInt(s.charAt(i))*(int)Math.pow(10, exponent);
					exponent++;
				}
				if(negativ) return -zahl;
				else return zahl;
			}
			
			public int intValue()
			{
				return this.value;
			}
			
			public double doubleValue()
			{
				return this.value;
			}
			
			public static MyInteger valueOf(String s) throws IllegalArgumentException
			{
				return new MyInteger(s);
			}
			
			public static MyInteger valueOf(int value)
			{
				return new MyInteger(value);
			}
			
			@Override
			public boolean equals(Object other)
			{
				if(other == null) return false;
				if(this == other) return true; 
				if(this.getClass() != other.getClass())	return false; 	

				MyInteger otherInt = (MyInteger)other; 	
				return (this.value == otherInt.value); 
			}
			
			@Override
			public int hashCode()
			{
				return this.value;
			}
			
			@Override
			public String toString()
			{
				return value+"";
			}
			
			public static int compare(int x, int y)
			{
				return (x < y) ? -1 : ((x == y) ? 0 : 1);
			}
			
			public int compareTo(MyInteger otherMyInteger)
			{
				return compare(this.value, otherMyInteger.value);
			}
		}
		```
	
	=== "MyIntegerMain.java"
		```java linenums="1"
		package aufgaben.aufgabe2;

		public class MyIntegerMain
		{

			public static void main(String[] args)
			{
				MyInteger 	mi1=null, mi2=null, mi3=null, mi4=null, 
							mi5=null, mi6=null, mi7=null, mi8=null,
							mi9=null, mi10=null;
				try {
					mi1 = new MyInteger("-2147483648");
					System.out.println(mi1.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				try {
					mi2 = new MyInteger("-00002147483648");
					System.out.println(mi2.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				try {
					mi3 = new MyInteger("hallo");
					System.out.println(mi3.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				mi4 = new MyInteger(-2147483648);
				try {
					mi5 = MyInteger.valueOf("-2147483648");
					System.out.println(mi5.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				try {
					mi6 = MyInteger.valueOf("-00002147483648");
					System.out.println(mi6.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				try {
					mi7 = MyInteger.valueOf("hallo");
					System.out.println(mi7.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				mi8 = MyInteger.valueOf(-2147483648);
				try {
					mi9 = MyInteger.valueOf("");
					System.out.println(mi9.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				try {
					mi10 = MyInteger.valueOf("+");
					System.out.println(mi10.intValue());
				}
				catch(IllegalArgumentException e)
				{
					System.out.println(e.getMessage());
				}
				System.out.println("mi1 equals mi2 ? : " + mi1.equals(mi2));
				System.out.println("mi1 equals mi3 ? : " + mi1.equals(mi3));
				System.out.println("mi1 equals mi4 ? : " + mi1.equals(mi4));
				System.out.println("mi1 equals mi5 ? : " + mi1.equals(mi5));
				System.out.println("mi1 equals mi6 ? : " + mi1.equals(mi6));
				System.out.println("mi1 equals mi7 ? : " + mi1.equals(mi7));
				System.out.println("mi1 equals mi8 ? : " + mi1.equals(mi8));
				System.out.println("mi1 equals mi9 ? : " + mi1.equals(mi9));
				System.out.println("mi1 equals mi10 ? : " + mi1.equals(mi10));
				
				System.out.println("hashCode mi1 : " + mi1.hashCode());
				System.out.println("hashCode mi2 : " + mi2.hashCode());
				try {
					System.out.println("hashCode mi3 : " + mi3.hashCode());
				}
				catch(NullPointerException e)
				{
					System.out.println("Objekt existiert nicht!");
				}
				System.out.println("hashCode mi4 : " + mi4.hashCode());
				
				System.out.println("intValue mi1 : " + mi1.intValue());
				System.out.println("intValue mi2 : " + mi2.intValue());
				try {
					System.out.println("intValue mi3 : " + mi3.intValue());
				}
				catch(NullPointerException e)
				{
					System.out.println("Objekt existiert nicht!");
				}
				System.out.println("intValue mi4 : " + mi4.intValue());
				
				System.out.println("doubleValue mi1 : " + mi1.doubleValue());
				System.out.println("doubleValue mi2 : " + mi2.doubleValue());
				try {
					System.out.println("doubleValue mi3 : " + mi3.doubleValue());
				}
				catch(NullPointerException e)
				{
					System.out.println("Objekt existiert nicht!");
				}
				System.out.println("doubleValue mi4 : " + mi4.doubleValue());
				
			}

		}
		```
	
	=== "MyIntegerTest.java"
		```java linenums="1"
		package aufgaben.aufgabe2;

		import static org.junit.jupiter.api.Assertions.*;

		import org.junit.jupiter.api.BeforeAll;
		import org.junit.jupiter.api.Test;

		class MyIntegerTest
		{
			static MyInteger mi1, mi2, mi3, mi4, mi5, mi6, mi7;
			
			@BeforeAll
			public static void setUpBeforeClass() throws Exception 
			{
				mi1 = new MyInteger("-2147483648");
				mi2 = new MyInteger("+2147483647");
				mi3 = new MyInteger(-1);
				mi4 = new MyInteger(1);
				mi5 = new MyInteger(0);
				mi6 = new MyInteger("-1");
				mi7 = new MyInteger(2147483647);
				
			}
			
			@Test
			void testHashCode()
			{
				assertTrue(mi2.hashCode()==mi7.hashCode(), " hashCode of mi2 and mi7 should be equal");
				assertEquals(-2147483648, mi1.hashCode(), "hashCode of mi1 should be -2147483648");
				assertEquals(0, mi5.hashCode(), "hashCode of mi5 should be 0");
			}

			@Test
			void testMyIntegerInt()
			{
				assertNotNull(mi4, "mi4 should be not null");
				assertTrue(mi3.equals(mi6), "mi3 and mi6 should be equal");
				assertTrue(mi7.equals(mi2), "mi7 and mi2 should be equal");
			}

			@Test
			void testMyIntegerString()
			{
				assertNotNull(mi1, "mi1 should be not null");
				assertNotNull(mi2, "mi2 should be not null");
				assertNotNull(mi6, "mi6 should be not null");
				assertTrue(mi3.equals(mi6), "mi3 and mi6 should be equal");
				assertTrue(mi7.equals(mi2), "mi7 and mi2 should be equal");
			}

			@Test
			void testParseInt()
			{
				assertEquals(-2147483648, MyInteger.parseInt("-2147483648"), "should be -2147483648");
				assertEquals(+2147483647, MyInteger.parseInt("+00002147483647"), "should be +2147483647");
				assertEquals(-1, MyInteger.parseInt("-0000001"), "should be -1");	
			}
			
			
			@Test
			void testIntValue()
			{
				assertEquals(-2147483648, mi1.intValue(), "should be -2147483648");
				assertEquals(+2147483647, mi2.intValue(), "should be +2147483647");
				assertEquals(+2147483647, mi7.intValue(), "should be +2147483647");
			}

			@Test
			void testDoubleValue()
			{
				assertEquals(-2147483648.0, mi1.doubleValue(), "should be -2147483648.0");
				assertEquals(+2147483647.0, mi2.doubleValue(), "should be +2147483647.0");
				assertEquals(+2147483647.0, mi7.doubleValue(), "should be +2147483647.0");
			}

			@Test
			void testValueOfString()
			{
				assertTrue(mi1.equals(MyInteger.valueOf("-2147483648")), "should be equal to mi1");
				assertTrue(mi2.equals(MyInteger.valueOf("2147483647")), "should be equal to mi2");
				assertTrue(mi7.equals(MyInteger.valueOf("2147483647")), "should be equal to mi7");
			}

			@Test
			void testValueOfInt()
			{
				assertTrue(mi1.equals(MyInteger.valueOf(-2147483648)), "should be equal to mi1");
				assertTrue(mi2.equals(MyInteger.valueOf(2147483647)), "should be equal to mi2");
				assertTrue(mi7.equals(MyInteger.valueOf(2147483647)), "should be equal to mi7");
			}

			@Test
			void testEqualsObject()
			{
				assertTrue(mi3.equals(mi6), "mi3 and mi6 should be equal");
				assertTrue(mi7.equals(mi2), "mi7 and mi2 should be equal");
				assertFalse(mi3.equals(mi4), "mi3 and mi4 should not be equal");
				assertFalse(mi3.equals(mi5), "mi3 and mi5 should not be equal");
			}

			@Test
			void testToString()
			{
				assertEquals("-2147483648", mi1.toString(), "should be '-2147483648'");
				assertEquals("2147483647", mi2.toString(), "should be '2147483647' mi2");
				assertEquals("2147483647", mi7.toString(), "should be '2147483647' mi7");
			}

			@Test
			void testCompare()
			{
				assertTrue(MyInteger.compare(5, 4)>0, "5,4 should be > 0");
				assertTrue(MyInteger.compare(4, 4)==0, "4,4 should be == 0");
				assertTrue(MyInteger.compare(4, 5)<0, "4,5 should be < 0");
				assertTrue(MyInteger.compare(MyInteger.MAX_VALUE, MyInteger.MIN_VALUE)>0, "MAX,MIN should be > 0");
				assertTrue(MyInteger.compare(MyInteger.MAX_VALUE, MyInteger.MAX_VALUE)==0, "MAX,MAX should be == 0");
				assertTrue(MyInteger.compare(MyInteger.MIN_VALUE, MyInteger.MAX_VALUE)<0, "MIN,MAX should be > 0");
			}

			@Test
			void testCompareTo()
			{
				assertTrue(mi1.compareTo(mi2)<0, "mi1, mi2 should be < 0");
				assertTrue(mi2.compareTo(mi1)>0, "mi2, mi1 should be > 0");
				assertTrue(mi2.compareTo(mi7)==0, "mi2, mi7 should be == 0");
				assertTrue(mi3.compareTo(mi6)==0, "mi3, mi6 should be == 0");
			}

		}
		```



##### Aufgabe 3 (Solitaire)

??? "Aufgabe 3"

 	**Information und Vorbereitung**

 	Wir beginnen, ein [Englisches Solitär](https://de.wikipedia.org/wiki/Solit%C3%A4r_(Brettspiel)) zu programmieren. Einige kennen es auch als Steckhalma. Ziel des Spiels ist, alle Steine bis auf einen (der am besten in der Mitte übrig bleibt), zu entfernen. Ein Zug ist wie folgt: ein Stein springt über einen anderen Stein und der übersprungene Stein wird entfernt. Es gibt viele [Lösungen](http://www.franks-fundgrube.de/sport/solitaire.html) dafür. Einige Klassen sind bereits gegeben:

 	??? "Klasse Point.java"
 		```java
 		package aufgaben.aufgabe3;

		/*
		 * ein Point repreasentiert eine Position
		 * im Spielfeld, bestehend aus der Nummer 
		 * fuer die Zeile (row) und der Nummer
		 * fuer die Spalte (col)
		 */
		public class Point {
			private int row;
			private int col;
			
			/*
			 * Konstruktor zur Erzeugung einer 
			 * Position bestehend aus row und col
			 */
			public Point(int row, int col)
			{
				this.row = row;
				this.col = col;
			}
			
			public int getRow() {
				return this.row;
			}

			public int getCol() {
				return this.col;
			}
			
			@Override
			public String toString()
			{
				return "("+ this.row + "," + this.col + ")";
			}
		}
 		```

 	??? "Klasse Move.java"
 		```java
		package aufgaben.aufgabe3;

		/*
		 * diese Klasse repraesentiert einen Zug
		 * Variablen sind Point from
		 * und Point to
		 * es wird nicht geprueft, ob der Zug ueberhaupt
		 * moeglich ist
		 */
		public class Move {
			private Point from;
			private Point to;
			
			/*
			 * ein Zug von dem from-Point (fromRow,fromCol)
			 * zum to-Point (toRow,toCol)
			 */
			public Move(int fromRow, int fromCol, int toRow, int toCol)
			{
				this.from = new Point(fromRow, fromCol);
				this.to = new Point(toRow, toCol);
			}
			
			/*
			 * in dem Konstruktor werden in this.from und this.to nicht einfach
			 * die Referenzen von from und to gespeichert, sondern davon Kopien
			 * erstellt, damit das Programm robuster gegen das Aendern von
			 * Referenzen ist
			 */
			public Move(Point from, Point to)
			{
				this.from = new Point(from.getRow(), from.getCol());
				this.to = new Point(to.getRow(), to.getCol());
			}
			
			/*
			 * der Getter fuer den Point from gibt keine Referenz auf
			 * den Point from zurueck, sondern eine Kopie (einen Klon)
			 * von from --> Referenzen koennen "verbogen" werden, aber
			 * die Kopien bleiben unveraendert
			 */
			public Point getFrom()
			{
				return new Point(this.from.getRow(), this.from.getCol());
			}
			
			/*
			 * der Getter fuer den Point to gibt keine Referenz auf
			 * den Point to zurueck, sondern eine Kopie (einen Klon)
			 * von to --> Referenzen koennen "verbogen" werden, aber
			 * die Kopien bleiben unveraendert
			 */
			public Point getTo()
			{
				return new Point(this.to.getRow(), this.to.getCol());
			}

		}
 
 	??? "Klasse Moves.java"
 		```java
		package aufgaben.aufgabe3;

		/*
		 * diese Klasse repraesentiert eine Folge 
		 * von Zuegen (Move), die in einem Array
		 * moves gespeichert sind
		 */
		public class Moves {
			private Move[] moves;
			
			/*
			 * der Konstruktor erstellt ein leeres moves-Array
			 * (d.h. noch keine Zuege (Moves) gespeichert)
			 */
			public Moves()
			{
				this.moves = new Move[0];
			}
			
			/*
			 * der Konstruktor erstellt ein moves-Array mit einem
			 * Move - dem erste Zug (firstMove) 
			 */
			public Moves(Move firstMove)
			{
				this.moves = new Move[1];
				this.moves[0] = firstMove;
			}
			
			/*
			 * Anzahl der bisher gespeicherten Zuege
			 */
			public int getLength()
			{
				return this.moves.length;
			}
			
			/*
			 * fuegt einen Zug (nextMove) zum moves-Array hinzu
			 * dazu muss das moves-Array um 1 laenger sein als zuvor
			 * es wird eine Kopie aller Zuege erstellt und dann
			 * der nextMove hinzugefuegt
			 */
			public void addMove(Move nextMove)
			{
				Move[] newMoves = new Move[this.moves.length + 1];
				for (int index = 0; index < this.moves.length; index++) {
					newMoves[index] = this.moves[index];
				}
				newMoves[newMoves.length - 1] = new Move(nextMove.getFrom(), nextMove.getTo());
				this.moves = newMoves;
			}
			
			/*
			 * gibt den Move zurueck, der im moves-Array unter dem Index index
			 * gespeichert ist;
			 * kann sein, dass index kein korrekter Index im moves-Array ist, 
			 * dann wird eine IllegalArgumentException geworfen
			 */
			public Move getMoveAtIndex(int index) throws IllegalArgumentException
			{
				try {
					return this.moves[index];
				}
				catch(ArrayIndexOutOfBoundsException e)
				{
					throw new IllegalArgumentException("kein gueltiger Index!");
				}
			}
			
			/*
			 * Ausgabe aller im moves-Array gespeicherten Zuege
			 * wird nur zum Debuggen benoetigt
			 */
			public void printMoves()
			{
				System.out.printf("%n---%n");
				for (int index = 0; index < this.moves.length; index++) {
					Move move = this.moves[index];
					Point from = move.getFrom();
					Point to = move.getTo();
					System.out.println(from.toString() + " --> " + to.toString());
				}
				System.out.printf("%n---%n%n");
			}
		}
		```
 
 	??? "enum State.java"
 		```java
		package aufgaben.aufgabe3;

		/*
		 * FREE - der Platz ist ein Spielfeld, aber kein Spielstein drauf
		 * USED - der Platz ist ein Spielfeld mit Spielstein drauf
		 * NOT  - der Platz gehoert nicht zum Spielfeld
		 */
		public enum State {
			FREE, USED, NOT
		}		
		```

 	**Aufgabe**

 	Befüllen Sie die Klasse `Solitaire.java`, wie in den Kommentaren beschrieben:
 
 	??? "Klasse Solitaire.java"
 		```java
		package aufgaben.aufgabe3.loesung;

		public class Solitaire {
			private Moves game;
			private State[][] field;
			
			public Solitaire()
			{
				this.game = new Moves();
				this.field = new State[7][7];
				for(int row = 0; row < this.field.length; row++)
				{
					for(int col = 0; col < this.field[row].length; col++)
					{
						if((row < 2 || row > 4) && (col < 2 || col > 4))
						{
							this.field[row][col] = State.NOT;
						}
						else
						{
							this.field[row][col] = State.USED;
						}
					}
				}
				this.field[3][3] = State.FREE;
			}
			
			/*
			 * Geben Sie das Spielfeld aus! Am Anfang sollte auf der
			 * Konsole so ein Bild erscheinen:
			 *     o o o     
			 *     o o o     
			 * o o o o o o o 
			 * o o o   o o o 
			 * o o o o o o o 
			 *     o o o     
			 *     o o o 
			 * 
			 */
			public void print()
			{

			}
			
			/*
			 * diese Methode gibt ein true zurueck, wenn von der
			 * uebergebenen Position (row,col) ein Zug moeglich ist
			 * d.h. 
			 * 1. auf der angegebenen Position muss ein Stein sein
			 * 2. zwei Steine weiter (oben, unten, rechts oder links)
			 * 		darf kein Stein sein
			 * 3. dazwischen muss ein Stein sein
			 */
			public boolean possibleFrom(int row, int col)
			{

				return false;
			}
			
			/*
			 * diese Methode gibt alle Positionen (Point) zurueck,
			 * AUF die von (fromRow,fromCol) aus gesprungen werden
			 * kann
			 */
			public Point[] possibleTo(int fromRow, int fromCol)
			{
				if(!possibleFrom(fromRow, fromCol)) return new Point[0];
				
				// naechste Zeile muss entfernt werden!
				return null;
			}
			
			/* 
			 * diese Methode erzeugt ein Moves-Objekt
			 * in dieses Moves-Objekt werden mithilfe der
			 * Objektmethode addMove() (aus Moves) alle
			 * moeglichen Zuege hinzugefuegt
			 * (moeglich im aktuellen Zustand von field[][])
			 */
			public Moves possibleMoves()
			{
				Moves possibleMoves = new Moves();

				// next line for debugging
				possibleMoves.printMoves();
				return possibleMoves;
			}
				
			/*
			 * gibt ein true zurueck, wenn im aktuellen Zustand 
			 * von field[][] ueberhaupt noch ein Zug moeglich ist
			 * sonst false
			 */
			public boolean movePossible()
			{

				return false;
			}
			
			/*
			 * ruft die Methode move(Move move) auf,
			 * wenn ein Zug moeglich ist (dann true zurueck)
			 * sonst false
			 */
			public boolean moveFirstPossible()
			{
				if(!movePossible()) return false;
				/*
				 *  hier einen moeglichen Zug ausfuehren
				 *  den ersten, den Sie finden (siehe
				 *  possibleMoves() )
				 */
				return true;
			}
			
			/*
			 * hier wird der Zug Move move ausgefuehrt
			 * nach dem Zug ist 
			 * 1. die from-Position leer
			 * 2. die to-Position mit einem Stein besetzt
			 * 3. dazwischen leer (Stein wird "entfernt")
			 * falls Zug nicht moeglich, wird eine 
			 * IllegalArgumentException geworfen 
			 */
			public void move(Move move) throws IllegalArgumentException
			{

			}

		}
		```

		Sie können selbstverständlich beliebig viele weitere (Hilfs-)Methoden hinzufügen. 

		Testen Sie Ihr Spiel in einer `Testklasse`. Führen Sie einige Züge aus und geben danach immer das Spielfeld auf die Konsole aus. Die Konsole könnte z.B. dann so aussehen:
 
 	??? "mögliche Konsolenausgaben"
		```bash
		    o o o     
		    o o o     
		o o o o o o o 
		o o o   o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(1,3) --> (3,3)
		(3,1) --> (3,3)
		(3,5) --> (3,3)
		(5,3) --> (3,3)

		---

		    o o o     
		    o   o     
		o o o   o o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(2,1) --> (2,3)
		(2,5) --> (2,3)
		(4,3) --> (2,3)

		---

		    o o o     
		    o   o     
		o     o o o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(0,2) --> (2,2)
		(2,4) --> (2,2)
		(3,3) --> (1,3)
		(4,1) --> (2,1)
		(4,2) --> (2,2)

		---

		      o o     
		        o     
		o   o o o o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(0,4) --> (0,2)
		(2,3) --> (2,1)
		(3,2) --> (1,2)
		(3,3) --> (1,3)
		(4,1) --> (2,1)

		---

		    o         
		        o     
		o   o o o o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(2,3) --> (2,1)
		(2,4) --> (0,4)
		(3,2) --> (1,2)
		(3,3) --> (1,3)
		(4,1) --> (2,1)

		---

		    o         
		        o     
		o o     o o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(2,0) --> (2,2)
		(2,4) --> (0,4)
		(2,5) --> (2,3)
		(4,2) --> (2,2)
		(4,3) --> (2,3)

		---

		    o         
		        o     
		    o   o o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(2,4) --> (0,4)
		(2,5) --> (2,3)
		(3,2) --> (1,2)
		(4,0) --> (2,0)
		(4,1) --> (2,1)
		(4,3) --> (2,3)

		---

		    o   o     
		              
		    o     o o 
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(2,6) --> (2,4)
		(3,2) --> (1,2)
		(4,0) --> (2,0)
		(4,1) --> (2,1)
		(4,3) --> (2,3)
		(4,4) --> (2,4)

		---

		    o   o     
		              
		    o   o     
		o o o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(3,2) --> (1,2)
		(3,4) --> (1,4)
		(4,0) --> (2,0)
		(4,1) --> (2,1)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)

		---

		    o   o     
		    o         
		        o     
		o o   o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(0,2) --> (2,2)
		(3,0) --> (3,2)
		(3,4) --> (1,4)
		(3,4) --> (3,2)
		(4,0) --> (2,0)
		(4,1) --> (2,1)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)
		(5,2) --> (3,2)

		---

		        o     
		              
		    o   o     
		o o   o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(3,0) --> (3,2)
		(3,4) --> (1,4)
		(3,4) --> (3,2)
		(4,0) --> (2,0)
		(4,1) --> (2,1)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)
		(5,2) --> (3,2)

		---

		        o     
		              
		    o   o     
		    o o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(3,2) --> (1,2)
		(3,3) --> (3,1)
		(3,4) --> (1,4)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)

		---

		        o     
		    o         
		        o     
		      o o o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(3,4) --> (1,4)
		(3,4) --> (3,2)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)
		(5,2) --> (3,2)

		---

		        o     
		    o   o     
		              
		      o   o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(0,4) --> (2,4)
		(3,6) --> (3,4)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)
		(5,2) --> (3,2)
		(5,4) --> (3,4)

		---

		              
		    o         
		        o     
		      o   o o 
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(3,6) --> (3,4)
		(4,3) --> (2,3)
		(4,5) --> (2,5)
		(4,6) --> (2,6)
		(5,2) --> (3,2)
		(5,4) --> (3,4)

		---

		              
		    o         
		        o     
		      o o     
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(3,3) --> (3,5)
		(3,4) --> (1,4)
		(3,4) --> (3,2)
		(4,3) --> (2,3)
		(5,2) --> (3,2)

		---

		              
		    o         
		        o     
		          o   
		o o o o o o o 
		    o o o     
		    o o o     


		---
		(4,5) --> (2,5)
		(5,2) --> (3,2)
		(5,3) --> (3,3)
		(5,4) --> (3,4)

		---

		              
		    o         
		        o o   
		              
		o o o o o   o 
		    o o o     
		    o o o     


		---
		(2,4) --> (2,6)
		(2,5) --> (2,3)
		(4,3) --> (4,5)
		(5,2) --> (3,2)
		(5,3) --> (3,3)
		(5,4) --> (3,4)

		---

		              
		    o         
		            o 
		              
		o o o o o   o 
		    o o o     
		    o o o     


		---
		(4,3) --> (4,5)
		(5,2) --> (3,2)
		(5,3) --> (3,3)
		(5,4) --> (3,4)

		---

		              
		    o         
		            o 
		              
		o o o     o o 
		    o o o     
		    o o o     


		---
		(4,1) --> (4,3)
		(4,6) --> (4,4)
		(5,2) --> (3,2)
		(6,3) --> (4,3)
		(6,4) --> (4,4)

		---

		              
		    o         
		            o 
		              
		o     o   o o 
		    o o o     
		    o o o     


		---
		(4,6) --> (4,4)
		(5,3) --> (3,3)
		(6,2) --> (4,2)
		(6,4) --> (4,4)

		---

		              
		    o         
		            o 
		              
		o     o o     
		    o o o     
		    o o o     


		---
		(4,3) --> (4,5)
		(4,4) --> (4,2)
		(5,3) --> (3,3)
		(5,4) --> (3,4)
		(6,2) --> (4,2)

		---

		              
		    o         
		            o 
		              
		o         o   
		    o o o     
		    o o o     


		---
		(6,2) --> (4,2)
		(6,3) --> (4,3)
		(6,4) --> (4,4)

		---

		              
		    o         
		            o 
		              
		o   o     o   
		      o o     
		      o o     


		---
		(5,4) --> (5,2)
		(6,3) --> (4,3)
		(6,4) --> (4,4)
		(6,4) --> (6,2)

		---

		              
		    o         
		            o 
		              
		o   o     o   
		    o         
		      o o     


		---
		(4,2) --> (6,2)
		(5,2) --> (3,2)
		(6,4) --> (6,2)

		---

		              
		    o         
		            o 
		              
		o         o   
		              
		    o o o     

		              
		    o         
		            o 
		              
		o         o   
		              
		    o o o     

		```

 	Dabei steht z.B. 
 	```bash

		---
		(1,3) --> (3,3)
		(3,1) --> (3,3)
		(3,5) --> (3,3)
		(5,3) --> (3,3)

		---
	```
	für die in dem Zustand darüber möglichen Züge.	


??? question "eine mögliche Lösung für Aufgabe 3"
	
	=== "Solitaire.java"
		```java linenums="1"
		package aufgaben.aufgabe3;

		public class Solitaire {
			private Moves game;
			private State[][] field;
			
			public Solitaire()
			{
				this.game = new Moves();
				this.field = new State[7][7];
				for(int row = 0; row < this.field.length; row++)
				{
					for(int col = 0; col < this.field[row].length; col++)
					{
						if((row < 2 || row > 4) && (col < 2 || col > 4))
						{
							this.field[row][col] = State.NOT;
						}
						else
						{
							this.field[row][col] = State.USED;
						}
					}
				}
				this.field[3][3] = State.FREE;
			}
			
			public void print()
			{
				for(int row = 0; row < this.field.length; row++)
				{
					for(int col = 0; col < this.field[row].length; col++)
					{
						if(this.field[row][col] == State.USED) System.out.print("o ");
						else System.out.print("  ");
					}
					System.out.println();
				}
				System.out.println();
			}
			
			public boolean possibleFrom(int row, int col)
			{
				if(row>=0 && row<7 && col>=0 && col<7 && this.field[row][col] == State.USED)
				{
					// up ?
					if(row > 1 && this.field[row-1][col] == State.USED && this.field[row-2][col] == State.FREE) return true;
					// down ?
					if(row < 5 && this.field[row+1][col] == State.USED && this.field[row+2][col] == State.FREE) return true;
					// right ?
					if(col < 5 && this.field[row][col+1] == State.USED && this.field[row][col+2] == State.FREE) return true;
					// left ?
					if(col > 1 && this.field[row][col-1] == State.USED && this.field[row][col-2] == State.FREE) return true;
				}
				return false;
			}
			
			public Point[] possibleTo(int fromRow, int fromCol)
			{
				if(!possibleFrom(fromRow, fromCol)) return new Point[0];
				
				// there is at least one To-Point
				int nrOfPossibleTos = 0;
				if(fromRow > 1 && this.field[fromRow-1][fromCol] == State.USED && this.field[fromRow-2][fromCol] == State.FREE) nrOfPossibleTos++;
				if(fromRow < 5 && this.field[fromRow+1][fromCol] == State.USED && this.field[fromRow+2][fromCol] == State.FREE) nrOfPossibleTos++;
				if(fromCol < 5 && this.field[fromRow][fromCol+1] == State.USED && this.field[fromRow][fromCol+2] == State.FREE) nrOfPossibleTos++;
				if(fromCol > 1 && this.field[fromRow][fromCol-1] == State.USED && this.field[fromRow][fromCol-2] == State.FREE) nrOfPossibleTos++;
				
				Point[] tos = new Point[nrOfPossibleTos];
				int index = 0;
				if(fromRow > 1 && this.field[fromRow-1][fromCol] == State.USED && this.field[fromRow-2][fromCol] == State.FREE) tos[index++] = new Point(fromRow-2, fromCol);
				if(fromRow < 5 && this.field[fromRow+1][fromCol] == State.USED && this.field[fromRow+2][fromCol] == State.FREE) tos[index++] = new Point(fromRow+2, fromCol);
				if(fromCol < 5 && this.field[fromRow][fromCol+1] == State.USED && this.field[fromRow][fromCol+2] == State.FREE) tos[index++] = new Point(fromRow, fromCol+2);
				if(fromCol > 1 && this.field[fromRow][fromCol-1] == State.USED && this.field[fromRow][fromCol-2] == State.FREE) tos[index++] = new Point(fromRow, fromCol-2);
				return tos;
			}
			
			public Moves possibleMoves()
			{
				Moves possibleMoves = new Moves();
				for(int row = 0; row < this.field.length; row++)
				{
					for(int col = 0; col < this.field[row].length; col++)
					{
						if(possibleFrom(row,col))
						{
							Point[] tos = this.possibleTo(row, col);
							Point from = new Point(row,col);
							for(int index=0; index<tos.length; index++)
							{
								possibleMoves.addMove(new Move(from, tos[index]));
							}
						}
					}
				}
				// next line for debug
				possibleMoves.printMoves();
				return possibleMoves;
			}
				
			public boolean movePossible()
			{
				for(int row = 0; row < this.field.length; row++)
				{
					for(int col = 0; col < this.field[row].length; col++)
					{
						if(possibleFrom(row,col)) return true;
					}
				}
				return false;
			}
			
			public boolean moveFirstPossible()
			{
				if(!movePossible()) return false;
				else {
					Moves possibleMoves = this.possibleMoves();
					try {
						Move move = possibleMoves.getMoveAtIndex(0);
						this.move(move);
						return true;
					}
					catch(IllegalArgumentException e)
					{
						System.out.println("Zug nicht moeglich! Index : 0");
						return false;
					}
				}
			}
			
			public void move(Move move) throws IllegalArgumentException
			{
				Point from = move.getFrom();
				Point to = move.getTo();
				int fromRow = from.getRow();
				int fromCol = from.getCol();
				int toRow = to.getRow();
				int toCol = to.getCol();
				int middleRow = (fromRow + toRow) / 2;
				int middleCol = (fromCol + toCol) / 2;
				try {
					this.field[fromRow][fromCol] = State.FREE;
					this.field[middleRow][middleCol] = State.FREE;
					this.field[toRow][toCol] = State.USED;	
				}
				catch(ArrayIndexOutOfBoundsException e)
				{
					throw new IllegalArgumentException("Zug nicht moeglich! (" + fromRow + ", " + fromCol + ") --> "
							+ "( " + toRow + ", " + toCol + ") ");
				}
			}

		}
		```
	
	=== "Testklasse.java"
		```java linenums="1"
		package aufgaben.aufgabe3;

		public class Testklasse {

			public static void main(String[] args) {
				Solitaire s = new Solitaire();
				s.print();
				while(s.moveFirstPossible())
				{
					s.print();
				}
				s.print();
			}

		}
		```



##### Aufgabe 4 (test-driven development)

??? "Aufgabe 4"
	1. Implementieren Sie eine Methode `public static int singleNumber(int[] nums)` durch testgetriebene Entwicklung. Für `nums` soll Folgendes gelten: 
		- es ist nicht leer, d.h. `1 <= nums.length`, 
		- jede Zahl in `nums` ist genau zwei Mal in `nums` enthalten, bis auf eine - diese existiert nur genau ein Mal in `nums`
	2. Die Methode gibt die Zahl zurück, die nur genau ein Mal in `nums` enthalten ist. Bsp.:
		```bash
		// Beispiel 1
		Input: nums = [2,2,1]
		Output: 1

		// Beispiel 2
		Input: nums = [4,1,2,1,2]
		Output: 4

		// Beispiel 3
		Input: nums = [1]
		Output: 1
		```
	3. Sie können beliebig viele weitere Methoden implementieren, die Ihnen helfen. Diese müssen aber auch testgetrieben entwickelt werden. 
	4. Pushen Sie alle Klassen (also natürlich auch die Testklasse) in Ihr Remote-Repository. 



##### Aufgabe 5 (Operationen über Mengen)

??? "Aufgabe 5"
	Implementieren Sie eine Klasse `SetOperations`. 

	1. Erstellen Sie drei Objektvariablen (z.B. `numbers1`, `numbers2` und `both`) vom Typ `Set<Integer>`. Erzeugen Sie für diese Objektvariablen `TreeSet`-Objekte.
	2. Erstellen Sie eine Objektmethode `fill()`. In dieser Methode sollen die beiden Sets `numbers1` und `numbers2` mit Zufallszahlen aus dem Wertebereich `0..99` befüllt werden. Erzeugen Sie jeweils `100` Zufallszahlen (die `number`s-Sets werden dann aber jeweils weniger als 100 Einträge haben, da doppelte Elemente nicht aufgenommen werden.) <br>
	Beachten Sie, dass die `nextInt()`-Methode ein `int` zurückliefert. Dieses int muss zunächst in ein `Integer`-Objekt umgewandelt werden (`Integer.valueOf(int)`) und dieses `Integer`-Objekt wird dann dem Set hinzugefügt (wenn es nicht schon enthalten ist).
	3. Erstellen Sie eine Objektmethode `fillBothUnion()`. In dieser Methode wird die `both`-Menge derart befüllt, dass `both` einer **Vereinigung** der Mengen `numbers1` und `numbers2` entspricht. Für Methoden auf Mengen siehe z.B. [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Set.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Set.html).
	4. Erstellen Sie eine Objektmethode `fillBothIntersection()`. In dieser Methode wird die `both`-Menge derart befüllt, dass `both` einem **Schnitt** der Mengen `numbers1` und `numbers2` entspricht. 
	5.	Erstellen Sie eine Objektmethode `fillBothDifference()`. In dieser Methode wird die `both`-Menge derart befüllt, dass `both` der **Differenz** der Menge `numbers1` minus der Menge `numbers2` entspricht. 
	6. Schreiben Sie eine Methode `print()` so, dass jede Menge als ein Quadrat (10x10) aus Punkten auf der Konsole erscheint. Es wird ein Punkt gezeichnet, wenn die entsprechende Zahl (die 100 Punkte bilden die Zahlen 0..99 ab) in der Menge enthalten ist und es wird kein Punkt gezeichnet, wenn die Zahl nicht vorhanden ist. (Anstelle von Punkten können Sie auch jedes beliebige Zeichen (`o, *, #, x, ...`) verwenden). <br>
	Gestalten Sie die Ausgabe so, dass die drei Mengen `numbers1`, `numbers2` und `both` nebeneinander auf der Konsole erscheinen. 
	7. Testen Sie alle drei Methoden `fillBothUnion()`, `fillBothIntersection()` und `fillBothDifference()`. Die Ausgabe könnte wie folgt sein (`A` stellt die Menge `numbers1` dar, `B` die Menge `numbers2` und rechts ist jeweils die `both`-Menge dargestellt): 

		![aufgabe5](./files/39_aufgabe5.png)

		![aufgabe5](./files/40_aufgabe5.png)

		![aufgabe5](./files/41_aufgabe5.png)

	**Tipps**: 

	- Für die schwarzen Punkte habe ich das Character `'\u25cf'` verwendet (ein passendes Leerzeichen dazu ist `'\u2009'`). Das Zeichen für die Vereinigung ist `'\u222a'` und für den Schnitt `'\u2229'`. 
	- Schauen Sie sich auch im Skript [Mengenoperationen](./collections/#mengenoperationen) an. 



??? question "eine mögliche Lösung für Aufgabe 5"
	
	=== "SetOperations.java"
		```java linenums="1"
		package aufgaben.aufgabe5;

		import java.util.*;

		public class SetOperations {
			Set<Integer> numbers1;
			Set<Integer> numbers2;
			Set<Integer> both;
			final static int UPPER_BOUND = 100;
			
			public SetOperations()
			{
				this.numbers1 = new TreeSet<>();
				this.numbers2 = new TreeSet<>();
				this.both = new TreeSet<>();
			}
			
			public void fill()
			{
				Random r = new Random();
				int nextIntNr = 0;
				Integer nextIntegerNr = Integer.valueOf(nextIntNr);
				// fill numbers1
				for(int i=0; i<UPPER_BOUND; i++)
				{
					nextIntNr = r.nextInt(UPPER_BOUND);
					nextIntegerNr = Integer.valueOf(nextIntNr);
					this.numbers1.add(nextIntegerNr);
				}
				// fill numbers2
				for(int i=0; i<UPPER_BOUND; i++)
				{
					nextIntNr = r.nextInt(UPPER_BOUND);
					nextIntegerNr = Integer.valueOf(nextIntNr);
					this.numbers2.add(nextIntegerNr);
				} 
			}
			
			public void fillBothUnion()
			{
				System.out.println();
				System.out.println("----------------------------------------------------------------------------");
				System.out.println("         A                           B                        A "+'\u222a'+" B ");
				System.out.println("----------------------------------------------------------------------------");
				this.both.addAll(numbers1);
				this.both.addAll(numbers2);
			}
			
			public void fillBothIntersection()
			{
				System.out.println();
				System.out.println("----------------------------------------------------------------------------");
				System.out.println("         A                           B                        A "+'\u2229'+" B ");
				System.out.println("----------------------------------------------------------------------------");
				this.both.addAll(numbers1);
				this.both.retainAll(numbers2);
			}
			
			public void fillBothDifference()
			{
				System.out.println();
				System.out.println("----------------------------------------------------------------------------");
				System.out.println("         A                           B                          A-B ");
				System.out.println("----------------------------------------------------------------------------");
				this.both.addAll(numbers1);
				this.both.removeAll(numbers2);
			}
			
			public Integer printOneLine(Iterator<Integer> it, Integer next, int curRow)
			{
				int curNr = 0; // row*10+col
				for(int col=0; col<10; col++)
				{
					curNr = curRow*10+col;
					if(next!=null && next.intValue()==curNr) 
					{
						System.out.print('\u25cf'+" ");
						if(it.hasNext())
						{
							next = it.next();
						}
					}
					else
					{
						//System.out.print('\u25cc'+" ");
						System.out.print('\u2009'+" ");
					}
				}
				return next;
			}
			
			private void printSpaces(int nrOfSpaces)
			{
				for(int spaces=0; spaces<nrOfSpaces; spaces++) System.out.print(" ");
			}
			
			private void justTestPrint()
			{ 
				System.out.print(numbers1.size() + " : [ ");
				for(Integer i : numbers1) System.out.print(i.intValue() + " ");
				System.out.println("]");
				System.out.println();
				System.out.print(numbers2.size() + " : [ ");
				for(Integer i : numbers2) System.out.print(i.intValue() + " ");
				System.out.println("]");
				System.out.println();
				System.out.print(both.size() + " : [ ");
				for(Integer i : both) System.out.print(i.intValue() + " ");
				System.out.println("]");
				System.out.println();
			}
			
			public void print()
			{
				//justTestPrint();
				System.out.println();
			
				Iterator<Integer> it1 = numbers1.iterator();
				Integer next1 = (it1.hasNext()) ? it1.next() : null;
				
				Iterator<Integer> it2 = numbers2.iterator();
				Integer next2 = (it2.hasNext()) ? it2.next() : null;
				
				Iterator<Integer> itb = both.iterator();
				Integer nextB = (itb.hasNext()) ? itb.next() : null;
				
				for(int row=0; row<10; row++)
				{
					next1 = this.printOneLine(it1, next1, row);
					printSpaces(8);
					next2 = this.printOneLine(it2, next2, row);
					printSpaces(8);
					nextB = this.printOneLine(itb, nextB, row);
					System.out.println();
				}
				System.out.println();
				
				
			}
		}
		```
	
	=== "Testklasse.java"
		```java linenums="1"
		package aufgaben.aufgabe5;

		public class Testklasse {

			public static void main(String[] args) {
				SetOperations so = new SetOperations();
				so.fill();
				
				so.fillBothUnion();
				so.print();
				
				so.fillBothIntersection();
				so.print();
				
				so.fillBothDifference();
				so.print();
			}

		}
		```



##### Aufgabe 6 (Maps)

??? "Aufgabe 6"

	1. Testen Sie folgenden Code:

		```java
		long a = 1000;
		Integer b = 1000; 		
		Integer c = 1000; 		
		System.out.println(a == b);
		System.out.println(b == c);
		```

		Was stellen Sie fest und warum? Ändern Sie den Vergleich der Variablen `b` und `c` so, dass `true` ausgegeben wird. Die ersten drei Zeilen bleiben dabei unverändert. Sie benötigen also einen anderen Vergleich als `b == c`. 

	2. Erstellen Sie eine Klasse `PizzaMenu`. Erzeugen Sie darin eine `HashMap` und fügen Sie folgende Pizzen mit ihren Preisen ein:

		| Pizza | Preis | 
		|-------|-------|
		|Margherita |7,00 |
		|Veggie	|7,50 |
		|Salami	|8,50 |
		|Schinken |8,50 |

	3. Schreiben Sie eine Methode `public static boolean existingPizza(Map<String,Double> menu, String pizza)`, die testet, ob eine eingegebene Pizza im Menü existiert. 

	4. Schreiben Sie eine Methode `public static void printPrice(Map<String,Double> menu, String pizza)`, die für eine eingegebene Pizza den Preis ausgibt. Verwenden Sie dazu `existingPizza()`. Beispielausgabe: 

		```bash
		Salami: 8.50 Euro
		Hawaii: Diese Pizza gibt es nicht.
		```

	5. Schreiben Sie eine `printMenu()`-Methode, die ausgibt, wie viele Pizzen es im Menü gibt. Wenn das Menü nicht leer ist, sollen zudem alle Pizzen mit ihrem Preis ausgegeben werden. Beispielausgabe:

		```bash
		// Beispiel fuer leeres Menue
		##############
		Das Menu enthält 0 Pizzen.
		##############
		```

		```bash
		// Beispiel fuer Menue
		##############
		Das Menu enthält 4 Pizzen.
		Pizza Margherita: 7.00 Euro
		Pizza Salami: 8.50 Euro
		Pizza Schinken: 8.50 Euro
		Pizza Veggie: 7.50 Euro
		##############
		```		

	6. Ändern Sie den Preis der `Schinken`-Pizza in der `HashMap` zu `8,20`. 

	7. Schreiben Sie eine Methode `public static void affordablePizza(Map<String,Double> menu, double maxPrice)`, die alle Pizzen ausgibt, die man sich für einen bestimmten Betrag leisten kann. Beispielausgabe:

		```bash
		Eingegebener Betrag: 4.0 Euro
		Leider gibt es zu diesem Preis keine Pizza.
		Eingegebener Betrag: 8.0 Euro
		Mögliche Pizzen: [Margherita, Veggie]
		```

	8. Das Menu wird überarbeitet, löschen Sie daher alle Einträge. Füllen Sie das Menü anschließend mit diesen neuen Pizzen:

		| Pizza | Preis | 
		|-------|-------|
		|Verde |7,00 |
		|Hawaii	|8,20 |
		|Tradizionale	|8,50 |

	9. Legen Sie ein zweites Menü an und kopieren Sie alle Einträge aus dem ersten Menü hinein. Löschen Sie im zweiten Menü die Pizza `Hawaii` und geben Sie die Namen aller noch verfügbaren Pizzen aus.

	10. Finden Sie in Ihrem Code ein Beispiel für Auto-Boxing und schreiben Sie einen entsprechenden Kommentar.



??? question "eine mögliche Lösung für Aufgabe 6"
	
	=== "PizzaMenu.java"
		```java linenums="1"
		package aufgaben.aufgabe6;

		import java.util.ArrayList;
		import java.util.HashMap;
		import java.util.Map;
		import java.util.Set;
		import java.util.List;


		public class PizzaMenu {	

			//3. 
			public static boolean existingPizza(Map<String,Double> menu, String pizza) {
				if(menu.containsKey(pizza)){			
					return true;
				}		
				return false;
			}

			//4.
			public static void printPrice(Map<String,Double> menu, String pizza) {
				if(existingPizza(menu, pizza)){
					System.out.println(pizza + ": " + menu.get(pizza));
				}else {
					System.out.println(pizza + ": " + "Diese Pizza gibt es nicht.");
				}
			}

			//5.
			public static void printMenu(Map<String,Double> menu) {
				System.out.println();
				System.out.println("##############");
				System.out.println("Das Menu enthält " + menu.size() + " Pizzen.");

				if (!menu.isEmpty()) {
					for(Map.Entry<String, Double> entry : menu.entrySet()) 
					{
						System.out.println("Pizza : " + entry.getKey() + " - " + entry.getValue());
					}
				}

				System.out.println("##############");		
			}

			//7.
			public static void affordablePizza(Map<String,Double> menu, double maxPrice) {
				List<String> pizzen = new ArrayList<>();

				System.out.println("Eingegebener Betrag: " + maxPrice + " Euro"); 
				for(Map.Entry<String, Double> eintrag : menu.entrySet())
				{
					if (eintrag.getValue() <= maxPrice) { //Auto-Boxing von double zu Double
						pizzen.add(eintrag.getKey());
					}
				}
				if (pizzen.size() == 0) {
					System.out.println("Leider gibt es zu diesem Preis keine Pizza.");
				}else {			
					System.out.println("Mögliche Pizzen: " + pizzen);
				}	
			}		


			public static void main(String[] args) {

				//2.
				Map<String,Double> menu1 = new HashMap<>();
				menu1.put("Margherita", 7.0); //Auto-Boxing von double zu Double
				menu1.put("Veggie", 7.5); //Auto-Boxing von double zu Double
				menu1.put("Salami", 8.5); //Auto-Boxing von double zu Double
				menu1.put("Schinken", 8.5); //Auto-Boxing von double zu Double

				//4.		
				printPrice(menu1, "Salami");
				printPrice(menu1, "Hawaii");

				//5.
				printMenu(menu1);

				//6. 
				menu1.put("Schinken", 8.20); //Auto-Boxing von double zu Double

				//7.
				affordablePizza(menu1, 4.0);
				affordablePizza(menu1, 8.0);

				//8. 
				menu1.clear();
				printMenu(menu1);

				menu1.put("Verde", 7.0); //Auto-Boxing von double zu Double
				menu1.put("Hawaii", 8.2); //Auto-Boxing von double zu Double
				menu1.put("Tradizionale", 8.5); //Auto-Boxing von double zu Double

				printMenu(menu1);

				//9.
				Map<String,Double> menu2 = new HashMap<>();
				menu2.putAll(menu1);
				menu2.remove("Hawaii");
				Set<String> pizzaNames = menu2.keySet();
				System.out.println();
				System.out.println("Es gibt folgende Pizzen: " + pizzaNames);
			}

		}
		```



##### Aufgabe 7 (Interfaces)

??? "Aufgabe 7"

	1. Das [*Observer*-Entwurfsmuster](https://de.wikipedia.org/wiki/Beobachter_(Entwurfsmuster)) gehört zu den am meisten verwendeten *Designmustern/Designpattern/Pattern* in der Programmierung. Es wird auch *Beobachter*-Muster oder *Publisher*-Pattern genannt. Wir werden dieses Muster in Kürze sehr häufig anwenden, wenn wir *Nutzerereignisse*  in grafischen Oberflächen behandeln. Man kann sich dieses Pattern so vorstellen, dass der *Publisher* eine Zeitung oder auch Slack ist und dass *Listener* diese Zeitung (oder Slack) "abonnieren". Immer, wenn eine Nachricht veröffentlicht wird, dann erfahren alle Abonnenten davon. Wir werden eine (einfache) Implementierung dieses Entwurfsmusters durchführen.

	2. Erstellen Sie ein Interface `Publisher` mit folgenden (abstrakten) Methoden:

		- `public boolean register(Listener listener);`
	- `public boolean unregister(Listener listener);`
	- `public void notifyListeners();` 
	- `public String getUpdate(Listener listener);`

	3. Erstellen Sie ein weiteres Interface `Listener` mit folgenden (abstrakten) Methoden:

		- `public void update();`
	- `public void setPublisher(Publisher publisher);`
	- `public void removePublisher(Publisher publisher);`

	4. Erstellen Sie eine Klasse `Slack`, die das `Publisher`-Interface implementiert. Objektvariablen der Klasse sind

		- `private Set<Listener> listeners;` (speichert alle "Abonnenten"; kann gerne auch eine Liste sein)
	- `private int nrOfMessages;` (speichert die aktuelle Nummer einer veröffentlichten Nachricht - die Nachrichten, die veröffentlicht werden, sollen fortlaufend nummeriert werden) 

		- Im parameterlosen Konstruktor werden die Menge (oder Liste) erzeugt und die `nrOfMessages` auf `0 gesetzt. 

		- In der Methode `register(Listener listener)` wird der `listener` in die Set `listeners` eingefügt. Geben Sie ein `true` zurück, wenn `listener` tatsächlich eingefügt wurde und `false` sonst (falls er schon in der Menge (oder Liste) war.

		- In der Methode `unregister(Listener listener)` wird der `listener` wieder aus der Set `listeners` gelöscht. Geben Sie ein `true` zurück, wenn `listener` tatsächlich gelöscht wurde und `false` sonst (falls er nicht in der Menge (oder Liste) war.

		- In der Methode `notifyListeners()` wird für alle `listener` aus der Menge `listeners` die `update()`-Methode aufgerufen (siehe `Listener` und `Student`). 

		- Die Methode `getUpdate(Listener obj)` liefert einfach folgenden String zurück: `"Breaking News " + this.nrOfMessages`.

		- Erstellen Sie eine Methode `public void publishNews()`, in der die `nrOfMessages` um 1 erhöht und die 
		Methode `notifyListeners()` aufgerufen wird. 

	5. Erstellen Sie eine Klasse `Student`, die das `Listener`-Interface implementiert. Objektvariablen der Klasse sind

		- `private String name;` (speichert den Namen von `Student`)
	- `private Publisher publisher;` (speichert den `Publisher`, an den sich `Student` anmeldet) 

		- Im parametrisierten Konstruktor `public Student(String name)` wird der Name initalisiert. 

		- In der Methode `setPublisher(Publisher publisher)` wird die `register()`-Methode des `publisher` aufgerufen und der Wert der Objektvariable `publisher` gesetzt. Geben Sie bei erfolgreicher Anmeldung an den `publisher` auf die Konsole `this.name + " registered!"` aus. 

		- In der Methode `removePublisher(Publisher publisher)` meldet sich `Student` wieder vom `publisher` ab (Aufruf von `unregister()` und Ausgabe auf die Konsole `this.name + " deregistered!"`.

		- In der Methode `update()` wird die `getUpdate()`-Methode des `publisher` aufgerufen und die zurückgegebene Nachricht `msg` wie folgt auf die Konsole ausgegben: `this.name + " received " + msg`. 

		- Implementieren Sie für `Student` auch die Methoden `equals()` und `hashCode()`. 

	6. Wenn Sie Ihre Implementierung mit folgender Klasse testen:

		```java
		public class Testklasse {

			public static void main(String[] args) 
			{		
				final int NR_OF_STUDENTS = 5;
				Slack slack = new Slack();
				
				Student[] students = new Student[NR_OF_STUDENTS];
				Character c = 'A';
				for(int index=0; index < students.length; index++)
				{
					students[index] = new Student(c.toString());
					c++;
					students[index].setPublisher(slack);
				}
				slack.publishNews();
				
				System.out.println();
				students[1].removePublisher(slack);
				students[3].removePublisher(slack);
				System.out.println();
				slack.publishNews();
				
				System.out.println();
				students[1].setPublisher(slack);
				students[2].removePublisher(slack);
				students[4].removePublisher(slack);	
				System.out.println();
				slack.publishNews();
				
				System.out.println();
				students[0].removePublisher(slack);
				students[1].removePublisher(slack);
				students[3].setPublisher(slack);
				System.out.println();
				slack.publishNews();
			}

		}
		```

		dann sollte die Ausgabe ungefähr so sein: 

		```bash
		A registered!
		B registered!
		C registered!
		D registered!
		E registered!
		D received Breaking News 1
		C received Breaking News 1
		B received Breaking News 1
		A received Breaking News 1
		E received Breaking News 1

		B deregistered!
		D deregistered!

		C received Breaking News 2
		A received Breaking News 2
		E received Breaking News 2

		B registered!
		C deregistered!
		E deregistered!

		B received Breaking News 3
		A received Breaking News 3

		A deregistered!
		B deregistered!
		D registered!

		D received Breaking News 4
		```



??? question "eine mögliche Lösung für Aufgabe 7"
	
	=== "Listener.java"
		```java linenums="1"
		package aufgaben.aufgabe7;

		public interface Listener {
			
			//method to update the listener, used by publisher
			public void update();
			
			//attach with publisher to observe
			public void setPublisher(Publisher publisher);
			
			//attach with publisher to observe
			public void removePublisher(Publisher publisher);
		}
		```
	
	=== "Publisher.java"
		```java linenums="1"
		package aufgaben.aufgabe7;

		public interface Publisher 
		{
			//methods to register and unregister listeners (observers)
			public boolean register(Listener listener);
			public boolean unregister(Listener listener);
			
			//method to notify listeners of change
			public void notifyListeners();
			
			//method to get updates from publisher (subject)
			public String getUpdate(Listener listener);
		}
		```
	
	=== "Slack.java"
		```java linenums="1"
		package aufgaben.aufgabe7;

		import java.util.*;

		public class Slack implements Publisher
		{
			private Set<Listener> listeners;
			private int nrOfMessages;
			
			public Slack()
			{
				this.listeners = new HashSet<>();
				this.nrOfMessages = 0;
			}
			
			@Override
			public boolean register(Listener listener) 
			{
				return listeners.add(listener);
			}

			@Override
			public boolean unregister(Listener listener) 
			{
				return listeners.remove(listener);		
			}

			@Override
			public void notifyListeners() 
			{
				for(Listener listener : listeners)
				{
					listener.update();
				}	
			}

			@Override
			public String getUpdate(Listener obj) 
			{
				return "Breaking News " + this.nrOfMessages;
			}
			
			public void publishNews()
			{
				this.nrOfMessages++;
				this.notifyListeners();
			}

		}
		```

	
	=== "Student.java"
		```java linenums="1"
		package aufgaben.aufgabe7;

		public class Student implements Listener
		{
			private String name;
			private Publisher publisher;
			
			public Student(String name)
			{
				this.name = name;
			}
			
			@Override
			public void update() 
			{
				String msg = this.publisher.getUpdate(this);
				System.out.println(this.name + " received " + msg);	
			}

			@Override
			public void setPublisher(Publisher publisher) 
			{
				this.publisher = publisher;
				if(this.publisher.register(this))
				{
					System.out.println(this.name + " registered!");
				}
			}

			@Override
			public void removePublisher(Publisher publisher) 
			{
				if(this.publisher.unregister(this)) 
				{
					System.out.println(this.name + " deregistered!");
					this.publisher = null;
				}	
			}
			
			@Override
			public boolean equals(Object o)
			{
				if(o==null) return false;
				if(this==o) return true;
				if(this.getClass()!=o.getClass()) return false;
				
				Student so = (Student)o;
				return this.name.equals(so.name);
			}
			
			@Override
			public int hashCode()
			{
				return this.name.hashCode();
			}
		}
		```

	
	=== "Testklasse.java"
		```java linenums="1"
		package aufgaben.aufgabe7;

		public class Testklasse {

			public static void main(String[] args) 
			{		
				final int NR_OF_STUDENTS = 5;
				Slack slack = new Slack();
				
				Student[] students = new Student[NR_OF_STUDENTS];
				Character c = 'A';
				for(int index=0; index < students.length; index++)
				{
					students[index] = new Student(c.toString());
					c++;
					students[index].setPublisher(slack);
				}
				slack.publishNews();
				
				System.out.println();
				students[1].removePublisher(slack);
				students[3].removePublisher(slack);
				System.out.println();
				slack.publishNews();
				
				System.out.println();
				students[1].setPublisher(slack);
				students[2].removePublisher(slack);
				students[4].removePublisher(slack);	
				System.out.println();
				slack.publishNews();
				
				System.out.println();
				students[0].removePublisher(slack);
				students[1].removePublisher(slack);
				students[3].setPublisher(slack);
				System.out.println();
				slack.publishNews();
			}

		}
		```



##### Aufgabe 8 (GUI)

??? "Aufgabe 8"

	1. Erstellen Sie die GUI für einen solchen Taschenrechner:

		![uebung8](./files/61_uebung8.png)

		Kann auch schicker sein :-) Viel Spaß!



##### Aufgabe 9 (Ereignisbehandlung)

??? "Aufgabe 9"

	1. Implementieren Sie für den Taschenrechner aus Aufgabe 8 das `ActionListener`-Interface, so dass der Taschenrechner über möglichst viel Funktionalität verfügt. 
	2. Minimumanforderung ist, dass durch die Eingabe von Ziffern Zahlen entstehen und dass die Buttons `+`, `-`, `/`, `+` und `=` insofern funktionieren, dass dadurch tatsächlich gerechnet wird, auch wenn die Regel "Punktrechnung geht vor Strichrechnung" nicht eingehalten wird. 
	3. Eine erste Verbesserung (ist schon **Zusatz**) wäre, dass die Regel "Punktrechnung geht vor Strichrechnung" eingehalten wird.
	4. Ab dann ist alles **Zusatz** und bedarf in vielen Fällen der Erweiterung der GUI: Verwendung und Beachten von Klammern (ist aber sehr schwer!), Speichern von Zwischenergebnissen, komplexere Operationen durch zuätzliche Tasten (Wurzel, Potenzieren, ...). Alles, was Sie möchten - Viel Spaß!



##### Aufgabe 10 (Zeichnen)

??? "Aufgabe 10"

	1. Wir berechnen PI mithilfe der [Monte-Carlo-Methode](https://groups.uni-paderborn.de/reiss/AnalyseBuch/Grundlagen/Geometrie/pi/area.html). Dazu zeichnen wir ein Quadrat und darin einen Kreisbogen (einen Viertelkreis), dessen Radius der Seitenlänge des Quadrats entspricht:

		![aufgabe10](./files/85_aufgabe10.png)

	2. Nun zeichnen wir in dieses Quadrat viele kleine ausgefüllte Kreise (Durchmesser z.B. `2` Pixel). Die Positionen dieser Kreise werden zufällig ermittelt. Wenn die zufällig ermittelte Position für diesen Kreis **im Kreisbogen** liegt, dann sind die kleinen Kreise rot, wenn die Position außerhalb liegt, dann sind die Kreise blau. Es entsteht bei ganz vielen (z.B. `50000`) Punkten so ein Bild:

		![aufgabe10](./files/86_aufgabe10.png)

	3. **Tipp:** Ob ein Punkt mit den Koordinaten `(x,y)` in einem (Viertel-)Kreis liegt, ermitteln Sie mithilfe der [Kreisgleichung](https://studyflix.de/mathematik/kreisgleichung-1867). Sei `(xm, ym)` der Mittelpunkt und `r` der Radius des Kreises, dann gilt `(x-xm)^2 + (y-ym)^2 = r^2` für alle Punkte auf dem Kreis (für Punkte im Kreis gilt <= und für Punkte außerhalb des Kreises >).

	4. Der Quotient aus der Anzahl der roten Punkte und der Gesamtanzahl der Punkte ist eine Näherung von PI/4. Geben Sie Ihre Annäherung von PI auf der Konsole aus.



