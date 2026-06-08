.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Vorgehensweise
==============

Wenn Coding-Agenten direkt mit dem Programmieren beginnen, kann dies zu Code
führen, der das falsche Problem löst. In Claude Code könnt ihr den Planungsmodus
verwenden, um die Erkundung von der Ausführung zu trennen.

.. note::
   Der Planungsmodus ist nützlich, verursacht aber auch zusätzlichen Aufwand.
   Bei Aufgaben, deren Umfang klar ist und die nur eine kleine Änderung
   erfordern, wie :abbr:`z. B. (zum Beispiel)` das Korrigieren eines
   Tippfehlers oder das Umbenennen einer Variablen, bittet euren Coding-Agenten,
   dies direkt zu erledigen.

   Die Planungsphase ist am nützlichsten, wenn ihr euch über den Ansatz unsicher
   seid, wenn die Änderung mehrere Dateien betrifft oder wenn ihr mit dem zu
   ändernden Code nicht vertraut bist.

Der empfohlene Arbeitsablauf umfasst vier Phasen:

#. Erkunden

   In Planungsmodus liest der Coding-Agent Dateien und beantwortet Fragen, ohne
   Änderungen vorzunehmen, :abbr:`z. B. (zum Beispiel)`:

      Lies :file:`src/cusy/tasks` und mache dir ein Bild davon, wie Items
      definiert sind.

      Schau dir auch an, wie Items persistent gespeichert werden.

#. Plan

   Bittet den Coding-Agenten, einen detaillierten Umsetzungsplan zu erstellen,
   :abbr:`z. B. (zum Beispiel)`:

       Wird ein *Owner* angegeben, soll der Status automatisch von ``todo`` zu
       ``assigned`` geändert werden.

#. Implementieren

   Verlasst :abbr:`ggf. (gegebenenfalls)` den Planungsmodus von Claude Code und
   lasst den Coding-Agenten programmieren, wobei er seine Arbeit anhand des
   Plans überprüftk :abbr:`z. B. (zum Beispiel)`:

       Implementiert Tests für die Funktion :func:`assign` gemäß des Plans.
       Führt die Testsuite aus und stellt sicher, dass diese Tests fehlschlagen.
       Schreibt :func:`assign` gemäß des Plans und ruft die Testsuite erneut
       auf. Sofern ein Test fehlschlägt, behebt den Fehler in der Funktion bis
       alle Tests bestanden werden.

#. Commit

   Bittet den Coding-Agenten, einen Commit mit einer aussagekräftigen Meldung
   durchzuführen und einen Pull- oder Merge-Request zu stellen.
