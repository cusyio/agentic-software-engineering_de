.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sicherheit
==========

Skills sollten üblicherweise selbst erstellt werden. Skills verleihen
Coding-Agenten neue Fähigkeiten, die sie sehr leistungsstark machen, aber auch,
euren Coding-Agenten dazu veranlassen können, Tools aufzurufen oder Code auf
eine Weise auszuführen, die nicht den vorgeblichen Zwecken entsprechen.

Wichtige Sicherheitsmaßnahmen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Überprüft alle im Skill enthaltenen Dateien: :file:`SKILL.md`, Skripte, Bilder
  und andere Ressourcen.
* Achtet auf ungewöhnliche Muster wie unerwartete Netzwerkaufrufe,
  Dateizugriffsmuster oder Vorgänge, die nicht dem angegebenen Zweck des Skills
  entsprechen.
* Skills, die Daten von externen URLs abrufen, stellen ein besonderes Risiko
  dar, da die abgerufenen Inhalte bösartige Anweisungen enthalten können. Selbst
  vertrauenswürdige Skills können kompromittiert werden, wenn sich ihre
  externen Abhängigkeiten im Laufe der Zeit ändern.
* Bösartige Skills können Tools (Dateioperationen, Bash-Befehle, Codeausführung)
  auf schädliche Weise aufrufen.
* Skills mit Zugriff auf sensible Daten könnten so konzipiert sein, dass sie
  Informationen an externe Systeme weitergeben.
* Verwendet nur Skills aus vertrauenswürdigen Quellen. Seid besonders
  vorsichtig, wenn Skills in Produktionssysteme integriert werden sollen.
