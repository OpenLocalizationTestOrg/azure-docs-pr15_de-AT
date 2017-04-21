<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Bynder | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Bynder."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Lernprogramm: Azure Active Directory-Integration mit Bynder

Das Ziel dieses Lernprogramms ist, wie Bynder in Azure Active Directory (Azure AD) integriert werden.

Azure AD Bynder Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Bynder Zugriff
- Sie können die Benutzer automatisch angemeldet-Bynder (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Bynder benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Bynder Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Microsoft Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Bynder aus der Galerie hinzufügen
2. Konfigurieren und Testen von Microsoft Azure AD einmaliges Anmelden


## <a name="adding-bynder-from-the-gallery"></a>Bynder aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Bynder in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Bynder aus dem Katalog hinzufügen.

**Um Bynder aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Bynder**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **Bynder aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Microsoft Azure AD einmaliges Anmelden
Ziel dieses Abschnitts wird erläutert, wie Sie konfigurieren und Testen von Microsoft namens "Britta Simon" Testbenutzer Azure AD einmaliges Anmelden mit Bynder auf.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Bynder einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Bynder hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Bynder.

Konfigurieren und Testen Sie Microsoft Azure AD einmaliges Anmelden mit Bynder müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Microsoft Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Microsoft Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer Bynder testen Benutzer](#creating-a-bynder-test-user)** - Gegenstück Britta Simon Bynder enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Microsoft Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD einmaliges Anmelden konfigurieren

In diesem Abschnitt Microsoft Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der Bynder-Anwendung konfigurieren.

**Konfigurieren Sie einmaliges Anmelden für Microsoft Azure AD mit Bynder, führen Sie die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Bynder** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Bynder** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)

3. Möchten Sie auf der **App-Einstellungen konfigurieren** die Anwendung im **IDP initiiert Modus**konfigurieren, führen Sie die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)

    ein. Geben Sie im Feld **Antwort-URL** eine URL dem folgenden Muster: `https://<company name>.getbynder.com/sso/SAML/authenticate/`

    b. Klicken Sie auf **Weiter**

4. Möchten Sie die Anwendung auf der **App-Einstellungen konfigurieren** im **SP initiiert Modus** konfigurieren, klicken Sie auf **"Show advanced Settings (optional)"** und geben Sie die **Anmelde-URL** , und klicken Sie auf **Weiter**.



    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster: `https://<company name>.getbynder.com/login/`

    b. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Der Wert für die Anmelde-URL in diesem Lernprogramm ist nur eine Placeholfer. Um den tatsächlichen Wert für Ihre Umgebung zu erhalten, wenden Sie sich an Bynder.

5. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Bynder** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

6. Um SSO für die Anwendung konfigurierten zu erhalten, wenden Sie die Bynder. Anfügen der heruntergeladenen Metadatendatei und gemeinsam mit Bynder SSO auf Seite einrichten.

7. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-bynder-test-user"></a>Erstellen einen Testbenutzer Bynder

Dieser Abschnitt soll Benutzer Britta Simon in Bynder erstellen. Bynder unterstützt Just-in-Time-Bereitstellung, die standardmäßig aktiviert.

Es ist keine Aufgabe für Sie in diesem Abschnitt. Ein neuer Benutzer wird ein Zugriffsversuch auf Bynder noch nicht erstellt.

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie den Bynder-Support wenden.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges Anmelden ihren Zugriff auf Bynder aktivieren.
    
   ![Benutzer zuweisen][200]

**Britta Simon Bynder zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Bynder**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)

1. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Ihre Microsoft Azure AD-auf Konfiguration mit der testen.
 
Beim Anklicken der Kachel Bynder in der Sie sollte automatisch zur Anwendung Bynder angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
