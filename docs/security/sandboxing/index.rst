.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sandboxing
==========

Code-Sandboxing hat sich von einem Nischen-Sicherheitstool zu einer
unverzichtbaren Infrastruktur für moderne Anwendungen entwickelt. Zwei
wesentliche Trends haben Sandboxing für die Produktentwicklung unverzichtbar
gemacht:

* KI- und LLM-Anwendungen

  :abbr:`LLMs (Large Language Models)` generieren Code, der sicher ausgeführt
  werden muss. KI-Agenten, Datenanalyse-Tools und UI-Frameworks müssen nicht
  vertrauenswürdigen, dynamisch generierten Code ausführen. Und da
  Coding-Agenten zunehmend autonom Code ausführen, Builds durchführen und mit
  dem Dateisystem interagieren können, birgt der uneingeschränkte Zugriff auf
  eine Entwicklungsumgebung reale Risiken, die bis hin zur Offenlegung von
  Anmeldedaten reichen. Einige Coding-Agenten erlauben zwar, Berechtigungen
  festzulegen, :abbr:`z. B. (zum Beispiel)` automatisch, mit einer Whitelist.
  Diese Berechtigungen bleiben jedoch anfällig für `Lethal Trifecta
  <https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_, wenn euer
  Coding-Agent Zugriff auf private Daten hat, nicht vertrauenswürdigen Inhalten
  ausgesetzt ist und extern kommunizieren kann. In diesen Fällen sollte eine
  eigene Sandbox definiert werden, in der der agentische Code in isolierten
  Umgebungen mit eingeschränktem Dateisystemzugriff, kontrollierter
  Netzwerkkonnektivität und begrenzter Ressourcennutzung ausgeführt werden kann.

* User-Programmable Platforms

  Viele :abbr:`SaaS (Software as a Service)`-Anwendungen, Datentools und
  Entwicklungsplattformen ermöglichen mittlerweile, eigenen Code über Plugins,
  benutzerdefinierte Skripte oder Datentransformationen einzureichen. Dies
  erfordert eine sichere Isolierung, um Sicherheitslücken zu verhindern. Der
  gleiche Bedarf besteht bei :abbr:`CDEs (Cloud Development Environments)` und
  Online-:abbr:`IDEs (Integrated Development Environments)` wie `GitHub
  Codespaces <https://github.com/features/codespaces>`_, `Gitpod
  <https://gitpod.io/>`_ und `Coder <https://coder.com/>`_, die die Umgebung
  jedes User von der Host-Infrastruktur und anderen Users isolieren müssen.

Sandboxing sollte daher die übliche Vorgehensweise sein und nicht mehr nur eine
optionale Erweiterung. Mittlerweile gibt es auch ein breites Spektrum von
Sandboxing-Optionen. Über die integrierten Sandbox-Modi der Coding-Agenten
hinaus gibt es verschiedene Optionen im Spannungsfeld zwischen kurzlebigen und
dauerhaften Lösungen. Über die grundlegende Isolierung hinaus sollten
Entwicklungsteams die praktischen Anforderungen an eine produktive Sandbox
berücksichtigen. Dazu gehören alle für die Entwicklung und das Testen
erforderlichen Komponenten sowie eine sichere und unkomplizierte
Authentifizierung bei externen Diensten. Entwicklungsteams benötigen
Port-Weiterleitung sowie ausreichende CPU- und Speicherressourcen für die
Arbeitslasten der Coding-Agenten. Ob die Sandbox standardmäßig kurzlebig oder
zur Wiederherstellung von Sitzungen dauerhaft sein soll, ist eine
Design-Entscheidung, die von den Prioritäten des Teams in Bezug auf Sicherheit,
Kosten und Kontinuität der Arbeitsabläufen abhängt.

Darüberhinaus hat sich der Fokus von Sandboxing von einem reinen Sicherheitstool
zu einer Plattformfunktion gewandelt, die sicher leistungsstarke Funktionen
bereitstellen soll. So nutzt beispielsweise `Hugging Face das Sandboxing von e2b
<https://e2b.dev/blog/how-hugging-face-is-using-e2b-to-replicate-deepseek-r1>`_
für Pipelines im Bereich des verstärkenden Lernens, und `Groq setzt e2b
<https://e2b.dev/blog/groqs-compound-ai-models-are-powered-by-e2b>`_ für
*Compound AI*-Systeme ein, die LLMs mit der Ausführung von Live-Code
kombinieren. Das bedeutet, dass Sandboxen nicht nur sicher, sondern auch
schnell, zuverlässig und benutzerfreundlich sein müssen. Moderne Lösungen werden
anhand ihrer :abbr:`SDKs (Software Development Kits)`, ihrer
Ausführungsgeschwindigkeit und ihrer Integrierbarkeit bewertet.

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   technologies
   platforms
