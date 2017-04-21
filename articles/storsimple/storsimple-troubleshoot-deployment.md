<properties 
   pageTitle="StorSimple Bereitstellung Fehlerbehebung | Microsoft Azure"
   description="Beschreibt das diagnostizieren und beheben Fehler beim Bereitstellen von StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Problembehandlung bei StorSimple Gerät Bereitstellungsprobleme

## <a name="overview"></a>Übersicht

Dieser Artikel enthält nützliche Hinweise zur Problembehandlung für die Microsoft Azure StorSimple Bereitstellung. Es beschreibt Probleme, Ursachen und empfohlenen Schritte können Sie die Probleme, die auftreten können, wenn Sie StorSimple konfigurieren. Diese Informationen gelten für StorSimple lokalen physischen Gerät und StorSimple virtuelles Gerät.

> [AZURE.NOTE] Device-Konfiguration Probleme, die Sie möglicherweise stoßen können auftreten, wenn das Gerät zum ersten Mal bereitstellen, oder sie später auftreten können, wenn das Gerät funktionstüchtig ist. Dieser Artikel konzentriert sich auf die Problembehandlung bei der erstmaligen Bereitstellung. Wechseln Sie zum Behandeln von betrieblichen Gerät [Problembehandlung betriebliche Gerät](storsimple-troubleshoot-operational-device.md)Probleme.

Dieser Artikel beschreibt die Tools zur Bereitstellung von StorSimple bei und bietet ein schrittweises Beispiel zur Problembehandlung.

## <a name="first-time-deployment-issues"></a>Probleme bei der erstmaligen Bereitstellung

Wenn Sie ein Problem stoßen, wenn das Gerät zum ersten Mal bereitstellen, beachten Sie Folgendes:

