<properties 
   pageTitle="Szenarien mit Datenspeicher See | Microsoft Azure" 
   description="Aufgenommen, verarbeitet, heruntergeladen und in einem Datenspeicher See dargestellte Szenarien und Tools verwenden, die Daten verstehen" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Verwenden von Azure See Datenspeicher für große Daten

Gibt es vier wichtigste Phasen in großen Datenverarbeitung:

* Einnahme von großen Datenmengen in einem Datenspeicher in Echtzeit oder in batches
* Verarbeitung der Daten
* Herunterladen der Daten
* Visualisierung der Daten

In diesem Artikel betrachten wir diese Phasen bei Azure See Datenspeicher Optionen und Tools für Ihre Bedürfnisse Datenmengen.

## <a name="ingest-data-into-data-lake-store"></a>Erfassung von Daten in den Datenspeicher See

Dieser Abschnitt beschreibt die unterschiedlichen Quellen von Daten und unterschiedlich, in denen Daten berücksichtigen Datenspeicher See aufgenommen werden können.

![Erfassung von Daten in dem Datenspeicher] (./media/data-lake-store-data-scenarios/ingest-data.png "Erfassung von Daten in dem Datenspeicher")

### <a name="ad-hoc-data"></a>Ad-hoc-Daten

Kleinere Datasets, die dies eine große Anwendung für Prototypen verwendet. Es gibt verschiedene technologische ad-hoc-Daten der Datenquelle.

