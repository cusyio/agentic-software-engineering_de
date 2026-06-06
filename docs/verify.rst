Verifizieren
============

Coding-Agenten beenden ihre Arbeit, wenn ihre Arbeit „fertig“ erscheint. Erst
wenn an den Coding-Agenten zurückgemeldet wird, dass eure Testsuite, der Linter
:abbr:`u. a. (und andere)` fehlerfrei durchliefen, ist die Feedback-Schleife
vollständig und die Aufgabe abgeschlossen.

Sobald die Überprüfung existiert, solltet ihr festlegen, wie streng sie den
Stopp steuert:

In einer einzigen Eingabeaufforderung
    Bittet den Coding-Agenten, die Überprüfung auszuführen und in derselben
    Nachricht zu iterieren. Dies funktioniert heute bei jeder Aufgabe.
Über eine Sitzung hinweg
    In Claude Code könnt ihr die die Überprüfung auch als `/goal-Bedingung
    <https://code.claude.com/docs/en/goal>`_ festlegen. Ein separater Evaluator
    überprüft sie nach jedem Zug erneut, und Claude arbeitet so lange weiter,
    bis sie erfüllt ist. Dies sorgt dafür, dass ein unbeaufsichtigter Lauf auch
    ohne euer Zutun korrekt abgeschlossen wird.
Als deterministisches Kriterium
    Ein Stop-Hook führt eure Prüfung als Skript aus und verhindert, dass der
    Schritt beendet wird, bevor er bestanden ist.

    Lasst den Coding-Agenten Beweise vorlegen, anstatt den Erfolg einfach zu
    behaupten; dies kann die Testausgabe sein oder einen Screenshot des
    Ergebnisses. Das Überprüfen der Beweise geht schneller, als die
    Verifizierung selbst erneut durchzuführen, und funktioniert auch bei
    Sitzungen, die ihr nicht beobachtet habt.

Durch eine zweite Meinung
    Ein Verifizierung-Subagent oder ein dynamischer Workflow, der seine eigenen
    Ergebnisse überprüft, lässt ein neues Modell versuchen, das Ergebnis zu
    widerlegen, sodass der Agent, der die Arbeit ausführt, nicht derjenige ist,
    der sie bewertet.
