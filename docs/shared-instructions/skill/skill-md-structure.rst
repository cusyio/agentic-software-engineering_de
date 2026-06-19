.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

:file:`SKILL.md`-Struktur
=========================

Jeder Skill benötigt eine :file:`SKILL.md`-Datei mit YAML-Frontmatter:

.. literalinclude:: ../../../.agents/skills/SKILL.md
   :caption: .agents/skills/SKILL-NAME/SKILL.md
   :language: yaml
   :linenos:

``name:``
    * Maximal 64 Zeichen
    * nur Kleinbuchstaben, Zahlen und Bindestriche
    * keine reservierten Wörter wie :abbr:`z. B. (zum Beispiel)` ``claude``

    Gute Beispiele sind ``pdf2txt`` oder ``rest-api``.

``description:``
    * darf nicht leer sein
    * Maximal 1024 Zeichen
    * beschreiben, was der Skill tut und wann er verwendet werden sollte. Ein
      gutes Beispiel ist

      .. code-block:: yaml

         description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.

Einheitliche Terminologie
    Wählt einen Begriff und verwendet ihn durchgehend im Skill, also mischt
    nicht ``API endpoint`` mit ``URL``, ``API route`` und ``path``.
Vermeidet Windows-Pfadangaben
    Verwendet in Dateipfaden stets Schrägstriche (``/``), auch unter Windows.
Vermeidet, zu viele Optionen anzubieten
    Präsentiert nicht mehrere Ansätze, es sei denn, dies ist notwendig.
Muster für die schrittweise Offenlegung (engl. *progressive disclosure*)
    #. Damit der Skill eine optimale Leistung erbringt, sollte die
       :file:`SKILL.md`-Datei weniger als 500 Zeilen lang sein.
    #. Teilt den Inhalt in separate Dateien auf, wenn iher euchdieser Grenze
       nähert.
    #. Verwendet die in :doc:`directory-structure` beschriebenen Muster, um
       Anleitungen, Code und Ressourcen effektiv zu organisieren.

Beispiel
~~~~~~~~

.. literalinclude:: ../../../.agents/skills/rest-api/SKILL.md
   :caption: .agents/skills/rest-api/SKILL.md
   :language: yaml
   :linenos:

``name:``
    * Maximal 64 Zeichen
    * nur Kleinbuchstaben, Zahlen und Bindestriche
    * keine reservierten Wörter wie :abbr:`z. B. (zum Beispiel)` ``claude``

    Gute Beispiele sind ``pdf2txt`` oder ``rest-api``.

``description:``
    * darf nicht leer sein
    * Maximal 1024 Zeichen
    * beschreiben, was der Skill tut und wann er verwendet werden sollte. Ein
      gutes Beispiel ist

      .. code-block:: yaml

         description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.

Einheitliche Terminologie
    Wählt einen Begriff und verwendet ihn durchgehend im Skill, also mischt
    nicht ``API endpoint`` mit ``URL``, ``API route`` und ``path``.
Vermeidet Windows-Pfadangaben
    Verwendet in Dateipfaden stets Schrägstriche (``/``), auch unter Windows.
Vermeidet, zu viele Optionen anzubieten
    Präsentiert nicht mehrere Ansätze, es sei denn, dies ist notwendig.
Muster für die schrittweise Offenlegung (engl. *progressive disclosure*)
    #. Damit der Skill eine optimale Leistung erbringt, sollte die
       :file:`SKILL.md`-Datei weniger als 500 Zeilen lang sein.
    #. Teilt den Inhalt in separate Dateien auf, wenn iher euchdieser Grenze
       nähert.
    #. Verwendet die in :doc:`directory-structure` beschriebenen Muster, um
       Anleitungen, Code und Ressourcen effektiv zu organisieren.

Beispiel
~~~~~~~~

.. literalinclude:: ../../../.agents/skills/rest-api/SKILL.md
   :caption: .agents/skills/rest-api/SKILL.md
   :language: yaml