| Datenquelle        | Nehmen sie mit                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Lokalen computer     | <ul> <li>[Azure-Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure plattformübergreifende CLI](data-lake-store-get-started-cli.md)</li> <li>[Mit den Daten See Tools für Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Azure-Speicher-Blob | <ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy-Werkzeug](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp auf HDInsight cluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Streamingdaten

Dies ist von verschiedenen Quellen wie Applikationen, Geräten, Sensoren generierte können. Diese Daten können von verschiedenen Tools in einem Datenspeicher See aufgenommen werden. Diese Tools werden normalerweise erfassen und verarbeiten die Daten vom Ereignis in Echtzeit, und Schreiben Sie dann die Ereignisse in Batches in See Datenspeicher, damit sie weiterverarbeitet werden können. 

Tools, mit denen Sie, folgen:
 
* [Azure Stream Analytics] (.. / Stream-Analytics-Daten-See-Ausgabe) - Ereignisse aufgenommen zu Ereignis geschrieben werden können Azure Data Lake Ausgabe Azure See Datenspeicher verwenden.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) - schreiben Daten direkt auf See Datenspeicher von Storm-Cluster.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) -Ereignisse von Ereignis empfangen und notieren sie See Datenspeicher mit [Daten See Speicher .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationale Daten

Sie können auch Datenquellen aus relationalen Datenbanken. Über einen Zeitraum hinweg sammeln relationale Datenbanken riesige Datenmengen wichtige Erkenntnisse können durch eine große Datenpipeline verarbeitet. Die folgenden Tools können Sie um solche Daten in Lake Datenspeicher zu verschieben.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Serverdaten (Upload über benutzerdefinierte Programme)

Dieses Dataset wird speziell bezeichnet da Analysis Serverdaten häufig mit großen Daten ist und große Mengen an Protokolldateien erfordert auf See Datenspeicher. Eines der folgenden Tools können Sie eigene Skripts oder Programme diese Daten schreiben.

* [Azure plattformübergreifende CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Datenspeicher See .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Für Serverdaten hochladen, und auch für andere Arten von Daten (z. B. sozialen Gefühle) hochladen ist ein guter Ansatz, weil Sie die Flexibilität, Ihre Daten hochladen Komponente als Teil der größeren Datenmengen Anwendung eine eigene benutzerdefinierten Skripts/Applikationen schreiben. In einigen Fällen kann dieser Code aus einem Skript oder einfache Befehlszeilenprogramm dauern. In anderen Fällen kann der Code große Datenverarbeitung in einer Business-Anwendung oder Lösung integriert verwendet werden.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Daten mit Azure HDInsight

Die meisten unterstützen HDInsight Cluster (Hadoop, HBase, Sturm) See Datenspeicher als ein Speicher-Repository. HDInsight-Cluster Zugriff auf Daten von Azure Storage Blobs (WASB). Zur Steigerung der Leistung können Sie die Daten von WASB bei den Cluster See Datenspeicher kopieren. Die folgenden Tools können Sie die Daten kopieren.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy Dienst](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Daten in lokalen oder IaaS Hadoop-Cluster

Große Datenmengen können in vorhandene Hadoop Cluster lokal auf Computern mit bietet gespeichert. Hadoop Cluster zur lokalen Bereitstellung möglicherweise oder möglicherweise innerhalb eines Clusters IaaS Azure. Vorschriften dieser Daten in Azure See Datenspeicher für einen einmaligen Ansatz oder wiederkehrende kopieren kann. Es gibt verschiedene Optionen zu. Nachfolgend eine Liste der alternativen und die zugehörigen Kompromisse.

| Ansatz  | Details | Vorteile   | Hinweise  |
|-----------|---------|--------------|-----------------|
| Kopiert Daten direkt aus Hadoop Cluster Azure See Datenspeicher verwenden Sie Azure Data Factory (ADF) | [ADF unterstützt bietet als Datenquelle](../data-factory/data-factory-hdfs-connector.md) | ADF unterstützt die vordefinierten bietet erstklassige End-to-End-Management und Überwachung | Erfordert Data Management Gateway lokal oder im IaaS-Cluster bereitgestellt werden |
| Exportieren von Daten aus Hadoop als Dateien. Kopieren Sie die Dateien in Azure See Datenspeicher mit entsprechenden Mechanismus.                                   | Sie können Dateien mit Azure See Datenspeicher: <ul><li>[Azure PowerShell für Windows-Betriebssystem](data-lake-store-get-started-powershell.md)</li><li>[Azure plattformübergreifende CLI für Windows OS](data-lake-store-get-started-cli.md)</li><li>Benutzerdefinierte Anwendung mit alle Daten See Speicher SDK</li></ul> | Schnellen Einstieg. Kann benutzerdefinierte uploads                                                   | Mehrstufigen Prozess, bei dem mehrere Technologies. Management und Überwachung wächst mit der Zeit angesichts der benutzerdefinierten Tools eine Herausforderung sein |
| Verwenden Sie Distcp, um Daten aus Hadoop in Azure-Speicher kopieren. Dann Daten Sie aus dem Azure-Speicher auf See Datenspeicher geeigneten Mechanismus. | Sie können Daten aus dem Azure-Speicher mit See Datenspeicher: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy-Werkzeug](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp auf HDInsight-Cluster](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Sie können Open-Source-Tools verwenden. | Mehrstufigen Prozess, bei dem mehrere technologies |

### <a name="really-large-datasets"></a>Sehr große datasets

Für das Hochladen von Datasets in mehreren Terabyte liegen, kann beschriebenen Methoden manchmal langsam und teuer sein. In solchen Fällen können Sie die folgenden Optionen.

* **Azure ExpressRoute verwenden**. Azure ExpressRoute können Sie private Verbindung zwischen Azure Rechenzentren und der Infrastruktur vor Ort zu erstellen. Dies bietet eine zuverlässige Option für große Datenmengen übertragen. Weitere Informationen finden Sie in der [Dokumentation zu Azure ExpressRoute](../expressroute/expressroute-introduction.md).


* **"Offline" Hochladen von Daten**. Wenn mithilfe von Azure ExpressRoute aus irgendeinem Grund nicht möglich ist, können [Azure Import/Export-Dienst](../storage/storage-import-export-service.md) Sie Festplatten mit Daten aus dem Azure-Rechenzentrum zu liefern. Daten werden zuerst in Azure Storage Blobs hochgeladen. [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) oder [AdlCopy-Tool](data-lake-store-copy-data-azure-storage-blob.md) können dann um Daten aus Azure Storage Blobs See Datenspeicher zu kopieren.

    >[AZURE.NOTE] Mit dem Import/Export-Dienst die Dateigröße auf den Azure-Rechenzentrum Schiff Datenträgern nicht größer als 200 GB sein sollte.


## <a name="process-data-stored-in-data-lake-store"></a>Verarbeiten von Daten im Datenspeicher See

Sobald die Daten im Datenspeicher See verfügbar ist können Sie diese Daten mit den unterstützten big Data Analyse ausführen. Derzeit können Sie Azure HDInsight und Azure Data Lake Analytics Analysis Arbeitsplätze auf See Datenspeicher gespeicherten Daten ausführen verwenden. 

![Analyse-Daten im Datenspeicher See] (./media/data-lake-store-data-scenarios/analyze-data.png "Analyse-Daten im Datenspeicher See")

Betrachten Sie die folgenden Beispiele.

* [Erstellen Sie einen HDInsight-Cluster mit See Datenspeicher Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Datenspeicher See Daten herunterladen

Sie sollten auch herunterladen oder Daten von Azure See Datenspeicher für Szenarien wie:

* Verschieben Sie Daten in anderen Repositories mit vorhandenen Datenverarbeitung Pipelines. Sie möchten z. B. Daten aus See Datenspeicher in Azure SQL-Datenbank oder lokalen SQL Server verschieben.
* Herunterladen Sie Daten auf dem lokalen Computer bei IDE-Umgebung beim Erstellen von Anwendungsprototypen

![Ausgang Daten aus dem Datenspeicher] (./media/data-lake-store-data-scenarios/egress-data.png "Ausgang Daten aus dem Datenspeicher")

In solchen Fällen können Sie eine der folgenden Optionen:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Die folgenden Methoden können Sie eigene Skripts/Download von Daten aus See Datenspeicher schreiben.

* [Azure plattformübergreifende CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Datenspeicher See .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Daten Sie im Datenspeicher See

Aus Diensten können Daten im Datenspeicher See optisch erstellt.

![Visualisierung Daten im Datenspeicher See] (./media/data-lake-store-data-scenarios/visualize-data.png "Visualisierung Daten im Datenspeicher See")

* Sie können mithilfe von [Azure Data Factory zum Verschieben von Daten aus dem Datenspeicher in Azure SQL Data Warehouse](../data-factory/data-factory-data-movement-activities.md#supported-data-stores) starten
* Danach können Sie erstellen visuelle Darstellung der Daten [integriert Power BI Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) .
