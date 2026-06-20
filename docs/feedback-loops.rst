.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Rückkopplungsschleifen
======================

Coding-Agenten beenden ihre Arbeit, wenn ihre Arbeit „fertig“ erscheint. Jedoch
erst wenn an den Coding-Agenten zurückgemeldet wird, dass eure Testsuite, euer
Linter :abbr:`u. a. (und andere)` fehlerfrei durchliefen, ist die
Feedback-Schleife tatsächlich vollständig und die Aufgabe abgeschlossen.

Python unterstützt euch dabei durch hervorragende Fehlermeldungen, die direkt an
den Coding-Agenten zurückgegeben werden können; je mehr Hinweise oder sogar
Lösungsvorschläge sie enthalten, desto besser.

.. seealso::
   * :pep:`PEP 657 – Include Fine Grained Error Locations in Tracebacks <657>`

Sobald die Überprüfung existiert, solltet ihr festlegen, wie streng sie den
Stopp steuert:

In einer einzigen Eingabeaufforderung
    Bittet den Coding-Agenten, die Überprüfung auszuführen und in derselben
    Nachricht zu iterieren. Dies funktioniert heute bei jeder Aufgabe.
Über eine Sitzung hinweg
    In Claude Code könnt ihr die die Überprüfung auch als `/goal-Bedingung
    <https://code.claude.com/docs/de/goal>`_ festlegen. Ein separater Evaluator
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

.. seealso::
   * `Don’t waste your back pressure
     <https://banay.me/dont-waste-your-backpressure/>`_

Beispiel
--------

.. code-block:: md

   ## Documentation review process

   1. Write your documentation in accordance with the guidelines in `DOCSTRINGS.md`
   2. Check the documentation against the checklist:
      - Ensure consistency in terminology
      - Make sure the examples follow the standard format
      - Ensure that all required sections are present
   3. If any issues are identified:
      - Note down each issue with a specific reference to the relevant section
      - Revise the documentation
      - Review the checklist again
   4. Only proceed once all requirements have been met
   5. Finalise the document and save it

Überprüfen und Überarbeiten
---------------------------

Analog zu :doc:`testgetriebener Entwicklung <python-basics:test/tdd>` sollten
zunächst die Ergebnisse eures Skills überprüft werden. Nur so könnt ihr
sicherstellen, dass euer Skill echte und nicht nur imaginäre Probleme löst:

#. Lücken entdecken

   Lasst euren Coding-Agenten eine repräsentative Aufgabe ohne Skill ausführen
   und dokumentiert den konkreten Fehler oder den fehlenden Kontext

#. Erstellt Tests

   Entwickelt mehrere Szenarien, die diese Lücken testen

#. Legt eine Grundlinie fest

   Messt die Leistung des Coding-Agenten ohne den Skill

#. Verfasst minimale Anweisungen

   Die Anweisung sollte so kurz wie möglich sein, um die Lücken zu schließen und
   die Tests zu bestehen

#. Iteriert

   Führt Tests durch, vergleicht diese mit der Grundlinie und verfeinert den
   Skill

#. Beobachtet, wie euer Coding-Agent durch die Skills navigiert

   Achtet bei der Iteration an den Skills darauf, wie euer Coding-Agent diese in
   der Praxis tatsächlich nutzt. Achtet insbesondere auf folgendes:

   Unerwartete Erkundungspfade
       Werden eure Dateien in der von euch erwarteten Reihenfolge gelesen?
   Übersehene Verknüpfungen
       Folgt euer Coding-Agent den von euch angegebenen Verweisen?
   Übermäßige Beachtung
       Wird wiederholt dieselbe Datei gelesen, sollte :abbr:`evtl. (eventuell)`
       deren Inhalt in die :file:`SKILL.md`-Datei verschoben werden.
   Ignorierte Inhalte
       Wird nie auf eine verschachtelte Datei zugegriffen, ist diese häufig
       unwichtig oder ihre Bedeutung ist nicht hinreichend gekennzeichnet.
