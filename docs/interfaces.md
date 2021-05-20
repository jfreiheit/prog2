# Interfaces

*Interfaces* sind auch abstrakte Klassen. Interfaces enthalten ausschließlich abstrakte Methoden (keine Methode darf implementiert sein). Interfaces beschreiben **Schnittstellen**. Für Interfaces wird nicht das Schlüsselwort `class`, sondern `interface` verwendet. Klassen erben nicht von Interfaces, sondern **implementieren** sie. Deshalb wird auch nicht das Schlüsselwort `extends`, sondern das Schlüsselwort `implements` verwendet. Während in Java nur von genau einer Klasse geerbt werden kann (also auch nur von genau einer abstrakten Klasse), kann eine Klasse kann beliebig viele Interfaces implementieren. 

Interfaces sind automatisch `abstract`, d.h. das Schlüsselwort `abstract` muss nicht angegeben werden. Auch die Methoden in Interfaces müssen nicht als abstrakt gekennzeichnet werden. Interfaces können, wie abstrakte Klassen auch, als Typen verwendet werden. 

| Abtrakte Klasse | Interface |
|-----------------|-----------|

