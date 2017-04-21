<properties
   pageTitle="Bereitstellen einer Anwendung Node.js, virtuelle Linux-Computer in Azure"
   description="Weitere Informationen zum Bereitstellen einer Anwendung Node.js virtuelle Linux-Computer in Azure."
   services=""
   documentationCenter="nodejs"
   authors="stepro"
   manager="dmitryr"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/02/2016"
   ms.author="stephpr"/>

# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Bereitstellen einer Anwendung Node.js, virtuelle Linux-Computer in Azure

Dieses Lernprogramm veranschaulicht, wie eine Anwendung Node.js und Linux virtuellen Computern in Azure bereitstellen. Die Anleitung in diesem Lernprogramm können auf jedem Betriebssystem der ausgeführt Node.js folgen.

Erfahren Sie, wie:

- Verzweigung und Clone ein GitHub Repository mit einer einfachen TODO-Anwendung;
- Erstellen Sie und konfigurieren Sie zwei virtuelle Linux-Computer in Azure, die Anwendung auszuführen.
- Die Anwendung indem Updates auf Web-Front-End-VM durchlaufen.

> [AZURE.NOTE]
> Um dieses Lernprogramm benötigen Sie ein GitHub-Konto und ein Microsoft Azure-Konto und Git aus einer verwenden.

