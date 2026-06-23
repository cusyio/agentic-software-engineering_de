.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Guardrails
==========

Der Begriff *AI-Guardrails* tauchte erstmals im Jahr 2023 auf, als ChatGPT
seinen Dienst für Verbraucher einführte. Die ersten Versionen wiesen zahlreiche
Sicherheits- und Datenschutzlücken auf, was schließlich viele Projekte dazu
veranlasste, Schutzmaßnahmen zu installieren, um Datenschutzprobleme,
Sicherheitsrisiken und fragwürdige Inhalte mit ethischen Problemen einzudämmen.

Wer sich jedoch mit Hash-Filtern auskennt, weiß, dass dabei Übereinstimmungen
mit geringfügigen Änderungen übersprungen werden können. So haben :abbr:`z. B.
(zum Beispiel)` `Daphne Ippolito et al. <https://arxiv.org/abs/2210.17546>`_
nachgewiesen, dass der Copilot-Filter für urheberrechtlich geschützten Code
umgangen werden kann, indem die Variablennamen in Französisch geändert wurden:

.. blacken-docs:off

.. code-block:: python
   :emphasize-lines: 1

   float Q_rsqrt( float number )
   {
     long i;
     float x2 , y;
     const float threehalfs = 1.5F;

     x2 = number * 0.5F;
     y  = number;
     i  = * ( long * ) &y;

.. blacken-docs:on

Copilot hörte an dieser Stelle mit der Generierung auf.

Der Prompt mit französischem Variablennamen umgeht jedoch den Filter:

.. blacken-docs:off

.. code-block:: python
   :emphasize-lines: 1

   float Q_sqrt( float nombre )
   {
     long i ;
     float x2 , y;
     const float trois_moitie = 1.5 F;

     x2 = nombre * 0.5F;
     y  = nombre;
     i  = * ( lo ng * ) &y;
     i  = 0x5f3759df - ( i >> 1 )
     y  = * ( float * ) &i;
     y  = y * (trois_moitie - (x2*y*y));
     //y = y * (trois_moitie - (x2*y*y));

     return nombre * y;
   }

.. blacken-docs:on

Das Modell wurde mit aktivierter Option *“Block suggestions matching public
code”* ausgeführt. Die Prompts sind hervorgehoben.

So sind diese Filter nur in Ausnahmefällen hilfreich, allerdings auch nur bei
sehr eng gefassten Datenschutzproblemen. Wie viele Eingabe-/Ausgabefilter
sind sie von ihrer Konzeption her anfällig und lassen sich nicht gut skalieren,
da sie ja weiterhin in einem großen, nicht deterministisches System
funktionieren sollen – eine Lösung, die schon immer unzureichend ist.

.. tip::
   Ihr solltet euch nicht auf AI-Guardrails verlassen sondern eigene
   Sicherheitsvorkehrungen vor und nach euren API-Aufrufen integrieren.

NVIDIA NeMo Guardrails
----------------------

`NVIDIA NeMo <https://www.nvidia.com/de-de/ai-data-science/products/nemo/>`_ ist
eine agentenorientierte, offene Suite zur Optimierung und Steuerung von
KI-Agenten. Die `NeMo Guardrails Library
<https://docs.nvidia.com/nemo/guardrails/latest/about-nemo-guardrails-library/overview>`_
(→ `PyPI <https://pypi.org/project/nemoguardrails/>`_, → `GitHub
<https://github.com/NVIDIA-NeMo/Guardrails>`_) ist ein Python-Paket zum
Erstellen programmierbarer Guardrails für LLM-basierte Anwendungen. Sie
integriert sich in `Llama 3.1 NemoGuard 8B Content Safety
<https://build.nvidia.com/nvidia/llama-3_1-nemotron-safety-guard-8b-v3>`_,
`Llama-Guard <https://docs.nvidia.com/nemo/guardrails/latest/configure-guardrails/guardrail-catalog/third-party/llama-guard>`_ `und viele andere
<https://docs.nvidia.com/nemo/guardrails/latest/configure-guardrails/guardrail-catalog/third-party>`_. Außerdem sollen Jailbreak-Versuche erkannt werden, die
Tool-Integration überprüft und Aktionen der Agenten protokolliert werden.

Cloud- oder KI-Anbieter
-----------------------

Viele KI-Modellanbieter stellen auch ihre eigenen Guardrails zur Verfügung. Ihr
könnt zwar auf diese keinen Einfluss nehmen, dennoch solltet ihr sie in diesen
Systemumgebungen verwenden. Einige von ihnen stellen jedoch auch separate
Guardrail-Modelle und deterministische Softwaretests zur Verfügung, die ihr
nutzen könnt.

Amazon Bedrock Guardrails
~~~~~~~~~~~~~~~~~~~~~~~~~

`Amazon Bedrock Guardrails <https://aws.amazon.com/bedrock/guardrails/>`_ bietet
mehrere Guardrail-Modellen und softwarebasierten Tests. so bieten die *Content
Filters* zwei Guardrail-Modelle, eines, das auf die Kategorien toxischer
Prompt-Stile in multimodalen Eingaben trainiert wurde, und ein anderes, das auf
potenzielle Prompt-Injection- oder Jailbreak-Angriffe trainiert wurde.

.. seealso::
   * `So konfigurieren Sie Inhaltsfilter
     <https://docs.aws.amazon.com/de_de/bedrock/latest/userguide/guardrails-content-filters-overview.html>`_
   * `Prompt-Angriffe
     <https://docs.aws.amazon.com/de_de/bedrock/latest/userguide/guardrails-prompt-attack.html>`_

Anthropic Claude Code
~~~~~~~~~~~~~~~~~~~~~

Claude Code bietet keine expliziten Guardrails an. Stattdessen sollen eigene
`Streaming-Ablehnungen
<https://platform.claude.com/docs/de/test-and-evaluate/strengthen-guardrails/handle-streaming-refusals>`_
formuliert werden.

.. warning::
   Exfiltration von Prompts ist relativ einfach, siehe `Effective Prompt
   Extraction from Language Models <https://arxiv.org/abs/2307.06865>`_.

Azure
~~~~~

Über die `Azure AI Foundry
<https://techcommunity.microsoft.com/blog/azureinfrastructureblog/guardrails-for-generative-ai-securing-developer-workflows/4505801>`_
können die Guardrails eingerichtet werden.

.. seealso::
   * `Prompt Shields
     <https://learn.microsoft.com/de-de/azure/ai-services/content-safety/concepts/jailbreak-detection>`_
   * `Spotlighting
     <https://learn.microsoft.com/de-de/azure/foundry/openai/concepts/content-filter-prompt-shields#spotlighting-preview>`_

OpenAI
~~~~~~

In `OpenAI Guardrails <https://guardrails.openai.com/>`_ werden eigene
Guardrail-Modelle sowie Anleitungen zur Konfiguration dieser Modelle
bereitgestellt.

.. seealso::
   * `github.com/openai/openai-guardrails-python
     <https://github.com/openai/openai-guardrails-python>`_

   * `gpt-oss-safeguard
     <https://huggingface.co/collections/openai/gpt-oss-safeguard>`_

     * `User guide for gpt-oss-safeguard
       <https://developers.openai.com/cookbook/articles/gpt-oss-safeguard-guide>`_

OpenGuardrails
~~~~~~~~~~~~~~

`OpenGuardrails <https://openguardrails.com>`_ hat zwei offene Modelle und einen
Trainingsdatensatz bei `Huggungface <https://huggingface.co/openguardrails>`_
veröffentlicht. Darüberhinaus bietet OpenGuardrails Open-Source-Software, mit
der Guardrails sowohl für normale LLM-/KI-Workflows als auch für agentische
Aufgaben bereitgestellt werden können.

.. seealso::
   * `OpenGuardrails: A Configurable, Unified, and Scalable Guardrails Platform
     for Large Language Models <https://arxiv.org/abs/2510.19169>`_

Guardrails werden euch nicht retten
-----------------------------------

Guardrails sind nützlich, aber auch fehleranfällig. Um Datensicherheit umfassend
zu gewährleisten, braucht es mehrere Wege, um Risiken anzugehen und zu
kontrollieren. Bei Guardrails sollten sie nicht davon ausgehen, unsicheres
Verhalten verhindern, aktuelle Beispiele zeigen das Gegenteil:

.. figure:: sliong.png
   :alt: Son Luong (@sluongng): Codex just found a “workaround” of not having
         sudo on my pc…

   Quelle: https://x.com/sluongng/status/2060746160558543217
