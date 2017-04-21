<properties
   pageTitle="Anwendung aktualisieren: Datenserialisierung | Microsoft Azure"
   description="Best Practices für Datenserialisierung und wie sie parallele Upgrades der Anwendung."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>


# <a name="how-data-serialization-affects-an-application-upgrade"></a>Einfluss der Datenserialisierung anwendungsaktualisierung

Bei einer [anwendungsaktualisierung](service-fabric-application-upgrade.md)wird die Aktualisierung auf eine Teilmenge der Knoten eine Aktualisierungsdomäne gleichzeitig angewendet. Dabei werden einige Domänen Upgrade auf die neuere Version der Anwendung und einige Upgrade Domänen werden auf die ältere Version der Anwendung. Während der Installation die neue Version der Anwendung muss die alte Version Ihrer Daten lesen und muss die alte Version der Anwendung die neue Version Ihrer Daten lesen. Wenn das Format nicht vorwärts und rückwärts kompatibel ist, die Aktualisierung fehlschlagen oder schlimmer noch, Daten verloren oder beschädigt. Dieser Artikel beschreibt, was ist das Datenformat und bietet bewährte dafür, dass Ihre Daten vorwärts und rückwärts kompatibel.


## <a name="what-makes-up-your-data-format"></a>Was macht das Datenformat?

Die C#-Klassen in Azure Service Fabric entnommen gespeichert und repliziert Daten. Anträge, die [Zuverlässige Sammlungen](service-fabric-reliable-services-reliable-collections.md)verwenden, ist für die Objekte zuverlässige Wörterbücher und Warteschlangen. Für Applikationen, die [Zuverlässige](service-fabric-reliable-actors-introduction.md)Akteure steht, die Unterstützung für den Akteur. Dieser C#-Klassen muss serialisierbar gespeichert und repliziert werden. Daher wird durch die Felder und Eigenschaften, sowie serialisiert werden wie sie serialisiert, das Datenformat definiert. In einer `IReliableDictionary<int, MyClass>` eine serialisierte Daten `int` und einem serialisierten `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Code wird das Ergebnis in einer Formatänderung Daten

Da das Format von C#-Klassen bestimmt ist, möglicherweise Änderungen an den Klassen eine Formatänderung Daten. Vorsicht ist geboten, um sicherzustellen, dass eine Aktualisierung der Formatänderung Daten verarbeiten kann. Beispiele, in denen Daten Formatänderungen verursachen:

- Hinzufügen oder Entfernen von Feldern oder Eigenschaften
- Umbenennen von Feldern oder Eigenschaften
- Die Typen der Felder oder Eigenschaften ändern
- Ändern den Klassennamen oder namespace

### <a name="data-contract-as-the-default-serializer"></a>Datenvertrag als Standard-Serialisierungsprogramm

Das Serialisierungsprogramm obliegt normalerweise die Daten gelesen und deserialisiert in der aktuellen Version, wenn die Daten in eine ältere oder *neuere* Version. Standard-Serialisierungsprogramm ist [Datenvertragsserialisierer](https://msdn.microsoft.com/library/ms733127.aspx), klar definierten Versionsregeln hat. Zuverlässige Sammlungen gestatten das Serialisierungsprogramm überschrieben werden, aber zuverlässige Akteure derzeit nicht. Das Serialisierungsprogramm Daten spielt eine wichtige Rolle bei parallelen Updates. Der Datenvertragsserialisierer ist das Serialisierungsprogramm für Service Fabric Applikationen empfohlen.


## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Wie wirkt sich auf das Format eines parallelen Updates

Während einer Aktualisierung im Wechsel sind zwei Hauptszenarien, das Serialisierungsprogramm eine ältere oder *neuere* Version Ihrer Daten auftreten:

1. Nachdem ein Knoten aktualisiert wird und wieder startet, lädt neue Serialisierungsprogramm beibehaltene Daten auf der Festplatte von der alten Version.
2. Während der Aktualisierung wird der Cluster aus der alten und neuen Versionen des Codes enthalten. Da Replikate in verschiedenen Domänen aktualisieren darf und Replikate Daten senden hat möglicherweise neue und/oder alte Version Ihrer Daten durch die neue und/oder alte Version des Serialisierungsprogramms.

> [AZURE.NOTE] "Neue Version" und "veraltet" finden Sie hier auf die Version des Codes, der ausgeführt wird. "Neue Serialisierungsprogramm" bezieht sich auf Serialisierungsprogramm Code, der in der neuen Version der Anwendung ausgeführt wird. "Neuen Daten" bezieht sich auf die serialisierten C#-Klasse aus der neuen Version der Anwendung.

Die beiden Versionen von Code und Daten Format muss vorwärts und rückwärts kompatibel. Wenn sie nicht kompatibel sind, kann die Aktualisierung fehlschlagen oder verloren. Die Aktualisierung fehlschlagen, weil Code oder Serialisierungsprogramm auslösen kann Ausnahmen oder einer Störung die Version stößt. Wenn beispielsweise eine neue Eigenschaft wurde hinzugefügt, aber alte Serialisierungsprogramm während der Deserialisierung verwirft verloren Daten.

Datenvertrag wird empfohlen, dass Ihre Daten kompatibel ist. Es wurde klar definierte Versionsregeln zum Hinzufügen, entfernen und Ändern der Felder. Es unterstützt auch unbekannte Felder, in die Serialisierung und Deserialisierung verknüpfen, und Umgang mit klassenvererbung. Weitere Informationen finden Sie unter [Datenvertrag verwenden](https://msdn.microsoft.com/library/ms733127.aspx).


## <a name="next-steps"></a>Nächste Schritte

[Umstellen der Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch ein Upgrade der Anwendung mit Visual Studio.

[Umstellen der Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine anwendungsaktualisierung mit PowerShell.

Steuern Sie, wie Ihre Anwendung aktualisiert [Aktualisieren Parameter](service-fabric-application-upgrade-parameters.md).

Informationen Sie zum erweiterten Funktionen beim Upgrade Ihrer Anwendung auf [Weiterführende Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von Problemen in Anwendungsupgrades auf die Schritte [Zur Problembehandlung Anwendungsupgrades ](service-fabric-application-upgrade-troubleshooting.md).
