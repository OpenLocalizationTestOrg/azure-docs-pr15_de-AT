<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration mit Mozy | Microsoft Azure" 
    description="Erfahren Sie, wie mit Mozy Enterprise Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Lernprogramm: Azure Active Directory Integration mit Mozy
  
Ziel dieses Lernprogramms ist die Integration von Azure und Mozy Unternehmen anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Mozy Enterprise Mieter
  
Nach diesem Lernprogramm werden können einzelne Zeichen in der Anwendung die Mozy Enterprise Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, die Sie Mozy Enterprise zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration Mozy Unternehmen
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Szenario")
##<a name="enabling-the-application-integration-for-mozy-enterprise"></a>Die Application Integration Mozy Unternehmen
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Mozy Unternehmen aktivieren.

###<a name="to-enable-the-application-integration-for-mozy-enterprise-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Mozy Enterprise:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Mozy Enterprise**.

    ![Anwendung Galerie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Mozy Enterprise aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Mozy Enterprise] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Mozy Enterprise")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Mozy Enterprise mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat Ihrem Mandanten Mozy Enterprise hochladen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Mozy Enterprise** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Mozy Enterprise** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Mozy Enterprise Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. MozyEnterprise.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "Konfigurieren von app-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Mozy Unternehmen** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Mozy Enterprise.

6.  Klicken Sie unter **Konfiguration** auf **Authentifizierungsrichtlinie**.

    ![Authentifizierungsrichtlinie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Authentifizierungsrichtlinie")

7.  Im Abschnitt **Authentifizierungsrichtlinie** Schritte:

    ![Authentifizierungsrichtlinie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Authentifizierungsrichtlinie")

    1.  Wählen Sie **Verzeichnisdienst** als **Anbieter**.
    2.  Wählen Sie **LDAP-Push**.
    3.  Klicken Sie auf die Registerkarte **SAML-Authentifizierung** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Mozy Unternehmen** **Authentifizierung anfordern URL** -Wert und fügen Sie ihn in das Textfeld **URL Authentifizierung** .
    5.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Mozy Enterprise** **Identity-Provider-ID** -Wert, und fügen Sie ihn in das Textfeld **SAML-Endpunkt** .
    6.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    7.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie das gesamte Zertifikat in **SAML Zertifikat** Textbox.
    8.  Wählen Sie **Aktivieren SSO-Administratoren mit ihrer Netzwerkanmeldeinformationen anmelden**.
    9.  Klicken Sie auf **Speichern**.

8.  Wählen Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Mozy Unternehmen** Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **abgeschlossen**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD Benutzer Mozy Enterprise anmelden können, müssen sie Mozy Unternehmen bereitgestellt werden.  
Bei Mozy Enterprise ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Mozy Enterprise** .

2.  Klicken Sie auf **Benutzer**, und klicken Sie dann auf **Neuen Benutzer hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Benutzer")

    >[AZURE.NOTE]Die Option **Neue Benutzer hinzufügen** wird nur angezeigt, nur wenn **Mozy** als Anbieter unter **Authentifizierungsrichtlinie**ausgewählt ist. SAML-Authentifizierung konfiguriert sind dann Benutzer bei ihrer ersten Anmeldung durch einmaliges auf automatisch.

3.  Gehen Sie im Dialogfeld neuen Benutzer:

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Hinzufügen von Benutzern")

    1.  Wählen Sie eine Gruppe aus der Liste **Wählen Sie eine Gruppe** .
    2.  Wählen Sie in der Liste **welche Benutzer** ein.
    3.  Geben Sie im Textfeld **Benutzername** den Namen des Azure AD-Benutzerobjekt.
    4.  Geben Sie im Feld **E-Mail** die e-Mail-Adresse des Benutzers Azure AD.
    5.  Wählen Sie die **Anweisung e-Mail senden**.
    6.  Klicken Sie auf **Benutzer hinzufügen**.

    >[AZURE.NOTE]Nach dem Erstellen des Benutzers wird eine e-Mail Azure AD-Benutzer gesendet, die enthält einen Link, um das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE]Können Sie alle anderen Mozy Enterprise Benutzer Konto Erstellungstools oder APIs von Mozy Enterprise Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
 
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-mozy-enterprise-perform-the-following-steps"></a>Mozy Enterprise Benutzer zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Mozy Enterprise **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)