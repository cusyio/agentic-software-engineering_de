.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Agentisches Software-Engineering
================================

Agentische Programmierumgebungen wie Claude Code oder Cursor können im Gegensatz
zu einem Chatbot nicht nur Fragen beantworten, sondern auch eure Dateien lesen,
Befehle ausführen, Änderungen vornehmen und autonom Probleme lösen. Dies
verändert unsere Arbeitsweise: anstatt selbst Code zu schreiben und die
agentische Programmierumgebung zu bitten, ihn zu überprüfen, beschreiben wir
nun, was wir wollen, und der Agent recherchiert, plant und setzt es um.

Mit der zunehmenden Verwendung von Coding-Agenten wächst auch die Zahl der
Studien, die vor einer zu großen Selbstzufriedenheit im Umgang mit
LLM-generiertem Code warnen. Zwar gibt es zahlreiche Belege dafür, dass diese
Tools die Entwicklung beschleunigen können – insbesondere bei der Erstellung von
Prototypen und bei Greenfield-Projekten.  Die Studien zeigen jedoch, dass die
:doc:`Code-Qualität <Python4DataScience:productive/qa/index>` im Laufe der Zeit
nachlassen kann.

Die Studie `AI Copilot Code Quality
<https://www.gitclear.com/ai_assistant_code_quality_2025_research>`_ von
GitClear aus dem Jahr 2024 ergab, dass Code-Duplikate und Code-Churn stärker als
erwartet zugenommen haben, während die Refactoring-Aktivitäten in den
Commit-Verläufen zurückgegangen sind. Ein ähnlicher Trend zeigt sich in der
Microsoft-Studie `The Impact of Generative AI on Critical Thinking
<https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/>`_:
KI-gestütztes Selbstvertrauen geht oft auf Kosten des kritischen Denkens.

Die Beschleunigung eines Teils des Workflows erhöht den Druck auf die anderen
Teile. Wir stellten fest, dass der effektive Einsatz von LLM-Agenten eine
Fokussierung auf die :doc:`Code-Qualität
<Python4DataScience:productive/qa/index>` erfordert und etablierte Praktiken wie
:doc:`python-basics:test/tdd` und :term:`statische Testverfahren <Statische
Testverfahren>` an Bedeutung gewinnen, vor allem wenn sie direkt in die
Coding-Workflows integriert werden.

Dieses Tutorial behandelt Vorgehensweisen, die sich in unseren Teams und für
Data Scientists, die Coding-Agenten in verschiedensten Codebasen und Umgebungen
verwenden, als wirksam erwiesen haben.

Die meisten unserer Empfehlungen basieren jedoch auf einer Einschränkung: Das
Kontextfenster der Coding-Agenten füllt sich schnell, und die Leistung nimmt ab,
je mehr es sich füllt. Ein Kontextfenster enthält eure gesamte Konversation,
einschließlich jeder Nachricht, jeder Datei, die eingelesen wurde, und jeder
Befehlsausgabe. Eine einzige Debugging-Sitzung oder die Erkundung einer
Codebasis kann Zehntausende von Tokens generieren und verbrauchen.

Dies ist von Bedeutung, da die LLM-Leistung mit zunehmender Füllung des Kontexts
abnimmt. Wenn das Kontextfenster voll wird, fangen Coding-Agenten an, frühere
Anweisungen zu „vergessen“ oder mehr Fehler zu machen. Das Kontextfenster ist
die wichtigste Ressource, die es zu verwalten gilt. Um zu sehen, wie sich eine
Sitzung in der Praxis füllt, verfolgt die Token-Nutzung kontinuierlich.

.. seealso::
   :doc:`context`

Dieses Tutorial ist als Einführung in die agentische Softwareentwicklung
gedacht. Für eine Einführung in Python gibt es das
:doc:`python-basics:index`-Tutorial und für den Python Data Science Stack,
Bibliotheken wie :doc:`Python4DataScience:workspace/ipython/index`,
:doc:`Python4DataScience:workspace/numpy/index`,
:doc:`Python4DataScience:workspace/pandas/index`, und verwandte Tools das
:doc:`Python4DataScience:index`-Tutorial. Darüberhinaus gibt es von
uns noch das `Jupyter Tutorial
<https://jupyter-tutorial.readthedocs.io/de/latest/>`_ und das `PyViz Tutorial
<https://pyviz-tutorial.readthedocs.io/de/latest/index.html>`_ sowie im `cusy
Design System <https://www.cusy.design/de/latest/index.html>`_ eine Anleitung
zur `Datenvisualisierung <https://www.cusy.design/de/latest/viz/index.html>`_.

