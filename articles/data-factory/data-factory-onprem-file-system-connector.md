<properties
    pageTitle="Verschieben von Daten in und aus einem Dateisystem | Microsoft Azure"
    description="Erfahren Sie, wie Daten in und aus einem lokalen Dateisystem mit Azure Data Factory."
    services="data-factory"
    documentationCenter=""
    authors="linda33wj"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Verschieben von Daten in und aus einem lokalen Dateisystem mit Azure Data Factory

Dieser Artikel beschreibt eine mögliche Verwendung von Azure Data Factory Kopieraktivität zum Verschieben von Daten in und aus einem lokalen Dateisystem. Finden Sie eine Liste von Datenspeichern, die als Quellen oder senken mit dem lokalen Dateisystem verwendet [unterstützten Datenquellen und Datensenken](data-factory-data-movement-activities.md#supported-data-stores) . Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Data Factory unterstützt die Verbindung mit einem lokalen Dateisystem über Data Management Gateway. Data Management Gateway und eine schrittweise Anleitung zum Einrichten von Gateway finden Sie unter [Verschieben von Daten zwischen lokalen und Cloud mit Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).

> [AZURE.NOTE]
> Neben Daten Management Gateway müssen keine binären Dateien sowie von einem lokalen Dateisystem installiert werden.
>
> Tipps zur Behebung von Problemen/Gateway verbindungsbezogenen finden Sie unter [Problembehandlung bei Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) .

## <a name="linux-file-share"></a>Linux-Dateifreigabe

Führen Sie die folgenden beiden Schritte um eine Linux-Dateifreigabe mit der Datei verknüpfte Serverdienst verwenden:

- Installieren Sie [Samba](https://www.samba.org/) auf dem Linux-Server.
- Installieren und Konfigurieren von Data Management Gateway auf einem WindowsServer. Installieren von Data Management Gateway auf einem Linux-Server wird nicht unterstützt.

## <a name="copy-wizard"></a>Assistent zum Kopieren von
Am einfachsten erstellen Sie eine Pipeline, die Daten zu und von einem lokalen Dateisystem kopiert werden Projektkopier-Assistenten verwenden. Eine kurze exemplarische Vorgehensweise finden Sie unter [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md).

Die folgenden Beispiele bieten Beispiel JSON-Definitionen, mit denen Sie eine Pipeline mithilfe der [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)und [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Sie zeigen, wie Daten zwischen einem lokalen Dateisystem und Azure BLOB-Speicher kopieren. Allerdings können Sie Daten *direkt* aus allen Quellen zu senken in [unterstützten Datenquellen und Datensenken](data-factory-data-movement-activities.md#supported-data-stores) mithilfe Kopieraktivität in Azure Data Factory aufgeführten kopieren.


## <a name="sample-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Beispiel: Daten aus einem lokalen Dateisystem in Azure BLOB-Speicher

Dieses Beispiel veranschaulicht, wie Daten aus einem lokalen Dateisystem in Azure BLOB-Speicher kopieren.

Das Beispiel verfügt über die folgenden Data Factory Entitäten:

- Eine verknüpfte Dienst vom Typ [OnPremisesFileServer](data-factory-onprem-file-system-connector.md#onpremisesfileserver-linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Dateifreigabe](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties).
- Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Die [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im folgende Beispiel Kopiervorgang Zeitreihen von einem lokalen Dateisystem Azure Blob-Speicher pro Stunde. In diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten nach Beispiele beschrieben.

Als ersten Schritt richten Sie gemäß der Anleitung im [Verschieben von Daten zwischen lokalen und Cloud mit Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md)Data Management Gateway.

**Lokale Datei verknüpften Serverdienst:**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

Wir empfehlen die **EncryptedCredential** -Eigenschaft stattdessen Eigenschaften **Userid** und **Password** . Details zu diesem Dienst verknüpften finden Sie unter [File Server Service verknüpft](#onpremisesfileserver-linked-service-properties) .

**Azure verknüpfter Dienst:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Lokale Datei Eingabedatasets System:**

Daten werden eine neue Datei stündlich übernommen. Ordnerpfad und Dateiname Eigenschaften werden anhand der Startzeit des Segments.  

Festlegen von `"external": "true"` Data Factory teilt mit, dass das Dataset Daten Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.

    {
      "name": "OnpremisesFileSystemInput",
      "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
          "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ]
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Azure BLOB-Speicher Ausgabe Dataset:**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Stunde Teile der Startzeit.

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Kopieren Sie Projektvorgang:**

Die Pipeline enthält eine kopieraktivität, die konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In der Pipeline JSON-Definition **Geben** soll **FileSystemSource**und **Senke** Typ auf **BlobSink**festgelegt ist.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T19:00:00",
        "description":"Pipeline for copy activity",
        "activities":[  
          {
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "OnpremisesFileSystemInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "FileSystemSource"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

## <a name="sample-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Beispiel: Daten von Azure SQL-Datenbank auf einem lokalen Dateisystem

Das folgende Beispiel zeigt:

- Eine verknüpfte Dienst vom Typ AzureSqlDatabase.
- Eine verknüpfte Dienst vom Typ OnPremisesFileServer.
- Eingabedatasets vom Typ AzureSqlTable.
- Ein ausgabedataset vom Typ Dateifreigabe.
- Eine Rohrleitung mit einer Kopie, die SQLSource aus und FileSystemSink verwendet.

Das Beispiel kopiert Zeitreihen-Daten aus einer SQL Azure Tabelle einem lokalen Dateisystem stündlich. Nach den Beispielen sind JSON-Eigenschaften, die in diesen Beispielen verwendet werden in Abschnitten beschrieben.

**SQL Azure verknüpft Service:**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Lokale Datei verknüpften Serverdienst:**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

Wir empfehlen die **EncryptedCredential** -Eigenschaft anstelle der Eigenschaften **Userid** und **Password** . Details zu diesem Dienst verknüpften finden Sie unter [File System Service verknüpft](#onpremisesfileserver-linked-service-properties) .

**Eingabedatasets für SQL Azure:**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in SQL Azure und enthält eine Spalte namens "Timestampcolumn" für Zeitreihen-Daten.

Festlegen von ``“external”: ”true”`` Data Factory teilt mit, dass das Dataset Daten Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Lokale Datei System ausgabedataset:**

Daten werden stündlich eine neue Datei kopiert. Der Ordnerpfad und der Dateiname für das Blob werden anhand der Startzeit des Segments.

    {
      "name": "OnpremisesFileSystemOutput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
          "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ]
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Pipeline mit einer Kopie:**

Die Pipeline enthält eine kopieraktivität, die konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In der Pipeline JSON-Definition **Geben** soll **SQLSource aus**und welche **Senke** auf **FileSystemSink**festgelegt ist. Die SQL-Abfrage, die für die **SqlReaderQuery** -Eigenschaft angegeben wählt die Daten in der letzten Stunde kopieren.


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T20:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "OnpremisesFileSystemOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "FileSystemSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 3,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

## <a name="on-premises-file-server-linked-service-properties"></a>Lokalen Dateiserver verknüpften Eigenschaften

Sie können eine lokalen Dateisystem mit einer Azure Data Factory auf lokalen Dateiserver verknüpft Service verknüpfen. Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die der Dienst auf lokalen Dateiserver verknüpft sind.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Sicherstellen Sie, dass die Type-Eigenschaft auf **OnPremisesFileServer**festgelegt ist. | Ja
Host | Gibt den Stammpfad des Ordners, den Sie kopieren möchten. Verwenden Sie das Escapezeichen ' \ ' für spezielle Zeichen in der Zeichenfolge. Beispiele finden Sie unter [Beispiel Service und Dataset-Definitionen verknüpft](#sample-linked-service-and-dataset-definitions) . | Ja
Benutzer-ID | Geben Sie die ID des Benutzers, der auf den Server zugreifen. | Nein (EncryptedCredential auswählen)
Kennwort | Geben Sie das Kennwort für den Benutzer (Benutzer-ID). | Nein (EncryptedCredential auswählen
encryptedCredential | Geben Sie die verschlüsselten Anmeldeinformationen mit New-AzureRmDataFactoryEncryptValue-Cmdlet abrufen können. | Nein (möchten Sie Benutzer-ID und das Kennwort unverschlüsselt angeben)
gatewayName | Gibt den Namen des Gateways, das für die Verbindung mit dem lokalen Dateiserver Data Factory verwenden soll. | Ja

Informationen zum Festlegen von Anmeldeinformationen für eine lokale Dateisystem-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="sample-linked-service-and-dataset-definitions"></a>Beispiel für verknüpfte Dienst und Dataset-Definitionen
Szenario | Verknüpfte Dienstdefinition hosten | FolderPath im Dataset-definition
-------- | --------------------------------- | --------------------- |
Lokaler Ordner auf Daten Management Gateway-Computer: <br/><br/>Beispiele: D:\\ \* oder D:\folder\subfolder\\* | D:\\ \\ (für Data Management Gateway 2.0 und höher) <br/><br/> Localhost (für frühere Versionen als Data Management Gateway 2.0) | .\\\\ oder Ordner\\\\Unterordner (Data Management Gateway 2.0 und höher) <br/><br/>D:\\ \\ oder d\\\\Ordner\\\\Unterordner (Gateway-Version unter 2.0)
Freigegebenen Remoteordner: <br/><br/>Beispiele: \\ \\Myserver\\teilen\\ \* oder \\ \\Myserver\\teilen\\Ordner\\Unterordner\\* | \\\\\\\\MeinServer\\\\freigeben | .\\\\ oder Ordner\\\\Unterordner


**Die Gateway-Version zu finden:**

1. Starten Sie [Data Management Gateway Configuration Manager](data-factory-data-management-gateway.md#data-management-gateway-configuration-manager) auf dem Computer.
2. Wechseln Sie zur Registerkarte **Hilfe** .

> [AZURE.NOTE] Es wird empfohlen, Sie [aktualisieren Ihr Gateway Data Management Gateway 2.0 oder höher](data-factory-data-management-gateway.md#update-data-management-gateway) auf die neuesten Features und Updates nutzen.

**Beispiel: Benutzernamen und Kennwörter im nur-Text verwenden**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

**Beispiel: Verwenden von encryptedcredential**

    {
      "Name": " OnPremisesFileServerLinkedService ",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "D:\\",
          "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
          "gatewayName": "mygateway"
        }
      }
    }

## <a name="on-premises-file-system-dataset-type-properties"></a>Lokale Datei System Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und zum Definieren von Datasets verfügbaren Eigenschaften finden Sie unter [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen.

Abschnitt TypeProperties unterscheidet sich für jeden Datensatz. Erläutert, wie der Speicherort und das Format der Daten in den Datenspeicher. Abschnitt TypeProperties für das Dataset Typ **Dateifreigabe** hat folgende Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
folderPath | Gibt die untergeordneten Ordner. Verwenden Sie das Escapezeichen ' \' für spezielle Zeichen in der Zeichenfolge. Beispiele finden Sie unter [Beispiel Service und Dataset-Definitionen verknüpft](#sample-linked-service-and-dataset-definitions) .<br/><br/>Kombinieren diese Eigenschaft mit **PartitionBy** basierend auf Pfade zu Start/Ende Datum / Uhrzeit. | Ja
Dateiname | Geben Sie den Namen der Datei in **FolderPath** soll die Tabelle auf eine bestimmte Datei im Ordner. Wenn Sie einen Wert für diese Eigenschaft nicht angeben, zeigt die Tabelle für alle Dateien im Ordner.<br/><br/>Dateiname für ausgabedataset nicht angegeben ist, wird der Name der generierten Datei im folgenden Format: <br/><br/>`Data.<Guid>.txt`(Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) | Nein
partitionedBy | PartitionedBy können dynamische FolderPath/Dateiname für Zeitreihen-Daten. Ein Beispiel ist FolderPath Stunde Daten parametrisiert. | Nein
Format | Folgende Format unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**und **ParquetFormat**. **Typeigenschaft unter Format** auf einen der folgenden Werte festgelegt. Einzelheiten finden Sie unter [TextFormat angeben](#specifying-textformat), [Angabe AvroFormat](#specifying-avroformat) [JsonFormat angeben](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [ParquetFormat angeben](#specifying-parquetformat) . Wenn Sie Dateien kopieren möchten-wird zwischen dateibasierte Speicher (binären Kopie) überspringen Formatabschnitt sowohl Eingabe- und Dataset-Definitionen. | Nein
fileFilter | Gibt einen Filter an eine Teilmenge statt alle Dateien in den Ordnerpfad verwendet werden. <br/><br/>Zulässige Werte: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiel 1: "FileFilter": "* .log"<br/>Beispiel 2: "FileFilter": - 1 - 2014. TXT"<br/><br/>Hinweis: Diese FileFilter für Dateifreigabe Eingabedatasets. | Nein
| Komprimierung | Geben Sie Art und Grad der Komprimierung der Daten. Unterstützte Typen sind **GZip**und **Deflate** **BZip2**. Unterstützte sind **Optimal** und **schnell**. Derzeit werden Einstellungen für Daten in **AvroFormat** oder **OrcFormat**nicht unterstützt. Weitere Informationen finden Sie im Abschnitt [Komprimierung unterstützt](#compression-support) . | Nein |


> [AZURE.NOTE] Dateiname und FileFilter können nicht gleichzeitig verwendet werden.

### <a name="using-partitionedby-property"></a>Mithilfe der PartitionedBy-Eigenschaft

Wie im vorherigen Abschnitt erwähnt, können Sie eine dynamische Ordnerpfad und Dateiname für Zeitreihen-Daten mit PartitionedBy angeben. Sie können dazu mit Data Factory Makros und Systemvariablen, SliceStart und SliceEnd, die zum logischen Zeitraum einen bestimmten Datenslice anzugeben.

Weitere Detailinformationen Zeitreihen Datasets finden Sie unter Planung und Slices [Erstellen Datasets](data-factory-create-datasets.md), [Planung und Ausführung](data-factory-scheduling-and-execution.md)und [Pipelines erstellen](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Beispiel 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy":
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} mit dem Wert der Daten Factory Systemvariablen SliceStart im Format (YYYYMMDDHH) ersetzt. SliceStart verweist auf das Slice Startzeit. Der Ordnerpfad ist für jedes Segment. Beispiel: Wikisampledataout/Wikidatagateway/2014100103 oder Wikisampledataout/Wikidatagateway/2014100104.

#### <a name="sample-2"></a>Beispiel 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],

In diesem Beispiel werden Jahr, Monat, Tag und Zeitpunkt der SliceStart in separaten Variablen extrahiert, die die FolderPath und Dateinamen verwenden.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="file-share-copy-activity-type-properties"></a>Dateifreigabe kopieren Typeneigenschaften

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Rekursive | Gibt an, ob die Daten rekursiv aus dem Unterordner oder aus dem angegebenen Ordner schreibgeschützt ist. | True, False (Standard)| Nein |

**FileSystemSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | Definiert das Verhalten beim Kopieren wird die Quelle BlobSource oder Dateisystem. | **PreserveHierarchy:** Behält die Hierarchie im Zielordner. Also der relative Pfad der Quelldatei in den Quellordner identisch mit den relativen Pfad der Zieldatei in den Zielordner.<br/><br/>**FlattenHierarchy:** Alle Dateien aus dem Quellordner werden auf der ersten Ebene des Zielordners erstellt. Die Zieldateien sind mit einem automatisch generierten Namen erstellt.<br/><br/>**MergeFiles:** Führt alle Dateien aus dem Quellordner in eine Datei. Blob Namen Dateiname angegeben ist, ist der zusammengeführten Dateiname angegebenen Namen. Andernfalls ist es eine automatisch generierte Datei. | Nein |

### <a name="recursive-and-copybehavior-examples"></a>Beispiele für rekursive und copyBehavior
Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen der Werte für die rekursive und CopyBehavior.

Rekursive Wert | CopyBehavior Wert | Resultierendes Verhalten
--------- | ------------ | --------
true | preserveHierarchy | Für Quellordner Ordner1 mit der folgenden Struktur,<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>der Zielordner Ordner1 wird mit derselben Struktur wie die Quelle erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5  
true | flattenHierarchy | Für Quellordner Ordner1 mit der folgenden Struktur,<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Das Ziel wird Ordner1 mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für File5
true | mergeFiles | Für Quellordner Ordner1 mit der folgenden Struktur,<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Das Ziel wird Ordner1 mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1 + Datei2 + Datei3 + File4 + Datei 5 Inhalt in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt.
falsch | preserveHierarchy | Für Quellordner Ordner1 mit der folgenden Struktur,<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>der Zielordner Ordner1 wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Subfolder1 Datei3, File4 und File5 wird nicht übernommen.
falsch | flattenHierarchy | Für Quellordner Ordner1 mit der folgenden Struktur,<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>der Zielordner Ordner1 wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei2<br/><br/>Subfolder1 Datei3, File4 und File5 wird nicht übernommen.
falsch | mergeFiles | Für Quellordner Ordner1 mit der folgenden Struktur,<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>der Zielordner Ordner1 wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Eine Datei mit einem automatisch generierten Dateinamen werden Datei1 + Datei2 Inhalt zusammengeführt.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierte Namen Datei1<br/><br/>Subfolder1 Datei3, File4 und File5 wird nicht übernommen.


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
 Zu den wichtigsten Faktoren, Datentransfer (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren Leistungseinbußen, finden Sie unter [Kopieraktivität Performance und tuning Guide](data-factory-copy-activity-performance.md).
