.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Hooks
=====

Hooks sind benutzerdefinierte Handler – ein Skript, ein HTTP-Endpunkt, ein
:term:`MCP`-Tool oder ein kurzer LLM-Prompt –, der an einem bestimmten Punkt im
Lebenszyklus eines Coding-Agenten ausgelöst wird. Hooks erhalten strukturierte
Daten darüber, was als Nächstes geschehen wird, und können den Vorgang
beobachten, protokollieren, ändern oder blockieren, bevor die Ausführung
fortgesetzt wird. `Claude Code <https://code.claude.com/docs/de/hooks>`_,
`Cursor <https://cursor.com/docs/hooks>`_ und `Codex
<https://developers.openai.com/codex/hooks>`_ stellen solche Hooks zur
Verfügung.

Vor einem Jahr haben wir noch empfohlen, den Datenverkehr über ein LLM-Gateway
wie `OpenRouter <https://openrouter.ai/>`_, `Portkey <https://portkey.ai/>`_
oder `LiteLLM <https://www.litellm.ai/>`_ von außen zu analysieren.

.. seealso::
   * :doc:`cusyio:blog/ai-programming-tools`

Mit dem Aufkommen von :term:`MCP`-Datenverkehr, :doc:`Slash-Befehlen
<slash-commands>` sowie dem Lesen und Bearbeiten von lokalen Dateien, die alle
in der Ausführungsumgebung des Coding-Agenten ablaufen, hatten die Gateways
jedoch keinen Einblick mehr und verloren an Bedeutung. Hooks hingegen haben
Zugriff auf den vollständigen Kontext einer Sitzung und sind daher gut geeignet,
um Entwicklungsrichtlinien für die Coding-Agenten einzuhalten.

Das folgende Diagramm veranschaulicht die Ausführung eines einzelnen
``PreToolUse``-Hooks, wenn Claude Code einen Bash-Befehl ausführt. Das Ereignis
wird bedingungslos ausgelöst, anschließend wird die Auswahl durch den
benutzerdefinierten Matcher und die ``if``-Filter begrenzt. Wenn beide
übereinstimmen, wird der Hook-Befehl ausgeführt und bestimmt das Ergebnis. Ist
dies bei einem der beiden nicht der Fall, wird der Hook übersprungen und das
Tool fährt fort:

.. mermaid::

   flowchart LR
       A[Coding agent runs<br><br><pre>bash rm -rf /tmp/build</pre>] --> B[PreToolUse fires<br><pre>bash rm -rf /tmp/build</pre><br><pre>tool_name: 'Bash'</pre>]
       B --> C{Your watcher<br><br>'Bash' match?}
       C -->|no| D[Hook not matched<br><br>Tool proceeds]
       C -->|yes| E{Your if<br><br>'bash rm *' match?}
      D --> |Tool call allowed| F[Coding agent continues]
       E --> |no| D
       E --> |yes| G[Your hook command<br><br><pre>block-rm.sh</pre>]
       G --> H[Blocked<br><br><pre>permissionDecision: 'deny'</pre>]
       H -->|Tool call blocked| F
       %%{init:{'themeCSS':'#flowchart-G-13 rect, #flowchart-H-15 rect, #L_G_H_0, #L_H_F_0 { stroke: red;}; #flowchart-D-5 rect, #L_D_F_0 { stroke: green;};'}}%%

Hooks werden innerhalb der Agent-Loop ausgeführt
    Sie befinden sich zwischen der Entscheidung des Coding-Agenten, eine Aktion
    auszuführen, und der tatsächlichen Ausführung. Der ``PreToolUse``-Hook sieht
    die Befehlszeichenfolge, bevor sie ausgeführt wird, genauso wie er die
    Argumente eines :term:`MCP`-Toolaufrufs sieht, bevor der Aufruf erfolgt. Es
    gibt keinen Datenverkehr, der gespiegelt werden muss, keinen Proxy und kein
    Zertifikat, dem vertraut werden muss.
Hooks erfassen den vollständigen Kontext
    Der strukturierte Payload umfasst die Sitzungs-ID, das Arbeitsverzeichnis,
    das Modell, den Tool-Namen, die Tool-Eingabe und oft auch den vollständigen
    Pfad zum Transkript.
Hooks können Daten an jeden beliebigen Ort weiterleiten
    Die meisten Coding-Agenten unterstützen mehrere Handler-Typen. Ein Hook kann
    ein Skript ausführen, eine ``POST``-Anfrage an einen HTTPS-Endpunkt senden,
    ein Tool auf einem verbundenen :term:`MCP`-Server aufrufen oder einen
    kleinen LLM-Prompt inline auswerten.
