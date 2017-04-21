<properties
    pageTitle="Azure-Speicher Tabelle Design Guide | Microsoft Azure"
    description="Design skalierbare und leistungsfähige Tabellen in Azure Table Storage"
    services="storage"
    documentationCenter="na"
    authors="jasonnewyork" 
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Azure-Speicher Tabelle Design Guide: Designing skalierbare und leistungsfähige Tabellen

## <a name="overview"></a>Übersicht

Skalierbares Design und leistungsfähigen Tabellen zu eine Reihe von Faktoren wie Leistung, skalierbar und Kosten berücksichtigen. Wenn Sie zuvor Schemas für relationale Datenbanken entworfen haben, diese Aspekte werden vertraut, aber einige Ähnlichkeit zwischen Azure Tabelle Service-Speichermodell und relationale Modelle gibt, gibt es auch zahlreiche wichtige Unterschiede. Diese Unterschiede führen in der Regel sehr unterschiedliche Designs, intuitiv oder falsche jemandem mit relationalen Datenbanken vertraut aussehen kann, aber die sinnvoll für einen NoSQL Schlüssel-Wert-Speicher wie der Azure-Tabelle entwerfen. Ihr Entwurf Unterschiede spiegeln die Tatsache, dass der Dienst soll Cloud-Skalierung unterstützen, der Milliarden von Entitäten (Zeilen in relationalen Datenbanksystemen) oder für Datasets, die sehr hohen Transaktionsvolumen unterstützen müssen enthalten kann: Sie daher anders wie Ihre Daten speichern und verstehen, wie der Dienst funktioniert. Ein gut durchdachte NoSQL-Datenspeicher können Ihre Lösung weiter (und kostengünstiger) als eine Lösung, die eine relationale Datenbank verwendet. Dieses Handbuch hilft diese Themen.  

## <a name="about-the-azure-table-service"></a>Über den Azure-Diensts

In diesem Abschnitt werden einige der wichtigsten Features des Diensts Tabelle, die insbesondere entwerfen sowie Ereignisablaufdiagramme. Wenn Sie neue Azure-Speicher und den Dienst, zuerst lesen Sie [Einführung in Microsoft Azure Storage](storage-introduction.md) und [Einstieg in Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md) vor dem Lesen des Rest dieses Artikels. Zwar den Schwerpunkt dieses Handbuchs auf den Dienst, enthält es eine Diskussion der Azure-Warteschlange und Blob, und sie zusammen mit der Tabelle Service in einer Lösung Verwendung.  

Was ist der Tabelle Service? Erwartungsgemäß vom Namen, verwendet der Dienst tabellarischer Daten speichern. In standard-Terminologie jede Zeile der Tabelle eine Entität darstellt, und Spalten speichern die Eigenschaften dieser Entität. Jede Entität hat Schlüssel eindeutig identifiziert und eine Timestamp-Spalte, die der Dienst verwendet zum Nachverfolgen die Entität wurde aktualisiert (Dies geschieht automatisch und den Zeitstempel können nicht manuell mit einem beliebigen Wert überschrieben). Der Dienst verwendet diese Zeitstempel letzte Änderung (LMT) Parallelität verwalten.  

>[AZURE.NOTE] Dienstvorgänge REST API Tabelle geben ebenfalls einen **ETag** -Wert aus der Zeitstempel letzte Änderung (LMT). In diesem Dokument werden die Begriffe ETag und LMT Synonyme, da sie den gleichen Daten verweisen.  

