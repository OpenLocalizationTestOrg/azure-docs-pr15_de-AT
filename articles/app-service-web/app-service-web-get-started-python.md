<properties 
    pageTitle="Bereitstellen Ihrer ersten Python Anwendung in Azure Minuten | Microsoft Azure" 
    description="Erfahren Sie, wie einfach die webapps durch Bereitstellung einer Beispiel-app in App Service ausgeführt wird. Starten Sie Entwicklung schnell und die sehen Sie Ergebnisse sofort." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-python-web-app-to-azure-in-five-minutes"></a>Bereitstellen Sie Ihrer ersten Python Anwendung in Azure in fünf Minuten

In diesem Lernprogramm können Sie Ihrer ersten Python Anwendung in [Azure App Service](../app-service/app-service-value-prop-what-is.md)bereitstellen.
App Service können Sie Web-apps [mobile-app back-Ends](/documentation/learning-paths/appservice-mobileapps/)und [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md)erstellen.

Sie können: 

- Erstellen Sie eine Webanwendung in Azure App Service.
- Bereitstellen Sie Python-Beispielcode.
- Siehe Code live in der Produktion ausgeführt.
- Aktualisieren Sie Ihrer Anwendung wie Sie [Push Git begeht](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Git](http://www.git-scm.com/downloads).
- [Azure CLI](../xplat-cli-install.md).
- Ein Microsoft Azure-Konto. Haben Sie ein Konto, können Sie [sich für eine kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F) oder [die Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Sie können [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751) ohne ein Azure-Konto. Erstellen einer app Starter und spielen für bis zu einer Stunde – keine Kreditkarte, keine Zusagen.

## <a name="deploy-a-python-web-app"></a>Bereitstellen einer Python Web app

1. Öffnen Sie eine neue Befehlszeile von Windows PowerShell-Fenster, Linux Shell oder OS X Terminal. Ausführen `git --version` und `azure --version` , ob Git und Azure CLI auf Ihrem Computer installiert sind.

    ![Testen Sie Installation des CLI-Tools für Ihrer ersten Anwendung in Azure](./media/app-service-web-get-started/1-test-tools.png)

    Wenn Sie die Tools installiert haben, finden Sie unter [Komponenten](#Prerequisites) herunterzuladen.

3. Melden Sie sich bei Azure wie folgt an:

        azure login

    Führen Sie den Hilfetext den Anmeldevorgang fort.

    ![Melden Sie sich bei Azure Erstellen Ihrer ersten Anwendung](./media/app-service-web-get-started/3-azure-login.png)

4. Azure-CLI ASM-Modus ändern, und legen Sie die Bereitstellung für App-Dienst fest. Sie werden mit den Anmeldeinformationen später Code bereitstellen.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Ändern in ein Arbeitsverzeichnis (`CD`) und Klonen Sie die Beispiel-app wie folgt:

        git clone https://github.com/Azure-Samples/app-service-web-python-get-started.git

2. Ändern Sie im Repository die Beispiel-app. Zum Beispiel:

        cd app-service-web-python-get-started

4. Erstellen Sie die App-Dienstressource app in Azure mit einer eindeutigen app und die Bereitstellung zuvor konfigurierten Wenn Sie aufgefordert werden, geben Sie die Anzahl der gewünschten Region.

        azure site create <app_name> --git --gitusername <username>

    ![Erstellen der Azure-Ressource Ihrer ersten Anwendung in Azure](./media/app-service-web-get-started-languages/python-site-create.png)

    Ihre Anwendung wird jetzt in Azure erstellt. Außerdem ist das aktuelle Verzeichnis Git initialisiert und mit der neuen App Service-app als eine remote Git.
    Finden Sie die app-URL (http://&lt;Anwendungsname >. *.azurewebsites.NET) schöne HTML-Standardseite, sondern wir tatsächlich jetzt Code vorhanden.

4. Bereitstellen Sie Beispielcode für Ihre Azure-Anwendung, wie Sie Code mit Git würde. Wenn Sie aufgefordert werden, verwenden Sie das Kennwort zuvor konfigurierten.

        git push azure master

    ![Push-Code auf Ihrer ersten Anwendung in Azure](./media/app-service-web-get-started-languages/python-git-push.png)

    `git push`nicht nur Code in Azure, sondern auch Bereitstellungsaufgaben in das Bereitstellungsmodul auslöst. 
    Haben Sie requirements.txt (Python) Dateien im Projektstammverzeichnis (Repository) stellt das Bereitstellungsskript die erforderlichen Pakete für Sie. 

Herzlichen Glückwunsch, Sie Ihre app Azure App Service bereitgestellt haben.

## <a name="see-your-app-running-live"></a>Finden Sie Ihre Anwendung mit live

Um Ihr Leben in Azure ausgeführte app anzuzeigen, führen Sie diesen Befehl aus dem Verzeichnis in Ihrem Repository:

    azure site browse

## <a name="make-updates-to-your-app"></a>Updates für Ihre Anwendung erstellen

Git können nun um jederzeit Ihr Projekt (Repository) Stamm der Website ein Update zu drücken. Sie tun es genauso wie den Code zum ersten Mal bereitgestellt. Beispielsweise jedes Mal, wenn Sie eine neue Änderung getestet haben lokal übertragen möchten, führen Sie einfach die folgenden Befehle aus der Projektstamm (Repository):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Nächste Schritte

[Erstellen, konfigurieren und Bereitstellen einer Django Web app in Azure in Visual Studio](web-sites-python-ptvs-django-mysql.md). Anhand dieses Lernprogramms erfahren Sie Grundkenntnisse in Azure ausgeführt werden Python Web app müssen einschließlich:

- Erstellen und Bereitstellen einer Python-Anwendung mithilfe einer Vorlage.
- Festlegen Sie Python-Version.
- Erstellen Sie virtuelle Umgebungen.
- Verbinden Sie mit einer Datenbank.

Oder möchten Sie Ihrer ersten Anwendung. Zum Beispiel:

- Probieren Sie [dazu Code in Azure bereitstellen](../app-service-web/web-sites-deploy.md). Z. B. zum Bereitstellen eines GitHub Repositories wählen Sie **GitHub** anstelle von **Lokalen Git Repository** in den **Bereitstellungsoptionen**.
- Nehmen Sie Ihre Azure-Anwendung auf die nächste Ebene. Benutzerauthentifizierung. Skalierung auf Anforderung. Einige Performance-Alarme einrichten. Mit wenigen Mausklicks. Finden Sie unter [Hinzufügen auf Ihrer ersten Anwendung](app-service-web-get-started-2.md).

