<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Sprinklr | Microsoft Azure" 
    description="Erfahren Sie, wie mit Sprinklr in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Lernprogramm: Azure Active Directory-Integration mit Sprinklr
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Sprinklr anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Sprinklr Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Sprinklr Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sprinklr zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Sprinklr
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Szenario")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Die Application Integration für Sprinklr
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Sprinklr aktivieren.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Sprinklr:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Sprinklr**.

    ![Anwendung Galerie] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Sprinklr aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Sprinklr mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Sprinklr** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Sprinklr** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Durch Zeichen In URL** den URL dem folgenden Muster "*https://\<Name Mieters\>. sprinklr.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Sprinklr** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Sprinklr.

6.  Gehen Sie zu **Administration \> Settings**.

    ![Verwaltung] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Verwaltung")

7.  Zu **Partner verwalten \> einmaliges** auf der linken.

    ![Verwalten von Partner] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Verwalten von Partner")

8.  Klicken Sie auf **+ einmaliges Add-Ons**.

    ![Einzelne Zeichen-Ons] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Einzelne Zeichen-Ons")

9.  Führen Sie die folgenden Schritte auf der Seite **Einmaliges Anmelden** :

    ![Einzelne Zeichen-Ons] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Einzelne Zeichen-Ons")

    1.  Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration (z. B.: *WAADSSOTest*).
    2.  Wählen Sie **aktiviert**.
    3.  **Neuen SSO-Zertifikat**auswählen
    4.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    5.  Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **Identity Provider Zertifikat** Textbox einfügen
    6.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Sprinklr** **Identität Anbieter-ID** -Wert und fügen Sie ihn in das Textfeld **Entitäts-Id** .
    7.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Sprinklr** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .
    8.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Sprinklr** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieter Abmelden URL** .
    9.  Als **SAML Benutzer-ID-Typ**wählen **Assertion enthält "s sprinklr.com Benutzername**.
    10. **SAML ID Speicherort**wählen Sie **Benutzer-ID im Element Kennung der Betreff-Anweisung ist**.
    11. Schließen Sie **Speichern**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
AAD Benutzer anmelden müssen sie für den Zugriff in der Sprinklr-Anwendung bereitgestellt werden.  
Dieser Abschnitt beschreibt das AAD Benutzerkonten in Sprinklr erstellen.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Um ein Benutzerkonto in Sprinklr bereitzustellen, führen Sie die folgenden Schritte:

1.  Sprinklr der Website Ihres Unternehmens als Administrator anmelden.

2.  Gehen Sie zu **Administration \> Settings**.

    ![Verwaltung] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Verwaltung")

3.  Gehen Sie zu **Clientcomputer verwalten \> Benutzer** im linken Bereich.

    ![Standardeinstellungen] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Standardeinstellungen")

4.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Standardeinstellungen] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Standardeinstellungen")

5.  Dialogfeld " **Benutzer bearbeiten** " die folgenden Schritte:

    ![Benutzer bearbeiten] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Benutzer bearbeiten")

    1.  Geben Sie in der **E-Mail**, **Vorname** und **Nachname** Textfelder soll Informationen eines Benutzerkontos Azure AD bereitstellen.
    2.  Wählen Sie **Kennwort deaktiviert**.
    3.  Wählen Sie eine **Sprache**aus.
    4.  Wählen Sie einen **Typ**.
    5.  Klicken Sie auf **Aktualisieren**.

    >[AZURE.IMPORTANT] Ein Benutzer über ein Identitätsprovider anmelden wird **Deaktiviert Kennwort** muss ausgewählt.

6.  Zur **Rolle**und führen Sie die folgenden Schritte aus:

    ![Partnerrollen] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partnerrollen")

    1.  Wählen Sie in der Liste **Global** **alle\_Berechtigungen**.
    2.  Klicken Sie auf **Aktualisieren**.

>[AZURE.NOTE] Können Sie alle anderen Sprinklr Benutzer Konto Erstellungstools oder APIs von Sprinklr Bereitstellung Azure AD-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Sprinklr:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Sprinklr **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)