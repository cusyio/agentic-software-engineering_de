.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Glossar
=======

.. glossary::
   :sorted:

   Agent2Agent
   A2A
       `Agent2Agent <https://a2a-protocol.org/latest/>`_ ist ein offenes
       Protokoll, das die Kommunikation und Interoperabilität zwischen
       agentenbasierten Anwendungen ermöglicht.

   Agent Scan
   Snyk Agent Scan
       `Snyk Agent Scan <https://github.com/snyk/agent-scan>`_ ist ein
       Sicherheitsscanner für Agent-Ökosysteme, der lokale Komponenten –
       darunter :term:`MCP`-Server und :doc:`Skills
       <shared-instructions/skill/index>` – aufspürt und Risiken wie
       Prompt-Injection, `Tool-Poisoning
       <https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks>`__,
       toxische Abläufe, fest codierte Geheimnisse und unsicheren Umgang mit
       Anmeldedaten aufzeigt. Er schließt eine sich abzeichnende Lücke in der
       Transparenz der Agent-Lieferkette und bietet eine praktische Möglichkeit,
       schnell wachsende Agentensysteme zu erfassen und zu testen.
       Sicherheitsscanner für :term:`Model Context Protocol`

   FastMCP
       Python-Framework, das die Einrichtung, das Protokoll-Handling und das
       Fehlermanagement eines :term:`MCP`-Servers vereinfacht, indem es die
       Komplexität des Protokolls abstrahiert und Entwicklungsteams ermöglicht,
       MCP-Ressourcen und -Tools über intuitive
       Python-:doc:`python-basics:functions/decorators` zu definieren. Diese
       Abstraktion ermöglicht es Teams, sich auf die Geschäftslogik zu
       konzentrieren, was zu übersichtlicheren und besser wartbaren
       MCP-Implementierungen führt.

       Während FastMCP 1.0 bereits in das offizielle `MCP Python SDK
       <https://github.com/modelcontextprotocol/python-sdk>`_ integriert ist,
       entwickelt sich der MCP-Standard weiterhin rasant weiter. Daher solltet
       ihr die Veröffentlichung von Version 2.0 im Auge zu behalten und
       sicherstellen, dass ihr mit den Änderungen an der offiziellen
       Spezifikation Schritt haltet.

       .. seealso::
          * `README.v2.md
            <https://github.com/modelcontextprotocol/python-sdk/blob/main/README.v2.md>`_
          * `Migration Guide: v1 to v2
            <https://github.com/modelcontextprotocol/python-sdk/blob/main/docs/migration.md>`_

   Model Context Protocol
   MCP
       offener Standard, der festlegt, wie LLM-Anwendungen und -Agenten mit
       externen Datenquellen und Tools integriert werden, wodurch die Qualität
       der von KI generierten Ergebnisse erheblich verbessert werden soll. MCP
       konzentriert sich auf den Kontext und den Zugriff auf Tools und
       unterscheidet sich damit vom :term:`Agent2Agent` (:term:`A2A`)-Protokoll,
       das die Kommunikation zwischen Agenten regelt. Es spezifiziert Server
       (für Daten und Tools wie Datenbanken, Wikis und Dienste) sowie Clients
       (Agenten, Anwendungen und Coding-Assistenten). Frameworks wie
       :term:`FastMCP` sind ebenso entstanden wie die `MCP Registry
       <https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/>`_
       für die Erkennung öffentlicher und proprietärer Tools. Das Protokoll
       weist jedoch auch architektonische Lücken auf und rief `Kritik
       <https://julsimon.medium.com/why-mcps-disregard-for-40-years-of-rpc-best-practices-will-burn-enterprises-8ef85ce5bc9b>`_
       hervor, da etablierte RPC-Best-Practices außer Acht gelassen wurden. Bei
       Produktionsanwendungen sollten Entwicklungsteams genaue
       Sicherheitsüberprüfungen vornehmen, indem sie :term:`Toxic Flows` mit
       Tools wie :term:`Agent Scan` eindämmen und das Autorisierungsmodul zur
       Laufzeit detailliert überwachen.

       .. seealso::
          * `What is the Model Context Protocol (MCP)?
            <https://modelcontextprotocol.io/docs/getting-started/intro>`_

   Toxic Flows
       Mit dem Aufkommen von Agenten, die zahlreiche Berechtigungen benötigen,
       wie beispielsweise `OpenClaw <https://openclaw.ai>`_, setzen
       Entwciklungsteams Agenten zunehmend in Umgebungen ein, in denen diese
       einer tödlichen Dreifachgefahr (`lethal trifecta
       <https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_)
       ausgesetzt sind:

       #. Zugriff auf private Daten
       #. Kontakt mit nicht vertrauenswürdigen Inhalten und
       #. die Möglichkeit zur externen Kommunikation.

       Mit wachsenden Fähigkeiten vergrößert sich auch die Angriffsfläche,
       wodurch Systeme Risiken wie Prompt-Injection und `Tool-Poisoning
       <https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks>`__
       ausgesetzt sind.

       Die Analyse von Toxic Flows ist weiterhin eine der wichtigsten Techniken
       zur Untersuchung agentenbasierter Systeme, um unsichere Datenpfade und
       potenzielle Angriffsvektoren zu identifizieren. Diese Risiken beschränken
       sich nicht mehr nur auf :term:`MCP`-Integrationen; wir haben ähnliche
       Muster auch bei :doc:`Skills <shared-instructions/skill/index>`
       beobachtet, wo ein böswilliger Akteur eine scheinbar nützliche Funktion
       so verpacken kann, dass sie eine versteckte Anweisung zum Abzug
       sensibler Daten enthält. Wir empfehlen Entwicklungstteams, die mit
       Agenten arbeiten, dringend, eine Toxic-Flow-Analyse durchzuführen und
       Tools wie :term:`Agent Scan` zu nutzen, um unsichere Datenpfade zu
       identifizieren, bevor sie ausgenutzt werden.


   .. _start-context-strategies:

   Prompt-Caching
       stellt statische Anweisungen vorab bereit, was Kosten senkt und die Zeit
       bis zum ersten Token verkürzt.

       .. seealso::
          `Prompt caching
          <https://platform.claude.com/docs/de/build-with-claude/prompt-caching>`_

   Dynamic retrieval
       geht über grundlegende :abbr:`RAG (Retrieval-Augmented Generation)`
       hinaus, indem es Tools auswählt und nur die notwendigen
       :term:`MCP`-Server lädt, wodurch eine unnötige Kontexterweiterung
       vermieden wird.

   Context Graphs
       modellieren institutionelles Schlussfolgern – wie Richtlinien, Ausnahmen
       und Präzedenzfälle – als strukturierte, abfragbare Daten. Techniken zum
       Kontextmanagement nutzen *Stateful Compression*, und Sub-Agenten, um
       Zwischenschritte in lang andauernden Workflows zusammenzufassen.

       .. seealso::
          `Context Graphs
          <https://trustgraph.ai/guides/key-concepts/context-graphs/>`_

   .. _end-context-strategies:

   .. _start-sandboxes:

   Sprites
       `Sprites <https://sprites.dev/>`_ ist eine zustandsbehaftete
       Sandbox-Umgebung von `Fly.io <https://fly.io/>`_, die auf Basis von
       :ref:`firecracker-microvm`-microVMs für die isolierte Ausführung von
       Coding-Agenten entwickelt wurde.

       Während die meisten Sandboxen kurzlebig sind – sie werden für eine
       Aufgabe gestartet und verschwinden anschließend wieder –, bietet
       *Sprites* dauerhafte Linux-Umgebungen mit unbegrenzten Checkpoint- und
       Wiederherstellungsfunktionen. Dies ermöglicht es Entwicklungsteams, einen
       Snapshot des gesamten Umgebungszustands zu erstellen – einschließlich
       installierter Abhängigkeiten, Laufzeitkonfiguration und Änderungen am
       Dateisystem – und einen Rollback durchzuführen, wenn ein Agent aus der
       Bahn gerät. Dies geht über das hinaus, was :doc:`Git
       <Python4DataScience:productive/git/index>` allein wiederherstellen kann,
       da es den Systemzustand erfasst, den die Versionskontrolle nicht
       verfolgt.

   Development Containers
       `Development Containers <https://containers.dev>`_ bieten eine
       standardisierte Methode zur Definition reproduzierbarer,
       containerisierter Entwicklungsumgebungen mithilfe der
       :file:`devcontainer.json`-Konfigurationsdatei.

       Ursprünglich entwickelt, um Teams einheitliche Entwicklungsumgebungen zu
       bieten, haben Dev-Containers einen überzeugenden neuen Anwendungsfall als
       isolierte Ausführungsumgebungen für Coding-Agenten gefunden. Durch die
       Ausführung eines Agenten in einem Dev-Container wird dieser vom
       Dateisystem, den Anmeldedaten und dem Netzwerk des Hosts isoliert, sodass
       Teams den Agenten weitreichende Berechtigungen erteilen können, ohne die
       Host-Maschine zu gefährden.

       Die `offene Spezifikation <https://containers.dev/implementors/spec/>`_
       wird nativ von `VS Code
       <https://containers.dev/supporting#visual-studio-code>`_ und
       VS Code-basierten Tools wie Cursor unterstützt.

   DevPod
       `DevPod <https://devpod.sh>`_ erweitert die Dev-Container-Unterstützung
       über :abbr:`SSH (Secure Shell)` auf jeden Editor- oder Terminal-Workflow.
       Dev-Containers verfolgen einen *ephemeral-by-default*-Ansatz,
       :abbr:`d. h. (das heißt)`, der Container wird bei jedem Start neu aus der
       Konfiguration erstellt, der eine saubere Sicherheitsgrenze bietet,
       allerdings auf Kosten der Neuinstallation von Tools und Abhängigkeiten.

   .. _end-sandboxes:
