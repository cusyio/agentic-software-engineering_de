Sicherheit
==========

Einige Coding-Agenten erlauben, die Berechtigungen dort festzulegen,
:abbr:`z. B. (zum Beispiel)` automatisch oder mit einer Whitelist oder in einer
Sandbox. Diese Berechtigungen bleiben jedoch anfällig für `Lethal Trifecta
<https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_, als wenn euer
Coding-Agent Zugriff auf private Daten hat, nicht vertrauenswürdigen Inhalten
ausgesetzt ist und extern kommunizieren kann.

In diesen Fällen definieren wir unsere eigenen virtuellen Umgebungen und
Berechtigungen, sodass der Coding-Agent nicht aus dieser Umgebung ausbrechen
kann.

.. seealso::
   * Bundesamt für Sicherheit in der Informationstechnik (BSI): `Evasion-Attacks
     auf LLMs – Eine Checkliste zur Härtung des LLM-Systems
     <https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/KI/Evasion-Angriffe_auf_LLMs-Checkliste.pdf>`_