- Wenn Sie ein physisches Gerät Probleme, unbedingt Hardware installiert und wie beschrieben in [Ihr Gerät StorSimple 8100 installieren](storsimple-8100-hardware-installation.md) oder [das Gerät StorSimple 8600](storsimple-8600-hardware-installation.md)konfiguriert wurde.
- Überprüfen Sie die erforderlichen Komponenten für die Bereitstellung. Stellen Sie sicher, dass alle Informationen, die in der [Checkliste für die Bereitstellung](storsimple-deployment-walkthrough.md#deployment-configuration-checklist)beschrieben.
- Überprüfen Sie die Versionshinweise StorSimple, um festzustellen, ob das Problem beschrieben wird. Die Versionshinweise enthalten Workarounds für bekannte Installationsprobleme. 

Während der gerätebereitstellung Probleme am häufigsten von Benutzern Gesicht auftreten wenn sie den Assistenten ausführen und wenn sie das Gerät über Windows PowerShell für StorSimple registrieren. (Mithilfe von Windows PowerShell für StorSimple registrieren und konfigurieren Sie das Gerät StorSimple. Informationen für die geräteregistrierung finden Sie unter [Schritt 3: Konfigurieren und registrieren Sie Ihr Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

In den folgenden Abschnitten können Sie Probleme beheben, die auftreten, wenn Sie StorSimple zum ersten Mal konfigurieren.

## <a name="first-time-setup-wizard-process"></a>Erstmalige Einrichtung-Assistent

Die folgenden Schritte Zusammenfassen des Installationsvorgangs Assistenten. Ausführliche Informationen finden Sie unter [Bereitstellen der lokalen StorSimple-Gerät](storsimple-deployment-walkthrough.md).

1. Führen Sie das Cmdlet [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) um den Setup-Assistenten zu starten, Sie die restlichen Schritte. 
2. Konfigurieren Sie das Netzwerk: der Setup-Assistenten können Sie Netzwerk für 0 Netzwerkschnittstelle Daten auf dem Gerät StorSimple konfigurieren. Dazu gehören die folgenden:
  - Virtuelle IP-Adresse (VIP), Subnetzmaske und Gateway – das Cmdlet " [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) " wird im Hintergrund ausgeführt. IP-Adresse, Subnetzmaske und Gateway für die Daten 0 Netzwerkschnittstelle konfiguriert auf dem StorSimple-Gerät.
  - Primärer DNS-Server – das Cmdlet " [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) " wird im Hintergrund ausgeführt. Konfiguriert die DNS-Clienteinstellungen für die Projektmappe StorSimple.
  - NTP-Server – das Cmdlet " [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) " wird im Hintergrund ausgeführt. NTP-Einstellungen für Ihre Lösung StorSimple konfiguriert.
  - Optionale Webproxy – das Cmdlet " [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) " wird im Hintergrund ausgeführt. Es legt und Web Proxy-Konfiguration für die StorSimple-Lösung.
3. Die Passwörter: Nächstes Geräteadministrator und StorSimple Snapshot-Manager Kennwörter eingerichtet ist. Wenn Sie Update 1 ausgeführt, dann nicht müssen Sie das Kennwort StorSimple Snapshot-Manager einrichten.
  - Das Administratorkennwort Gerät wird das Gerät anmelden verwendet. Kennwort für das Gerät ist **Kennwort1**.
  - StorSimple Snapshot-Manager-Kennwort ist erforderlich, wenn Sie ein Gerät StorSimple Snapshot-Manager konfigurieren. Müssen Sie zuerst das Kennwort im Setup-Assistenten festgelegt und dann festlegen und Ändern von StorSimple Manager-Dienst. Dieses Kennwort authentifiziert das Gerät mit StorSimple Snapshot-Manager.
 
    > [AZURE.IMPORTANT] Kennwörter sind vor der Registrierung erfasst, aber erst angewendet, nachdem Sie das Gerät erfolgreich registriert. Ist ein Fehler ein Kennwort zuweisen, werden Sie aufgefordert, das Kennwort erneut eingeben, bis der Kennwörter (die Komplexität erfüllen) gesammelt werden.

4. Das Gerät registriert: der letzte Schritt ist das Gerät unter Microsoft Azure StorSimple Manager-Dienst registriert. Die Registrierung muss [der Dienstregistrierungsschlüssel erhalten](storsimple-manage-service.md#get-the-service-registration-key) von klassischen Azure-Portal und im Setup-Assistenten bereitstellen. Nachdem das Gerät erfolgreich registriert wurde, wird ein Dienst Daten Verschlüsselungsschlüssel bereitgestellt. Achten Sie darauf, diesen Verschlüsselungsschlüssel an einem sicheren Ort speichern, da es alle nachfolgenden Geräte mit dem Dienst registrieren muss.

## <a name="common-errors-during-device-deployment"></a>Häufige Fehler bei der gerätebereitstellung

Der folgenden Tabelle sind häufige Fehler beim auftreten können Sie:

- Konfigurieren Sie die erforderlichen Netzwerk.
- Konfigurieren Sie die optionale Web-Proxyeinstellungen.
- Der Geräteadministrator und StorSimple Snapshot-Manager Kennwörter einrichten. 
- Registrieren Sie das Gerät. 

## <a name="errors-during-the-required-network-settings"></a>Fehler bei der erforderlichen Einstellungen

| Nein.| Fehlermeldung | Mögliche Ursachen | Empfohlene Aktion |
| ---| ------------- | --------------- | ------------------ |
| 1  | Rufen Sie HcsSetupWizard: Dieser Befehl kann nur auf aktive Domänencontroller ausgeführt werden. | Der passiven Controller wurde konfiguriert wird.| Befehl vom aktiven Controller. Weitere Informationen finden Sie unter [Identifizieren einer aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Rufen Sie HcsSetupWizard: Gerät ist nicht bereit. | Es sind Probleme mit der Netzwerkkonnektivität auf 0.| Überprüfen Sie die Netzwerk-Konnektivität auf 0.|
| 3 | Rufen Sie HcsSetupWizard: Ist ein IP-Adressenkonflikt mit einem anderen System im Netzwerk (Ausnahme von HRESULT: 0x80070263). | Daten 0 bereitgestellte IP wurde bereits von einem anderen System. | Geben Sie eine neue IP-Adresse nicht verwendet wird.|
| 4 | Rufen Sie HcsSetupWizard: Cluster-Ressource fehlgeschlagen. (Ausnahme von HRESULT: 0x800713AE). | Doppelte VIP. Die angegebene IP-Adresse wird bereits verwendet.| Geben Sie eine neue IP-Adresse nicht verwendet wird.|
| 5 | Rufen Sie HcsSetupWizard: Ungültiger IPv4-Adresse. | Die IP-Adresse wird in einem falschen Format bereitgestellt.| Überprüfen Sie das Format, und geben Sie die IP-Adresse erneut. Weitere Informationen finden Sie unter [IPv4-Adressierung][1]. |
| 6 | Rufen Sie HcsSetupWizard: Ungültige IPv6-Adresse. | Die IP-Adresse wird in einem falschen Format bereitgestellt.| Überprüfen Sie das Format, und geben Sie die IP-Adresse erneut. Weitere Informationen finden Sie unter [IPv6-Adressierung][2].|
| 7 | Aufrufen-HcsSetupWizard: Stehen keine weitere Endpunkte aus der Endpunktzuordnungsdienst. (Ausnahme von HRESULT: 0x800706D9) | Die Cluster-Funktionen funktionieren nicht. | [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) weiter.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Fehler bei der optionalen Webproxyeinstellungen

| Nein.| Fehlermeldung | Mögliche Ursachen | Empfohlene Aktion |
| ---| ------------- | --------------- | ------------------ |
| 1  | Rufen Sie HcsSetupWizard: Ungültiger Parameter (Ausnahme von HRESULT: 0 x 80070057) | Einer der Parameter für die Proxyeinstellungen ist ungültig.| Der URI wird nicht im richtigen Format bereitgestellt. Verwenden Sie Folgendes Format: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Rufen Sie HcsSetupWizard: RPC-Server nicht verfügbar (Ausnahme von HRESULT: 0x800706ba) | Die Ursache ist eine der folgenden:<ol><li>Der Cluster ist nicht.</li><li>Passive Controller nicht mit aktiven Domänencontroller kommunizieren und Befehl vom passiven Controller.</li></ol> | Je nach Ursache:<ol><li>[Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) zu Cluster ist.</li><li>Führen Sie den Befehl vom aktiven Controller. Wenn Sie den Befehl vom passiven Controller ausführen möchten, müssen Sie sicherstellen, dass der passive Controller mit dem aktiven Controller kommunizieren kann. Sie benötigen, [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) , wird diese Verbindung unterbrochen.</li></ol> |
| 3 | Rufen Sie HcsSetupWizard: RPC-Aufruf ist (Ausnahme von HRESULT: 0x800706be) | Cluster ist. | [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) zu Cluster ist.|
| 4 | Aufrufen-HcsSetupWizard: Die Clusterressource wurde nicht gefunden (Ausnahme von HRESULT: 0x8007138f) | Die Ressource wurde nicht gefunden. Dies kann bei die Installation nicht korrekt war. | Sie müssen das Gerät auf die werkseitigen Standardeinstellungen zurückgesetzt. [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) Clusterressource erstellen.|
| 5 | Rufen Sie HcsSetupWizard: Cluster-Ressource nicht online (Ausnahme von HRESULT: 0x8007138c)| Cluster-Ressourcen sind nicht online. | [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) weiter.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Fehler im Zusammenhang mit Geräteadministrator und StorSimple Snapshot-Manager Kennwörter

Das standardmäßige Gerät Administratorkennwort ist **Kennwort1**. Dieses Kennwort läuft nach der ersten Anmeldung; Daher müssen Sie den Assistenten verwenden, ändern. Wenn das Gerät zum ersten Mal registrieren, müssen Sie ein neues Gerät Administratorkennwort angeben. 

Verwenden Sie auf dem Windows Server Host StorSimple Snapshot-Manager-Software das Gerät verwalten, müssen Sie auch ein StorSimple Snapshot-Manager bei der erstmaligen Registrierung bereitstellen. 

Stellen Sie sicher, dass Kennwörter die Mindestanforderungen:

- Administratorkennwort Gerät muss zwischen 8 und 15 Zeichen lang sein.
- Ihr Kennwort StorSimple Snapshot-Manager sollte 14 oder 15 Zeichen lang sein.
- Kennwörter sollten 3 der folgenden 4 Zeichen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. 
- Ihr Kennwort darf nicht mit dem letzten 24 Kennwörter übereinstimmen.

Halten Sie außerdem Kennwörter jedes Jahr und können geändert werden, nur, nachdem Sie das Gerät erfolgreich registriert. Wenn die Registrierung aus irgendeinem Grund fehlschlägt, werden das Kennwort nicht geändert. Weitere Informationen zum Geräteadministrator und StorSimple Snapshot-Manager Kennwörter finden Sie [StorSimple Manager-Dienst StorSimple Kennwörter ändern](storsimple-change-passwords.md).

Eine oder mehrere der folgenden Fehler treten möglicherweise beim Geräteadministrator und StorSimple Snapshot-Manager Kennwörter einrichten.

| Nein.| Fehlermeldung | Empfohlene Aktion |
| ---| ------------- | ------------------ | 
| 1  | Das Kennwort überschreitet die maximale Länge. |Verwenden Sie ein Kennwort, das diese Anforderung erfüllt:<ul><li>Ihr Administratorkennwort Gerät muss zwischen 8 und 15 Zeichen lang sein.</li><li>Ihr Kennwort StorSimple Snapshot-Manager muss 14 oder 15 Zeichen lang sein.</li></ul> | 
| 2 | Das Kennwort erfüllt nicht die erforderliche Länge. | Verwenden Sie ein Kennwort, das diese Anforderung erfüllt:<ul><li>Ihr Administratorkennwort Gerät muss zwischen 8 und 15 Zeichen lang sein.</li><li>Ihr Kennwort StorSimple Snapshot-Manager muss 14 oder 15 Zeichen lang sein.</lu></ul> |
| 3 | Das Kennwort muss Kleinbuchstaben enthalten. | Kennwörter müssen 3 der folgenden 4 Zeichen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. Stellen Sie sicher, dass Ihr Kennwort diese erfüllt. |
| 4 | Das Kennwort darf numerische Zeichen enthalten. | Kennwörter müssen 3 der folgenden 4 Zeichen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. Stellen Sie sicher, dass Ihr Kennwort diese erfüllt. |
| 5 | Das Kennwort muss Zeichen enthalten. | Kennwörter müssen 3 der folgenden 4 Zeichen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. Stellen Sie sicher, dass Ihr Kennwort diese erfüllt. |
| 6 | Das Kennwort muss 4 Zeichen folgende 3: Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen. | Ihr Kennwort enthält keine erforderlichen Typen von Zeichen. Stellen Sie sicher, dass Ihr Kennwort diese erfüllt. |
| 7 | Parameter entspricht nicht bestätigen. | Stellen Sie sicher, dass alle, Ihr Kennwort erfüllt richtig eingegeben. |
| 8 | Ihr Kennwort darf nicht mit dem Standardwert übereinstimmen. | Das Standardkennwort lautet *Kennwort1*. Sie müssen dieses Kennwort ändern, nachdem Sie zum ersten Mal anmelden. |
| 9 | Das eingegebene Kennwort entspricht nicht das Gerätekennwort ein. Geben Sie das Kennwort erneut ein. | Überprüfen Sie das Kennwort, und geben Sie ihn erneut. |

Kennwörter werden gesammelt, bevor das Gerät registriert ist, aber nur nach erfolgreicher Registrierung gelten. Kennwort Wiederherstellung Workflow muss das Gerät registriert werden. 

> [AZURE.IMPORTANT] Im Allgemeinen schlägt der Versuch, ein Kennwort anwenden versucht die Software wiederholt das Kennwort sammeln, bis er erfolgreich ist. In seltenen Fällen kann das Kennwort angewendet werden. In diesem Fall Sie das Gerät registrieren und fortfahren, aber die Kennwörter werden nicht geändert. Erhalten Sie keinen Hinweis darauf, welche war – das Administratorkennwort Gerät oder das Kennwort StorSimple Snapshot-Manager. In diesem Fall sollten Sie beide Kennwörter ändern.

Sie können die Kennwörter im klassischen Azure-Portal über den Dienst StorSimple Manager zurücksetzen. Weitere Informationen finden Sie auf: 

- [Ändern des Administratorkennwortes Gerät](storsimple-change-passwords.md#change-the-device-administrator-password).
- [StorSimple Snapshot-Manager-Kennwort ändern](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Fehler bei der geräteregistrierung

Sie verwenden im Microsoft Azure StorSimple Manager-Dienst das Gerät. Sie können eine oder mehrere der folgenden Probleme während der Registrierung des Geräts auftreten.

| Nein.| Fehlermeldung | Mögliche Ursachen | Empfohlene Aktion |
| ---| ------------- | --------------- | ------------------ |
| 1  | : 350027 Fehler das Gerät mit StorSimple Manager zu registrieren. |  | Warten Sie einige Minuten, und versuchen Sie es dann erneut. Wenn das Problem weiterhin besteht, [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md).|
| 2  | Fehler 350013: Fehler bei der Registrierung des Geräts. Dies kann durch falsche Dienstregistrierungsschlüssel sein. | | Registrieren Sie das Gerät erneut mit dem richtigen Service-Registrierungsschlüssel. Weitere Informationen finden Sie unter [Abrufen der Dienstregistrierungsschlüssel.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | 350063 Authentifizierung StorSimple Manager-Dienst übergeben, aber Registrierung Fehler. Wiederholen Sie den Vorgang später. | Dieser Fehler gibt an, dass Authentifizierung mit ACS Ablauf der Registeraufruf an den Dienst ist fehlgeschlagen. Dies kann ein Ergebnis einen sporadischen Netzwerkfehler sein. | Wenn das Problem weiterhin besteht, wenden Sie sich an [Microsoft Support Services](storsimple-contact-microsoft-support.md). |
| 4 | Fehler 350049: Der Dienst konnte nicht während der Registrierung erreicht. | Bei Aufruf des Dienstes wird eine Webausnahme empfangen. In einigen Fällen dadurch möglicherweise behoben mit den Vorgang später wiederholen. | Überprüfen Sie die IP-Adresse und DNS-Namen, und wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, [wenden Sie sich an Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Fehler 350031: Das Gerät ist bereits registriert. | | Keine Aktion erforderlich. |
| 6 | 350016 Geräteregistrierung Fehler. | |Stellen Sie sicher, dass der Registrierungsschlüssel korrekt ist. |
| 7 | Rufen Sie HcsSetupWizard: Fehler beim Registrieren des Geräts; Dies kann durch falsche IP-Adresse oder DNS-Name sein. Netzwerkkonfiguration überprüfen, und versuchen Sie es erneut. Wenn das Problem weiterhin besteht, [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md). (Fehler 350050) | Stellen Sie sicher, dass das Gerät im externe Netzwerk anpingen kann. Wenn Sie keine Verbindung zum externen Netzwerk verfügen, kann die Registrierung mit diesem Fehler fehlschlagen. Dieser Fehler kann aus mindestens einem der folgenden sein:<ul><li>Falsche IP</li><li>Falsche Subnetz</li><li>Falscher gateway</li><li>Falsche DNS-Clienteinstellungen</li></ul> | Schritte im [schrittweise Problembehandlung wird](#step-by-step-storsimple-troubleshooting-example)angezeigt. |
| 8 | Rufen Sie HcsSetupWizard: Der aktuelle Vorgang fehlgeschlagen aufgrund eines internen Fehlers [0x1FBE2] Wiederholen Sie den Vorgang später. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. | Dies ist ein allgemeiner Fehler, Dienst oder Agent für alle Benutzer unsichtbar Fehler ausgelöst. Die häufigste Ursache möglicherweise die ACS-Authentifizierung fehlgeschlagen ist. Eine mögliche Ursache für diesen Fehler ist, Probleme mit NTP-Server-Konfiguration auf dem Gerät ist nicht korrekt eingestellt. | Korrigieren Sie die Zeit (Wenn Probleme) und dann Registrierung erneut. Verwenden Sie den Befehl Set-HcsSystem - Zeitzone die Zeitzone einstellen, nutzen Sie jedes Wort in der Zeitzone (beispielsweise "Pacific Standard Time").  Wenn dieses Problem weiterhin besteht, Schritte [Kontakt Microsoft Support](storsimple-contact-microsoft-support.md) weiter. |
| 9 | Warnung: Das Gerät konnte nicht aktiviert werden. Den Geräteadministrator und StorSimple Snapshot-Manager Kennwörter wurden nicht geändert. | Schlägt die Registrierung fehl, werden den Geräteadministrator und StorSimple Snapshot-Manager Kennwörter nicht geändert. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Tools zur Bereitstellung von StorSimple

StorSimple enthält mehrere Tools, mit denen Sie Ihre Lösung StorSimple beheben. Dazu gehören:

- Supportpakete und Geräteprotokolle 
- Cmdlets, die speziell für die Problembehandlung 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Pakete und Geräteprotokolle für die Problembehandlung unterstützen

Support-Paket enthält alle relevanten Protokolle, die das Microsoft Support-Team mit Gerät Problemen helfen können. Windows PowerShell für StorSimple können verschlüsselte Supportpaket generieren, die dann Supportpersonal freigeben können.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Anzeigen der Protokolle oder den Inhalt des Pakets support

1. Verwenden Sie Windows PowerShell für StorSimple zu einem Support-Paket unter [Erstellen und Verwalten einer Support-Paket](storsimple-create-manage-support-package.md).

2. Die [Entschlüsselung Skript](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokal auf dem Clientcomputer herunterladen.

3. Verwenden Sie diese [Anleitung](storsimple-create-manage-support-package.md#edit-a-support-package) zum Öffnen und Entschlüsseln das Supportpaket.

4. Paketprotokolle entschlüsselten Support sind in Etw-Etvx-Format. Führen Sie die folgenden Schritte zum Anzeigen dieser Dateien in der Windows-Ereignisanzeige:
  1. **Befehl** auf Ihrem Windows-Client ausführen. Die Ereignisanzeige wird gestartet.
  2. Klicken Sie im **Aktionsbereich** auf **Gespeicherte Protokolldatei öffnen** und auf die Protokolldateien im Etvx-Etw-Format (das Supportpaket). Sie können die Datei jetzt anzeigen. Nach dem Öffnen der Datei können Sie mit der rechten Maustaste und speichern Sie die Datei als Text.
   
    > [AZURE.IMPORTANT] Das Cmdlet " **Get-WinEvent** " können Sie diese Dateien in Windows PowerShell öffnen. Weitere Informationen finden Sie in der Dokumentation der Windows PowerShell-Cmdlet [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) .

5. Suchen Sie die Protokolle in der Ereignisanzeige öffnen, nach die folgenden Protokolle, die Probleme im Zusammenhang mit der Konfiguration enthalten:

  - Hcs_pfconfig/Operational Protokoll
  - Hcs_pfconfig-Konfiguration

6. Suchen Sie in den Protokolldateien Zeichenfolgen beziehen die Cmdlets von der Setup-Assistent aufgerufen. Finden Sie eine Liste dieser Cmdlets [erstmaligen Installationsvorgangs Assistenten](#first-time-setup-wizard-process) . 

7. Wenn Sie nicht die Ursache des Problems herausfinden können, können Sie für weitere Schritte [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) . Gehen Sie in [eine Support-Anfrage erstellen](storsimple-contact-microsoft-support.md#create-a-support-request) , wenn Sie Microsoft-Support wenden.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlets zur Problembehandlung

Verwenden Sie die folgenden Windows PowerShell-Cmdlets Konnektivitätsfehler erkennen.

- `Get-NetAdapter`: Verwenden Sie dieses Cmdlet die Integrität der Netzwerkschnittstellen erkennen. 

- `Test-Connection`: Verwenden Sie dieses Cmdlet die Netzwerkkonnektivität innerhalb und außerhalb des Netzwerks überprüfen.

- `Test-HcsmConnection`: Verwenden Sie dieses Cmdlet Verbindung erfolgreich registrierte Geräte überprüft.

Wenn Sie Update 1 auf dem StorSimple-Gerät ausführen, stehen folgende Diagnose Cmdlets.

- `Sync-HcsTime`: Verwenden Sie dieses Cmdlet Gerät anzeigen und eine Synchronisierung mit dem NTP-Server.

- `Enable-HcsPing`und `Disable-HcsPing`: Diese Cmdlets Hosts Ping Netzwerkschnittstellen auf dem StorSimple-Gerät zu verwenden. Standardmäßig reagieren StorSimple Netzwerkschnittstellen nicht auf Ping-Anfragen.

- `Trace-HcsRoute`: Verwenden Sie dieses Cmdlet als Tool zum Verfolgen von Routen. Sendet Pakete an alle Router auf dem Weg zu einem Ziel einen Zeitraum und berechnet Ergebnisse auf Grundlage der von den einzelnen Hops zurückgegebenen Pakete. Da `Trace-HcsRoute` zeigt den Paketverlust bei jedem Router und jeder Verbindung können Sie feststellen, welche Router oder Verbindungen möglicherweise Netzwerkprobleme verursacht. 

- `Get-HcsRoutingTable`: Verwenden Sie dieses Cmdlet die lokale IP-Routingtabelle angezeigt.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Das Cmdlet "Get-NetAdapter" beheben

Beim Konfigurieren von Netzwerkschnittstellen Gerät erstmalige Bereitstellung steht der Hardwarestatus nicht in der StorSimple Manager-Dienst UI da das Gerät noch nicht mit dem Dienst registriert ist. Außerdem kann Hardware Statusseite nicht immer korrekt Zustand das Gerät wider besonders wenn Probleme bei der Synchronisierung Service auswirken. In diesen Fällen können Sie die `Get-NetAdapter` Cmdlet bestimmt den Zustand und Status der Netzwerkschnittstellen.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Eine Liste aller Netzwerkadapter auf dem Gerät anzeigen

1. Starten Sie Windows PowerShell für StorSimple, und geben Sie `Get-NetAdapter`. 

2. Verwenden Sie die Ausgabe der `Get-NetAdapter` Cmdlet und folgt den Status der Netzwerkschnittstelle.
  - Wenn die Schnittstelle fehlerfrei und aktiviert ist, wird als **von** **IfIndex** Status angezeigt.
  - Wenn die Schnittstelle ist nicht physisch miteinander verbunden (über ein Netzwerkkabel) ist fehlerfrei, ist **IfIndex** als **deaktiviert**angezeigt.
  - Wenn die Schnittstelle fehlerfrei, aber nicht aktiviert ist, wird der Status **IfIndex** als **NotPresent**angezeigt.
  - Wenn die Schnittstelle nicht vorhanden ist, wird es nicht in dieser Liste angezeigt. StorSimple-Verwaltungsdienst Benutzeroberfläche zeigt weiterhin diese Schnittstelle ausgefallen.

Weitere Informationen zur Verwendung dieses Cmdlet finden Sie [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) in den Windows PowerShell-Cmdlet. 

Die folgenden Abschnitte zeigen Beispiele aus der `Get-NetAdapter` Cmdlet. 

 In diesen Beispielen Controller 0 wurde der passiven Controller und wurde wie folgt konfiguriert:

- Daten 0 Daten 1, Daten 2 und Daten 3 Netzwerk Schnittstellen auf dem Gerät vorhanden.
- Daten 4 und 5 Daten Netzwerkschnittstellenkarten waren nicht vorhanden; Daher sind sie nicht in der Ausgabe aufgeführt.
- Daten 0 wurde aktiviert.

Domänencontroller 1 wurde active Controller und wurde wie folgt konfiguriert:

- Daten 0, Daten 1, Daten 2, Daten 3, 4 Daten und Daten 5 Netzwerk Schnittstellen auf dem Gerät vorhanden.
- Daten 0 wurde aktiviert.

**Beispiel-Ausgabe-Controller 0**

Die Ausgabe vom Controller (passive Controller) 0 lautet. Daten 1, Daten 2 und Daten 3 sind nicht verbunden. Daten 4 und 5 Daten werden nicht aufgelistet, da sie nicht auf dem Gerät vorhanden sind. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Beispiel-Ausgabe-Controller 1**

Die Ausgabe vom Controller 1 (aktive Controller) lautet. Nur Daten 0 Netzwerkschnittstelle auf dem Gerät konfiguriert und arbeiten.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Das Cmdlet "Verbindung testen" beheben

Sie können die `Test-Connection` -Cmdlet zum bestimmen, ob das Gerät StorSimple mit dem externen Netzwerk herstellen kann. Wenn alle die Netzwerkparameter, einschließlich DNS, im Setup-Assistenten korrekt konfiguriert sind, können Sie die `Test-Connection` Cmdlet Ping eine bekannte Adresse außerhalb des Netzwerks wie outlook.com. 

Aktivieren Sie Ping zur Problembehandlung bei Verbindungsproblemen mit diesem Cmdlet Wenn Ping deaktiviert ist.

Siehe die folgenden Beispiele aus der `Test-Connection` Cmdlet. 

> [AZURE.NOTE] Im ersten Beispiel wird das Gerät mit einem falschen DNS konfiguriert. Im zweiten Beispiel ist der DNS richtig.
 
**Beispiel-Ausgabe – falsche DNS**

Im folgenden Beispiel wird keine Ausgabe für IPv4- und IPv6-Adressen gibt an, dass der DNS nicht aufgelöst. Dies bedeutet, dass keine mit dem externen Netzwerk Verbindung und korrekte DNS bereitgestellt werden. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Beispielausgabe – DNS korrigieren**

Im folgenden Beispiel gibt das DNS die IPv4-Adresse, dass DNS ordnungsgemäß konfiguriert ist. Dies bestätigt, dass die Verbindung mit dem externen Netzwerk. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Mit dem Test-HcsmConnection-Cmdlet beheben

Verwenden der `Test-HcsmConnection` -Cmdlet für ein Gerät, das bereits verbunden und StorSimple Manager-Dienst registriert. Dieses Cmdlet können Sie die Konnektivität zwischen einem registrierten Gerät und den entsprechenden StorSimple Manager-Dienst überprüfen. Sie können diesen Befehl auf Windows PowerShell für StorSimple ausführen. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Test-HcsmConnection-Cmdlet ausführen

1. Stellen Sie sicher, dass das Gerät registriert ist.

2. Überprüfen Sie Gerät. Wird das Gerät im Wartungsmodus oder offline deaktiviert ist möglicherweise die folgenden Fehler angezeigt: 

   - ErrorCode.CiSDeviceDecommissioned – gibt an, dass das Gerät deaktiviert wird.
   - ErrorCode.DeviceNotReady – gibt an, dass das Gerät im Wartungsmodus befindet.
   - ErrorCode.DeviceNotReady – gibt an, dass das Gerät nicht online ist.

3. Stellen Sie sicher, dass der StorSimple Manager-Dienst ausgeführt wird (verwenden Sie das Cmdlet " [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) "). Wenn der Dienst nicht ausgeführt wird, möglicherweise die folgenden Fehler angezeigt:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – bedeutet dies, dass Ausnahmefehler ClusterResource erhalten haben.

4. Überprüfen Sie das Token Access Control Service (ACS). Wenn eine Webausnahme ausgelöst, möglicherweise kein Gateway, fehlende Proxyauthentifizierung, falsche DNS oder ein Authentifizierungsfehler aus. Sie können die folgenden Fehler angezeigt:

   - ErrorCode.CiSApplianceGateway – zeigt die HttpStatusCode.BadGateway Ausnahme: der Namensauflösungsdienst konnte den Hostnamen nicht auflösen. 
   - ErrorCode.CiSApplianceProxy – gibt die Ausnahme HttpStatusCode.ProxyAuthenticationRequired (HTTP-Statuscode 407): der Client konnte nicht mit dem Proxyserver authentifizieren. 
   - ErrorCode.CiSApplianceDNSError – gibt die WebExceptionStatus.NameResolutionFailure-Ausnahme: der Namensauflösungsdienst konnte den Hostnamen nicht auflösen.
   - ErrorCode.CiSApplianceACSError – gibt an, dass lieferte einen Authentifizierungsfehler Verbindung.
   
    Prüfen Sie keine Webausnahme ausgelöst wird, ErrorCode.CiSApplianceFailure. Dies bedeutet, dass die Appliance ist fehlgeschlagen.

5. Überprüfen Sie die Cloud Service-Konnektivität. Wenn der Dienst eine Webausnahme auslöst, wird möglicherweise die folgenden Fehler angezeigt:

  - ErrorCode.CiSApplianceGateway – gibt die Ausnahme HttpStatusCode.BadGateway: ein zwischengeschalteten Proxyserver von einen Proxy oder vom ursprünglichen Server eine ungültige Anforderung erhalten.
  - ErrorCode.CiSApplianceProxy – gibt die Ausnahme HttpStatusCode.ProxyAuthenticationRequired (HTTP-Statuscode 407): der Client konnte nicht mit dem Proxyserver authentifizieren. 
  - ErrorCode.CiSApplianceDNSError – gibt die WebExceptionStatus.NameResolutionFailure-Ausnahme: der Namensauflösungsdienst konnte den Hostnamen nicht auflösen.
  - ErrorCode.CiSApplianceACSError – gibt an, dass lieferte einen Authentifizierungsfehler Verbindung.
  
    Prüfen Sie keine Webausnahme ausgelöst wird, ErrorCode.CiSApplianceSaasServiceError. Dies weist auf ein Problem mit der StorSimple Manager-Dienst.
 
6. Überprüfen Sie die Azure Service Bus-Verbindung. ErrorCode.CiSApplianceServiceBusError gibt an, dass Service Bus das Gerät nicht möglich.
 
Die Protokolldateien CiSCommandletLog0Curr.errlog und CiSAgentsvc0Curr.errlog müssen Informationen wie Ausnahmeinformationen. 

Weitere Informationen dazu, wie Sie das Cmdlet verwenden, fahren Sie mit [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) in Windows PowerShell-Referenzdokumentation.

> [AZURE.IMPORTANT] Sie können dieses Cmdlet für die aktiven und passiven Controller ausführen. 
 
Siehe die folgenden Beispiele aus der `Test-HcsmConnection` Cmdlet. 

**Beispielausgabe – erfolgreich registrierte Geräte mit StorSimple-Version (Juli 2014)**

Das erste Beispiel ist ein Gerät, das mit der StorSimple Manager-Dienst registriert und verfügt über keine Konnektivitätsprobleme. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Ausgabe – erfolgreich registrierte Gerät unter StorSimple Update 1**

Wenn Sie auf Ihrem Gerät StorSimple Update 1 ausführen, müssen Sie nicht mit der ausführlichen Option ausführen.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Beispielausgabe – offline Gerät StorSimple Version (Juli 2014)**

In diesem Beispiel wird von einem Gerät mit dem Status **Offline** im klassischen Azure-Portal.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Das Gerät konnte keine Verbindung mit der aktuellen Web Proxy-Konfiguration. Dies könnte ein Problem mit der Web-Proxy-Konfiguration oder ein Netzwerkkonnektivitätsproblem. In diesem Fall sollten Sie sicherstellen, dass die Web-Proxyeinstellungen korrekt sind und der Web-Proxy-Server online und erreichbar. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Sync-HcsTime-Cmdlet beheben

Verwenden Sie dieses Cmdlet Gerät Uhrzeit angezeigt. Wenn das Gerät einen Versatz mit dem NTP-Server hat, dann können dieses Cmdlet Sie Kraft-Zeit mit dem NTP-Server synchronisieren. Ist der Abstand zwischen Gerät und NTP-Server mehr als 5 Minuten, wird eine Warnung angezeigt. Wenn der Offset 15 Minuten überschreitet, wird das Gerät offline gehen. Dieses Cmdlet können weiterhin eine Synchronisierung erzwingen. Jedoch überschreitet der Offset 15 Stunden werden dann Sie Synchronisierung erzwingen nicht die Zeit und eine Fehlermeldung wird angezeigt.

**Ausgabe – erzwungene Synchronisierung mit HcsTime synchronisieren**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Problembehandlung bei HcsPing aktivieren und deaktivieren HcsPing Cmdlets

Verwenden Sie diese Cmdlets um sicherzustellen, dass die Netzwerkschnittstellen auf dem Gerät auf ICMP-Ping-Anfragen reagieren. Standardmäßig reagieren StorSimple Netzwerkschnittstellen nicht auf Ping-Anfragen. Mit diesem Cmdlet ist am einfachsten zu wissen, ob das Gerät online und erreichbar ist.  

**Ausgabe – HcsPing aktivieren und deaktivieren HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Trace-HcsRoute-Cmdlet beheben

Verwenden Sie dieses Cmdlet als Tool zum Verfolgen von Routen. Sendet Pakete an alle Router auf dem Weg zu einem Ziel einen Zeitraum und berechnet Ergebnisse auf Grundlage der von den einzelnen Hops zurückgegebenen Pakete. Da das Cmdlet den Paketverlust bei jedem Router und jeder Verbindung anzeigt, können Sie feststellen, welche Router oder Links möglicherweise Netzwerkprobleme verursacht.

**Beispielausgabe zeigt, wie die Route eines Pakets mit Trace-HcsRoute verfolgen**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Das Cmdlet "Get-HcsRoutingTable" beheben

Verwenden Sie dieses Cmdlet die routing-Tabelle für Ihr StorSimple-Gerät anzeigen. Routing-Tabelle ist ein Satz von Regeln, die ermitteln können, von Datenpaketen über ein Netzwerk IP (Internet Protocol) weitergeleitet werden. 

Die Routingtabelle zeigt die Schnittstellen und Gateways, die Daten an den angegebenen Netzwerken weiterleitet. Es gibt auch die Routingmetrik die Entscheidungsträger für den Pfad zu einem bestimmten Ziel. Je niedriger die Routingmetrik, je höher die Einstellung. 

Angenommen, Sie haben 2 Netzwerkschnittstellen Daten 2 und Daten 3 mit dem Internet verbunden. Wenn die Routingmetrik für Daten 2 und Daten 3 15 und 261 sind, ist Daten 2 mit niedrigeren Routingmetrik die bevorzugte Schnittstelle mit dem Internet herstellen.

Wenn Sie auf Ihrem Gerät StorSimple Update 1 ausgeführt, hat Ihre Daten 0 Netzwerkschnittstelle die höchste Einstellung für den clouddatenverkehr Dies impliziert, dass auch wenn andere Cloud-fähigen Schnittstellen sind clouddatenverkehr Daten 0 weitergeleitet wird. 

Beim Ausführen der `Get-HcsRoutingTable` Cmdlet ohne Parameter (wie im folgenden Beispiel dargestellt) das Cmdlet IPv4- und IPv6-Routingtabellen ausgegeben wird. Können auch `Get-HcsRoutingTable -IPv4` oder `Get-HcsRoutingTable -IPv6` zu einer entsprechenden Routingtabelle.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Schrittweise Problembehandlung StorSimple-Beispiel

Das folgende Beispiel zeigt schrittweise Problembehandlung bei der Bereitstellung StorSimple. In diesem Beispielszenario schlägt fehl mit Fehlermeldung, dass die Einstellungen oder der DNS-Name stimmt geräteregistrierung.

Die Fehlermeldung ist:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Der Fehler kann folgende Ursachen haben:

- Falsche Hardwareinstallation
- Fehlerhafte Netzwerk-Schnittstellen
- Falsche IP-Adresse, Subnetzmaske, Gateway, primären DNS-Server oder Web-proxy
- Falscher Schlüssel
- Falsche Firewall settings

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Lokalisierung und Behebung der Registrierung Geräteproblem

1. Überprüfen Sie Ihre Gerätekonfiguration: aktive Domänencontroller ausführen `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Der Setup-Assistent muss auf dem aktiven Domänencontroller ausgeführt. Überprüfen Sie aktive Domänencontroller verbunden sind, betrachten Sie das Banner in der seriellen Konsole angezeigt. Das Banner gibt an, ob 0 oder 1 Controllers besteht und ob der Controller aktiv oder Passiv ist. Weitere Informationen finden Sie auf [einem aktiven Domänencontroller auf Ihrem Gerät identifizieren](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Stellen Sie sicher, dass das Gerät korrekt verkabelt: Überprüfen Sie das Netzwerk auf dem Gerät wieder Ebene Verkabelung. Die Verkabelung ist spezifisch für das Modell. Weitere Informationen finden Sie, [Installieren Sie das Gerät StorSimple 8100](storsimple-8100-hardware-installation.md) oder [Geräts StorSimple 8600](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Wenn Sie 10 Gigabit-Netzwerk-Ports verwenden, müssen Sie bereitgestellten QSFP SFP-Adapter und SFP-Kabel verwenden. Weitere Informationen finden Sie in der [Liste der Kabel, Switches und Transceiver vom OEM Lieferanten Mellanox Ports empfohlen](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Überprüfen Sie den Status der Netzwerkschnittstelle:

   - Verwenden Sie das Cmdlet "Get-NetAdapter" um der Status der Netzwerkschnittstellen Daten 0 zu erkennen. 
   - Wenn der Link nicht funktioniert, zeigen **Ifindex** Status, dass die Schnittstelle heruntergefahren ist. Sie müssen die Netzwerkschnittstelle des Ports an die Einheit und die Option überprüfen. Sie benötigen auch fehlerhafte Kabel auszuschließen. 
   - Wenn Sie vermuten, dass Daten 0 Port auf dem aktiven Domänencontroller ausgefallen ist, Sie dies überprüfen können Daten 0 Port 1 Controller anschließen. Zum bestätigen, trennen Sie das Netzwerkkabel von der Rückseite des Geräts vom Controller 0 Kabel Controller 1 und führen Sie das Cmdlet "Get-NetAdapter" erneut aus. 
   Wenn Daten 0 an port ein Domänencontroller schlägt fehl, für weitere Schritte [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) . Sie müssen den Controller in Ihrem System ersetzen.
 
4. Überprüfen Sie die Verbindung zum Switch:
   - Stellen Sie sicher, dass Daten 0 Netzwerkschnittstellen auf 0 und 1-Controller in der primären Einheit im selben Subnetz sind. 
   - Überprüfen Sie den Hub oder Router. In der Regel sollten Sie beide Controller mit dem gleichen Hub oder Router verbinden. 
   - Stellen Sie sicher, dass die Optionen für die Verbindung verwendeten Daten 0 für beide Controller im gleichen vLAN.
   
5. Vermeiden Sie Benutzerfehler:

  - Führen Sie den Assistenten erneut (run **Invoke-HcsSetupWizard**) und geben Sie die Werte wieder, um sicherzustellen, dass keine Fehler vorhanden sind. 
  - Überprüfen Sie die Registrierung Schlüssel verwendet. Der gleichen Registrierungsschlüssel kann auf mehrere Geräte an einen StorSimple-Manager-Dienst verwendet werden. Gehen Sie vor in [Dienst-Registrierungsschlüssel zu erhalten](storsimple-manage-service.md#get-the-service-registration-key) , um sicherzustellen, dass Sie den richtigen Registrierungsschlüssel verwenden.

    > [AZURE.IMPORTANT] Haben Sie mehrere Dienste ausführen, müssen Sie sicherstellen, dass der Registrierungsschlüssel für den entsprechenden Dienst verwendet wird, um das Gerät zu registrieren. Wenn Sie ein Gerät mit dem falschen StorSimple Manager-Dienst registriert haben, müssen Sie für weitere Schritte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) . Möglicherweise setzen Sie Factory des Geräts (der Datenverlust führen) dann auf den gewünschten Dienst herstellen.

6. Verwenden Sie das Cmdlet "Verbindung testen", überprüfen Sie die Konnektivität mit dem externen Netzwerk. Weitere Informationen finden Sie auf [Problembehandlung mit dem Cmdlet Verbindung testen](#troubleshoot-with-the-test-connection-cmdlet).

7. Firewall Durchdringung prüfen. Wenn Sie überprüft haben die virtuellen IP-Adresse (VIP), Subnetzmaske, Gateway und DNS-Einstellungen alle richtig sind und Verbindungsprobleme weiterhin angezeigt und es ist möglich, dass Ihre Firewall die Kommunikation zwischen dem Gerät und dem externen Netzwerk blockiert. Sie müssen sicherstellen, dass die Ports 80 und 443 auf dem StorSimple-Gerät für die ausgehende Kommunikation verfügbar sind. Weitere Informationen finden Sie unter [Netzwerk an das Gerät StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Überprüfen Sie die Protokolle. Zur [Support-Pakete und Geräteprotokolle für die Problembehandlung](#support-packages-and-device-logs-available-for-troubleshooting).

9. Wenn die vorhergehenden Schritte nicht das Problem und [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) , um Hilfe lösen.

## <a name="next-steps"></a>Nächste Schritte
[Informationen zum Betrieb Gerät zu lösen](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
