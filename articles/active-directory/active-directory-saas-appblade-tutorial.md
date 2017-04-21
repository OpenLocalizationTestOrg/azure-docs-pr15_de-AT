<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration AppBlade | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und AppBlade."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>Lernprogramm: Azure Active Directory-Integration mit AppBlade

Das Ziel dieses Lernprogramms ist, wie AppBlade in Azure Active Directory (Azure AD) integriert werden.  
Azure AD AppBlade Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf AppBlade Zugriff
- Sie können die Benutzer automatisch angemeldet-AppBlade (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit AppBlade benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein AppBlade Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. AppBlade aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-appblade-from-the-gallery"></a>AppBlade aus der Galerie hinzufügen
Konfigurieren Sie die Integration der AppBlade in Azure AD müssen Sie der Liste der verwalteten SaaS-apps AppBlade aus der Galerie hinzufügen.

**Um AppBlade aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
 
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.
 
    ![Applikationen][4]

6. Geben Sie im Suchfeld **AppBlade**.

![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **AppBlade aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_02.png)



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll konfigurieren und Testen Azure AD SSO-AppBlade basierend auf einen Testbenutzer namens "Britta Simon" angezeigt.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Gegenstück Benutzer AppBlade eines Benutzers in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in AppBlade hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in AppBlade.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit AppBlade müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer AppBlade testen Benutzer](#creating-a-appblade-test-user)** - Gegenstück Britta Simon AppBlade enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung AppBlade.



**Konfigurieren Azure AD einmaliges Anmelden mit AppBlade die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **AppBlade** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei AppBlade** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_03.png) >

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_04.png) 


    ein. Geben Sie im Textfeld Anmelde-URL die URL die Benutzer anmelden, der AppBlade-Anwendung mit dem folgenden Muster: **"https://companyname.appblade.com/saml/tenantid"**.

    b. Klicken Sie auf **Weiter**.


4. Auf der Seite **Konfigurieren einmaliges Anmelden am AppBlade** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_05.png) 

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Um SSO für die Anwendung konfigurierten erhalten, wenden Sie die AppBlade über **support@appblade.com** und die heruntergeladenen Metadatendatei per e-Mail. Auch bitten sie **SSO-Aussteller URL** als **https://appblade.com/saml**konfigurieren. Diese Einstellung ist für einmaliges Anmelden zu.


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-appblade-test-user"></a>Erstellen einen Testbenutzer AppBlade

Dieser Abschnitt soll Benutzer Britta Simon in AppBlade erstellen. AppBlade unterstützt Just-in-Time-Bereitstellung, die standardmäßig aktiviert. **Stellen Sie sicher, der Domänennamen für Benutzer bereitstellen mit AppBlade konfiguriert ist. Danach werden nur die Just-in-Time-benutzerbereitstellung arbeiten.**

Hat der Benutzer eine e-Mail-Adresse endet mit der Domäne dann der Benutzer automatisch das Konto als Mitglied der Berechtigungsstufe wird angegebenen von AppBlade für Ihr Konto konfiguriert, d.h. "Basic" (eine grundlegende Benutzer nur Programme installieren), "Teammitglied" (ein Benutzer, neue Versionen der app hochladen und Verwalten von Projekten) oder "Administrator" (vollständige Administratorrechte auf das Konto). Normalerweise würde eine Basic auswählen und fördern Benutzer über ein Admin-Login (AppBlade muss ein Benutzername Admin e-Mail-basierte vorab konfigurieren oder einen Benutzer nach Anmeldung im Auftrag des Kunden fördern).

Es ist keine Aufgabe für Sie in diesem Abschnitt. Ein neuer Benutzer wird ein Zugriffsversuch auf AppBlade noch nicht erstellt. [Konfiguration von Azure AD einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie den AppBlade-Support wenden.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges Anmelden ihren Zugriff auf AppBlade aktivieren.

![Benutzer zuweisen][200] 

**Britta Simon AppBlade zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **AppBlade**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die AppBlade Fläche in der Sie sollte automatisch zur Anwendung AppBlade angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_205.png
