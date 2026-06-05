Setup
=====

Ihr könnt Claude Code in der `CLAUDE.md
<https://code.claude.com/docs/en/memory#how-claude-md-files-load>`_-Datei
konfigurieren. Die meisten anderen Agenten verwenden hingegen `AGENTS.md
<https://agents.md/>`_. Üblicherweise solltet Ihr jedoch nicht einfach eine
bestehende Konfigurationsdatei übernehmen, sondern sie auf Grundlage eurer
individuellen Anforderungen selbst entwickeln.

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
   :lines: 1-2

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
   :lines: 4-7

.. seealso::
   * :ref:`python-basics:uv`
   * :doc:`Python4DataScience:productive/envs/uv/claude-cursor`

Code-Qualität und Linting
-------------------------

Üblicherweise lassen wir die Code-Qualität und Syntax :abbr:`z. B. (zum
Beispiel)` überprüfen mit :doc:`Python4DataScience:productive/qa/ruff`, `ty
<https://docs.astral.sh/ty/>`_, `prek <https://prek.j178.dev/>`_ und
:doc:`Python4DataScience:productive/qa/wily`.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 9-11

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
   :lines: 13-15

Testen
------

Viele unserer Projekte sind :doc:`python-basics:test/tdd` mit
:doc:`python-basics:test/pytest/index` und :doc:`python-basics:test/hypothesis`.
Darüberhinaus sollte :doc:`Mocking <python-basics:test/mock>` und
:ref:`python-basics:monkeypatch-fixture` vermieden werden.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 17-25

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
   :lines: 27-29

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
   :lines: 31-33

Kommandozeilenwerkzeuge
-----------------------

Bei Kommandozeilenwerkzeugen wollen wir üblicherweise ein `--verbose`-Flag, das
Log-Ausgaben liefert, die für die Fehlerbehebung nützlich sind.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 35-36
