.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Skills und Programmierumgebung
==============================

Der Skills-Mechanismus hängt vollständig davon ab, dass das Modell Zugriff auf
ein Dateisystem, Werkzeuge zur Navigation darin und die Möglichkeit zur
Ausführung von Befehlen in dieser Umgebung hat.

Dies ist der größte Unterschied zwischen *Skills* und früheren Versuchen, die
Fähigkeiten von LLMs zu erweitern, wie beispielsweise :term:`MCP`. So bieten
Skills mehrere Vorteile:

* sie sind leistungsstark
* sie sind einfach zu erstellen
* und LLMs können in :doc:`sicheren Programmierumgebungen <../../security>` zur
  Verfügung gestellt werden
