.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sandboxing-Technologien
=======================

Verschiedene Sandboxing-Technologien stellen unterschiedliche Abwägungen
zwischen den vier Schlüsselfaktoren Sicherheitsisolierung, Leistung,
Startgeschwindigkeit sowie Kompatibilität dar. Keine Technologie ist perfekt –
jede geht mit anderen Kompromissen einher.

MicroVMs
--------

MicroVMs bieten die höchste Sicherheitsisolierung. Sie nutzen
Hardware-Virtualisierung, um jeder Sandbox einen eigenen Kernel, eigenen
Speicherbereich und eigene virtuelle Geräte zuzuweisen. Dadurch entsteht eine
hardwaregestützte Grenze zwischen Gastcode und dem Host-Betriebssystem, wodurch
die bei Containern auftretenden Sicherheitslücken aufgrund des gemeinsam
genutzten Kernels vermieden werden. Die wichtigste Neuerung sind deutlich
schnellere Startzeiten, wodurch sich MicroVMs auch für kurzlebige Workloads
eignen. Damit haben sie die alte Dichotomie *langsame, sichere VMs versus
schnelle, unsichere Container* weitgehend überholt, da sie das Beste aus beiden
Welten vereinen und sich zum neuen Standard für sichere, kurzlebige
Codeausführung entwickelt haben.

.. _firecracker-microvm:

Firecracker
~~~~~~~~~~~

`Firecracker <https://firecracker-microvm.github.io>`_ wurde von :abbr:`AWS
(Amazon Web Services)` entwickelt und als Open-Source-Projekt veröffentlicht.
Der Virtual Machine Monitor (VMM) nutzt :abbr:`KVM (Linux Kernel Virtual
Machine)`, um ressourcenschonende Mikro-VMs zu erstellen und zu verwalten. Seine
DesignIdee ist minimalistisch: Er verzichtet bewusst auf alle nicht essenziellen
Geräte wie USB-Controller, Grafikkarten und Soundkarten, was die potenzielle
Angriffsfläche drastisch reduziert und den Speicherbedarf jeder Mikro-VM auf
weniger als 5 MB senkt.

Firecracker läuft als Prozess im User Space auf einem Host-Rechner und wird über
eine `RESTful-API
<https://de.wikipedia.org/wiki/Representational_State_Transfer>`_ gesteuert.
Diese API ermöglicht die programmatische Konfiguration der Mikro-VM,
einschließlich der Festlegung der Anzahl der vCPUs, der Speichergröße sowie des
Anbindens von Netzwerkschnittstellen oder Block-Devices. Dieser API-gesteuerte
Ansatz ist entscheidend für die Automatisierung des Lebenszyklus von Sandboxes
in Cloud-nativen Anwendungen.

Die herausragende Leistung von Firecracker ist die Startgeschwindigkeit: Die
MicroVM kann in nur 125 Millisekunden starten und Code im User-Space ausführen.
Dies schließt die Lücke zwischen den langsamen Startzeiten herkömmlicher VMs mit
oft mehreren Sekunden und dem schnellen Start von Containern und eignet sich
somit für Workloads mit hohem Durchsatz und bedarfsorientierter Nutzung wie
serverless-Funktionen. Diese Fähigkeit ermöglicht es Diensten wie `AWS Lambda
<https://aws.amazon.com/lambda/>`_, `AWS Fargate
<https://aws.amazon.com/fargate/>`_, :ref:`e2b` und `Fly.io <https://fly.io/>`_
isolierte Ausführungsumgebungen in großem Maßstab bereitzustellen.

Für eine mehrschichtige Verteidigung (`Defense-in-Depth
<https://en.wikipedia.org/wiki/Defense_in_depth_(computing)>`_) setzt
Firecracker `Jailer
<https://github.com/firecracker-microvm/firecracker/blob/main/docs/jailer.md>`_
Prozess ein, der mithilfe von Linux `cgroups
<https://en.wikipedia.org/wiki/Cgroups>`_ und -`Namespaces
<https://en.wikipedia.org/wiki/Linux_namespaces>`_ eine sichere Umgebung
einrichtet, um den :abbr:`VMM (Virtual Machine Monitor)`-Prozess selbst zu
isolieren. Dies bietet eine zweite Sicherheitsebene für den Fall, dass die
Virtualisierungsbarriere kompromittiert wird.

