.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Gemeinsame Context-Anweisungen
==============================

Je mehr Erfahrung Teams im Umgang mit Coding-Agenten sammeln, desto weiniger
sollten einzelne Mitglieder eines Software-Entwicklungsteams Prompts von Grund
auf neu erfassen. Wir empfehlen daher kuratierte, gemeinsam genutzte Anweisungen
für Software-Teams mit gemeinsam genutzten Entwicklungsressourcen.

Anfangs konzentrierte sich diese Vorgehensweise auf die Pflege von universellen
Prompt-Bibliotheken für gängige Aufgaben. Mittlerweile lassen sich solche
Anweisungen in Dateien wie `CLAUDE.md
<https://code.claude.com/docs/en/memory#how-claude-md-files-load>`_ oder
`AGENTS.md <https://agents.md/>`_ im :doc:`Git
<Python4DataScience:productive/git/index>`-Repository gemeinsam verwalten.

    Das Vorhandensein einer :file:`AGENTS.md`-Datei geht mit einem geringeren
    Token-Verbrauch und einer schnelleren Aufgabenbearbeitung bei realen
    Pull-Requests einher.

– `On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents
<https://arxiv.org/abs/2601.20404>`_

.. warning::
   Anthropic empfiehlt 200 Zeilen als Obergrenze, siehe `My CLAUDE.md is too
   large <https://code.claude.com/docs/en/memory#my-claude-md-is-too-large>`_.

Cross-Agent-Konfiguration
-------------------------

Wenn ihr in euren Projekten mehrere Agenten unterstützen
wollt, könnt ihr einfach in der :file:`CLAUDE.md` mit ``@AGENTS.md`` auf die
Konfiguration in eurer :file:`AGENTS.md` verweisen.

.. seealso::
   `Claude Code Docs: AGENTS.md
   <https://code.claude.com/docs/en/memory#agents-md>`_

Allgemeine Vorgehensweise
-------------------------

Ich lasse mir üblicherweise gerne zunächst fünf Lösungsvorschläge machen, bevor
der mutmaßlich effektivste umgesetzt wird:

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 6-7

.. _uv:

uv
--

Viele Agenten verwenden üblicherweise ``pip``, wenn Pakete installiert oder
Skripte ausgeführt werden sollen. Eine `CLAUDE.md
<https://code.claude.com/docs/en/memory#how-claude-md-files-load>`_- oder
:file:`AGENTS.md`-Datei im Stammverzeichnis eures Projekts
überschreibt diese Standardeinstellung, sodass in jeder Sitzung stattdessen
:doc:`Python4DataScience:productive/envs/uv/index` verwendet wird. Eine mögliche
Konfiguration für eine :file:`AGENTS.md`-Datei ist:

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 10-12

.. seealso::
   * :ref:`python-basics:uv`
   * :doc:`Python4DataScience:productive/envs/uv/claude-cursor`

.. _code-quality:

Code-Qualität und Linting
-------------------------

Üblicherweise lassen wir die Code-Qualität und Syntax :abbr:`z. B. (zum
Beispiel)` überprüfen mit :doc:`Python4DataScience:productive/qa/ruff`, `ty
<https://docs.astral.sh/ty/>`_, `prek <https://prek.j178.dev/>`_ und
:doc:`Python4DataScience:productive/qa/wily`.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 14-16

Typisierung
-----------

Vermeidet übermäßige Typ-Umwandlungen. Wenn ihr feststellt, dass im Code häufig
Typumwandlungen vorkommen, sollte der Code so umgestaltet werden, dass
geeignetere Typen verwendet werden. Typumwandlungen sollten idealerweise nur an
Schnittstellen zu externen Systemen vorgenommen werden. Verwendet :doc:`Type
Hints <python:library/typing>` für alle Funktionsparameter und Rückgabetypen.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 18-20

.. _testing:

Testen
------

Viele unserer Projekte sind :doc:`python-basics:test/tdd` mit
:doc:`python-basics:test/pytest/index` und :doc:`python-basics:test/hypothesis`.
Darüberhinaus sollte :doc:`Mocking <python-basics:test/mock>` und
:ref:`python-basics:monkeypatch-fixture` vermieden werden.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 22-30

.. _documentation:

Dokumentation
-------------

Wir verwenden in allen Funktionen und Klassen
:doc:`python-basics:document/sphinx/docstrings` im :ref:`Google-Stil
<python-basics:google-docstrings>`. Außerdem schreiben wir üblicherweise
:doc:`Doctests <python-basics:document/doctest>` um die Dokumentation zu
überprüfen.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 32-34

.. _logging:

Logging
-------

Üblicherweise nutzen wir :doc:`python-basics:logging/index` um Einblicke in
Fehler zu erhalten. Wir wollen im Code keine ``print``-Statements zum Debuggen.
Wir verwenden Logging jedoch nicht, um Stack-Traces zu verbergen, wenn der
Fehler ohnehin auftritt. Auch sollen
:doc:`python-basics:control-flow/exceptions` nicht verborgen werden. Wenn eine
Exception doch abgefangen werden soll, sollte sie zumindest geloggt werden.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 36-38

Kommandozeilenwerkzeuge
-----------------------

Bei Kommandozeilenwerkzeugen wollen wir üblicherweise ein `--verbose`-Flag, das
Log-Ausgaben liefert, die für die Fehlerbehebung nützlich sind.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 40-41
