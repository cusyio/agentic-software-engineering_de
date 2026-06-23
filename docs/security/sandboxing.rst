.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sandboxing
==========

Da Coding-Agenten zunehmend autonom Code ausführen, Builds durchführen und mit
dem Dateisystem interagieren können, birgt der uneingeschränkte Zugriff auf eine
Entwicklungsumgebung reale Risiken, die bis hin zur Offenlegung von Anmeldedaten
reichen. Sandboxing sollte daher die übliche Vorgehensweise sein und nicht nur
eine optionale Erweiterung.

Einige Coding-Agenten erlauben zwar, Berechtigungen festzulegen, :abbr:`z. B.
(zum Beispiel)` automatisch, mit einer Whitelist oder in einer Sandbox. Diese
Berechtigungen bleiben jedoch anfällig für `Lethal Trifecta
<https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_, wenn euer
Coding-Agent Zugriff auf private Daten hat, nicht vertrauenswürdigen Inhalten
ausgesetzt ist und extern kommunizieren kann.

In diesen Fällen definieren wir unsere eigene Sandbox, sodass der Code der
Agenten in isolierten Umgebungen mit eingeschränktem Dateisystemzugriff,
kontrollierter Netzwerkkonnektivität und begrenzter Ressourcennutzung ausgeführt
wird.

Mittlerweile gibt es ein breites Spektrum von Sandboxing-Optionen. Über die
integrierten Sandbox-Modi der Coding-Agenten hinaus gibt es verschiedene
Optionen im Spannungsfeld zwischen kurzlebigen und dauerhaften Lösungen:

.. include:: ../glossary.rst
   :start-after: start-containers:
   :end-before: end-containers:

Über die grundlegende Isolierung hinaus sollten Entwicklungsteams die
praktischen Anforderungen an eine produktive Sandbox berücksichtigen. Dazu
gehören alle für die Entwicklung und das Testen erforderlichen Komponenten sowie
eine sichere und unkomplizierte Authentifizierung bei externen Diensten.
Entwicklungsteams benötigen Portweiterleitung sowie ausreichende CPU- und
Speicherressourcen für die Arbeitslasten der Coding-Agenten. Ob die Sandbox
standardmäßig kurzlebig oder zur Wiederherstellung von Sitzungen dauerhaft sein
soll, ist eine Designentscheidung, die von den Prioritäten des Teams in Bezug
auf Sicherheit, Kosten und Kontinuität der Arbeitsabläufen abhängt.

.. seealso::
   * Bundesamt für Sicherheit in der Informationstechnik (BSI): `Evasion-Attacks
     auf LLMs – Eine Checkliste zur Härtung des LLM-Systems
     <https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/KI/Evasion-Angriffe_auf_LLMs-Checkliste.pdf>`_
