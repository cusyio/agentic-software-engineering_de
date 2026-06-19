.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Skills
======

Da sich Coding-Agenten von einfachen Chat-Schnittstellen hin zur autonomen
Aufgabenausführung entwickeln, ist das :doc:`../../context` zu einer
entscheidenden Herausforderung geworden. Agent-Skills bieten einen offenen
Standard für die Modularisierung von Kontexten, indem sie Anweisungen,
ausführbare Skripte und zugehörige Ressourcen wie :abbr:`z. B. (zum Beispiel)`
:doc:`testgetriebene Entwicklung <python-basics:test/tdd>` bündeln. Während
:doc:`../agents` bei jeder Sitzung geladen wird, werden Skills nur bei Bedarf
auf der Grundlage ihrer Beschreibungen geladen, was den Token-Verbrauch
reduziert und Probleme wie die Erschöpfung des Kontextfensters oder die
Überfrachtung der Agentenanweisungen mindert.

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

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   env
   skills-mcp
   skill-md-structure
   directory-structure
   security
   plugins