Das folgende Beispiel zeigt eine einfache Tabellenentwurf, Mitarbeiter und Abteilung Entitäten zu speichern. Viele Beispiele in diesem Handbuch basieren auf dieser einfachen Entwurf.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zeitstempel</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Don</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Jun</td>
<td>CAO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Abteilung</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Vertrieb</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Bisher sieht ähnlich einer Tabelle in einer relationalen Datenbank mit wichtigen Unterschieden, die erforderlichen Spalten und die Fähigkeit, mehrere Entitätstypen in derselben Tabelle zu speichern. Darüber hinaus hat jede benutzerdefinierte Eigenschaften **FirstName** oder **Alter** einen Datentyp wie ganze Zahl oder Zeichenfolge, die nur eine Spalte in einer relationalen Datenbank Anders als in einer relationalen Datenbank mit weniger Schemata Art der Tabelle bedeutet, dass eine Eigenschaft nicht den gleichen Datentyp für jede Entität muss. Um komplexe Datentypen in einer Eigenschaft zu speichern, müssen Sie einem serialisierten JSON oder XML-Format verwenden. Weitere Informationen über die Tabelle Service unterstützt Datentypen finden Sie unter unterstützten Datumsbereiche Benennung Regeln und Größe Einschränkungen [Verstehen des Datenmodells Tabelle Service](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Wie Sie sehen, ist die Wahl der **PartitionKey** und **RowKey** für gute Tabellenentwurf. Jede Entität in einer Tabelle gespeicherten muss eine eindeutige Kombination aus **PartitionKey** und **RowKey**. Als werden Schlüssel in einer relationalen Datenbank, die Werte **PartitionKey** und **RowKey** indiziert einen gruppierten Index erstellen, der schnelle Suchen ermöglicht. Allerdings wird der Dienst keine sekundären Indizes erstellt sind nur zwei indizierten Eigenschaften (Muster beschrieben dieser Einschränkung deutlich Funktionsweise höher anzeigen).  

Eine Tabelle besteht aus einer oder mehreren Partitionen, und wie Sie sehen, werden viele Designentscheidungen vorgenommenen Auswahl einer geeigneten **PartitionKey** und **RowKey** zur Optimierung Ihrer Lösung. Eine Lösung könnte darin bestehen, eine einzelne Tabelle, die alle Entitäten in Partitionen unterteilt enthält, aber in der Regel muss eine Lösung mehrere Tabellen. Tabellen können Sie logisch organisieren Entitäten, Zugriff auf die Daten durch Zugriffssteuerungslisten verwalten und eine gesamte Tabelle mit einem einzigen Speicher ablegen.  

### <a name="table-partitions"></a>Tabellenpartitionen  
Kontoname, Tabellen- und **PartitionKey** identifizieren zusammen in der Speicherdienst speichert die Entität in der Tabelle Service Partition. Als Teil der Adressierungsschema für Entitäten, Partitionen definieren ein Bereichs für Transaktionen (siehe [Entitätsgruppentransaktionen](#entity-group-transactions) unten), sowie Grundlage wie der Dienst skaliert. Weitere Informationen zu Partitionen finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md).  

In der Tabellendienst einzelner Knoten Dienste oder vollständiger Partitionen und Dienst skaliert von dynamischen Lastenausgleich Partitionen auf Knoten. Der Dienst ist ein Knoten unter Last, können *Teilen* an Partitionen von diesem Knoten an andere Knoten bedient; Wenn Datenverkehr nachlässt, der Dienst kann *Zusammenführen* Partition zwischen stillen Knoten auf einem einzelnen Knoten.  

Weitere Informationen über die internen Details der Dienst und wie der Dienst Partitionen verwaltet, finden Sie das Papier [Microsoft Azure Storage: ein hochgradig verfügbaren Cloud-Speicherservice mit starken Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Entität Transaktionen
In der Tabelle Service sind Entitätsgruppentransaktionen (EGTs) der einzige integrierte Mechanismus zum atomaren Updates für mehrere Personen. EGTs werden auch als *Transaktionen* in Dokumentation bezeichnet. EGTs können nur Entitäten in derselben Partition (Freigabe gleichen Partitionsschlüssel in einer Tabelle) gespeicherten so bearbeiten jederzeit atomaren Transaktionsverhalten über mehrere Entitäten müssen Sie sicherstellen, dass die Entitäten in derselben Partition. Deshalb oft um mehrere Entitätstypen in derselben Tabelle (und Partition) und nicht unter Verwendung mehrerer Tabellen für verschiedene Entitätstypen. Eine einzelne EGT kann höchstens 100 Entitäten bearbeiten.  Wenn Sie mehrere gleichzeitige EGTs zur Verarbeitung, ist es wichtig übermitteln, dass die EGTs nicht auf Entitäten betreiben, die häufig in EGTs andernfalls Verarbeitung verzögert werden kann.

EGTs führen auch potenzielle Nachteil für Sie in Ihrem Entwurf auswerten: mehrere Partitionen erhöht die Skalierung Ihrer Anwendung Azure hat mehr Chancen für Ausgleich der Anforderungslast Knoten, da dies möglicherweise die Möglichkeit der Anwendung atomare Transaktionen und starke Konsistenz für Ihre Daten einschränken. Außerdem gibt es bestimmte Skalierbarkeit Zielen auf eine Partition, die den Durchsatz von Transaktionen einschränken Sie für einen einzelnen Knoten erwarten: Weitere Informationen zu den Zielen Erweiterbarkeit Azure-Speicherkonten und Tabelle finden Sie in [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md). In späteren Abschnitten dieses Handbuchs besprechen Sie verschiedene Design Strategien, mit denen Sie verwalten diese Kompromisse und erklärt den besten Ihre Erfordernisse der Clientanwendung Grundlage Partitionsschlüssel auswählen.  

### <a name="capacity-considerations"></a>Aspekte der Kapazität
Die folgende Tabelle enthält einige der Schlüsselwerte Achten Sie beim Entwerfen einer Tabelle servicelösung:  

|Gesamtkapazität eines Kontos Azure-Speicher|500 TB|
|------------------------------------------|------|
|Anzahl von Tabellen in einem Azure Storage-Konto | Begrenzung nur durch die Kapazität des Speicherkontos |
|Anzahl der Partitionen in einer Tabelle | Begrenzung nur durch die Kapazität des Speicherkontos |
|Anzahl von Entitäten in einer partition | Begrenzung nur durch die Kapazität des Speicherkontos|
|Größe einer einzelnen Entität | Bis zu 1 MB maximal 255 Eigenschaften (einschließlich **PartitionKey**, **RowKey**und **Zeitstempel**) |
|Größe des **PartitionKey** | Eine Zeichenfolge von bis zu 1 KB |
| Größe des **RowKey** | Eine Zeichenfolge von bis zu 1 KB |
|Ein Segment Buchung | Eine Transaktion kann höchstens 100 Einheiten und die Nutzlast muss weniger als 4 MB sein. Eine EGT kann eine Entität nur einmal aktualisieren. |

Weitere Informationen finden Sie unter [Understanding Tabelle Service-Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Aspekte der Kosten  
Tabellenspeicher ist relativ günstig sollten, aber Sie Kostenvoranschläge für Kapazitätsauslastung und die Menge der Transaktionen als Teil der Evaluierung einer Lösung, die die Tabelle verwendet. Allerdings ist in vielen Szenarios Verbesserung denormalisierte oder doppelte Daten speichern Leistung oder Skalierung Ihrer Lösung Ansatz zu. Weitere Informationen zu Preisen finden Sie unter [Azure Storage Preise](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Richtlinien für den Entwurf von Tabellen  
Diese Listen zusammenfassen einige wichtigen Richtlinien, die Sie bedenken sollten beim Entwerfen von Tabellen wird, und dabei sie alle später ausführlicher in. Diese Richtlinien unterscheiden sich erheblich von den Richtlinien für relationale üblich.  

Entwerfen der Tabelle servicelösung effizient *Lesen* :

-   ***Entwerfen Sie für Lesevorgänge Applikationen Abfragen.*** Beim Entwerfen von Tabellen denken Abfragen (insbesondere die Wartezeit sensiblen), die Sie ausführen bevor Sie wie Entitäten aktualisiert wird. Dies führt in der Regel eine effiziente und leistungsfähige Lösung.  
-   ***Geben Sie PartitionKey und RowKey in Ihren Abfragen.*** *Zeigen Sie Abfragen* wie diese sind am effizientesten Tabelle Dienstabfragen.  
-   ***Berücksichtigen Sie Speichern von Kopien von Entitäten.*** Tabellenspeicher ist kostengünstig so sollten Sie dieselbe Entität um effizientere Abfragen aktivieren (mit unterschiedlichen Schlüsseln) speichern.  
-   ***Berücksichtigen Sie Denormalisieren von Daten.*** Tabellenspeicher ist kostengünstig so berücksichtigen Denormalisieren von Daten. Speichern Sie Zusammenfassung Elemente z. B., sodass Abfragen für Daten nur eine einzelne Entität zugreifen müssen.  
-   ***Verwenden von zusammengesetzten Schlüsselwerten.*** Der einzige Schlüssel sind **PartitionKey** und **RowKey**. Z. B. zusammengesetzte Schlüsselwerten alternative Schlüssel Zugriffspfade für Personen zu aktivieren.  
-   ***Verwenden Sie Abfrageprojektion.*** Sie können die Datenmenge reduzieren, die Sie mithilfe von Abfragen, die nur die Felder auszuwählen, die Sie über das Netzwerk übertragen.  

Entwerfen der Tabelle servicelösung effizient *zu schreiben* :  

-   ***Hot-Partitionen nicht erstellt werden.*** Wählen Sie Tasten, mit denen sich Ihre Anfragen über mehrere Partitionen zu jedem Zeitpunkt.  
-   ***Vermeiden Sie Spitzen im Datenverkehr.*** Glätten des Datenverkehrs über einen angemessenen Zeitraum und Spitzen im Datenverkehr zu vermeiden.
-   ***Erstellen Sie nicht unbedingt eine separate Tabelle für jeden Entitätstyp.*** Wenn Sie atomare Transaktionen über Entitätstypen benötigen, können Sie diese mehrere Entitätstypen in der gleichen Partition in derselben Tabelle speichern.
-   ***Betrachten Sie den maximalen Durchsatz erreichen müssen.*** Beachten Sie die Skalierbarkeitsziele für den Dienst, und stellen sicher, dass Ihr Design nicht übertreffen verursacht wird.  

Dieses Handbuch erhalten Sie Beispiele, die alle diese Prinzipien umzusetzen.  

## <a name="design-for-querying"></a>Entwurf für Abfragen  
Tabelle Service Solutions können Intensive schreibintensiv oder eine Kombination der beiden gelesen werden. Dieser Abschnitt konzentriert sich auf die Dinge zu beachten beim Entwerfen des Tabelle Diensts effizient Lesevorgänge. Normalerweise ist ein Design, dass Lesevorgänge effizient unterstützt auch für Schreibvorgänge. Allerdings sind zusätzliche Aspekte zu beachten beim support Schreibvorgänge im nächsten Abschnitt [Entwurf für die Änderung von Daten](#design-for-data-modification)beschrieben.

Ein guter Ausgangspunkt für den Entwurf der Tabelle Service-Lösung können Daten effizient gelesen werden Fragen "Was Abfragen Anwendung wird ausgeführt, um die Daten aus der Tabelle Service?"  

>[AZURE.NOTE] Mit dem Dienst Tabelle ist es wichtig zu Entwurf korrekt vorne ist schwierig und teuer später ändern. Beispielsweise in einer relationalen Datenbank kann oft Performance-Probleme einfach durch Hinzufügen von Indizes zu einer vorhandenen Datenbank: Dies ist nicht mit dem Dienst.  

Dieser Abschnitt behandelt die Hauptprobleme, die Adresse muss beim Entwerfen der Tabellen für die Abfrage. In diesem Abschnitt behandelten Themen:

- [Wie wirkt sich auf die Wahl des PartitionKey und RowKey Leistung](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
- [Auswählen einer geeigneten PartitionKey](#choosing-an-appropriate-partitionkey)
- [Optimieren von Abfragen für den Dienst](#optimizing-queries-for-the-table-service)
- [Sortieren von Daten in der Tabelle service](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Wie wirkt sich auf die Wahl des PartitionKey und RowKey Leistung  

In den folgenden Beispielen angenommen der Dienst speichert Mitarbeiterentitäten mit der folgenden Struktur (die meisten Beispiele lassen die **Timestamp** -Eigenschaft aus Gründen der Übersichtlichkeit):  

|*Spaltenname* |*Datentyp*|
|--------------|-----------|
|**PartitionKey** (Abteilungsname)|Zeichenfolge|
|**RowKey** (Mitarbeiter-Id)|Zeichenfolge|
|**Vorname**|Zeichenfolge|
|**Nachname**|Zeichenfolge|
|**ALTER**|Ganze Zahl|
|**EmailAddress**|Zeichenfolge|

Die früheren [Azure Table Service Overview](#overview) beschrieben die Hauptmerkmale der Azure-Diensts, die direkten Einfluss auf Abfrage entwerfen. Diese führen die folgenden Richtlinien für das Entwerfen von Dienstabfragen. Beachten Sie, dass die Filtersyntax in den folgenden Beispielen aus der Tabelle Service REST-API Weitere Informationen finden Sie unter [Abfrage Entitäten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

-   ***Auf Grundlage*** ist die effizienteste Suche verwendet und empfiehlt sich für große suchen oder Lookups erfordern die niedrigste Latenz verwendet werden. Die Indizes können eine Abfrage eine einzelne Entität suchen sehr effizient durch die **PartitionKey** und **RowKey** angeben. Beispiel: $filter = (PartitionKey Eq "Sales") und (RowKey Eq '2')  
-   Zweite beste ist eine ***Bereichsabfrage*** verwendet **PartitionKey** Filter auf einen Wertebereich **RowKey** mehrere Entitäten zurückgegeben. **PartitionKey** -Wert identifiziert eine bestimmte Partition und **RowKey** Werte kennzeichnen eine Teilmenge der Elemente in dieser Partition. Beispiel: $filter = PartitionKey Eq 'Sales und RowKey ge' und RowKey Lt ' t '  
-   3 beste ist eine ***Partition Scannen*** , **PartitionKey** verwendet und auf einem anderen Schlüssel-Eigenschaft filtert und kann mehr als eine Entität zurückgeben. **PartitionKey** -Wert identifiziert eine bestimmte Partition, und wählen Sie die Werte für eine Teilmenge der Elemente in dieser Partition. Beispiel: $filter = PartitionKey Eq 'Sales' und LastName Eq "Smith"  
-   Ein ***Tabellenscan*** enthält keinen **PartitionKey** und ist sehr ineffizient, da alle Partitionen durchsucht, die die Tabelle wiederum für alle übereinstimmenden Elemente bilden. Führt einen Tabellenscan, unabhängig davon, ob der Filter **RowKey**verwendet. Beispiel: $filter = Nachname Eq 'Jones'  
-   Abfragen, die mehrere Entitäten zurückgegeben werden **PartitionKey** und **RowKey** sortiert. Um zu vermeiden, müssen die Entitäten im Client, **RowKey** , die am häufigsten verwendete Sortierreihenfolge definiert auswählen  

Beachten Sie, dass mit einem "**oder**" an einen Filter basierend auf **RowKey** Werte partitionsscan nicht als eine Bereichsabfrage behandelt. Daher sollten Sie Abfragen, wie z. B. Filter verwenden: $filter = PartitionKey Eq 'Sales' und (RowKey Eq '121' oder RowKey Eq '322')  

Beispiele für clientseitigen Code, mit dem der Speicher-Clientbibliothek effiziente Abfragen ausführen, finden Sie unter:  

-   [Mit der Speicher-Clientbibliothek Punktabfrage ausführen](#executing-a-point-query-using-the-storage-client-library)
-   [Abrufen von mehreren Personen mit LINQ](#retrieving-multiple-entities-using-linq)
-   [Serverseitige Projektion](#server-side-projection)  

Beispiele für clientseitigen Code, die mehrere Entitätstypen in derselben Tabelle gespeichert behandeln können, finden Sie unter:  

-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Auswählen einer geeigneten PartitionKey  

Verwendeter **PartitionKey** sollte zwischen der Notwendigkeit ermöglicht die Verwendung von EGTs (auf die Konsistenz) die Anforderung zum Verteilen von Entitäten auf mehrere Partitionen (um eine skalierbare Lösung).  

Eine extrem konnte alle Entitäten in einer Partition speichern, aber dies Begrenzen der Skalierung der Lösung und würde verhindern den Dienst Lastenausgleich Anfragen. Im anderen Extremfall können Sie eine Entität pro Partition speichern, die hochgradig skalierbare und ermöglicht des Tabelle Diensts Lastenausgleich Anfragen, aber das würde verhindern die Verwendung entitätsgruppentransaktionen.  

Eine ideale **PartitionKey** ist, ermöglicht effiziente Abfragen verwenden und mit ausreichende Partitionen um sicherzustellen, dass Ihre Lösung skalierbar ist. Normalerweise finden Sie Ihre Entitäten eine geeignete Eigenschaft verfügen, die Ihre Entitäten über ausreichende Partitionen verteilt.

>[AZURE.NOTE] Beispielsweise kann in einem System, in dem Informationen zu Benutzern oder Mitarbeiter gespeichert UserID gute PartitionKey sein. Sie möglicherweise mehrere Entitäten, die eine bestimmte Benutzer-ID als Partitionsschlüssel verwendet. Jede Entität, die Daten zu einem Benutzer in einer einzelnen Partition gruppiert, und so diese Entitäten sind über entitätsgruppentransaktionen, während hochskalierbare.

Es gibt weitere Aspekte bei der Wahl des **PartitionKey** , die wie Sie einfügen, aktualisieren und Löschen von Entitäten werden auf: Siehe Abschnitt unten [Entwurf für die Änderung von Daten](#design-for-data-modification) .  

### <a name="optimizing-queries-for-the-table-service"></a>Optimieren von Abfragen für den Dienst  

Der Dienst automatisch Entitäten mit den **PartitionKey** und **RowKey** -Werten in einem gruppierten Index indiziert, daher der Grund für die Abfragen zeigen die effizienteste Verwendung. Es gibt jedoch keine Indizes als auf den gruppierten Index für **PartitionKey** und **RowKey**.

Viele Designs müssen ermöglichen die Suche nach Entitäten anhand mehrerer Kriterien erfüllen. Beispielsweise suchen Mitarbeiterentitäten basierend auf e-Mail, Mitarbeiter-Id oder Nachnamen. Die folgenden Muster im Abschnitt [Tabelle](#table-design-patterns) Adresstypen diese Anforderung und Methoden umgehen die Tatsache, dass der Dienst keinen sekundären Indizes:  

-   [Die Partition Sekundärindex Muster](#intra-partition-secondary-index-pattern) - Speicher mehrere Kopien jeder Entität mit unterschiedlichen **RowKey** Werte (in der gleichen Partition) ermöglichen schnelle und effiziente Suchvorgänge und alternative Sortierung Aufträge mit verschiedenen **RowKey** -Werten.  
-   [Sekundärer Index Muster zwischen partition](#inter-partition-secondary-index-pattern) - Speichern mehrerer Kopien jeder Entität mit unterschiedliche RowKey-Werte in separaten Partitionen oder in separaten Tabellen zu ermöglichen schnelle und effiziente Suchvorgänge alternative Sortierreihenfolgen **RowKey** unterscheiden.  
-   [Index Entitäten Muster](#index-entities-pattern) - Index Entitäten um effiziente Suchvorgänge zu ermöglichen, die Listen von Elementen zurückgeben verwalten.  

### <a name="sorting-data-in-the-table-service"></a>Sortieren von Daten in der Tabelle service  

Der Dienst gibt Entitäten aufsteigend sortiert basierend auf **PartitionKey** und **RowKey**. Diese Zeichenfolgenwerte sind und um sicherzustellen, dass numerische Werte korrekt sortiert, sollten mit einer festen Länge konvertiert und mit Nullen aufgefüllt. Ist der Mitarbeiter-Id-Wert als **RowKey** verwenden einen ganzzahligen Wert sollten Sie Mitarbeiter-Id **123** in **00000123**konvertieren.  

Häufig müssen Daten in einer anderen Reihenfolge sortiert: Sortieren z. B. Mitarbeiter oder bei Datum. Die folgenden Muster im Abschnitt [Tabelle](#table-design-patterns) behandelt, wie alternative Sortierreihenfolgen für die Entitäten:  

-   [Die Partition Sekundärindex Muster](#intra-partition-secondary-index-pattern) - Speichern mehrerer Kopien jeder Entität mithilfe der RowKey unterscheiden (in der gleichen Partition) ermöglichen schnelle und effiziente Suchvorgänge alternative Sortierreihenfolgen RowKey unterscheiden.  
-   [Zwischen partitionieren Sekundärindex Muster](#inter-partition-secondary-index-pattern) - Speicher mehrere Kopien jeder Entität mit unterschiedlichen RowKey-Werten in Partitionen in separaten Tabellen separate zu ermöglichen schnelle und effiziente Suchvorgänge alternative Sortierreihenfolgen mit RowKey unterscheiden.
-   [Log Tail Muster](#log-tail-pattern) - Abrufen der *n* Elemente einer Partition mit einem **RowKey** -Wert, der in umgekehrter Datum und Zeit sortiert zuletzt hinzugefügt wurde.  

## <a name="design-for-data-modification"></a>Entwurf für die Änderung von Daten
Dieser Abschnitt konzentriert sich auf die Vorüberlegungen zur Optimierung Einfügevorgänge, Updates und löscht. In einigen Fällen müssen Sie den Kompromiss zwischen auswerten, die für Entwürfe, die Änderung von Daten zu optimieren, wie Entwürfe für relationale Datenbanken (obwohl die Verfahren für die Verwaltung der Entwurf Kompromisse in einer relationalen Datenbank unterscheiden) Abfragen optimieren. Die [Tabelle](#table-design-patterns) beschreibt einige detaillierte Entwurfsmuster für den Dienst und werden einige dieser Nachteile. In der Praxis finden viele Entwürfe für Abfragen von Entitäten optimiert funktionieren auch Entitäten ändern.  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a>Optimieren der Leistung INSERT-, Update- und delete-Operationen  

Zum Aktualisieren oder Löschen einer Entität, müssen Sie ermitteln, wobei die Werte **PartitionKey** und **RowKey** sein. In dieser Hinsicht befolgen die Wahl des **PartitionKey** und **RowKey** zum Ändern von Entitäten ähnliche Kriterien wahlweise für Point-Abfragen um Entitäten so effizient wie möglich zu identifizieren. Sie wollen keine ineffiziente Partition oder Tabelle Scannen zu einer Entität entdecken die Werte **PartitionKey** und **RowKey** müssen Sie aktualisieren oder zu löschen.  

Die folgenden Muster im Abschnitt [Tabelle](#table-design-patterns) Adresse optimieren die Leistung oder die Insert aktualisieren und löschen:  

-   [Hohes Volumen löschen Muster](#high-volume-delete-pattern) - ermöglichen das Löschen eine große Anzahl von Elementen alle Elemente gleichzeitig löschen in eigenen separaten Tabellen gespeichert. Löschen Sie die Elemente löschen der Tabelle.  
-   [Serie Datenmuster](#data-series-pattern) - Speicher vollständig Datenreihen in Einheit die Anzahl der Anfragen minimieren Sie machen.  
-   [Große Unternehmen Muster](#wide-entities-pattern) - verwenden Sie mehrere physische Entitäten logische Entitäten mit mehr als 252 Eigenschaften gespeichert.  
-   [Großer Entitäten Muster](#large-entities-pattern) - Verwendung Blob zur Speicherung von großen Eigenschaftswerte.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Sicherstellung der Konsistenz in Ihren gespeicherten Entitäten  

Andere, die Ihre der Schlüssel zur Optimierung von Änderungen Wahl entscheidend wie Konsistenz mit atomaren Transaktionen. Nur können eine EGT Sie auf Elemente in der gleichen Partition gespeichert.  

Die folgenden Muster im Abschnitt [Tabelle](#table-design-patterns) Adresse verwalten Konsistenz:  

-   [Die Partition Sekundärindex Muster](#intra-partition-secondary-index-pattern) - Speicher mehrere Kopien jeder Entität mit unterschiedlichen **RowKey** Werte (in der gleichen Partition) ermöglichen schnelle und effiziente Suchvorgänge und alternative Sortierung Aufträge mit verschiedenen **RowKey** -Werten.  
-   [Sekundärer Index Muster zwischen partition](#inter-partition-secondary-index-pattern) - Speichern mehrerer Kopien jeder Entität mit unterschiedliche RowKey-Werte in separaten Partitionen oder in separaten Tabellen zu ermöglichen schnelle und effiziente Suchvorgänge alternative Sortierreihenfolgen **RowKey** unterscheiden.  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) - schließlich konsistentes Verhalten in Partition oder Storage Systemgrenzen mit Azure Warteschlangen aktivieren.
-   [Index Entitäten Muster](#index-entities-pattern) - Index Entitäten um effiziente Suchvorgänge zu ermöglichen, die Listen von Elementen zurückgeben verwalten.  
-   [Denormalisierung Muster](#denormalization-pattern) - kombinieren zugehöriger Daten zusammen in einer einzelnen Entität können Sie zum Abrufen der Daten Sie mit einer zentrale Abfrage müssen.  
-   [Serie Datenmuster](#data-series-pattern) - Speicher vollständig Datenreihen in Einheit die Anzahl der Anfragen minimieren Sie machen.  

Informationen zu entitätsgruppentransaktionen finden Sie im Abschnitt [Entitätsgruppentransaktionen](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Sicherstellen des Entwurfs für effiziente Änderungen ermöglicht effiziente Abfragen  

In vielen Fällen sollte ein Entwurf für effiziente Abfragen Ergebnisse effizienter ändern, aber Sie immer prüfen, ob dies für Ihr spezielles Szenario zutrifft. Muster im Abschnitt [Tabelle](#table-design-patterns) explizit zwischen Abfragen und Ändern von Entitäten bewerten und Sie immer berücksichtigen die Anzahl der jeweiligen Operation.  

Die folgenden Muster im Abschnitt [Tabelle](#table-design-patterns) Adresse Kompromisse zwischen Entwurf für effiziente Abfragen und effiziente Änderung entwerfen:  

-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern) - Verbindung **RowKey** Werte zu einem Client Daten mit einer zentrale Abfrage nachschlagen verwenden.  
-   [Log Tail Muster](#log-tail-pattern) - Abrufen der *n* Elemente einer Partition mit einem **RowKey** -Wert, der in umgekehrter Datum und Zeit sortiert zuletzt hinzugefügt wurde.  

## <a name="encrypting-table-data"></a>Verschlüsseln von Daten    
     
.NET Azure Storage-Clientbibliothek unterstützt die Verschlüsselung von Entitätseigenschaften Zeichenfolge für das Einfügen und Ersetzen-Operationen. Die verschlüsselten Zeichenfolgen werden als binäre Eigenschaften im Dienst gespeichert und sie nach der Entschlüsselung in Zeichenfolgen konvertiert.    

Für Tabellen, die Verschlüsselungsrichtlinie geben Benutzer Eigenschaften verschlüsselt werden. Dies erfolgt durch Festlegen ein [EncryptProperty] (für POCO-Entitäten, die von TableEntity abgeleitet) oder einen Konfliktlöser Verschlüsselung Anforderung Optionen. Ein Resolver Verschlüsselung ist ein Delegat, die einen Partitionsschlüssel Zeilenschlüssel und Eigenschaftennamen und gibt einen booleschen Wert, der angibt, ob diese Eigenschaft verschlüsselt werden soll. Bei der Verschlüsselung nutzt die Clientbibliothek diese Informationen um zu entscheiden, ob eine Eigenschaft beim Schreiben in das Netzwerk verschlüsselt werden soll. Der Delegat kann auch der Logik vor wie Eigenschaften sind verschlüsselt. (Z. B. Wenn X, dann verschlüsseln Eigenschaft A; andernfalls verschlüsseln Eigenschaften A und b) Beachten Sie, dass es nicht erforderlich, diese Informationen lesen oder Abfragen von Entitäten.

Beachten Sie, dass Seriendruck wird derzeit nicht unterstützt. Eine Teilmenge der Eigenschaften verschlüsselt wurden möglicherweise bereits mit einem anderen Schlüssel, werden einfach Zusammenführen neuen Eigenschaften und Aktualisieren der Metadaten Datenverlust führen. Zusammenführen entweder erfordert zusätzliche Service-Aufrufe der Dienst bereits vorhandene Entität gelesen oder mit einem neuen Schlüssel pro Eigenschaft, die nicht für Leistung.     

Informationen zum Verschlüsseln von Daten finden Sie unter [clientseitige Verschlüsselung und Azure Schlüssel Depot für Microsoft Azure-Speicher](storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>Modellierung von Vertrauensstellungen  

Erstellen von Domänenmodellen ist ein wichtiger Schritt im Entwurf komplexer Systeme. Normalerweise verwenden Sie die Modellierung Entitäten und der Beziehung zwischen ihnen so zu der Geschäftsdomäne Entwurf des Systems zu informieren. Dieser Abschnitt behandelt wie einige der häufig verwendeten Beziehung Domänenmodelle Designs für den Dienst gefunden übersetzen. Der Prozess der Zuordnung von logische Datenmodell in einer physischen NoSQL-basierten Datenmodell unterscheidet, die beim Entwerfen einer relationalen Datenbank verwendet. Entwurf relationaler Datenbanken nimmt in der Regel eine Normalisierung von Daten optimiert für Redundanz – und eine deklarative Abfragen Funktion, die Funktionsweise der Implementierung wie der Datenbank abstrahiert minimieren.  

### <a name="one-to-many-relationships"></a>1: n-Beziehung  

1: n-Beziehungen zwischen Unternehmen häufig auftreten: eine Abteilung hat z. B. viele Mitarbeiter. Es gibt verschiedene Wege zur Implementierung des 1: n-Beziehung in der Tabellendienst vor-und Nachteile, die auf dem jeweiligen Szenario.  

Beispiel von einem großen multinationalen Unternehmen mit Zehntausenden von Abteilungen und Mitarbeiterentitäten jede Abteilung viele Mitarbeiter und jeder Mitarbeiter hat eine bestimmte Abteilung zugeordnet. Eine Möglichkeit ist die Abteilung und Mitarbeiterentitäten wie diese gespeichert:  

![][1]

Dieses Beispiel zeigt eine implizite 1: n-Beziehung zwischen den Typen basierend auf dem **PartitionKey** -Wert. Jede Abteilung kann viele Mitarbeiter verfügen.  

Dieses Beispiel zeigt auch eine abteilungsentität und seine verwandte Mitarbeiterentitäten in derselben Partition. Sie können verschiedene Partitionen, Tabellen oder sogar Speicherkonten für unterschiedliche Objekttypen verwenden.  

Ein alternativer Ansatz ist zu denormalisieren von Daten speichern nur Mitarbeiterentitäten Abteilung denormalisierte Daten wie im folgenden Beispiel gezeigt. In diesem speziellen Szenario denormalisierte dabei nicht das beste möglicherweise Sie müssen möglicherweise die Details der Abteilungsleiter nicht ändern, da Sie dazu alle Mitarbeiter der Abteilung aktualisieren.  

![][2]

Weitere Informationen finden Sie unter [Denormalisierung Muster](#denormalization-pattern) in diesem Handbuch.  

In der folgenden Tabelle werden die vor- und Nachteile der einzelnen Mitarbeiter und abteilungsentitäten, die eine 1: n-Beziehung beschriebenen Verfahren zusammengefasst. Sie sollten auch berücksichtigen, wie oft Sie erwarten verschiedene Operationen: möglicherweise akzeptabel, eine Design, das diesen Vorgang nur selten Fall aufwendig enthält.  

<table>
<tr>
<th>Ansatz</th>
<th>Profis</th>
<th>Nachteile</th>
</tr>
<tr>
<td>Separate Entitätstypen, dieselbe Partition derselben Tabelle</td>
<td>
<ul>
<li>Sie können eine abteilungsentität in einem Vorgang aktualisieren.</li>
<li>Können Sie eine EGT Konsistenz haben eine Anforderung für eine abteilungsentität ändern Wenn Sie aktualisieren/einfügen/Löschen einer Mitarbeiterentität. Wenn beispielsweise eine Abteilung Mitarbeiterzahl für jede Abteilung verwalten.</li>
</ul>
</td>
<td>
<ul>
<li>Sie müssen einen Mitarbeiter und eine Abteilung Entity Client Aktivitäten abrufen.</li>
<li>Speichervorgänge geschehen in der gleichen Partition. Bei hohen Transaktionsvolumen kann dies einen Hotspot führen.</li>
<li>Sie können keine neue Abteilung eine EGT Mitarbeiter verschieben.</li>
</ul>
</td>
</tr>
<tr>
<td>Separate Entitätstypen, verschiedene Partitionen, Tabellen oder Speicherkonten</td>
<td>
<ul>
<li>Sie können eine abteilungsentität oder Employee-Entität in einem Vorgang aktualisieren.</li>
<li>Bei hohen Transaktionsvolumen kann helfen die Last auf mehrere Partitionen verteilt.</li>
</ul>
</td>
<td>
<ul>
<li>Sie müssen einen Mitarbeiter und eine Abteilung Entity Client Aktivitäten abrufen.</li>
<li>Sie können EGTs Konsistenz Wenn Sie aktualisieren/einfügen/Löschen des Mitarbeiters und Aktualisieren einer Abteilung. Beispielsweise wird eine Anzahl der Mitarbeiter in einer abteilungsentität aktualisiert.</li>
<li>Sie können keine neue Abteilung eine EGT Mitarbeiter verschieben.</li>
</ul>
</td>
</tr>
<tr>
<td>Denormalisieren Sie in einzelner Entitätstyp</td>
<td>
<ul>
<li>Sie können die Informationen mit einer Anforderung abrufen.</li>
</ul>
</td>
<td>
<ul>
<li>Möglicherweise teuer Konsistenz ggf. Abteilung Informationen (Dies würde alle Mitarbeiter in einer Abteilung aktualisieren) aktualisieren.</li>
</ul>
</td>
</tr>
</table>

Wie wählen Sie zwischen diesen Optionen und die vor-und Nachteile sind, hängt von der bestimmten Anwendungsszenarios. Beispielsweise, wie oft Sie abteilungsentitäten ändere; alle Mitarbeiter Abfragen die zusätzliche Abteilung Informationen?; wie weit sind Sie die Skalierungseigenschaften Partitionen oder das Speicherkonto?  

### <a name="one-to-one-relationships"></a>1: 1-Beziehung  

Domänenmodelle können 1: 1-Beziehung zwischen Entitäten enthalten. Benötigen Sie eine 1: 1-Beziehung in den Dienst implementieren, müssen Sie auch wählen zwei verknüpften Entitäten verknüpfen, wenn beide abgerufen werden müssen. Dieser Link kann eine Verknüpfung in Form von **PartitionKey** und **RowKey** in jede Entität verknüpfte Entität speichern implizite, auf eine Konvention in die Schlüsselwerte oder explizit sein. Eine Beschreibung, ob verknüpften Entitäten in der Partition gespeichert werden sollten, finden Sie im Abschnitt [1: n-Beziehung](#one-to-many-relationships).  

Beachten Sie, dass auch Aspekte der Implementierung, die 1: 1-Beziehungen in den Dienst implementieren führen:  

-   Verarbeitung große Entitäten (Weitere Informationen finden Sie in [Großen Unternehmen Muster](#large-entities-pattern)).  
-   Implementieren (Weitere Informationen finden Sie unter [Steuern des Zugriffs mit Shared Access Signatures](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-the-client"></a>Der Client teilnehmen  

Verfahren zum Modell Beziehungen in den Dienst gibt, sollten Sie nicht vergessen, dass zwei Hauptgründe für die Verwendung der Tabelle Service Skalierbarkeits- und Performance. Wenn Sie, dass viele Beziehungen Modellierung werden die Leistung und Skalierung Ihrer Lösung beeinträchtigt feststellen, sollten Sie Fragen bei Bedarf alle Beziehungen in Ihrer Ausführung zu erstellen. Möglicherweise zu vereinfachen das Design Skalierbarkeits- und Performance Ihrer Lösung die Clientanwendung erforderlichen Joins durchführen lassen.  

Beispielsweise haben Sie kleine Tabellen, die Daten enthalten, die nicht häufig ändern, können Sie diese Daten einmal abgerufen und auf dem Client zwischengespeichert. Daher können wiederholte Roundtrips um dieselben Daten abzurufen. In den Beispielen haben wir in diesem Handbuch sind voraussichtlich mehrere Abteilungen in einem kleinen Unternehmen gering und selten so es gut für die Clientanwendung einmal herunterladen kann Daten und Cache Nachschlagen Daten ändern.  

### <a name="inheritance-relationships"></a>Vererbung von Vertrauensstellungen  

Wenn die Clientanwendung eine Reihe von Klassen, die Teil einer Beziehung Vererbung zur Darstellung von Geschäftsentitäten verwendet, können die Entitäten in der Tabelle Service einfach beibehalten. Beispielsweise müssen Sie möglicherweise den folgenden definierten Satz von Klassen in der Clientanwendung ist **Person** eine abstrakte Klasse.

![][3]

Sie können Instanzen von zwei konkreten Klassen in der Tabelle Service mit einer einzelnen Personentabelle mit Entitäten in diesem Aussehen beibehalten:  

![][4]

Weitere Informationen zum Arbeiten mit mehreren Entitätstypen in derselben Tabelle im Clientcode finden Sie im Abschnitt [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types) in diesem Handbuch. Diese Beispiele Entitätstyp im Clientcode erkennen.  

## <a name="table-design-patterns"></a>Tabelle
In den vorherigen Abschnitten haben Sie gesehen, dass einige detaillierte Diskussionen zum Optimieren der Tabellenentwurf für beide Abrufen von Entitätsdaten mithilfe von Abfragen und zum Einfügen, aktualisieren und Löschen von Entitätsdaten. Dieser Abschnitt beschreibt einige Muster für Tabelle Service Solutions geeignet. Darüber hinaus sehen Sie, wie Sie einige der Probleme und Kompromisse, die zuvor in diesem Handbuch ausgelöst praktisch behandeln können. Das folgende Diagramm fasst die Beziehung zwischen den verschiedenen Mustern:  

![][5]

Die Karte Muster zeigt einige Beziehungen (Blau) Muster und Antimuster (Orange), die in diesem Handbuch dokumentiert sind. Gibt es viele weitere Muster Überlegung Wert sind. Beispielsweise ist eine wichtigen Szenarien für Dienst [Materialisierte Ansicht Muster](https://msdn.microsoft.com/library/azure/dn589782.aspx) aus dem [Befehl Abfrage Verantwortung Segregation (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) verwenden.  

### <a name="intra-partition-secondary-index-pattern"></a>Intra-Partition Sekundärindex Muster
Speichern mehrerer Kopien jeder Entität mit unterschiedlichen **RowKey** Werten (in der gleichen Partition) schnelle und effiziente Suchvorgänge und alternative Sortierung nach verschiedenen **RowKey** Werte. Updates zwischen Kopien konsistent gehalten werden mithilfe des EGT.  

#### <a name="context-and-problem"></a>Kontext und problem
Der Dienst indiziert automatisch Entitäten die Werte **PartitionKey** und **RowKey** . Dies ermöglicht eine Clientanwendung eine Entität effizient mit diesen Werten abzurufen. Beispielsweise können über Tabellenstruktur unten eine Clientanwendung auf Grundlage eine Mitarbeiter-Entität abrufen, indem Sie den Abteilungsnamen und die Mitarbeiter-Id (die Werte **PartitionKey** und **RowKey** ). Ein Client kann auch sortierte nach Mitarbeiter-Id in jeder Abteilung Entitäten abrufen.

![][6]

Möchten Sie auch eine Mitarbeiterentität basierend auf dem Wert einer anderen Eigenschaft wie e-Mail-Adresse finden verwenden Sie einen weniger effizienten partitionsscan, um eine Übereinstimmung zu finden. Ist der Dienst keine sekundären Indizes bereit. Darüber hinaus gibt es keine Option, um eine Liste der Mitarbeiter in einer anderen Reihenfolge als **RowKey** Reihenfolge sortiert.  

#### <a name="solution"></a>Lösung
Zum Umgehen der mangelnden Sekundärindizes können Sie mehrere Kopien jeder Entität jede Kopie mit einem anderen **RowKey** Wert speichern. Speichern eine Entität mit den unten können Sie effizient Mitarbeiterentitäten basierend auf e-Mail-Adresse oder den Mitarbeiter-Id abrufen. Die Präfixwerte für **RowKey**, "Empid_" und "Email_" können Sie Abfragen für einen einzelnen Mitarbeiter oder Mitarbeiter mit e-Mail-Adressen oder Mitarbeiter-Ids.  

![][7]

Die folgenden beiden Filterkriterien (eine Suche nach Mitarbeiter-Id und eine Suche nach e-Mail-Adresse) angeben zeigen Abfragen:  

-   $filter = (PartitionKey Eq "Sales") und (RowKey Eq 'empid_000223')  
-   $filter = (PartitionKey Eq "Sales") und (RowKey Eq'email_jonesj@contoso.com')  

Bei einer Abfrage für einen Mitarbeiterentitäten können Sie einen Bereich sortiert Mitarbeiter Id oder einem Bereich sortiert e-Mail-Adresse durch Abfragen von Entitäten mit dem entsprechenden Präfix **RowKey**.  

-   Alle Angestellten in der Verkaufsabteilung mit Mitarbeiter-Id im Bereich 000100 000199 verwenden suchen: $filter = (PartitionKey Eq "Sales") und (RowKey ge 'empid_000100') und (RowKey le 'empid_000199')  
-   Alle Angestellten in der Verkaufsabteilung eine e-Mail-Adresse beginnend mit dem Buchstaben "a" mit suchen: $filter = (PartitionKey Eq "Sales") und (RowKey ge 'Email_a') und (RowKey 'Email_b')  

 Beachten Sie, dass in den obigen Beispielen verwendeten Filtersyntax aus der Tabelle Service REST-API Weitere Informationen finden Sie unter [Abfrage Entitäten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Tabellenspeicher ist relativ günstig Kostenaufwand doppelte Daten speichern ein Hauptanliegen nicht verwenden. Sie sollten immer auswerten die Kosten für den Entwurf basierend auf der erwarteten Speicherbedarf und fügen nur doppelte Entitäten Abfragen unterstützen die Clientanwendung ausführen.  
-   Da Sekundärindex Entitäten in derselben Partition wie die ursprünglichen Elemente gespeichert werden, sollten Sie sicherstellen, Skalierbarkeitsziele für eine einzelne Partition nicht überschreiten.  
-   Sie können doppelte Elemente miteinander konsistent mit EGTs zwei Kopien der Entität automatisch aktualisiert. Dies bedeutet, dass alle Kopien einer Entität in der Partition gespeichert werden soll. Weitere Informationen finden Sie im Abschnitt [Entitätsgruppentransaktionen verwenden](#entity-group-transactions).  
-   Für **RowKey** verwendete Wert muss für jede Entität eindeutig sein. Erwägen Sie zusammengesetzte Schlüsselwerten.  
-   Textabstand **RowKey** (z. B. die Mitarbeiter-Id 000223), numerische Werte können richtige Sortierung und Filterung basierend auf oberen und unteren Grenzen.  
-   Sie müssen nicht unbedingt alle Eigenschaften der Entität duplizieren. Wenn Abfragen, Suche **RowKey** Elemente e-Mail Adresse nie Alter des Mitarbeiters, konnte diese Entitäten beispielsweise folgende Struktur haben:

![][8]

-   Es ist normalerweise besser duplizierte Daten speichert und sicherstellen, dass Sie die Daten abrufen können, Sie mit einer einzigen Abfrage müssen, als Abfrage zu einer Entität und anderen erforderlichen Daten suchen verwenden.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster, wenn die Clientanwendung Entitäten mit einer Reihe von verschiedenen Schlüssel beim Abrufen von Entitäten in unterschiedlichen Sortierreihenfolgen Cient abrufen muss, und jede Entität mit einer Reihe von eindeutigen Werten zu identifizieren. Aber sollten Sie nicht die Partition Skalierungseigenschaften überschreiten bei Entität Lookups über unterschiedliche **RowKey** -Werte.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Sekundärer Index zwischen Partition Muster](#inter-partition-secondary-index-pattern)
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)
-   [Entität Transaktionen](#entity-group-transactions)
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Sekundärer Index zwischen Partition Muster
Mehrere Kopien jeder Entität mit unterschiedlichen **RowKey** Werte in separate Partitionen oder in separaten Tabellen schnelle und effiziente Suchvorgänge und alternative Sortierung nach verwenden unterschiedliche **RowKey** -Werte.  

#### <a name="context-and-problem"></a>Kontext und problem
Der Dienst indiziert automatisch Entitäten die Werte **PartitionKey** und **RowKey** . Dies ermöglicht eine Clientanwendung eine Entität effizient mit diesen Werten abzurufen. Beispielsweise können über Tabellenstruktur unten eine Clientanwendung auf Grundlage eine Mitarbeiter-Entität abrufen, indem Sie den Abteilungsnamen und die Mitarbeiter-Id (die Werte **PartitionKey** und **RowKey** ). Ein Client kann auch sortierte nach Mitarbeiter-Id in jeder Abteilung Entitäten abrufen.  

![][9]

Möchten Sie auch eine Mitarbeiterentität basierend auf dem Wert einer anderen Eigenschaft wie e-Mail-Adresse finden verwenden Sie einen weniger effizienten partitionsscan, um eine Übereinstimmung zu finden. Ist der Dienst keine sekundären Indizes bereit. Darüber hinaus gibt es keine Option, um eine Liste der Mitarbeiter in einer anderen Reihenfolge als **RowKey** Reihenfolge sortiert.  

Sie erwarten eine sehr große Anzahl von Transaktionen für diese Entitäten und möglichst der Drosselung des Clients-Diensts.  

#### <a name="solution"></a>Lösung  
Zum Umgehen der mangelnden Sekundärindizes können Sie mehrere Kopien jeder Entität mit jeder Kopie unter Verwendung verschiedener Werte für **PartitionKey** und **RowKey** speichern. Speichern eine Entität mit den unten können Sie effizient Mitarbeiterentitäten basierend auf e-Mail-Adresse oder den Mitarbeiter-Id abrufen. Präfixwerte für **PartitionKey**, "Empid_" und "Email_" können Sie zum Identifizieren der Index für eine Abfrage verwenden möchten.  

![][10]

Die folgenden beiden Filterkriterien (eine Suche nach Mitarbeiter-Id und eine Suche nach e-Mail-Adresse) angeben zeigen Abfragen:  

-   $filter = (PartitionKey Eq ' Empid_Sales') und (RowKey Eq '000223')
-   $filter = (PartitionKey Eq ' Email_Sales') und (RowKey Eq'jonesj@contoso.com')  

Bei einer Abfrage für einen Mitarbeiterentitäten können Sie einen Bereich sortiert Mitarbeiter Id oder einem Bereich sortiert e-Mail-Adresse durch Abfragen von Entitäten mit dem entsprechenden Präfix **RowKey**.  

-   Alle Angestellten in der Verkaufsabteilung mit Mitarbeiter-Id in den Bereich **000100** , **000199** Mitarbeiter Id Reihenfolge verwendet sortiert suchen: $filter = (PartitionKey Eq ' Empid_Sales') und (RowKey ge '000100') und (RowKey le '000199')  
-   Alle Angestellten in der Verkaufsabteilung eine e-Mail-Adresse zu suchen, die mit "a" in Verwendung der e-Mail-Adresse Reihenfolge sortiert: $filter = (PartitionKey Eq ' Email_Sales') und (RowKey ge "a") und (RowKey "b")  

Beachten Sie, dass in den obigen Beispielen verwendeten Filtersyntax aus der Tabelle Service REST-API Weitere Informationen finden Sie unter [Abfrage Entitäten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  
Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Sie können doppelten Entitäten schließlich miteinander konsistent mit [schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) zu primären und sekundären Index Entitäten.  
-   Tabellenspeicher ist relativ günstig Kostenaufwand doppelte Daten speichern ein Hauptanliegen nicht verwenden. Sie sollten immer auswerten die Kosten für den Entwurf basierend auf der erwarteten Speicherbedarf und fügen nur doppelte Entitäten Abfragen unterstützen die Clientanwendung ausführen.  
-   Für **RowKey** verwendete Wert muss für jede Entität eindeutig sein. Erwägen Sie zusammengesetzte Schlüsselwerten.  
-   Textabstand **RowKey** (z. B. die Mitarbeiter-Id 000223), numerische Werte können richtige Sortierung und Filterung basierend auf oberen und unteren Grenzen.  
-   Sie müssen nicht unbedingt alle Eigenschaften der Entität duplizieren. Wenn Abfragen, Suche **RowKey** Elemente e-Mail Adresse nie Alter des Mitarbeiters, konnte diese Entitäten beispielsweise folgende Struktur haben:

    ![][11]

-   Es ist normalerweise besser doppelte Daten speichern und Abrufen von alle Daten mit einer einzigen Abfrage als Abfrage zu einer Entität mit sekundären Index und eine weitere für die Suche erforderlichen Daten im primären Index verwenden.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  
Verwenden Sie dieses Muster, wenn die Clientanwendung Entitäten mit einer Reihe von verschiedenen Schlüssel beim Abrufen von Entitäten in unterschiedlichen Sortierreihenfolgen Cient abrufen muss, und jede Entität mit einer Reihe von eindeutigen Werten zu identifizieren. Verwenden Sie dieses Muster, wenn Sie überschreiten die Skalierungseigenschaften Partition bei Entität Lookups über unterschiedliche **RowKey** -Werte.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung
Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  
-   [Intra-Partition Sekundärindex Muster](#intra-partition-secondary-index-pattern)  
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Entität Transaktionen](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Schließlich konsistent Transaktionen Muster  

Aktivieren Sie schließlich konsistentes Verhalten in Partition oder Storage Systemgrenzen mithilfe von Azure Warteschlangen.  

#### <a name="context-and-problem"></a>Kontext und problem  

EGTs aktivieren atomare Transaktionen über mehrere Entitäten, die den gleichen Partition Schlüssel. Für Leistung und Erweiterbarkeit, könnten Elemente speichern, die Konsistenz in separate Partitionen oder in einem separaten Speichersystem benötigen: in diesem Fall können Sie EGTs Konsistenz. Z. B. möglicherweise erforderlich, eventuelle Konsistenz zwischen:  

-   Entitäten, die in verschiedenen Konten in zwei verschiedenen Partitionen in der gleichen Tabelle in verschiedenen Tabellen gespeichert.  
-   Eine Entität in der Tabelle Service gespeichert und ein Blob im Blob-Dienst gespeichert.  
-   Eine Entität, Tisch und eine Datei in einem Dateisystem gespeichert.  
-   Ein Entität Speicher in der Tabelle Service indiziert noch mit dem Azure-Suchdienst.  

#### <a name="solution"></a>Lösung  

Mithilfe von Azure-Warteschlangen können Sie eine Lösung implementieren, die über Partitionen oder Speichersysteme eventual Consistency bereitstellt.
Um dies zu veranschaulichen, vorausgesetzt, dass Sie unbedingt alte Mitarbeiter Elemente archivieren. Alte Mitarbeiterentitäten sind selten abgefragt und ausgeschlossen Aktivitäten, die Mitarbeiter betreffen. Zum Implementieren dieser Anforderung speichern Sie aktive Mitarbeiter bei der **aktuellen** Tabelle alte in **die Archivtabelle** . Archivierung eines Mitarbeiters müssen Sie die Entität aus der **aktuellen** Tabelle löschen und **die Archivtabelle** Entität hinzufügen, jedoch können Sie eine EGT diese beiden Vorgänge. Um zu vermeiden, die Fehler in beide oder keines von beiden Tabellen eine Entität angezeigt wird, muss die Archivoperation schließlich konsistent sein. Das folgende Sequenzdiagramm werden die Schritte bei diesem Vorgang. Ausnahmepfade in folgenden Text genauer vorgesehen.  

![][12]

Ein Client startet das Archiv mit einer Nachricht in einer Warteschlange Azure dabei Mitarbeiter #456 archivieren. Eine Worker-Rolle fragt die Warteschlange auf neue Nachrichten; Wenn gefunden, liest die Meldung und eine verborgene Kopie der Warteschlange verbleibt. Worker-Rolle anschließend eine Kopie der Entität aus der **aktuellen** Tabelle abruft, fügt eine Kopie in **die Archivtabelle** und löscht dann das Original aus der **aktuellen** Tabelle. Wenn keine aus den vorherigen Schritten Fehler, löscht Worker-Rolle schließlich versteckte Nachricht aus der Warteschlange.  

In diesem Beispiel fügt Schritt 4 den Mitarbeiter in **die Archivtabelle** . Sie können ein Blob im Blob-Dienst oder eine Datei in einem Dateisystem Mitarbeiter hinzufügen.  

#### <a name="recovering-from-failures"></a>Wiederherstellung bei Fehlern  

Es ist wichtig, die Vorgänge in die Schritte **4** und **5** muss *Idempotent* Worker-Rolle muss die Archivoperation zu starten. Wenn Sie den Dienst verwenden, sollten Sie unter Schritt **4** eine Operation "Einfügen oder ersetzen" verwenden. Schritt **5** Verwenden einer "löschen, wenn vorhanden" Operation in der Clientbibliothek Sie verwenden. Wenn Sie ein anderes Speichersystem verwenden, müssen Sie eine entsprechende idempotente Operation verwenden.  

Wenn Worker-Rolle Schritt **6**niemals abgeschlossen ist, erscheint dann nach einem Timeout die Nachricht versuchen es erneut verarbeiten Worker-Rolle zur Warteschlange. Worker-Rolle kann überprüfen, wie oft eine Meldung in der Warteschlange wurde gelesen und ggf. kennzeichnen ist eine "unzustellbar" Meldung zur Untersuchung von einer separaten Warteschlange senden. Weitere Informationen zu Warteschlangennachrichten lesen und überprüft die Anzahl der Dequeue finden Sie [Nachrichten erhalten](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Die Clientanwendung sollte dafür geeigneten Wiederholungslogik Fehler Tabelle und Warteschlange sind vorübergehende Fehler  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte
Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Diese Lösung bietet keine für Transaktionsisolation. Beispielsweise konnte ein Client Tabellen **aktuelle** und **Archiv** lesen, Worker-Rolle war zwischen den Schritten **4** und **5**und Siehe inkonsistente Sichten der Daten. Beachten Sie, dass die Daten konsistent schließlich.  
-   Sie müssen sicher sein, dass die Schritte 4 und 5 Idempotent sind, um eventuelle Konsistenz.  
-   Sie können die Lösung mithilfe mehrerer Warteschlangen und Arbeitskraft Rolleninstanzen skalieren.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  
Verwenden Sie dieses Muster, wenn Sie eventuelle Konsistenz zwischen Entitäten zu gewährleisten, die in verschiedenen Partitionen oder Tabellen vorhanden. Sie können dieses Muster hin für Vorgänge in der Tabelle Service und BLOB-Dienst und andere Azure Storage Datenquellen wie Datenbank oder dem Dateisystem Konsistenz erweitern.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  
Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  
-   [Entität Transaktionen](#entity-group-transactions)  
-   [Zusammenführen oder ersetzen](#merge-or-replace)  

>[AZURE.NOTE] Wenn Transaktionsisolation Projektmappe wichtig ist, sollten Sie Tabellen zur EGTs Verwendung ändern.  

### <a name="index-entities-pattern"></a>Index Entitäten Muster
Verwalten Sie Index-Entitäten, um effiziente Suchvorgänge mit denen Listen von Elementen zurück.  

#### <a name="context-and-problem"></a>Kontext und problem  

Der Dienst indiziert automatisch Entitäten die Werte **PartitionKey** und **RowKey** . Dies ermöglicht eine Clientanwendung zum Abrufen einer Entität effizient mithilfe einer Abfrage zeigen. Beispielsweise kann mit der Struktur unten, eine Clientanwendung effizient eine Mitarbeiterentität abzurufen mit den Namen der Abteilung und die Mitarbeiter-Id ( **PartitionKey** und **RowKey**).  

![][13]

Möchten Sie auch eine Liste der Mitarbeiterentitäten basierend auf dem Wert einer anderen Eigenschaft nicht eindeutige wie Nachname, abrufen müssen Sie einen weniger effizienten partitionsscan entspricht, anstatt einen Index direkt suchen zu verwenden. Ist der Dienst keine sekundären Indizes bereit.  

#### <a name="solution"></a>Lösung  

Um Suche nach Nachnamen mit der obigen Entität zu aktivieren, müssen Sie Listen der Mitarbeiter-Ids. Möchten Sie abrufen von Mitarbeiterentitäten mit einem bestimmten Nachnamen wie Jones, müssen zuerst die Liste der Mitarbeiter-Ids für Mitarbeiter mit Jones Nachnamen suchen und Abrufen-Entitäten Mitarbeiter. Es gibt drei Optionen zum Speichern von Listen der Mitarbeiter-Ids:  

-   Verwenden Sie BLOB-Speicherung.  
-   Erstellen Sie Index-Entitäten in derselben Partition wie die Mitarbeiterentitäten.  
-   Erstellen Sie Index-Entitäten in einer separaten Partition oder Tabelle.  

<u>Option #1: Mit BLOB-Speicher</u>  

Für die erste Option erstellen Sie eine Liste der **PartitionKey** (Abteilung) und **RowKey** (Mitarbeiter-Id) Werte für Mitarbeiter, die Nachnamen einen Blob für jede eindeutige Name und in jedem BLOB-Speicher. Beim Hinzufügen oder ein Mitarbeiters löschen sollten Sie sicherstellen, dass der Inhalt des entsprechenden Blob schließlich Mitarbeiterentitäten entspricht.  

<u>Option #2:</u> Index-Entitäten in der gleichen Partition erstellen  

Verwenden Sie für die zweite Option Index-Entitäten, die die folgenden Daten gespeichert:  

![][14]

Die **EmployeeIDs** -Eigenschaft enthält eine Liste der Mitarbeiter-Ids für Mitarbeiter mit dem Nachnamen in **RowKey**gespeichert.  

Die folgenden Schritte beschreiben den Vorgang, den Sie befolgen sollten, wenn Sie einen neuen Mitarbeiter hinzufügen, verwenden Sie die zweite Option. In diesem Beispiel werden wir einen Mitarbeiter-Id 000152 und einen Nachnamen Jones in der Verkaufsabteilung hinzufügen:  
1.  Index-Entität mit einem **PartitionKey** -Wert "Sales" und der **RowKey** -Wert "Jones" Abrufen Speichern Sie das ETag der Entität, die in Schritt 2 verwendet.  
2.  Erstellen einer Entität Gruppe Transaktions (einem Batchvorgang), die fügt neue Employee-Entität (**PartitionKey** -Wert "Sales" und **RowKey** -Wert "000152") und aktualisiert die Index-Entität (**PartitionKey** -Wert "Sales" und **RowKey** -Wert "Jones") der Liste im Feld EmployeeIDs die neuen Mitarbeiter-Id hinzu. Weitere Informationen zu entitätsgruppentransaktionen finden Sie unter [Entitätsgruppentransaktionen](#entity-group-transactions).  
3.  Ausfall der entitätsgruppentransaktion Fehler Parallelität (jemand verändert nur die Entität Index) müssen Sie Schritt 1 erneut beginnen.  

Sie können einen ähnlichen Ansatz Mitarbeiters löschen, wenn Sie die zweite Option verwenden. Ändern der Nachname des Mitarbeiters ist etwas komplexer, da müssen Sie eine entitätsgruppentransaktion ausführen, die drei Entitäten aktualisiert: Employee-Entität Index Entität für den alten Nachnamen und Index-Entität für die neuen Nachnamen. Jede Entität muss vor jeder Änderung um ETag-Werte abzurufen, Sie vollständige Parallelität aktualisieren abgerufen werden.  

Die folgenden Schritte beschreiben den Vorgang folgen sollten, wenn Sie müssen alle Mitarbeiter mit einem bestimmten Nachnamen in einer Abteilung verwenden Sie die zweite Option. In diesem Beispiel werden alle Mitarbeiter mit dem Nachnamen Jones in der Verkaufsabteilung gesucht:  

1.  Index-Entität mit einem **PartitionKey** -Wert "Sales" und der **RowKey** -Wert "Jones" Abrufen  
2.  Die Liste der Mitarbeiter-Ids im Feld EmployeeIDs analysiert.  
3.  Benötigen Sie zusätzliche Informationen (wie ihre e-Mail-Adressen) Mitarbeiter rufen Sie aller Mitarbeiter Elemente **PartitionKey** Wert "Sales" **RowKey** Werte und die Liste der Mitarbeiter, die Sie in Schritt 2 erhalten ab.  

<u>Option #3:</u> Erstellen Sie Index-Entitäten in einer separaten Partition oder Tabelle  

Verwenden Sie für die dritte Option Index-Entitäten, die die folgenden Daten gespeichert:  

![][15]

Die **EmployeeIDs** -Eigenschaft enthält eine Liste der Mitarbeiter-Ids für Mitarbeiter mit dem Nachnamen in **RowKey**gespeichert.  

EGTs können Sie die dritte Option Konsistenz, da die Index Entitäten in einer separaten Partition aus der Employee-Entitäten sind. Sie sollten sicherstellen, dass die Index-Entitäten schließlich Mitarbeiterentitäten entsprechen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  
-   Erfordert mindestens zwei Abfragen zum Abrufen der entsprechender Entitäten: eine Abfrage Entitäten Index zu der Liste der **RowKey** -Werte und Abfragen für jede Entität in der Liste abzurufen.  
-   Eine einzelne Entität maximal 1 MB hat, angenommen Option #2 und Option #3 in der Projektmappe, die Liste der Mitarbeiter-Ids für alle angegebenen Nachnamen nie größer als 1 MB. Ist die Liste der Mitarbeiter-Ids größer als 1 MB groß sein, Option #1 verwenden und die Daten im BLOB-Speicher gespeichert.  
-   Option #2 verwenden müssen (mit EGTs hinzufügen und Löschen von Mitarbeitern und Nachnamen eines Mitarbeiters ändern) Sie bewerten, wenn das Transaktionsvolumen Skalierbarkeitsgrenzen in einer bestimmten Partition durchgeführt werden soll. Ist dies der Fall, sollten Sie eine schließlich einheitliche Lösung (Option #1 oder #3), die die Update-Anfragen mithilfe von Warteschlangen und Index Entitäten in einer separaten Partition aus der Employee-Entitäten zu speichern.  
-   Option #2 in dieser Lösung wird davon ausgegangen, dass Sie innerhalb einer Abteilung nach Nachnamen nachschlagen möchten: Sie beispielsweise zum Abrufen einer Liste von Nachnamen Jones in der Verkaufsabteilung. Alle Mitarbeiter mit einem Nachnamen Jones in der gesamten Organisation suchen können, verwenden Sie entweder Option #1 oder #3.
-   Eine Warteschlange basierende Lösung eventual Consistency implementieren (siehe [schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) für Weitere Informationen).  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster, wenn Sie eine Reihe von Entitäten suchen, die einen gemeinsame Eigenschaftswert, z. B. alle Mitarbeiter mit dem Nachnamen Jones gemeinsam verwenden.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  
-   [Entität Transaktionen](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormalisierung Muster  

Kombinieren Sie Daten in einer einzelnen Entität können Sie zum Abrufen der Daten mit einer zentrale Abfrage.  

#### <a name="context-and-problem"></a>Kontext und problem  

In einer relationalen Datenbank normalisieren Sie i. d. r. Daten zur Entfernung von Duplizierung resultierenden in Abfragen, die Daten aus mehreren Tabellen abrufen. Wenn der Daten in Azure-Tabellen normalisieren müssen Sie mehrere Roundtrips vom Client an den Server Ihre Daten abrufen. Z. B. Struktur unten Sie benötigen zwei Roundtrips zum Abrufen der Informationen für eine Abteilung: eine abteilungsentität abrufen, die die Manager-Id und dann eine weitere Anforderung Managerdetails in Employee-Entität abrufen.  

![][16]

#### <a name="solution"></a>Lösung  

Anstatt die Daten in zwei separaten Einheiten, Denormalisieren von Daten und eine Kopie des Vorgesetzten Details in der Department-Entität. Zum Beispiel:  

![][17]

Mit Abteilung mit diesen Eigenschaften gespeichert können Sie jetzt die Details Abrufen über eine Abteilung mit einer Abfrage zeigen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Gibt es einige Kosten Aufwand zweimal, einige Daten zu speichern. Die Leistung nutzen (weniger Anfragen Storage Service) in der Regel überwiegen die marginale Erhöhung Lagerhaltungskosten und Kosten wird teilweise durch eine Anzahl von Transaktionen, die Sie benötigen, um die Details einer Abteilung abrufen.  
-   Sie müssen die Konsistenz der beiden Entitäten, die Informationen über Managern speichern. Behandelt das Problem Konsistenz mit EGTs mehrere Entitäten in einer einzelnen atomaren Transaktion aktualisiert: in diesem Fall die Department-Entität und die Employee-Entität des Abteilungsleiters in derselben Partition gespeichert.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden
Verwenden Sie dieses Muster, wenn Sie häufig nach Informationen suchen müssen. Dieses Muster reduziert die Anzahl der Abfragen, die der Client zum Abrufen der Daten erforderlichen vornehmen muss.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung
Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  
-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Entität Transaktionen](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Zusammengesetzte Schlüssel Muster  

Verwenden Sie zusammengesetzte **RowKey** Werte, um auf einem Client mit einer zentrale Abfrage Daten suchen.  

#### <a name="context-and-problem"></a>Kontext und problem  

In einer relationalen Datenbank ist es ganz natürlich mit Joins Abfragen verwandte Daten in einer einzelnen Abfrage an den Client zurückgegeben. Die Mitarbeiter-Id können Sie eine Liste der verknüpften Entitäten nachschlagen, die Leistung und Überprüfen von Daten für diesen Mitarbeiter.  

Angenommen Sie, Speichern von Employee-Entitäten in der Tabelle Service mit der folgenden Struktur:  

![][18]

Müssen auch Verlaufsdaten zur Überprüfung und Leistung für jedes Jahr arbeitete der Mitarbeiter Ihrer Organisation speichern und Sie müssen Zugriff auf diese Informationen nach Jahr. Eine Möglichkeit ist eine weitere Tabelle erstellen, die Elemente mit der folgenden Struktur gespeichert:  

![][19]

Beachten Sie, dass bei diesem Ansatz Sie entscheiden können, einige Informationen (wie Vorname und Nachname) duplizieren in die neue Entität Daten mit einer Anforderung abrufen können. Jedoch können nicht Sie starke Konsistenz eine EGT atomar aktualisiert die beiden Entitäten verwenden können.  

#### <a name="solution"></a>Lösung
Speichern Sie einen neuen Entitätstyp in der Originaltabelle mit Entitäten mit der folgenden Struktur:  

![][20]

Beachten Sie, dass die **RowKey** jetzt einen zusammengesetzten Schlüssel die Mitarbeiter-Id und das Jahr der Daten, mit dem die Leistung abzurufen und Daten mit einer Anforderung für eine einzelne Entität, besteht.  

Im folgenden Beispiel wird erläutert, wie die Überprüfung Daten für einen bestimmten Mitarbeiter (z. B. 000123 Mitarbeiter in der Vertriebsabteilung) abrufen können:  

$filter = (PartitionKey Eq "Sales") und (RowKey ge 'empid_000123') und (RowKey 'empid_000124') & $select = RowKey Manager Rating Peer Bewertung, Kommentare  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte
Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Verwenden Sie geeignete Trennzeichen, die den **RowKey** -Wert analysieren erleichtert: z. B. **000123_2012**.  
-   Speichern Sie diese Entität auch in der gleichen Partition wie andere Entitäten mit Daten für denselben Mitarbeiter die bedeutet, dass Sie mithilfe von EGTs starke Konsistenz.
-   Sie sollten berücksichtigen, wie oft Sie die Daten, um zu bestimmen, ob dieses Muster eignet Abfragen.  Z. B. Wenn Sie die Daten nur selten und die wichtigsten Daten häufig zugreifen sollten Sie sie als separate Elemente beibehalten.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster, Sie müssen eine oder mehrere verwandte Elemente, wenn Sie Abfragen häufig.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Entität Transaktionen](#entity-group-transactions)  
-   [Arbeiten mit heterogenen Entitätstypen](#working-with-heterogeneous-entity-types)  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Log Tail-Muster  

Abrufen von *n* Elemente einer Partition mit einem **RowKey** -Wert, der in umgekehrter Datum und Zeit sortiert zuletzt hinzugefügt wurde.  

#### <a name="context-and-problem"></a>Kontext und problem  

Häufig wird das zuletzt erstellte Elemente abrufen, beispielsweise die letzten zehn Anträge von einem Mitarbeiter Kosten. Abfragen unterstützen eine Abfrageoperation **$top** um die ersten *n* Elemente aus einer Menge zurück: Gibt es keine entsprechenden Vorgang die letzten n Elemente in einer Menge zurück.  

#### <a name="solution"></a>Lösung  

Speichern Sie Elemente **RowKey** , die natürlich sortiert in umgekehrter Zeitpunkt Reihenfolge entsprechend den letzte Eintrag immer die erste in der Tabelle ist.  

Können verwenden Sie um die zehn neuesten Ausgaben Anträge von einem Mitarbeiter abrufen, eine umgekehrte Teilstrichwert der aktuellen Zeit abgeleitet. Im folgende C#-Codebeispiel zeigt eine Möglichkeit, einen geeigneten "invertiert Ticks" Wert für **RowKey** erstellen, von der letzten sortiert, die ältesten:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Sie können zurück zum Zeitwert Datum mithilfe des folgenden Codes abrufen:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Die Table-Abfrage sieht folgendermaßen aus:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Müssen Sie Auffüllen den umgekehrten Tick-Wert mit vorangestellten Nullen um sicherzustellen, dass der Zeichenfolgenwert wie erwartet sortiert.  
-   Sie müssen der Skalierbarkeitsziele auf der Partition berücksichtigen. Achten Sie Hotspots Partitionen.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster, wenn Sie müssen Zugriff auf Entitäten reverse Zeitpunkt oder wenn Sie die zuletzt hinzugefügten Elemente zugreifen müssen.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Vorangestellt / append Anti-Muster](#prepend-append-anti-pattern)  
-   [Abrufen von Entitäten](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Hohes Volumen Löschmuster  

Zum Aktivieren der Löschung eine große Anzahl von Elementen alle Elemente gleichzeitig löschen in eigenen separaten Tabellen gespeichert; Löschen Sie die Elemente löschen der Tabelle.  

#### <a name="context-and-problem"></a>Kontext und problem  

Vielen Löschen alter dem muss nicht mehr für eine Clientanwendung zur Verfügung oder die Anwendung auf ein anderes Speichermedium archiviert. Diese Daten normalerweise identifizieren, einem Datum: Angenommen, Sie müssen alle Login Clientanforderungen Datensätze löschen, die älter als 60 Tage sind.  

Ein Entwurf ist das Datum und die Zeit der Anmeldung in **RowKey**:  

![][21]

Dies vermeidet Partition Hotspots, da Anwendung kann einfügen und Löschen von Entitäten Login für jeden Benutzer in einer separaten Partition. Dieser Ansatz kann jedoch teuer und zeitaufwändig haben Sie eine große Anzahl von Entitäten müssen Sie zunächst einen Tabellenscan auszuführen, um die zu löschenden Entitäten zu identifizieren und Sie müssen jede alte Entität löschen. Beachten Sie, dass die Anzahl der Roundtrips zum Server erforderlich, um alte Objekte Löschen von mehreren Löschvorgängen in EGTs Batchverarbeitung reduzieren können.  

#### <a name="solution"></a>Lösung  

Verwenden Sie eine separate Tabelle für jeden Tag der Anmeldeversuche. Können Sie den Entität Entwurf oben um Hotspots zu vermeiden, wenn Sie Entitäten einfügen und Löschen von alten Einheiten jetzt einfach Löschen einer Tabelle täglich kann (einzelne Speichervorgang) statt finden und löschen jeden Tag Hunderte und Tausende von einzelnen Benutzernamen Entitäten.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Unterstützt der Entwurf andere Weise die Anwendung die Daten wie bestimmte Entitäten mit anderen Daten oder Generieren von zusammengefassten Daten verwenden?  
-   Vermeidet Ihr Entwurf Hotspots beim Einfügen neuer?  
-   Erwarten Sie eine Verzögerung, wenn Sie denselben Tabellennamen verwenden, nach dem löschen möchten. Es empfiehlt sich immer eindeutige Namen verwenden.  
-   Erwarten Sie einige Einschränkung, wenn Sie zuerst eine neue Tabelle verwenden, und der Dienst die Zugriffsmuster lernt Partitionen über Knoten verteilt. Wie häufig müssen Sie neue Tabellen erstellen sollten.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster Wenn eine große Anzahl von Elementen, die gleichzeitig löschen müssen.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Entität Transaktionen](#entity-group-transactions)
-   [Ändern von Entitäten](#modifying-entities)  

### <a name="data-series-pattern"></a>Serie Datenmuster  

Speicher Datenreihen in einer Entität zu machen Sie die Anzahl der Anfragen abgeschlossen  

#### <a name="context-and-problem"></a>Kontext und problem  

Ein häufiges Szenario ist für eine Anwendung Daten speichern, die sie normalerweise gleichzeitig abrufen. Die Anwendung kann z. B. wie viele Sofortnachrichten jeder Mitarbeiter pro Stunde sendet aufzeichnen und dann mithilfe dieser Informationen, wie viele Nachrichten zeichnen jeden Benutzer über den vorhergehenden 24 Stunden gesendet. Ein Entwurf könnten 24 Entitäten für jeden Mitarbeiter speichern:  

![][22]

Mit diesem Entwurf können Sie problemlos suchen und aktualisieren die Entität für jeden Mitarbeiter zu aktualisieren, wenn die Anwendung den anzahlwert aktualisieren muss. Allerdings müssen Sie zum Abrufen von Informationen zum Zeichnen eines Diagramms der Aktivität für die letzten 24 Stunden 24 Elemente abrufen.  

#### <a name="solution"></a>Lösung  

Mit der folgenden Design mit einer separaten Eigenschaft um die Nachrichtenanzahl für jede Stunde zu speichern:  

![][23]

Einem Vorgang können Sie mit diesem Entwurf Nachrichtenanzahl für einen Mitarbeiter für eine bestimmte Stunde aktualisiert. Jetzt können Sie die Informationen benötigen Sie das Diagramm mit einer Anforderung für eine einzelne Entität darstellen abrufen.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  
-   Passt die vollständige Datenreihe nicht in einer einzelnen Entität (eine Entität kann bis zu 252 Eigenschaften aufweisen), verwenden Sie einen alternativen Datenspeicher wie ein Blob.  
-   Haben Sie mehrere Clients gleichzeitig aktualisieren einer Entität, müssen Sie das **ETag** Parallelität implementiert verwenden. Sie haben viele Kunden auftreten Konfliktpotential.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster, aktualisieren und Abrufen einer Datenreihe, einer einzelnen Entität zugeordnet werden sollen.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Großer Entitäten Muster](#large-entities-pattern)  
-   [Zusammenführen oder ersetzen](#merge-or-replace)  
-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) (Wenn Sie die Datenreihe in ein Blob speichern)  

### <a name="wide-entities-pattern"></a>Große Unternehmen Muster  

Verwenden Sie mehrere physische Entitäten logische Entitäten mit mehr als 252 Eigenschaften speichern.  

#### <a name="context-and-problem"></a>Kontext und problem  

Eine einzelne Entität kann nicht mehr als 252 Eigenschaften (außer der obligatorischen Systemeigenschaften) und kann nicht mehr als 1 MB Daten insgesamt speichern. In einer relationalen Datenbank in der Regel erhalten um Grenzwerte für die Größe einer Zeile Sie eine neue Tabelle hinzufügen und eine 1: 1 Beziehung erzwingen.  

#### <a name="solution"></a>Lösung  

Sie können den Dienst mehrere Entitäten stellen ein Objekt einzelne große Unternehmen mit mehr als 252 Eigenschaften speichern. Z. B. Wenn Sie die Anzahl der IM Nachrichten von jedem Mitarbeiter der letzten 365 Tage speichern möchten, können das folgende Design Sie, das zwei Entitäten mit unterschiedlichen Schemas verwendet:  

![][24]

Möchten Sie eine Änderung vornehmen, die erfordert, dass beide Entitäten zu halten aktualisieren miteinander synchronisiert können Sie eine EGT. Einem einzelnen Vorgang können Sie andernfalls die Anzahl der Nachrichten für einen bestimmten Tag aktualisieren. Zum Abrufen der Daten für einen einzelnen Mitarbeiter müssen Sie beide Entitäten abrufen, Sie mit zwei effizienter, die **PartitionKey** und **RowKey** -Wert verwenden.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Mindestens zwei Speichertransaktionen umfasst eine vollständige logische Entität abrufen: einen einzelnen physischen Entität abzurufen.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Dieses Muster bei muss Entitäten gespeichert, deren Größe oder Anzahl der Eigenschaften die Grenzen für eine einzelne Entität in der Tabelle Service überschreitet.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Entität Transaktionen](#entity-group-transactions)
-   [Zusammenführen oder ersetzen](#merge-or-replace)

### <a name="large-entities-pattern"></a>Großer Entitäten Muster  

Verwenden Sie BLOB-Speicher zum Speichern großer Werte.  

#### <a name="context-and-problem"></a>Kontext und problem  

Eine einzelne Entität kann nicht mehr als 1 MB Daten insgesamt speichern. Wenn eine oder mehrere Ihrer Eigenschaften Werte, die die Gesamtgröße der Entität speichern, die diesen Wert überschreiten, verursachen, kann nicht die gesamte Entität in der Tabelle Service speichern.  

#### <a name="solution"></a>Lösung  

Wenn die Entität 1 MB Größe überschreitet, da eine oder mehrere Eigenschaften eine große Menge von Daten enthalten, können Datenspeicher im Blob-Dienst und speichern Sie die Adresse des BLOBs in eine Eigenschaft der Entität. Beispielsweise können Sie das Foto des Mitarbeiters im BLOB-Speicher speichern und speichern einen Link zu Fotos in der **Foto** -Eigenschaft der Employee-Entität:  

![][25]

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Um eventuelle Konsistenz zwischen der Entität in der Tabelle Service und die Daten im Blob-Dienst verwalten, verwenden Sie [schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern) Entitäten verwalten.
-   Abrufen einer vollständigen Entität umfasst mindestens zwei Speichertransaktionen: eine Entität und eine zum Abrufen von BLOB-Daten abzurufen.  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Verwenden Sie dieses Muster, wenn Entitäten gespeichert, deren Größe die Grenzen für eine einzelne Entität in der Tabelle Service überschreitet.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Schließlich konsistent Transaktionen Muster](#eventually-consistent-transactions-pattern)  
-   [Große Unternehmen Muster](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>
### <a name="prependappend-anti-pattern"></a>Antimuster voran/anhängen  

Erhöhen Sie Skalierbarkeit, wenn umfangreiche eingefügt haben, indem die fügt auf mehrere Partitionen.  

#### <a name="context-and-problem"></a>Kontext und problem  

Voranstellen oder Anfügen von Entitäten an gespeicherten Entitäten normalerweise führt die Anwendung die erste oder letzte Partition einer Sequenz von Partitionen neue Elemente hinzufügen. In diesem Fall statt alle fügt gleichzeitig in der gleichen Partition Erstellen eines Hotspots, das vom Netzwerklastenausgleich fügt über mehrere Knoten und was die Anwendung der Skalierbarkeit Ziele für Partition der Tabelle verhindert. Beispielsweise haben Sie eine Anwendung das Netzwerk anmeldet und Zugriff auf Ressourcen von Mitarbeitern, dann eine Entitätsstruktur wie folgt könnte die aktuelle Stunde Partition zu einem Hotspot, wenn das Volumen der Transaktionen Skalierbarkeit für eine einzelne Partition erreicht:  

![][26]

#### <a name="solution"></a>Lösung  

Die folgenden alternativen Entitätsstruktur vermeidet einen Hotspot auf einer bestimmten Partition als Anwendung protokolliert Ereignisse:  

![][27]

Beachten Sie dass in diesem Beispiel die **PartitionKey** und **RowKey** Verbundschlüssel sind. **PartitionKey** verwendet die Abteilung und die Mitarbeiter-Id die Protokollierung auf mehrere Partitionen verteilt.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei Verwendung dieses Muster implementiert:  

-   Unterstützt alternative Schlüssel Struktur, die effiziente Erstellung von hot Partitionen auf fügt vermeidet Abfragen die Clientanwendung macht?  
-   Heißt die erwartete Anzahl von Transaktionen Sie dürften Zielvorgaben Skalierbarkeit für eine einzelne Partition und vom Speicherdienst gedrosselt werden?  

#### <a name="when-to-use-this-pattern"></a>Dieses Muster verwenden  

Vermeiden Sie die Anzahl von Transaktionen ist beim Zugriff auf einer aktiven Partition vom Speicherdienst Drosselung führen Prepend/append-Antimuster.  

#### <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung  

Die folgenden Muster und können auch relevant, wenn dieses Muster implementiert:  

-   [Zusammengesetzte Schlüssel Muster](#compound-key-pattern)  
-   [Log Tail-Muster](#log-tail-pattern)  
-   [Ändern von Entitäten](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Anti-Protokoll Datenmuster  

In der Regel verwenden den BLOB-Dienst anstelle der Dienst Sie Protokolldaten gespeichert.  

#### <a name="context-and-problem"></a>Kontext und problem  

Ein allgemeinen Anwendungsfall Protokolldaten ist eine Auswahl der Protokolleinträge an einem bestimmten Zeitpunkt abgerufen: z. B. alle Fehler und wichtige Nachrichten, die zwischen 15:04 und 15:06 an einem bestimmten Datum die Anwendung protokolliert werden soll. Möchten, nicht mit Datum und Uhrzeit die Nachricht die Partition ermittelt Entitäten Protokoll speichern: denn zu jedem Zeitpunkt die Protokoll-Elemente denselben **PartitionKey** -Wert in einer aktiven Partition führt (siehe Abschnitt [Antimusters Prepend/append](#prepend-append-anti-pattern)). Beispielsweise führt das folgende Entitätsschema für eine Nachricht protokollieren in einer aktiven Partition, da alle Lognachrichten auf die Partition für das aktuelle Datum und die Uhrzeit geschrieben:  

![][28]

In diesem Beispiel **RowKey** enthält das Datum und Uhrzeit der Nachricht protokollieren, dass Protokollnachrichten gespeichert sind Zeitpunkt sortiert und enthält eine Nachrichten-Id bei mehreren Protokollnachrichten dasselbe Datum und dieselbe Uhrzeit freigeben.  

Ein anderer Ansatz ist die Verwendung **PartitionKey** , die sicherstellt, dass Nachrichten in verschiedenen Partitionen geschrieben. Stellt die Quelle die Nachricht Nachrichten über viele Partitionen verteilen, könnte Sie folgenden Entitätsschema verwenden:  

![][29]

Das Problem mit diesem Schema werden, rufen Sie die Protokolleinträge für eine bestimmte Zeitspanne jeder Partition in der Tabelle durchsuchen müssen.

#### <a name="solution"></a>Lösung  

Im vorherige Abschnitt markiert das Problem mit dem Dienst um Protokolleinträge und vorgeschlagene zwei nicht zufriedenstellend, Entwürfe zu speichern. Eine Lösung zur aktiven Partition mit Leistungseinbußen Protokoll Nachrichten; die Lösung führte schlechte Leistung aufgrund der Anforderung Scannen jeder Partition in der Tabelle Protokollnachrichten für einen bestimmten Zeitraum abgerufen. BLOB-Speicher bietet eine bessere Lösung für diese Art von Szenario, und dies ist wie Azure Speicheranalyse speichert die Protokolldaten sammelt.  

Dieser Abschnitt beschreibt, wie Speicheranalyse speichert Daten im BLOB-Speicher zur Veranschaulichung dieses Ansatzes zum Speichern von Daten, die normalerweise vom Bereich Abfragen.  

Speicheranalyse speichert Nachrichten protokollieren in ein Format mit Trennzeichen in mehrere Blobs. Getrennte Format erleichtert eine Clientanwendung zum Analysieren der Daten in die Nachricht.  

Speicheranalyse verwendet eine Benennungskonvention für Blobs, die BLOBs suchen können (oder Blobs), die die protokollierten Nachrichten enthalten, nach denen Sie suchen. Ein Blob mit dem Namen "queue/2014/07/31/1800/000001.log" enthält z. B. Protokollnachrichten Warteschlangendienst Stunde ab 18:00 Uhr am 31. Juli 2014 beziehen. "000001" gibt an, dass dies die erste Protokolldatei für diesen Zeitraum. Speicheranalyse zeichnet auch die Timestamps der ersten und letzten Protokolldatei Nachrichten als Teil der Blob-Metadaten in der Datei gespeichert. Die API für BLOB-Speicher ermöglicht Blobs in einem Container basierend auf einem Präfix suchen: alle Blobs finden, die für die Stunde ab 18 Uhr Warteschlangendaten enthalten, können Sie das Präfix "Warteschlange/2014/07/31/1800".  

Analytics Speicherpuffer protokollieren intern und dann regelmäßig entsprechende Blob aktualisiert oder erstellt eine neue mit der neuesten Einträge. Dies reduziert die Anzahl der Schreibvorgänge für den BLOB-Dienst ausführen müssen.  

Wenn Sie in Ihrer Anwendung eine ähnliche Lösung implementieren, müssen Sie berücksichtigen Kompromiss zwischen Zuverlässigkeit (und schreiben wie jedes Protokolleintrags Blob-Speicher) und Kosten und skalierbar (Pufferung Updates in Ihrer Anwendung und Blob-Speicher im Stapel schreiben) verwalten.  

#### <a name="issues-and-considerations"></a>Probleme und Aspekte  

Beachten Sie Folgendes bei wie Protokolldaten gespeichert:  

-   Erstellen einer Tabellenentwurf, der potenzielle hot Partitionen vermeidet finden Sie Ihre Daten effizient zugreifen.  
-   Um Daten zu verarbeiten, muss ein Client häufig viele Datensätze laden.  
-   Obwohl Daten häufig strukturiert ist, möglicherweise BLOB-Speicher besser.  

### <a name="implementation-considerations"></a>Aspekte der Implementierung  

Dieser Abschnitt beschreibt einige der Aspekte berücksichtigen, wenn Sie in den vorherigen Abschnitten beschriebenen Muster implementieren. Diesem Abschnitt stellt in C# geschrieben, mit dem der Speicher-Clientbibliothek (Version 4.3.0 gleichzeitig schreiben).  

### <a name="retrieving-entities"></a>Abrufen von Entitäten  

Wie im Abschnitt [Entwurf für Abfragen](#design-for-querying), ist die effizienteste Abfrage eine zeigen. In einigen Szenarien müssen Sie jedoch mehrere Elemente abrufen. Dieser Abschnitt beschreibt einige gemeinsame Entitäten die Clientbibliothek Speicher abrufen.  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a>Mit der Speicher-Clientbibliothek Punktabfrage ausführen  

Die einfachste Möglichkeit zum Ausführen einer Punktabfrage ist die Verwendung von Tabelle **Abrufen** Vorgang wie in der folgende C#-Codeausschnitt gezeigt, die eine Entität mit einer **PartitionKey** der Wert "Sales" und eine **RowKey** der Wert "212" abgerufen:  

    TableOperation retrieveOperation =
        TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
    var retrieveResult = employeeTable.Execute(retrieveOperation);
    if (retrieveResult.Result != null)
    {
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
    }  

Beachten Sie, wie in diesem Beispiel wird die Entität erwartet Typ **EmployeeEntity**abgerufen.  

#### <a name="retrieving-multiple-entities-using-linq"></a>Abrufen von mehreren Personen mit LINQ  

Sie können mehrere Entitäten abrufen, von LINQ mit Speicher-Clientbibliothek und eine Abfrage mit einer **where** -Klausel angeben. Um einen Tabellenscan zu vermeiden, sollte immer aufnehmen **PartitionKey** -Wert in der Where-Klausel und möglichst **RowKey** Wert Tabelle und Partition überprüft. Der Dienst unterstützt eine begrenzte Anzahl von Vergleichsoperatoren (größer als, größer als oder gleich, kleiner als, kleiner als oder gleich, gleich und ungleich) für die Verwendung in der Klausel. Der folgende C#-Codeausschnitt Ruft alle Mitarbeiter, deren Nachname mit "B beginnt" (vorausgesetzt, der **RowKey** speichert den Nachnamen) in der Vertriebsabteilung (vorausgesetzt das **PartitionKey** speichert den Abteilungsnamen):  

    TableQuery<EmployeeEntity> employeeQuery =
            employeeTable.CreateQuery<EmployeeEntity>();
    var query = (from employee in employeeQuery
                where employee.PartitionKey == "Sales" &&
                employee.RowKey.CompareTo("B") >= 0 &&
                employee.RowKey.CompareTo("C") < 0
                select employee).AsTableQuery();
    var employees = query.Execute();  

Beachten Sie, wie die Abfrage **RowKey** und **PartitionKey** bessere Leistung angibt.  

Der folgende Code-Beispiel zeigt äquivalente mithilfe der fluent-API (für Weitere Informationen über fluent-APIs im Allgemeinen finden Sie [Optimale Methoden zum Entwerfen einer Fluent-API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

    TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
     TableQuery.CombineFilters(
        TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition(
        "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
    ),
    TableOperators.And,
    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
     )
    );
    var employees = employeeTable.ExecuteQuery(employeeQuery);  


>[AZURE.NOTE] Das Beispiel schachtelt mehrere **CombineFilters** Methoden gehören die drei Filter.  

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Abrufen einer großen Anzahl von Elementen aus einer Abfrage  

Eine optimale Abfrage gibt ein einzelnes Objekt basierend auf einem **PartitionKey** und **RowKey** -Wert. Jedoch in einigen Szenarios erforderlich, viele Elemente von der gleichen Partition oder Partitionen zurückgeben möglicherweise.  

In solchen Situationen sollten Sie immer die Leistung der Anwendung testen.  

Eine Abfrage für den Dienst möglicherweise bis zu 1.000 Elemente gleichzeitig und kann maximal fünf Sekunden ausgeführt. Resultset mehr als 1.000 Entitäten enthält, wenn die Abfrage nicht innerhalb von fünf Sekunden abgeschlossen wurde oder die Abfrage Partition Grenze überschreitet, gibt der Dienst Fortsetzungstoken aktivieren die Clientanwendung den nächsten Satz von Entitäten anfordern. Weitere Informationen wie Fortsetzung Arbeit Token finden Sie [Abfragetimeout und Paginierung](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Wenn Sie Speicher-Clientbibliothek verwenden, können sie automatisch Fortsetzungstoken behandelt Entitäten aus dem Dienst zurück. Im folgende C# -[NULL] Beispiel mit Speicher-Clientbibliothek automatisch behandelt Fortsetzungstoken, wenn der Dienst eine Antwort zurückgibt:  

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

    var employees = employeeTable.ExecuteQuery(employeeQuery);
    foreach (var emp in employees)
    {
        ...
    }  

Im folgende C#-Code behandelt Fortsetzungstoken explizit:  

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

    TableContinuationToken continuationToken = null;

    do
    {
        var employees = employeeTable.ExecuteQuerySegmented(
            employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
     ...
    }
    continuationToken = employees.ContinuationToken;
    } while (continuationToken != null);  

Fortsetzungstoken explizit verwenden, können Sie steuern, wann die Anwendung das nächste Segment der Daten abruft. Beispielsweise wenn Ihre Clientanwendung Blättern in einer Tabelle gespeicherten Elemente kann können Benutzer nicht alle Elemente von der Abfrage abgerufen werden, damit die Anwendung das nächste Segment abgerufen, wenn das Paging für alle Elemente im aktuellen Bereich beendet wurde nur ein Fortsetzungstoken verwenden durchblättern. Dieser Ansatz hat mehrere Vorteile:  

-   Sie können Daten aus dem Dienst zu beschränken und über das Netzwerk verschieben.  
-   Sie können asynchrone e/a in .NET auszuführen.  
-   Sie können das Fortsetzungstoken dauerhaft zu serialisieren, damit bei einem Absturz der Anwendung fortgesetzt werden kann.  

>[AZURE.NOTE] Ein Fortsetzungstoken gibt in der Regel ein Segment mit 1.000 Einheiten weniger sein kann. Dies trifft auch zu, wenn die Anzahl der Einträge einschränken eine Abfrage **dauern gibt** die ersten n Elemente zurück, die mit Ihren Suchkriterien übereinstimmen: der Dienst möglicherweise ein Segment mit weniger als n Entitäten mit Fortsetzungstoken können die verbleibenden Elemente abgerufen.  

Im folgende C#-Code zeigt, wie die Anzahl der innerhalb eines Segments zurückgegebenen Entitäten ändern:  

    employeeQuery.TakeCount = 50;  

#### <a name="server-side-projection"></a>Serverseitige Projektion  

Eine einzelne Entität kann bis zu 255 und bis zu 1 MB Größe. Wenn Sie die Tabelle und Entitäten abrufen, möglicherweise nicht alle Eigenschaften und können vermeiden, dass Daten unnötig (Latenz und Kosten). Serverseitige Projektion können, Sie müssen, nur die Eigenschaften übertragen. Im folgende Beispiel ist nur die **Email-** Eigenschaft (mit **PartitionKey**, **RowKey**, **Timestamp**und **ETag**) ruft der Entitäten, die von der Abfrage ausgewählt.  

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    List<string> columns = new List<string>() { "Email" };
    TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

    var entities = employeeTable.ExecuteQuery(employeeQuery);
    foreach (var e in entities)
    {
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
    }  

Beachten Sie, dass der **RowKey** -Wert verfügbar, obwohl sie nicht in der Liste der abzurufenden Eigenschaften enthalten ist.  

### <a name="modifying-entities"></a>Ändern von Entitäten  

Der Speicher-Clientbibliothek können Sie Ihre Entitäten in der Tabelle Service durch Einfügen, löschen und Aktualisieren von Entitäten gespeichert ändern. Sie können EGTs Stapel mehrere Insert, Update und Delete-Operationen zu weniger Roundtrips erforderlich und die Leistung Ihrer Lösung.  

Beachten Sie, dass Ausnahmen ausgelöst, wenn der Speicher-Clientbibliothek normalerweise eine EGT führt den Index der Entität, die den Stapel nicht verursacht. Dies ist hilfreich, wenn Sie Code Debuggen, der EGTs verwendet.  

Sie sollten berücksichtigen, wie Ihr Design beeinflusst die Behandlung die Clientanwendung Parallelität und Aktualisierungsvorgänge.  

#### <a name="managing-concurrency"></a>Verwalten von Parallelität  

Implementiert der Dienst vollständige Parallelität auf der Ebene der einzelnen Elemente **Einfügen**, **Merge**und **Delete** Operations überprüft zwar ein Client zum Erzwingen der Tabelle diese Kontrollen zu umgehen. Weitere Informationen zur Verwaltung der Parallelität durch des Diensts finden Sie unter [Managing Concurrency in Microsoft Azure-Speicher](storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Zusammenführen oder ersetzen  

Die **Replace** -Methode der **TableOperation** -Klasse ersetzt immer vollständige Entität in der Tabelle Service. Wenn diese Eigenschaft in gespeicherte Entität vorhanden ist keine Eigenschaft in der Anforderung enthalten sind, entfernt die Anforderung Eigenschaft gespeicherte Entität. Wenn explizit Entfernen einer Eigenschaft aus gespeichert werden soll, müssen Sie für jede Eigenschaft in der Anforderung angeben.  

**Merge** -Methode der **TableOperation** -Klasse können Sie die Datenmenge reduzieren, die an den Dienst senden, wenn eine Entität aktualisieren möchten. **Merge** -Methode ersetzt alle Eigenschaften in der gespeicherten Entität mit Eigenschaftswerten aus der Entität in der Anforderung enthalten jedoch intakt Eigenschaften in der gespeicherten Entität, die nicht in der Anforderung enthalten sind. Dies ist nützlich, wenn Sie große Elemente und nur wenige Eigenschaften einer Anforderung aktualisieren müssen.  

>[AZURE.NOTE] Die Methoden **Ersetzen** und **Zusammenführen** fehl, wenn die Einheit nicht vorhanden ist. Alternativ können Sie die Methoden **InsertOrReplace** und **InsertOrMerge** , die eine neue Entität erstellen, falls er vorhanden ist.  

### <a name="working-with-heterogeneous-entity-types"></a>Arbeiten mit heterogenen Entitätstypen  

Der Dienst ist ein *Schema weniger* Tabellenspeicher, der eine einzelne Tabelle Entitäten verschiedene große Flexibilität in Ihrem Entwurf speichern kann. Das folgende Beispiel veranschaulicht eine Tabelle Employee und abteilungsentitäten gespeichert:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zeitstempel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Beachten Sie, dass jede Entität muss noch **PartitionKey**, **RowKey**und **Timestamp** -Werte muss eine Reihe von Eigenschaften. Darüber hinaus geht der Typ einer Entität an, sofern Sie diese Information an. Es gibt zwei Optionen zum Identifizieren des Entität vom Typs:  

-   Den Entitätstyp der **RowKey** (oder **PartitionKey**) vorangestellt. **EMPLOYEE_000123** oder **DEPARTMENT_SALES** als **RowKey** -Werten.  
-   Verwenden Sie eine separate Eigenschaft Entitätstyps aufzeichnen, wie in der folgenden Tabelle dargestellt.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zeitstempel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Mitarbeiter</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Mitarbeiter</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Abteilung</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Vorname</th>
<th>Nachname</th>
<th>ALTER</th>
<th>E-Mail</th>
</tr>
<tr>
<td>Mitarbeiter</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Die erste Option vorangestellt wird des Entitätstyps **RowKey**eignet sich besteht die Möglichkeit, zwei Einheiten verschiedener Typen möglicherweise denselben Schlüsselwert. Außerdem gruppiert Entitäten des gleichen Typs in der Partition.  

In diesem Abschnitt beschriebenen Techniken sind insbesondere die Diskussion [vererbungsbeziehungen](#inheritance-relationships) in diesem Handbuch im Abschnitt [Modellierung Beziehungen](#modelling-relationships).  

>[AZURE.NOTE] Sie sollten berücksichtigen, einschließlich der Versionsnummer in der Entität Typwert ermöglichen Clientanwendungen zu entwickeln POCO-Objekte mit unterschiedlichen Versionen.  

Der Rest dieses Abschnitts beschreibt einige der Funktionen in der Speicher-Clientbibliothek, die Arbeiten mit mehreren Entitätstypen in derselben Tabelle.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Abrufen von heterogenen Entitätstypen  

Speicher-Clientbibliothek verwenden, haben Sie drei Optionen für die Arbeit mit mehreren Entitätstypen.  

Sie kennen die Entität mit einem bestimmten **RowKey** und **PartitionKey** gespeichert, wenn den Entitätstyp können bei die Entität abzurufen, wie in den beiden vorhergehenden Beispielen dargestellt, die Elemente des Typs **EmployeeEntity**abrufen: [einer Abfrage zeigen die Speicher-Clientbibliothek verwenden](#executing-a-point-query-using-the-storage-client-library) und [mehrere Entitäten mit LINQ abrufen](#retrieving-multiple-entities-using-linq).  

Die zweite Option ist mit der **DynamicTableEntity** -Typ (eine Eigenschaftensammlung) statt einer konkreten POCO-Entitätstyp (diese Option kann auch Leistungssteigerung, da besteht keine Notwendigkeit zum Serialisieren und Deserialisieren der Entität zu). Im folgende C#-Code möglicherweise mehrere Elemente verschiedener Typen aus der Tabelle abgerufen, gibt jedoch alle Entitäten als **DynamicTableEntity** Instanzen. Es wird mithilfe der **EntityType** -Eigenschaft jeder Objekttyp bestimmen:  

    string filter =     TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey",
      QueryComparisons.Equal, "Sales"),
        TableOperators.And,
        TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey",
          QueryComparisons.LessThan, "F")
        )
    );
    TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
    var employees = employeeTable.ExecuteQuery(entityQuery);

    IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
    foreach (var e in entities)
    {
    EntityProperty entityTypeProperty;
    if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
    {
        if (entityTypeProperty.StringValue == "Employee")
        {
            // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
          }
     }
    }  

Beachten Sie, dass andere Eigenschaften rufen Sie **TryGetValue** -Methode auf die **Properties** -Eigenschaft der **DynamicTableEntity** -Klasse.  

Eine dritte Option ist mit der **DynamicTableEntity** -Typ und Instanz **EntityResolver** kombinieren. Dadurch werden mehrere POCO-Typen in der gleichen Abfrage aufgelöst. In diesem Beispiel verwendet der Delegaten **EntityResolver** **EntityType** -Eigenschaft zwischen zwei Unternehmen unterscheiden, die die Abfrage zurückgibt. Methode **beheben** verwendet **Resolver** Delegaten zu **DynamicTableEntity** Instanzen **TableEntity** Instanzen.  

    EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
    {

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
            resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
            resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
    };

    string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
    TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

    var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
    foreach (var e in entities)
    {
        if (e is DepartmentEntity)
        {
        ...
        }
        if (e is EmployeeEntity)
        {
        ...
        }
    }  

#### <a name="modifying-heterogeneous-entity-types"></a>Heterogene Entitätstypen ändern  

Sie müssen nicht den Typ einer Entität zu löschen, und Sie immer den Typ einer Entität einfügen. **DynamicTableEntity** Typ können Sie jedoch eine Entität ohne seinen Typ und ohne eine Entitätsklasse POCO aktualisieren. Das folgende Codebeispiel ruft eine Einheit und überprüft die **EmployeeCount** Eigenschaft vor der Aktualisierung vorhanden ist.  

    TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
    DynamicTableEntity department = (DynamicTableEntity)result.Result;

    EntityProperty countProperty;

    if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
    {
        throw new
            InvalidOperationException("Invalid entity, EmployeeCount property not found.");
    }
    countProperty.Int32Value += 1;
    employeeTable.Execute(TableOperation.Merge(department));  

### <a name="controlling-access-with-shared-access-signatures"></a>Steuern des Zugriffs mit Shared Access Signatures  

Shared Access Signatur (SAS) Token können um Clientanwendungen zu ändern (Abfragen) Tabellenentitäten direkt ohne Authentifizierung direkt mit dem Dienst zu aktivieren. Normalerweise sind drei Hauptvorteile von SAS in der Anwendung verwenden:  

-   Sie müssen nicht Ihr Konto Speicherschlüssel unsicher Plattform (z. B. Mobilgeräten) verteilen, um dieses Gerät Entitäten in der Tabelle Service zu zugreifen.  
-   Sie können die Arbeit Auslagern Web-und Workerrollen ausführen Entitäten Clientgeräte wie Endbenutzer-Computer und mobile Geräte verwalten.  
-   Können eine eingeschränkte oder zeitlich begrenzte Berechtigungen an einen Client (z. B. ermöglicht schreibgeschützten Zugriff auf bestimmte Ressourcen).  

Weitere Informationen zur Verwendung von SAS-Token mit dem Dienst finden Sie unter [Verwenden gemeinsame Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).  

Jedoch müssen Sie noch SAS-Token, die eine Clientanwendung für die Entitäten in der Tabelle Service gewähren generieren: sollte in einer Umgebung mit sicheren Zugriff auf Ihr Konto Speicherschlüssel dazu. Normalerweise verwenden Sie SAS-Token und Clientanwendungen, die Zugriff auf Ihre Elemente bieten Web oder Worker-Rolle. Da noch ein Mehraufwand generieren und SAS-Token zu Kunden wie sollten zu diesen Aufwand, besonders in Szenarien mit hohen Volumina beteiligt.  

Es ist möglich, ein SAS-Token zu generieren, die Zugriff auf eine Teilmenge der Elemente in einer Tabelle. Standardmäßig erstellt einen SAS-Token für die gesamte Tabelle, aber kann auch angeben, dass das SAS-Token **PartitionKey** einen Wertebereich oder einen Wertebereich **PartitionKey** und **RowKey** gewähren. Sie können SAS-Tokens für einzelne Benutzer des Systems zu generieren, SAS-Token des Benutzers nur auf ihre eigenen Entitäten in der Tabelle Service kann.  

### <a name="asynchronous-and-parallel-operations"></a>Asynchrone und parallele Vorgänge  

Sofern Sie Ihre Anfragen über mehrere Partitionen zuweisen, können Sie mithilfe von asynchronen oder parallele Abfragen Durchsatz und Client Reaktionsfähigkeit verbessern.
Beispielsweise müssen Sie mindestens zwei Workerrolleninstanzen Tabellen gleichzeitig zugreifen. Sie können Einzelunternehmer Rollen zuständig für bestimmte Gruppen von Partitionen oder einfach mehrere Workerrollen-Instanzen, die Partitionen einer Tabelle zugreifen.  

In einer Clientinstanz können Sie Speichervorgänge asynchron ausführen Durchsatz verbessern. Der Speicher-Clientbibliothek vereinfacht die asynchrone Abfragen und ändern. Beispielsweise beginnen Sie mit der synchronen Methode, die alle Entitäten in einer Partition wie im folgenden C#-Code abgerufen:  

    private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
    {
        string filter = TableQuery.GenerateFilterCondition(
            "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
            new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
            var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
            foreach (var emp in employees)
        {
        ...
        }
            continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
    }  

Dieser Code kann problemlos ändern, sodass die Abfrage asynchron wie folgt ausgeführt:  

    private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
    {
        string filter = TableQuery.GenerateFilterCondition(
            "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
            new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
            var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
            foreach (var emp in employees)
            {
             ...
            }
            continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
    }  

In diesem Beispiel asynchrone sehen Sie folgende Änderung zur synchronen Version:  

-   Die Methodensignatur jetzt **Async** -Modifizierer enthalten und gibt eine Instanz der **Aufgabe** zurück.  
-   Anstatt die **ExecuteSegmented** -Methode, um Ergebnisse abzurufen, die Methode jetzt die **ExecuteSegmentedAsync** -Methode und Modifizierer **erwarten** Ergebnisse asynchron abrufen verwendet.  

Die Clientanwendung kann diese Methode mehrmals (mit unterschiedlichen Werten für **Parameterwerte** ) aufrufen, und jede Abfrage wird in einem separaten Thread ausgeführt.  

Beachten Sie, dass es keine asynchrone Version der **Execute** -Methode der **TableQuery** -Klasse die Schnittstelle **IEnumerable** asynchronen Enumeration nicht unterstützt.  

Sie können auch einfügen, aktualisieren und Löschen von Entitäten asynchron. Im folgende C#-Beispiel zeigt eine einfache, synchrone Methode einfügen oder eine Mitarbeiterentität ersetzen:  

    private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
    {
        TableResult result = employeeTable
            .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
    }  

Dieser Code kann problemlos ändern, sodass die Aktualisierung asynchron wie folgt ausgeführt:  

    private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
    {
        TableResult result = await employeeTable
            .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
    }  

In diesem Beispiel asynchrone sehen Sie folgende Änderung zur synchronen Version:  

-   Die Methodensignatur jetzt **Async** -Modifizierer enthalten und gibt eine Instanz der **Aufgabe** zurück.  
-   Anstatt die **Execute** -Methode zum Aktualisieren der Entität die Methode nun die **ExecuteAsync** -Methode und Modifizierer **erwarten** Ergebnisse asynchron abrufen verwendet.  

Die Clientanwendung kann mehrere asynchrone Methoden wie diese, und jeder Aufruf wird in einem separaten Thread ausgeführt.  


### <a name="credits"></a>Gutschriften
Wir möchten folgenden Azure Team für ihre Beiträge danken: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah Serdar Ozler sowie Tom Hollander von Microsoft DX. 

Wir außerdem möchte die folgenden Microsoft MVP für ihr wertvolles Feedback bei Überprüfung: Igor Papirov und Edward Bakker.



[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png
 