.. _libkrun:

libkrun
~~~~~~~

`libkrun <https://github.com/libkrun/libkrun>`_ ist eine Virtualisierungslösung,
die ressourcenschonende, :abbr:`KVM (Kernel Virtual Machine)`-basierte virtuelle
Maschinen mit minimalem Overhead zu erstellen. Sie bildet die Kerntechnologie
hinter `microsandbox
<https://docs.microsandbox.dev/getting-started/introduction>`_. Sie ermöglicht
es Anwendungen, sichere Sandboxing-Funktionen direkt einzubetten und so eine
echte Isolation auf Hardware-Ebene mit eigenem Kernel und eigenem
Speicherbereich zu erreichen, während die Startzeiten mit denen von Containern
konkurrieren können.

Neben microsandbox wird libkrun von `Podman <https://podman.io/>`_ und `crun
<https://github.com/containers/crun>`_ verwendet.

Containerisierung – Isolierung auf Basis von Namespaces
-------------------------------------------------------

Die Containerisierung ist der am weitesten verbreitete Ansatz zur Isolierung von
Anwendungen. Dabei werden Linux-`Namespaces
<https://en.wikipedia.org/wiki/Linux_namespaces>`_ und -`Control Groups
(cgroups) <https://en.wikipedia.org/wiki/Cgroups>`_ genutzt, um isolierte
Umgebungen zu schaffen, die sich den Host-Kernel teilen. Auch wenn Container
nicht die strengsten Sicherheitsgrenzen bieten, stellen sie doch ein gutes
Gleichgewicht zwischen Kompatibilität, Leistung und einfacher Handhabung dar.

Docker-/OCI-Container
~~~~~~~~~~~~~~~~~~~~~

`Docker <https://www.docker.com>`_ und das umfassendere Ökosystem der `Open
Container Initiative (OCI) <https://opencontainers.org>`_ stellen den
De-facto-Standard für die Containerisierung dar. Container bündeln Anwendungen
mit ihren Abhängigkeiten und sorgen gleichzeitig durch Kernel-Funktionen für
eine Isolierung auf Prozessebene.

Container nutzen mehrere Funktionen des Linux-Kernels zur Isolation:

Namespaces
    wie :abbr:`PID (Process Identifier)`, Mount, Network, User ID, :abbr:`UTS
    (Unix Time Sharing)` und :abbr:`IPC (Inter-Process Communication)` trennen
    Prozessbäume, Dateisysteme und Netzwerk-Stacks voneinander
Control Groups
    begrenzen und überwachen die Ressourcennutzung von CPU, Arbeitsspeicher, I/O
Sicherheitsmodule
    `AppArmor <https://apparmor.net/>`_ oder `SELinux
    <https://github.com/SELinuxProject/selinux>`_ bieten zusätzliche
    Zugriffskontrollen

Im Gegensatz zu VMs nutzen Container den Kernel des Hosts gemeinsam, was sie
zwar leichtgewichtiger, aber potenziell weniger sicher macht.

Vorteile sind die extrem schnellen Startzeiten (10–50 ms), minimaler
Ressourcenaufwand, umfangreiches Ökosystem an Tools und Images, hervorragende
Kompatibilität mit bestehenden Anwendungen sowie ausgereifte
Orchestrierungsplattformen wie `Kubernetes <https://kubernetes.io/>`_. Das
Modell des gemeinsam genutzten Kernels ermöglicht eine effiziente
Ressourcennutzung und macht Container ideal für Microservices-Architekturen.

