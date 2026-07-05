.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Skills vs. MCP
==============

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

    .. code-block:: md

       Read `skills/uv-tdd/SKILL.md` and then create a project structure

    Das wird funktionieren, auch wenn diese Tools und Modelle kein integriertes
    Wissen über Skills haben.

Skills sind sicherer
    Die Anweisungen können in :doc:`sicheren Programmierumgebungen
    <../../security/sandboxing/index>` ausgeführt werden.

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
   * Simon Willison: `Skills compared to MCP
     <https://simonwillison.net/2025/Oct/16/claude-skills/#skills-compared-to-mcp>`_,
     16. Oktober 2025
