# Übungen

##### Übung 1 (Git)

??? "Übung 1"

	1. Erstellen Sie sich einen GitHub-Account (oder wählen Sie einen anderen Git-Diensteanbieter)
	2. Erstellen Sie sich dort ein zentrales Repository 
	3. Richten Sie Ihren `workspace`, in dem Sie alle Ihre Entwicklungen in diesem Semester durchführen wollen (Übungen, Aufgaben, ...) als lokales Git-Repository ein
	4. Synchroniseren Sie Ihr lokales und Ihr zentrales Git-Repository

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




