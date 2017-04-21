<properties
   pageTitle="Konfigurieren von Anwendung Abhängigkeitsmonitor (ADM) in Operations Management Suite (OMS) | Microsoft Azure"
   description="Anwendung Abhängigkeit Monitor (ADM) ist eine Operations Management Suite (OMS) Lösung, die automatisch erkennt Anwendungskomponenten auf Windows- und Linux-Systemen und Kommunikation zwischen Karten.  Dieser Artikel enthält Einzelheiten für ADM in Ihrer Umgebung bereitstellen und verwenden ihn in verschiedenen Szenarien."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Konfigurieren von Anwendung Abhängigkeitsmonitor Lösung in Operations Management Suite (OMS)
![Management-Warnsymbol](media/operations-management-suite-application-dependency-monitor/icon.png) Anwendung Abhängigkeit Monitor (ADM) automatisch ermittelt Komponenten auf Windows und Linux und ordnet die Kommunikation zwischen Diensten. Dadurch werden die Server angezeigt, wie Sie – als verbundener Systeme, die wichtige Dienste bereitstellen.  Anwendung Abhängigkeit zeigt Verbindungen zwischen Servern, Prozesse und benötigten Ports über TCP Verbindung Architektur ohne Konfiguration als Installation eines Agents.

Dieser Artikel beschreibt die Details der Anwendung Abhängigkeitsmonitor und Onboarding-Agents konfigurieren.  Informationen zur Verwendung von ADM finden Sie unter [verwenden Anwendung Abhängigkeitsmonitor Lösung in Operations Management Suite (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Anwendung Abhängigkeitsmonitor ist derzeit in privaten Vorschau.  Sie können Zugriff auf die privaten ADM-Vorschau an [https://aka.ms/getadm](https://aka.ms/getadm)anfordern.
>
>Bei privaten Vorschau OMS-Konten haben uneingeschränkten Zugriff auf Verwaltung  ADM-Knoten können aber Protokoll Analysedaten für AdmComputer_CL und AdmProcess_CL wie andere Lösung gemessen.
>
>Nach ADM public Preview eingibt, werden zu kostenlosen und kostenpflichtigen Kunden Einblicke und Analysen in OMS Preise planen verfügbar.  Kostenlos-Konten werden nur 5 ADM-Knoten.  Wenn Sie private Vorschau teilnehmen, nicht OMS Preise Plan registriert sind tritt ADM public Preview-Version wird ADM zu diesem Zeitpunkt deaktiviert. 



## <a name="connected-sources"></a>Verbundene Datenquellen
Die folgende Tabelle beschreibt die verbundenen Quellen der ADM-Lösung unterstützt.

| Verbundene Datenquelle | Unterstützt | Beschreibung |
|:--|:--|:--|
| [Windows-Agenten](../log-analytics/log-analytics-windows-agents.md) | Ja | ADM analysiert und sammelt Daten aus Windows-Agent-Computer.  <br><br>Beachten Sie, dass neben der Agent OMS Windows-Agenten Abhängigkeit Microsoft Agent.  Finden Sie unter [unterstützte Betriebssysteme](#supported-operating-systems) für eine vollständige Liste der Betriebssysteme. |
| [Linux-Agenten](../log-analytics/log-analytics-linux-agents.md) | Ja | ADM analysiert und sammelt Daten von Linux-Agent-Computer.  <br><br>Beachten Sie, dass neben der Agent OMS Linux-Agenten Abhängigkeit Microsoft Agent.  Finden Sie unter [unterstützte Betriebssysteme](#supported-operating-systems) für eine vollständige Liste der Betriebssysteme. |
| [SCOM Verwaltungsgruppe](../log-analytics/log-analytics-om-agents.md) | Ja | ADM analysiert und sammelt Daten von Windows- und Linux-Agents in einer verbundenen SCOM-Verwaltungsgruppe. <br><br>Eine direkte Verbindung von SCOM Agentcomputer mit OMS ist erforderlich. Vom OMS-Repository aus der Verwaltungsgruppe weitergeleitet Daten gesendet.|
| [Azure Storage-Konto](../log-analytics/log-analytics-azure-storage.md) | Nein | ADM sammelt Daten von Agent-Computern, es gibt also keine Daten zum Erfassen von Azure-Speicher. |

Beachten Sie, dass ADM nur 64-Bit-Plattformen unterstützt.

Unter Windows Microsoft Monitoring Agent (MMA) sammeln und Senden von SCOM und OMS verwendet Überwachungsdaten.  (Dieser Agent wird SCOM-Agenten, OMS Agent, MMA oder direkte Agent je nach Kontext bezeichnet.)  SCOM und OMS bieten verschiedene aus Feld Versionen von MMA, aber diese Versionen können jeden Bericht auf SCOM oder in OMS.  Auf Linux, OMS-Agent für Linux sammelt und Überwachungsdaten OMS sendet.  Können ADM auf Servern mit Direct OMS oder auf Servern, die OMS über SCOM Gruppen zugeordnet sind.  In dieser Dokumentation bezeichnen wir all diesen Agents – unter Linux oder Windows, ob SCOM mg oder OMS-als "OMS Agent", wenn der Kontext der spezifischen Namen des Agenten benötigt wird.

ADM-Agent sendet alle Daten und erfordert keine Änderungen Firewalls oder Ports.  ADM Daten werden immer vom OMS-Agent zu OMS, entweder direkt oder über den OMS-Gateway übertragen.

![ADM-Agenten](media/operations-management-suite-application-dependency-monitor/agents.png)

Wenn Sie Kunde SCOM mit einer Verwaltungsgruppe OMS verbunden sind:

- Wenn die SCOM-Agenten im Internet Verbindung mit OMS zugreifen können, ist keine weitere Konfiguration erforderlich.  
- Wenn die SCOM-Agenten OMS über das Internet zugreifen können, müssen Sie die OMS-Gateways SCOM arbeiten.
  
Bei Verwendung der direkten OMS-Agent müssen Sie der OMS-Agent selbst OMS oder OMS-Gateway herzustellen.  OMS-Gateway kann von [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666) heruntergeladen werden


### <a name="avoiding-duplicate-data"></a>Vermeiden von Duplikaten

Wenn Sie SCOM-Kunde sind, konfigurieren Sie nicht die SCOM-Agenten kommunizieren direkt mit OMS oder Daten zweimal gemeldet.  In ADM führt dies zu Computer zweimal in der Rechnerliste angezeigt werden.

Konfiguration von OMS soll nur in einem der folgenden Speicherorte:

- Bereich SCOM-Konsole Operations Management Suite für verwaltete Computer
- Azure betriebliche Informationen Konfiguration in den Eigenschaften des MMA

Beide Konfigurationen mit dem gleichen Arbeitsbereich in jedem verursacht Duplizierung von Daten. Verwenden mit unterschiedlichen Arbeitsbereichen führen in Konflikt stehende Konfiguration (eine ADM-Lösung aktiviert und ohne), die Daten fließen in ADM vollständig verhindern.  

Beachten Sie, dass auch bei die Maschine in SCOM-Konsole OMS-Konfiguration angegeben ist, ist eine Instanz Gruppe wie"Windows Server-Instanzen" aktiv, es weiterhin in der empfangenden OMS Computerkonfiguration über SCOM kann.



## <a name="management-packs"></a>Managementpacks
ADM in OMS-Arbeitsbereich aktiviert, ist 300 KB Management Pack auf allen Microsoft überwachen Agents in diesem Arbeitsbereich gesendet.  Bei Verwendung von SCOM Agents in einer [Verwaltungsgruppe verbunden](../log-analytics/log-analytics-om-agents.md)werden ADM-Management Pack von SCOM bereitgestellt.  Die Agenten direkt verbunden sind, werden die VA von OMS zugestellt.

Die VA heißt Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Bezieht sich auf *%Programfiles%\Microsoft Überwachung Agent\Agent\Health State\Management Servicepacks\*.  Das Management Pack verwendete Datenquelle ist *Files%\Microsoft Überwachung Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfiguration
Neben Windows und Linux Computer einen Agent installiert und OMS verbunden haben, Abhängigkeit Agent-Installationsprogramm muss ADM-Lösung heruntergeladen und dann als Stamm oder Administrator auf jedem verwalteten Server installiert.  Nach der Installation des ADM-Agents auf einem Server an OMS werden ADM-Abhängigkeit wird innerhalb von 10 Minuten angezeigt.  Wenn Sie Probleme haben, e-Mail- [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migrieren von BlueStripe FactFinder
Anwendung Abhängigkeitsmonitor liefert BlueStripe Technologie in OMS in Phasen. FactFinder für Kunden weiterhin unterstützt jedoch steht nicht mehr für die Bestellung.  Diese Vorabversion des Abhängigkeit kann nur mit OMS kommunizieren.  FactFinder Kunde sind, identifizieren Sie einen Satz von Test-Server für ADM FactFinder nicht verwaltet werden. 

### <a name="download-the-dependency-agent"></a>Abhängigkeit Agenten herunterladen
Microsoft Management Agent (MMA) und OMS Linux-Agent die Verbindung zwischen dem Computer und dem OMS bereitstellen, müssen alle Computer analysiert Anwendung Abhängigkeitsmonitor Abhängigkeit Agent installiert.  Unter Linux muss der OMS-Agent für Linux vor der Abhängigkeit Agent installiert sein. 

![Anwendung Abhängigkeitsmonitor Kachel](media/operations-management-suite-application-dependency-monitor/tile.png)

Abhängigkeit Agent herunterladen, klicken Sie auf **Lösung konfigurieren** der **Anwendung Abhängigkeitsmonitor** Kachel Blade **Abhängigkeit Agent** öffnen.  Blade Abhängigkeit Agent verbindet die Windows- und Linux-Agenten. Klicken Sie auf den entsprechenden Link, um jeden Agent herunterladen. Finden Sie im folgenden Weitere Informationen zum Installieren des Agenten auf verschiedenen Systemen.

### <a name="install-the-dependency-agent"></a>Installieren Sie den Agent Abhängigkeit

#### <a name="microsoft-windows"></a>Microsoft Windows
Installieren oder Deinstallieren des Agents sind Administratorrechte erforderlich.

Der Abhängigkeit ist auf Windows-Computern mit ADM-Agent Windows.exe Agent. Wenn Sie diese Datei ohne Optionen ausführen, wird es ein Assistent gestartet, der Sie folgen, um die Installation interaktiv ausführen.  

Gehen Sie die Abhängigkeit Agenteninstallation auf jedem Windows-Computer.

1.  Sicherstellen Sie, dass der OMS Agent ist mit der Connect Computer direkt in OMS.
2.  Windows-Agenten herunterladen und mit dem folgenden Befehl ausführen.<br>*ADM-Agent Windows.exe*
3.  Führen Sie den Assistenten zum Installieren des Agents.
4.  Wenn der Abhängigkeit Agent gestartet, Überprüfen der Protokolle Informationen. Auf Windows-Agenten ist Verzeichnis *C:\Program Files\BlueStripe\Collector\logs*. 

Abhängigkeit Agent für Windows kann von einem Administrator über das Bedienfeld deinstalliert werden.


#### <a name="linux"></a>Linux
Zum Installieren oder konfigurieren Sie den Agent ist Root-Zugriff erforderlich.

Der Abhängigkeit wird auf Linux-Computern mit ADM-Agent-Linux64.bin, ein Shell-Skript mit Selbstextrahierende Binärdatei Agent. Führen Sie die Datei mit sh oder fügen Sie execute-Berechtigungen für die Datei selbst.
 
Gehen Sie die Abhängigkeit Agent auf jedem Linux-Computer installieren.

1.  OMS-Agent installiert die Anleitung mit [Sammeln und Verwalten von Daten von Linux-Computern.  Bevor Linux Abhängigkeit Agent installiert sein muss](https://technet.microsoft.com/library/mt622052.aspx).
2.  Installieren Sie den Agent Linux Abhängigkeit als Root mit dem folgenden Befehl.<br>*sh ADM-Agent-Linux64.bin*.
3.  Wenn der Abhängigkeit Agent gestartet, Überprüfen der Protokolle Informationen. Das Protokollverzeichnis ist auf Linux-Agenten */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Deinstallieren des Agenten Abhängigkeit unter Linux
Vollständig deinstallieren Abhängigkeit Agent unter Linux müssen Sie Entfernen der Agent selbst und der Proxy automatisch mit dem Agent installiert ist.  Sie können mit dem folgenden Befehl deinstallieren.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Installation über die Befehlszeile
Im vorherige Abschnitt Anleitungen auf Standardoptionen Abhängigkeit Netzwerkmonitor-Agent installieren.  Die folgenden Abschnitte enthalten Hinweise zur Installation des Agenten über eine Befehlszeile mit benutzerdefinierten Optionen.

#### <a name="windows"></a>Windows
Verwenden Sie Optionen in der folgenden Tabelle für die Installation über die Befehlszeile. Eine Liste der Installation Flags führen Sie das Installationsprogramm mit der /? wie folgt gekennzeichnet.

    ADM-Agent-Windows.exe /?

| Kennzeichen | Beschreibung |
|:--|:--|
| / S | Ausführen einer unbeaufsichtigten Installation ohne Benutzereingaben. |

Dateien für Windows Abhängigkeit Agent befinden sich standardmäßig in *C:\Program Files\BlueStripe\Collector* .


#### <a name="linux"></a>Linux
Verwenden Sie Optionen in der folgenden Tabelle für die Installation. Um eine Liste der Installation ausführen Flags das Installationsprogramm mit - Flag wie folgt.

    ADM-Agent-Linux64.bin -help

| Beschreibung
|:--|:--|
| -s | Ausführen einer unbeaufsichtigten Installation ohne Benutzereingaben. |
| --Prüfen | Berechtigungen und Betriebssystem überprüft, aber nicht den Agenten installieren. |

Dateien für die Abhängigkeit Agent werden in den folgenden Verzeichnissen abgelegt.

| Dateien | Speicherort |
|:--|:--|
| Core-Dateien | /usr/lib/bluestripe-Collector |
| Protokolldateien | /var/OPT/Microsoft/Dependency-Agent/log |
| Konfigurationsdateien | /etc/opt/Microsoft/Dependency-Agent/config |
| Service-Programmdateien | /sbin/bluestripe-Collector<br>/sbin/bluestripe-Collector-Manager |
| Binäre Dateien | /var/OPT/Microsoft/Dependency-Agent/Storage |



## <a name="troubleshooting"></a>Problembehandlung
Mit Anwendung Abhängigkeit Problemen können Sie Informationen zur Problembehandlung von mehreren Komponenten mit den folgenden Informationen sammeln.

### <a name="windows-agents"></a>Windows-Agenten

#### <a name="microsoft-dependency-agent"></a>Abhängigkeit von Microsoft Agent
Daten zur Problembehandlung von Abhängigkeit Agent generiert ein Eingabeaufforderungsfenster als Administrator, und führen Sie das CollectBluestripeData.vbs-Skript mit dem folgenden Befehl.  Sie können-Flag Hilfe zusätzliche Optionen angezeigt.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Support-Datenpaket wird im Verzeichnis USERPROFILE für den aktuellen Benutzer gespeichert.  Sie können der Datei <filename> an einem anderen Speicherort speichern.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Microsoft Dependency Agent Management Pack für MMA
Abhängigkeit Agent Management Pack in Microsoft Management Agent ausgeführt wird.  Er empfängt Daten vom Agent Abhängigkeit und ADM-Cloud-Dienst weitergeleitet.
  
Stellen Sie sicher, dass das Management Pack folgendermaßen heruntergeladen wird.

1.  Suchen Sie nach einer Datei namens Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp in C:\Program Files\Microsoft Überwachung Agent\Agent\Health State\Management Servicepacks.  
2.  Die Datei ist nicht vorhanden, und der Agent einer Verwaltungsgruppe SCOM verbunden, dann sicher, dass sie durch Überprüfen der Management Packs im Arbeitsbereich Verwaltung der Betriebskonsole in SCOM importiert wurde.

ADM-MP schreibt Ereignisse in Operations Manager Windows-Ereignisprotokoll.  Das Protokoll kann über die System-Log-Lösung [in OMS durchsucht,](../log-analytics/log-analytics-log-searches.md) wo Sie die Dateien hochladen können.  Wenn Ereignisse aktiviert sind, werden sie in das Anwendungsereignisprotokoll mit der Ereignisquelle *AdmProxy*geschrieben.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Zum Sammeln von diagnostischen Ablaufverfolgungsinformationen ein Eingabeaufforderungsfenster als Administrator, und führen Sie die folgenden Befehle: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Traces werden in c:\Windows\Logs\OpsMgrTrace geschrieben.  Sie können die Protokollierung mit StopTracing.cmd beenden.


### <a name="linux-agents"></a>Linux-Agenten

#### <a name="microsoft-dependency-agent"></a>Abhängigkeit von Microsoft Agent
Daten zur Problembehandlung aus der Abhängigkeit Agent, melden Sie sich mit einem Konto mit Sudo oder Root-Berechtigungen generieren, und führen Sie den folgenden Befehl.  Sie können-Flag Hilfe zusätzliche Optionen angezeigt.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Support-Datenpaket wird in /var/opt/microsoft/dependency-agent/log gespeichert (wenn Stamm) der Agent-Installation im Verzeichnis oder in das Basisverzeichnis des Benutzers (falls kein Stammelement) Skript.  Sie können der Datei <filename> an einem anderen Speicherort speichern.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Microsoft Dependency Agent Fluentd Plug-in für Linux
Abhängigkeit Agent Fluentd Plug-in in OMS Linux-Agent ausgeführt wird.  Er empfängt Daten vom Agent Abhängigkeit und ADM-Cloud-Dienst weitergeleitet.  

Protokolle sind in die folgenden zwei Dateien geschrieben.

- /var/OPT/Microsoft/omsagent/log/omsagent.log
- Messages

#### <a name="oms-agent-for-linux"></a>OMS-Agenten für Linux
Eine Problembehandlung Ressource für OMS mit Linux-Servern finden Sie hier: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

Die Protokolle für den OMS-Agent für Linux befinden sich im */Var/opt/Microsoft/Omsagent/Log/*.  

Die Protokolle auf Omsconfig (Agentenkonfiguration) befinden sich im */Var/opt/Microsoft/Omsconfig/Log/*.
 
Das Protokoll für OMI und SCX die Performance Metrikdaten befinden sich in */Var/opt/Omi/Log/* und */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Datensammlung
Sie können jeden Agent übertragen etwa 25MB pro Tag, je nachdem, wie komplex die systemabhängigkeiten erwarten.  ADM-Abhängigkeitsdaten von jeder Agent alle 15 Sekunden gesendet.  

ADM-Agent nimmt normalerweise 0,1 % des Systemspeichers und 0,1 % der CPU.


## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
In den folgenden Abschnitten unterstützten Betriebssysteme Abhängigkeit Agent aufgelistet.   32-Bit-Architekturen werden für jedes Betriebssystem nicht unterstützt.

### <a name="windows-server"></a>WindowsServer
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-Desktop
- Hinweis: Windows 10 wird noch nicht unterstützt
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux CentOS Linux und Oracle Linux (RHEL-Kernel)
- Nur Standard und SMP-Linux Kernel-Versionen werden unterstützt.
- Nicht-standard-Kernel frei wie PAE und Xen, nicht für andere Linux-Distributionen unterstützt. Ein System mit der Version "2.6.16.21-0.8-xen" wird beispielsweise nicht unterstützt.
- Benutzerdefinierte Kernel einschließlich Kompilierungen standard Kernel werden nicht unterstützt.
- CentOS Plus Kernel wird nicht unterstützt.
- Oracle unverwüstliche Kernel (UEK) wird in einem anderen Abschnitt behandelt.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Version des Betriebssystems | Kernel-Version |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Version des Betriebssystems | Kernel-Version |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Version des Betriebssystems | Kernel-Version |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux mit unverwüstliche Kernel (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Version des Betriebssystems | Kernel-Version
|:--|:--|
| 6.2 | Oracle-2.6.32-300 (UEK R1) |
| 6.3 | Oracle-2.6.39-200 (UEK R2) |
| 6.4 | Oracle-2.6.39-400 (UEK R2) |
| 6.5 | Oracle-2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle-2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Version des Betriebssystems | Kernel-Version
|:--|:--|
| 5.8 | Oracle-2.6.32-300 (UEK R1) |
| 5.9 | Oracle-2.6.39-300 (UEK R2) |
| 5.10 | Oracle-2.6.39-400 (UEK R2) |
| 5.11 | Oracle-2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Version des Betriebssystems | Kernel-Version
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Version des Betriebssystems | Kernel-Version
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnose-und Nutzungsdaten
Microsoft sammelt automatisch Verwendung und Leistung durch die Verwendung der Anwendung Abhängigkeit Überwachungsdienst. Microsoft verwendet diese Daten zur Verfügung und verbessert die Qualität und Integrität der Anwendung Abhängigkeit Überwachungsdienst. Daten enthält Informationen über die Konfiguration des Betriebssystems und Version sowie IP-Adresse, DNS-Namen und Name der Arbeitsstation um genaue und effiziente Fehlerbehebung bereitstellen. Wir sammeln keine Namen, Adressen oder andere Kontaktinformationen.

Weitere Informationen über die Datensammlung und Verwendung finden Sie unter der [Microsoft Online Services-Datenschutzrichtlinie](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie einmal [Anwendung Abhängigkeitsmonitor verwenden](operations-management-suite-application-dependency-monitor.md) sie bereitgestellt und konfiguriert wurde.
