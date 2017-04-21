<properties
   pageTitle="Einfache Anwendung | Azure Referenzarchitektur | Microsoft Azure"
   description="Empfohlene Architektur für eine einfache Web-Anwendung in Microsoft Azure ausgeführt."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="mwasson"/>

# <a name="azure-reference-architecture-basic-web-application"></a>Azure Referenzarchitektur: einfache Anwendung

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt eine empfohlene Architektur für eine einfache Anwendung in Microsoft Azure. Die Architektur implementiert ein Web-front-End mit [Azure App Service][app-service], [Azure SQL] -Datenbank[ sql-db] in der Datenbank. Spätere Artikeln dieser Serie basieren auf diese grundlegende Architektur Komponenten wie Cache und CDN. 

> [AZURE.NOTE] Dieser Artikel konzentriert sich nicht auf die Entwicklung und Anwendungsframeworks nicht angenommen. Stattdessen soll kennen Azure verschiedene Dienste innerhalb dieser Anwendungsarchitektur zusammenwirken.

## <a name="architecture-diagram"></a>Architekturdiagramm

![[0]][0]

Die Architektur besteht aus folgenden Komponenten:

- **Ressourcengruppe**. Eine [Ressourcengruppe] [ resource-group] ist ein logischer Container für Azure-Ressourcen. 

- **App Service-app**. [Azure App Service] [ app-service] ist eine vollständig verwaltete Plattform zum Erstellen und Bereitstellen von Cloudanwendungen.     

- **App Service-Plan**. Eine [App Service-Plan] [ app-service-plans] bietet verwaltete virtuelle Maschinen (VMs), die Ihre Anwendung hosten. Alle apps auf dieselben VM-Instanzen ausführen Plan zugeordnet. 

