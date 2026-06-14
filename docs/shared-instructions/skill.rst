.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

``SKILL.md``
============

Da sich Coding-Agenten von einfachen Chat-Schnittstellen hin zur autonomen
Aufgabenausführung entwickeln, ist das :doc:`../context` zu einer entscheidenden
Herausforderung geworden. Agent-Skills bieten einen offenen Standard für die
Modularisierung von Kontexten, indem sie Anweisungen, ausführbare Skripte und
zugehörige Ressourcen wie :abbr:`z. B. (zum Beispiel)`
:doc:`testgetriebene Entwicklung <python-basics:test/tdd>` bündeln. Während
:doc:`agents` bei jeder Sitzung geladen wird, werden Skills nur bei Bedarf auf
der Grundlage ihrer Beschreibungen geladen, was den Token-Verbrauch reduziert
und Probleme wie die Erschöpfung des Kontextfensters oder die Überfrachtung der
Agentenanweisungen mindert.

Zu Beginn einer Sitzung können Coding-Agenten alle verfügbaren Skill-Dateien
scannen und für jede einzelne eine kurze Beschreibung aus der Markdown-Datei
auslesen. Dies ist sehr token-effizient: Jeder Skill beansprucht nur ein paar
Dutzend zusätzliche Token, wobei die vollständigen Details erst dann geladen
werden, wenn dies eine Aufgabe anfordert, bei deren Lösung der Skill helfen
kann.

.. seealso::
   * `Equipping agents for the real world with Agent Skills
     <https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills>`_
   * `Agent Skills-Dokumentation
     <https://platform.claude.com/docs/de/agents-and-tools/agent-skills/overview>`_
   * `Claude Skills Cookbook <https://github.com/anthropics/claude-cookbooks/tree/main/skills>`_
   * `Agent Skills Quickstart
     <https://agentskills.io/skill-creation/quickstart>`_
   * `Agent Skills Specification <https://agentskills.io/specification>`_
   * `Public repository for Agent Skills
     <https://github.com/anthropics/skills>`_

Skills und Programmierumgebung
------------------------------

Der Skills-Mechanismus hängt vollständig davon ab, dass das Modell Zugriff auf
ein Dateisystem, Werkzeuge zur Navigation darin und die Möglichkeit zur
Ausführung von Befehlen in dieser Umgebung hat.

Dies ist der größte Unterschied zwischen *Skills* und früheren Versuchen, die
Fähigkeiten von LLMs zu erweitern, wie beispielsweise :term:`MCP`. So bieten
Skills mehrere Vorteile:

* sie sind leistungsstark
* sie sind einfach zu erstellen
* und LLMs können in :doc:`sicheren Programmierumgebungen <../security>` zur
  Verfügung gestellt werden

Vorteile von Skills gegenüber MCP
---------------------------------

Skills sind effizienter
    Das `offizielle MCP von GitHub
    <https://github.com/github/github-mcp-server>`_ hingegen verbraucht allein
    schon Zehntausende von Kontext-Token, und sobald noch ein paar weitere
    hinzufügt werden, bleibt dem LLM kaum noch Platz, um tatsächlich nützliche
    Arbeit zu leisten.

    LLMs wissen hingegen, wie :samp:`{CLI-TOOL} --help` aufgerufen wird, sodass
    wir nicht viele Token darauf verwenden müssen, ihre Verwendung zu
    beschreiben – das Modell kann das später selbst herausfinden, wenn es nötig
    wird.

Skills können auch mit anderen Modellen verwendet werden
    Ihr könnt einfach einen Skills-Ordner nehmen und Codex CLI oder Gemini CLI
    darauf verweisen mit:

        Read `uv-tdd/SKILL.md` and then create a project structure

    Das wird funktionieren, auch wenn diese Tools und Modelle kein integriertes
    Wissen über Skills haben.

Skills sind sicherer
    Die Anweisungen können in :doc:`sicheren Programmierumgebungen
    <../security>` ausgeführt werden.

Skills sind einfacher
    :term:`MCP` ist eine vollständige Protokollspezifikation mit Hosts, Clients,
    Server, Ressourcen, Eingabeaufforderungen, Tools, Stichproben, Roots und
    drei verschiedenen Transportprotokollen: `stdio
    <https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#stdio>`_,
    `streamable HTTP
    <https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#streamable-http>`_
    und ursprünglich `HTTP with SSE
    <https://modelcontextprotocol.io/specification/2024-11-05/basic/transports#http-with-sse>`_.
    Skills hingegen basieren auf Markdown mit ein wenig YAML-Metadaten und
    einigen optionalen Skripten, die in der jeweiligen Umgebung ausführbar sind.
    Sie kommen damit der Idee von LLMs sehr viel näher, da einfach Text
    eingegeben werden kann, den das Modell interpretiert.

.. seealso::
   * `Skills compared to MCP
     <https://simonwillison.net/2025/Oct/16/claude-skills/#skills-compared-to-mcp>`_
     by Simon Willison, posted on 16th October 2025

Skill-Plugins
-------------

Mit ihrer zunehmenden Beliebtheit hat sich auch das umgebende Ökosystem
erweitert. `Plugin-Marktplätze
<https://code.claude.com/docs/de/plugin-marketplaces>`_ entstehen als
Möglichkeit, Skills zu versionieren und zu teilen, und in zahlreichen Projekten
wird untersucht, wie die Effektivität von Skills bewertet werden kann. Dennoch
solltet ihr nicht ungeprüft Skills von Drittanbietern verwenden, da diese
ernsthafte `Sicherheitsrisiken in der Software-Lieferkette
<https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/>`_ mit sich
bringen.

.. seealso::
   * `Entdecken und installieren Sie vorgefertigte Plugins über Marktplätze
     <https://code.claude.com/docs/de/discover-plugins>`_
   * `Plugins erstellen <https://code.claude.com/docs/de/plugins>`_
