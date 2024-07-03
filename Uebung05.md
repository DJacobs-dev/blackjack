# Uebung 5

## Aufgabe 1

### Croupier

Croupier besitzt:
Attribute:
- Deck aus Karten
- Hand aus Karten
- 1 Player
- 1 Card Counter
- 1 Game:
  - game state
  - turn

Methoden:
- receiveLines() -> Grundlegende Kommunikation mit Player und Card Counter
- createDeck()
- shuffleDeck()
- dealCard()
- calculateHand()
- calculateWin()
- askCardCounter()
- evaluatePlayer()
- removePlayer()
- handlePlayer()

### Player

Player besitzt:
Attribute:
- Hand aus Karten
- Money
- Croupier

Methoden:
- register()
- receiveLines() -> Grundlegende Kommunikation mit Croupier
- chooseAction()
- placeBet()


### Card Counter

Card Counter besitzt:
Attribute:
- Croupier
- Player
- DeckCount
- Stats

Methoden:
- register()
- receiveLines() -> Grundlegende Kommunikation mit Croupier
- getDeckCount()

## Aufgabe 2

### Allgemein

Grundlegende Kommunikation:
Den Ausgang bildet die Musterlösung zur Übung03 UDP_Chat.java.
Die Klassen Player, Croupier und CardCounter sind Abwandlungen der Grundklasse UDP_Chat.java.

Voraussetzungen für den Start eines Spiels:
- CardCounter muss beim Croupier registriert sein
- Player muss beim Croupier registriert sein

Ablauf des Spiels:
- Croupier teilt CardCounter die Anzahl der Decks mit
- Croupier erstellt Deck
- Croupier fordert Spieler auf, Einsatz zu tätigen
- Wenn Spieler Einsatz getätigt hat, teilt Croupier Karten aus
- Spieler entscheidet, ob er noch eine Karte möchte oder nicht
- Croupier prüft, ob Spieler BlackJack oder überkauft hat
- Falls ja, wird Spiel ausgezahlt
- Falls nein, sagt Spieler hit oder stand, bis er stand sagt
- Croupier zieht Karten, bis er 17 oder mehr hat (Soft 17 wird später implementiert)
- Croupier wertet Spiel aus und zahlt aus bzw. kassiert
- Croupier übermittelt Statistik des Spiels an CardCounter und fordert Empfehlung
- CardCounter gibt Empfehlung an Croupier
- Croupier entscheidet, ob Spieler aus Casino geworfen wird
- Falls nein, beginnt neues Spiel

Auf welche Dinge müssen Sie achten, um sicherzustellen, dass die gesendeten Nachrichten beim
Empfänger ankommen?
- Nachrichten müssen korrekt formatiert sein
- Empfangsbestätigung verlangen

Wie können Sie Fehlkommunikation vermeiden (wenn z.B. ein Paket falsch
interpretiert wird)?
- Prüfsumme oder
- Bei Empfangsbestätigung prüfen, ob Nachricht korrekt angekommen ist
- ggfs. Nachricht erneut senden

Wie können Sie sicherstellen, dass Ihr Programm mit den 2 anderen Programmen
kommunizieren kann und die Pakete so interpretiert, wie Sie das geplant haben?

- Croupier dient als Server und steuert den Kommunikations- und Spielablauf
- Gemeinsame Standards definieren
  - Nachrichtenformat:
    1. Wort: Aktion (register, deal, hit, stand, evaluate, recommend, remove, stats, etc.)
    2. Wort: Parameter/Werte