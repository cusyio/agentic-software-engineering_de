Agentische Softwareentwicklung
==============================

Agentische Programmierumgebungen wie Claude Code oder Cursor können im Gegensatz
zu einem Chatbot nicht nur Fragen beantworten, sondern auch eure Dateien lesen,
Befehle ausführen, Änderungen vornehmen und autonom Probleme lösen. Dies
verändert unsere Arbeitsweise: anstatt selbst Code zu schreiben und die
agentische Programmierumgebung zu bitten, ihn zu überprüfen, beschreiben wir
nun, was wir wollen, und der Agent recherchiert, plant und setzt es um.

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

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   setup
   context
   verify
   procedure
   jupyter
   context
   verify
   procedure
   security