Hooks lassen sich kombinieren
    Für dasselbe Ereignis können mehrere Hooks registriert werden. Sie werden
    parallel ausgeführt ohne dass sie voneinander wissen, und die Ergebnisse
    werden dann zusammengefasst.
Hooks sind *Fail-Open*
    Fehler in den Hooks werden als nicht schwerwiegend betrachtet. Wenn das
    Skript einen Fehler ausgibt, es zu Netzwerkstörungen oder Zeitüberschreitung
    kommt, fährt der Coding-Agent fort.

Welche Probleme lösen Hooks?
----------------------------

Verhindern unerwünschter Vorgänge
    Was unerwünschte Vorgänge sind, hängt stark vom jeweiligen Projekt ab. Zu
    den häufigsten Fällen gehören:

    * das Blockieren schädlicher Shell-Befehle, bevor sie ausgeführt werden
    * das Verweigern von Tool-Aufrufen, die in eine Produktionsdatenbank
      schreiben würden
    * das Entfernen von vertraulichen Informationen aus Prompts, bevor diese das
      Modell erreichen
    * das Verhindern, dass ein Coding-Agent hart codierte Anmeldedaten
      festschreibt.

    Das Muster ist bei all diesen Fällen dasselbe: Ein ``PreToolUse``- oder
    ``UserPromptSubmit``-Hook bewertet die Eingabe anhand eines Regelsatzes und
    gibt eine Ablehnungsentscheidung zurück, wenn eine Übereinstimmung vorliegt. Die Aktion wird nie ausgeführt.

    .. warning::
       Entgegen der häufigen Behauptung sind Hooks jedoch **nicht**
       deterministisch. Sie eignen sich daher nicht wirklich für die
       verpflichtende Durchsetzung von Richtlinien. Unter anderem *Fail-Open*
       erlaubt Modellen, blockierende Hooks zu umgehen, ohne dass dies bekannt
       wird.

Protokolle und Audits
    Ihr könnt mit Hooks jeden Prompt, jeden Tool-Aufruf und jede Antwort in
    einem zentralen Log-Service erfassen. Die Daten, die zuvor bestenfalls auf
    einzelnen Arbeitsplatzrechnern isoliert waren, können mit einem Hook
    zusammengefasst werden. So kann dann :abbr:`z. B. (zum Beispiel)` die
    Token-Nutzung nach Team, User oder Workflow aufschlüsseln, um ein klares
    Bild von der internen Nutzung des Coding-Agenten zu erhalten.
Codequalität und -sicherheit
    Hooks lassen sich bei Dateiänderungen mit ``afterFileEdit`` auslösen,
    wodurch sie sich ideal eignen, um Tools zur Überprüfung der Codequalität und
    -sicherheit auszuführen. Der Coding-Agent erhält damit ein sofortiges,
    strukturiertes Feedback und kann den fehlerhaften Code noch im selben
    Durchlauf neu generieren.
Workflow-Automatisierung
    Hooks können auch weitere Aufgaben übernehmen: so kann ich :abbr:`z. B. (zum
    Beispiel)` bei jeder Dateiänderung einen Formatter wie
    :doc:`Python4DataScience:productive/qa/ruff` ausführen oder bei
    ``SessionStart`` den :doc:`Git
    <Python4DataScience:productive/git/index>`-Status einfügen. Allgemein
    ermöglichen Hooks, das Projekt um kleine Workflow-Skripte zu erweitern, die
    jedes Team irgendwann einmal benötigt.

Hooks vs. Skills
----------------

Der entscheidende Unterschied ist, wer entscheidet, ob gehandelt wird. Hooks
werden durch Lifecycle-Events ausgelöst, Skills durch eigene Schlussfolgerungen
des Coding-Agenten.

Wie implementiert ihr einen Hook?
---------------------------------

Das Konfigurationsmodell ist bei den verschiedenen Coding-Agenten weitgehend
identisch. Eine
:doc:`Python4DataScience:data-processing/serialisation-formats/json/index`-Datei
befindet sich am jeweiligen Speicherort. Die Datei listet Ereignisse, optionale
Matcher und den auszuführenden Handler auf.

Im Folgenden wird derselbe Hook zum Blockieren von ``rm -rf`` vor dessen
Ausführung für jeden der drei oben genannten Coding-Agenten implementiert. Die
Form der Entscheidungsantwort und die Feldnamen unterscheiden sich, aber das
Muster ist in allen drei Fällen dasselbe:

