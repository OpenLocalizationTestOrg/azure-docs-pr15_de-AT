<properties
   pageTitle="Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch | Microsoft Azure"
   description="Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-windows-virtual-machines-vms--deployment-guide"></a>Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[Dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch DBMS) [Dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Zwischenspeichern für VMs und VHDs) [Dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software-RAID) [Dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage) [Dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Struktur einer Bereitstellung RDBMS) [Dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (hohe Verfügbarkeit und Disaster Recovery mit Azure VMs) [dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85AF-73666a6f7268 (SQL Server 2012 SP1 CU4 und höher) [Dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 und früheren Versionen) [Dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (mit einer SQL Server-Bilder aus Microsoft Azure Marketplace) [Dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Allgemeine SQL Server für SAP Azure Zusammenfassung) [Dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Einzelheiten zum SQL Server-RDBMS) [Dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Speicherkonfiguration) [dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Sicherung und Wiederherstellung) [Dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Leistungsaspekte für Sicherung und Wiederherstellung) [Dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (andere) [Dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[Deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch) [Deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP Ressourcen) [Deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild) [deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Szenario 1: Bereitstellen eines virtuellen Computers Azure Markt für SAP) [deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP) [deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md# a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Szenario 3: Verschieben einer VM lokal eine virtuelle Festplatte nicht verallgemeinert Azure mit SAP) [Deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Bereitstellung Szenarien der VMs für SAP auf Microsoft Azure) [Deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Bereitstellen von Azure PowerShell-Cmdlets) [Deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download und Import SAP relevanten PowerShell-Cmdlets) [Deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join VM in der lokalen Domäne - nur Windows) [Deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md# 6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [Deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (herunterladen, installieren und aktivieren Azure VM Agent) [Deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [Deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [Deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurieren Azure erweiterte Überwachung Erweiterung für SAP) [Deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness überprüfen Azure erweiterte Überwachung von SAP) [Deployment Guide 5.2]: Virtual-Machines-Windows-SAP-Deployment-Guide.MD#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health Check für Azure Infrastruktur Überwachungskonfiguration) [Deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Weitere Problembehandlung Überwachen der Azure-Infrastruktur für SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[Planning-guide]:virtual-machines-windows-sap-planning-guide.md (SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Planung und Implementierungshandbuch) [Planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Ressourcen) [Planning-guide-11]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High Availability (HA) und Disaster Recovery (DR) für SAP NetWeaver auf Azure-Computer ausgeführt) [Planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (hohe Verfügbarkeit für SAP-Anwendungsserver) [Planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Autostart für SAP-Instanzen verwenden) [planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (nur Cloud - VM-Bereitstellung in Azure ohne Abhängigkeit von lokalen Kundennetzwerk) [Planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-lokal - Bereitstellung von einem oder mehreren SAP VMs in Azure mit dem lokalen Netzwerk vollständig integriert wird) [Planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure Regionen) [Planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fehlerdomänen) [Planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Domänen aktualisieren) [Planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure Verfügbarkeit Sets) [Planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (der Microsoft Azure Virtual Machine-Konzept) [Planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Speicher) [Planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Verschieben einer VM lokal in Azure mit einer Diskette nicht verallgemeinert) [Planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Bereitstellen eines virtuellen Computers mit einem bestimmten Kunden-Image) [planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Vorbereitung zum Verschieben einer VM lokal in Azure mit einer Datenträger nicht verallgemeinert) [Planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Vorbereitung Bereitstellen eines virtuellen Computers mit einer bestimmten Kunden für SAP) [Planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (VMs mit SAP für Azure vorbereiten) [Planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Unterschied zwischen einer Azure und Azure Abbildern) [Planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (VHD lokal in Azure hochladen) [Planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Kopieren von Festplatten zwischen Azure-Speicherkonten) [Planung-Handbuch-5.5.1]: Virtual-Machines-Windows-SAP-Planning-Guide.MD#4efec401-91e0-40c0-8e64-f2dceadff646 (VM-VHD Struktur für SAP-Installationen) [Planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Einstellung Automount für angeschlossene Laufwerke) [Planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (einzelne VM mit SAP NetWeaver Demo-Schulung Szenario) [Planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Konzepte Cloud-Only Bereitstellung von SAP-Instanzen) [Planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring-Lösung für SAP) [planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md# ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium-Speicher)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-windows-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

Microsoft Azure ermöglicht Unternehmen in kürzester Zeit ohne lange beschaffungszyklen Compute und Speicherressourcen zu. Azure Virtual Machines können klassische Anwendung wie SAP NetWeaver Anwendung in Azure bereitstellen und Zuverlässigkeit und Verfügbarkeit ohne weitere Ressourcen verfügbaren lokalen erweitern. Microsoft Azure unterstützt auch standortübergreifende Konnektivität ermöglicht Unternehmen, ihre lokalen Domänen ihre Private Clouds und ihrer SAP-Systemlandschaft aktiv Azure Virtual Machines integrieren.

Dieses White Paper beschreibt Schritt für Schritt, wie Azure Virtual Machine für die Bereitstellung von SAP NetWeaver-basierte Anwendung bereit ist. Es wird vorausgesetzt, dass die Informationen in die Planung und Implementierungshandbuch] [Planungshandbuch] bezeichnet. Wenn nicht das jeweilige Dokument zuerst gelesen werden soll.

Das Papier ergänzt die SAP-Installationsdokumentation und Hinweise die primären Ressourcen für Installation und Bereitstellung von SAP-Software auf angegebenen Plattformen darstellen.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="introduction"></a>Einführung
Eine große Anzahl von Unternehmen verwenden SAP NetWeaver-basierte Applikationen – vor allem SAP Business Suite – ihre Mission kritische Ausführen von Geschäftsprozessen. Systemstatus ist daher eine wichtige Ressource und Bereitstellen von Enterprise Support im Fehlerfall einschließlich Performance-Ereignissen wird eine wichtige Anforderung.
Microsoft Azure bietet eine hervorragende Plattform Instrumentation Unterstützung an alle Unternehmen kritische Applikationen angepasst. Dieses Handbuch stellt sicher, dass ein Ziel für die Bereitstellung von SAP-Software so konfiguriert ist, dass Enterprise Support, unabhängig bieten zu, wie die virtuellen Computer erstellt wird, Microsoft Azure Virtual Machine es Azure Markt oder ein kundenspezifisches Image.
Schritte werden im folgenden, alle erforderlichen Setup ausführlich beschrieben.

## <a name="prerequisites-and-resources"></a>Komponenten und Ressourcen
### <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie beginnen, stellen Sie sicher, dass die erforderlichen Komponenten, die in den folgenden Kapiteln beschrieben sind erfüllt werden.

#### <a name="local-personal-computer"></a>Lokaler PC
Die Einrichtung einer Azure Virtual Machine für die Bereitstellung von SAP-Software umfasst mehrere Schritte. Um Windows-VMs oder Linux VMs verwalten, müssen Sie ein PowerShell-Skript und Microsoft Azure-Portal verwenden. Dafür ist eine lokale Personal Computer unter Windows 7 oder höher erforderlich. Wenn Sie nur Linux VMs verwalten und Linux-Maschine für diesen Vorgang verwenden möchten, können Sie auch der Azure-Befehlszeilenschnittstelle (CLI Azure).

#### <a name="internet-connection"></a>Internet-Verbindung
Herunterladen und Ausführen der erforderlichen Tools und Skripts, ist ein Internetzugang erforderlich. Microsoft Azure Virtual Machine ausführen Azure erweiterte Überwachung Erweiterung benötigt außerdem Zugriff auf das Internet. Bei diesem Azure VM Azure Virtual Network oder lokalen Domäne sicherstellen, dass die entsprechenden Proxyeinstellungen sind in Kapitel [Proxy konfigurieren] [ deployment-guide-configure-proxy] in diesem Dokument.

#### <a name="microsoft-azure-subscription"></a>Microsoft Azure-Abonnement
Ein Azure-Konto bereits vorhanden ist und nach Anmeldeinformationen bezeichnet.

#### <a name="topology-consideration-and-networking"></a>Topologie berücksichtigen und Netzwerke
Die Topologie und die Architektur der SAP-Bereitstellung in Azure muss definiert werden. Architektur in Bezug auf:

* Microsoft Azure Storage-Konto verwendet werden
* Virtuelles Netzwerk in das SAP-System bereitstellen
* Ressourcengruppe in das SAP-System bereitstellen
* Azure-Region im SAP-System bereitstellen
* SAP-Konfiguration (Ebene 2 oder 3-Tier)
* VM-Größe und Anzahl der zusätzlichen Festplatten der Unterbrechung bereitgestellt werden
* SAP-Transport und Korrektur Systemkonfiguration

Azure Speicherkonten oder Azure virtuelle Netzwerke sollte so erstellt und wurden bereits konfiguriert. Zum Erstellen und konfigurieren sie fällt in die Planung und Implementierungshandbuch] [Planungshandbuch].

#### <a name="sap-sizing"></a>Dimensionierung von SAP
* Geplante SAP Arbeitslast festgelegt wurden, z.B. mit SAP-Quicksizer und die entsprechende Anzahl SAPS bekannt 
* Die erforderliche CPU-Ressource und den Speicherverbrauch des SAP-Systems sollte bekannt sein
* Die erforderliche e/a-Vorgänge pro Sekunde sollte bekannt sein
* Die erforderliche Netzwerkbandbreite eventuelle Kommunikation zwischen verschiedenen VMs in Azure bekannt
* Die erforderliche Bandbreite zwischen den lokalen und Azure bereitgestellt SAP Systemen bekannt

#### <a name="resource-groups"></a>Ressourcengruppen
Ressourcengruppen sind ein neues Konzept, das alle Ressourcen enthalten, die den gleichen Lebenszyklus z.B. sie erstellt und gleichzeitig gelöscht werden. [Lesen Sie] [ resource-group-overview] Weitere Informationen zu Ressourcengruppen. 

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-Ressourcen
Bei der Konfiguration werden die folgenden Ressourcen benötigt:

* SAP-Hinweis [1928533]
    * die Liste der Azure Virtual Machine-Größen, die für die Bereitstellung von SAP-Software unterstützt werden 
    * wichtige Informationen pro Azure Virtual Machine
    * unterstützt SAP-Software und Kombination von Betriebssystem und DB
* SAP-Hinweis [2015553] mit Komponenten SAP unterstützt SAP Software in Microsoft Azure bereitstellen.
* SAP-Hinweis [1999351] enthält weitere Informationen zur Problembehandlung für erweiterte Azure Überwachung für SAP.
* SAP-Hinweis [2178632] mit Detailinformationen auf alle verfügbaren Überwachung Metriken für SAP auf Microsoft Azure. 
* SAP-Hinweis [1409604] mit erforderlichen SAP Host Agent-Version für Windows Azure Microsoft bei der Bereitstellung der neuen Azure Ressourcenmanager.
* SAP-Hinweis [2191498] mit erforderlichen SAP Host Agent-Version für Linux auf Microsoft Azure, bei der Bereitstellung der neuen Azure Ressourcenmanager.
* SAP-Hinweis [2243692] mit Informationen über Lizenzierung für SAP unter Linux auf Azure
* SAP-Hinweis [1984787] enthält allgemeine Informationen über SUSE LINUX Enterprise Server 12
* SAP-Hinweis [2002167] allgemeine Informationen Red Hat Enterprise Linux 7.x
* [SCN](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , die alle enthält erforderlich Hinweise für Linux
* SAP-spezifische [Azure PowerShell] gehören PowerShell-cmdlets[azure-ps]
* SAP bestimmte Azure CLI sind Teil der [Azure-CLI][azure-cli]
* [Microsoft Azure-Portal][azure-portal]

[comment]: <> (MSSedusch TODO hinzufügen ARM Patchebene für SAP Host Agent im SAP-Hinweis 1409604)
 
Die folgenden Handbücher behandelt das Thema von SAP auf Microsoft Azure:

* [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Planung und Implementierungshandbuch] [Planungshandbuch]
* [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch (dieses Dokument)] [Bereitstellungshandbuch]
* [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch DBMS] [Dbms-Guide]
* [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch für hohe Verfügbarkeit][ha-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Bereitstellungsszenarien für SAP auf Microsoft Azure VMs
In diesem Kapitel lernen Sie die verschiedenen Methoden zur Bereitstellung und die einzelnen Schritte für jede Bereitstellung.

### <a name="deployment-of-vms-for-sap"></a>Bereitstellung von VMs für SAP
Microsoft Azure bietet unterschiedliche VMs und zugeordnete Laufwerke. Dabei ist es wichtig, die Unterschiede seit Vorbereitung der VMs hängt die Bereitstellung abweichen. Im Allgemeinen sehen wir in den folgenden Szenarien:

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Bereitstellen eines virtuellen Computers aus dem Azure-Markt
Microsoft oder 3rd Party Bild Markt Azure VM bereitstellen möchten. Nachdem Sie die VM auf Microsoft Azure bereitgestellt, führen Sie die gleichen Richtlinien und Tools, die SAP in der VM installieren wie in einer lokalen Umgebung. Für die SAP-Software der Azure-VM installieren, empfehlen SAP und Microsoft speichern die SAP-Installationsmedien in Azure VHDs oder Erstellen einer Azure VM als "Dateiserver" enthält alle erforderlichen SAP-Installationsmedien arbeiten.

[comment]: <> (MSSedusch TODO warum wir brauchen Dateimanagement z. B. Dateiserver oder VHD empfehlen? Ist das lokale so unterschiedlich?)

Weitere Informationen finden Sie in Kapitel [Szenario 1: Bereitstellen eines virtuellen Computers Azure Markt für SAP] [Deployment Guide 3.2].

#### <a name="3688666f-281f-425b-a312-a77e7db2dfab"></a>Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild
Aufgrund bestimmter Patch in Bezug zur OS oder DBMS möglicherweise bereitgestellten Bilder aus dem Azure-Markt nicht Ihren Bedürfnissen. Daher müssen Sie eine eigene "private" OS-DB VM Bild mehrmals danach einsetzbare VM erstellen.
Die Schritte zum Erstellen eines privaten Bildes unterscheiden zwischen einem Windows- und Linux-Bild.

___

> ![Windows][Logo_Windows] Windows
>
> Zum Vorbereiten eines Windows-Abbilds, mit der mehrere virtuelle Maschinen bereitstellen, muss Windows-Einstellungen (wie Windows-SID und Hostname) abstrahiert/generalized auf lokale VM. Dies kann mithilfe von Sysprep wie unter <https://technet.microsoft.com/library/cc721940.aspx>beschrieben.
>
> ![Linux][Logo_Linux] Linux
>
> Um eine Linux-Abbilds, mit der mehrere virtuelle Computer bereitstellen, müssen Linux Einstellungen auf lokale VM abstrahiert/generalized sein. Dies kann mit Waagent-entziehen, wie in [diesem Artikel] beschriebenen[ virtual-machines-linux-capture-image] oder in [diesem Artikel][virtual-machines-linux-agent-user-guide-command-line-options].

___

Entweder mit dem SAP Software Bereitstellung-Manager ein neues SAP-System installieren, Sicherung einer Datenbank aus einer virtuellen Festplatte mit dem virtuellen Computer verbunden ist oder eine Sicherung von Azure-Speicher direkt wiederherstellen, wenn das DBMS unterstützt kann der Inhalt der Datenbank eingerichtet werden. (Siehe [DBMS Bereitstellung Guide][dbms-guide]). Wenn Sie ein SAP-System in Ihrer lokalen VM (besonders bei 2-Tier-Systemen) installiert haben, können Sie nach der Bereitstellung der Azure-VM System umbenennen Verfahren von SAP-Software-Bereitstellung der Hinweis ( [1619720]) unterstützt die SAP-Systemeinstellungen anpassen. Andernfalls können Sie die SAP-Software nach der Bereitstellung der Azure-VM installieren.

Weitere Informationen finden Sie in Kapitel [Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP] [Deployment Guide 3.3].

#### <a name="moving-a-vm-from-on-premises-to-microsoft-azure-with-a-non-generalized-disk"></a>Verschieben einer VM von lokalen Microsoft Azure mit einer Festplatte nicht verallgemeinert
Sie möchten ein bestimmtes SAP-System von lokalen Microsoft Azure verschieben. Dies erfolgt durch die VHD-Datei enthält das Betriebssystem, die SAP-Binärdateien und eventuelle DBMS-Binärdateien sowie die VHDs mit Daten- und des DBMS Microsoft Azure hochladen. Im Gegensatz zu den beschriebenen in Kapitel [Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild] [Deployment-Handbuch-3.1.2] über Sie möglichst Hostname, SAP SID und SAP-Benutzerkonten in der Azure-VM sie in der lokalen Umgebung konfiguriert wurden. Daher ist es nicht erforderlich, die das Betriebssystem verallgemeinern. Hier gilt hauptsächlich für standortübergreifende Szenarien, wo ein Teil der SAP-Landschaft lokalen und Teile auf Microsoft Azure ausgeführt wird.

Weitere Informationen finden Sie in Kapitel [Szenario 3: Verschieben einer VM lokal mit SAP eine virtuelle Festplatte nicht verallgemeinert Azure] [Deployment Guide 3.4].

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Szenario 1: Bereitstellen eines virtuellen Computers Azure Markt für SAP
Microsoft Azure bietet die Möglichkeit zum Bereitstellen einer Instanz VM Markt Azure bietet einige standard Betriebssystem-Images von Windows Server und andere Linux-Distributionen. Es kann auch ein Bild bereitstellen, z. B. SQL Server DBMS-SKUs enthält. DBMS-SKUs Bilder mit Details finden Sie [DBMS-Bereitstellungshandbuch] [Dbms-Guide]

SAP Folge von Schritten Bereitstellen eines virtuellen Computers Azure Markt würde so aussehen:

![Flussdiagramm der VM-Bereitstellung für SAP-Systeme mit einem VM-Image Azure Marketplace][deployment-guide-figure-100]

Nach dem Flussdiagramm müssen die folgenden Schritte ausgeführt werden:

#### <a name="create-virtual-machine-using-the-azure-portal"></a>Virtuelle Maschine mit Azure-Portal
Am einfachsten erstellen Sie einen neuen virtuellen Computer mit einem Bild von Azure Marketplace ist der Azure-Portal. Navigieren Sie zu <https://portal.azure.com/#create>. Eingeben des Betriebssystems in das Suchfeld z.B. Windows, SLES oder RHEL bereitstellen und die Version auswählen möchten. Stellen Sie sicher, wählen Sie das Bereitstellungsmodell "Azure-Ressourcen-Manager" und klicken Sie auf erstellen.

Der Assistent führt Sie durch die erforderlichen Parameter zum Erstellen des virtuellen Computers mit allen erforderlichen Ressourcen wie Netzwerkschnittstellen oder Speicherkonten. Diese Parameter sind:

1. Grundlagen
    1. Name: Der Name der Ressource z. B. Name des virtuellen Computers
    1. Benutzername und Kennwort-SSH öffentlichen Schlüssel: Geben Sie den Benutzernamen und das Kennwort des Benutzers während der Bereitstellung erstellt wird. Für einen virtuellen Linux-Maschine können Sie auch den öffentlichen SSH-Schlüssel, die Sie verwenden möchten eingeben Anmeldung mit SSH Computer.
    1. Abonnement: Wählen Sie das Abonnement, das den neuen virtuellen Computer bereitgestellt werden soll.
    1. Ressourcengruppe: Der Name der Ressourcengruppe. Sie können den Namen eine neue Ressourcengruppe oder eine Ressourcengruppe bereits einfügen
    1. Ort: Wählen Sie den Speicherort, den neuen virtuellen Computer bereitgestellt werden soll. Wenn Sie den virtuellen Computer mit dem lokalen Netzwerk herstellen möchten, müssen Sie das virtuelle Netzwerk Speicherort, die Azure mit Ihrem lokalen Netzwerk verbunden. Weitere Informationen finden Sie in Kapitel [Microsoft Azure Netzwerk] [ planning-guide-microsoft-azure-networking] im [Planungshandbuch] [Planungshandbuch].
1. Größe: Lesen Sie Hinweis [1928533] eine Liste der unterstützten VM-Typen. Außerdem stellen Sie sicher, dass den richtigen Typ Premium Speicher verwendet werden soll. Nicht alle virtuellen Computer unterstützen Premium speichern. Kapitel [Speicher: Microsoft Azure-Speicher und Datenträger] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] und [Azure Premium] [Planung-Handbuch-Azure-Premium-Storage] im [Planungshandbuch] [Planungshandbuch] Weitere Informationen.
1. Einstellungen
    1. Speicherkonto: Können Sie ein vorhandenes Speicherkonto auswählen oder eine neue erstellen. Lesen Sie Kapitel [Microsoft Azure Storage] [Dbms Guide 2.3] der [DBMS] [Dbms-Guide] Weitere Informationen über die verschiedenen Typen. Beachten Sie, dass nicht alle Speicher für die Ausführung von SAP-Applikationen unterstützt werden.
    1. Virtuelles Netzwerk und Subnetz: Wählen Sie das virtuelle Netzwerk zu Ihrem lokalen Netzwerk verbunden ist, wenn Sie den virtuellen Computer in Ihrem Intranet integrieren möchten.
    1. Öffentliche IP-Adresse: Wählen Sie die öffentliche IP-Adresse, die Sie oder geben Sie die Parameter, um eine neue öffentliche IP-Adresse erstellen möchten. Eine öffentliche IP-Adresse können Sie den virtuellen Computer über das Internet zugreifen. Vergewissern Sie sich außerdem eine Sicherheitsgruppe Netzwerk Zugriff auf den virtuellen Computer Filtern erstellen.
    1. Netzwerk-Sicherheitsgruppe: finden Sie unter [Network Security Group (NSG)] [ virtual-networks-nsg] Weitere Informationen.
    1. Überwachung: Sie können die Einstellung Diagnose deaktivieren. Es aktiviert automatisch beim Ausführen der Befehle ermöglichen Azure erweiterte Überwachung [Konfigurieren überwachen]Kapitel[deployment-guide-configure-monitoring-scenario-1].
    1. Verfügbarkeit: Wählen Sie eine Verfügbarkeit oder geben Sie die Parameter, um einen neuen Verfügbarkeit erstellen. Weitere Informationen finden Sie in Kapitel [Azure Verfügbarkeit legt] [Planung-Handbuch-3.2.3].
1. Zusammenfassung: Überprüfen Sie die Angaben auf der Zusammenfassungsseite und klicken Sie auf OK.

Nach Abschluss des Assistenten wird dem virtuellen Computer in der Ressourcengruppe bereitgestellt gewählte.

#### <a name="create-virtual-machine-using-a-template"></a>Erstellen Sie virtueller Computer mithilfe einer Vorlage
Sie können eine Bereitstellung mit einem SAP-Vorlagen veröffentlicht in [Azure-Schnellstart-Vorlagen Github Repository][azure-quickstart-templates-github]. Oder erstellen Sie einen virtuellen Computer mithilfe der [Azure-Portal][virtual-machines-windows-tutorial], [PowerShell] [ virtual-machines-ps-create-preconfigure-windows-resource-manager-vms] oder [Azure CLI] [ virtual-machines-linux-tutorial] manuell.

* [2-Tier-Konfiguration (nur ein virtueller Computer) Vorlagen] [ sap-templates-2-tier-marketplace-image] dieser Vorlage verwenden, wenn Sie ein nur ein virtueller Computer mit 2-Tier-System zu erstellen.
* [3-Tier-Konfiguration (mehrere virtuelle Maschinen) Vorlagen] [ sap-templates-3-tier-marketplace-image] verwenden Sie diese Vorlage, wenn Sie ein 3-Tier-System mit mehreren virtuellen Maschinen erstellen möchten.

Nach der Vorlagen oben öffnen navigiert Azure-Portal zum Fenster Parameter bearbeiten. Geben Sie Folgendes ein:

* **SapSystemId**: die ID des SAP-Systems
* **OsType**: Betriebssystem bereitstellen, z. B. Windows Server 2012 R2, SLES 12 oder RHEL 7.2 werden soll
    * Die Liste enthält nur Versionen von SAP auf Microsoft Azure unterstützt werden
* **SapSystemSize**: die Größe des SAP-Systems
    * Der Betrag der SAPS das neue System. Wenn Anzahl SAPS nicht sicher das System benötigen sind, wenden Sie sich an Ihre SAP Technology Partner oder Systemintegrator
* **SystemAvailability**: (nur 3-Tier-Vorlage) Verfügbarkeit 
    * HA für eine Konfiguration für HA-Installation wählen. Zwei Datenbankserver, und zwei Server für die Mobile Datenerfassung erstellt.
* StorageType: (nur 2-Tier-Vorlage) Speicher verwendet werden soll 
    * Für größere Systeme wird die Verwendung von Premium-Speicher empfohlen. Informationen Sie weitere über die verschiedenen Arten 
        * [Microsoft Azure Storage] [Dbms-Handbuch-2,3] des [DBMS] [Dbms-Guide]
        * [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten][storage-premium-storage-preview-portal]
        * [Einführung in Microsoft Azure-Speicher][storage-introduction]
* **AdminUsername** und **AdminPassword**: Benutzername und Kennwort
    * Ein neuer Benutzer erstellt, die sich auf dem Computer verwendet werden.
* **NewOrExistingSubnet**: Legt fest, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Haben Sie bereits ein virtuelles Netzwerk, das mit dem lokalen Netzwerk verbunden ist, wählen Sie vorhandene.
* **SubnetId**: die ID des Subnetzes auf dem virtuellen Computer sollen verbunden sein. Wählen Sie das Subnetz des VPN oder Express Route virtuellen Netzwerk virtuellen Computer mit dem lokalen Netzwerk herstellen. Die ID in der Regel sieht /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Nach der Eingabe aller Parameter wählen Sie das Abonnement und die Ressourcengruppe verwenden möchten. Wählen Sie eine vorhandene Ressourcengruppe oder erstellen Sie eine neue Auswahl "+ neue" im Kontextmenü. Wenn Sie eine neue Ressourcengruppe erstellen, müssen Sie die Region auswählen, in der Ressourcengruppe und den virtuellen Computer erstellt werden.

Überprüfen Sie die Vertragsbedingungen akzeptieren Sie und klicken Sie auf erstellen.

Beachten Sie, dass der Azure-VM-Agent standardmäßig bereitgestellt wird, wenn ein Bild aus dem Azure Marketplace verwenden.

#### <a name="configure-proxy-settings"></a>Proxyeinstellungen konfigurieren
Je nach Konfiguration des lokalen Netzwerks möglicherweise den Proxy auf dem virtuellen Computer konfigurieren, der an Ihrem lokalen Netzwerk über VPN oder Express Route erforderlich. Andernfalls den virtuellen Computer möglicherweise nicht auf das Internet zugreifen und daher kann nicht der erforderlichen Erweiterung herunterladen oder Überwachungsdaten sammeln. Kapitel [Proxy konfigurieren] [ deployment-guide-configure-proxy] dieses Dokuments.

#### <a name="join-domain-windows-only"></a>Domäne (nur Windows)
Bei die Bereitstellung in Azure mit lokalen besteht AD und DNS Azure-Standorten per Express Route (wird auch als standortübergreifende in der [Planung und Implementierung Guide][planning-guide]) wird erwartet, dass die VM eine lokale Domäne beitreten. Aspekte dieses Schritts werden in Kapitel [Join VM in der lokalen Domäne (nur Windows)] [Deployment Guide 4.3] dieses Dokuments beschrieben.

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurieren der Überwachung
Konfigurieren der Azure erweiterte Überwachung Erweiterung für SAP Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment-Handbuch-4,5] dieses Dokuments.

Überprüfen der erforderlichen Komponenten für die Überwachung von SAP minimale Versionen von SAP Kernel und SAP Host Agent in Kapitel SAP Ressourcen [Deployment Guide 2.2] dieses Dokuments aufgeführten Ressourcen.

#### <a name="monitoring-check"></a>Überwachen von Kontrollkästchen
Überprüfen Sie, ob die Überwachung funktioniert Kapitel [überprüft und Problembehandlung für End-to-End Monitoring Setup für SAP in Azure][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Buchen von Bereitstellungsschritten
Nach dem Erstellen der VM wird bereitgestellt werden und ist dann alle erforderlichen Softwarekomponenten in der VM installieren. Daher würde diese Bereitstellung benötigen entweder der Software installierte Knotenwert bereits für Microsoft Azure in andere virtuelle Computer oder Datenträger zugeordnet werden kann. Oder suchen wir nach Szenarien standortübergreifende Konnektivität für lokale Ressourcen (Shares installieren) ist eine bestimmte.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP
Wie in die Planung und Implementierungshandbuch] [Planungshandbuch] bereits detaillierte Schritte ist eine Möglichkeit zum Vorbereiten und erstellen ein benutzerdefiniertes Bild mehrere neue VMs erstellen. Die Reihenfolge der Schritte des Flussdiagramms aussehen würde:
 
![Flussdiagramm der Bereitstellung für SAP-Systeme mit einem VM-Image Private Markt][deployment-guide-figure-300]

Nach dem Flussdiagramm müssen die folgenden Schritte ausgeführt werden:

#### <a name="create-virtual-machine"></a>Erstellen virtueller Computer
Verwenden Sie zum Erstellen einer Bereitstellung mit einer privaten Betriebssystemabbild Azure-Portal der [Azure-Schnellstart-Vorlagen Github Repository]am SAP-Vorlagen[azure-quickstart-templates-github].
Zusätzlich können Sie einen virtuellen Computer mit [PowerShell] [ virtual-machines-upload-image-windows-resource-manager] manuell. 

* [2-Tier-Konfiguration (nur ein virtueller Computer) Vorlagen] [ sap-templates-2-tier-user-image] Verwendung einer 2-Tier-System nur ein virtueller Computer mit eigenen Betriebssystemabbild erstellt werden soll.
* [3-Tier-Konfiguration (mehrere virtuelle Maschinen) Vorlagen] [ sap-templates-3-tier-user-image] eine 3-Tier-System mehrere virtuelle Maschinen mit eigenen Betriebssystemabbild soll diese Vorlage verwenden.

Nach der Vorlagen oben öffnen navigiert Azure-Portal zum Fenster Parameter bearbeiten. Geben Sie Folgendes ein:

* **SapSystemId**: die ID des SAP-Systems
* **OsType**: Betriebssystem Windows oder Linux bereitstellen möchten
* **SapSystemSize**: die Größe des SAP-Systems
    * Der Betrag der SAPS das neue System. Wenn Anzahl SAPS nicht sicher das System benötigen sind, wenden Sie sich an Ihre SAP Technology Partner oder Systemintegrator
* **SystemAvailability**: (nur 3-Tier-Vorlage) Verfügbarkeit 
    * HA für eine Konfiguration für HA-Installation wählen. Zwei Datenbankserver, und zwei Server für die Mobile Datenerfassung erstellt.
* **StorageType**: (nur 2-Tier-Vorlage) Speicher verwendet werden soll 
    * Für größere Systeme wird die Verwendung von Premium-Speicher empfohlen. Informationen Sie weitere über die verschiedenen Arten 
        * [Microsoft Azure Storage] [Dbms-Handbuch-2,3] des [DBMS] [Dbms-Guide]
        * [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten][storage-premium-storage-preview-portal]
        * [Einführung in Microsoft Azure-Speicher][storage-introduction]
* **AdminUsername** und **AdminPassword**: Benutzername und Kennwort
    * Ein neuer Benutzer erstellt, die sich auf dem Computer verwendet werden.
* **UserImageVhdUri**: URI der privaten OS Bild Vhd z. B. https://`<accountname`>.blob.core.windows.net/vhds/userimage.vhd
* **UserImageStorageAccount**: Name des private Betriebssystemabbild z. B. Speicherort Speicherkonto `<accountname`> im Beispiel oben genannten URI
* **NewOrExistingSubnet**: Legt fest, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Haben Sie bereits ein virtuelles Netzwerk, das mit dem lokalen Netzwerk verbunden ist, wählen Sie vorhandene.
* **SubnetId**: die ID des Subnetzes auf dem virtuellen Computer sollen verbunden sein. Wählen Sie das Subnetz des VPN oder Express Route virtuellen Netzwerk virtuellen Computer mit dem lokalen Netzwerk herstellen. Die ID in der Regel sieht /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Nach der Eingabe aller Parameter wählen Sie das Abonnement und die Ressourcengruppe verwenden möchten. Wählen Sie eine vorhandene Ressourcengruppe oder erstellen Sie eine neue Auswahl "+ neue" im Kontextmenü. Wenn Sie eine neue Ressourcengruppe erstellen, müssen Sie die Region auswählen, in der Ressourcengruppe und den virtuellen Computer erstellt werden.

Überprüfen Sie die Vertragsbedingungen akzeptieren Sie und klicken Sie auf erstellen.

#### <a name="install-vm-agent-linux-only"></a>Installieren der VM-Agent (nur Linux)
Linux-Agent muss in das Bild bereits installiert sein, wenn Sie die oben aufgeführten Vorlagen verwenden möchten. Andernfalls schlägt die Bereitstellung fehl. Herunterladen und Installieren der VM-Agent in das Bild Kapitel [herunterladen, installieren und Aktivieren von Azure VM-Agent] [Deployment-Handbuch-4,4] dieses Dokuments.
Wenn Sie die oben aufgeführten Vorlagen verwenden, können Sie die VM-Agent anschließend installieren.

#### <a name="join-domain-windows-only"></a>Domäne (nur Windows)
Bei die Bereitstellung in Azure mit lokalen besteht AD und DNS Azure-Standorten per Express Route (wird auch als standortübergreifende in der [Planung und Implementierung Guide][planning-guide]) wird erwartet, dass die VM eine lokale Domäne beitreten. Aspekte dieses Schritts werden in Kapitel [Join VM in der lokalen Domäne (nur Windows)] [Deployment Guide 4.3] dieses Dokuments beschrieben.

#### <a name="configure-proxy-settings"></a>Proxyeinstellungen konfigurieren
Je nach Konfiguration des lokalen Netzwerks möglicherweise den Proxy auf dem virtuellen Computer konfigurieren, der an Ihrem lokalen Netzwerk über VPN oder Express Route erforderlich. Andernfalls den virtuellen Computer möglicherweise nicht auf das Internet zugreifen und daher kann nicht der erforderlichen Erweiterung herunterladen oder Überwachungsdaten sammeln. Kapitel [Proxy konfigurieren] [ deployment-guide-configure-proxy] dieses Dokuments.

#### <a name="configure-monitoring"></a>Konfigurieren der Überwachung
Konfigurieren von Azure Überwachung Erweiterung für SAP Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment-Handbuch-4,5] dieses Dokuments.
Überprüfen der erforderlichen Komponenten für die Überwachung von SAP minimale Versionen von SAP Kernel und SAP Host Agent in Kapitel SAP Ressourcen [Deployment Guide 2.2] dieses Dokuments aufgeführten Ressourcen.

#### <a name="monitoring-check"></a>Überwachen von Kontrollkästchen
Überprüfen Sie, ob die Überwachung funktioniert Kapitel [überprüft und Problembehandlung für End-to-End Monitoring Setup für SAP in Azure][deployment-guide-troubleshooting-chapter].

### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Szenario 3: Verschieben von lokalen SAP eine virtuelle Festplatte nicht verallgemeinert Azure mit einer VM
Dieses Szenario ist ein SAP-System einfach nach seiner aktuellen Form und die Form von lokalen Azure bei Adressierung. Bedeutet, dass kein Name, Ändern der Windows- oder Linux-Hostname und SAP SID oder erfolgt. In diesem Fall die virtuelle Festplatte nicht als Abbild während der Bereitstellung verwiesen jedoch direkt als Betriebssystem-Datenträger verwendet. Hinsichtlich der Bereitstellung unterscheidet dabei die beiden früheren Fälle durch die Tatsache, dass der VM-Agent automatisch während der Bereitstellung installiert werden kann. Daher Azure VM-Agent muss von Microsoft heruntergeladen werden und muss installiert und innerhalb des virtuellen Computers manuell aktiviert danach. Nachdem die Aufgabe erfolgreich war, können Sie weiterhin SAP Host überwachen Azure-Erweiterung und seine Konfiguration. Details zur Funktion der Azure-VM-Agent finden Sie in diesem Artikel:

[comment]: <> (MSSedusch TODO Update Windows untenstehenden Link) 

___

> ![Windows][Logo_Windows] Windows
>
> <http://Blogs.msdn.com/b/wats/Archive/2014/02/17/BGInfo-guest-Agent-Extension-for-Azure-VMs.aspx>
>
> ![Linux][Logo_Linux] Linux
>
> [Azure Linux-Agent-Benutzerhandbuch][virtual-machines-linux-agent-user-guide]

___

Der Workflow die einzelnen Schritte sieht folgendermaßen aus:
 
![Flussdiagramm der VM-Bereitstellung für SAP-Systeme mit einer VM][deployment-guide-figure-400]

Angenommen, die Diskette bereits geuploadet und in Azure (Siehe Planung und Implementierung Guide][planning-guide]) folgendermaßen definiert

#### <a name="create-virtual-machine"></a>Erstellen virtueller Computer
Verwenden Sie zum Erstellen einer Bereitstellung mit einer privaten OS Azure-Portal [Azure-Schnellstart-Vorlagen Github Repository]am SAP-Vorlage[azure-quickstart-templates-github]. Zusätzlich können Sie einen virtuellen Computer mit dem [PowerShell oder Azure CLI manuell.

* [2-Tier-Konfiguration (nur ein virtueller Computer) Vorlage][sap-templates-2-tier-os-disk]
    * Verwenden Sie diese Vorlage, wenn ein 2-Tier-System mit nur einer virtuellen Maschine erstellen möchten.

Nachdem Sie die oben genannte Vorlage geöffnet, navigiert der Azure-Portal zum Fenster Parameter bearbeiten. Geben Sie Folgendes ein:

* **SapSystemId**: die ID des SAP-Systems
* **OsType**: Betriebssystem Windows oder Linux bereitstellen möchten
* **SapSystemSize**: die Größe des SAP-Systems
    * Der Betrag der SAPS das neue System. Wenn Anzahl SAPS nicht sicher das System benötigen sind, wenden Sie sich an Ihre SAP Technology Partner oder Systemintegrator
* **StorageType**: (nur 2-Tier-Vorlage) Speicher verwendet werden soll 
    * Für größere Systeme wird die Verwendung von Premium-Speicher empfohlen. Informationen Sie weitere über die verschiedenen Arten 
        * [Microsoft Azure Storage] [Dbms-Handbuch-2,3] des [DBMS] [Dbms-Guide]
        * [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten][storage-premium-storage-preview-portal]
        * [Einführung in Microsoft Azure-Speicher][storage-introduction]
* **OsDiskVhdUri**: URI des privaten OS Festplatte z. B. https://`<accountname`>.blob.core.windows.net/vhds/osdisk.vhd
* **NewOrExistingSubnet**: Legt fest, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Haben Sie bereits ein virtuelles Netzwerk, das mit dem lokalen Netzwerk verbunden ist, wählen Sie vorhandene.
* **SubnetId**: die ID des Subnetzes auf dem virtuellen Computer sollen verbunden sein. Wählen Sie das Subnetz des VPN oder Express Route virtuellen Netzwerk virtuellen Computer mit dem lokalen Netzwerk herstellen. Die ID in der Regel sieht /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Nach der Eingabe aller Parameter wählen Sie das Abonnement und die Ressourcengruppe verwenden möchten. Wählen Sie eine vorhandene Ressourcengruppe oder erstellen Sie eine neue Auswahl "+ neue" im Kontextmenü. Wenn Sie eine neue Ressourcengruppe erstellen, müssen Sie die Region auswählen, in der Ressourcengruppe und den virtuellen Computer erstellt werden.

Überprüfen Sie die Vertragsbedingungen akzeptieren Sie und klicken Sie auf erstellen.

#### <a name="install-vm-agent"></a>VM-Agent installieren
VM-Agent muss die Betriebssystem-Datenträger bereits installiert sein, wenn Sie die oben aufgeführten Vorlagen verwenden möchten. Andernfalls schlägt die Bereitstellung fehl. Herunterladen und Installieren der VM-Agent auf dem virtuellen Computer Kapitel [herunterladen, installieren und Aktivieren von Azure VM-Agent] [Deployment-Handbuch-4,4] dieses Dokuments.

Wenn Sie die oben aufgeführten Vorlagen verwenden, können Sie die VM-Agent anschließend installieren.

#### <a name="join-domain-windows-only"></a>Domäne (nur Windows)
Bei die Bereitstellung in Azure mit lokalen besteht AD und DNS Azure-Standorten per Express Route (wird auch als standortübergreifende in der [Planung und Implementierung Guide][planning-guide]) wird erwartet, dass die VM eine lokale Domäne beitreten. Aspekte dieses Schritts werden in Kapitel [Join VM in der lokalen Domäne (nur Windows)] [Deployment Guide 4.3] dieses Dokuments beschrieben.

#### <a name="configure-proxy-settings"></a>Proxyeinstellungen konfigurieren
Je nach Konfiguration des lokalen Netzwerks möglicherweise den Proxy auf dem virtuellen Computer konfigurieren, der an Ihrem lokalen Netzwerk über VPN oder Express Route erforderlich. Andernfalls den virtuellen Computer möglicherweise nicht auf das Internet zugreifen und daher kann nicht der erforderlichen Erweiterung herunterladen oder Überwachungsdaten sammeln. Kapitel [Proxy konfigurieren] [ deployment-guide-configure-proxy] dieses Dokuments.

#### <a name="configure-monitoring"></a>Konfigurieren der Überwachung
Konfigurieren von Azure erweiterte Überwachung Erweiterung für SAP Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment-Handbuch-4,5] dieses Dokuments.

Überprüfen der erforderlichen Komponenten für die Überwachung von SAP minimale Versionen von SAP Kernel und SAP Host Agent in Kapitel SAP Ressourcen [Deployment Guide 2.2] dieses Dokuments aufgeführten Ressourcen.

#### <a name="monitoring-check"></a>Überwachen von Kontrollkästchen
Überprüfen Sie, ob die Überwachung funktioniert Kapitel [überprüft und Problembehandlung für End-to-End Monitoring Setup für SAP in Azure][deployment-guide-troubleshooting-chapter].

### <a name="scenario-4-updating-the-monitoring-configuration-for-sap"></a>Szenario 4: Aktualisieren der Überwachungskonfiguration für SAP
Es gibt Fälle, müssten Sie die Konfiguration die Überwachung aktualisieren:

* Gemeinsame MS-SAP-Team erweitert die Überwachungsfunktionen und weitere Leistungsindikatoren hinzufügen oder löschen einige Leistungsindikatoren entschieden. 
* Microsoft stellt eine neue Version des zugrunde liegenden Azure-Infrastruktur liefert die Daten, und die Azure erweiterte Überwachung Erweiterung für SAP zur Anpassung an diese Änderung.
* Zusätzliche Festplatten bereitgestellt, Azure-VM hinzufügen oder Entfernen einer virtuellen Festplatte. In diesem Fall müssen zum Aktualisieren der Auflistung Speicher Daten. Ändern die Konfiguration durch Hinzufügen oder Löschen von Endpunkten oder Zuweisen von IP-Adressen einer VM wird die Konfiguration die Überwachung nicht beeinflusst.
* Sie ändern die Größe der Azure-VM z.B. a5 auf jede beliebige Größe der VM.
* Azure-VM hinzufügen neue Netzwerkschnittstellen

Um die Konfiguration der Überwachung zu aktualisieren, gehen Sie folgendermaßen vor:

* Aktualisieren der Überwachungsinfrastruktur Schritte erläutert in Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment-Handbuch-4,5] dieses Dokuments. Eine Wiederholung der in diesem Kapitel beschriebene Skript erkennt, dass eine Überwachungskonfiguration bereitgestellt und zu der Überwachungskonfiguration führt. 

___

> ![Windows][Logo_Windows] Windows
>
> Für die Aktualisierung des Azure VM ist kein Benutzereingriff erforderlich. VM-Agent automatisch aktualisiert und erfordert keinen Neustart VM.
>
> ![Linux][Logo_Linux] Linux
>
> Führen Sie die Schritte in [diesem Artikel] [ virtual-machines-linux-update-agent] Azure Linux-Agent aktualisieren. 

___

## <a name="detailed-single-deployment-steps"></a>Detaillierte Schritte für einzelne Bereitstellung

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Bereitstellen von Azure PowerShell-cmdlets
* Gehe zu <https://azure.microsoft.com/downloads/>
* Abschnitt ist "Command-Line Tools Abschnitt"Windows PowerShell. Auf den Link 'Installieren'.
* Microsoft Download Manager wird mit einer Position mit .exe enden. Die Option "Ausführen".
* Ein Popup kommen gefragt, ob Sie Microsoft-Webplattform-Installer ausführen. Drücken Sie "Ja"
* Ein Bild wie dieses:
 
![Installationsbildschirm für Azure PowerShell-cmdlets][deployment-guide-figure-500]
<a name="figure-5"></a>

* Drücken Sie installieren, und stimmen Sie dem EULA.

Überprüfen Sie häufig die PowerShell-Cmdlets aktualisiert wurden. In der Regel werden Updates auf einen monatlichen Zeitraum. Die einfachste Möglichkeit hierzu ist die Installation Schritte wie oben beschrieben, bis der Installationsbildschirm in [dieser] [ deployment-guide-figure-5] Abbildung. In diesem Bildschirm wird das Veröffentlichungsdatum Cmdlets sowie die aktuelle Versionsnummer angezeigt. Wenn anders Hinweise [1928533] oder [2015553]angegeben ist, wird empfohlen, mit der neuesten Version von Azure PowerShell-Cmdlets arbeiten.

Die momentan installierte Version der Azure-Cmdlets auf den Desktop oder ein Notebook kann mit dem Befehl PS überprüft werden:

```powershell
Import-Module Azure
(Get-Module Azure).Version
```

Das Ergebnis dargestellt werden soll, wie folgt in [dieser] [ deployment-guide-figure-6] Abbildung.

![Ergebnis der Prüfung der Azure-PS-cmdlet][deployment-guide-figure-600]
<a name="figure-6"></a>

Wenn die aktuelle Azure Cmdlet Version auf dem Desktop/Notebook installiert ist, der erste Bildschirm nach dem Starten der Microsoft-Webplattform-Installer sieht geringfügig im Vergleich zum in [dieser] [ deployment-guide-figure-5] Abbildung.

Beachten Sie den Kreis unten in der [Abbildung] [ deployment-guide-figure-7] unten.
 
![Installationsbildschirm für Azure PowerShell-Cmdlets, dass die aktuellste Version von Azure PS Cmdlets installiert][deployment-guide-figure-700]
<a name="figure-7"></a>

Wenn die Anzeige als [über][deployment-guide-figure-7], dass Azure Cmdlet aktuellste bereits installiert ist, besteht keine Notwendigkeit, um die Installation fortzusetzen. In diesem Fall können Sie "in dieser Phase die Installation beenden'.

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Bereitstellen von Azure CLI
* Gehe zu <https://azure.microsoft.com/downloads/>
* Abschnitt ist "Command-Line Tools Abschnitt"Azure line Interface". Auf den Link Install für Ihr Betriebssystem.

Überprüfen Sie häufig die CLI Azure aktualisiert wurden. In der Regel werden Updates auf einen monatlichen Zeitraum. Die einfachste Möglichkeit hierzu ist die Installation beschriebenen Schritte.

Die momentan installierte Version von Azure CLI auf den Desktop oder ein Notebook kann mit dem Befehl überprüft werden:

```
azure --version
```

Das Ergebnis dargestellt werden soll, wie folgt in [dieser] [ deployment-guide-figure-azure-cli-version] Abbildung.

![Ergebnis der Prüfung der Azure-CLI][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Join VM in der lokalen Domäne (nur Windows)
In Fällen, in SAP VMs standortübergreifende Szenario bereitgestellt, in denen lokale AD und DNS in Azure erweitert, wird erwartet, dass die virtuellen Computer in einer lokalen Domäne angehören. Ausführliche Anleitung an einen virtuellen Computer zu einer lokalen Domäne zusätzliche Software erforderlich ist eine lokale Domäne von Kunden. Normalerweise zu einer lokalen Domäne beitreten einer VM bedeutet zusätzliche Software wie Malware-Schutzsoftware oder verschiedene Agenten des backup oder monitoring installieren.

Darüber hinaus müssen Sie zu Fällen, Proxyeinstellungen für Internet gezwungen sind, beitreten zu einer Domäne hat die lokalen Windows System Account(S-1-5-18) VM Guest auch dazu. Einfachste wird zwingen Sie den Proxy mit Domänengruppenrichtlinie die Systeme in der Domäne angewendet.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Herunterladen, installieren und Aktivieren von Azure VM-Agent
Die folgenden Schritte sind erforderlich, wenn ein virtueller Computer für SAP von Betriebssystem-Images bereitgestellt wird, der nicht z. B. nicht Syspreped für Windows verallgemeinert. Es ist nicht erforderlich, die Agenteninstallation für virtuelle Maschinen von Azure Marketplace bereitgestellt. Diese Bilder enthalten bereits Azure-Agent.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows

* Azure VM-Agenten herunterladen:
    * Azure VM Agent-Installationspaket von herunterladen: <https://go.microsoft.com/fwlink/?LinkId=394789>
    * Speichern Sie VM-Agent MSI-Paket auf dem Laptop oder Server lokal
* Installieren Sie den Agent Azure VM:
    * Der bereitgestellte Azure VM mit Terminal Services (RDP) verbinden
    * Öffnen Sie Windows Explorer-Fenster des virtuellen Computers und ein Zielverzeichnis für die MSI-Datei von der VM-Agent
    * Drag & drop Azure VM Agent Installer MSI-Datei von Ihrem lokalen Laptop-Server in das Zielverzeichnis der VM-Agent auf dem virtuellen Computer
    * Durch Doppelklicken Sie auf die MSI-Datei in der VM
    * Für lokale Domänen VM hinzugefügt, stellen sicher, dass mögliche Internet-Proxyeinstellungen für das Konto Lokales System Windows (S-1-5-18) auf dem virtuellen Computer anwenden sowie im Kapitel [Proxy konfigurieren][deployment-guide-configure-proxy]. VM-Agent in diesem Kontext ausgeführt und muss Azure herstellen können.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Installieren Sie VM-Agent für Linux mit dem folgenden Befehl

- **SLES**

```
sudo zypper install WALinuxAgent
```
- **RHEL**

```
sudo yum install WALinuxAgent
```

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Proxy konfigurieren
Die Schritte zum Konfigurieren des Proxys unterscheiden sich zwischen Windows und Linux.

#### <a name="windows"></a>Windows
Diese Einstellung müssen für das lokale Systemkonto Zugriff auf das Internet gültig sein. Wenn die Proxyeinstellungen nicht durch Gruppenrichtlinien festgelegt sind, können Sie sie für das Konto LocalSystem folgendermaßen konfigurieren konfigurieren.

1.  Öffnen Sie "Gpedit.msc"
1.  Navigieren Sie zu Computerkonfiguration -> Administrative Vorlagen Windows-Komponenten -> -> Internet Explorer und "machen Proxy Settings pro Computer (und nicht pro Benutzer) aktivieren
1.  Die Systemsteuerungsoption, und navigieren Sie zu Netzwerk und Internet -> Internetoptionen
1.  Öffnen Sie die Registerkarte Verbindung, und klicken Sie auf LAN settings
1.  Deaktivieren Sie "automatisch suchen Settings"
1.  Aktivieren Sie "Proxyserver für LAN verwenden", und geben Sie die Proxy-Host und port

#### <a name="linux"></a>Linux
Konfigurieren Sie den korrekten Proxy in der Konfigurationsdatei von Microsoft Azure Gast Agent befindet sich unter /etc/waagent.conf. Die folgenden Parameter müssen festgelegt werden:

```
HttpProxy.Host=<proxy host e.g. proxy.corp.local>
HttpProxy.Port=<port of the proxy host e.g. 80>
```

Starten Sie den Agent nach Änderung die Konfiguration mit

```
sudo service waagent restart
```

Die Proxyeinstellungen in /etc/waagent.conf gelten auch für die erforderliche VM Extensions. Wenn Sie Azure Repositories verwenden möchten, stellen Sie sicher, dass der Datenverkehr zu diesen Repositories nicht lokalen Intranet. Erstellt Benutzer definierten Routen Erzwungene Tunnel aktivieren, müssen Sie eine Route hinzufügen weitergeleitet, die Datenverkehr an Repositories direkt mit dem Internet und nicht über die Standort-zu-Standort-Verbindung.

- **SLES** Außerdem müssen Routen für IP-Adressen im /etc/regionserverclnt.cfg hinzufügen. Ein Beispiel ist in der Abbildung unten dargestellt. 

- **RHEL** Außerdem müssen Routen für die IP-Adressen der Hosts der /etc/yum.repos.d/rhui-load-balancers hinzufügen. Ein Beispiel ist in der Abbildung unten dargestellt.

Weitere Informationen über Benutzer definierten Routen Siehe [dieses][virtual-networks-udr-overview].

![Erzwungene Tunnel][deployment-guide-figure-50]

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurieren von Azure erweiterte Überwachung Erweiterung für SAP
Die VM vorbereitet, Kapitel [Bereitstellung Szenarien der VMs für SAP auf Microsoft Azure] [Deployment-Handbuch-3] Azure VM-Agent wird auf dem Computer installiert. Der nächste Schritt ist Azure erweiterte Überwachung Bereitstellen der Erweiterung für SAP, die in Azure Extension Repository in globalen Rechenzentren von Microsoft Azure. Weitere Informationen überprüfen Sie die Planung und Implementierungshandbuch] [Planung-Handbuch-9.1]. 

Verwenden von Azure PowerShell oder Azure CLI installieren und Konfigurieren der Azure erweiterte Überwachung Erweiterung für SAP. Lesen Sie Kapitel Azure PowerShell [Deployment-Handbuch-4.5.1] Wenn die Erweiterung auf einem Windows- oder Linux-VM auf einem Windows-Computer installieren möchten. Für die Installation der Erweiterung auf einer Linux-VM mit Linux lesen Desktop Kapitel Azure CLI [Deployment Guide 4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell für Linux- und Windows-VMs
So führen die Azure erweiterte Überwachung Erweiterung für SAP installieren:

* Stellen Sie sicher, dass Sie die neueste Version des Microsoft Azure PowerShell-Cmdlets installiert haben. Kapitel [Bereitstellen von Azure PowerShell-Cmdlets] [Deployment Guide-4.1] dieses Dokuments.  
* Führen Sie das folgende PowerShell-Cmdlet. Führen Sie eine Liste der verfügbaren Umgebungen Cmdlet Get-AzureRmEnvironment. Wenn Sie öffentliche Azure verwenden möchten, ist Ihre Umgebung AzureCloud. Azure in China wählen Sie AzureChinaCloud.

```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>
    
    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

Nachdem Sie Ihre Kontodaten und Azure Virtual Machine angegeben, das Skript erforderlichen Extensions bereit und erforderlichen Features aktivieren. Dies kann einige Minuten dauern.
Bitte lesen Sie [MSDN] [ msdn-set-azurermvmaemextension] Weitere Informationen über die Gruppe AzureRmVMAEMExtension.
  
![Ergebnisbild der erfolgreichen Ausführung der SAP bestimmten Azure Cmdlet Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

Eine erfolgreiche Ausführung des Set-AzureRmVMAEMExtension wird alle Überwachungsfunktionen für SAP Host konfiguriert werden. 

Die Ausgabe liefern soll das Skript sieht folgendermaßen aus:

* Bestätigung der VM Überwachungskonfiguration für die Base virtuelle Festplatte (mit Betriebssystem) sowie alle zusätzlichen virtuellen Festplatten bereitgestellt wurde konfiguriert.
* Die nächsten beiden Nachrichten bestätigen die Konfiguration des Storage-Metriken für ein bestimmtes Konto. 
* Eine Zeile der Ausgabe geben den Status auf die tatsächliche Aktualisierung der Konfiguration der Überwachung.
* Ein anderes erscheint bestätigt, dass die Konfiguration bereitgestellt oder aktualisiert wurden.
* Die letzte Zeile der Ausgabe dient testen Sie die Konfiguration die Überwachung angezeigt.
* Überprüfen, dass alle Schritte der Azure erweiterte Überwachung erfolgreich ausgeführt wurde und der Azure-Infrastruktur die erforderlichen Daten bietet, fortsetzen überprüft Bereitschaft Azure erweiterte Überwachung Erweiterung für SAP Kapitel [Readiness überprüfen Azure erweiterte Überwachung für SAP] [Deployment-Handbuch-5.1] in diesem Dokument. 
* Warten Sie weiter damit 15 bis 30 Minuten Azure Diagnostics relevanten Daten haben.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI für Linux-VMs

So führen die Azure erweiterte Überwachung Erweiterung für SAP mit Azure CLI installieren:

1. Azure-CLI installieren, wie in [dieser] [ azure-cli] Artikel
1. Anmeldung mit Ihrem Azure-Konto

    ```
    azure login
    ```
1. Azure-Ressourcen-Manager-Modus wechseln

    ```
    azure config mode arm
    ```
1. Azure erweiterte Überwachung

    ```
    azure vm enable-aem <resource-group-name> <vm-name>
    ```  
1. Überprüfen Sie die Azure erweiterte Überwachung auf Azure Linux VM. Überprüfen Sie, ob die Datei /var/lib/AzureEnhancedMonitor/PerfCounters vorhanden ist. Wenn vorhanden, von AEM mit gesammelten Anzeigeinformationen:

    ```
    cat /var/lib/AzureEnhancedMonitor/PerfCounters
    ```
    Ausgabe erhalten:
    
    ```
    2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
    2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
    …
    …
    ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Überprüfung und Problembehandlung für End-to-End Monitoring Setup für SAP in Azure
Nach der Azure-VM bereitgestellt und Einrichten der entsprechenden Azure Überwachungsinfrastruktur kontrollieren Sie alle Komponenten von Azure erweiterte Überwachung richtig. 

Daher überprüft Bereitschaft der Azure erweiterte Überwachung Erweiterung für SAP ausführen, Kapitel [Readiness überprüfen Azure erweiterte Überwachung für SAP] [Deployment Guide 5.1]. Wenn das Ergebnis dieser Prüfung positiv ist und alle relevanten Leistungsindikatoren, wurde Azure Überwachung erfolgreich. In diesem Fall die Installation der SAP-Agent gemäß Kapitel SAP Ressourcen [Deployment Guide 2.2] dieses Dokuments aufgeführten Hinweise fort. Weist das Ergebnis der Prüfung Readiness fehlende Leistungsindikatoren fortfahren ausführen Health Check für die Azure-Infrastruktur überwachen Kapitel [Health Check für Azure Infrastruktur Überwachungskonfiguration] [Deployment Guide 5.2]. Überprüfen Sie bei Problemen mit Azure Überwachungskonfiguration Kapitel [Weitere Problembehandlung bei Azure Monitoring-Infrastruktur für SAP] [Deployment Guide-5.3] für weitere Hilfe zur Problembehandlung.

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Readiness überprüfen Azure erweiterte Überwachung für SAP
Mit diesem Kontrollkästchen Achten Sie Metriken innerhalb Ihrer SAP-Anwendung angezeigt werden vollständig vom zugrunde liegenden Azure Monitoring-Infrastruktur bereitgestellt werden. 

#### <a name="execute-the-readiness-check-on-a-windows-vm"></a>Readiness-Prüfung auf einer Windows-VM ausführen
Um die Bereitschaft Kontrollkästchen Anmeldung der Azure Virtual Machine (Admin-Konto ist nicht erforderlich) ausführen, und führen Sie die folgenden Schritte aus:

* Öffnen eine Windows-Befehlszeile und ändern in den Installationsordner von Azure Monitoring-Erweiterung für SAP-C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop

Das Version Teil im Pfad oben Überwachung Erweiterung variieren. Wenn mehrere Ordner überwachen Erweiterungsversion im Installationsordner angezeigt wird, überprüfen Sie die Konfiguration der Windows-Dienst 'AzureEnhancedMonitoring' und wechseln Sie zum Ordner "Pfad zur ausführbaren Datei" angegeben.
 
![Eigenschaften der Dienst die Azure erweiterte Überwachung Erweiterung für SAP][deployment-guide-figure-1000]

* Azperflib.exe im Befehlsfenster ohne Parameter ausführen.

> [AZURE.NOTE] Die azperflib.exe in einer Schleife ausgeführt und die gesammelten Leistungsindikatoren alle 60 Sekunden aktualisiert. Um die Schleife zu beenden, schließen Sie das Befehlsfenster.

Wenn Azure erweiterte Überwachung Erweiterung nicht installiert, oder der Dienst 'AzureEnhancedMonitoring' wird nicht ausgeführt, wurde die Erweiterung nicht ordnungsgemäß konfiguriert. In diesem Fall führen Sie Kapitel [Weitere Problembehandlung bei Azure Monitoring-Infrastruktur für SAP] [Deployment Guide-5.3] Weitere Informationen wie die Erweiterung erneut.

##### <a name="check-the-output-of-azperflibexe"></a>Überprüfen Sie die Ausgabe von azperflib.exe
Die Ausgabe von azperflib.exe zeigt alle Azure Leistungsindikatoren für SAP aufgefüllt. Am Ende der Liste der gesammelten Leistungsindikatoren finden Sie eine Zusammenfassung und eine Statusanzeige gibt den Status der Azure an. 
 
![Ausgabe des Health Check durch Ausführen von azperflib.exe, die angibt, dass keine Probleme vorliegen][deployment-guide-figure-1100]
<a name="figure-11"></a>

Überprüfen Sie die Ergebnisse für die Ausgabe des Betrags der Leistungsindikatoren insgesamt zurückgegeben, sind leer und "Health Check" gemeldete oben in der Abbildung [oben][deployment-guide-figure-11].

Sie können die Ergebniswerte wie folgt interpretieren:

| Azperflib.exe Ergebnisse | Überwachen der Bereitschaftsstatus Azure |
| ------------------------------|----------------------------------- |
| **Leistungsindikatoren gesamt: leer** | Die folgenden 2 Azure-Speicher Leistungsindikatoren können leer. <ul><li>Speicher lesen Op Wartezeit Server MS</li><li>Speicher lesen Op Wartezeit E2E MS</li></ul>Alle anderen Indikatoren müssen Werte enthalten. |
| **Health check** | Status OK Wenn Retoure zeigt nur OK |

Wenn nicht beide azperflib.exe zeigen, dass alle gefüllte Leistungsindikatoren ordnungsgemäß zurückgegeben werden zurückgegeben, die Anweisungen des Health Check für den Azure Überwachungskonfiguration Kapitel [Health Check für Azure Infrastruktur Überwachungskonfiguration] [Deployment-Handbuch-5.2] unten.

#### <a name="execute-the-readiness-check-on-a-linux-vm"></a>Bereitschaftsprüfung auf Linux VM ausführen
Um die Bereitschaft Überprüfung ausführen, Verbinden mit SSH Azure Virtual Machine, und führen Sie folgende Schritte aus:

* Überprüfen Sie die Ausgabe von Azure erweiterte Überwachung Erweiterung
    * Weitere /var/lib/AzureEnhancedMonitor/PerfCounters
        * Geben Sie eine Liste der Leistungsindikatoren. Die Datei darf nicht leer sein.
    * Katze /var/lib/AzureEnhancedMonitor/PerfCounters | Grep-Fehler
        * Sollte der Fehler keine z. B. 3 ist eine Zeile, Config zurück. Fehler; 0; 0; **keine**0; 1456416792; Servercs Tst;
    * Weitere /var/lib/AzureEnhancedMonitor/LatestErrorRecord
        * Sollte leer sein oder darf nicht vorhanden sein
* Wenn das erste Kontrollkästchen oben nicht erfolgreich war, führen Sie diese zusätzlichen Tests:
    * Stellen Sie sicher, dass die Waagent installiert und gestartet ist
        * Sudo ls-al/Var/lib/Waagent
            * den Inhalt des Verzeichnisses Waagent sollten aufgeführt werden.
        * PS-Ax | Grep waagent
            * weisen einen Eintrag wie "Python-/usr/sbin/waagent-Daemon"
    * Sicherstellen Sie, dass die Linux-Diagnose-Erweiterung installiert und gestartet ist
        * Sudo sh - C ' ls-al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-* "
            * den Inhalt des Verzeichnisses Linux Diagnose Erweiterung sollten aufgeführt werden.
        * PS-Ax | Grep-Diagnose
            * weisen einen Eintrag wie "Python-/var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py-Daemon"
    * Sicherstellen Sie, dass Azure erweiterte Überwachung Erweiterung installiert und gestartet ist
        * Sudo sh - C ' ls-al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/ "
            * den Inhalt des Verzeichnisses Azure erweiterte Überwachung Erweiterung sollten aufgeführt werden.
        * PS-Ax | Grep AzureEnhanced
            * sollte ein Eintrag wie "Python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py Daemon" anzeigen
* Installieren Sie der SAP-Agent, siehe Hinweis [1031096] und überprüfen Sie die Ausgabe von saposcol
    * Führen Sie /usr/sap/hostctrl/exe/saposcol -d
    * Dump Ccm auszuführen
    * Überprüfen Sie, ob die Metrik "Virtualization_Configuration\Enhanced Monitoring Zugriff" true ist
* Haben Sie bereits einen SAP NetWeaver ABAP Anwendungsserver installiert, öffnen Sie Transaktion ST06 und Kontrollkästchen Erweiterte Überwachung aktiviert ist.

Folgt einer Kontrolle über Fehler, Kapitel [Weitere Problembehandlung bei Azure Monitoring-Infrastruktur für SAP] [Deployment Guide-5.3] Weitere Informationen wie die Erweiterung erneut.

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Health Check für Azure Infrastruktur Überwachungskonfiguration
Wenn die Überwachung Daten ist nicht ordnungsgemäß zugestellt werden durch den Test gemäß Kapitel [Readiness überprüfen Azure erweiterte Überwachung für SAP] [Deployment Guide 5.1], führen Test-AzureRmVMAEMExtension-Cmdlet zu testen, ob die aktuelle Konfiguration der Infrastruktur Azure Überwachung und Monitoring-Erweiterung für SAP korrekt ist.

Um die Konfiguration der Überwachung zu testen, führen Sie folgende Schritte:

* Stellen Sie sicher, dass Sie die neueste Version des Microsoft Azure PowerShell-Cmdlets installiert haben, wie in Kapitel [Bereitstellen von Azure PowerShell-Cmdlets] [Deployment Guide-4.1] dieses Dokuments.
* Führen Sie das folgende PowerShell-Cmdlet. Führen Sie eine Liste der verfügbaren Umgebungen Cmdlet Get-AzureRmEnvironment. Wenn Sie öffentliche Azure verwenden möchten, ist Ihre Umgebung AzureCloud. Azure in China wählen Sie AzureChinaCloud.

```powershell
$env = Get-AzureRmEnvironment -Name <name of the environment>
Login-AzureRmAccount -Environment $env
Set-AzureRmContext -SubscriptionName <subscription name>
Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

* Nachdem Sie Ihre Kontodaten und Azure Virtual Machine angegeben, wird das Skript die Konfiguration des virtuellen Computers testen, die.

 
![Festlegen des SAP bestimmte Azure Cmdlets Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

Nach Eingabe der Informationen zu Ihrem Konto und Azure Virtual Machine Testen des Skripts die Konfiguration des virtuellen Computers gewählte.
 
![Ausgabe der erfolgreichen Test Azure Überwachung Infrastruktur für SAP][deployment-guide-figure-1300]

Stellen Sie sicher, dass jede mit OK gekennzeichnet ist. Wenn einige der Kontrollen nicht ok sind, führen Sie das Cmdlet Update Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment-Handbuch-4,5] dieses Dokuments. Warten Sie 15 Minuten und Überprüfung gemäß Kapitel [Readiness überprüfen Azure erweiterte Überwachung für SAP] [Deployment Guide 5.1] und [Health Check für Azure Infrastruktur Überwachungskonfiguration] [Deployment-Handbuch-5.2] erneut. Die Kontrollen weiterhin ein Problem mit einigen oder allen Leistungsindikatoren angeben, fahren Sie mit Kapitel [Weitere Problembehandlung bei Azure Monitoring-Infrastruktur für SAP] [Deployment Guide 5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Weitere Fehlerbehebung Azure Monitoring-Infrastruktur für SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure-Leistungsindikatoren nicht überhaupt angezeigt
Sammlung von Leistungsindikatoren in Azure erfolgt durch den Windows-Dienst 'AzureEnhancedMonitoring'. Wenn der Dienst nicht richtig installiert wurde oder nicht in der VM ausgeführt wird, können überhaupt keine Performance-Kennzahlen erfasst werden.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Das Installationsverzeichnis der Erweiterung Azure erweiterte Überwachung ist leer 

###### <a name="issue"></a>Problem
Das Installationsverzeichnis C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop ist leer.

###### <a name="solution"></a>Lösung
Die Erweiterung ist nicht installiert. Überprüfen Sie, ob ein Proxy-Problem (wie zuvor beschrieben) ist. Sie müssen den Computer neu starten oder das Skript Skript Set AzureRmVMAEMExtension erneut ausführen

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Service für Azure erweiterte Überwachung ist nicht vorhanden. 

###### <a name="issue"></a>Problem
Windows-Dienst 'AzureEnhancedMonitoring' ist nicht vorhanden. Azperflib.exe: Die azperlib.exe Ausgabe löst einen Fehler aus wie in [Abbildung][deployment-guide-figure-14].
 
![Ausführung von azperflib.exe gibt an, dass der Dienst Azure erweiterte Überwachung Erweiterung für SAP nicht ausgeführt wird][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Lösung
Wenn der Dienst nicht vorhanden ist, wie in [Abbildung][deployment-guide-figure-14], Azure Überwachung Erweiterung für SAP wurde nicht ordnungsgemäß installiert. Die Erweiterung für Ihr Bereitstellungsszenario in Kapitel [Bereitstellung Szenarien der VMs für SAP auf Microsoft Azure] beschriebenen Schritten erneut [Deployment Guide 3]. 

Überprüfen Sie nach der Bereitstellung der Erweiterung, ob Azure Leistungsindikatoren innerhalb der Azure-VM nach 1 Stunde bereitgestellt werden.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Service für Azure erweiterte Überwachung vorhanden, aber nicht gestartet werden 

###### <a name="issue"></a>Problem
Windows-Dienst 'AzureEnhancedMonitoring' vorhanden und aktiviert, aber nicht gestartet. Überprüfen Sie das Anwendungsereignisprotokoll Weitere Informationen.

###### <a name="solution"></a>Lösung
Ungültige Konfiguration. Überwachung Erweiterung für die VM wieder aktiviert, Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment Guide 4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Einige Azure Leistungsindikatoren fehlen
Sammlung von Leistungsindikatoren in Azure erfolgt durch den Windows-Dienst 'AzureEnhancedMonitoring', der Daten aus verschiedenen Quellen abgerufen. Einige Konfigurationsdaten werden lokal gesammelt und Leistungsdaten von Azure Diagnostics lesen Speicher Leistungsindikatoren aus der Anmeldung Abonnementstufe Speicher verwendet.

Wenn Problembehandlung mit Hinweis [1999351] nicht helfen, führen das Skript AzureRmVMAEMExtension Gruppe erneut. Sie müssen Stunde warten, da Storage Analytics oder Diagnose Leistungsindikatoren möglicherweise nicht erstellt wird, sobald sie aktiviert sind. Wenn das Problem weiterhin besteht, öffnen Sie eine Nachricht SAP Kunden Support für die Komponente BC-OP-NT-AZR.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure-Leistungsindikatoren nicht überhaupt angezeigt

Sammlung von Leistungsindikatoren in Azure erfolgt durch ein Daemon. Wenn der Daemon nicht ausgeführt wird, können überhaupt keine Performance-Kennzahlen erfasst werden.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Das Installationsverzeichnis der Erweiterung Azure erweiterte Überwachung ist leer 

###### <a name="issue"></a>Problem
Das Verzeichnis/Var/Lib/Waagent/enthält ein Unterverzeichnis für die Erweiterung Azure erweiterte Überwachung.

###### <a name="solution"></a>Lösung
Die Erweiterung ist nicht installiert. Überprüfen Sie, ob ein Proxy-Problem (wie zuvor beschrieben) ist. Sie müssen den Computer neu starten und erneut ausführen des Konfigurationsskripts Set AzureRmVMAEMExtension

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Einige Azure Leistungsindikatoren fehlen

Sammlung von Leistungsindikatoren in Azure erfolgt durch ein Daemon, der Daten aus verschiedenen Quellen abgerufen. Einige Konfigurationsdaten werden lokal gesammelt und Leistungsdaten von Azure Diagnostics lesen Speicher Leistungsindikatoren aus der Anmeldung Abonnementstufe Speicher verwendet.

Eine vollständige und aktuelle Liste bekannter Probleme finden Sie SAP-Hinweis [1999351] enthält weitere Informationen zur Problembehandlung für erweiterte Azure Überwachung für SAP.

Behandlung von Problemen mit SAP-Hinweis [1999351] nicht helfen, führen Sie erneut das Konfigurationsskript Set AzureRmVMAEMExtension Kapitel [konfigurieren Azure erweiterte Überwachung Erweiterung für SAP] [Deployment Guide 4.5]. Sie müssen Stunde warten, da Storage Analytics oder Diagnose Leistungsindikatoren möglicherweise nicht erstellt wird, sobald sie aktiviert sind. Wenn das Problem weiterhin besteht, öffnen Sie eine SAP Kunden Support Nachricht Komponente BC-OP-NT-AZR für Windows oder BC-OP-LNX-AZR für einen virtuellen Linux-Maschine.