> Wenn Sie ein GitHub-Konto haben, können Sie sich [hier](https://github.com/join)anmelden.

> Wenn Sie ein [Microsoft Azure](https://azure.microsoft.com/) -Konto haben, können Sie eine kostenlose Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)anmelden. Dies auch führt Sie durch die Anmeldung für ein [Microsoft-Konto](http://account.microsoft.com) Wenn Sie noch nicht vorhanden sind. Auch wenn Sie ein Visual Studio-Abonnent sind, können Sie [Aktivieren Ihre MSDN-Vorteile](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> Haben Sie keine Git auf dem Entwicklungscomputer, dann bei einem Macintosh- oder Windows-Computer installieren Sie Git [hier](http://www.git-scm.com). Wenn Linux installieren Git mit der für Sie am besten geeigneten Mechanismus wie `sudo apt-get install git`.

## <a name="forking-and-cloning-the-todo-application"></a>Verzweigung und Cloning die TODO-Anwendung

TODO Anwendung dieses Lernprogramm implementiert ein einfaches Web-Frontend über MongoDB-Instanz, die überwacht eine Aufgabenliste. Gehen Sie nach der Anmeldung zu GitHub [hier](https://github.com/stepro/node-todo) finden die Anwendung und Verzweigung über den Link in der rechten oberen Ecke. Dies sollte ein Repository in Ihrem Konto mit dem Namen *Kontoname*/node-todo erstellen.

Jetzt auf Ihrem Entwicklungscomputer Klonen dieses Repository:

    git clone https://github.com/accountname/node-todo.git

Diese lokalen Klon des Repositorys verwenden etwas später wir bei den Quellcode ändern.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Erstellen und Konfigurieren der virtuelle Linux-Computer

Azure bietet hervorragende Unterstützung für raw Compute virtuelle Linux-Computer verwenden. Dieses Lernprogramm zeigt, wie Sie einfach zwei virtuelle Linux-Computer hochfahren und TODO in bereitzustellen, Web-Frontend mit MongoDB- Instanz auf der anderen, Teil.

### <a name="creating-virtual-machines"></a>Erstellen virtueller Computer

Die einfachste Möglichkeit zum Erstellen eines neuen virtuellen Computers in Azure ist der Azure-Portal verwenden. Klicken Sie auf [hier](https://portal.azure.com) anmelden und Azure-Portal in Ihrem Webbrowser. Nach dem Laden der Azure-Portal folgende Schritte:

- Klicken Sie auf den Link "+ neue".
- Kategorie "Compute" und wählen Sie "Ubuntu Server 14.04 LTS";
- Wählen Sie Resource Manager-Bereitstellungsmodell aus und auf "Erstellen"
- Füllen Sie die Grundlagen für die folgenden Richtlinien:
  - Geben Sie einen Namen ein, den Sie später problemlos erkennen können.
  - Wählen Sie für dieses Lernprogramm Kennwortauthentifizierung.
  - Erstellen Sie eine neue Ressourcengruppe mit einem eindeutigen Namen.
- Für die Größe des virtuellen Computers lautet "A1 Standard" eine vernünftige Wahl für dieses Lernprogramm.
- Weitere Einstellungen ist Typ "Standard" und die übrigen Standardeinstellungen akzeptieren.
- Starten Sie auf der Zusammenfassungsseite erstellen.

Durchführen des obigen Vorgangs zweimal um zwei virtuelle Linux-Computer für Web-Frontend und MongoDB-Instanz zu erstellen. Erstellen der virtuellen Computer dauert ca. 5 bis 10 Minuten.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Zuweisen eines DNS-Eintrags für virtuelle Maschinen

In Azure erstellte virtuelle Maschinen sind standardmäßig nur über eine öffentliche IP-Adresse wie 1.2.3.4. Lassen Sie den Computer leichter identifizieren durch Zuweisen von DNS-Einträgen.

Sobald das Portal gibt an, dass die virtuellen Computer erstellt haben, klicken Sie auf "Virtueller Computer" in der linken Navigationsleiste und suchen Sie Ihre Computer. Für jeden Computer:

- Essentials Seitenregister, und klicken Sie auf die öffentliche IP-Adresse;
- Die öffentliche IP-Adresskonfiguration einen DNS-Namen zuweisen und speichern.

Das Portal wird sichergestellt, dass die von Ihnen angegebene Name verfügbar ist. Nach dem Speichern der Konfigurations müssen die virtuellen Computer Hostnamen wie `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>Verbindung mit virtuellen Computern

Wenn der virtuelle Computer bereitgestellt wurden, wurden sie vorkonfigurierte Remoteverbindungen über SSH. Dies ist der Mechanismus, mit dem der virtuelle Computer konfigurieren. Wenn Sie Windows für die Entwicklung verwenden, müssen Sie SSH-Client erhalten, wenn Sie nicht bereits eine verfügen. Eine gemeinsame Auswahl ist kitten, die [hier](http://www.chiark.greenend.org.uk/~sgtatham/putty/)heruntergeladen werden kann. Macintosh und Linux OSes enthalten eine Version von SSH vorinstalliert.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurieren von Front-End-VM

SSH mit kitten, ssh Befehlszeile oder Ihrem bevorzugten SSH Tool erstellten Web Front-End-Computer. Sie sollten eine Willkommensnachricht gefolgt von einer Befehlszeile finden Sie unter.

Zunächst stellen wir sicher, dass Git und Knoten installiert sind:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs
    
Da einige systemeigenen Node.js-Modulen der Anwendung Web-Front-End benötigt, müssen wir auch die wesentlichen Buildtools installieren:

    sudo apt-get install -y build-essential

Wir installieren schließlich Node.js-Anwendung *immer*so ausgeführt Node.js Server Applications aufgerufen:

    sudo npm install -g forever
    
Dies sind alle Abhängigkeiten mussten auf diesem virtuellen Computer zum Ausführen der Anwendung Web-Frontend also, ausgeführt. Dazu erstellen wir zuerst bare Klon des GitHub-Repository, die Sie zuvor gespalten, sodass Updates (wir dabei Update später behandeln) virtuellen Computer veröffentlichen, und Klonen bare Klon um das Repository zur Verfügung, die tatsächlich ausgeführt werden können.

Ab dem Basisverzeichnis (~), führen Sie folgende Befehle ( *Kontoname* Ihres Kontonamens GitHub ersetzt):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Jetzt geben Sie Knoten Todo-Verzeichnis, und führen Sie diese Befehle aus:

    npm install
    forever start server.js
    
Die Anwendung Web-Front-End wird jetzt ausgeführt, aber ist ein weiterer Schritt, bevor Sie die Anwendung über einen Webbrowser zugreifen können. Der virtuelle Computer erstellten durch eine Azure Ressource bezeichnet eine *Netzwerk-Sicherheitsgruppe*, die für Sie erstellt wurde, wenn der virtuelle Computer bereitgestellt geschützt. Diese Ressource lässt derzeit nur externe Anfragen an Port 22 virtuellen Computer ermöglicht SSH-Kommunikation mit dem Computer jedoch nichts weitergeleitet werden. So braucht auf Port 8080 konfiguriert ist die Anwendung TODO anzeigen, dieser Port auch geöffnet werden.

Azure-Portal zurück und führen die folgenden Schritte aus:

- Klicken Sie auf "Ressourcengruppen" in der linken Navigationsleiste;
- Wählen Sie die Ressourcengruppe, die den virtuellen Computer enthält.
- Wählen Sie in der resultierenden Liste von Ressourcen Netzwerk-Sicherheitsgruppe (einer mit einem Schild);
- Wählen Sie in den Eigenschaften "eingehende Sicherheitsregeln";
- Klicken Sie in der Symbolleiste auf "Hinzufügen"
- Geben Sie einen Namen wie "Standard-zulassen-Todo";
- Legen Sie das Protokoll "TCP";
- Legen Sie den Ziel Portbereich "8080";
- Klicken Sie auf OK, und warten Sie die Sicherheitsregel erstellt werden.

Nach dem Erstellen dieser Regel die TODO Anwendung im Internet öffentlich sichtbar und können Sie suchen, beispielsweise mit einer URL wie:

    http://machinename.region.cloudapp.azure.com:8080

Sie werden feststellen, dass, obwohl wir MongoDB virtuellen Computer noch nicht konfiguriert haben, TODO Anwendung scheint ziemlich funktionsfähig. Ist das Quell-Repository hartcodiert einer bereits bereitgestellten MongoDB Instanz ist. Nachdem wir MongoDB virtuellen Computer konfiguriert haben, wir zurückgehen und den Quellcode um private MongoDB Instanz stattdessen nutzen.

### <a name="configuring-the-mongodb-virtual-machine"></a>MongoDB virtuellen Computer konfigurieren

SSH auf dem zweiten Computer mit kitten, ssh Befehlszeile oder Ihrem bevorzugten SSH Tool erstellten. Installieren Sie nach der Begrüßung und Befehlszeile, MongoDB (diese Schritte wurden [hier](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Standardmäßig ist MongoDB konfiguriert, damit es nur lokal zugegriffen werden kann. Für dieses Lernprogramm wird damit die Anwendung dem virtuellen Computer möglich MongoDB konfiguriert. In einem Kontext Sudo /etc/mongod.conf-Datei öffnen, und suchen Sie die `# network interfaces` Abschnitt. Ändern der `net.bindIp` Konfigurationswert auf `0.0.0.0`.

> [AZURE.NOTE]
> Diese Konfiguration ist für die Zwecke dieses Lernprogramms. Es ist **keine** empfohlene Vorgehensweise und sollte nicht in der Produktion verwendet werden.

Jetzt gewährleisten Sie die MongoDB-Dienst gestartet wurde:

    sudo service mongod restart

MongoDB verwendet den standardmäßig Port 27017. Dabei, die wir benötigten Port 8080 auf dem Web-Front-End virtuellen Computer öffnen, müssen wir also Anschluss 27017 MongoDB virtuellen Computers öffnen.

Azure-Portal zurück und führen die folgenden Schritte aus:

* Klicken Sie auf "Ressourcengruppen" in der linken Navigationsleiste;
* Wählen Sie die Ressourcengruppe, die MongoDB virtuellen Computer enthält.
* Wählen Sie in der resultierenden Liste der Ressourcen der Netzwerk-Sicherheitsgruppe (einer mit einem Schild) mit demselben Namen MongoDB virtuellen Computer gab;
* Wählen Sie in den Eigenschaften "eingehende Sicherheitsregeln";
* Klicken Sie in der Symbolleiste auf "Hinzufügen"
* Geben Sie einen Namen wie "Standard-zulassen-Mongo";
* Legen Sie das Protokoll "TCP";
* Legen Sie den Portbereich Ziel "27017"
* Klicken Sie auf OK, und warten Sie die Sicherheitsregel erstellt werden.

## <a name="iterating-on-the-todo-application"></a>TODO-Anwendung durchlaufen
Bisher haben wir zwei virtuelle Linux-Computer bereitgestellt: eines, das die Anwendung-Front-WebFrontEnd und einem mit MongoDB-Instanz ausgeführt wird. Ein Problem-Web-Frontend tatsächlich verwendet nicht bereitgestellte MongoDB Instanz noch. Nun aktualisieren Sie Code für das Front-End-Umgebungsvariable statt eine hartcodierte Instanz verwenden.

### <a name="changing-the-todo-application"></a>Ändern der TODO

Öffnen Sie auf dem Entwicklungscomputer, geklont von Knoten Todo-Repository, das `node-todo/config/database.js` in Ihrem bevorzugten Editor-Datei und ändern Sie den Url-Wert von hartcodierten Wert wie `mongodb://...` , `process.env.MONGODB`.

Änderungen Sie die, und drücken Sie auf GitHub-Master:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Leider veröffentlichen nicht diese Änderung Web Frontend virtuellen Computer. Virtuellen Computer ermöglichen einen einfachen, aber effektiven Mechanismus für Updates veröffentlichen, damit Sie schnell die Auswirkung einer Änderung in der live-Umgebung beobachten können wir einigen weitere Änderungen vornehmen.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurieren von Front-End-VM
Beachten Sie, dass wir zuvor ein bare Klon des Knotens Todo-Repository auf dem Web-Front-End virtuellen Computer erstellt. Es stellt sich heraus, dass dadurch einen neuen Git remote erstellt, Änderungen abgelegt werden können. Liefert jedoch keine Druck zu diesem remote sehr schnelle iterationsmodell, das Entwickler suchen Wenn Sie ihren Code arbeiten.

Wir wollen möglich ist sicherzustellen, dass tritt ein Push an das remote-Repository auf dem virtuellen Computer TODO-Anwendung automatisch aktualisiert. Glücklicherweise ist dies einfach Git.

Git stellt eine Reihe von Hooks, die zu bestimmten Zeiten reagieren auf Aktionen im Repository aufgerufen werden. Diese werden mithilfe von Shell-Skripten in des Repositorys angegeben `hooks` Ordner. Der Haken, die die AutoUpdate-Szenario für die `post-update` Ereignis.

In einer SSH-Sitzung Web Frontend virtuellen Computer ändern die `~/node-todo.git/hooks` Verzeichnis und fügen Sie den folgenden Inhalt in einer Datei namens `post-update` (ersetzt `machinename` und `region` mit Ihrem virtuellen MongoDB):

    #!/bin/bash
    
    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info
    
Sicherstellen Sie, dass diese Datei ausführbar ist, durch den folgenden Befehl ausführen:

    chmod 755 post-update

Dieses Skript wird sichergestellt, dass die aktuelle Serveranwendung beendet der Code in der geklonten Repository der neuesten aktualisiert, aktualisierten Abhängigkeiten sind und der Server. Es wird sichergestellt, dass die Umgebung in Vorbereitung unserer ersten Anwendungsupdate empfangen zu MongoDB-Instanz aus einer Umgebungsvariablen konfiguriert wurde.

### <a name="configuring-your-development-machine"></a>Konfigurieren der Entwicklungscomputer
Jetzt erhalten wir Ihrem Entwicklungscomputer Web Frontend virtuellen Computer angeschlossen. Dies ist so einfach wie das Hinzufügen von bare Repository auf dem virtuellen Computer als Fernbedienung. Führen Sie den folgenden Befehl dazu ( *Benutzer* mit Web Frontend virtuellen Computer Benutzername und *Computername* und *Region* gegebenenfalls ersetzen):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

Dies ist erforderlich aktivieren drücken oder Web Frontend virtuellen Computer ändert sich veröffentlichen.

### <a name="publishing-updates"></a>Publishing-Updates

Wir veröffentlichen eine Änderung, die so weit, dass die Anwendung eigene Instanz MongoDB vorgenommen wurde:

    git push azure master

Sie sollten wie folgt ausgegeben:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Nachdem dieser Befehl abgeschlossen ist, aktualisieren Sie die Anwendung in einem Webbrowser. Sie sollen sehen, dass hier Aufgabenliste leer und nicht freigegebenen bereitgestellten MongoDB-Instanz an.

Um das Lernprogramm abzuschließen, lassen Sie ein anderes, besser sichtbar ändern. Öffnen Sie auf dem Entwicklungscomputer die node-todo/public/index.html-Datei mit Ihrem bevorzugten Editor. Suchen Sie Großbildschirm Header und ändern Sie den Titel "Ich Todo-Aholic", "Ich Todo-Aholic auf Azure!".

Jetzt sehen wir übernehmen:

    git commit -am "Azurify the title"

Dieses Mal, wir die Änderung in Azure veröffentlichen, bevor Sie zurück zu GitHub Repository verschieben:

    git push azure master

Sobald dieser Befehl abgeschlossen ist, aktualisiert die Webseite, und Sie sehen die Änderung. Seit gut, schieben Sie die Änderung wieder auf den Ursprung remote: 

    git push origin master

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel zeigte, wie eine Anwendung Node.js und Linux virtuellen Computern in Azure bereitstellen. Weitere Informationen über virtuelle Linux-Computer in Azure finden Sie unter [Einführung in Linux auf Azure](/documentation/articles/virtual-machines-linux-introduction/).
    
Weitere Informationen zu Azure Node.js Anwendungsentwicklung [Node.js Developer Center](/develop/nodejs/)anzeigen
