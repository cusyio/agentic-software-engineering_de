.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Context Engineering
===================

Zunächst als Optimierungstaktik gedacht, hat sich Context Engineering zu einem
grundlegenden architektonischen Aspekt moderner KI-Systeme entwickelt. Im
Gegensatz zum Prompt Engineering, das sich auf die Formulierung konzentriert,
konzentriert sich Context Engineering bewusst auf die Informationsumgebung der
KI.

Da Agenten immer komplexere Aufgaben bewältigen müssen, führt das Einfügen von
Rohdaten in den Kontext zu `Context rot
<https://www.understandingai.org/p/context-rot-the-emerging-challenge>`_ und
einer Verschlechterung der Schlussfolgerungen. Um dem entgegenzuwirken, wechseln
wir von statischen, monolithischen Prompts zur schrittweisen Offenlegung des
Kontexts: anstatt alle Anweisungen und Referenzen, die ein Agent benötigen
könnte, im Voraus zu laden, beginnen wir mit einem schlanken Index der
verfügbaren Informationen – der Coding-Agent bestimmt, welche Prompts oder
Kontexte relevant sind, und ruft nur das Nötige ab, wodurch sich das
`Signal-Rausch-Verhältnis
<https://de.wikipedia.org/wiki/Signal-Rausch-Verh%C3%A4ltnis>`_ deutlich
verbessert.

Strategien
----------

Um die Qualität der Coding-Agenten möglichst hoch zu halten, sollte sich deren
Kontext auf die aktuelle Problemstellung konzentrieren. Die folgenden Strategien
können euch helfen, den Kontext klein zu halten.

Verwaltet den Kontext proaktiv
    * Lasst euch die aktuelle Token-Nutzung ständig anzeigen.
    * Löscht den Kontext wenn ihr zu einer anderen Aufgabe wechselt. Meist habt
      ihr auch die Möglichkeit, den Kontext vorher abzuspeichern, sodass ihr
      später wieder darauf zurückgreifen könnt. :abbr:`Ggf. (Gegebenenfalls)`
      könnt ihr dabei noch Hinweise geben, was bei der Zusammenfassung
      beibehalten werden soll.

Reduziert den Overhead des :abbr:`MCP (Model Context Protocol)`)-Servers
    * MCP-Tool-Definitionen werden üblicherweise zurückgestellt, sodass nur die
      Tool-Namen im Kontext verfügbar ist, bis der Coding-Agent ein bestimmtes
      Tool verwendet.
    * Verwendet nach Möglichkeit CLI-Tools – sie sind immer noch
      kontexteffizienter als MCP-Server, da sie keine tool-spezifischen Einträge
      hinzufügen. Coding-Agenten können CLI-Befehle direkt ausführen.
    * Deaktiviert nicht verwendete MCP-Server.
Installiert die sprachspezifischen Plugins
    Diese Plugins ermöglichen den Coding-Agenten präzise Symbolnavigation
    anstelle textbasierter Suchen und reduzieren so unnötige Datei-Lesevorgänge
    beim Durchsuchen von unbekanntem Code. Installierte Sprachserver melden
    zudem Typfehler automatisch nach Änderungen, sodass Coding-Agenten Fehler
    erkennen können, ohne einen Compiler ausführen zu müssen. In Claude Code ist
    dies ``pyright-lsp`` für den Pyright-Language-Server.
Verlagert die Arbeit zu Hooks und Skills
    * Benutzerdefinierte Hooks können Daten vorverarbeiten, bevor sie an
      Coding-Agenten weitergeleitet werden. Anstatt den Coding-Agenten Log-Datei
      nach dem Log-Level ``ERROR`` suchen zu lassen und nur die entsprechenden
      Zeilen zurückzugeben, wodurch der Kontext von Zehntausenden Tokens auf
      Hunderte reduziert wird.
    * `Agent Skills <https://agentskills.io/home>`_ vermitteln Fachwissen,
      sodass der Coding-Agent es nicht selbst recherchieren muss. Ein
      ``codebase-overview``-Skill könnte beispielsweise die Architektur eures
      Projekts, wichtige Verzeichnisse und Namenskonventionen beschreiben. Wenn
      der Coding-Agent den Skill aufruft, erhält es diesen Kontext sofort,
      anstatt Token dafür zu verbrauchen, mehrere Dateien lesen zu müssen um die
      Struktur zu verstehen.

Weitere Techniken versuchen, dieses Verhältnis weiter zu verbessern:

`Prompt-Caching <https://platform.claude.com/docs/en/build-with-claude/prompt-caching>`_
    stellt statische Anweisungen vorab bereit, was Kosten senkt und die Zeit bis
    zum ersten Token verkürzt.
Dynamic retrieval
    geht über grundlegende :abbr:`RAG (Retrieval-Augmented Generation)` hinaus,
    indem es Tools auswählt und nur die notwendigen :abbr:`MCP (Model Context
    Protocol)`-Server lädt, wodurch eine unnötige Kontexterweiterung vermieden
    wird.
`Context Graphs <https://trustgraph.ai/guides/key-concepts/context-graphs/>`_
    modellieren institutionelles Schlussfolgern – wie Richtlinien, Ausnahmen und
    Präzedenzfälle – als strukturierte, abfragbare Daten. Techniken zum
    Kontextmanagement nutzen *Stateful Compression*, und Sub-Agenten, um
    Zwischenschritte in lang andauernden Workflows zusammenzufassen.
