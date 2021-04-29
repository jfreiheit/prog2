# Übungen

##### Übung 1 (Git)

??? "Übung 1"

	1. Erstellen Sie sich einen GitHub-Account (oder wählen Sie einen anderen Git-Diensteanbieter)
	2. Erstellen Sie sich dort ein zentrales Repository 
	3. Richten Sie Ihren `workspace`, in dem Sie alle Ihre Entwicklungen in diesem Semester durchführen wollen (Übungen, Aufgaben, ...) als lokales Git-Repository ein
	4. Synchroniseren Sie Ihr lokales und Ihr zentrales Git-Repository


??? question "Video zu Übung 1 ([**Git**](../git/#git))"

	<iframe src="https://mediathek.htw-berlin.de/media/embed?key=5db6028a0361a277e7cff404504dd3e4&width=720&height=389&autoplay=false&autolightsoff=false&loop=false&chapters=false&related=false&responsive=false&t=0" data-src="" class="iframeLoaded" width="720" height="389" frameborder="0" allowfullscreen="allowfullscreen" allowtransparency="true" scrolling="no"></iframe>
	- alles zu [EGit](../git/#egit-git-mit-eclipse) herausgenommen

##### Übung 2 (enum und zweidimensionale Arrays)

??? "Übung 2"

	1. Gegeben ist die folgende Klasse `TicTacToe`:

		```java linenums="1"
		package uebungen.uebung2;

		public class TicTacToe 
		{
			enum State {EMPTY, RED, BLACK};
			State[][] field;

			public TicTacToe()
			{
				field = new State[3][3];
				for(int i=0; i<field.length; i++)
				  for(int j=0; j<field[i].length; j++)
				  	field[i][j]=State.EMPTY;
			}

			public void makeMove(int i, int j, State player)
			{
				if(field[i][j]==State.EMPTY && player!=State.EMPTY)   
					field[i][j]=player;
			}
		}
		```
	
	2. Fügen Sie alle notwendigen Klammern `{ }` ein, so dass die Anweisungsblöcke korrekt geklammert sind. 
	3. Erweitern Sie die Klasse `TicTacToe` um eine `print()`-Methode, die das Spielfeld auf die Konsole ausgibt (Setzen Sie z.B. für den Player `RED` ein `x` und für den Player `Black` ein `o` und für `EMPTY` ein Leerzeichen oder ein `-`). Die Ausgabe nach jeweils 2 Zügen von `RED` und `BLACK` könnte dann z.B. so aussehen: 

		```bash
		- o o 
		- x - 
		- - x 
		```

	4. Erweitern Sie die Klasse `TicTacToe` um eine `gewonnen()`-Methode (`true`, wenn ein Spieler drei Felder horizontal, diagonal oder vertikal belegt hat; ansonsten `false`).
	5. Erweitern Sie die Klasse `TicTacToe` um eine `unentschieden()`-Methode (`true`, wenn alle Felder besetzt sind, aber kein Spieler gewonnen hat; ansonsten `false`).
	6. Erstellen Sie eine Test-Klasse mit `main()`-Methode. Erstellen sie darin ein Objekt der Klasse `TicTacToe`. Führen Sie Züge aus (`makeMove()`) und prüfen Sie, ob gewonnen wurde oder unentschieden ist (mit entsprechenden Ausgaben). 
	7. Für 6. müssen Sie in der Testklasse Ihr `enum State` importieren. Warum ist das so? Was könnten Sie machen, damit das nicht notwendig ist?

	8. *Zusatz:* Sie können die Klasse `TicTacToe` beliebig erweitern, z.B.:
		- um Ausgaben, wenn gewonnen bzw. es unentschieden ist,
		- um Fehler in den Indizes `i` und `j` bei der `makeMove()`-Methode abzufangen,
		- eine Methode `spielen()` implementieren, die zufällig für die Spieler die Steine setzt usw.

	**Viel Spaß!**

??? question "Video zu Übung 2 (TicTacToe)"

	<iframe src="https://mediathek.htw-berlin.de/media/embed?key=a8ecee0fb66531428448ec96de483acc&width=720&height=389&autoplay=false&autolightsoff=false&loop=false&chapters=false&related=false&responsive=false&t=0" data-src="" class="iframeLoaded" width="720" height="389" frameborder="0" allowfullscreen="allowfullscreen" allowtransparency="true" scrolling="no"></iframe>


??? question "mögliche Lösung für Übung 2"
	
	=== "TicTacToe.java"
		```java linenums="1"
		package uebungen.uebung2;

		import java.util.Random;

		public class TicTacToe 
		{

			State[][] field;

			public TicTacToe()
			{
				this.field = new State[3][3];
				for(int i=0; i<this.field.length; i++)
				{
					for(int j=0; j<this.field[i].length; j++)
					{
						field[i][j]=State.EMPTY;
					}
				}
			}

			public void makeMove(int row, int col, State player)
			{
				if(row>=0 && row<this.field.length 
						&& col>=0 && col<this.field[row].length 
						&& this.field[row][col] == State.EMPTY)
				{
					if(player!=State.EMPTY) 
					{
						this.field[row][col]=player;
					}
				}
			}

			public void print()
			{
				for(int row=0; row<this.field.length; row++)
				{
					for(int col=0; col<this.field[row].length; col++)
					{
						if(field[row][col]==State.EMPTY)
						{
							System.out.print("- ");
						}
						else if(field[row][col]==State.RED)
						{
							System.out.print("x ");
						}
						else // BLACK
						{
							System.out.print("o ");
						}
					}
					System.out.println();
				}
				System.out.println();
			}
			
			public boolean gewonnen(State player)
			{
				if(player == State.EMPTY) return false;
				
				// alle drei Zeilen pruefen
				for(int row=0; row<this.field.length; row++)
				{
					if(this.field[row][0] == player && this.field[row][1] == player && this.field[row][2] == player)
					{
						return true;
					}
				}
				
				// alle drei Spalten pruefen
				for(int col=0; col<this.field.length; col++)
				{
					if(this.field[0][col] == player && this.field[1][col] == player && this.field[2][col] == player)
					{
						return true;
					}
				}
				
				// Diagonale von links oben nach rechts unten
				if(this.field[0][0] == player && this.field[1][1] == player && this.field[2][2] == player)
				{
					return true;
				}
				
				// Diagonale von rechts oben nach links unten
				if(this.field[0][2] == player && this.field[1][1] == player && this.field[2][0] == player)
				{
					return true;
				}
				return false;
			}
			
			public void printResultat()
			{
				if(this.gewonnen(State.RED))
				{
					System.out.println("Rot hat gewonnen!!!");
				}
				else if(this.gewonnen(State.BLACK))
				{
					System.out.println("Schwarz hat gewonnen!!!");
				}
				else if(this.unentschieden())
				{
					System.out.println("Unentschieden!!!");
				}
			}
			
			public void makeRandomMove(State player)
			{
				if(player != State.EMPTY)
				{
					Random r = new Random();
					int row = r.nextInt(3);
					int col = r.nextInt(3);
					while(this.field[row][col]!=State.EMPTY)
					{
						row = r.nextInt(3);
						col = r.nextInt(3);
					}
					this.field[row][col]=player;
				}
			}
			
			public void spielen()
			{
				State player = State.RED;
				while(!(this.unentschieden() || this.gewonnen(State.RED) || this.gewonnen(State.BLACK)))
				{
					this.makeRandomMove(player);
					this.print();
					this.printResultat();
					if(player == State.RED)
					{
						player = State.BLACK;
					}
					else
					{
						player = State.RED;
					}
					
					// player = (player == State.RED) ? State.BLACK : State.RED;
				}
			}
			
			public boolean voll()
			{
				for(int row=0; row<this.field.length; row++)
				{
					for(int col=0; col<this.field[row].length; col++)
					{
						if(field[row][col]==State.EMPTY)
						{
							return false;
						}
					}
				}
				return true;
			}
			
			public boolean unentschieden()
			{
				return (this.voll() && !this.gewonnen(State.RED) && !this.gewonnen(State.BLACK));
			}
		}
		```

	=== "State.java"
		```java linenums="1"
		package uebungen.uebung2;

		public enum State {
			EMPTY, RED, BLACK
		}
		```
	
	=== "TestTicTacToe.java"
		```java linenums="1"
		package uebungen.uebung2;

		public class TestTicTacToe {

			public static void main(String[] args) 
			{
				TicTacToe ttt = new TicTacToe();
				ttt.print();
				/*
				ttt.makeMove(1, 1, State.RED);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(1, 2, State.BLACK);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(1, -1, State.BLACK);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(0, 1, State.RED);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(2, 1, State.BLACK);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(1, 0, State.RED);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(0, 0, State.BLACK);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(0, 2, State.RED);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(2, 0, State.BLACK);
				ttt.printResultat();
				ttt.print();
				ttt.makeMove(2, 2, State.RED);
				ttt.printResultat();
				ttt.print();
				*/
				ttt.spielen();
				// ttt.makeRandomMove(State.RED);
				// ttt.print();
			}

		}

		```


##### Übung 3 (Exceptions)

??? "Übung 3"

	1. Schreiben Sie ein Programm zur Eingabe von zwei Zahlen mithilfe der Klasse `JOptionPane` aus dem und deren Division! Fangen Sie folgende Ausnahmen ab:
		- Falls die Eingabe keiner Zahl entspricht.
		- Falls die zweite Zahl eine 0 ist.

	2. **Scenario**:
		- Fenster zur Eingabe von Zahl 1 öffnet sich: <br/>
			![uebung2](./files/22_uebung2.png)
		- falsche Eingabe - keine Zahl:  <br/>
			![uebung2](./files/23_uebung2.png)
		- Fenster öffnet sich erneut (andere Nachricht!):  <br/>
			![uebung2](./files/24_uebung2.png)
		- Fenster zur Eingabe von Zahl 2 öffnet sich:  <br/>
			![uebung2](./files/25_uebung2.png)
		- die Division Zahl1/Zahl2 schlägt fehl (`ArithmeticException`), deshalb (andere Nachricht!):  <br/>
			![uebung2](./files/26_uebung2.png)
		- Ergebnis  <br/>
			![uebung2](./files/27_uebung2.png)

	3. Lagern Sie eine solche Eingabemöglichkeit in eine wiederverwendbare Methode aus, z.B. `public int inputInt(int min, int max)`, welche die eingegebene Zahl zurückgibt, wobei die eingegebene Zahl im Bereich `[min, max]` liegen muss.

	4. Lesen Sie eine Anzahl von Sekunden ein und schreiben Sie eine Umrechnung, so dass folgende Ausgabe entsteht (die Eingabe ist hier über die Konsole gezeigt) :
		```bash
		Gib eine Anzahl von Sekunden ein: 3456789
		3456789 Sekunden sind 40 Tage, 13 Minuten, 9 Sekunden.
		```

		```bash
		Gib eine Anzahl von Sekunden ein: 2345678
		2345678 Sekunden sind 27 Tage, 3 Stunden, 34 Minuten, 38 Sekunden.
		```

		```bash
		Gib eine Anzahl von Sekunden ein: 123456
		123456 Sekunden sind 1 Tag, 10 Stunden, 17 Minuten, 36 Sekunden.
		```

		```bash
		Gib eine Anzahl von Sekunden ein: 12345
		12345 Sekunden sind 3 Stunden, 25 Minuten, 45 Sekunden.		
		```

	5. Lesen Sie eine Zahl ein und geben Sie die Quersumme der Zahl aus.


	**Viel Spaß!**

??? question "Video zu Übung 3 (Exceptions)"

	<iframe src="https://mediathek.htw-berlin.de/media/embed?key=b18e9318e77865ab2f923c454a28886a&width=720&height=450&autoplay=false&autolightsoff=false&loop=false&chapters=false&related=false&responsive=false&t=0" data-src="" class="iframeLoaded" width="720" height="450" frameborder="0" allowfullscreen="allowfullscreen" allowtransparency="true" scrolling="no"></iframe>


??? question "mögliche Lösung für Übung 3"
	
	=== "Uebung3.java"
		```java linenums="1"
		package uebungen.uebung3;

		import javax.swing.JOptionPane;

		public class Uebung3 
		{
			public static int inputInt(int min, int max, String message)
			{
				boolean eingabeOk = false;
				int zahl = 0;
				// String message = "Geben Sie eine Zahl ein : ";
				while(!eingabeOk)
				{
					String eingabe1 = JOptionPane.showInputDialog(message);

					try {
						zahl = Integer.valueOf(eingabe1);
						if(zahl>=min && zahl<=max)
						{
							eingabeOk = true;
						}
						else
						{
							message = "Zahl muss zwischen " + min + " und " + max + " liegen!";
						}
					} 
					catch (NumberFormatException e) {
						message = "Eingabe war keine Zahl! Bitte Zahl eingeben";
					}
				}
				return zahl;
			}
			
			public static int inputInt(String message)
			{
				return inputInt(Integer.MIN_VALUE,Integer.MAX_VALUE, message);
			}
			
			public static int division(int divident, int divisor) throws ArithmeticException
			{
				int quotient = divident / divisor;
				return quotient;
			}
			
			public static void printDivision()
			{
				int zahl1 = inputInt("Geben Sie eine Zahl1 ein : ");
				boolean zahl2NotZero = false;
				String message = "Geben Sie eine Zahl2 ein : ";
				while(!zahl2NotZero)
				{
					int zahl2 = inputInt(message);
					int result = 0;
					try {
						result = division(zahl1, zahl2);
						zahl2NotZero = true;
					} catch (ArithmeticException e) {
						message = "Zahl2 darf nicht 0 sein!";
					}
					System.out.println(zahl1 + " / " + zahl2 + " = " + result);
				}
			}
			
			public static int quersumme(int zahl)
			{
				int ganz = zahl;
				int quersumme = 0;
				while(ganz>0)
				{
					int rest = ganz % 10;
					quersumme += rest;
					ganz = ganz / 10;
				}
				return quersumme;
			}
			
			public static void printUmrechnungSek()
			{
				int sek = inputInt("Anzahl Sekunden : ");
				int tage 	 = sek/(24*60*60);
				int stunden  = sek/(60*60) 	- (tage*24);
				int minuten  = sek/(60) 	- (tage*24*60) 		- (stunden*60);
				int sekunden = sek 			- (tage*24*60*60) 	- (stunden*60*60) - (minuten*60);
				
				String s = sek + " Sekunden sind ";
				if(tage>1) 	s += tage +" Tage, ";
				else if(tage==1) s+= "1 Tag, ";
				if(stunden>1) s += stunden +" Stunden, ";
				else if(stunden==1) s += "1 Stunden, ";
				if(minuten>1) s += minuten + " Minuten, ";
				else if(minuten==1) s += "1 Minute, ";
				if(sekunden>1) s += sekunden +" Sekunden.";
				else if(sekunden==1) s += "1 Sekunde.";
				System.out.println(s);
			}

			public static void main(String[] args) 
			{
				printDivision();
				int zahl = inputInt("Geben Sie eine Zahl ein");
				// System.out.println("Quersumme von " + zahl + " ist " + quersumme(zahl));
				JOptionPane.showMessageDialog(null, "Quersumme von " + zahl + " ist " + quersumme(zahl));
				
				printUmrechnungSek();
			}
		}
		```

	=== "module-info.java"
		```java linenums="1"		
		module SoSe2021 {
			requires java.desktop;
		}
		```


##### Übung 4 (Test-driven development)

??? "Übung 4"

	1. Implementieren Sie eine Methode `public static int strStr(String haystack, String needle)` durch testgetriebene Entwicklung. Die Methode gibt den Anfangsindex des ersten Auftretens von `needle` in `haystack` aus, z.B. 
	```bash
	// Beispiel 1
	Input: haystack = "hello", needle = "ll"
	Output: 2		// ll beginnt am Index 2

	// Beispiel 2
	Input: haystack = "aaaaa", needle = "bba"
	Output: -1		// bba kommt nicht vor 

	// Beispiel 3
	Input: haystack = "", needle = ""
	Output: 0		// "leerer" String ueberall, also auch bei 0
	```
	Wenn `needle` nicht in `haystack` enthalten ist, wird `-1` zurückgegeben. 

	2. Implementieren Sie eine Methode `public static int[][] permutations(int[] nums)` durch testgetriebene Entwicklung. Die Methode gibt ein Array von `int`-Arrays zurück, welches alle Permutationen der Zahlen aus `nums` enthält, z.B. 
	```bash
	// Beispiel 1
	Input: nums = [1,2,3]
	Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

	// Beispiel 2
	Input: nums = [0,1]
	Output: [[0,1],[1,0]]

	// Beispiel 3
	Input: nums = [1]
	Output: [[1]]
	```
	Sie dürfen folgende Annahmen treffen: 
		- `1 <= nums.length <= 6`
		- `-10 <= nums[i] <= 10`
		- `nums` enthält keine Doppelungen
	
	**Viel Spaß!**



## Zusatz


##### Test-driven development

??? "parseDouble(String)"

	1. In der [Aufgabe 2](../aufgaben/#aufgabe-2-myinteger) sollen Sie für die Klasse `MyInteger` eine Methode `parseInt(String s)` schreiben, die einen String `s` in eine `int`-Zahl umwandelt, wenn dies möglich ist. 
	2. In dieser Übung wollen wir eine solche (statische) Methode `parseDouble(String s)` für eine Klasse `MyDouble` testgetrieben entwickeln. Überlegen Sie sich dazu einige Strings, die Sie umwandeln wollen und die dazugehörigen erwarteten Ergebnisse. Es muss nicht vollständig implementiert werden. Es geht ums Prinzip. Mithilfe von `assertThrows()` können Sie übrigens prüfen, ob eine Exception geworfen wird (wenn `s` keiner Zahl entspricht) - siehe dazu z.B. [hier](https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions) oder [hier](https://mkyong.com/junit5/junit-5-expected-exception/).

	**Viel Spaß!**
