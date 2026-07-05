.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Plattformen
===========

Dieser Abschnitt enthält eine detaillierte, strukturierte Analyse jeder
wichtigen Plattform.

.. _e2b:

e2b
---

`e2b <https://e2b.dev>`_ ist eine Open-Source-Cloud-Laufzeitumgebung, die
speziell auf die Anforderungen von KI-Anwendungen und autonomen Agenten
zugeschnitten ist. Sie bietet Entwicklungsteams Sandbox-basierte
Cloud-Umgebungen auf Basis von :ref:`Firecracker-Mikro-VMs
<firecracker-microvm>` und ermöglicht so die sichere Ausführung von
KI-generiertem Code. Die Plattform legt großen Wert darauf, Entwicklungsteams
durch ihre SDKs ein nahtloses Entwickler-Erlebnis zu bieten, und ist als
Backend-Infrastruktur für agentenbasierte Arbeitsabläufe konzipiert.

.. csv-table:: GitHub-Insights
    :header: "Stars", "Mitwirkende", "Commit-Aktivität", "Lizenz"

    ".. image:: https://raster.shields.io/github/stars/e2b-dev/E2B",".. image:: https://raster.shields.io/github/contributors/e2b-dev/E2B",".. image:: https://raster.shields.io/github/commit-activity/y/e2b-dev/E2B",".. image:: https://raster.shields.io/github/license/e2b-dev/E2B"

e2b bietet eine `Anleitung zum Self-Hosting
<https://github.com/e2b-dev/infra/blob/main/self-host.md>`_ sowie
Terraform-Skripte für die Bereitstellung der Infrastruktur auf eurem GCP- oder
AWS-Konto; Azure- und Linux sollen folgen.

Die Sandboxen bieten uneingeschränkte Dateisystem-I/O-Funktionen. Die
:abbr:`SDKs (Software Development Kits)` für Python und JavaScript ermöglichen
das programmgesteuerte Hoch- und Herunterladen von Dateien, wodurch es einfach
ist, einem Agenten Kontextinformationen bereitzustellen oder Artefakte von ihm
abzurufen. Die Umgebungen sind persistent, :abbr:`d. h. (das heißt)`, Änderungen
am Dateisystem und installierte Pakete bleiben über mehrere Ausführungsaufrufe
innerhalb einer einzigen Sitzung hinweg erhalten.

Die Sandboxen verfügen standardmäßig über uneingeschränkten Internetzugang.
Darüber hinaus kann jeder innerhalb der Sandbox ausgeführte Dienst (:abbr:`z. B.
(zum Beispiel)` ein Web-Server) über eine eindeutige, sichere URL, die von der
e2b-Plattform bereitgestellt wird, für das öffentliche Internet zugänglich
gemacht werden, was Anwendungsfälle wie das Hosten generierter Webanwendungen
oder die Bereitstellung von APIs aus der Sandbox heraus erleichtert.

e2b ist äußerst vielseitig und eignet sich sowohl für kurzlebige als auch für
lang laufende Workloads. Die schnelle Startzeit (~150–200 ms) ist ideal für
kurzlebige Aufgaben wie die Ausführung eines einzelnen Code-Schnipsels zur
Datenanalyse. Es werden jedoch auch Sitzungen mit einer Dauer von bis zu 24
Stunden unterstützt und ist damit robust genug für komplexe, zustandsbehaftete
agentenbasierte Aufgaben, Entwicklungsumgebungen oder anspruchsvolle
Trainingsschleifen im Bereich des Reinforcement-Learning, die einen persistenten
Zustand erfordern.

.. _microsandbox:

microsandbox
------------

`microsandbox <https://docs.microsandbox.dev/getting-started/introduction>`_ ist
eine selbst gehostete Plattform, die sich ausschließlich darauf konzentriert,
maximale Sicherheit bei der Ausführung von nicht vertrauenswürdigem Code zu
gewährleisten. Sie kombiniert die Isolation auf Hardware-Ebene von auf
:ref:`libkrun` basierenden Micro-VMs mit der Startgeschwindigkeit von Containern
unter 200 ms und der vollständigen Kontrolle, die ein Selbst-Hosting bietet. Sie
wurde entwickelt, um den Zielkonflikt zwischen Sicherheit, Geschwindigkeit und
Kontrolle ohne Kompromisse zu lösen.

