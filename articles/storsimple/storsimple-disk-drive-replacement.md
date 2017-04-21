<properties 
   pageTitle="Ersetzen einer Festplatte auf einem Gerät StorSimple | Microsoft Azure"
   description="Erläutert das Ersetzen einer Festplatte auf eine primäre StorSimple-Einheit oder Anlage EBOD."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-a-disk-drive-on-your-storsimple-device"></a>Ersetzen einer Festplatte auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert das Entfernen und Ersetzen eine Fehlfunktion oder fehlerhafte Festplatte Microsoft Azure StorSimple-Gerät. Um ein Laufwerk ersetzen, müssen Sie:

- Lösen Sie die sabotagekontakt Sperre

- Entfernen der Festplatte

- Installieren Sie die Ersatz-Festplattenlaufwerk

>[AZURE.IMPORTANT] Entfernen und Austauschen einer Festplatte, die Sicherheitshinweise [StorSimple Hardware-Komponente](storsimple-hardware-component-replacement.md)ersetzen.

## <a name="disengage-the-antitamper-lock"></a>Lösen Sie die sabotagekontakt Sperre

Diese Prozedur beschreibt, wie sabotagekontakt Sperren auf dem Gerät StorSimple verriegelt oder geöffnet beim Ersetzen der Festplatten werden können. Sabotagekontakt Sperren werden in Laufwerk Carrier behandelt, und sie durch eine kleine Öffnung im Abschnitt Riegel des Handles erfolgt. Laufwerke werden mit Sperren setzen auf die gesperrte Position bereitgestellt.

#### <a name="to-unlock-the-antitamper-lock"></a>Sabotagekontakt Sperre entsperrt

1. Legen Sie die Taste ("manipulationssicher" T10 Klemmen, die von Microsoft bereitgestellte) in die Öffnung des Handles und der Steckdose. 

    >[AZURE.NOTE] Sabotagekontakt Sperre aktiviert ist, wird die rote Anzeige in der Öffnung sichtbar.

    ![Gesperrte Festplatte](./media/storsimple-disk-drive-replacement/IC741056.png)

    **Abbildung 1** Manipulationssichere Sperre beschäftigt

  	|Bezeichnung|Beschreibung|
  	|:----|:----------|
  	|1|Indikator Blende|
  	|2|Sabotagekontakt Sperren|

2. Drehen Sie den Schlüssel im Uhrzeigersinn bis rote Indikator nicht in die Öffnung über den Schlüssel angezeigt wird.

3. Entfernen Sie den Schlüssel.

    ![Nicht gesperrte Festplatte](./media/storsimple-disk-drive-replacement/IC741057.png)

    **Abbildung 2** Nicht gesperrte Festplatte

4. Das Laufwerk kann jetzt entfernt werden.

Führen Sie die Schritte in umgekehrter Reihenfolge der Sperre zu.

## <a name="remove-the-disk-drive"></a>Entfernen der Festplatte

Das Gerät StorSimple unterstützt eine RAID 10 wie Leerzeichen Speicherkonfiguration. Das bedeutet, sie können normalerweise mit einer fehlerhaften Festplatte Solid State Drive (SSD) oder Festplatte (HDD). 

>[AZURE.IMPORTANT]
>
>- Wenn Ihr System mehr als eine ausgefallene Festplatte aufweist, entfernen Sie nicht mehrere SSD oder Festplatte aus dem System jederzeit rechtzeitig. Dadurch kann zu Datenverlust führen.
>
>- Stellen Sie sicher, dass Sie Ersatz SSD einen Steckplatz versehen, die zuvor SSD enthalten. Ebenso legen Sie einen Ersatz-Festplatte in einen Steckplatz, der zuvor eine Festplatte enthalten
>
>- Steckplätze sind nummeriert in der Azure-Verwaltungsportal von 0 bis 11. Daher weist das Portal in Steckplatz 2 ein Datenträger, auf dem Gerät fehlgeschlagen suchen Sie nach der fehlerhaften Festplatte in den dritten Steckplatz oben links.

Laufwerke können entfernt und ersetzt, während das System ausgeführt wird.

#### <a name="to-remove-a-drive"></a>Ein Laufwerk entfernen

1. Identifizieren Sie die fehlerhafte Festplatte im klassischen Azure-Portal, **Geräte**gehen > **Wartung** > **Hardware-Status**. Da in der primären Einheit oder Anlage EBOD (bei Verwendung ein Modells 8600) eine Festplatte ausfallen kann, betrachten Sie Datenträger **Gemeinsam genutzten Komponenten** und **EBOD Gehäuse freigegebenen Komponenten**. Ein ausgefallenes Laufwerk in einer Einheit wird mit einem roten Status angezeigt.

2. Suchen Sie die Laufwerke vor der primären Einheit oder EBOD-Gehäuse. 

3. Wenn der Datenträger nicht gesperrt ist, fahren Sie mit dem nächsten Schritt fort. Wenn der Datenträger gesperrt ist, entsperren Sie Verfahren in [lösen sabotagekontakt Sperren](#disengage-the-antitamper-lock).

4. Drücken Sie den schwarzen Riegel auf Carrier Modul und ziehen Sie den Griff des Festplattenträgers, und von der Vorderseite des Gehäuses. 

    ![Ein Handle freigeben](./media/storsimple-disk-drive-replacement/IC741051.png)

    **Abbildung 3** Das Laufwerk Handle freigeben

5. Wenn der Griff des Festplattenträgers vollständig ausgezogen, schieben Sie den Laufwerkträger aus dem Gehäuse. 

    ![Schieben die Diskette aus Laufwerk](./media/storsimple-disk-drive-replacement/IC741052.png)
    
    **Abbildung 4** Schieben das Laufwerk aus dem Träger

## <a name="install-the-replacement-disk-drive"></a>Installieren Sie die Ersatz-Festplattenlaufwerk

Nachdem ein Laufwerk Geräts StorSimple ausgefallen und Sie entfernt haben, gehen Sie durch eine neue Festplatte ersetzen.

#### <a name="to-insert-a-drive"></a>Einfügen ein Laufwerks

1. Sicherstellen Sie, dass der Griff des Festplattenträgers vollständig ausgezogen ist, wie in der folgenden Abbildung dargestellt.

    ![Laufwerk mit erweiterten](./media/storsimple-disk-drive-replacement/IC741044.png)

    **Abbildung 5** Mit erweiterten Laufwerk

2. Schieben Sie der Laufwerkträger in das Gehäuse. 

    ![Datenträger in Laufwerk Carrier schieben](./media/storsimple-disk-drive-replacement/IC741045.png)

    **Abbildung 6**  Der Laufwerkträger schieben in das Gehäuse

3. Schließen Sie mit dem Laufwerk eingelegt den Griff des Festplattenträgers gleichzeitig drücken Laufwerkträgers in das Gehäuse, bis der Griff des Festplattenträgers verriegelte Position einrastet.

4. Verwenden Sie die Taste (nicht manipulierbaren Torx Schraubendreher) dem Tragegriff sichern bereitgestellten wurde zu der Schraube eine Drehung im Uhrzeigersinn drehen.

5. Überprüfen, ob der Austausch erfolgreich war und das Laufwerk betriebsbereit klassische Azure-Portal zugreifen und navigieren zur **Wartung** > **Hardware-Status**. **Freigegebene Komponenten** oder **EBOD Gehäuse freigegebenen Komponenten**sollte der Laufwerkstatus Grün, dass sie fehlerfrei ist.

    >[AZURE.NOTE] Der Datenträgerstatus nach der Ersetzung Grün mehrere Stunden dauern.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).
