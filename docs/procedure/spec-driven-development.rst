.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Spec-Driven-Development
=======================

Entwicklungsteams sehen sich zunehmend mit Herausforderungen hinsichtlich der
Vorhersehbarkeit und Wartbarkeit konfrontiert, wenn Anforderungen und Kontext
ausschließlich in kurzlebigen Chat-Verläufen gespeichert sind. Um diesem Problem
zu begegnen, sind im letzten Jahr Tools für die spezifikationsgesteuerte
Entwicklung (:abbr:`SDD (Spec-Driven-Development)`) entstanden. Auch wenn sich
die Definition des Begriffs noch weiterentwickelt, bezieht er sich im
Allgemeinen auf Workflows, die mit einer strukturierten Funktionsspezifikation
beginnen und diese dann in mehreren Schritten in kleinere Teile, Lösungen und
Aufgaben aufschlüsseln. Die Spezifikation kann ein einzelnes Dokument sein oder
eine Reihe von Dokumenten oder strukturierte Artefakte, die verschiedene
funktionale Aspekte erfassen.

Wir untersuchten bereits einige Tools, die spezifikationsgesteuerten Entwicklung
unterschiedlich interpretiert haben:

`Kiro <https://kiro.dev>`_
    führt Entwicklungsteams durch drei Workflow-Phasen – Anforderungen, Design
    und Aufgabenerstellung
`GitHub Spec Kit <https://github.github.com/spec-kit/>`_
    folgt einem ähnlichen dreistufigen Prozess, bietet jedoch eine
    umfangreichere Koordination, konfigurierbare Eingabeaufforderungen und eine
    *Constitutional Compliance* (Verfassungskonformität), in der unveränderliche
    Prinzipien definiert werden.
`Tessl Framework <https://tessl.io>`_
    verfolgt einen radikaleren Ansatz, bei dem nicht der Code, sondern die
    Spezifikation selbst zum gepflegten Artefakt wird.

Die Arbeitsabläufe sind jedoch bei allen drei Tools aufwendig und sehr
spezifisch: je nach Umfang und Art der Aufgabe verhalten sich diese Tools sehr
unterschiedlich – einige erzeugen umfangreiche Spezifikationsdateien, die schwer
zu überprüfen sind, bei anderen sind die :abbr:`PRDs (Product Requirements
Documents)` oder User Stories sehr unklar.

.. seealso::
   * Birgitta Böckeler: `Understanding Spec-Driven-Development: Kiro, spec-kit,
     and Tessl
     <https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html>`_

OpenSpec
--------

`OpenSpec <https://openspec.dev>`_ ist ein Open-Source-SDD-Framework, das eine
schlanke Spezifikationsschicht einführt und sicherstellt, dass Menschen und
Coding-Agenten sich darüber abstimmen, was entwickelt werden soll, bevor Code
generiert wird.

Der Standard-Workflow von OpenSpec `OPSX Workflow
<https://github.com/Fission-AI/OpenSpec/blob/main/docs/opsx.md>`_ zeichnet ein
minimalistischer Workflow aus, der sich oft auf die folgenden vier Aktionen,
nicht Phasen, reduziert: *Create, Implement, Update, Archive* – jede von ihnen
kann zu jedem Zeitpunkt ausgeführt werden. Dabei legt OpenSpec den Fokus auf
Spezifikationsänderungen, anstatt von vornherein eine vollständige Spezifikation
definieren zu müssen, wodurch es sich gut für `Brownfield
<https://de.wikipedia.org/wiki/Brownfield_(Softwareentwicklung)>`_-Projekte
eignet.

OpenSpec grenzt sich damit angenehm ab vom der schwerfälligen und starren
:abbr:`BMAD (Breakthrough Method for Agile Ai Driven Development)`-Methode.
OpenSpec könnte durch das iterative Vorgehen und die Tool-Unabhängigkeit ein
entwicklungsfreundliches Framework sein, das wir aktuell noch weiter evaluieren.

.. seealso::
   * `OpenSpec Documentation
     <https://github.com/Fission-AI/OpenSpec/blob/main/docs/README.md>`_
   * `Getting Started
     <https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md>`_
   * `Core Concepts at a Glance
     <https://github.com/Fission-AI/OpenSpec/blob/main/docs/overview.md>`_