.. tab:: Claude Code

   .. code-block:: javascript
      :caption: .claude/settings.json

      {
      "hooks": {
        "PreToolUse": [
          {
            "matcher": "Bash",
            "hooks": [
              {
                "type": "command",
                "if": "Bash(rm -rf *)",
                "command": "$PROJECT_DIR/.claude/hooks/block-rm.sh"
              }
            ]
          }
        ]
      }
      }

.. tab:: Cursor

   .. code-block:: javascript
      :caption: .cursor/hooks.json

      {
      "version": 2026-1,
      "hooks": {
        "beforeShellExecution": [
          {
            "command": "$PROJECT_DIR/.cursor/hooks/block-rm.sh"
          }
        ]
      }
      }

.. tab:: Codex

   .. code-block:: javascript
      :caption: .codex/hooks.json

      {
      "hooks": {
        "PreToolUse": [
          {
            "matcher": "Bash",
            "hooks": [
              {
                "type": "command",
                "command": "$PROJECT_DIR/.codex/block-rm.sh",
                "timeout": 10
              }
            ]
          }
        ]
      }
      }

.. tip::
   Hooks werden vom Projektverzeichnis aus ausgeführt, ihre Ausführungsumgebung
   kann jedoch variieren. Verwendet für Dateien, auf die in Hook-Befehlen
   verwiesen wird, daher stets absolute Pfade.

.. tip::
   Vergesst nicht, die Skripte ausführbar zu machen:

   .. code-block:: console

      $ chmod +x ~/.claude/hooks/block-rm.sh

.. tip::
   Startet nach Änderungen an der Hook-Datei immer eine neue Sitzung, da die
   Änderungen nicht für bereits laufende Sitzungen gelten.

Leider liefert jeder Anbieter sein eigenes Konfigurationsformat, sein eigenes
Event-Vokabular, sein eigenes Eingabeschema und seine eigene Form für
Entscheidungsantworten aus. Eine für Claude Code geschriebene Regel zum
Erkennen vertraulicher Daten ist nicht dasselbe Skript, das für Cursor oder
Codex funktioniert. Ein Projekt, das sich auf einen Satz von Hooks einigt,
müsste letztendlich für jeden Coding-Agenten eigene Implementierungen pflegen
und manuell miteinander abstimmen.

Typische Hooks
--------------

Von den Dutzenden von Ereignissen, die die großen Coding-Agenten bereitstellen,
decken die folgenden fünf den Großteil dessen ab, was wir tatsächlich benötigen:

``SessionStart``, ``sessionStart``
    wird ausgelöst, **bevor** der Coding-Agent irgendwelche weiteren Aktionen
    ausführt. Dadurch wird die Ausgabe des Hooks genauso hoch gewichtet wie eine
    Eingabe, nicht nur als Datei. Zudem ist der Hook deterministisch und wird
    zuverlässig ausgeführt. Schließlich kann er auch noch dynamisch sein und
    :doc:`Git <Python4DataScience:productive/git/index>`-Status,
    Umgebungsvariablen, API-Aufrufe oder den Laufzeitstatus einbeziehen.

    .. tip::
       Ihr solltet einen solchen Hook jedoch nicht als umfassenden
       Dokumentationsauszug behandeln. Wenn euer Hook 3.000 Token Kontext
       ausgibt, habt ihr das :ref:`Agents.md
       <overloaded-agent-instructions>`-Problem mit anderen Mitteln
       nachgebildet.

``UserPromptSubmit``, ``beforeSubmitPrompt``
    wird ausgelöst, wenn ein User eine Eingabe an den Agenten übermittelt. Dies
    ist die Schranke für eingehende Daten. Ein Hook an dieser Stelle kann die
    Eingabe nach vertraulichen Informationen durchsuchen, die :abbr:`z. B. (zum
    Beispiel)` aus einer :file:`.env`-Datei eingefügt wurden, personenbezogene
    Daten anonymisieren, bevor sie das Modell erreichen, oder Eingaben
    blockieren, die einem ``deny``-Muster entsprechen.
``PreToolUse``, ``preToolUse``
    wird ausgelöst, bevor ein vom Agenten aufgerufenes Tool ausgeführt wird.
    Dies ist die Schranke für ausgehende Aktionen. Ein Hook an dieser Stelle
    erhält den Namen des Tools (``Bash``, ``Write``, ``Edit``,
    ``mcp__github__create_pr``), die Argumente und das Arbeitsverzeichnis. Fast
    alle Anwendungsfälle zum Verhindern unerwünschter Vorgänge in Echtzeit
    finden hier statt.
