<properties 
    pageTitle="Splitter Karte Probleme mit Recovery Manager | Microsoft Azure" 
    description="Verwenden Sie RecoveryManager-Klasse, um Probleme mit Splitter" 
    services="sql-database" 
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/05/2016" 
    ms.author="ddove"/>

# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>Splitter Karte Probleme mithilfe der RecoveryManager-Klasse

[RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) -Klasse ermöglicht ADO.NET-Anwendungen die leichter erkennen und Beheben von Inkonsistenzen zwischen der globalen shardzuordnung (GSM) und die lokalen shardzuordnung (LSM) Sharding Datenbank-Umgebung. 

GSM und LSM nachverfolgen, die Zuordnung der einzelnen Datenbanken in einer Sharding Umgebung. Gelegentlich tritt eine Unterbrechung zwischen GSM und der LSM. In diesem Fall verwenden Sie die RecoveryManager-Klasse erkennen und reparieren den Umbruch.

RecoveryManager-Klasse ist Teil der [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md). 


![Shardzuordnung][1]


Begriffsdefinitionen finden Sie unter [elastische Datenbank Tools Glossar](sql-database-elastic-scale-glossary.md). Wie **ShardMapManager** zur Verwaltung von Daten in eine Sharding Lösung finden Sie unter [Splitter Management zuordnen](sql-database-elastic-scale-shard-map-management.md).


## <a name="why-use-the-recovery-manager"></a>Wozu dient den Recovery Manager?

In einer datenbankumgebung Sharding gibt einen Mandanten pro Datenbank und viele Datenbanken pro Server. Es können auch viele Server in der Umgebung. Jede Datenbank wird Map Splitter zugeordnet, damit Anrufe an den richtigen Server und die Datenbank weitergeleitet werden können. Datenbanken nach einem **shardingschlüssel**nachverfolgt werden, und jeder Splitter erhält einen **Bereich von Schlüsselwerten**. Beispielsweise kann ein shardingschlüssel die Namen von "D" bis "F" dargestellt Die Zuordnung aller Splitter (aka Datenbanken) und deren Zuordnung sind in der **globalen shardzuordnung (GSM)**enthalten. Jede Datenbank enthält auch eine Übersicht der Splitter enthaltenen Bereiche – Dies wird als **lokale Splitter (LSM) zuordnen**. Wenn eine Anwendung einen herstellt, wird die Zuordnung mit der app für den schnellen Abruf zwischengespeichert. Die LSM zum zwischengespeicherte Daten überprüfen. 

GSM und LSM können aus den folgenden Gründen nicht synchronisiert werden:

1. Das Löschen einen Splitter, dessen Bereich vermutlich mehr oder einen umbenennen. Ein Splitter führt eine **verwaiste Splitter Zuordnung**löschen. Ähnlich, kann eine umbenannte Datenbank eine verwaiste Splitter-Zuordnung. Je nach der Absicht der Änderung der Splitter entfernt werden müssen oder Splitter Speicherort aktualisiert werden muss. Zum Wiederherstellen einer gelöschten Datenbank finden Sie unter [Wiederherstellen einer Datenbank zu einem früheren Zeitpunkt Wiederherstellen einer gelöschten oder Wiederherstellen eines Rechenzentrums hat](sql-database-troubleshoot-backup-and-restore.md).
2. Geo-Failover-Ereignisses. Dazu muss eine den Servernamen und die Namen der shardzuordnungs-Manager in der Anwendung zu aktualisieren und Splitter Mappingdetails für alle Splitter Splitter Zuordnung aktualisieren. Bei einem Failover Geo sollten solche Wiederherstellungslogik Failover-Workflow automatisiert werden. Automatisieren von Wiederherstellungsmaßnahmen ermöglicht eine reibungslose Verwaltung Geo-fähigen Datenbanken und vermeidet manuelle Benutzeraktionen.
3. Einen oder die ShardMapManager-Datenbank wird eine zuvor Punkt in Zeit wiederhergestellt.

Weitere Informationen zu Azure SQL-Datenbank elastische Datenbanktools Geo-Replikation und Wiederherstellung finden Sie Folgendes: 

