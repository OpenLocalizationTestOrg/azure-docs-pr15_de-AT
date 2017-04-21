<properties
    pageTitle="Remote Desktop Linux VM | Microsoft Azure"
    description="Informationen Sie zum Installieren und Konfigurieren von Remotedesktop Verbindung zu einem Microsoft Azure Linux VM"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Remotedesktop Verbindung zu einem Microsoft Azure Linux VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="overview"></a>Übersicht

RDP (Remote Desktop Protocol) ist ein proprietäres Protokoll für Windows. Wie können wir RDP Remoteverbindung zum Linux VM (Virtual Machine)?

Diese Anleitung erhalten Sie die Antwort! Helfen Sie installieren und konfigurieren Xrdp auf Ihrer Microsoft Azure Linux VM und können für die Verbindung mit dem Remotedesktop von einem Windows-Computer. Das Beispiel in diesem Handbuch unter Ubuntu oder OpenSUSE Linux VM werden.

Xrdp ist ein open-Source-RDP-Server dem Linux-Server mit dem Remotedesktop von einem Windows-Computer herstellen kann. Besser als VNC (Virtual Network Computing) durchgeführt. VNC ist dieser Streifen "JPEG" Qualität und langsam Verhalten RDP schnell und klar ist.


> [AZURE.NOTE] Sie müssen ein Microsoft Azure VM unter Linux. Zum Erstellen und Einrichten einer Linux-VM finden im [Lernprogramm Azure Linux VM](virtual-machines-linux-classic-createportal.md).


##<a name="create-endpoint-for-remote-desktop"></a>Erstellen Sie für den Remotedesktop
Der Standardendpunkt 3389 werden für Remotedesktop in diesem Dokument. So richten Sie 3389 Endpunkt als Remote Desktop Linux VM wie unten:


![Bild](./media/virtual-machines-linux-classic-remote-desktop/no1.png)


Wenn Sie nicht, dass zum Endpunkt der VM einrichten wissen, finden Sie unter [Hinweise](virtual-machines-linux-classic-setup-endpoints.md).


##<a name="install-gnome-desktop"></a>Gnome-Desktop installiert

Mit Linux VM durch kitten und installieren `Gnome Desktop`.

Für Ubuntu verwenden:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


Verwenden Sie für OpenSUSE:

    #sudo zypper install gnome-session

##<a name="install-xrdp"></a>Xrdp installieren

Für Ubuntu verwenden:

    #sudo apt-get install xrdp

Verwenden Sie für OpenSUSE:

> [AZURE.NOTE] Aktualisierung die OpenSUSE-Version mit der dabei in folgenden Befehl unten ein Beispielbefehl für wird `OpenSUSE 13.2`.

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


##<a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Xrdp starten und Xdrp Service beim Computerstart festlegen

Verwenden Sie für OpenSUSE:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

Für Ubuntu Xrdp gestartet und Eanbled beim Computerstart automatisch nach der Installation.

##<a name="using-xfce-if-you-are-using-ubuntu-version-later-than-ubuntu-1204lts"></a>Xfce verwenden, wenn Sie Ubuntu-Version höher als Ubuntu 12.04LTS verwenden

Weil aktuelle Xrdp von Ubuntu Version als Ubuntu 12.04LTS Gnome-Desktop nicht unterstützen, werden `xfce` Desktop statt.

Installieren Sie `xfce`, verwenden:

    #sudo apt-get install xubuntu-desktop

Aktivieren Sie `xfce`, verwenden:

    #echo xfce4-session >~/.xsession

Bearbeiten Sie die Config-Datei `/etc/xrdp/startwm.sh`, verwenden:

    #sudo vi /etc/xrdp/startwm.sh   

Zeile hinzufügen `xfce4-session` vor dem `/etc/X11/Xsession`.

Xrdp Dienst, verwenden:

    #sudo service xrdp restart


##<a name="connect-your-linux-vm-from-a-windows-machine"></a>Linux VM aus einem Windows-Computer herstellen
In einem Windows-Computer der Remotedesktopclient starten, geben Sie Ihren Linux VM DNS-Namen zu oder `Dashboard` der VM in Azure-Verwaltungsportal und auf `Connect` Linux VM Verbindung Siehe unten Anmeldefenster:

![Bild](./media/virtual-machines-linux-classic-remote-desktop/no2.png)

Melden Sie sich mit den `user`  &  `password` Ihrer Linux VM und der Remotedesktop von Microsoft Azure Linux-VM jetzt!


##<a name="next"></a>Weiter
Weitere Informationen mit Xrdp konnte finden Sie [hier](http://www.xrdp.org/).
