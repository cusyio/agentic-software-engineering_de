.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sicherheit
==========

.. seealso::
   :doc:`Skill-Sicherheit <shared-instructions/skill/security>`

Einige Coding-Agenten erlauben, die Berechtigungen dort festzulegen,
:abbr:`z. B. (zum Beispiel)` automatisch, mit einer Whitelist oder in einer
Sandbox. Diese Berechtigungen bleiben jedoch anfällig für `Lethal Trifecta
<https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_, als wenn euer
Coding-Agent Zugriff auf private Daten hat, nicht vertrauenswürdigen Inhalten
ausgesetzt ist und extern kommunizieren kann.

In diesen Fällen definieren wir unsere eigene Sandbox, sodass der Code der
Agenten in isolierten Umgebungen mit eingeschränktem Dateisystemzugriff,
kontrollierter Netzwerkkonnektivität und begrenzter Ressourcennutzung ausgeführt
wird.

Da Coding-Agenten zunehmend autonom Code ausführen, Builds durchführen und mit
dem Dateisystem interagieren können, birgt der uneingeschränkte Zugriff auf eine
Entwicklungsumgebung reale Risiken, die bis hin zur Offenlegung von Anmeldedaten
reichen. Sandboxing sollte daher die übliche Vorgehensweise sein und nicht nur
eine optionale Erweiterung.

Mittlerweile gibt es ein breites Spektrum von Sandboxing-Optionen. Über die
integrierten Sandbox-Modi der Coding-Agenten hinaus gibt es verschiedene
Optionen im Spannungsfeld zwischen kurzlebigen und dauerhaften Lösungen:

`Sprites <https://sprites.dev/>`_
    ist eine zustandsbehaftete Sandbox-Umgebung von `Fly.io <https://fly.io/>`_,
    die auf Basis von `Firecracker
    <https://firecracker-microvm.github.io>`_-microVMs für die isolierte
    Ausführung von Coding-Agenten entwickelt wurde.

    Während die meisten Sandboxen kurzlebig sind – sie werden für eine Aufgabe
    gestartet und verschwinden anschließend wieder –, bietet *Sprites*
    dauerhafte Linux-Umgebungen mit unbegrenzten Checkpoint- und
    Wiederherstellungsfunktionen. Dies ermöglicht es Entwicklungsteams, einen
    Snapshot des gesamten Umgebungszustands zu erstellen – einschließlich
    installierter Abhängigkeiten, Laufzeitkonfiguration und Änderungen am
    Dateisystem – und einen Rollback durchzuführen, wenn ein Agent aus der Bahn
    gerät. Dies geht über das hinaus, was :doc:`Git
    <Python4DataScience:productive/git/index>` allein wiederherstellen kann, da
    es den Systemzustand erfasst, den die Versionskontrolle nicht verfolgt.

`Development Containers <https://containers.dev>`_
    bieten eine standardisierte Methode zur Definition reproduzierbarer,
    containerisierter Entwicklungsumgebungen mithilfe der
    :file:`devcontainer.json`-Konfigurationsdatei.

    Ursprünglich entwickelt, um Teams einheitliche Entwicklungsumgebungen zu
    bieten, haben Dev-Containers einen überzeugenden neuen Anwendungsfall als
    isolierte Ausführungsumgebungen für Coding-Agenten gefunden. Durch die
    Ausführung eines Agenten in einem Dev-Container wird dieser vom Dateisystem,
    den Anmeldedaten und dem Netzwerk des Hosts isoliert, sodass Teams den
    Agenten weitreichende Berechtigungen erteilen können, ohne die Host-Maschine
    zu gefährden.

    Die `offene Spezifikation <https://containers.dev/implementors/spec/>`_ wird
    nativ von `VS Code <https://containers.dev/supporting#visual-studio-code>`_
    und VS Code-basierten Tools wie Cursor unterstützt.

`DevPod <https://devpod.sh>`_
    erweitert die Dev-Container-Unterstützung über :abbr:`SSH (Secure Shell)`
    auf jeden Editor- oder Terminal-Workflow. Dev-Containers verfolgen einen
    *ephemeral-by-default*-Ansatz, :abbr:`d. h. (das heißt)`, der Container wird
    bei jedem Start neu aus der Konfiguration erstellt, der eine saubere
    Sicherheitsgrenze bietet, allerdings auf Kosten der Neuinstallation von
    Tools und Abhängigkeiten.

Über die grundlegende Isolierung hinaus sollten Entwicklungsteams die
praktischen Anforderungen an eine produktive Sandbox berücksichtigen. Dazu
gehören alle für die Entwicklung und das Testen erforderlichen Komponenten sowie
eine sichere und unkomplizierte Authentifizierung bei externen Diensten.
Entwicklungsteams benötigen Portweiterleitung sowie ausreichende CPU- und
Speicherressourcen für die Arbeitslasten der Coding-Agenten. Ob die Sandbox
standardmäßig kurzlebig oder zur Wiederherstellung von Sitzungen dauerhaft sein
soll, ist eine Designentscheidung, die von den Prioritäten des Teams in Bezug
auf Sicherheit, Kosten und Kontinuität der Arbeitsabläufen abhängt.

.. seealso::
   * Bundesamt für Sicherheit in der Informationstechnik (BSI): `Evasion-Attacks
     auf LLMs – Eine Checkliste zur Härtung des LLM-Systems
     <https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/KI/Evasion-Angriffe_auf_LLMs-Checkliste.pdf>`_
