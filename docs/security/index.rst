.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sicherheit
==========

Da moderne KI-Agenten autonom agieren und Tools aufrufen können, ist ein
mehrschichtiges Sicherheitskonzept zwingend erforderlich, das böswillige oder
versehentliche Eingaben herausfiltert und verhindert, dass Prompt-Injektions
eure Daten und Infrastruktur gefährden. Die effektivsten
KI-Sicherheitsarchitekturen lassen sich in zwei Hauptkategorien unterteilen:

:doc:`guardrails`
    überwachen, bereinigen und filtern, was ein KI-Modell liest und schreibt.
    Sie überprüfen eingehende Daten auf gängige Jailbreak-Versuche,
    Prompt-Injektions oder bösartige Code. Generierte Antworten werden ebenfalls
    überprüft, bevor sie zurückgegeben werden, um sicherzustellen, dass die
    Ausgabe sicher und konform ist.
:doc:`sandboxing/index`
    schützt eure Systeme, falls die Guardrails umgangen werden oder versagen.
    Die Coding-Agenten werden abgeschottet, um zu verhindern, dass bösartiger
    Code eure Umgebung verändert oder Daten abfließen lässt.

.. seealso::
   * :doc:`Skill-Sicherheit <../shared-instructions/skill/security>`
   * Bundesamt für Sicherheit in der Informationstechnik (BSI): `Evasion-Attacks
     auf LLMs – Eine Checkliste zur Härtung des LLM-Systems
     <https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/KI/Evasion-Angriffe_auf_LLMs-Checkliste.pdf>`_

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   guardrails
   sandboxing/index
