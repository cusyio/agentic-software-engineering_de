.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Slash-Befehle
=============

Mit Slash-Befehlen könnt ihr Coding-Agenten schnell und vorrangig über die
Tastatur steuern. Gebt im Editor ``/`` ein, um das Slash-Popup zu öffnen, wählt
einen Befehl aus, und euer Coding-Agent führt Aktionen wie das Wechseln von
Modellen, das ändern der Berechtigungen, das Löschen des Kontexts oder einen
bestimmten Workflow aus, ohne dass ihr das Eingabefenster verlassen müsst.

Hier die Links zur Dokumentation der Slash-Befehle für drei gängige
Coding-Agenten:

* `Claude Code Befehle <https://code.claude.com/docs/de/commands>`_
* `Curosr CLI Slash-Befehle
  <https://cursor.com/de/docs/cli/reference/slash-commands>`_
* `Slash commands in Codex CLI
  <https://developers.openai.com/codex/cli/slash-commands>`_

Skills vs. Slash-Befehle
------------------------

Auf den ersten Blick mögen Slash-Befehle und
:doc:`skill/index` ähnlich erscheinen. Sie agieren jedoch auf völlig
unterschiedlichen Ebenen und adressieren unterschiedliche Probleme. Die Wahl
zwischen ihnen beeinflusst, wie viel manuelle Arbeit ihr in jeder Sitzung
erledigen müsst.

Der offensichtlichste Unterschied ist, dass Skills automatisch aufgerufen
werden, je nachdem, was wir gerade tun, wohingegen Slash-Befehlen zunächst
eingegeben werden müssen.

Skills sind prozessorientiert
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sie legen Folgendes fest:

#. Was löst den Skill aus?
#. Welche Schritte sollen nach der Auslösung befolgt werden?
#. Welche Ausgabe oder Aktion soll erfolgen?

Dabei ist der erste Schritt entscheidend: euer Coding-Agent analysiert den
Kontext eurer aktuellen Tätigkeit – welche Dateien sind geöffnet, was wird
gerade geschrieben, wie lautet die Aufgabenbeschreibung – und gleicht diese
Informationen mit den verfügbaren Skills ab. Bei einer Übereinstimmung wird der
Skill ausgeführt. Genau das macht Skills ideal für wiederkehrende, vorhersehbare
Aufgaben, die konsistent erledigt werden sollen, ohne jedes Mal eine
Eingabeaufforderung zu erhalten.

Slash-Befehle sind explizite Anweisungen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ihr gebt :samp:`/{COMMAND}` ein, und euer Coding-Agent führt diesen Befehl aus.
Es wird nichts ausgeführt, was ihr nicht explizit angewiesen habt.

Beispielsweise könnt ihr in Claude Code mit ``/simplify`` und ``/batch`` gut
*Overengineering* beheben. Und wenn ihr mit einem überladenen Kontextfenster zu
kämpfen habt, könnt ihr einfach mit ``/compact`` `Context Rot
<https://www.understandingai.org/p/context-rot-the-emerging-challenge>`_
verhindern.

Benutzerdefinierte Slash-Befehle
--------------------------------

Ihr könnt auch benutzerdefinierte Slash-Befehle erstellen für für lange, häufig
verwendete Eingabeaufforderungen. So könnt ihr statt langer Anweisungen nur
einen kurzen Slash-Befehl eingeben. Hierzu könnt ihr in Claude Code einfach
:samp:`/{NEW_COMMAND}` eingeben. Benutzerdefinierte Slash-Befehle für Claude
Code werden im Verzeichnis :file:`.claude/commands/` gespeichert. Jede Datei
enthält den Text der Anweisung. Wenn ihr den Slash-Befehl eingebt, liest Claude
die Datei ein und führt sie aus. Erst an dieser Stelle beginnt die Grenze
zwischen Befehlen und Skills zu verschwimmen.

Wann solltet ihr Slash-Befehle verwenden?
-----------------------------------------

Verwendet einen Slash-Befehl, wenn die Aufgabe situationsabhängig, reaktiv oder
einmalig ist. Im folgenden einige Situationen, in denen Slash-Befehle die
richtige Wahl sind, weil sie nicht automatisch ausgelöst, sondern von euch
selbst entschieden werden sollten:

Den Kontext verringern
    Wenn eine Sitzung schon eine Weile läuft, das Kontextfenster immer länger
    wird und es zu Verzögerungen oder Inkonsistenzen kommt, kann mit
    ``/compact`` in Claude Code das Fenster gekürzt werden.
Den Kontext kurzzeitig ignorieren
    ``/btw`` ist für Situationen gedacht, in denen ihr Claude Code mitten in
    einer Aufgabe eine Frage stellen wollt, ohne dass der Arbeitsfluss
    unterbrochen wird.
Debugging einer bestimmten Ausgabe
    Wenn ihr überraschend eine Fehlermeldung erhaltet, könnt ihr ``/simplify``
    nur für diesen Abschnitt verwenden.
Routinemäßige Aufgaben
    Wenn ihr Claude Code einmal im Monat eure gesamte Codebasis auf bestimmte
    Muster überprüfen wollt, ist das ist ein Fall für einen Slash-Befehl, nicht
    für einen Skill.