- **Deployment-Steckplätze.**  [Bereitstellung Steckplatz] [ deployment-slots] können Sie eine Bereitstellung bereitstellen und tauschen Sie es mit der Bereitstellung in der Produktion. Auf diese Weise vermeiden Sie direkt in die Produktion bereitstellen. Siehe Abschnitt [Verwaltung](#manageability-considerations) für bestimmte Recommendations.   

- **IP-Adresse.** App Service-app hat eine öffentliche IP-Adresse und einen Domänennamen ein. Der Domänenname ist eine untergeordnete Domäne der `azurewebsites.net`, wie `contoso.azurewebsites.net`. Verwenden einen benutzerdefinierten Domänennamen `contoso.com`, erstellen Sie DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zugeordnet. Weitere Informationen finden Sie unter [Konfigurieren einer benutzerdefinierten Domänennamen in Azure App Service][custom-domain-name].

- **Azure SQL-Datenbank**. [SQL-Datenbank] [ sql-db] ist ein relationale Datenbank-as-a-Service in der Cloud. 

- **Logische Server.** In Azure SQL-Datenbank hostet ein logischer Server Datenbanken. Sie können mehrere Datenbanken pro logischen Server erstellen. 

- **Azure-Speicher.** Erstellen Sie ein Konto Azure-Speicher mit einem BLOB-Container zum Speichern von Diagnoseprotokollen. 

- **Azure Active Directory** (Azure AD). Azure AD verwenden oder eine andere Identitätsprovider für die Authentifizierung.

## <a name="recommendations"></a>Empfehlung

### <a name="app-service-plan"></a>App Service-plan

Verwenden Sie Ebenen Standard oder Premium, da skalieren, skalieren und SSL, unter anderem unterstützen. Jede Ebene unterstützt mehrere *Instanzen*die Anzahl von Kernen und Speicher unterscheiden. Nachdem Sie einen Plan erstellen, können Sie Ebenen oder Größe ändern. Weitere Informationen zu app Service-Plänen finden Sie unter [App Servicepreise][app-service-plans-tiers].

Instanzen sind im Plan App Service kostenpflichtig, wenn die Anwendung beendet wird. Stellen Sie sicher, Pläne löschen, die Sie (z. B. testbereitstellungen verwenden).

### <a name="sql-database"></a>SQL-Datenbank

Die [V12 Version] [ sql-db-v12] der SQL-Datenbank. SQL-Datenbank unterstützt Basic, Standard und Premium [Dienstebenen][sql-db-service-tiers], mehrere Leistung innerhalb der einzelnen Tiers, gemessen in [Datenbank Buchungseinheiten (DTUs)][sql-dtu]. Planen von Kapazität und wählen Sie eine Ebene und Leistung, die die Mindestanforderungen erfüllt.

### <a name="region"></a>Region

Bereitstellen der App Service-Plan und die SQL-Datenbank in derselben Region Netzwerklatenz minimieren. Im Allgemeinen wählen Sie eine Region am nächsten an Ihre Benutzer. 

Die Ressourcengruppe hat auch einen Bereich gibt, Bereitstellung Metadaten gespeichert werden. Platzieren der Ressourcengruppe und seine Ressourcen im selben Bereich. Dies kann während der Bereitstellung Verfügbarkeit wenn Problem in einigen Azure Rechenzentren.  


## <a name="scalability-considerations"></a>Aspekte der Skalierung

### <a name="scaling-the-app-service-app"></a>Skalieren der App Service-app

Gibt es zwei Arten skaliert App Service-app:

- *Skalieren*, d. h. die Größe ändern. Instanz bestimmt den Speicher Anzahl von Kernen und Speicher für jede VM-Instanz. Manuell können skalieren durch Ändern der Größe oder der Plan-Ebene.  

- *Dezentrales Skalieren*, d. h. Instanzen erhöhter Arbeitslast hinzufügen. Jeder Tarif hat eine maximale Anzahl von Instanzen. Sie können manuell skalieren, ändern Sie die Anzahl der Instanzen oder [Skalierung] [ web-app-autoscale] zu Azure automatisch hinzufügen oder Entfernen von Instanzen basierend auf einem Zeitplan bzw. Metriken.

    - Eine skalieren Profil definiert die minimale und maximale Anzahl der Instanzen. Profile können geplant werden. Beispielsweise können Sie unterschiedliche Profile für Wochentage und Wochenenden erstellen. 

    - Ein Profil hat Regeln für optional beim Hinzufügen oder Entfernen von Instanzen, in die Mindest- und Mazimum durch das Profil definiert. Beispiel: zwei Instanzen hinzufügen, ist CPU-Auslastung über 70 % 5 Minuten. Jeder Skalierung erfolgt schnell &mdash; in der Regel innerhalb von Sekunden.
    
    - Skalierungsgröße Regeln enthalten *eine Abkühldauer, die das Wartezeitintervall nach einer Aktion vor dem Ausführen einer anderen Aktion* . Die Abkühldauer kann zu wieder stabilisieren.
 
Hier sind unsere Vorschläge für eine Webanwendung skalieren:

- Soweit möglich, vermeiden Sie Aufwärts und abwärts skalieren, da einen Neustart der Anwendung auslösen. Wählen Sie Tier und Größe, die Anforderungen unter normalen Last Leistung und Skalieren die Instanzen Änderungen Verkehrsaufkommen.    

- Automatische Skalierung zu aktivieren. Wenn die Anwendung eine Arbeitslast vorhersagbare, reguläre Profilen um Instanz zählt voraus planen. Verwenden Sie die Arbeitslast nicht vorhersagbar ist, regelbasierte Skalierung Last reagieren, sobald sie auftreten. Sie können beide Methoden kombinieren. 

- CPU ist im Allgemeinen ein guter Autoscale Regeln. Laden Sie, testen Sie die Anwendung, Identifizieren potenzieller Engpässe und Daten skalieren Regeln basieren.  

- Eine kürzere Abkühldauer Instanzen, und eine länger Abkühldauer für das Entfernen von Instanzen festgelegt. Beispielsweise festlegen Sie 5 Minuten eine Instanz hinzugefügt, jedoch 60 Minuten eine Instanz entfernt. Plötzlicher Belastung empfiehlt sich schnell neue Instanzen hinzufügen, behandeln den Datenverkehr und dann allmählich wieder vergrößern.
    
### <a name="scaling-sql-database"></a>SQL Datenbank skalieren

Benötigen Sie eine höhere Ebene oder Leistung Servicelevel für SQL-Datenbank, können Sie einzelne Datenbanken mit Ausfälle skalieren. Details finden Sie unter [Ändern Service-Tier und Leistung einer SQL-Datenbank][sql-db-scale]. 

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Gleichzeitig schreiben die SLA für App Service ist 99,95 % und SLA für SQL-Datenbank ist 99,99 % Stufen Basic, Standard und Premium. 

> [AZURE.NOTE] App Service SLA gilt für einzelne und mehrfache Instanzen.  

### <a name="backups"></a>Backups

SQL-Datenbank bietet Point-in-Time-Wiederherstellung und Geo-Wiederherstellung. Diese Funktionen stehen auf allen Ebenen und automatisch aktiviert. Sie müssen planen oder die Backups zu verwalten. 

- Verwenden Sie Point-in-Time-Wiederherstellung von [Benutzerfehlern][sql-human-error]. Wird Ihre Datenbank zu einem früheren Zeitpunkt.

- Geo-Wiederherstellung zur [Wiederherstellung bei einem Dienstausfall]verwenden[sql-outage-recovery]. Eine Datenbank wiederhergestellt aus einer Geo redundante Sicherung.

Weitere Informationen zu diesen Optionen finden Sie unter [Cloud Business Continuity und Disaster Recovery für Datenbank mit SQL-Datenbank][sql-backup]. 

App Service bietet eine [Sicherung und Wiederherstellung] [ web-app-backup] Funktion. Jedoch beachten, dass die gesicherten Dateien app in Klartext gehören. Dazu gehören Kennwörter wie Verbindungszeichenfolgen. Verwenden des Sicherungsfeatures App Service sichern Ihre SQL-Datenbanken, da die Datenbank in eine SQL-.bacpac-Datei exportiert [DTUs]verwendet[sql-dtu]. Verwenden Sie stattdessen die beschriebenen SQL Datenbank Point-in-Time-Wiederherstellung. 

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Erstellen Sie separate Ressourcengruppen für Produktion, Entwicklung und Testen Sie Umgebung. Dies erleichtert die Bereitstellung verwalten, Test Bereitstellung löschen und Zugriffsrechte zuweisen.

Beim Zuweisen von Ressourcen zu Ressourcengruppen Folgendes:

- Lebenszyklus. Im Allgemeinen stellen Sie Ressourcen mit dem gleichen Lebenszyklus in derselben Ressourcengruppe. 

- Zugriff. Sie können [Rollenbasierte Zugriffskontrolle] [ rbac] (RBAC) Richtlinien auf die Ressourcen in einer Gruppe anwenden.

- Abrechnung. Die mehrstufigen Kosten zeigen die eine Ressourcengruppe.  

Weitere Informationen finden Sie unter [Übersicht über Azure Ressource-Manager][resource-group].

### <a name="deployment"></a>Bereitstellung

Die Bereitstellung umfasst zwei Schritte:

- Bereitstellung von Azure Ressourcen. Wir empfehlen die Verwendung von [Azure Resoure Manager Vorlagen] [ arm-template] für diesen Schritt. Vorlagen erleichtern die Bereitstellung über PowerShell oder CLI Azure automatisieren. 

- Bereitstellen der Anwendung (Code, Binärdateien und Dateien). Sie haben mehrere Optionen, einschließlich der Bereitstellung von einem lokalen Repository Git mit Visual Studio oder kontinuierliche Bereitstellung von Cloud-basierten Datenquellen-Steuerelement. [Bereitstellen Ihrer Anwendung auf Azure App Service]finden Sie unter[deploy].  
 
Eine App Service-app hat immer eine Bereitstellung Steckplatz mit dem Namen `production`, live Standort darstellt. Es wird empfohlen, staging-Steckplatz für die Bereitstellung von Updates erstellen. Staging-Steckplatz bietet folgende Vorteile:

- Sie können die Bereitstellung erfolgreich vor Austausch in Produktion, überprüfen.

- Bereitstellen in staging-Steckplatz wird sichergestellt, dass alle Instanzen vor in Betrieb ausgetauscht erwärmt werden. Viele Programme besitzen eine erhebliche Aufwärmdauer und -Kaltstartzeit. 

Es wird empfohlen, einen dritten Steckplatz Aufnahme die letzten als funktionierend bekannten Bereitstellung erstellen. Nachdem Staging- und tauschen Verschieben der vorherigen Bereitstellung in der Produktion (die jetzt Staging) in zweifelsfrei funktionierenden Steckplatz. So, wenn Sie später ein Problem feststellen können Sie schnell auf die letzte als funktionierend bekannte Version zurückkehren. 

![[1]][1]

Wenn Sie eine frühere Version wiederherzustellen, sicherstellen Sie, dass Änderungen Schema Datenbank abwärtskompatibel sind.

Verwenden Sie keine Slots auf Ihre Bereitstellung in der Produktion zum Testen, denn alle apps innerhalb derselben Anwendung Serviceplan dieselben VM-Instanzen. Auslastungstests können z. B. die Produktion Website beeinträchtigen. Erstellen Sie stattdessen separate App Pläne für Produktion und Test. Mit Test-Installationen in einen eigenen Plan, isolieren sie aus der Produktionsflussversion. 

### <a name="configuration"></a>Konfiguration

Konfiguration als [app-Einstellungen]speichern[app-settings]. Einstellungen der app in Ihren Vorlagen Ressourcenmanager oder mithilfe von PowerShell. Zur Laufzeit werden app als Umgebungsvariablen zur Verfügung. 

Überprüfen Sie nie Kennwörter, Zugriffstasten und Verbindungszeichenfolgen in Datenquellen-Steuerelement. Stattdessen übergeben Sie als Parameter ein Bereitstellungsskript, die diese Werte als app-Einstellung speichert. 

Beim Ersetzen eines Bereitstellung Steckplatz sind app-Einstellungen standardmäßig ausgetauscht. Benötigen Sie verschiedene Einstellungen für Produktion und Staging können Sie app-Einstellungen erstellen, die nicht ausgelagert abrufen und in einen Steckplatz "halten". 

### <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

[Diagnoseprotokoll]aktivieren[diagnostic-logs], einschließlich der Anwendung protokollieren und Web-Server anmelden. Konfigurieren der Protokollierung zum BLOB-Speicher verwenden. Aus Gründen der Leistung erstellen Sie einer separaten Speicherkonto für die Diagnoseprotokolle Verwenden Sie nicht das gleiche Speicherkonto für Protokolle und Daten. Ausführliche Anleitung zur Protokollierung finden Sie [Hinweise zur Überwachung und Diagnose][monitoring-guidance]. 

Einen Dienst wie [Neue Relikt] [ new-relic] oder [Anwendung Einblicke] [ app-insights] Monitor Anwendungsleistung und Belastung. Beachten Sie die [Datenrate Grenzen] [ app-insights-data-rate] Anwendung Einblicke.

Durchführen von Auslastungstests mithilfe eines Tools wie [Visual Studio Team][vsts]. Eine allgemeine Übersicht der Leistungsanalyse Cloudanwendungen finden Sie unter [Performance Analysis Primer][perf-analysis].

Tipps für die Problembehandlung der Anwendung:

- [Problembehandlung bei Blade] [ troubleshoot-blade] im Azure Portal Identität Lösung allgemeiner Probleme.

- [Streaming-Protokoll] aktivieren[ web-app-log-stream] Protokollierung Informationen in Echtzeit. 

- [Kudu Dashboard] [ kudu] bietet mehrere Tools zum Überwachen und Debuggen Ihrer Anwendung. Weitere Informationen finden Sie unter [Azure Websites online-Tools, die Sie kennen sollten] [ kudu] (Blogbeitrag). Sie erreichen Kudu Dashboard von Azure-Portal. Öffnen Sie das Blade für Ihre Anwendung und **Tools**, klicken auf **Kudu**.

- Wenn Sie Visual Studio verwenden, finden Sie im Artikel [Problembehandlung eine Webanwendung in Azure App Service mithilfe von Visual Studio] [ troubleshoot-web-app] zum Debuggen und Fehlerbehebung.


## <a name="security-considerations"></a>Sicherheitsaspekte

> [AZURE.NOTE] Dieser Abschnitt zeigt, einige Sicherheitsaspekte, die in diesem Artikel beschriebenen Azure-Dienste sind. Es ist keine vollständige Liste der bewährten Sicherheitsmethoden. Einige zusätzliche Sicherheitsaspekte finden Sie unter [Secure einer Anwendung in Azure App Service][app-service-security].

**SQL-Datenbank überwachen**. Überwachung können Sie die Einhaltung behördlicher Auflagen und Einsichten in Diskrepanzen und Anomalien, die Unternehmen hinweisen oder Verstößen Sicherheit. Einführung [in die SQL-Datenbank Überwachung][sql-audit].

**Bereitstellung Steckplätze**. Jede Bereitstellung Steckplatz hat eine öffentliche IP-Adresse. Sichern nicht produktiven Steckplätze mit [Active Directory Azure Login][aad-auth], so dass nur Mitglieder der Entwicklungs- und DevOps Teams diese Endpunkte erreichen. 

**Protokollierung**. Protokolle sollten Benutzer Kennwörter oder andere Informationen, die zum Identitätsdiebstahl commit verwendet möglicherweise niemals notieren. Bereinigen Sie die Details aus den Daten vor dem Speichern.   

**SSL**. Eine App Service-app enthält SSL-Endpunkt für eine Subdomäne von `azurewebsites.net` ohne zusätzliche Kosten mit einem Platzhalterzertifikat für die `*.azurewebsites.net` Domäne.     Verwenden Sie einen benutzerdefinierten Domänennamen müssen Sie ein Zertifikat bereitstellen, die die benutzerdefinierte Domäne übereinstimmt. Die einfachste Möglichkeit ist ein Zertifikat durch den Azure-Verwaltungsportal kaufen. Sie können auch Zertifikate von anderen Zertifizierungsstellen. Weitere Informationen finden Sie unter [kaufen und konfigurieren ein SSL-Zertifikat für Ihren Azure App Service][ssl-cert]. 

Als bewährte Methode Ihre app sollte HTTPS erzwingen, indem Sie HTTP-Anfragen umleiten. Sie können dies in Ihrer Anwendung implementiert oder URL Rewrite Regel wie [HTTPS für eine Anwendung in Azure App Service aktivieren]unter[ssl-redirect].

### <a name="authentication"></a>Authentifizierung

Wir empfehlen Authentifizierung über ein Identitätsprovider (IDP) wie Azure AD Facebook, Google, Twitter, OAuth mit 2 oder OpenID verbinden (OIDC) für die Authentifizierung fließen. 

Azure AD können Sie Benutzer und Gruppen verwalten, Anwendungsrollen erstellen, integrieren Ihre lokalen Identitäten und Back-End-Dienste wie Office 365 und Skype für Unternehmen.

Vermeiden Sie die Anwendung Benutzernamen und Anmeldeinformationen direkt verwalten als eine große Angriffsfläche erstellt. Zumindest müssen Sie e-Mail, Passwort-Wiederherstellung und mehrstufige Authentifizierung haben; Kennwortstärke zu überprüfen; und Kennworthashes sicher. Große Identitätsprovider behandeln alle Dinge, und ständig überwachen und verbessern ihre Sicherheitsmaßnahmen. 

Verwenden Sie [App-Authentifizierung] [ app-service-auth] Authentifizierungsablauf OAuth-OIDC implementiert. App-Authentifizierung bietet folgende Vorteile:

- Einfach zu konfigurieren.
    
- Einfache Authentifizierung Szenarien ist kein Code erforderlich.
    
- Unterstützt Delegierte Berechtigung; verwenden also OAuth-Zugriffstoken können Ressourcen für den Benutzer.
    
- Bietet einen integrierten token Cache.

Einige Nachteile der App-Authentifizierung:  

- Optionen für die Anpassung begrenzt. 

- Delegierte Berechtigung ist ein Back-End-Ressource pro Sitzung auf.
    
- Wenn Sie mehrere Identitätsanbieter verwenden, ist keine integrierten Mechanismen für home Realm Discovery.

- Multi-Tenant-Szenarios muss die Anwendung die Logik zum Überprüfen der Tokenausgeber implementieren.



## <a name="deploying-the-sample-solution"></a>Bereitstellen der Beispielprojektmappe

Eine Beispielvorlage Resoure Manager für diese Architektur ist auf GitHub verfügbar. Downloaden sie [hier][paas-basic-arm-template].

Um die Vorlage mithilfe von PowerShell bereitzustellen, führen Sie folgende Befehle ein:

```
New-AzureRmResourceGroup -Name <resource-group-name> -Location "West US"

$parameters = @{"appName"="<app-name>";"environment"="dev";"locationShort"="uw";"databaseName"="app-db";"administratorLogin"="<admin>";"administratorLoginPassword"="<password>"}
    
New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile .\PaaS-Basic.json -TemplateParameterObject  $parameters
```

Weitere Informationen finden Sie unter [Bereitstellen Ressourcen Azure Ressourcenmanager Vorlagen][deploy-arm-template].


## <a name="next-steps"></a>Nächste Schritte

- Verbessert Skalierbarkeits- und durch Funktionen wie caching, CDN und Verarbeitung für lang andauernde Vorgänge im Hintergrund. [Web-Anwendung mit verbesserter Skalierbarkeit](guidance-web-apps-scalability.md)anzeigen

<!-- links -->

[aad-auth]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-data-rate]: ../application-insights/app-insights-pricing.md
[app-service]: https://azure.microsoft.com/en-us/documentation/services/app-service/
[app-service-auth]: ../app-service-api/app-service-api-authentication.md
[app-service-plans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[app-service-plans-tiers]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[app-service-security]: ../app-service-web/web-sites-security.md
[app-settings]: ../app-service-web/web-sites-configure.md
[arm-template]: ../resource-group-overview.md
[custom-domain-name]: ../app-service-web/web-sites-custom-domain-name.md
[deploy]: ../app-service-web/web-sites-deploy.md
[deploy-arm-template]: ../resource-group-template-deploy.md
[deployment-slots]: ../app-service-web/web-sites-staged-publishing.md
[diagnostic-logs]: ../app-service-web/web-sites-enable-diagnostic-log.md
[kudu]: https://azure.microsoft.com/en-us/blog/windows-azure-websites-online-tools-you-should-know-about/
[monitoring-guidance]: ../best-practices-monitoring.md
[new-relic]: http://newrelic.com/
[paas-basic-arm-template]: https://github.com/mspnp/blueprints/tree/master/paas-basic/Paas-Basic/Templates
[perf-analysis]: https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md
[rbac]: ../active-directory/role-based-access-control-what-is.md
[resource-group]: ../resource-group-overview.md
[sla]: https://azure.microsoft.com/en-us/support/legal/sla/
[sql-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-backup]: ../sql-database/sql-database-business-continuity.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-db-scale]: ../sql-database/sql-database-scale-up-powershell.md
[sql-db-service-tiers]: ../sql-database/sql-database-service-tiers.md
[sql-db-v12]: ../sql-database/sql-database-v12-whats-new.md
[sql-dtu]: ../sql-database/sql-database-service-tiers.md#understanding-dtus
[sql-human-error]: ../sql-database/sql-database-business-continuity.md#recover-a-database-after-a-user-or-application-error
[sql-outage-recovery]: ../sql-database/sql-database-business-continuity.md#recover-a-database-to-another-region-from-an-azure-regional-data-center-outage
[ssl-redirect]: ../app-service-web/web-sites-configure-ssl-certificate.md#4-enforce-https-on-your-app
[sql-resource-limits]: ../sql-database/sql-database-resource-limits.md
[ssl-cert]: ../app-service-web/web-sites-purchase-ssl-web-site.md
[troubleshoot-blade]: https://azure.microsoft.com/en-us/updates/self-service-troubleshooting-for-app-service-web-apps-customers/
[troubleshoot-web-app]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[vsts]: https://www.visualstudio.com/en-us/features/vso-cloud-load-testing-vs.aspx
[web-app-autoscale]: ../app-service-web/web-sites-scale.md#scaling-to-standard-or-premium-mode
[web-app-backup]: ../app-service-web/web-sites-backup.md
[web-app-log-stream]: ../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs
[0]: ./media/blueprints/paas-basic-web-app.png "Architektur einer einfachen Azure Web-Anwendung"
[1]: ./media/blueprints/paas-basic-web-app-staging-slots.png "Steckplätze für Produktion austauschen und staging-Bereitstellung"