Alle Tutorials dienen als Seminarunterlagen für unsere aufeinander abgestimmten
Trainings:

+---------------+--------------------------------------------------------------+
| Dauer         | Titel                                                        |
+===============+==============================================================+
| 3 Tage        | `Einführung in Python`_                                      |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Fortgeschrittenes Python`_                                  |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Entwurfsmuster in Python`_                                  |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Effizient Testen mit Python`_                               |
+---------------+--------------------------------------------------------------+
| 1 Tag         | `Software-Dokumentation mit Sphinx`_                         |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Technisches Schreiben`_                                     |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Jupyter-Notebooks für effiziente Data-Science-Workflows`_   |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Numerische Berechnungen mit NumPy`_                         |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Daten analysieren mit pandas`_                              |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Daten lesen, schreiben und bereitstellen mit Python`_       |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Daten bereinigen und validieren mit Python`_                |
+---------------+--------------------------------------------------------------+
| 5 Tage        | `Daten visualisieren mit Python`_                            |
+---------------+--------------------------------------------------------------+
| 1 Tag         | `Datenvisualisierungen gestalten`_                           |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Dashboards erstellen`_                                      |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Code und Daten versioniert und reproduzierbar speichern`_   |
+---------------+--------------------------------------------------------------+
| Abonnement    | `Neues aus Python für Data-Science`_                         |
| à 2 h im      |                                                              |
| Quartal       |                                                              |
+---------------+--------------------------------------------------------------+

.. _`Einführung in Python`:
   https://cusy.io/de/unsere-schulungsangebote/einfuehrung-in-python
.. _`Fortgeschrittenes Python`:
   https://cusy.io/de/unsere-schulungsangebote/fortgeschrittenes-python
.. _`Entwurfsmuster in Python`:
   https://cusy.io/de/unsere-schulungsangebote/entwurfsmuster-in-python
.. _`Effizient Testen mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/effizient-testen-mit-python
.. _`Software-Dokumentation mit Sphinx`:
   https://cusy.io/de/unsere-schulungsangebote/software-dokumentation-mit-sphinx
.. _`Technisches Schreiben`:
   https://cusy.io/de/unsere-schulungsangebote/technisches-schreiben
.. _`Jupyter-Notebooks für effiziente Data-Science-Workflows`:
   https://cusy.io/de/unsere-schulungsangebote/jupyter-notebooks-fuer-effiziente-data-science-workflows
.. _`Numerische Berechnungen mit NumPy`:
   https://cusy.io/de/unsere-schulungsangebote/numerische-berechnungen-mit-numpy
.. _`Daten analysieren mit pandas`:
   https://cusy.io/de/unsere-schulungsangebote/daten-analysieren-mit-pandas
.. _`Daten lesen, schreiben und bereitstellen mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-lesen-schreiben-und-bereitstellen-mit-python
.. _`Daten bereinigen und validieren mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-bereinigen-und-validieren-mit-python
.. _`Daten visualisieren mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-visualisieren-mit-python
.. _`Datenvisualisierungen gestalten`:
   https://cusy.io/de/unsere-schulungsangebote/datenvisualisierungen-gestalten
.. _`Dashboards erstellen`:
   https://cusy.io/de/unsere-schulungsangebote/dashboards-erstellen
.. _`Code und Daten versioniert und reproduzierbar speichern`:
   https://cusy.io/de/unsere-schulungsangebote/code-und-daten-versioniert-und-reproduzierbar-speichern
.. _`Neues aus Python für Data-Science`:
   https://cusy.io/de/unsere-schulungsangebote/neues-aus-python-fuer-data-science

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   context
   shared-instructions/index
   feedback-loops
   procedure/index
   security/index
   jupyter
   glossary
