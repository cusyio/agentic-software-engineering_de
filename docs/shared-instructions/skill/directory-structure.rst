.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Verzeichnisstruktur
===================

Wenn euer Skill wächst, könnt ihr zusätzliche Inhalte in eigenen Dateien
bündeln, die euer Coding-Agent nur bei Bedarf lädt. Wenn ihr :abbr:`z. B. (zum
Beispiel)` in eurer :file:`SKILLS.md`-Datei angebt, dass die Beschreibung für
die E-Mail-Validierung in :file:`./email-validation.md` liegt, dann wird diese
auch nur in diesem Kontext berücksichtigt.

Das vollständige Verzeichnis könnte dann so aussehen:

.. code-block:: console

   rest-api/
   ├── SKILL.md               # Main instructions, loaded when triggered
   ├── email-validation.md    # Only loaded for Email validation
   ├── reference.md           # API reference (loaded as needed)
   └── scripts/
       ├── utility.py         # Utility script (executed, not loaded)
       └── validate_email.py  # Validation script

.. warning::
   Tief verschachtelte Referenzen sollten vermieden werden, da sie :abbr:`ggf.
   (gegebenenfalls)` bei Bedarf nicht oder nicht vollständig eingelesen werden.

Referenzdateien
    Bei Referenzdateien, die länger als 100 Zeilen sind, fügt oben ein
    Inhaltsverzeichnis ein. Dadurch wird sichergestellt, dass euer
    Coding-Agent den gesamten Umfang der verfügbaren Informationen erkennen
    kann, :abbr:`z. B. (zum Beispiel)`:

    .. code-block:: md

       # API Reference

       ## Contents
       - Setup
       - Authentication
       - Core methods (create, read, update, delete)
       - Error handling

       ## Setup
       …

       ## Authentication
       …

:file:`scripts`
    Behandelt beim Schreiben von Skripten für Skills deren Fehlerzustände
    selbst und leitet sie nicht an euren Coding-Agenten weiter.