Der gemeinsam genutzte Kernel schafft jedoch auch potenzielle Angriffsvektoren,
:abbr:`sog. (sogenannte)` *Container Escape Vulnerabilities*. Die meisten
Sicherheitsvorfälle resultieren jedoch aus Fehlkonfigurationen. Eine
ordnungsgemäße Konfiguration, der Verzicht auf privilegierte Container und die
Verwendung minimaler Basis-Images reduzieren die Risiken erheblich – deshalb
bevorzugen Plattformen, auf denen potentiell feindlicher Code ausgeführt werden
kann, Mikro-VMs oder :ref:`language-runtimes`.

Docker-/OCI-Container sind daher nur geeignet für die Bereitstellung
vertrauenswürdiger Anwendungen, Entwicklungsumgebungen, CI/CD-Pipelines und
Microservices-Architekturen, nicht hingegen für die Ausführung nicht
vertrauenswürdigen Codes aus externen Quellen oder Szenarien, die eine maximale
Sicherheitsisolierung erfordern.

Dennoch sind `Docker Hub <https://hub.docker.com/>`_ und `Kubernetes
<https://kubernetes.io/>`_ als Container für Entwicklungsumgebungen sowie
`Gitpod <https://gitpod.io>`_ und `Coder <https://coder.com>`_ als :abbr:`CDEs
(Cloud Development Environments)` weit verbreitet für die Software-Entwicklung.

.. _language-runtimes:

Language-Runtimes
-----------------

Dies ist die leichtgewichtigste Form des Sandboxings, bei der die Isolierung
nicht durch das Betriebssystem oder die Hardware, sondern durch die
Laufzeitumgebung selbst erzwungen wird. Dieser Ansatz bietet die schnellsten
Startzeiten und den geringsten Ressourcenaufwand, ist jedoch hinsichtlich der
Kompatibilität am restriktivsten.

WebAssembly (WASM)
~~~~~~~~~~~~~~~~~~

Das Sicherheitsmodell von `WebAssembly <https://webassembly.org>`_ basiert auf
zwei grundlegenden Prinzipien:

Memory Safety
    WASM-Code wird in einem Speicherbereich ausgeführt, der vollständig vom
    Speicher des Host-Prozesses isoliert ist. Jeder Speicherzugriff wird von der
    Laufzeitumgebung automatisch überprüft, wodurch verhindert wird, dass
    Pufferüberläufe den Host oder andere WASM-Module beeinträchtigen. Der
    Aufrufstapel wird ebenfalls von der Laufzeitumgebung verwaltet und ist für
    den WASM-Code nicht zugänglich, wodurch herkömmliche Stack-Smashing-Angriffe
    neutralisiert werden.
Capability-based security
    Ein WASM-Modul ist zunächst inaktiv. Es verfügt über keine eigene Fähigkeit,
    auf das Dateisystem, das Netzwerk oder andere externe Ressourcen
    zuzugreifen. Um I/O-Vorgänge auszuführen, muss die Host-Umgebung diese
    Fähigkeiten explizit bereitstellen, indem sie bei der Instanziierung
    Funktionen (:abbr:`sog. (sogenannte)` ``imports``) übergibt. Diese *Deny by
    Default*-Strategie stellt sicher, dass ein Modul nur das tun kann, wozu es
    ausdrücklich berechtigt wurde.

Diese Technologie wird auf Docker- und Kubernetes-Plattformen genutzt:

`Fermyon Spin <https://spinframework.dev/>`_
    ist ein Framework zum Erstellen und Ausführen ereignisgesteuerter
    Microservice-Anwendungen mit WebAssembly-Komponenten
`WasmEdge <https://wasmedge.org/>`_
    ist eine ressourcenschonende, leistungsstarke und erweiterbare
    WebAssembly-Laufzeitumgebung für Cloud-native, Edge- und dezentrale
    Anwendungen
`Wasmtime <https://wasmtime.dev/>`_
    ist eine schlanke WebAssembly-Laufzeitumgebung, die schnell, sicher und
    standardkonform ist

.. seealso::
   * `WebAssembly on Kubernetes
     <https://www.cncf.io/blog/2024/03/12/webassembly-on-kubernetes-from-containers-to-wasm-part-01/>`_
