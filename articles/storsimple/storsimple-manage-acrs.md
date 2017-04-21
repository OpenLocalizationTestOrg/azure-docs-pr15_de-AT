<properties 
   pageTitle="Verwalten von Access Control Einträgen in StorSimple | Microsoft Azure"
   description="Beschreibt, wie Access Control Datensätze (ACRs) bestimmt, welche Hosts auf ein Volume zugreifen können auf dem Gerät StorSimple verwenden."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Verwenden des StorSimple Manager-Dienstes Zugriff Steuerelement Datensätze verwalten

## <a name="overview"></a>Übersicht

Access Control Datensätze (ACRs) können Sie angeben, welche Hosts auf ein Volume auf dem StorSimple-Gerät herstellen können. ACRs sind auf einen bestimmten Datenträger und iSCSI-qualifizierte Namen (IQNs) Hosts enthalten. Wenn ein Host versucht, einen Datenträger herstellen, überprüft das Gerät ACR zugeordnet, Volume IQN-Namen und ist eine Übereinstimmung, die Verbindung wird hergestellt. Bereich Datensätze Access auf der Seite **Konfigurieren** zeigt alle Datensätze Access Control mit der entsprechenden IQNs Hosts.

In diesem Lernprogramm wird folgenden allgemeinen ACR bezogene Aufgaben erläutert:

- Hinzufügen eines Steuerelements Zugriff 
- Bearbeiten eines Datensatzes Access control 
- Löschen eines Datensatzes Access control 

> [AZURE.IMPORTANT] 
> 
> - Wenn ein Volume ein ACR zuweisen, achten Sie die Lautstärke von mehr als einem nicht gruppierten Server nicht gleichzeitig zugegriffen werden kann, da dies das Volume beschädigt. 
> - Beim Löschen eines ACR von einem Volume stellen Sie sicher, dass der entsprechende Host nicht das Volume zugreift, da das Löschen einer schreibgeschützten Unterbrechung führen.

## <a name="add-an-access-control-record"></a>Hinzufügen eines Steuerelements Zugriff

Mithilfe die Seite StorSimple Manager Service **Konfigurieren** ACRs hinzufügen. In der Regel ordnen Sie eine ACR mit ein.

Die folgenden Schritte ein ACR hinzufügen.

#### <a name="to-add-an-access-control-record"></a>Einen Datensatz auf Steuerelement hinzufügen

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfigurieren** .

2. Tabellarische Liste unter **Access Control-Datensätze**Geben Sie einen **Namen** für Ihre ACR.

3. Geben Sie die IQN des Windows Hosts unter **iSCSI-Initiatornamen**. Zum Abrufen der IQN des Windows Server-Hosts wie folgt:

   - Starten Sie Microsoft iSCSI-Initiator auf dem Windows-Server.
   - Markieren Sie im Fenster **iSCSI Initiator Properties** auf der Registerkarte **Konfiguration** und kopieren Sie die Zeichenfolge aus dem Feld **Initiator** .
   - Fügen Sie dieser Zeichenfolge im Feld **iSCSI-Initiatornamen** ACRs Tabelle im klassischen Azure-Portal.

4. Klicken Sie auf **Speichern** , um die neu erstellte ACR speichern. Tabellarische Liste wird aktualisiert, um diese Ergänzung widerzuspiegeln.

## <a name="edit-an-access-control-record"></a>Bearbeiten eines Datensatzes Access control

Mithilfe die Seite **Konfigurieren** im klassischen Azure-Portal ACRs bearbeiten. 

> [AZURE.NOTE] Sie können diese ACRs, die zurzeit nicht verwendet. Bearbeiten ein ACR zugeordnete ein Volume, das momentan verwendet wird müssen Sie zunächst das Volume offline.

Führen Sie die folgenden Schritte zum Bearbeiten eines ACR.

#### <a name="to-edit-an-access-control-record"></a>Einen Datensatz auf Steuerelement bearbeiten

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfigurieren** .

2. Bewegen Sie Tabellarische Liste der Access Control Datensätze über ACR, die Sie ändern möchten.

3. Geben Sie einen neuen und/oder IQN der ACR.

4. Klicken Sie auf **Speichern** , um die geänderte ACR speichern. Tabellarische Liste wird entsprechend aktualisiert.

## <a name="delete-an-access-control-record"></a>Löschen eines Datensatzes Access control

Mithilfe die Seite **Konfigurieren** im klassischen Azure-Portal ACRs löschen. 

> [AZURE.NOTE] Sie können diese ACRs, die zurzeit nicht verwendet. Um eine ACR zugeordnete ein Volume, das momentan verwendet wird löschen, müssen Sie zunächst das Volume offline.

Führen Sie die folgenden Schritte aus, um eine Access Control löschen.

#### <a name="to-delete-an-access-control-record"></a>So löschen Sie einen Zugriffsdatensatz Steuerelement

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfigurieren** .

2. Bewegen Sie die Tabellarische Liste der Access Control Datensätze (ACRs) über ACR, die Sie löschen möchten.

3. Löschsymbol (**X**) erscheint in der rechten Spalte extreme ACR, die Sie auswählen. Klicken Sie auf **X** , um die ACR löschen.

4. Zur Bestätigung aufgefordert werden, klicken Sie auf **Ja,** um den Löschvorgang fortzusetzen. Tabellarische Liste aktualisiert die Streichung.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple-Volumes verwalten](storsimple-manage-volumes.md).

- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
 