.. csv-table:: GitHub-Insights
    :header: "Stars", "Mitwirkende", "Commit-Aktivität", "Lizenz"

    ".. image:: https://raster.shields.io/github/stars/superradcompany/microsandbox",".. image:: https://raster.shields.io/github/contributors/superradcompany/microsandbox",".. image:: https://raster.shields.io/github/commit-activity/y/superradcompany/microsandbox",".. image:: https://raster.shields.io/github/license/superradcompany/microsandbox"

microsandbox unterstützt sowohl persistente als auch kurzlebige Dateisysteme. In
projektbasierten Workflows (``msr``) werden Dateiänderungen und Installationen
innerhalb einer Sandbox automatisch in einem lokalen Verzeichnis :file:`./menv`
auf dem Host gespeichert. So könnt ihr eine Sandbox anhalten und neu starten,
ohne eure Arbeit zu verlieren. Für einmalige Aufgaben werden zudem temporäre
Sandboxen (``msx``) unterstützt, die nach der Ausführung keine Spuren
hinterlassen. Damit ist microsandbox äußerst flexibel und eignet sich sowohl für
kurzlebige, zustandslose Aufgaben als auch für lang laufende, zustandsbehaftete
Workloads. Der Befehl ``msx`` ist für schnelle, kurzlebige Ausführungen
konzipiert, während der projektbasierte Befehl ``msr`` mit seinem persistenten
Zustand ideal für laufende Entwicklungsarbeiten oder komplexe, mehrstufige
Prozesse ist, bei denen der Kontext erhalten bleiben muss.

Zudem können die Sandboxen für `ein- und ausgehenden Datenverkehr
<https://docs.microsandbox.dev/sdk/python/networking>`_ konfiguriert werden mit
veröffentlichten Ports, DNS- und TLS-Interception sowie der Behandlung von
Geheimhaltungsverstößen.

.. _kata_containers:

Kata Containers
---------------

`Kata Containers <https://katacontainers.io>`_ ist eine Open-Source
Container-Laufzeitumgebung, die die Geschwindigkeit von Containern mit der
Sicherheit virtueller Maschinen verbindet. Sie wurde 2017 von der Open
Infrastructure Foundation ins Leben gerufen und verfolgt einen einzigartigen
Ansatz für die Containersicherheit, indem jeder Container in einer eigenen,
ressourcenschonenden virtuellen Maschine ausgeführt wird. Dieser hybride Ansatz
behebt die grundlegenden Sicherheitsbedenken herkömmlicher
Container-Laufzeitumgebungen und gewährleistet gleichzeitig die Kompatibilität
mit dem Container-Ökosystem.

.. csv-table:: GitHub-Insights
    :header: "Stars", "Mitwirkende", "Commit-Aktivität", "Lizenz"

    ".. image:: https://raster.shields.io/github/stars/kata-containers/kata-containers",".. image:: https://raster.shields.io/github/contributors/kata-containers/kata-containers",".. image:: https://raster.shields.io/github/commit-activity/y/kata-containers/kata-containers",".. image:: https://raster.shields.io/github/license/kata-containers/kata-containers"

Kata Containers erstellt für jeden Container eine ressourcenschonende virtuelle
Maschine, die eine hardwaregestützte Isolierung gewährleistet und gleichzeitig
die Kompatibilität mit dem Container-Ökosystem sicherstellt. Es unterstützt
mehrere Hypervisors, darunter `QEMU <https://www.qemu.org/>`_, `Cloud-Hypervisor
<https://www.cloudhypervisor.org/>`_ und `Firecracker
<https://github.com/firecracker-microvm/firecracker-containerd>`_.

Kata Containers erlaubt vollständige Dateisystemfunktionen innerhalb des
Container-/VM-Hybrids mit standardmäßigen Optionen zum Einbinden von
Container-Volumes und zur Speicherung. Auch der Netzwerkzugriff umfasst die
containerüblichen Netzwerkmodelle und Kubernetes-Netzwerkintegration. Insgesamt
lösen Kata Containers das Dilemma *Container vs. VM*, indem es beides
bereitstellt und damit die betrieblichen Vorteile von Containern (schneller
Start, hohe Dichte, Orchestrierung) bei gleichzeitiger Gewährleistung der
Sicherheit von VMs (Hardware-Isolation, dedizierter Kernel) bereitstellt. Dies
macht die Lösung besonders wertvoll für Produktionsumgebungen, in denen nicht
vertrauenswürdige Workloads ausgeführt werden oder die Einhaltung gesetzlicher
Vorschriften erforderlich ist.
