<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Google Apps | Microsoft Azure"
    description="Erfahren Sie, wie mit Google Apps Azure Active Directory-auf automatisierte Bereitstellung und mehr!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-google-apps-with-azure-active-directory"></a>Exemplarische Vorgehensweise: Google Apps Azure Active Directory integrieren

In diesem Lernprogramm wird Ihre Umgebung Google Apps Azure Active Directory (Azure AD) Verbindung angezeigt. So aktivieren Sie automatische Benutzer bereitstellen einmaliges Google Apps konfigurieren und wie Benutzer auf Google Apps erfahren. 

##<a name="prerequisites"></a>Erforderliche Komponenten

1. Zugriff auf Azure Active Directory über das [klassische Azure-Portal](https://manage.windowsazure.com)müssen Sie ein gültiges Azure-Abonnement verfügen.

2. Sie müssen gültigen Mieter für [Google Apps for Work](https://www.google.com/work/apps/) oder [Google Apps for Education](https://www.google.com/edu/products/productivity-tools/). Sie können ein kostenloses Testabo für Dienst.

##<a name="video-tutorial"></a>Video-Lernprogramm

Einmaliges Google Apps in 2 Minuten Aktivieren von:

> [AZURE.VIDEO enable-single-sign-on-to-google-apps-in-2-minutes-with-azure-ad]

##<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. **F: sind Chromebooks und andere Geräte Chrom Azure AD einmaliges Anmelden?**

    A: Ja, werden Benutzer ihre Chromebook Geräte mit ihren Azure AD-Anmeldeinformationen anmelden. Finden Sie dieses [Google Apps unterstützen Artikel](https://support.google.com/chrome/a/answer/6060880) Informationen auf warum Benutzer zweimal aufgefordert Anmeldeinformationen abrufen können.

2. **F: Wenn ich einmaliges Anmelden aktivieren, werden Benutzer mithilfe ihrer Azure AD-Anmeldeinformationen anmelden von Google-Produkten wie Google Classroom Google Mail, Google Drive, YouTube?**

    A: Ja, je nachdem, [welche Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) können Sie aktivieren oder deaktivieren Sie für Ihre Organisation.

3. **F: aktivieren kann ich einmaliges Anmelden für nur eine Teilmenge der Benutzer Google Apps?**

    Nein, einmaliges einschalten sofort alle ihre Azure AD-Anmeldeinformationen authentifiziert Benutzer Google Apps benötigen. Da Google Apps nicht mehrere Identitätsanbieter unterstützen, der Identitätsanbieter für Ihre Umgebung Google Apps kann Azure AD oder Google- jedoch nicht beides gleichzeitig.

4. **F: Wenn über Windows ein Benutzer angemeldet ist, werden sie automatisch Google Apps authentifizieren, ohne aufgefordert, ein Kennwort?**

    A: Es gibt zwei Optionen zum Aktivieren dieses Szenarios. Erstens konnte Windows 10 Geräte über [Azure Active Directory beitreten](active-directory-azureadjoin-overview.md)Benutzer anmelden. Alternativ können Benutzer Windows-Geräten anmelden, die einer lokalen Active Directory Domäne für einmaliges Anmelden in Azure AD über eine [Active Directory-Verbunddienste (AD FS)](active-directory-aadconnect-user-signin.md) Bereitstellung aktiviert wurde. Folgen Sie der Anleitung unten einmaliges Anmelden zwischen Azure aktivieren, beide Optionen benötigen und Google Apps.

##<a name="step-1-add-google-apps-to-your-directory"></a>Schritt 1: Hinzufügen von Google Apps zum Verzeichnis

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Wählen Sie im linken Navigationsbereich des Active Directory.][0]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis Google Apps hinzufügen möchten.

3. Klicken Sie im oberen Menü auf **Applications** .

    ![Klicken Sie auf Anwendung.][1]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Klicken Sie auf Hinzufügen, um eine neue Anwendung hinzufügen.][2]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Klicken Sie auf eine Anwendung aus dem Katalog hinzufügen.][3]

6. Geben Sie im **Suchfeld** **Google Apps**. Wählen Sie **Google Apps** aus den Ergebnissen aus, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Google Apps hinzufügen.][4]