* [Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md) 
* [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md)  
* [ShardMap Verwaltung](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Abrufen von RecoveryManager aus einem ShardMapManager 

Der erste Schritt ist eine RecoveryManager-Instanz erstellt. Die [GetRecoveryManager-Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) gibt Recovery Manager für die aktuelle [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) -Instanz zurück. Um Inkonsistenzen in der shardzuordnung zu beheben, muss der RecoveryManager für bestimmte shardzuordnung abgerufen werden. 

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 

In diesem Beispiel wird der RecoveryManager aus der ShardMapManager initialisiert. Mit einem ShardMap ShardMapManager ist bereits initialisiert. 

Da diese Anwendungscode Splitter Karte bearbeitet, sollte die Anmeldeinformationen in der Factorymethode (im obigen Beispiel SmmConnectionString) Anmeldeinformationen der Schreib Berechtigungen auf der GSM-Datenbank die Verbindungszeichenfolge verweist. Diese Anmeldeinformationen unterscheiden sich normalerweise von Anmeldeinformationen zum Öffnen von Verbindungen für datenabhängiges routing verwendet. Weitere Informationen finden Sie unter [Using Anmeldeinformationen im Datenbankclient elastische](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Die ShardMap entfernen einen gelöschten einen

[DetachShard-Methode](https://msdn.microsoft.com/library/azure/dn842083.aspx) trennt angegebenen Splitter aus der shardzuordnung und löscht verknüpft mit der Zuordnung.  

* Der Pfadparameter befindet Splitter, insbesondere Servernamen und Datenbanknamen der Splitter abgetrennt werden. 
* Der Parameter ShardMapName ist Zuordnungsname Splitter. Dies ist nur erforderlich, wenn mehrere Splitter wird von der gleichen shardzuordnungs-Manager verwaltet werden. Optional. 

**Wichtig**: Verwenden Sie dieses Verfahren nur, wenn Sie sicher, dass der Bereich für sind die aktualisierte Zuordnung leer ist. Die oben genannten Methoden aktivieren Daten für den Bereich verschoben wird, Sie auf Schecks im Code enthalten ist.

In diesem Beispiel wird der Splitter Karte Splitter entfernt. 

    rm.DetachShard(s.Location, customerMap); 

Die Zuordnung der Splitter Position im GSM vor dem Löschen der Splitter. Da der Splitter gelöscht wurde, wird davon ausgegangen, dies war beabsichtigt und Bereich Sharding wird nicht mehr verwendet. Wenn dies nicht der Fall ist, können Sie Point-in-Time-Wiederherstellung ausführen. den Splitter aus einem früheren Point-in-Time wiederherstellen. (In diesem Fall überprüfen Sie Abschnitt Splitter Inkonsistenzen zu erkennen.) Zum Wiederherstellen [eine Datenbank zu einem früheren Zeitpunkt wiederherstellen, eine gelöschte Datenbank wiederherstellen oder Wiederherstellen eines Rechenzentrums hat](sql-database-troubleshoot-backup-and-restore.md)angezeigt.

Da davon, dass das Datenbank löschen beabsichtigt war ausgegangen wird, wird der abschließende administrative Bereinigung Splitter in der shardzuordnungs-Manager den Eintrag löschen. Dies verhindert, dass die Anwendung versehentlich Informationen in einen Bereich nicht erwartungsgemäß geschrieben.

## <a name="to-detect-mapping-differences"></a>Zuordnung Unterschiede 

[DetectMappingDifferences-Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) wählt einen Splitter-Karten (lokal oder global) gibt als Quelle der Wahrheit und Mappings auf beiden Karten Splitter (GSM und LSM) gleicht.

    rm.DetectMappingDifferences(location, shardMapName);

* Der *Speicherort* gibt den Servernamen und den Datenbanknamen. 
* Der Parameter *ShardMapName* ist Zuordnungsname Splitter. Dies ist nur erforderlich, wenn mehrere Splitter wird von der gleichen shardzuordnungs-Manager verwaltet werden. Optional. 

## <a name="to-resolve-mapping-differences"></a>Zuordnung Unterschiede auflösen

Die [ResolveMappingDifferences-Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) wählt einen Splitter-Karten (lokale oder globale) als Quelle der Wahrheit und Mappings auf beiden Karten Splitter (GSM und LSM) gleicht.

    ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   
* Der Parameter *RecoveryToken* Listet die Unterschiede in der Zuordnung GSM und LSM für bestimmte Splitter. 

* [MappingDifferenceResolution-Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) wird an die Methode zum Auflösen von den Unterschied zwischen der Splitter Mappings verwendet. 
* **MappingDifferenceResolution.KeepShardMapping** wird empfohlen, wenn die LSM enthält die genaue Zuordnung und daher die Zuordnung in der Splitter verwendet werden sollte. Dies ist normalerweise der Fall bei einem Failover: der Splitter befindet sich jetzt auf einem neuen Server. Der Splitter zunächst von GSM (RecoveryManager.DetachShard-Methode) entfernt werden muss, ist eine Zuordnung nicht mehr auf GSM vorhanden. Daher der Splitter Zuordnung erneut herstellen muss die LSM verwendet werden.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Die ShardMap nach der Wiederherstellung einen zuordnen Sie einen 

Die [AttachShard-Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) fügt angegebenen Splitter Splitter-Karte. Dann erkennt Inkonsistenzen Karte Splitter und Mappings Splitter an der Splitter Wiederherstellung entsprechend aktualisiert. Angenommen die Datenbank ebenfalls umbenannt, entsprechend dem ursprünglichen Dateinamen (vor Wenn der Splitter wiederhergestellt wurde), seit Punkt Zeit Wiederherstellung standardmäßig eine neue Datenbank mit dem Zeitstempel angehängt. 

    rm.AttachShard(location, shardMapName) 

* *Der Pfadparameter* ist den Servernamen und den Datenbanknamen der Splitter angefügt wird. 

* Der Parameter *ShardMapName* ist Zuordnungsname Splitter. Dies ist nur erforderlich, wenn mehrere Splitter wird von der gleichen shardzuordnungs-Manager verwaltet werden. Optional. 

Dieses Beispiel fügt einen Splitter-Karte, die zuletzt aus zuvor Punkt in Zeit wiederhergestellt wurde. Da Splitter (nämlich die Zuordnung für den Splitter in der LSM) wiederhergestellt wurde, ist es möglicherweise inkonsistent mit der Splitter GSM. Außerhalb dieser Beispielcode wurde der Splitter wiederhergestellt und in den ursprünglichen Namen der Datenbank umbenannt. Da es wiederhergestellt wurde, davon ist die Zuordnung der LSM vertrauenswürdigen Zuordnung. 

    rm.AttachShard(s.Location, customerMap); 
    var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Splitter Speicherorte nach einem Geo-Failover (Wiederherstellen) der Splitter aktualisieren

Bei einem Failover Geo sekundäre Datenbank schreiben verfügbar gemacht und wird der neuen primären Datenbank. Der Name des Servers und der Datenbank (je nach Konfiguration) möglicherweise abweichen vom ursprünglichen primären. Daher müssen die Zuordnungseinträge Splitter GSM und LSM behoben werden. Wenn die Datenbank auf einen anderen Namen oder Speicherort oder in einem früheren Zustand wiederhergestellt wird, kann dies ebenso Inkonsistenzen in Splitter Karten führen. Der Shardzuordnungs-Manager behandelt die Verteilung der geöffneten Verbindungen mit der richtigen Datenbank. Verteilung basiert auf den Daten in der shardzuordnung und der Wert der shardingschlüssel, die das Ziel der Anwendung Anforderung. Nach einem Failover Geo muss diese Informationen präzise Servername, Datenbankname und Splitter Zuordnung der wiederhergestellten Datenbank aktualisiert werden. 

## <a name="best-practices"></a>Bewährte Methoden

Geo-Failover und Recovery sind Vorgänge, die normalerweise von einem Administrator Cloud absichtlich mit einer Azure SQL-Datenbanken Business Continuity-Funktionen Anwendung verwaltet. Business Continuity-Planung erfordert Prozesse, Verfahren und Maßnahmen, damit die geschäftlichen Abläufe ohne Unterbrechung fortgesetzt werden können. Die verfügbaren Methoden basierend als Teil der RecoveryManager-Klasse in diesem Workflow verwendet werden soll, um sicherzustellen, dass GSM und LSM aktuell bleiben auf Recovery-Maßnahmen. Es gibt 5 grundlegende Schritte dazu ordnungsgemäß GSM und LSM Informationen nach einem Failover wider. Der Anwendungscode auszuführende Schritte kann vorhandenen Tools und Workflow integriert. 

1. Abrufen der RecoveryManager aus der ShardMapManager. 
2. Alten Splitter aus der shardzuordnung zu trennen.
3. Shardzuordnung, einschließlich der neuen Splitter zuordnen Sie neue Splitter.
4. Erkennen Sie Inkonsistenzen in der Zuordnung zwischen den GSM und LSM. 
5. Beheben Sie Unterschiede zwischen GSM und LSM vertrauen der LSM. 

In diesem Beispiel führt die folgenden Schritte aus:
1. Der Splitter Positionen vor dem Failover an Shardzuordnung entfernt Splitter.
2. Fügt Splitter an Shardzuordnung reflektieren die neuen Splitter Speicherorte (der Parameter "Configuration.SecondaryServer" ist der gleichen Datenbank Name den neuen Namen).
3. Die Recovery-Token durch Zuordnung Unterschiede zwischen GSM und LSM für jede Splitter abgerufen. 
4. Löst die Inkonsistenzen die Zuordnung LSM jeder Splitter vertrauen. 

    Var Splitter = Smm. GetShards();  Foreach (Splitter s Splitter) {Wenn (s.Location.Server == Configuration.PrimaryServer) {ShardLocation SlNew = neue ShardLocation (Configuration.SecondaryServer, s.Location.Database) 
        
          rm.DetachShard(s.Location); 
        
          rm.AttachShard(slNew); 
        
          var gs = rm.DetectMappingDifferences(slNew); 
    
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 



[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png
 
