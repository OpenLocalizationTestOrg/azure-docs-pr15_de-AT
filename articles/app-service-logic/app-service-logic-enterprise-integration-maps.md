<properties 
    pageTitle="Überblick über Karten Enterprise Integrationspaket | Microsoft Azure App Service | Microsoft Azure" 
    description="Erfahren Sie, wie Unternehmen Integrationspaket und Logik apps Karten mit" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-maps-and-the-enterprise-integration-pack"></a>Erfahren Sie mehr über Karten und Enterprise-Integrationspaket

## <a name="overview"></a>Übersicht
Unternehmensintegration verwendet Karten, um XML-Daten von einem Format in ein anderes Format umwandeln. 

## <a name="what-is-a-map"></a>Was ist ein Schema?
Eine Zuordnung ist ein XML-Dokument, das definiert, welche Daten in einem Dokument in ein anderes Format umgewandelt werden. 

## <a name="why-use-maps"></a>Warum verwendet wird?
Angenommen, Sie erhalten regelmäßig B2B Bestell- und Rechnungsstatus von einem Kunden YYYMMDD Format für Datumsangaben verwendet. In Ihrer Organisation speichern Sie jedoch Datumsangaben im Format MMDDYYY. Eine Zuordnung zum *Transformieren* Datumsformat YYYMMDD können in der MMDDYYY Sie vor dem Speichern der Bestellung oder Rechnung Details in Ihrer Kundendatenbank Aktivität.

## <a name="how-do-i-create-a-map"></a>Wie kann ich eine Karte erstellen?
[Enterprise-Integrationspaket](./app-service-logic-enterprise-integration-overview.md "erfahren Sie mehr über Enterprise-Integrationspaket") für Visual Studio 2015 können Biztalk-Integration Projekte erstellt werden.  Erstellen einer Integration Map-Datei können Sie Elemente zwischen zwei XML-Schemadateien visuell zuordnen.  Nach dem Erstellen dieses Projekts wird ein XSLT-Dokument ausgegeben.

## <a name="how-to-upload-a-map"></a>Wie eine Zuordnung hochladen?
Von Azure-Portal:  
1. Wählen Sie **Durchsuchen**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie im Suchfeld Filter **Integration** und wählen **Integration Konten** aus der Liste     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Konto Integration** in die Sie die Karte hinzufügen  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Wählen Sie das Musterelement **Karten**  
![](./media/app-service-logic-enterprise-integration-maps/map-1.png)  
5. Wählen Sie die Schaltfläche **Hinzufügen** Karten-Blatt, das geöffnet wird  
![](./media/app-service-logic-enterprise-integration-maps/map-2.png)  
6. Geben Sie einen **Namen** für die Karte, zum Hochladen der Datei und wählen Sie dann das Ordnersymbol rechts im Textfeld **zuordnen** . Nachdem der Upload abgeschlossen ist, klicken Sie auf **OK** .  
![](./media/app-service-logic-enterprise-integration-maps/map-3.png)  
7. Die Karte wird jetzt berücksichtigt Integration hinzugefügt. Sie erhalten eine Benachrichtigung, die den Erfolg oder Misserfolg die Zuordnungsdatei hinzufügen angibt. Nachdem Sie die Benachrichtigung erhalten, wählen Sie das Musterelement **Karten** : wird neu hinzugefügte Karte Blatt wird angezeigt    
![](./media/app-service-logic-enterprise-integration-maps/map-4.png)  

## <a name="how-to-edit-a-map"></a>Wie ein bearbeiten?
Um eine Karte zu bearbeiten, müssen Sie eine neue Zuordnungsdatei mit der gewünschten hochladen. Zunächst können Sie die Karte herunterladen und bearbeiten. 

Gehen Sie folgendermaßen eine neue Zuordnung hochgeladen, die eine vorhandene Zuordnung ersetzt:  
1. Wählen Sie das Musterelement **Karten**  
2. Wählen Sie die Karte zu bearbeiten, wenn das Blade wird geöffnet  
3. Wählen Sie auf dem Blatt **zugeordnet** Verknüpfung **Aktualisieren**  
![](./media/app-service-logic-enterprise-integration-maps/edit-1.png)   
4. Wählen Sie die Zuordnungsdatei mit Datei-Auswahldialogfeld das öffnet hochladen möchten und wählen Sie dann **Öffnen** in der Dateiauswahl   
![](./media/app-service-logic-enterprise-integration-maps/edit-2.png)   
5. Erhalten Sie eine Benachrichtigung Popup nach die Zuordnung hochgeladen wird.    

## <a name="how-to-delete-a-map"></a>Wie Sie eine Zuordnung löschen?
1. Wählen Sie das Musterelement **Karten**  
2. Wählen Sie die Karte zu löschen, wenn das Blade wird geöffnet  
3. Wählen Sie die Verknüpfung **Löschen**    
![](./media/app-service-logic-enterprise-integration-maps/delete.png)   
4. Bestätigen Sie wirklich die Zuordnung löschen möchten.  
![](./media/app-service-logic-enterprise-integration-maps/delete-confirmation-1.png)   

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket")  
- [Weitere Informationen zu Verträgen] (./app-service-logic-enterprise-integration-agreements.md "Erfahren Sie mehr über Enterprise Integration Abkommen")  
- [Weitere Informationen zu Transformationen] (./app-service-logic-enterprise-integration-transform.md "Erfahren Sie mehr über Unternehmensintegration transformiert")  