7. Sie sollten nun Seite Schnellstart Google Apps finden Sie unter:

    ![Google Apps Quick Start Page in Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Schritt 2: Aktivieren Sie einmaliges Anmelden

1. Azure AD auf Schnellstart für Google Apps klicken auf die Schaltfläche **Konfigurieren einmaliges Anmelden** .

    ![Die einzelnen anmelden Schaltfläche Konfigurieren][6]

2. Ein Dialogfeld wird geöffnet, und Sie sehen einen Bildschirm mit der Frage "Wie Benutzer Google Apps anmelden möchten?" **Azure AD einmaliges**wählen Sie aus und dann auf **Weiter**.

    ![Wählen Sie Azure AD einmaliges Anmelden][7]

    > [AZURE.NOTE] Weitere Informationen zu den verschiedenen einzelnen Zeichen auf Optionen, [Klicken Sie hier](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work) zu

3. Auf der Seite **Konfigurieren App Settings** Feld **Anmelde-URL** Geben Sie Google Apps Mieter URL im folgenden Format:`https://mail.google.com/a/<yourdomain>`

    ![Geben Sie die URL Mieter][8]

4. Geben Sie auf der Seite **einmaliges Anmelden für automatische Konfiguration** in der Domäne für Ihren Mandanten Google Apps. Klicken Sie dann auf **Konfigurieren** .

    ![Geben Sie den Domänennamen ein, und drücken Sie konfigurieren.](./media/active-directory-saas-google-apps-tutorial/ga-auto-config.png)

    > [AZURE.NOTE] Wenn Sie-auf manuell konfigurieren möchten, finden Sie unter [Optionaler Schritt: manuell konfigurieren Sie einmaliges Anmelden](#optional-step-manually-configure-single-sign-on)

5. Melden Sie sich bei Ihrem Google Apps-Administratorkonto. Klicken Sie auf **Zulassen** , um Azure Active Directory Konfiguration Ihres Abonnements Google Apps zu ermöglichen.

    ![Geben Sie den Domänennamen ein, und drücken Sie konfigurieren.](./media/active-directory-saas-google-apps-tutorial/ga-consent.PNG)

6. Warten Sie, während Active Directory Azure Ihrem Mandanten Google Apps konfiguriert. Sobald dies abgeschlossen ist, klicken Sie auf **Weiter**.

10. Geben Sie auf der letzten Seite des Dialogfelds eine e-Mail-Adresse möchten Sie e-Mail-Benachrichtigungen für Fehler und Warnungen im Zusammenhang mit der Wartung dieser Konfiguration für einzelne Zeichen.

    ![Geben Sie Ihre e-Mail-Adresse.][14]

11. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen. Testen Ihrer Konfiguration finden Sie im Abschnitt unten [Google Apps Benutzer zuweisen](#step-4-assign-users-to-google-apps).

##<a name="optional-step-manually-configure-single-sign-on"></a>Optionaler Schritt: Single Sign-On manuell konfigurieren

Wenn Sie-auf manuell einrichten möchten, führen Sie folgenden Schritte aus:

1. Azure AD auf Schnellstart für Google Apps klicken auf die Schaltfläche **Konfigurieren einmaliges Anmelden** .

    ![Die einzelnen anmelden Schaltfläche Konfigurieren][6]

2. Ein Dialogfeld wird geöffnet, und Sie sehen einen Bildschirm mit der Frage "Wie Benutzer Google Apps anmelden möchten?" **Azure AD einmaliges**wählen Sie aus und dann auf **Weiter**.

    ![Wählen Sie Azure AD einmaliges Anmelden][7]

    > [AZURE.NOTE] Weitere Informationen zu den verschiedenen einzelnen Zeichen auf Optionen, [Klicken Sie hier](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work) zu

3. Auf der Seite **Konfigurieren App Settings** Feld **Anmelde-URL** Geben Sie Google Apps Mieter URL im folgenden Format:`https://mail.google.com/a/<yourdomain>`

    ![Geben Sie die URL Mieter][8]

4. Wählen Sie auf der Seite **automatisch konfigurieren Sie einmaliges Anmelden** das Kontrollkästchen **manuell konfigurieren diese Anwendung für einmaliges Anmelden**. Klicken Sie auf **Weiter**.

    ![Wählen Sie manuelle Konfiguration.](./media/active-directory-saas-google-apps-tutorial/ga-auto-skip.PNG)

4. Auf der Seite **Konfigurieren einmaliges Google Apps** auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Downloaden Sie das Zertifikat.][9]

5. Öffnen einer neuen Registerkarte im Browser und melden Sie sich als Administrator an [Google Apps-Verwaltungskonsole](http://admin.google.com/) .

6. Klicken Sie auf **Sicherheit**. Wenn der Link nicht angezeigt wird, kann im Menü **Weitere Steuerelemente** am unteren Rand des Bildschirms ausgeblendet.

    ![Klicken Sie auf Sicherheit.][10]

7. Klicken Sie auf der Registerkarte **Sicherheit** auf **Richten Sie einmaliges Anmelden (SSO).**

    ![Klicken Sie auf SSO.][11]

8. Führen Sie die folgende Konfiguration ändern:

    ![Konfigurieren von SSO][12]

    - **Setup-SSO mit Drittanbieter Identität**auswählen.

    - Kopieren Sie in Azure AD die **URL des Diensts für einmaliges Anmelden**und Feld **Zeichen in URL der** Google Apps einfügen.

    - Azure AD kopieren Sie **einzelner Abmeldung Dienst-URL**, und fügen Sie ihn in das Feld **URL der Abmeldeseite** Google Apps.

    - In Azure AD **Ändern Kennwort URL**kopieren und in das Feld **Kennwort URL ändern** in Google Apps einfügen.

    - Google Apps für **Verifizierungszertifikat**Laden des Zertifikats, die Sie in Schritt 4 # heruntergeladen.

    - Klicken Sie auf **Speichern**.

9. Kontrollkästchen Sie in Azure AD Konfiguration für einzelne Zeichen Bestätigung Zertifikat aktivieren, das Sie in Google Apps hochgeladen. Klicken Sie auf **Weiter**.

    ![Aktivieren Sie das Kontrollkästchen Bestätigung][13]

10. Geben Sie auf der letzten Seite des Dialogfelds eine e-Mail-Adresse möchten Sie e-Mail-Benachrichtigungen für Fehler und Warnungen im Zusammenhang mit der Wartung dieser Konfiguration für einzelne Zeichen. 

    ![Geben Sie Ihre e-Mail-Adresse.][14]

11. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen. Testen Ihrer Konfiguration finden Sie im Abschnitt unten [Google Apps Benutzer zuweisen](#step-4-assign-users-to-google-apps).

##<a name="step-3-enable-automated-user-provisioning"></a>Schritt 3: Automatisierte Bereitstellung von Benutzer

> [AZURE.NOTE] Eine andere Alternative für Benutzer bereitstellen, Google Apps automatisieren ist Ihre lokale Active Directory Identitäten Google Apps Vorschriften [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) verwenden. Im Gegensatz dazu stellt die Lösung in diesem Lernprogramm Ihre Azure Active Directory (Wolke) Benutzer und e-Mail-aktivierte Gruppen Google Apps.

1. Melden Sie die [Google Apps-Verwaltungskonsole](http://admin.google.com/) das Administratorkonto an, und klicken Sie auf **Sicherheit**. Wenn der Link nicht angezeigt wird, kann im Menü **Weitere Steuerelemente** am unteren Rand des Bildschirms ausgeblendet.

    ![Klicken Sie auf Sicherheit.][10]

2. Klicken Sie auf der Seite **Sicherheit** **-API-Referenz**.

    ![Klicken Sie auf-API-Referenz.][15]

3. Wählen Sie **Aktivieren API-Zugriff**.

    ![Klicken Sie auf-API-Referenz.][16]

    > [AZURE.IMPORTANT] Google Apps, deren Benutzernamen in Azure Active Directory *muss* bereitstellen wollen werden für jeden Benutzer eine benutzerdefinierte Domäne gebunden. Beispielsweise wie Benutzernamen bob@contoso.onmicrosoft.com nicht akzeptiert werden von Google Apps während bob@contoso.com akzeptiert werden. Sie können einen vorhandenen Benutzer Domäne ändern ihre Eigenschaften in Azure AD. Informationen zum Festlegen eine benutzerdefinierte Domäne für Azure Active Directory und Google Apps sind unten aufgeführt.

4. Wenn Sie bisher noch einen benutzerdefinierten Domänennamen zu Azure Active Directory hinzugefügt haben, befolgen Sie die folgenden Schritte aus:

    - Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**. Wählen Sie in der Verzeichnisliste das Verzeichnis ein. 

    - Klicken Sie auf **Domänen** der obersten Ebene aus und klicken Sie dann auf **benutzerdefinierte Domäne hinzufügen**.

        ![Hinzufügen einer benutzerdefinierten Domäne][17]

    - Geben Sie den Domänennamen in das Feld **Domäne ein** . Dies sollte den gleichen Domänennamen, den Sie für Google Apps verwenden möchten. Wenn Sie bereit sind, klicken Sie auf **Hinzufügen** .

        ![Geben Sie den Domänennamen ein.][18]

    - Klicken Sie auf **Weiter** zur Bestätigungsseite zu wechseln. Überprüfen Sie diese Domäne besitzen, müssen Sie die Domäne DNS-Einträge entsprechend der Werte auf dieser Seite bearbeiten. Sie können überprüfen, **MX-** oder **TXT-Datensätze**je nach der Option **Datensatztyp Auswahl** verwenden. Umfassendere Informationen zur Azure AD Domain-Namen überprüfen finden Sie unter [Hinzufügen Ihren eigenen Domänennamen Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).

        ![Überprüfen Sie den Domänennamen ein.][19]

    - Wiederholen Sie die obigen Schritte für alle Domänen, die Sie zum Verzeichnis hinzufügen möchten.

5. Damit Sie alle Domänen mit Azure überprüft haben, müssen Sie nun diese erneut mit Google Apps überprüfen. Führen Sie für jede Domäne mit Google Apps bereits registriert ist die folgenden Schritte:

    - [Google Apps-Verwaltungskonsole](http://admin.google.com/)klicken Sie auf **Domänen**.

        ![Klicken Sie auf Domänen][20]

    - Klicken Sie auf **eine Domäne oder einen Domänenalias**.

        ![Hinzufügen einer neuen Domäne][21]

    - Wählen Sie **eine andere Domäne hinzufügen**, und geben Sie den Namen der Domäne, die Sie hinzufügen möchten.

        ![Geben Sie den Domänennamen][22]

    - Klicken Sie auf **Weiter und Domäne Inhaberschaft**. Die Schritte zum Überprüfen, ob Sie den Domainnamen. Umfassende Informationen zum Überprüfen Ihrer Domäne mit Google Apps finden Sie unter [Überprüfen der Website Besitz Google Apps](https://support.google.com/webmasters/answer/35179).

    - Wiederholen Sie die obigen Schritte für alle weiteren Domänen Google Apps hinzufügen möchten.

    > [AZURE.WARNING] Wenn die primäre Domäne für Ihren Mandanten Google Apps ändern und wenn Sie bereits einmaliges Anmelden mit Azure AD konfiguriert, müssen Sie Schritt #3 wiederholen [Schritt: Aktivieren Sie einmaliges Anmelden](#step-two-enable-single-sign-on).

6. Klicken Sie in der [Verwaltungskonsole für Google Apps](http://admin.google.com/)auf **Administratorrollen**.

    ![Klicken Sie auf Google Apps][26]

7. Bestimmen Sie die Admin-Konto verwalten Benutzer bereitstellen möchten. Bearbeiten Sie für die **Admin-Rolle** des Kontos die **Berechtigungen** für diese Rolle Stellen Sie sicher, dass alle **API-Administratorrechte** aktiviert, so dass dieses Konto für die Bereitstellung verwendet werden kann.

    ![Klicken Sie auf Google Apps][27]

    > [AZURE.NOTE] Konfigurieren einer ist am besten für diesen Schritt ein neues Administratorkonto in Google Apps erstellen. Diese Konto muss Administratorrolle zugeordnet, die die erforderlichen API-Berechtigungen verfügt.

8. Klicken Sie in Azure Active Directory im Menü obersten Ebene auf **Programme** und klicken Sie dann auf **Google Apps**.

    ![Klicken Sie auf Google Apps][23]

9. Klicken Sie auf der Seite Schnellstart für Google Apps auf **Konfigurieren Benutzer bereitstellen**.

    ![Konfigurieren Sie Benutzer bereitstellen][24]

10. Das Dialogfeld, das geöffnet wird, klicken Sie auf in Google Apps-Administratorkonto zu authentifizieren, die Bereitstellung verwalten möchten **Benutzer bereitstellen können** .

    ![Bereitstellung aktivieren][25]

11. Bestätigen Sie, dass Sie Azure Active Directory Erlaubnis, Ihrem Mandanten Google Apps ändern möchten.

    ![Berechtigungen zu bestätigen.][28]

12. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen.

##<a name="step-4-assign-users-to-google-apps"></a>Schritt 4: Zuweisen von Benutzern zu Google Apps

1. Starten Sie Ihre Konfiguration einen Test-Account im Verzeichnis erstellen.

2. Klicken Sie auf der Seite Google Apps Quick Start auf **Benutzer zuweisen** .

    ![Klicken Sie auf Benutzer zuweisen][29]

3. Wählen Sie Ihre Testbenutzer und klicken Sie **weisen** am unteren Bildschirmrand:

 - Wenn Sie nicht aktivieren Sie benutzerbereitstellung automatisierter sehen die folgende Meldung zu bestätigen:

        ![Confirm the assignment.][30]

 - Wenn Automatisches provisioning Benutzer aktiviert haben, dann sehen Sie aufgefordert, die definieren, welche Rolle der Benutzer in Google Apps muss. Neu eingerichtete Benutzer sollten in Ihrer Umgebung Google Apps nach einigen Minuten angezeigt.

4. Testen Sie Ihre Einstellungen für einzelnen Zeichen öffnen Sie die Abdeckung auf [https://myapps.microsoft.com](https://myapps.microsoft.com/)und das Testkonto melden Sie an und auf **Google Apps**.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-google-apps-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-google-apps-tutorial/add-app.png
[3]: ./media/active-directory-saas-google-apps-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-google-apps-tutorial/add-gapps.png
[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png