``PostToolUse``, ``postToolUse``
    wird ausgelöst, nachdem ein Tool zurückkehrt. Hier werden die Ausgaben des
    Tools überprüft. Der häufigste Anwendungsfall ist die Erkennung von
    Datenexfiltration. Ein ``PostToolUse``-Hook für ``Bash`` und ``Read`` sieht
    den Inhalt und kann entscheiden, ob der Agent diesen im weiteren Verlauf der
    Konversation verwenden darf.
``SessionEnd``, ``sessionEnd``
    wird ausgelöst, wenn der Agent einen Schritt oder eine Sitzung beendet.
    Dieser Hook erlaubt, ein umfassendes Observability-Konzept zu erstellen.
    Ein Handler erfasst hier das vollständige Transkript und leitet es an einen
    zentralen Log-Service weiter. Sobald sich das Transkript in einem
    abfragbaren System befindet, werden Fragen möglich wie *„Bei welcher Sitzung
    wurde auf diese Daten zugegriffen?“* oder *„Welche Eingabeaufforderungen hat
    dazu geführt, dass der Coding-Agent von dieser falsche Annahme ausgeht?“*
    Echtzeit-Blockierung und rückwirkende Analyse basieren auf denselben Daten,
    die nur unterschiedlich genutzt werden.

Auch die anderen Hooks sind für bestimmte Automatisierungen nützlich, die fünf
oben genannten Ereignisse bilden bei uns jedoch jedoch meist die Grundlage für
unsere Entwicklungsrichtlinien. Wenn ihr diese Ereignisse konfiguriert und die
Daten an einen zentralen Log-Service weitergeleitet werden, ist der Großteil
dieser Richtlinien bereits abgedeckt.

Hook-Schwerpunkte der verschiedenen Coding-Agenten
--------------------------------------------------

Claude Code
    bietet die umfassendste Auswahl. Es stellt Ereignisse Für den gesamten
    Lebenszyklus bereit, darunter `Setup
    <https://code.claude.com/docs/de/hooks#setup>`_, `WorktreeCreate
    <https://code.claude.com/docs/de/hooks#worktreecreate>`_/`WorktreeRemove
    <https://code.claude.com/docs/de/hooks#worktreeremove>`_, `TaskCreated
    <https://code.claude.com/docs/de/hooks#taskcreated>`_/`TaskCompleted
    <https://code.claude.com/docs/de/hooks#taskcompleted>`_, `TeammateIdle
    <https://code.claude.com/docs/de/hooks#teammateidle>`_ und `Elicitation
    <https://code.claude.com/docs/de/hooks#elicitation>`_. Für Projekte, die
    auf Claude Code setzen, ist das Potenzial zum Verhindern unerwünschter
    Vorgänge sowie für Protokolle und Audits entsprechend höher.
Cursor
    setzt im Wesentlichen auf Introspektion von Agenten-Loops. Es ist der
    einzige Anbieter, der `afterAgentResponse
    <https://cursor.com/docs/hooks#afteragentresponse>`_ und `afterAgentThought
    <https://cursor.com/docs/hooks#afteragentthought>`_ bereitstellt, wodurch
    nicht nur Tool-Aufrufe sondern auch die Zwischenergebnisse des Modells
    sichtbar werden. Cursor stellt mit `beforeReadFile
    <https://cursor.com/docs/hooks#beforereadfile>`_, `afterFileEdit
    <https://cursor.com/docs/hooks#afterfileedit>`_, `beforeTabFileRead
    <https://cursor.com/docs/hooks#beforetabfileread>`_ und `afterTabFileEdit
    <https://cursor.com/docs/hooks#aftertabfileedit>`_ zudem die
    detailliertesten Hooks für Dateioperationen bereit, was die Integration von
    Tools zur Codequalität wie :doc:`Python4DataScience:productive/qa/ruff`
    vereinfacht.
Codex
    fasst viele Aspekte unter den Kategorien `PreToolUse
    <https://developers.openai.com/codex/hooks#pretooluse>`_ und `PostToolUse
    <https://developers.openai.com/codex/hooks#posttooluse>`_ zusammen.
    Shell-Befehle, :term:`MCP`-Aufrufe und Dateioperationen werden alle über die
    generischen Tool-Ereignisse abgewickelt. Das erleichtert zwar die
    Nachvollziehbarkeit, verlagert jedoch die Arbeit auf den Matcher.
