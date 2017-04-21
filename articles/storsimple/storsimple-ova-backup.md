<properties 
   pageTitle="StorSimple Virtual Array backup Tutorial | Microsoft Azure"
   description="Beschreibt das StorSimple Virtual Array Freigaben und Volumes sichern."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Sichern Sie Ihre Virtual Array StorSimple

## <a name="overview"></a>Übersicht 

Diese Anleitung gilt für Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple lokalen virtuellen Gerät oder virtuelles Gerät StorSimple) ausgeführten März 2016 allgemein verfügbar (GA) Version oder höher.

StorSimple Virtual Array ist ein hybrider Cloud lokalen virtuellen Speichergerät, die als Dateiserver oder ein iSCSI-Server konfiguriert werden kann. Sie können Sicherungskopien aus einer Sicherung wiederherstellen und Device-Failover durchführen, wenn Disaster Recovery erforderlich ist. Als Dateiserver konfiguriert, kann auch auf Recovery. In diesem Lernprogramm beschreibt mit klassischen Azure-Portal oder StorSimple Webbenutzeroberfläche geplant und manuell Sichern Ihres virtuellen StorSimple-Arrays.


## <a name="back-up-shares-and-volumes"></a>Sichern von Freigaben und volumes

Backups Point-in-Time-Schutz, Verbesserung der Recovery-Fähigkeit und Minimierung der Wiederherstellungszeiten Freigaben und Volumes. Sie sichern Sie eine Freigabe oder ein Volume auf dem Gerät StorSimple auf zwei Arten: **geplant** oder **manuell**. Jede der Methoden wird in den folgenden Abschnitten erläutert.

> [AZURE.NOTE] In dieser Version werden geplante Backups durch eine Standardrichtlinie erstellt, die täglich zu einer bestimmten Zeit ausgeführt wird und alle Freigaben oder Volumes auf dem Gerät gesichert. Es ist nicht möglich, benutzerdefinierte Richtlinien für geplante Backups erstellen.

## <a name="change-the-backup-schedule"></a>Ändern Sie den Sicherungszeitplan

Virtuelle StorSimple-Gerät hat eine Standardrichtlinie backup, die zu einem bestimmten Zeitpunkt des Tages (22:30) beginnt und sichert alle Freigaben oder Volumes auf dem Gerät einmal täglich. Sie können die Zeit ändern, mit der Sicherung beginnt jedoch die Häufigkeit und Aufbewahrungsfristen (die Anzahl der Sicherungskopien beibehalten angibt) geändert werden können. Diese Backups ist das gesamte virtuelle Gerät gesichert; Wir empfehlen daher, diese Sicherungen Spitzenzeiten.

Die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) backup Standardanfangszeit ändern.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>So ändern Sie die Startzeit für die backup-Standardrichtlinie

1. Navigieren Sie zu der Registerkarte **Konfiguration** Gerät.

2. Geben Sie die Startzeit für die tägliche Sicherung Abschnitt **Backup** .

3. Klicken Sie auf **Speichern**.

### <a name="take-a-manual-backup"></a>Nehmen Sie eine manuelle Sicherung

Neben geplanten Backups nehmen manuell (auf Anforderung) backup jederzeit Sie.

#### <a name="to-create-a-manual-on-demand-backup"></a>Erstellen eine manuelle Sicherung (auf Anforderung)

1. Navigieren Sie zu der Registerkarte **Freigaben** oder **Volumes** .

2. Klicken Sie am unteren Rand der Seite auf **alle Backup**. Sie werden aufgefordert zu überprüfen, ob jetzt dauern soll. Klicken Sie auf Check ![Symbol überprüfen](./media/storsimple-ova-backup/image3.png) um die Sicherung fortzusetzen.

    ![Sicherung bestätigen](./media/storsimple-ova-backup/image4.png)

    Sie werden benachrichtigt, dass ein backup-Auftrag gestartet wird.

    ![Sicherung starten](./media/storsimple-ova-backup/image5.png)

    Sie werden benachrichtigt, dass das Projekt erfolgreich erstellt wurde.

    ![der Sicherungsauftrag wurde erstellt](./media/storsimple-ova-backup/image7.png)

3. Um die Arbeit zu verfolgen, klicken Sie auf **Auftrag anzeigen**.

4. Nach Abschluss des Sicherungsauftrags wechseln Sie zur Registerkarte **Sicherungskatalog** . Die abgeschlossene Sicherung sollte angezeigt werden.

    ![Abgeschlossenen Sicherung](./media/storsimple-ova-backup/image8.png)

5. Filterauswahl auf das entsprechende Gerät, Sicherungsrichtlinie und Zeitraum festgelegt, und klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-backup/image3.png).

    Die Sicherung sollte in der Liste Sicherungssätze angezeigt, die im Katalog angezeigt.

## <a name="view-existing-backups"></a>Vorhandene Sicherungskopien anzeigen

Die folgenden Schritte im klassischen Azure-Portal an vorhandenen Sicherungen.

#### <a name="to-view-existing-backups"></a>Vorhandene Sicherungskopien anzeigen

1. Klicken Sie auf der Seite StorSimple Manager auf **Backup** -Katalog.

2. Wählen Sie einen Sicherungssatz wie folgt:

    1. Wählen Sie das Gerät.

    2. Wählen Sie in der Dropdown-Liste die Freigabe oder das Volume für die Sicherung, die Sie auswählen möchten.

    3. Geben Sie den Zeitraum.

    4. Klicken Sie auf Check ![](./media/storsimple-ova-backup/image3.png) zum Ausführen dieser Abfrage.

    Die Sicherungskopien der ausgewählten Freigabe zugeordnet oder Volume sollte in der Liste der Sicherungssätze angezeigt.

![Video_icon](./media/storsimple-ova-backup/video_icon.png) **Video verfügbar**

Das Video zeigt, wie Sie Freigaben erstellen, Freigaben sichern und Wiederherstellen von Daten auf ein virtuelles Array StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [virtuelle StorSimple Array verwalten](storsimple-ova-web-ui-admin.md).
