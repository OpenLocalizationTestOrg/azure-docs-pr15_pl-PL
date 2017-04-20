<properties
   pageTitle="SAP NetWeaver na Linux wirtualnych maszyn — Podręcznik wdrażania bazami danych | Microsoft Azure"
   description="SAP NetWeaver na Linux wirtualnych maszyn — Podręcznik wdrażania bazami danych"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP NetWeaver na Azure wirtualnych maszyn — Podręcznik wdrażania bazami danych

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

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver na Linux wirtualnych maszyn — Podręcznik wdrażania bazami danych) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (buforowanie maszyny wirtualne i wirtualnych dysków twardych) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (oprogramowania RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (magazyn Microsoft Azure) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (struktura wdrożenia RDBMS) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (wysokiej dostępności i odzyskiwanie z maszyny wirtualne Azure) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (program SQL Server 2012 z dodatkiem SP1 CU4 lub w nowszej wersji) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (program SQL Server 2012 z dodatkiem SP1 CU3 i starszych wersjach) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (za pomocą obrazów programu SQL Server poza Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (ogólne programu SQL Server dla SAP na podsumowanie Azure) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (szczegóły do programu SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Konfiguracja miejsca do magazynowania) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (kopia zapasowa i przywracanie) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (zagadnienia dotyczące wydajności kopii zapasowych i przywracanie) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (inne) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver na Linux wirtualnych maszyn — Podręcznik wdrażania) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP zasobów) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (wdrażanie maszyn wirtualnych z obraz niestandardowy) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (scenariusz 1: Wdrażanie maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace dla SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenariusz 2: Wdrażanie maszyn wirtualnych z obraz niestandardowy dla SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 ( Scenariusz 3: Przenoszenie maszyn wirtualnych ze źródeł lokalnych uogólniony wirtualnego dysku twardego Azure za pomocą SAP) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (wdrożenia scenariuszy z maszyny wirtualne dla systemu SAP w programie Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (wdrażanie Azure poleceń cmdlet) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (plik do pobrania i SAP w celu importowania odpowiednich poleceń cmdlet programu PowerShell) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (dołączanie maszyn wirtualnych do domeny lokalnej — tylko w systemie Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [wdrożenia — przewodnik 4.4]: Virtual-Machines-Linux-SAP-Deployment-Guide.MD#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (pobieranie, instalowanie i Włącz agenta maszyn wirtualnych Azure) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure programu PowerShell) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (polecenie Azure) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurowanie Azure rozszerzony monitorowania rozszerzenie dla SAP) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (gotowość do wyszukania Azure rozszerzony monitorowania SAP) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (sprawdzanie kondycji dla Azure Monitorowanie Konfiguracja infrastruktury) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (dodatkowo monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver na Linux wirtualnych maszyn — planowanie i przewodnik po implementacji) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (zasobów) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (wysoka dostępność (HA) i odzyskiwanie danych (DR) dla SAP NetWeaver uruchomione na maszyn wirtualnych Azure) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (wysoką dostępność dla serwerów aplikacji SAP) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (przy użyciu Autostart wystąpień SAP) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (tylko do chmury - wdrożeń maszyn wirtualnych do Azure bez zależności w sieci lokalnej klienta) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (między lokalna – wdrożenia jednego lub wielu SAP maszyny wirtualne do Azure z wymaganiami jest w pełni zintegrowany z siecią lokalnego) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regiony) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (uaktualnianie domen) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088-(błędów domen) f9be - 4c 97-958a - 27996255c 665 (zestawach dostępność Azure) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (koncepcja Microsoft maszyn wirtualnych Azure) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (magazynowanie Premium Azure) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (przenoszenie maszyn wirtualnych ze źródeł lokalnych Azure za pomocą dysku uogólniony) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (wdrażanie maszyn wirtualnych z określonego obrazu klienta) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (przygotowanie do przenoszenia maszyn wirtualnych ze źródeł lokalnych Azure za pomocą dysku uogólniony) [ Planning-Guide-5.2.2]:Virtual-Machines-Linux-SAP-Planning-Guide.MD#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (przygotowanie do wdrażania maszyn wirtualnych z określonego obrazu klienta dla SAP) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (przygotowywanie maszyny wirtualne z SAP dla Azure) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (różnicę między Azure dysku i obraz Azure) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (przekazywanie wirtualnego dysku twardego ze źródeł lokalnych Azure) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopiowanie dyski między kontami miejsca do magazynowania Azure) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (maszyn wirtualnych-wirtualnego dysku twardego struktura wdrożeniach systemu SAP) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (ustawienie automatycznej instalacji na dyskach dołączony) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (pojedynczy maszyn wirtualnych z programem SAP NetWeaver pokaz szkolenie scenariusz) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (pojęcia Cloud-Only wdrożenie wystąpień SAP) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure monitorowania rozwiązaniem SAP) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (magazynowanie Premium Azure)

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
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
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
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
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
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.

Ten przewodnik jest częścią dokumentację dotyczącą wykonania i wdrażanie oprogramowania SAP w programie Microsoft Azure. Przed czytania tego przewodnika, przeczytaj [planowania i przewodnik po implementacji] [-przewodnik po planowaniu pod]. W tym dokumencie opisano wdrażania różnych relacyjne bazy danych zarządzania systemów (RDBMS) i powiązanych produktów w połączeniu z SAP w programie Microsoft Azure wirtualnych maszyn przy użyciu infrastruktury Azure jako możliwości usług (IaaS).

Papier uzupełnia dokumentacji instalacji SAP i notatki SAP, reprezentujące podstawowego zasoby dotyczące instalacji i wdrożenia oprogramowania SAP na określonych platformach

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Ogólne
W tym rozdziału zagadnienia uruchomienia SAP związane z systemów bazami danych w maszyny wirtualne Azure są wprowadzane. Istnieje kilka odwołania do określonych systemów bazami danych, w tym rozdziałów. Zamiast tego do określonych systemów bazami danych są obsługiwane w ramach tego dokumentu po tym rozdziałów.

### <a name="definitions-upfront"></a>Definicje ustalonymi
W całym dokumencie Stosujemy następujące warunki:

* IaaS: Infrastruktura jako usługa.
* PaaS: Platforma jako usługa.
* Władz akredytacji bezpieczeństwa: Oprogramowanie jako usługa.
* Składnik SAP: poszczególnych SAP aplikacji, takiej jak ECC, PC, Menedżer rozwiązanie lub EP.  Składniki SAP mogą być oparte na tradycyjnych technologii ABAP lub Java lub aplikacji innych niż NetWeaver podstawie, takich jak obiektów biznesowych.
* Środowisko SAP: jeden lub więcej składników SAP logicznie grupowane realizację funkcji biznesowej, takich jak rozwoju, QAS, szkolenia, DR lub produkcji.
* Pozioma SAP: Odnosi się do całej SAP środkami klienta IT pozioma. Pozioma SAP zawiera wszystkie produkcji i środowisku produkcyjnym nie.
* SAP System: Kombinacja bazami danych i warstwy aplikacji np systemu SAP ERP dla deweloperów Przepustowości SAP testowymi, systemu SAP CRM produkcji, itp. We wdrożeniach Azure go nie jest obsługiwane podziału tych dwóch warstw między wdrożeniem lokalnym a Azure. Oznacza to, którego systemu SAP albo wdrożyć lokalnego lub zostanie wdrożony w Azure. Jednak można wdrażać systemów SAP pozioma Azure lub lokalnego. Na przykład możesz wdrożyć rozwoju SAP CRM i przetestować systemów w Azure, ale SAP CRM produkcji systemu lokalnego.
* Wdrażanie tylko do chmury: miejsce, w którym Azure subskrypcja nie jest połączony za pośrednictwem witryny do witryny lub połączenie ExpressRoute infrastruktury sieciowej lokalnego wdrożenia. Wspólne Azure dokumentacji tego rodzaju wdrożeń opisano także jako "Tylko do chmury" wdrożenia. Maszyn wirtualnych wdrożony w ten sposób są dostępne za pośrednictwem Internetu i publicznej internetowe punkty końcowe przypisane do maszyny wirtualne platformy Azure. Lokalnej usługi Active Directory (AD) i DNS nie jest używane w Azure w tego typu wdrożenia. W związku z tym maszyny wirtualne nie są uwzględniane w lokalnej usługi Active Directory. Uwaga: Tylko do chmury wdrożeń w tym dokumencie są określane jako ukończone krajobrazów SAP, które są uruchomione wyłącznie w Azure bez rozszerzenia usługi Active Directory lub rozpoznawanie nazw ze źródeł lokalnych w chmurze publicznej. Tylko do chmury konfiguracje nie są obsługiwane dla systemów SAP produkcji lub miejsce, w którym SAP STMS lub innych zasobów lokalnych trzeba używać między systemów SAP hostowana w Azure i zasobów znajdujących się w lokalnej konfiguracji.
* Lokalne krzyżowe: Opisuje scenariusz miejsce, w którym maszyny wirtualne są rozmieszczane subskrypcji usługi Azure witryny do witryny, wiele witryn lub ExpressRoute łączność między datacenter(s) lokalnego i Azure. Wspólne Azure dokumentacji, tego rodzaju wdrożeń opisano także jako scenariusze krzyżowe lokalnej. Powód połączenia ma rozszerzenie domeny lokalnej usługi Active Directory w lokalnej i lokalnego DNS do Azure. Pozioma lokalnego jest używane Azure składniki majątku subskrypcji. Wystąpienia tego numeru wewnętrznego, maszyny wirtualne może być częścią domeny lokalnej. Użytkownicy domeny domeny lokalnej mają dostęp do serwera i można uruchamiać usług na tych maszyny wirtualne (na przykład bazami danych usług). Komunikacji i rozpoznawania nazw między maszyny wirtualne wdrożony w lokalnym i maszyny wirtualne wdrożony w Azure jest możliwe. Oczekuje się najbardziej typowy scenariusz wdrażania aktywów SAP Azure. Zobacz [ten] [ vpn-gateway-cross-premises-options] artykuł i [tym] [ vpn-gateway-site-to-site-create] uzyskać więcej informacji.

> [AZURE.NOTE] Wdrożenia lokalne krzyżowe systemów SAP miejsce, w którym maszyn wirtualnych Azure z systemami SAP są członkami domeny lokalnej są obsługiwane dla systemów SAP produkcji. Konfiguracje lokalnej krzyżowe są obsługiwane w przypadku wdrażania części lub wykonywanie krajobrazów SAP do Azure. Nawet uruchomionych pełny pozioma SAP w Azure wymaga o tych maszyny wirtualne będące częścią domeny lokalnej i REKLAM. W poprzednich wersjach dokumentacji zajmowaliśmy scenariuszy hybrydowego IT, gdzie terminów "Hybrydowe" jest umieszczone w fakt, że jest łączność między lokalnej lokalnego i Azure. W tym przypadku "Hybrydowych" również oznacza, że maszyny wirtualne platformy Azure są częścią usługi Active Directory w lokalnej.

Niektóre dokumentacji firmy Microsoft opisano scenariusze lokalnej krzyżowe nieco inaczej, zwłaszcza w przypadku konfiguracji HA bazami danych. W przypadku systemu SAP powiązanych dokumentów, scenariusz lokalnej krzyżowe tylko wrzenia w dół o witryny do witryny lub prywatnych łączności (ExpressRoute) i fakt, że pozioma SAP jest rozdzielona między wdrożeniem lokalnym a Azure.

### <a name="resources"></a>Zasoby
Dla tematu wdrożeniach systemu SAP Azure dostępne są następujące przewodniki:

* [SAP NetWeaver na Azure wirtualnych maszyn — planowanie i przewodnik po implementacji] [-przewodnik po planowaniu pod]
* [SAP NetWeaver na Azure wirtualnych maszyn — Podręcznik wdrażania] [Podręcznik wdrażania]
* [SAP NetWeaver na Azure wirtualnych maszyn — Podręcznik wdrażania bazami danych (ten dokument)] [bazami danych — przewodnik]

Poniższe uwagi SAP są związane z tym tematem SAP Azure:

| Numer przypisu   | Tytuł
|------------|--------
| [1928533] | Aplikacji SAP Azure: typy obsługiwanych produktów i maszyn wirtualnych Azure
| [2015553] | SAP w programie Microsoft Azure: obsługuje wymagania wstępne
| [1999351] | Rozwiązywanie problemów z rozszerzonym Azure monitorowania SAP
| [2178632] | Klucz monitorowania metryki SAP w programie Microsoft Azure
| [1409604] | Wirtualizacji w systemie Windows: rozszerzony monitorowania
| [2191498] | SAP w systemie Linux z Azure: rozszerzony monitorowania
| [2039619] | Aplikacje SAP w systemie Microsoft Azure za pomocą bazy danych Oracle: obsługiwane produkty i wersji
| [2233094] | DB6: Aplikacji SAP Azure Linux, UNIX i systemu Windows — dodatkowych informacji za pomocą programu IBM DB2
| [2243692] | Linux Microsoft Azure (IaaS) maszyn wirtualnych: problemy z licencją SAP
| [1984787] | SUSE LINUX Enterprise Server 12: Uwagi dotyczące instalacji
| [2002167] | Czerwone funkcję Enterprise Linux 7.x: Instalowanie i uaktualnianie

Przeczytaj również [SCN typu Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , zawierającego wszystkie notatki SAP Linux.

Powinien pojawić się wiedzą na temat architektury Microsoft Azure i jak wdrożyć i obsługiwanej maszyn wirtualnych usługi Microsoft Azure. Więcej informacji można znaleźć tutaj <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Firma Microsoft **nie** są omawiane platformy Microsoft Azure jako oferty usług (PaaS) platformy Microsoft Azure. W tym dokumencie jest o uruchamianiu systemu zarządzania bazy danych (bazami danych) w programie Microsoft Azure maszyn wirtualnych (IaaS) tak, jak należy uruchomić bazami danych w środowisku lokalnym. Funkcje bazy danych i funkcji między te dwie oferty są bardzo różne i nie może być mieszane ze sobą. Zobacz też: <https://azure.microsoft.com/services/sql-database/>

Ponieważ firma Microsoft są omawiane IaaS, ogólnie systemu Windows i Linux oraz bazami danych instalacja i konfiguracja są zasadniczo taki sam, jak ani maszyn wirtualnych systemu od zera metalu komputerze można zainstalować lokalnego. Istnieją jednak pewne architektura i systemu zarządzania wykonania decyzji, które będą inne, gdy wykorzystanie IaaS. Celem tego dokumentu jest specyficzna dla architektury i systemu zarządzania różnice, które możesz należy przygotować podczas korzystania z IaaS.

Zwykle ogólna obszarów różnicą, że zostaną omówione w tym dokumencie są:

* Planowanie pisane z wielkiej litery układu maszyn wirtualnych-wirtualnego dysku twardego systemów SAP, aby upewnić się, że masz odpowiednie dane pliku układu i można uzyskać za mało operacji i/o na SEKUNDĘ dla z pracą.
* Zagadnienia sieci podczas korzystania z IaaS.
* Funkcje bazy danych używać w celu zoptymalizowania układu bazy danych.
* Kopia zapasowa i przywracanie uwagi w IaaS.
* Korzystanie z różnych typów obrazów na potrzeby wdrożenia.
* Wysoka dostępność Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktura wdrożenia RDBMS
W kolejności wykonaj sygnalizować, jest to konieczne dowiedzieć się, co przedstawiono [] [wdrożenia przewodnik-3] sygnalizować [Podręcznik wdrażania] [wdrożenia przewodnika]. Wiedza na temat różnych serii maszyn wirtualnych i ich różnice i różnic Azure standardowy i miejsca do magazynowania Premium należy rozumieć i znane przed przeczytaniem tego rozdziału.

Poczekaj, aż marca 2015 r Azure wirtualnych dysków twardych, które zawierają system operacyjny były ograniczone do 127 GB w polu rozmiar. To ograniczenie masz zniesione w marca 2015 r (Aby uzyskać więcej informacji o wyboru <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). Stamtąd na wirtualnych dysków twardych zawierająca system operacyjny może mieć ten sam rozmiar co inne wirtualnego dysku twardego. Jednak firma Microsoft nadal woli struktury wdrożenia miejsce, w którym system operacyjny, bazami danych i ewentualnego SAP plików binarnych są oddzielone od plików bazy danych. W związku z tym oczekuje się, że systemów SAP uruchomione w maszyn wirtualnych Azure będą mieć podstawowej maszyn wirtualnych (lub wirtualnego dysku twardego) zainstalowany w systemie operacyjnym, bazy danych zarządzania system plików wykonywalnych i pliki wykonywalne SAP. Pliki dziennika i danych bazami danych są przechowywane w magazynie Azure (standardowy lub miejsca do magazynowania Premium) w oddzielnym pliku wirtualnego dysku twardego i dołączone jako dyski logiczne do oryginalnego obrazu Azure system operacyjny maszyn wirtualnych. 

W zależności od używanie Azure standardowy lub miejsce do magazynowania Premium (np. za pomocą serii Zasadami lub serii GS maszyny wirtualne) są inne przydziałów Azure, opisanych [tutaj][virtual-machines-sizes]. Podczas planowania usługi Azure wirtualnych dysków twardych, należy znaleźć najlepsze saldo przydziałów dla następujących elementów:

* Liczba plików danych.
* Liczba wirtualnych dysków twardych, które zawierają pliki.
* Kwot operacji i/o na SEKUNDĘ jednego wirtualnego dysku twardego.
* Przepustowość na wirtualnego dysku twardego.
* Liczba dodatkowych możliwości wirtualnych dysków twardych na rozmiar pamięci Wirtualnej.
* Ogólna przepustowość miejsca do magazynowania umożliwiają maszyny.
 
Azure będzie wymuszać przydziału operacji i/o na SEKUNDĘ na dysku wirtualnego dysku twardego. Tych przydziałów różnią się w wirtualnych dysków twardych na standardowy magazyn Azure i Premium miejsca do magazynowania. Opóźnienia we/wy będzie również znacznie różni się między tymi dwoma typami miejsca do magazynowania z miejsca do magazynowania Premium przedstawiania czynników wpływających na lepsze opóźnienia we/wy. Każdy z różnych typów maszyn wirtualnych mieć ograniczoną liczbą wirtualnych dysków twardych, które mogą dołączyć. Ograniczenie innego jest, że tylko niektórych typów maszyn wirtualnych można wykorzystać magazyn Premium Azure. Oznacza to, że decyzja dla określonego typu maszyn wirtualnych może nie tylko być prowadzone przez wymagania Procesora i pamięci, ale także przez operacji i/o na SEKUNDĘ, opóźnienie i dyskiem wymagania przepustowość, które zwykle skalowania liczbę wirtualnych dysków twardych lub rodzaj dysków w magazynie Premium. Zwłaszcza z nośnikami Premium rozmiar wirtualnego dysku twardego również może być ustawieniem liczbę operacji i/o na SEKUNDĘ i przepustowość powinien być osiągnięte przez każdego wirtualnego dysku twardego.

Fakt, że ogólnego wskaźnika operacji i/o na SEKUNDĘ liczba wirtualnych dysków twardych zainstalowany i rozmiar maszyn wirtualnych jest wszystkich powiązanych ze sobą, może powodować Azure konfiguracji systemu SAP jest inny niż jego wdrożenia lokalnego. Limity operacji i/o na SEKUNDĘ na LUN są zazwyczaj konfigurowane we wdrożeniach lokalnego. Z magazynu Azure te limity są stałe lub jak Premium miejsca do magazynowania w zależności od typu dysku. Dlatego z lokalnego wdrożenia widzimy konfiguracji klienta serwerów bazy danych, które używają wielu różnych wielkości dla specjalnych plików wykonywalnych, takich jak SAP i bazami danych lub specjalne wielkości tymczasowej bazy danych lub tabel. Po przeniesieniu system lokalnego Azure może prowadzić do niepotrzebnego potencjalne przepustowości operacji i/o na SEKUNDĘ przez marnowania wirtualnego dysku twardego dla plików wykonywalnych lub baz danych, których nie należy wykonywać dowolne albo nie wiele operacji i/o na SEKUNDĘ. Dlatego w maszyny wirtualne Azure zaleca się pliki wykonywalne bazami danych i SAP w celu zainstalowania na dysku systemu operacyjnego, jeśli to możliwe.

Rozmieszczenie plików bazy danych i pliki dziennika i typ Azure zajęte, należy zdefiniować operacji i/o na SEKUNDĘ, opóźnienia i wymagania dotyczące przepustowości. Aby masz za mało operacji i/o na SEKUNDĘ dla dziennika transakcji, może być wymuszone korzystać z wielu wirtualnych dysków twardych pliku dziennika transakcji lub użyj większy dysk Premium przestrzeni dyskowej, aby. W takim przypadku jedną po prostu chcesz utworzyć oprogramowania RAID (np. Windows puli miejsca do magazynowania dla Windows lub MDADM i LVM (logiczne Volume Manager) Linux) z wirtualnych dysków twardych, które będą zawierać dziennik transakcji.

___

> ![Systemu Windows][Logo_Windows] Systemu Windows
>
> D:\ w maszyn wirtualnych Azure jest nietrwałe dysk, którym jest przechowywana w niektórych lokalnych dyskach w węźle Azure obliczeń. Ponieważ jest nietrwałe, oznacza to, że wszelkie zmiany wprowadzone w zawartości na dysku D:\ są tracone po uruchomieniu maszyn wirtualnych. "Wszelkie zmiany" Firma Microsoft oznacza zapisane pliki, katalogi utworzone, zainstalowane aplikacje itp.
>
> ![Linux][Logo_Linux] Linux
>
> Maszyny wirtualne Azure Linux automatycznie zainstalować dysk w /mnt/resource, czyli dysk nietrwałe kopii przez lokalnych dyskach w węźle Azure obliczeń. Ponieważ jest nietrwałe, oznacza to, że wszelkie zmiany wprowadzone do zawartości w /mnt/resource są tracone po uruchomieniu maszyn wirtualnych. Za wszelkie zmiany firma Microsoft oznacza pliki zapisane, katalogów tworzonych, zainstalowane aplikacje itp.

___

Zależne od Azure serii maszyn wirtualnych, lokalnych dyskach w węźle obliczeń Pokaż różne działania, które można podzielić, takich jak:

* A0-A7: Bardzo ograniczoną wydajność. Nie można używać do wszelkich działań poza pliku strony systemu windows
* A8 A11: Właściwości bardzo dobrej wydajności operacji i/o na SEKUNDĘ niektórych dziesięć tysięcy i > przepustowości 1GB/s
* Serii: Właściwości bardzo dobrej wydajności operacji i/o na SEKUNDĘ niektórych dziesięć tysięcy i > przepustowości 1GB/s
* Seria Zasadami: Właściwości bardzo dobrej wydajności operacji i/o na SEKUNDĘ niektórych dziesięć tysięcy i > przepustowości 1GB/s
* Seria G: Właściwości bardzo dobrej wydajności operacji i/o na SEKUNDĘ niektórych dziesięć tysięcy i > przepustowości 1GB/s
* Serii GS: Właściwości bardzo dobrej wydajności operacji i/o na SEKUNDĘ niektórych dziesięć tysięcy i > przepustowości 1GB/s

Powyższe instrukcje są zastosowanie typy maszyn wirtualnych, które są autoryzowane jako zgodne z SAP. Serii maszyn wirtualnych z doskonałym operacji i/o na SEKUNDĘ i przepustowość kwalifikuje się do pobudzenia przy niektórych funkcji bazami danych, takich jak tymczasowe lub tymczasowy obszar tabel.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Buforowanie maszyny wirtualne i wirtualnych dysków twardych
Tworząc tych dysków-wirtualnych dysków twardych za pośrednictwem portalu lub możemy zainstalować przekazane wirtualnych dysków twardych do maszyny wirtualne, firma Microsoft można wybrać, czy Wy między maszyn wirtualnych i tych wirtualnych dysków twardych znajduje się w magazynie Azure w pamięci podręcznej. Azure Standard i Premium miejsca do magazynowania dla tego typu pamięci podręcznej za pomocą dwóch różnych technologii. W obu przypadkach pamięci podręcznej sam może być dysku kopii na tym samym dyskach używane przez dysku tymczasowym (D:\ w systemie Windows) lub /mnt/resource w systemie Linux maszyn wirtualnych.
 
Magazyn standardowy Azure typy możliwych pamięci podręcznej są:

* Nie pamięci podręcznej
* Przeczytaj pamięci podręcznej
* Odczytywanie i zapisywanie w pamięci podręcznej

Aby można było uzyskiwać spójne i deterministyczne wydajności, należy ustawić buforowanie ilość miejsca do magazynowania standardowy Azure dla wszystkich wirtualnych dysków twardych zawierające **bazami danych zawartości związanej z plików danych, pliki dziennika i miejsce tabeli, aby "Brak"**. Buforowanie maszyn wirtualnych może pozostać z domyślnymi.

Do przechowywania Premium Azure istnieją następujące opcje buforowania:

* Nie pamięci podręcznej
* Przeczytaj pamięci podręcznej

Zalecenia dotyczące miejsca do magazynowania Premium Azure jest je wykorzystać **więcej pamięci podręcznej dla plików danych** bazy danych, SAP i wybrać **nie buforowanie VHD(s) plików dziennika**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Oprogramowanie RAID
Jak już wspomniano, będzie konieczne saldo liczbę operacji i/o na SEKUNDĘ wymagany dla plików bazy danych przez liczbę wirtualnych dysków twardych można skonfigurować, a następnie wpisz maksymalną operacji i/o na SEKUNDĘ maszyn wirtualnych Azure zapewni na dysku wirtualnego dysku twardego lub miejsce do magazynowania Premium. Najprostszym sposobem na zajęcie się obciążenia operacji i/o na SEKUNDĘ przez wirtualnych dysków twardych jest tworzenie oprogramowania RAID na różnych wirtualnych dysków twardych. Następnie umieść liczbę plików danych programu SAP bazami danych na jednostek Logicznych, carved poza oprogramowania RAID. Zależne od wymagania, że warto rozważyć użycie pamięci Premium także od dwóch z trzech różnych dyski magazynu Premium zapewniają wyższą przydziału operacji i/o na SEKUNDĘ niż wirtualnych dysków twardych według standardowego magazynu. Oprócz znaczną lepiej opóźnienie we/wy dostarczony przez Magazyn Premium Azure. 

Taki sam dotyczy dziennik transakcji różnych systemów bazami danych. Z dużą ilością ich tylko dodawanie więcej plików Tlog nie pomocy, ponieważ systemy bazami danych zapisać w jednym z plików tylko w czasie. Jeśli potrzebne są operacji i/o na SEKUNDĘ większe niż jeden Standard, który zależności magazynu zapewnia wirtualny dysk twardy, można paskowych przez wielu standardowy wirtualnych dysków twardych z miejsca do magazynowania lub można użyć większe typ dysków Premium miejsca do magazynowania, który poza większe operacji i/o na SEKUNDĘ również zapewnia czynniki mniejsze opóźnienia do zapisu operacji dotyczących w dzienniku transakcji.
 
Sytuacje wystąpił we wdrożeniach Azure, które chcesz preferować w pełni, używając oprogramowania są RAID:

* Dziennik transakcji dziennika/ponowne wykonywanie wymagają operacji więcej i/o na SEKUNDĘ niż Azure przewiduje się pojedynczego wirtualnego dysku twardego. Jak wcześniej wspomniano to można rozwiązać, tworząc LUN przez wielu wirtualnych dysków twardych, używając oprogramowania RAID.
* Nierówna we/wy rozkład obciążenia nad plikami dane różnych SAP bazy danych. W takich przypadkach jedną możliwości naciśnięcie przydziału wolisz często jeden plik danych. Dlatego Zbliżasz przydział operacji i/o na SEKUNDĘ jednego wirtualnego dysku twardego nie są jeszcze się inne pliki danych. W takich przypadkach Najprostszym rozwiązaniem jest tworzenie jednej LUN przez wielu wirtualnych dysków twardych, używając oprogramowania RAID. 
* Nie wiesz, co dokładnie obciążenie We/Wy na plik danych jest i tylko około wie, co jest ogólny obciążenia operacji i/o na SEKUNDĘ przed bazami danych. Najłatwiej wykonać jest utworzenie jednego LUN za pomocą oprogramowania RAID. Suma kwot wielu wirtualnych dysków twardych za tym LUN następnie należy spełnić znane stopa operacji i/o na SEKUNDĘ.

___

> ![Systemu Windows][Logo_Windows] Systemu Windows
>
> Lepiej jest zastosowania systemu Windows Server 2012 lub nowszy spacje miejsca do magazynowania, ponieważ jest to bardziej efektywne niż rozkładanie Windows starszych wersji systemu Windows. Należy pamiętać, że może być konieczne tworzenie puli miejsca do magazynowania systemu Windows i spacje miejsca do magazynowania za polecenia programu PowerShell, korzystając z systemu Windows Server 2012 jako System operacyjny. Polecenia programu PowerShell można znaleźć tutaj <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Do tworzenia oprogramowania RAID w systemie Linux obsługiwane są tylko MDADM i LVM (logiczne Volume Manager). Aby uzyskać więcej informacji przeczytaj następujące artykuły:
>
> * [Konfigurowanie oprogramowania RAID w systemie Linux] [ virtual-machines-linux-configure-raid] (w przypadku MDADM)
> * [Konfigurowanie LVM na maszyny Linux platformy Azure][virtual-machines-linux-configure-lvm]


___

Zagadnienia dotyczące Używanie serii maszyn wirtualnych, które są w stanie do pracy z magazyn Premium Azure zazwyczaj są następujące:

* Wymagania dla opóźnienia we/wy, znajdujące się blisko dostarczanie urządzeń SAN i NAS.
* Żądanie czynników wpływających na lepsze opóźnienie we/wy niż zapewnia Azure standardowego magazynu.
* Wyższa operacji i/o na SEKUNDĘ na maszyn wirtualnych niż może być osiągnięte z wielu standardowy miejsca do magazynowania wirtualnych dysków twardych przed określonego typu maszyn wirtualnych.

Ponieważ źródłowych magazyn Azure replikuje każdego wirtualnego dysku twardego węzłów magazynu co najmniej trzy proste RAID 0 rozkładanie mogą być używane. Istnieje potrzeba wykonania RAID5 lub RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Magazyn Microsoft Azure
Magazyn Microsoft Azure będzie przechowywana podstawowej maszyn wirtualnych (z systemem operacyjnym) i wirtualnych dysków twardych lub obiektów blob do co najmniej 3 węzłów oddzielnie. Podczas tworzenia konta miejsca do magazynowania, jest wybór ochrony jak pokazano poniżej:

![Replikacja Geo włączona dla konta magazynu platformy Azure][dbms-guide-figure-100]

Azure (lokalnie zbędne) replikacji lokalnego magazynu zawiera poziom ochrony przed utratą danych ze względu na błąd infrastruktury, który klientom kilku może zapewnić do wdrożenia. Jak pokazano powyżej dostępne są różne opcje 4 z piątego są odmiany jednego z pierwsze trzy. Szukasz dokładniejsze ich można wyróżnia się:

* **Premium lokalnie zbędne przestrzeni dyskowej (LRS)**: Magazyn Premium Azure udostępnia obsługę dysku wysokiej wydajności i małych opóźnień maszyn wirtualnych z systemem obciążenia I-O-intensywną. Istnieją 3 repliki danych w tym samym Azure centrum danych Azure regionu. Kopie będą w różnych błędów i uaktualnianie domen (dla pojęcia Zobacz [to] [planowania — przewodnik 3,2] rozdziału [planowania Guide][planning-guide]). W przypadku replice danych wychodzących z usługi Magazyn węzła lub awarii dysku nowego replice jest generowany automatycznie.
* **Lokalnie zbędne przestrzeni dyskowej (LRS)**: W tym przypadku istnieją 3 repliki danych w tym samym Azure centrum danych Azure regionu. Kopie będą w różnych błędów i uaktualnianie domen (dla pojęcia Zobacz [to] [planowania — przewodnik 3,2] rozdziału [planowania Guide][planning-guide]). W przypadku replice danych wychodzących z usługi Magazyn węzła lub awarii dysku nowego replice jest generowany automatycznie. 
* **Geo zbędne przestrzeni dyskowej (GRS)**: W tym przypadku jest asynchroniczne replikacji, tak aby będzie kanału informacyjnego dodatkowe 3 repliki danych w innym regionie Azure, która znajduje się w większości przypadków tego samego regionu geograficznego (na przykład Europa Północna i zachód Europe). Spowoduje to 3 dodatkowych replik, tak, aby w sumie znajdują się repliki 6. Odmiany tego jest dodatkiem miejsce, w którym można używać danych w regionie Azure zreplikowanej geo celach odczytu (odczytu Geo-zbędne).
* **Strefa zbędne przestrzeni dyskowej (ZRS)**: W tym przypadku 3 repliki dane pozostaną w tym samym regionie Azure. Zgodnie z opisem w [] rozdziału [planowania — przewodnik-3.1] [przewodnika po planowaniu] [-przewodnik po planowaniu pod] Azure region może być liczbą centrach danych w pobliżu. W przypadku LRS repliki czy rozłożoną różnych centrach danych, że jeden region Azure.

Więcej informacji można znaleźć [w tym miejscu][storage-redundancy].
 
> [AZURE.NOTE] W przypadku wdrożeń bazami danych nie jest zalecane zastosowania Geo zbędne przestrzeni dyskowej
>
> Azure miejsca do magazynowania Geo replikacji jest asynchroniczna. Replikacja poszczególnych wirtualnych dysków twardych zainstalowany do pojedynczej maszyn wirtualnych nie są synchronizowane w kroku blokady. Dlatego nie jest odpowiedni do replikacji plików bazami danych, które są rozłożone w różnych wirtualnych dysków twardych lub używany przed oprogramowaniem RAID według wielu wirtualnych dysków twardych. Oprogramowanie bazami danych wymaga, aby trwałych miejsca dokładnie jest synchronizowane w różnych jednostek logicznych i źródłowych dysków-wirtualnych dysków twardych i dysków. Oprogramowanie bazami danych używa różne mechanizmy sekwencji Jo zapisu działaniami i bazami danych zgłasza, że miejsca, w których skierowane replikacji jest uszkodzony, jeśli te zależy od nawet kilku milisekund. W związku z tym jeśli jedną naprawdę chce konfiguracji bazy danych z bazy danych rozciągnięciu w wielu wirtualnych dysków twardych replikowane geo takie replikacji potrzebuje do wykonania z oznacza bazy danych i funkcji. Nie miały jedną na Azure miejsca do magazynowania Geo replikacji do wykonywania tego zadania. 
>
> Problem najprościej jest po wyjaśnić z systemem przykład. Załóżmy, że masz system SAP przekazane do Azure, w której znajduje się 8 wirtualnych dysków twardych plikami danych bazami danych oraz jeden wirtualny dysk twardy znajduje się plik dziennika transakcji. Każdą z nich te 9 wirtualnych dysków twardych ma dane zapisane w spójny sposób zgodnie z bazami danych, czy dane są zapisywane w plikach dziennika danych lub transakcji.
>
> W celu prawidłowo geo skopiowanymi dane i zachować obraz spójnej bazy danych zawartości wszystkich dziewięciu wirtualnych dysków twardych musi być replikowane geo w takiej kolejności operacji We/Wy były wykonywane dziewięć różnych wirtualnych dysków twardych. Jednak replikacji geo magazyn Azure nie zezwala na deklarować zależności między wirtualnych dysków twardych. Oznacza to, że magazyn Microsoft Azure geo replikacja nie rozpozna faktu, że zawartość tych dziewięciu wirtualnych dysków twardych różnych są powiązane ze sobą i zmiany danych są zgodne, tylko wtedy, gdy replikacji w kolejności operacji We/Wy stało przez wszystkich 9 wirtualnych dysków twardych.
>
> Oprócz szanse jest duży, że replikowane geo obrazów w scenariuszu nie zawierają obraz spójnej bazy danych, także jest wyświetlany z nośnikami zbędne geo, które mogą spowodować poważne zmniejszenie wydajności wpływ na wydajność. Podsumowanie nie należy używać tego typu nadmiarowości miejsca do magazynowania dla obciążenia typ bazami danych.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Mapowanie wirtualnych dysków twardych do konta magazynu usługi Azure maszyn wirtualnych
Konto miejsca do magazynowania Azure to nie tylko administracyjne konstrukcji, ale również temat ograniczeń. Ograniczenia dotyczące różnią się na czy porozmawiamy o konto Azure standardowy miejsca do magazynowania lub konto Azure Premium miejsca do magazynowania. Dokładne możliwości i ograniczenia są wymienione [tutaj][storage-scalability-targets]
 
Co do przechowywania standardowy Azure należy zwrócić uwagę na operacji i/o na SEKUNDĘ dla każdego konta miejsca do magazynowania jest ograniczona do (wiersz zawierający "Suma żądanie kurs" w [artykule][storage-scalability-targets]). Ponadto jest początkowej limit 100 kont miejsca do magazynowania dla subskrypcji Azure (od lipca 2015 r.). Dlatego zalecane jest saldo operacji i/o na SEKUNDĘ maszyny wirtualne między wieloma kontami miejsca do magazynowania, używając standardowego magazynu Azure. Dlatego pojedynczy maszyn wirtualnych używa najlepiej jednego konta miejsca do magazynowania, jeśli to możliwe. Dlatego jeśli porozmawiamy o wdrożeniach bazami danych, gdzie każdego wirtualny dysk twardy, znajdującej się na standardowy magazyn Azure mogą osiągnąć limit przydziału, należy wdrożyć tylko 30-40 wirtualnych dysków twardych na konto miejsca do magazynowania Azure używającej standardowego magazynu Azure. Z drugiej strony Jeśli wykorzystać magazyn Premium Azure i przechowywania bazy danych dużych ilości, może być cienkie pod względem operacji i/o na SEKUNDĘ. Ale konto Azure Premium miejsca do magazynowania jest sposób bardziej restrykcyjnie wielkości danych niż konto Azure standardowy miejsca do magazynowania. W wyniku tylko można wdrażać ograniczoną liczbą wirtualnych dysków twardych w ramach konta usługi Azure Premium miejsca do magazynowania przed naciśnięcie limit głośności danych. U myślenie zakończenia konto Azure miejsca do magazynowania jako "Wirtualnej SAN", który ma ograniczone możliwości w operacji i/o na SEKUNDĘ i/lub wydajność. W wyniku zadanie pozostanie, tak jak wdrożenia lokalne, aby zdefiniować układ wirtualnych dysków twardych różnych systemów SAP w różnych "urojonych SAN urządzenia" lub konta usługi Azure miejsca do magazynowania.
 
Do przechowywania standardowy Azure nie zaleca się prezentowanie miejsca do magazynowania z magazynu innego konta do pojedynczej maszyn wirtualnych, jeśli to możliwe.

Dlatego przy użyciu usługi Katalogowej lub serii GS maszyny wirtualne Azure jest możliwe instalacji wirtualnych dysków twardych poza Azure standardowy magazyn kont i miejsca do magazynowania Premium. Przypadków użycia, takich jak zapisywanie kopii zapasowych do standardowego magazynu kopii wirtualnych dysków twardych, dlatego po danych bazami danych i plików dziennika ilość miejsca do magazynowania Premium dostępne są miejsce, w którym można użyć takich niejednorodnymi miejsca do magazynowania. 

Na podstawie wdrożeń klienta i badania około 30-40 wirtualnych dysków twardych zawierające pliki danych bazy danych i plików dziennika może włączona na jednym Azure miejsca do magazynowania konto standardowe wydajności. Jak wspomniano wcześniej, limitów miejsca do magazynowania nominalnej Azure prawdopodobnie będzie możliwości danych, które mogą być przechowywane, a nie operacji i/o na SEKUNDĘ.

Jako SAN urządzeń z lokalnym programem, udostępnianie wymaga niektórych monitorowania, aby ostatecznie wykrywanie utrudnień na konto Azure miejsca do magazynowania. Rozszerzenie monitorowania Azure SAP i Azure Portal są narzędzia, których można używać do wykrycia zajęty kont miejsca do magazynowania Azure może przedstawiania utratę jakości wydajności Jo.  Jeśli zostanie wykryta sytuacja, zalecane jest, aby przenieść zajęty maszyny wirtualne do innego konta magazynu platformy Azure. Zajrzyj do [Podręcznik wdrażania] [Podręcznik wdrażania] Aby uzyskać szczegółowe informacje na temat aktywacji hosta SAP możliwości.

Inny artykuł podsumowywania najważniejsze wskazówki wokół Azure standardowego magazynu i konta usługi Azure standardowy przestrzeni dyskowej można znaleźć tutaj <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Przenoszenie wdrożony maszyny wirtualne bazami danych z magazynu standardowego Azure do magazynu Premium Azure
Firma Microsoft wystąpić sytuacje, w jakiś miejsce, w którym jako odbiorca ma zostać przeniesiona wdrożonym maszyn wirtualnych z magazynu standardowego Azure do magazynu Premium Azure. Nie jest to możliwe bez fizycznego przenoszenia danych. Istnieje kilka sposobów, aby osiągnąć cel:

* Nowe konto Azure Premium miejsca do magazynowania może po prostu skopiować wszystkie wirtualnych dysków twardych, podstawa wirtualnego dysku twardego a także danych wirtualnych dysków twardych. Często wybrano liczbę wirtualnych dysków twardych w magazynie standardowy Azure nie ze względu na fakt, że potrzebne wielkość danych. Ale wymagany tak wielu wirtualnych dysków twardych ze względu na operacji i/o na SEKUNDĘ. Teraz, gdy zostanie przeniesiony do magazynowania Premium Azure może przejdziesz sposób mniej wirtualnych dysków twardych uzyskanie niektórych przepustowość operacji i/o na SEKUNDĘ. Względu na fakt, że w magazynie standardowy Azure płacisz dla używanych danych, a nie rozmiar nominalna dysku, liczba wirtualnych dysków twardych czy naprawdę nie ma znaczenia, pod względem kosztów. Z miejscem do magazynowania Premium Azure, chcesz zapłacić za rozmiar nominalna dysku. Dlatego większość klienta wypróbować przechowywać liczbę wirtualnych dysków twardych Azure Premium pod numerem niezbędne do osiągnięcia przepustowość operacji i/o na SEKUNDĘ niezbędne. Tak większość klientów zdecyduj, przed tak proste 1:1 Kopiuj.
* Jeśli jeszcze nie jest zainstalowany, możesz zainstalować pojedynczy wirtualny dysk twardy, zawierające kopii zapasowej bazy danych bazy danych usługi SAP. Po wykonaniu kopii zapasowej Odinstaluj wszystkie wirtualnych dysków twardych, łącznie z wirtualnego dysku twardego zawierający kopię zapasową i kopiowanie podstawowej wirtualnego dysku twardego i wirtualnego dysku twardego z kopii zapasowej do konta usługi Magazyn Premium Azure. Czy następnie wdrożyć maszyn wirtualnych, oparte na podstawie wirtualny dysk twardy i zainstalować wirtualny dysk twardy z kopii zapasowej. Teraz możesz utworzyć dodatkowe dyski pustego miejsca do magazynowania Premium maszyn wirtualnych, służące do przywracania bazy danych do. Przy założeniu, że bazami danych umożliwia zmianę ścieżki do plików danych i dziennika w ramach procesu przywracania.
* Inną możliwością jest odmian byłego procesu, miejsce, w którym możesz po prostu pliku kopii zapasowej wirtualnego dysku twardego do magazynu Premium Azure i dołączyć go przed maszyn wirtualnych, który nowo wdrożony i zainstalowany.
* Czwarta możliwość należy wybrać po potrzebę Aby zmienić liczbę plików danych bazy danych. W takim przypadku czy wykonywanie kopii jednolitego systemu SAP przy użyciu eksportu/importu. Położenie tych eksportowanie plików do wirtualny dysk twardy, który jest kopiowana do konta usługi Azure Premium miejsca do magazynowania i dołączyć go do maszyn wirtualnych, używanego do uruchamiania procesu importowania. Klienci za pomocą tej możliwości przede wszystkim, gdy chcą zmniejszyć liczbę plików danych.

### <a name="deployment-of-vms-for-sap-in-azure"></a>Wdrożenie maszyny wirtualne dla systemu SAP platformy Azure
Microsoft Azure oferuje wiele sposobów wdrażanie maszyny wirtualne i skojarzone dysków. Co jest ważne zrozumienie różnic od przygotowania maszyny wirtualne mogą się różnić zależy od sposobu wdrożenia. Na ogół firma Microsoft Sprawdź do scenariusze opisane w kolejnych rozdziałów.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Wdrażanie maszyn wirtualnych z Azure Marketplace
Chcesz wykonać firmy Microsoft lub strona 3 opisane obraz z usługi Azure Marketplace wdrażanie usługi maszyn wirtualnych. Po wdrożeniu programu maszyn wirtualnych w Azure, należy wykonać tę samą wskazówek i narzędzi do zainstalowania oprogramowania SAP w swojej maszyn wirtualnych w sposób jak w środowisku lokalnym. Dotyczące instalowania oprogramowania SAP wewnątrz maszyn wirtualnych Azure, SAP i Microsoft zaleca się przekazywanie i przechowywanie nośnik instalacyjny SAP w wirtualnych dysków twardych Azure lub tworzenia maszyn wirtualnych Azure działa zgodnie z "serwer plików", który zawiera wszystkie niezbędne nośnik instalacyjny SAP.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Wdrażanie maszyn wirtualnych z określonego obrazu uogólniony klienta
Ze względu na określone wymagania dotyczące w odniesieniu do Twojej wersji systemu operacyjnego lub bazami danych dostarczonych obrazy Azure Marketplace może nie odpowiada Twoim potrzebom. Dlatego konieczne może być Tworzenie maszyn wirtualnych za pomocą własnego "private" obrazu maszyn wirtualnych systemu operacyjnego i bazami danych, które mogą być rozmieszczone po kilka razy. Aby przygotować "private" obraz duplikatów, system operacyjny musi być uogólniony na maszyn wirtualnych lokalnego. Zajrzyj do [Podręcznik wdrażania] [Podręcznik wdrażania] Aby uzyskać szczegółowe informacje na temat uogólniać maszyny.

Jeśli zainstalowano już SAP zawartości w swojej maszyn wirtualnych lokalnego (zwłaszcza w przypadku systemów poziom 2), możesz dopasować ustawienia systemu SAP po wdrażania maszyn wirtualnych Azure za pomocą wystąpienia nazwy procedury obsługiwane przez SAP oprogramowania inicjowania obsługi administracyjnej Menedżera (Uwaga systemu SAP [1619720]). W przeciwnym razie możesz zainstalować oprogramowanie SAP później po wdrażania maszyn wirtualnych Azure.
 
Z bazy danych zawartości używane przez aplikację SAP można albo wygenerować zawartość świeżo podczas instalacji SAP lub zawartości można zaimportować do Azure za pomocą wirtualny dysk twardy z kopii zapasowej bazy danych bazami danych lub wykorzystując możliwości Pozwalające bezpośrednio kopii zapasowej do magazyn Microsoft Azure. W tym przypadku można również przygotować wirtualnych dysków twardych z bazami danych danych i dziennika pliki lokalnego, a następnie zaimportować je jako dyski do Azure. Ale transfer danych bazami danych, który jest wyświetlany załadowana z lokalnego Azure sprawdzają się podczas pracy nad dysków wirtualny dysk twardy, które trzeba przygotować lokalnego.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Przenoszenie maszyn wirtualnych ze źródeł lokalnych Azure dyskiem uogólniony
Planowanie przenieść określonego systemu SAP z lokalnego Azure (dźwigu i shift). Można to zrobić, przekazując wirtualny dysk twardy zawierający system operacyjny, plików binarnych SAP i ewentualnego bazami danych plików binarnych oraz wirtualnych dysków twardych z plikami danych i dziennika Pozwalające Azure. W przeciwnym Scenariusz #2 powyżej, przechowujesz hostname, identyfikator SAP zabezpieczeń i kont użytkowników systemu SAP w maszyn wirtualnych Azure one zostały skonfigurowane w lokalnym środowisku. Dlatego uogólnianie obraz nie jest konieczne. Tym przypadku głównie zostaną zastosowane dla lokalnej krzyżowe scenariuszy, gdzie część pozioma SAP jest uruchomiona w lokalnym i części Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Wysoką dostępność i funkcji odzyskiwania Azure maszyny wirtualne
Azure oferuje następujące funkcje wysokiej dostępności (HA) i odzyskiwanie danych (DR), które mają zastosowanie do innych składników, które jako należy użyć w przypadku wdrożeń SAP i bazami danych

### <a name="vms-deployed-on-azure-nodes"></a>Maszyny wirtualne wdrożony w węzłach Azure
Platformy Azure nie oferuje funkcje, takie jak Live migracji dla wdrożonej maszyny wirtualne. Oznacza to, jeśli w klastrze serwerów, na którym jest używany maszyny konieczne jest konserwacji, maszyn wirtualnych należy pobrać zatrzymaniu i ponownym uruchomieniu. Zachowanie w Azure odbywa się przy użyciu tak o nazwie uaktualnianie domen w ramach klastrów serwerów. Tylko jedną domenę uaktualnienie w danej chwili jest rejestrowana. Podczas ponownego uruchamiania będzie przerwy w świadczeniu usług podczas zamykania maszyn wirtualnych, przeprowadzany jest konserwacji i ponowne uruchomienie maszyn wirtualnych. Większość dostawców bazami danych jednak zapewniają wysoką dostępność i odzyskiwanie funkcje, które będą szybko Jeśli ponowne uruchomienie usług bazami danych w innym węźle węzeł podstawowy jest niedostępny. Platformy Azure udostępnia funkcje do rozpowszechniania maszyny wirtualne, przechowywania i innych usług Azure w domenach uaktualnienie, aby upewnić się, że planowanej konserwacji lub infrastruktury błędów czy tylko wpływ mały podzbiór maszyny wirtualne lub usług.  Planowanie staranne jest możliwe uzyskanie poziomy dostępności porównywalna do infrastruktury lokalnej.

Microsoft Azure dostępność zestawów jest logiczna grupa maszyny wirtualne lub usług, które zapewnia maszyny wirtualne i innych usług są rozdzielane różnych błędów i uaktualnianie domen w klastrze tak, aby może istnieć tylko jeden zamknięcia węzeł w każdym punkcie w czasie (przeczytaj [ten] [ virtual-machines-manage-availability] artykuł, aby uzyskać więcej informacji).

Musi być skonfigurowany w celu wdrażania maszyny wirtualne, jak pokazano poniżej:

![Definicja zestawu dostępność HA bazami danych konfiguracji][dbms-guide-figure-200]

Jeśli chcemy utworzyć wysoce dostępnych konfiguracji wdrożenia bazami danych (niezależnie od poszczególnych HA bazami danych funkcji używanych) maszyny wirtualne bazami danych należy:

* Dodawanie maszyny wirtualne do tej samej sieci wirtualnej Azure (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* Maszyny wirtualne konfiguracji HA należy również w tej samej podsieci. Rozpoznawanie nazw między różnych podsieci nie jest możliwe we wdrożeniach tylko do chmury, będzie działać tylko rozpoznawanie adresów IP. Przy użyciu witryny do witryny lub łączności ExpressRoute dla wdrożenia lokalne krzyżowe, sieci z co najmniej jedną podsieć będzie można już zostały ustanowione. Rozpoznawanie nazw, które zostaną wykonane zgodnie z lokalnego AD zasad i infrastruktury sieciowej. 
[komentarz]: <>  (Test zadania MSSedusch, jeśli nadal ma wartość PRAWDA, w imieniu)

#### <a name="ip-addresses"></a>Adresy IP
Zdecydowanie zaleca się w sposób mechanizm ustawień maszyny wirtualne HA konfiguracji. Polegaj na adresów IP w celu rozwiązania partnerów HA w konfiguracji HA nie jest niezawodne platformy Azure chyba że statyczne adresy IP są używane. Istnieją dwa pojęcia "Zamknięcie" Azure:

* Zamknij za pośrednictwem polecenia cmdlet Azure Portal lub Azure programu PowerShell AzureRmVM tabulatora: W tym przypadku maszyny wirtualnej pobiera zamknięcia i usuń zaznaczenie pola przydzielone. Konto Azure nie jest już zostanie naliczona dla tego maszyn wirtualnych, tak aby tylko opłaty, które będzie powodowało do przechowywania używane. Jednak jeśli nie statyczny adres IP prywatnego interfejsu sieci, adres IP został wydany i nie gwarantuje, że interfejsu sieciowego otrzymuje stary adres IP przypisany ponownie po ponownym uruchomieniem maszyn wirtualnych. Podczas wykonywania polecenia Zamknij w dół przez Azure Portal lub dzwoniąc AzureRmVM Zatrzymaj automatyczne spowoduje, że do alokacji. Jeśli nie chcesz deallocat komputera za pomocą tabulatora AzureRmVM - StayProvisioned 
* Wyłączenie maszyn wirtualnych z poziomu OS maszyn wirtualnych otrzymuje Zamknij i nie Anuluj przydzielone. Jednak w tym przypadku Azure konta będą nadal obciążony Głosowa, mimo że jest zamknięty. W takim przypadku przypisania adresu IP w celu zatrzymania maszyn wirtualnych pozostaną niezmienione. Zamykanie maszyn wirtualnych z poziomu nie zostanie automatycznie wymusić do alokacji.

Nawet w przypadku scenariuszy lokalnej krzyżowe domyślnie zamknięcia i do alokacji będzie oznaczać do przypisywanie adresów IP z Głosowa, nawet jeśli zasady lokalnego w ustawieniach DHCP są różne. 

* Wyjątkiem jest jedną przypisuje statycznego adresu IP do karty sieciowej jako opisane [tutaj][virtual-networks-reserved-private-ip].
* W takim przypadku adres IP pozostaje stały, dopóki nie zostanie usunięty z interfejsem sieci.

> [AZURE.IMPORTANT] Aby cały wdrożenia proste i łatwe w zarządzaniu, wyczyść zaleca się skonfigurować maszyny wirtualne, partnerstwo bazami danych HA lub Konfiguracja DR w Azure w sposób działa rozpoznawanie nazw między różne maszyny wirtualne związane.
 
## <a name="deployment-of-host-monitoring"></a>Wdrożenie hosta monitorowania
Wydajność zastosowania aplikacji SAP w Azure środowisku maszyn wirtualnych systemu SAP wymaga możliwość uzyskania hosta monitorowanie danych z fizycznie hosty z usługą Azure maszyn wirtualnych. Określonego poziomu poprawki SAP HostAgent będą wymagane, który umożliwia tę funkcję w SAPOSCOL i SAP HostAgent. Poziom dokładnie poprawkami opisano w Uwaga systemu SAP [1409604].

Aby uzyskać szczegółowe informacje dotyczące wdrażania składników, które dostarczają dane hosta SAPOSCOL i SAPHostAgent i zarządzania cyklu życia tych składników można znaleźć [Podręcznik wdrażania] [Podręcznik wdrażania]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Szczegóły do programu Microsoft SQL Server

### <a name="sql-server-iaas"></a>Program SQL Server IaaS
Począwszy od Microsoft Azure, możesz łatwo przeprowadzić migrację istniejącej aplikacji programu SQL Server oparte na platformie Windows Server do maszyn wirtualnych Azure. Program SQL Server w maszyny wirtualnej pozwala zmniejszyć całkowity koszt własność wdrażanie, zarządzanie i konserwacja popularności szerokość przez łatwe przeprowadzenie migracji tych aplikacji Microsoft Azure. Z programem SQL Server w maszyn wirtualnych Azure Administratorzy i deweloperzy nadal można używać w tej samej sytuacji i narzędzi administracyjnych, które są dostępne w lokalnym. 

> [AZURE.IMPORTANT] Pamiętaj, że firma Microsoft nie są omawiane Microsoft Azure SQL Database, które jest platformą jako oferty usługi platformy Microsoft Azure. Dyskusja w tym dokumencie jest o uruchamianiu programu SQL Server są znane w przypadku wdrożeń lokalnych w Azure maszyn wirtualnych, używanie infrastruktury jako możliwości usługi Azure. Funkcje bazy danych i funkcji między te dwie oferty są różne i nie może być mieszane ze sobą. Zobacz też: <https://azure.microsoft.com/services/sql-database/>
 
Zdecydowanie zaleca się aby zapoznać się z [tym] [ virtual-machines-sql-server-infrastructure-services] dokumentacji przed kontynuowaniem.

W poniższych sekcjach części części dokumentacji w obszarze łącze powyżej zostaną zagregowane i wymienionych. Szczegóły wokół SAP są wymienione także i bardziej szczegółowo opisano niektóre pojęcia. Jednak zaleca do pracy z dokumentacją powyżej pierwszego przed czytania dokumentacji określonego programu SQL Server.

Istnieje kilka programu SQL Server w IaaS określonych informacji, których warto wiedzieć przed kontynuowaniem:

* **Umowa dotycząca poziomu usług maszyn wirtualnych**: istnieje umowa dotycząca poziomu usług dla maszyn wirtualnych działa Azure, które można znaleźć tutaj: <https://azure.microsoft.com/support/legal/sla/>  
* **Obsługuje wersja SQL**: w przypadku klientów SAP obsługujemy SQL Server 2008 R2 lub nowszy na Microsoft Azure Virtual Machine. Wcześniejszych wersjach nie są obsługiwane. Przejrzyj tej ogólne [Poufności informacji pomocy technicznej](https://support.microsoft.com/kb/956893) , aby uzyskać więcej informacji. Pamiętaj, że ogólnie programu SQL Server 2008 jest obsługiwana przez firmę Microsoft także. Jednak ze względu na znaczną funkcje SAP, która została wprowadzona z programu SQL Server 2008 R2, SQL Server 2008 R2 jest minimalna zwolnienia dla SAP. Należy pamiętać, SQL Server 2012 i 2014 masz rozszerzonego z lepsza integracja do tego scenariusza IaaS (na przykład wykonywania kopii zapasowych bezpośrednio przed magazyn Azure). W związku z tym możemy ograniczyć tego dokumentu do programu SQL Server 2012 i 2014 z poziomu najnowszą poprawkę dla Azure.
* **Obsługa funkcji języka SQL**: najbardziej programu SQL Server funkcje są obsługiwane w programie Microsoft Azure środowisku maszyn wirtualnych systemu z pewnymi wyjątkami. **Program SQL Server awaryjnej przy użyciu udostępnionych dysków nie jest obsługiwane**.  Technologie, takie jak odzwierciedlające bazy danych, grupy dostępności (AlwaysOn), replikacji, dziennika wysyłki i Service Broker są obsługiwane w jednym regionie Azure rozproszone. Program SQL Server (AlwaysOn) również jest obsługiwany między różnymi regionami Azure opisane tutaj: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Przejrzyj [Poufności informacji pomocy technicznej](https://support.microsoft.com/kb/956893) , aby uzyskać więcej informacji. Przykład na temat instalacji konfiguracji (AlwaysOn) znajduje się w [tym] [ virtual-machines-workload-template-sql-alwayson] artykuł. Ponadto zapoznaj się z najważniejsze wskazówki udokumentowanych [tutaj][virtual-machines-sql-server-infrastructure-services] 
* **Wydajność SQL**: możemy pewność, że mogą się różnić Microsoft Azure hostowanej maszyn wirtualnych wykona dobrze w porównaniu z systemem innym publicznej chmury wirtualizacji ofert, ale poszczególnych wyników. Zapoznaj się z [tym] [ virtual-machines-sql-server-performance-best-practices] artykuł.
* **Używanie obrazów z usługi Azure Marketplace**: najszybszym sposobem na wdrażanie nowa maszyna Microsoft Azure wirtualna ma zastosowanie obrazu z usługi Azure Marketplace. Istnieje obrazów w Azure Marketplace, które zawierają programu SQL Server. Obrazy programu SQL Server już zainstalowanym nie można od razu SAP NetWeaver aplikacji. Przyczyną tego jest domyślne sortowanie programu SQL Server jest zainstalowany w tych obrazów i nie wymaga systemów SAP NetWeaver sortowania. Aby można było użyć tych obrazów, sprawdź, czy czynności opisanych w rozdziału [przy użyciu obrazów programu SQL Server poza Microsoft Azure Marketplace] [bazami danych — przewodnik 5.6]. 
* Aby uzyskać więcej informacji, zapoznaj się [Ceny szczegóły](https://azure.microsoft.com/pricing/) . [Przewodnik licencjonowania programu SQL Server 2012](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) i [SQL Server 2014 Przewodnik licencjonowania](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) są również ważne zasobów.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Wskazówki dotyczące konfigurowania programu SQL Server dla SAP dotyczące instalacji programu SQL Server w maszyny wirtualne Azure

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Zalecenia dotyczące struktury maszyn wirtualnych-wirtualnego dysku twardego dla systemu SAP dotyczące wdrożenia programu SQL Server
Zgodnie z ogólnym opisem pliki wykonywalne programu SQL Server powinny być znajduje się lub zainstalowany na dysku systemowym maszyn podstawowej wirtualny dysk twardy (dysk C:\).  Zazwyczaj większość systemowych baz danych programu SQL Server nie mogą być wykorzystane na wysokim poziomie przez obciążenie pracą SAP NetWeaver. W związku z tym systemowych baz danych programu SQL Server (wzorca, msdb i model) może pozostać na tym dysku C:\. Wyjątków może być tymczasowe, co w przypadku niektórych ERP SAP i wszystkie Przepustowości obciążeń pracą, może wymagać większą ilość danych lub głośność operacje wejścia/wyjścia, która nie mieści się w oryginalnym maszyn wirtualnych. Systemów można wykonać następujące czynności:

* Przenoszenie podstawowego tymczasowe pliki danych na tym samym dysk logiczny jako pliki danych podstawowych SAP bazy danych.
* Dodaj wszelkie dodatkowe tymczasowe pliki danych do wszystkich innych dysków logicznych, zawierającego plik danych bazy danych SAP.
* Dodawanie pliku dziennika tymczasowe do logiczne dysk, na którym znajduje się plik dziennika bazy danych użytkownika.
* **Wyłącznie dla typów maszyn wirtualnych, które używają lokalnych twarde SSD** na obliczeń węzeł tymczasowe plików danych i dziennika może znajdować się na dysku D:\. Jednak może być zalecane korzystanie z wielu plików danych tymczasowe. Należy pamiętać, że różnią się wielkości dysk D:\ zależy od typu maszyn wirtualnych.
 
Tę konfigurację włączyć tymczasowe wykorzystać więcej miejsca niż dyskiem systemowym jest możliwość udostępnienia. W celu określenia rozmiaru tymczasowe pisane z wielkiej litery, jedną Sprawdź rozmiarów tymczasowe na istniejących systemów, które są uruchamiane w lokalnej. Ponadto taka konfiguracja chcesz włączyć numery operacji i/o na SEKUNDĘ przed tymczasowe, której nie można przedstawić z dysku systemowym. Ponownie systemów, które są uruchomione w lokalnym może służyć do monitora obciążenie We/Wy przed tymczasowe tak, aby można czerpać liczby operacji i/o na SEKUNDĘ, które powinny być widoczne na swojej tymczasowe.

Konfiguracja maszyn wirtualnych, w której jest uruchomiony program SQL Server z bazą danych SAP i miejsce, w którym dane tymczasowe i pliku dziennika tymczasowe są umieszczane na dysku D:\ będzie wyglądać:
 
![Konfiguracja odwołania Azure IaaS maszyn wirtualnych protokołu SAP][dbms-guide-figure-300]

Należy pamiętać, że dysk D:\ ma zależy od typu maszyn wirtualnych o różnych rozmiarach. Zależne od rozmiaru wymagania tymczasowe możesz może być wymuszone pary plików danych i dziennika tymczasowe z SAP bazy danych plików danych i dziennika w przypadku gdy D:\ dysk jest za mały.

#### <a name="formatting-the-vhds"></a>Formatowanie wirtualnych dysków twardych
Dla programu SQL Server NTFS zablokować rozmiar wirtualnych dysków twardych zawierającą dane programu SQL Server i plików dziennika powinna być 64 KB. Nie jest konieczne sformatowanie D:\. Ten dysk zawiera wstępnie sformatowane.

Aby upewnić się, że przywracania lub tworzenia baz danych nie inicjuje pliki danych przez zerowanie zawartość plików, jedną upewnij się, że usługi SQL Server jest uruchomione w kontekście użytkownika ma określonych uprawnień. Zazwyczaj użytkowników do grupy Administratorzy systemu Windows mają te uprawnienia. Jeśli usługi programu SQL Server jest uruchamiana w kontekście użytkownika nie będący administratorami systemu Windows użytkownika, należy przypisać uprawnienie "Wykonanie zadania konserwacyjne głośności" tego użytkownika.  Zobacz szczegóły w tym artykule z bazy wiedzy Microsoft: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Wpływ kompresji bazy danych
W konfiguracjach miejsce, w którym przepustowości we/wy może stać się ograniczanie czynniki każdej miary, zmniejszając operacji i/o na SEKUNDĘ mogą ułatwić aby rozciągnąć obciążenie pracą, które jedną można uruchamiać w scenariuszu IaaS, takich jak Azure. W związku z tym jeśli jeszcze tego nie zrobiono, stosując program SQL Server PAGE kompresji zdecydowanie zalecane jest przez firmy Microsoft i SAP przed przekazywania SAP istniejącej bazy danych Azure.
 
Zalecenie wykonaj kompresję bazy danych przed przekazaniem Azure, wyznaczona wylogowywanie się z dwóch powodów:

* Ilość danych do przekazania jest niższa.
* Czas realizacji kompresji jest krótsza, przy założeniu, że użyć można silniejsze sprzętu z więcej procesorów lub większej przepustowości we/wy lub mniej we/wy opóźnienie lokalnego.
* Mniejszy rozmiar bazy danych może prowadzić do mniejszych koszty przydziału dysku

Kompresowanie bazy danych działa także w maszyn wirtualnych Azure, co lokalnego. Aby uzyskać więcej informacji na temat Kompresowanie istniejącego serwera SQL SAP bazy danych sprawdź, czy tutaj: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>Program SQL Server 2014 — przechowywania bazy danych plików bezpośrednio na Azure blogu miejsca do magazynowania
Program SQL Server 2014 otwiera możliwość przechowywać pliki bazy danych bezpośrednio w magazynie obiektów Blob platformy Azure bez "opakowanie" wirtualnego dysku twardego wokół nich. Zwłaszcza z za pomocą standardowej magazyn Azure lub mniejszych typów maszyn wirtualnych dzięki temu scenariusze, w którym można rozwiązać, limity operacji i/o na SEKUNDĘ którego będą wymuszane przez ograniczoną liczbę wirtualnych dysków twardych, które można zainstalować na kilka mniejszych typy maszyn wirtualnych. To działa w przypadku baz danych użytkowników, jednak nie dla systemowych baz danych programu SQL Server. Działa również danych i plików dziennika programu SQL Server. Jeśli chcesz wdrożyć bazę danych programu SQL Server SAP w ten sposób zamiast "zawijania" do wirtualnych dysków twardych, pamiętaj o następujących pamiętać:

* Konto magazynu używane musi znajdować się w tym samym regionie Azure jak ten, który służy do wdrażania maszyn wirtualnych SQL Server jest uruchomiony w.
* Na liście wcześniej w odniesieniu do rozpowszechniania wirtualnych dysków twardych w różnych kont miejsca do magazynowania Azure kwestie dla tej metody także wdrożeń. Oznacza, że liczba operacji We/Wy limitów konta magazynu platformy Azure.
[komentarz]: <>  (MSSedusch zadania, ale to będzie korzystać z sieci przepustowość sieci i nie przepustowość miejsca do magazynowania sieci, nie go?)

Szczegóły dotyczące tego typu wdrożenia są wymienione tutaj: <https://msdn.microsoft.com/library/dn385720.aspx>
 
W celu przechowywania plików danych programu SQL Server bezpośrednio na magazyn Premium Azure, trzeba mieć minimalna zwolnienia poprawki SQL Server 2014, która jest opisanych tutaj: <https://support.microsoft.com/kb/3063054>. Przechowywanie plików danych programu SQL Server na standardowy magazyn Azure działa z wersji SQL Server 2014. Jednak tej samej poprawki zawierać inną serię poprawek, które bezpośredniego zastosowania magazyn obiektów Blob platformy Azure dla plików danych programu SQL Server i kopie zapasowe niezawodne. W związku z tym zaleca się na ogół używania te poprawki.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 buforu puli rozszerzenia
Program SQL Server 2014 wprowadzono nową funkcję, nazywany rozszerzenia puli buforu. Ta funkcja rozszerza puli buforów programu SQL Server, które są przechowywane w pamięci z drugiego poziomu pamięci podręcznej, które jest przechowywana w lokalnym twarde SSD serwera lub maszyn wirtualnych. Dzięki temu przechowywania większych zestaw roboczy danych "w pamięci". W porównaniu do uzyskiwania dostępu do standardowego magazynu Azure dostęp do rozszerzenia puli buforów, który jest przechowywany na lokalnym twarde SSD o maszyn wirtualnych Azure jest wiele czynników szybciej.  W związku z tym używanie na lokalnym dysku D:\ typów maszyn wirtualnych, które mają doskonałe operacji i/o na SEKUNDĘ i przepustowość może być bardzo odpowiednim sposobem zmniejszenia obciążenia operacji i/o na SEKUNDĘ przed magazyn Azure i znacznie skrócić czas reakcji kwerend. Dotyczy to zwłaszcza w przypadku, gdy nie przy użyciu magazynu Premium. W przypadku przechowywania Premium i użycie pamięci podręcznej odczytu Azure Premium w węźle obliczeń zgodnie z zaleceniami plików danych oczekuje żadnych duży różnic. Przyczyną jest to, że oba pamięci podręcznej (program SQL Server buforu puli rozszerzenia i Premium miejsca do magazynowania odczytu w pamięci podręcznej) z lokalnych dyskach węzłów obliczeń.
Aby uzyskać więcej informacji na temat tej funkcji, sprawdź, czy niniejsza dokumentacja: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Wykonywanie kopii zapasowych i odzyskiwania zagadnienia związane z programu SQL Server
Podczas wdrażania programu SQL Server do Twojej kopii zapasowej metodologii należy przejrzeć Azure. Nawet jeśli system nie jest wydajność systemu, SAP obsługiwanych przez program SQL Server musi być kopię zapasową bazy danych okresowo. Ponieważ Magazyn Azure zachowuje trzy obrazy, kopii zapasowej jest teraz mniej ważne w odniesieniu do wyrównywania awarię miejsca do magazynowania. Powodem priorytet zachowaniu właściwy plan i przywracania kopii zapasowych jest więcej, który można zadośćuczynienia pod kątem błędów logicznych i ręczne, dostarczając punktu w funkcji odzyskiwania czasu. Aby celem jest albo użyj wykonywanie kopii zapasowych przywrócenia bazy danych do określonego punktu w czasie za pomocą kopii zapasowych w Azure zapełnić innego systemu, kopiując istniejącej bazy danych. Na przykład użytkownik może przekazać z poziomu 2 konfiguracji systemu SAP do konfiguracji systemu poziomu 3 tego samego systemu przywrócenia kopii zapasowej.

Istnieją trzy różne sposoby kopii zapasowej programu SQL Server do magazynu Azure:
 
1. Program SQL Server 2012 CU4 i nowszy mogą oryginalnie kopii zapasowej bazy danych do adresu URL. Jest to szczegółowo w blogu [nowych funkcji programu SQL Server 2014 — część 5 — Tworzenie kopii zapasowych i przywracanie ulepszenia](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Zobacz rozdział [SQL Server 2012 z dodatkiem SP1 CU4 lub w nowszej wersji] [bazami danych — przewodnik 5.5.1].
1. Wersje programu SQL Server przed SQL 2012 CU4 używać funkcji przekierowywania kopia zapasowa do wirtualnego dysku twardego i zasadniczo przejścia strumienia zapisu do lokalizacji przechowywania Azure, który został skonfigurowany. Zobacz rozdział [SQL Server 2012 z dodatkiem SP1 CU3 i starszych wersjach] [bazami danych — przewodnik 5.5.2].
1. Ostatnia metoda jest wykonywanie kopii zapasowej konwencjonalny programu SQL Server do polecenia dysku na dysk wirtualny dysk twardy.  To jest identyczny z wzorcem wdrożenia lokalnego i nie został omówiony omówione w tym dokumencie.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>Program SQL Server 2012 z dodatkiem SP1 CU4 lub w nowszej wersji
Ta funkcja umożliwia bezpośrednio kopia zapasowa Magazyn obiektów BLOB platformy Azure. Bez tej metody musisz kopii zapasowych do innych wirtualnych dysków twardych Azure, które chcesz wykorzystać możliwości wirtualnego dysku twardego i operacji i/o na SEKUNDĘ. Koncepcja zasadniczo są następujące:
 
 ![Używanie narzędzia Kopia zapasowa programu SQL Server 2012 z platformy Microsoft Azure magazynem obiektów BLOB][dbms-guide-figure-400]

Zaletą jest w tym przypadku jedną nie wymaga wydatki wirtualnych dysków twardych do przechowywania kopii zapasowych programu SQL Server na. Dlatego masz mniej wirtualnych dysków twardych przydzielonego i całego przepustowość operacji i/o na SEKUNDĘ wirtualnego dysku twardego może być używany do plików danych i dziennika. Należy zauważyć, że maksymalny rozmiar kopii zapasowej jest ograniczone do maksymalnie 1 TB opisane w sekcji "Ograniczenia" w tym artykule: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Jeśli rozmiaru kopii zapasowej, mimo przy użyciu narzędzia Kopia zapasowa SQL Server kompresji przekracza 1 TB w rozmiarze funkcji opisanych w rozdziału [SQL Server 2012 z dodatkiem SP1 CU3 i starszych wersjach] [bazami danych — przewodnik-5.5.2] w tym dokument ma być używany.

[Pokrewne dokumentację](https://msdn.microsoft.com/library/dn449492.aspx) opisującą przywracania bazy danych z kopii zapasowych przed magazyn obiektów Blob platformy Azure zaleca się nie chcesz przywrócić bezpośrednio z magazynu obiektów BLOB platformy Azure kopie zapasowe > 25 GB. Zalecenie w tym artykule po prostu jest oparty na zagadnienia dotyczące wydajności, a nie z powodu ograniczeń funkcjonalności. W związku z tym różne kryteria mogą stosować na podstawie indywidualnie.

Dokumentacja dotycząca jak skonfigurować i użyć tego typu kopii zapasowej można znaleźć w [tym](https://msdn.microsoft.com/library/dn466438.aspx) samouczku
 
Przykład sekwencji czynności mogą być odczytywane [tutaj](https://msdn.microsoft.com/library/dn435916.aspx).

Automatyzacja kopie zapasowe, jest najwyższe znaczenie, aby się upewnić, że inne nazwy obiektów blob dla każdej kopii zapasowej. W przeciwnym razie zostanie zastąpiony i łańcuch przywracania zostało przerwane.
 
Aby nie mieszać rzeczy między różnymi rodzajami 3 kopii zapasowych w górę zaleca się tworzyć różne kontenery poniżej miejsca do magazynowania konto używane do wykonywania kopii zapasowych. Kontenery może być przez maszyn wirtualnych tylko lub według typu maszyn wirtualnych i kopia zapasowa. Schemat może wyglądać:
 
 ![Za pomocą programu SQL Server 2012 kopia zapasowa Microsoft Azure magazyn obiektów BLOB — różne kontenery osobne konta miejsca do magazynowania][dbms-guide-figure-500]

W powyższym przykładzie do tego samego konta miejsca do magazynowania, w których wdrażane maszyny wirtualne nie jest wykonywane kopie zapasowe. Będzie nowego konta miejsca do magazynowania dla kopii zapasowych. W kont miejsca do magazynowania będzie różne kontenery utworzone za pomocą macierzy typu kopii zapasowej i nazwa maszyn wirtualnych. Takie segmentacji ułatwi administrować kopie zapasowe różnych maszyny wirtualne.

Blob jedną bezpośrednio zapisuje kopie zapasowe do, nie można dodać do liczby wirtualnych dysków twardych z maszyny. W związku z tym jedną można zwiększyć maksymalną liczbę wirtualnych dysków twardych zainstalowany z określonym SKU maszyn wirtualnych dla danych i transakcji pliku dziennika i nadal wykonywanie kopii zapasowej przed kontenera magazynu. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>Program SQL Server 2012 z dodatkiem SP1 CU3 i starszych wersjach
Najpierw należy wykonać w celu uzyskania kopii zapasowej bezpośrednio przed magazyn Azure jest pobieranie msi, które jest połączone z [tego](https://www.microsoft.com/download/details.aspx?id=40740) artykułu KBA.
 
Plik do pobrania x64 instalacji i dokumentacji. Plik zostanie zainstalowany program o nazwie: "Microsoft SQL Server kopii zapasowej do firmy Microsoft Azure narzędzia". Zapoznaj się z dokumentacją produktu dokładnie.  Narzędzie sprawdza się zasadniczo w następujący sposób:

* Na stronie programu SQL Server jest definiowana lokalizacji na dysku kopii zapasowej programu SQL Server (nie używaj dysk D:\ to).
* Narzędzie umożliwia definiowanie reguł, które mogą być używane w celu skierowania różnego rodzaju kopie zapasowe do różnych kontenerów magazyn Azure.
* Gdy zasady są w miejscu, narzędzie przekieruje strumienia zapisu kopii zapasowej do jednego z wirtualnych dysków twardych i dysków do lokalizacji przechowywania Azure, który został wcześniej zdefiniowany.
* Narzędzie pozostawi pliku niewielkie kilka rozmiarów KB na wirtualnego dysku twardego i dysku, który został zdefiniowany dla programu SQL Server kopii zapasowej. **Ten plik należy pozostawić w lokalizacji magazynu jest wymagane do przywrócenia ponownie z magazynu Azure.**
    * Jeśli wybrano opcję tworzenia kopii zapasowych na konto Microsoft Azure magazynowania utracono element zastępczy pliku (np. przez strata nośników, w której znajdował się element zastępczy pliku), może odzyskać element zastępczy pliku za pośrednictwem magazyn Microsoft Azure, pobierając ją z kontenera magazynu, w którym został on umieszczony. Następnie należy umieścić element zastępczy pliku do folderu na komputerze lokalnym, w którym narzędzie jest skonfigurowany do wykrywanie i Przekaż na tym samym kontenerze, używając tego samego hasła szyfrowania, jeśli użyto szyfrowania z istniejącą regułę. 

Oznacza to, że schemat zgodnie z powyższym opisem dla nowszej wersji programu SQL Server mogą być umieszczane w miejscu także dla wersji programu SQL Server, które nie zezwalają na bezpośredni adres lokalizacji przechowywania Azure.
 
Ta metoda nie powinny być używane z nowszej wersji programu SQL Server, które obsługuje wykonywania kopii zapasowych oryginalnie przed magazyn Azure. Wyjątki są, gdzie ograniczenia natywnych kopii zapasowej do Azure blokują natywnych wykonanie kopii zapasowej do Azure.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Inne możliwości kopii zapasowej bazy danych programu SQL Server
Inne możliwości kopii zapasowej bazy danych jest dołączanie dodatkowych wirtualnych dysków twardych do maszyn wirtualnych, którego używasz do przechowywania kopii zapasowych na. W takim przypadku należy upewnić się, że wirtualnych dysków twardych nie są uruchomione pełny. Jeśli jest to wielkość liter, trzeba odinstalować wirtualny dysk twardy, a więc do speak "archiwizować" i zamień go na nowy pusty wirtualnego dysku twardego. Przejście w dół ścieżka chcesz zachować tych wirtualnych dysków twardych w osobnym konta usługi Azure miejsca do magazynowania niż który wirtualnych dysków twardych z plików bazy danych.

Druga możliwość jest użycie dużych maszyn wirtualnych, który może mieć wiele wirtualnych dysków twardych dołączony. Np. D14 z 32VHDs. Do tworzenia elastyczne środowisko użyć spacji miejsca do magazynowania miejsce, w którym można utworzyć akcje, które są używane następnie jako kopii zapasowej elementów docelowych dla różnych serwerów bazami danych.
 
Niektóre z najważniejszych wskazówek masz opisane [tutaj](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) również. 

#### <a name="performance-considerations-for-backupsrestores"></a>Zagadnienia dotyczące wydajności do wykonywania kopii zapasowych i przywracanie
Jak wdrożeń zera wydajności tworzenia kopii zapasowych i przywracania jest uzależniony od ilości ile można odczytywać równolegle i przepustowość tych wielkości, co można. Ponadto użycie Procesora używane przez kompresji kopii zapasowej mogą być odtwarzane znaczącą rolę na maszyny wirtualne z wątkami Procesora tylko do 8. Dlatego jedną Załóżmy:

* Mniejsza liczba wirtualnych dysków twardych służy do przechowywania danych plików, im mniejszy ogólną wydajność w odczytu.
* Mniejsza liczba procesorów wątki w Głosowa, bardziej poważne wpływu kompresji kopii zapasowej.
* Mniej celów (BLOB lub wirtualnych dysków twardych) do zapisywania kopii zapasowej, mniejszy przepustowość.
* Im mniejszy maszyn wirtualnych rozmiaru, im mniejszy przydziału miejsca do magazynowania przepustowość zapisywania i odczytywania z magazynu Azure. Niezależnie od tego, czy kopie zapasowe są przechowywane bezpośrednio na obiektów Blob platformy Azure lub czy są one przechowywane w wirtualnych dysków twardych, ponownie przechowywanymi w obiektów blob platformy Azure.

Podczas używania obiektów BLOB Microsoft Azure miejsca do magazynowania jako docelowy kopii zapasowej w nowszej wersji, jest ograniczone do wyznaczające tylko jeden docelowy adres URL dla każdej kopii zapasowej.
 
Jednak podczas używania "Microsoft SQL Server kopia zapasowa Microsoft Azure Tool" w starszych wersjach, można określić więcej niż jeden plik docelowy. Więcej niż jeden element docelowy wykonywanie kopii zapasowej można skalować i przepustowość kopii zapasowej jest większa. To doprowadziłaby następnie wielu plików także na koncie magazyn Azure. W naszym badania przy użyciu wielu plików miejsc docelowych jedną ostatecznie osiągnięcia przepustowość, które jedną mógł osiągnąć z kopii zapasowej rozszerzenia realizowane w z programu SQL Server 2012 z dodatkiem SP1 CU4 na. Możesz również nie są zablokowane przez limit 1TB, tak jak w natywnej kopii zapasowej do Azure.

Należy jednak pamiętać, przepustowość jest również zależą od lokalizacji konta magazynu platformy Azure za pomocą kopii zapasowej. Uzyskać ogólny obraz może być Znajdź konto miejsca do magazynowania w innym regionie, niż maszyny wirtualne działają w. Np. chcesz uruchomić konfigurację maszyn wirtualnych w Europie Zachód, ale umieścić konta miejsca do magazynowania, które umożliwiają wykonywanie kopii zapasowej przed w Europie północnej. Który na pewno ma wpływ na przepustowość kopii zapasowej i nie może wygenerować przepustowości 150MB/sec według wydaje się w celu umożliwienia w przypadkach, gdzie magazynowania docelowej i maszyny wirtualne są uruchomione w tym samym regionalnym centrum danych.

#### <a name="managing-backup-blobs"></a>Zarządzanie obiektami blob kopii zapasowej
Jest wymagane do zarządzania kopie zapasowe na własny. Ponieważ oczekuje się, że wiele obiektów blob zostanie utworzona przez wykonywanie kopii zapasowej dziennika transakcji częste, zarządzanie tymi obiektami blob można łatwo przeciążać Azure Portal. Dlatego jest recommendable je wykorzystać Eksplorator magazynu Azure. Istnieje kilka warto z nich dostępne, które ułatwiają zarządzanie kontem Azure miejsca do magazynowania

* Microsoft Visual Studio z Azure SDK zainstalowany (<https://azure.microsoft.com/downloads/>)
* Eksplorator magazynu platformy Microsoft Azure (<https://azure.microsoft.com/downloads/>)
* 3 narzędzia innych firm

[komentarz]: <>  (Nie są jeszcze obsługiwane w CHMURZE)
[komentarz]: <>  (### Kopii zapasowych maszyn wirtualnych azure)
[komentarz]: <>  (Maszyny wirtualne w ramach systemu SAP kopię zapasową można za pomocą funkcji Kopia zapasowa maszyn wirtualnych Azure. Azure kopii zapasowej maszyn wirtualnych masz wprowadzone wcześniej w roku 2015 i w międzyczasie jest standardowa metoda wykonywania kopii zapasowej wykonania maszyn wirtualnych w Azure. Azure kopii zapasowej przechowuje kopii zapasowych w Azure i umożliwia przywracanie maszyny ponownie.) 
[komentarz]: <> (maszyny wirtualne, które uruchomienia bazy danych można zostaną utworzone kopie w sposób ciągły także systemy bazami danych obsługują VSS systemu Windows (głośność usługi kopiowania w tle - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>), jak np. programu SQL Server. Aby korzystanie z kopii zapasowych maszyn wirtualnych Azure może być sposób uzyskiwania dostępu do umożliwiająca przywrócenie kopii zapasowej bazy danych SAP. Należy jednak pamiętać, oparty na kopie zapasowe maszyn wirtualnych Azure, które w chwili przywraca baz danych nie jest możliwe. W związku z tym zaleca się wykonywanie kopii zapasowych baz danych z funkcjami bazami danych, a nie Polegaj na kopii zapasowych maszyn wirtualnych Azure.) [komentarz]: <> (Aby zapoznać się z kopii zapasowych maszyn wirtualnych Azure Uruchom tutaj <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Używanie obrazów programu SQL Server poza Microsoft Azure Marketplace
Firma Microsoft udostępnia maszyny wirtualne w Azure Marketplace, które już znajdują się wersje programu SQL Server. SAP — klientów, którzy wymagają licencji dla programu SQL Server i systemu Windows może to być możliwość zasadniczo obejmuje potrzebę licencji przez Obracająca się maszyny wirtualne z zainstalowanym programem SQL Server. Aby można było użyć tych obrazów SAP, należy wykonać następujące kwestie:

* Wersje programu SQL Server nie oceny uzyskiwanie wyższych kosztów niż tylko "Tylko do systemu Windows" maszyny wdrożony z usługi Azure Marketplace. Zobacz następujące artykuły do porównywania cen: <https://azure.microsoft.com/pricing/details/virtual-machines/> i <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Można używać tylko wersje programu SQL Server, które są obsługiwane przez SAP, takie jak programu SQL Server 2012.
* Sortowanie wystąpienie programu SQL Server, która jest instalowana w maszyny wirtualne, dostępnej w Azure Marketplace nie jest sortowanie SAP NetWeaver wymaga wystąpienie programu SQL Server, aby uruchomić. Możesz zmienić sortowanie, chociaż z instrukcjami w następnej sekcji.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Zmienianie sortowania serwera SQL programu Microsoft Windows i SQL Server maszyn wirtualnych
Ponieważ obrazy programu SQL Server Azure Marketplace nie są skonfigurowane do za pomocą sortowania, która jest wymagana przez aplikacje SAP NetWeaver, musi być zmieniane od razu po wdrożeniu. Dla programu SQL Server 2012 można to zrobić, wykonując następujące kroki zaraz po wdrożeniu maszyn wirtualnych i administrator może zalogować się do wdrożonym maszyn wirtualnych:

* Otwórz okno poleceń systemu Windows "Administrator".
* Zmienić katalogu C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Wykonywanie polecenia: Setup.exe/quiet Action = REBUILDDATABASE InstanceName = parametr/SQLSYSADMINACCOUNTS MSSQLSERVER =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> konta, które zostało zdefiniowane za pomocą konta administratora, wdrażając maszyn wirtualnych po raz pierwszy za pośrednictwem galerii.

Ten proces trwa tylko kilka minut. Aby upewnić się, czy krok zakończone w górę z poprawny wynik, należy wykonać następujące czynności:

* Otwórz program SQL Server Management Studio.
* Otwórz okno kwerendy.
* Wykonywanie sp_helpsort polecenie wzorca bazy danych programu SQL Server.

Oczekiwany wynik powinna wyglądać podobnie do:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Jeśli nie jest wynikiem, Zatrzymaj wdrażanie SAP i sprawdzić, dlaczego polecenie ustawienia nie działa zgodnie z oczekiwaniami. Wdrażanie aplikacji SAP NetWeaver na wystąpienie programu SQL Server z innego programu SQL Server strony kodowe niż wymienione powyżej **nie** jest obsługiwane.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>Program SQL Server wysokiej dostępności dla SAP platformy Azure
Jak wspomniano wcześniej w tym dokumencie, istnieje możliwość tworzenia udostępnionej przestrzeni dyskowej, które są niezbędne do zastosowania funkcji wysokiej dostępności najstarszych programu SQL Server. Ta funkcja zainstalować dwie lub więcej wystąpienia programu SQL Server w systemie Windows Server pracy awaryjnej klaster (WSFC) przy użyciu udostępnionym dysku dla baz danych użytkowników (i ewentualnie tymczasowe). Jest metoda standardowej wysokiej dostępności dłuższego czasu, która jest obsługiwana przez firmę SAP. Ponieważ Azure nie obsługuje udostępnionej przestrzeni dyskowej, nie zrealizowane szybkiej konfiguracji programu SQL Server konfiguracji klaster udostępnionym dysku. Jednak wielu innych metod wysokiej dostępności są nadal dostępne i opisane w poniższych sekcjach.

[komentarz]: <>  (Artykuł nadal odwołuje się do ASM)
[komentarz]: <>  (Przed czytania technologii różnych określonych wysoką dostępność dla programu SQL Server w Azure, jest bardzo dobra dokument, co daje bardziej szczegółowe informacje i wskaźniki [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>Wysyłki dziennika programu SQL Server
Jedną z metod wysokiej dostępności (HA) jest wysyłki programu SQL Server dziennika. Jeśli rozpoznawanie nazw działa maszyny wirtualne, uczestniczenie w konfiguracji HA, problem nie występuje i konfiguracji platformy Azure nie różnią się od dowolnego Instalatora, w której jest wykonywane w lokalnej. Zależne tylko rozpoznawanie adresów IP nie jest zalecane. W przypadku konfigurowania dziennika wysyłki i zasady wokół wysyłki dziennika Sprawdź ta dokumentacja:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Aby osiągnąć naprawdę dowolnej szybkiej, należy wdrożyć maszyny wirtualne, które znajdują się w takich wysyłki dziennika konfigurację do znajdować się w tym samym Azure dostępność zestawu.

#### <a name="database-mirroring"></a>Odzwierciedlające bazy danych
Lustrzane bazy danych jako obsługiwane przez SAP (zobacz Uwaga systemu SAP [965908]) zależy od tego, definiując partner pracy awaryjnej w parametrach połączenia SAP. W przypadku lokalnej między przyjęto założenie, że dwóch maszyny wirtualne znajdują się w tej samej domeny i że kontekst użytkownika dwa wystąpienia programu SQL Server, są uruchomione w są również użytkownicy domeny i masz wystarczających uprawnień w dwóch związane wystąpienia programu SQL Server. W związku z tym ustawienia odzwierciedlające bazy danych platformy Azure nie różnią się typowe lokalnego i konfiguracji.

Od wdrożeń tylko do chmury najprostszym metody ma innego konfigurowania domeny platformy Azure te maszyny wirtualne bazami danych (i najlepiej dedykowane SAP maszyny wirtualne) w jednej z tych domen.

Jeśli domena nie jest możliwe, jedną również użyć certyfikatów dla bazy danych, odzwierciedlające punkty końcowe w sposób opisany poniżej: <https://technet.microsoft.com/library/ms191477.aspx>

Samouczek do konfiguracji odzwierciedlające bazy danych platformy Azure można znaleźć tutaj: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>(AlwaysOn)
(AlwaysOn) jest obsługiwane dla SAP lokalnego (zobacz Uwaga systemu SAP [1772688]), jest obsługiwana może być używany w połączeniu z SAP w Azure. Fakt, że nie jest możliwe do tworzenia dysków udostępnionych w Azure nie oznacza, że nie można utworzyć jedną konfiguracji klaster pracy awaryjnej serwera (AlwaysOn) systemu Windows (WSFC) między różne maszyny wirtualne. Oznacza jedynie, nie ma możliwości zastosowania udostępnionym dysku jako kworum w konfiguracji klaster. W związku z tym można utworzyć konfiguracji WSFC (AlwaysOn) platformy Azure i po prostu nie wybierz odpowiedni typ kworum wykorzystuje udostępnionym dysku. Środowisko Azure te maszyny wirtualne wdrożonych w należy Rozwiąż maszyny wirtualne według nazwy i maszyny wirtualne powinien być na tej samej domeny. Dotyczy to tylko Azure i wdrożenia lokalne krzyżowe. Istnieje kilka zagadnień specjalnych wokół wdrażania programu SQL Server Dostępność grupy odbiornika (nie należy mylić z zestawem dostępność Azure) od Azure w tym momencie nie zezwala na po prostu utworzyć obiekt AD-DNS, jak to możliwe w lokalnej. Dlatego pewne czynności innej instalacji są niezbędne w celu wyeliminowania szczególne zachowanie Azure.

Zagadnienia za pomocą detektor grupy dostępności są:

* Za pomocą detektor grupy dostępność jest możliwe tylko z systemem Windows Server 2012 lub Windows Server 2012 R2 jako gość OS maszyn wirtualnych. Dla systemu Windows Server 2012 należy upewnić się, że zastosowano tę poprawkę: <https://support.microsoft.com/kb/2854082> 
* Dla systemu Windows Server 2008 R2 nie istnieje tę poprawkę i (AlwaysOn) czy trzeba używać w taki sam sposób jak odzwierciedlające bazy danych, określając partner pracy awaryjnej w parametrach połączenia (wykonywane za pomocą default.pfl parametru dbs/mss/serwerem SAP — zobacz Uwaga systemu SAP [965908]).
* Podczas korzystania z detektor grupy dostępność maszyny wirtualne bazy danych muszą być połączony z dedykowane równoważenia obciążenia. Rozpoznawanie nazw we wdrożeniach tylko do chmury albo wymaga wszystkie maszyny wirtualne systemu SAP (aplikacji serwerów, bazami danych serwera i () SCS serwera) znajdują się w tej samej sieci wirtualnej lub wymaga z warstwy aplikacji SAP konserwacji etc\host pliku, aby można było uzyskiwać nazwy maszyn wirtualnych maszyny wirtualne serwera SQL, które zostały rozwiązane. Aby uniknąć, czy Azure przypisuje nowych adresów IP w przypadku obu maszyny wirtualne niektórych zamknięcia, jedną przypisywania statyczne adresów IP dla interfejsów sieci maszyny wirtualne w konfiguracji (AlwaysOn) (definiowania statycznego adresu IP opisanej w [tym] [ virtual-networks-reserved-private-ip] artykuł) [komentarz]: <> (stare blogów) [komentarz]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Istnieją specjalne czynności wymagane w przypadku tworzenia konfiguracji klaster WSFC, których klaster wymaga specjalnych adres IP przypisany, ponieważ Azure z jego bieżącą funkcję chcesz przypisać klaster nazwę tego samego adresu IP przy tworzeniu węzeł klaster na. Oznacza to, że ręczne kroku należy wykonać, aby przypisać różne adresy IP z klastrem.
* Dostępność odbiornika grupy ma zostać utworzone w Azure z punktów końcowych TCP/IP, które są przypisane do maszyny wirtualne z głównego i pomocniczego repliki grupy dostępności.
* Może być potrzebne do zabezpieczenia te punkty końcowe przy użyciu list ACL.

[komentarz]: <>  (Blog stare zadania)
[komentarz]: <>  (Szczegółowe kroki i artykuły pierwszej potrzeby instalowania konfiguracji (AlwaysOn) Azure najlepiej doświadczonych po Instruktaż samouczka dostępne [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[komentarz]: <>  (Konfiguracji wstępnie (AlwaysOn) za pośrednictwem galerii Azure < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[komentarz]: <>  (Tworzenie detektor grupy dostępność jest najlepiej opisane w samouczku [this][virtual-machines-windows-classic-ps-sql-int-listener])
[komentarz]: <>  (Punkty końcowe sieci zabezpieczanie z list ACL zostały omówione najlepiej tutaj:)
[komentarz]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[komentarz]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[komentarz]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[komentarz]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Istnieje możliwość wdrożenia grupy dostępności (AlwaysOn) programu SQL Server w różnych regionów Azure także. Ta funkcja będzie używana łączności Azure VNet-Vnet ([więcej szczegółów][virtual-networks-configure-vnet-to-vnet-connection]).
[komentarz]: <> (blog stare zadania) [komentarz]: <> (instalacji programu SQL Server (AlwaysOn) Dostępność grupy w takiej sytuacji jest opisanych tutaj: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Podsumowanie na program SQL Server szybkiej platformy Azure
Uwzględniając fakt, że magazyn Azure chroni zawartości, jest mniej powodem trzymaj się obraz stałej gotowości. Oznacza to, że rozwiązania wysokiej dostępności wymaga tylko ochronę przed następujących przypadkach:

* Klaster niedostępności maszyn wirtualnych jako całość z powodu konserwacji na serwerze w Azure lub innych czynników
* Problemów dotyczących oprogramowania w wystąpienie programu SQL Server
* Ochrona przed ręcznego błędu miejsce, w którym dane otrzymuje usunięte i odzyskiwania w chwili jest wymagane

Przejrzenie pasujące technologii, którą jedną można argumentowało że dwóch pierwszych przypadkach mogą być objęte odzwierciedlające bazy danych lub (AlwaysOn), to trzecia tylko może być objęte wysyłanie dziennika.

Konieczne będzie saldo bardziej złożonej konfiguracji (AlwaysOn) i odzwierciedlające bazy danych, z zalet (AlwaysOn). Te korzyści mogą być wyświetlane, takich jak:

*   Czytelne repliki pomocniczą.
*   Kopie zapasowe z repliki pomocniczą.
*   Lepsza skalowalność.
*   Więcej niż jeden repliki pomocniczą.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Ogólne programu SQL Server dla SAP na podsumowanie Azure
Istnieje wiele zalecenia w tym przewodniku a zalecamy zapoznanie się z niniejszymi go więcej niż jeden raz przed zaplanowaniem Azure wdrożenia. Ogólnie, należy postępować górny dziesięć ogólne bazami danych na Azure konkretne punkty:

[komentarz]: <> przepustowość  (2.3 wyższymi niż co? Niż jeden wirtualny dysk twardy?)
1. Używać najnowszej wersji bazami danych, takich jak SQL Server 2014, zawierającego większość korzyści w Azure. W przypadku programu SQL Server jest SQL Server 2012 z dodatkiem SP1 CU4, które zawierałoby funkcji wykonywania kopii sprzętu magazyn Azure. Jednak w połączeniu z SAP zaleca co najmniej pakietu CU1 programu SQL Server 2014 z dodatkiem SP1 lub SQL Server 2012 z dodatkiem SP2 i najnowszych CU.
1. Starannie zaplanować usługi pozioma systemu SAP w Azure do układu plik danych i Azure ograniczenia:
    * Nie ma zbyt wiele wirtualnych dysków twardych, ale aby upewnić się, że przejdziesz do wymagane operacji i/o na SEKUNDĘ.
    * Należy pamiętać o operacji i/o na SEKUNDĘ są również ograniczone na konta na platformie Azure i że konta przestrzeni dyskowej są ograniczone w każdej subskrypcji Azure ([więcej szczegółów][azure-subscription-service-limits]). 
    * Tylko pasek wielu wirtualnych dysków twardych, jeśli musisz osiągnięcia wyższej przepustowości.
1. Nigdy nie instaluj oprogramowania lub umieszczenie plików, które wymagają pozostawanie na dysku D:\ jest wartością stałe i niczego na dysku zostaną utracone w systemie Windows, uruchom ponownie komputer.
1. Nie używaj Azure wirtualnego dysku twardego buforowanie Azure standardowego magazynu.
1. Nie używaj Azure replikowane geo kont miejsca do magazynowania.  Używanie lokalnie zbędne dla obciążenia bazami danych.
1. Replikacji bazy danych za pomocą rozwiązanie HA/DR dostawcą bazami danych.
1. Zawsze używać rozpoznawania nazw, nie Polegaj na adresy IP.
1. Za pomocą najwyższe możliwe kompresji bazy danych. W przypadku programu SQL Server jest kompresji strony.
1. Należy zachować ostrożność, przy użyciu programu SQL Server obrazy z usługi Azure Marketplace. Jeśli korzystasz z programu SQL Server, jedną, należy zmienić sortowanie wystąpienia przed zainstalowaniem żadnego systemu SAP NetWeaver nad nim.
1. Instalowanie i Konfigurowanie hosta SAP dla Azure monitorowania, zgodnie z opisem w [Podręcznik wdrażania] [Podręcznik wdrażania].

## <a name="specifics-to-sap-ase-on-windows"></a>Szczegóły do ASE SAP w systemie Windows
Począwszy od Microsoft Azure, możesz łatwo przeprowadzić migrację istniejącej aplikacji SAP ASE do maszyn wirtualnych Azure. ASE SAP w maszyny wirtualnej pozwala zmniejszyć całkowity koszt własność wdrażanie, zarządzanie i konserwacja popularności szerokość przez łatwe przeprowadzenie migracji tych aplikacji Microsoft Azure. Z ASE SAP w maszyn wirtualnych Azure Administratorzy i deweloperzy nadal można używać w tej samej sytuacji i narzędzi administracyjnych, które są dostępne w lokalnym.

Istnieje SLA maszyn wirtualnych Azure których można znaleźć tutaj: <https://azure.microsoft.com/support/legal/sla>

Pracujemy pewność, że mogą się różnić Microsoft Azure hostowanej maszyn wirtualnych wykona dobrze w porównaniu z systemem innym publicznej chmury wirtualizacji ofert, ale poszczególnych wyników. SAP w celu zmiany rozmiaru protokoły SAP liczby różnych SAP uwierzytelnione SKU maszyn wirtualnych znajdzie się w oddzielnym Uwaga systemu SAP [1928533].

Instrukcje i zalecenia dotyczące użycia magazyn Azure, wdrożenie systemu SAP maszyny wirtualne lub SAP w celu monitorowania dotyczą wdrożeń SAP ASE w połączeniu z aplikacji SAP podaną w całym pierwsze cztery rozdziały tego dokumentu.

### <a name="sap-ase-version-support"></a>Obsługa wersji ASE SAP 
SAP obecnie obsługuje ASE SAP wersji 16.0 do użytku z produktami pakietu firm SAP. Wszystkie aktualizacje dla SAP ASE server lub sterowniki JDBC i ODBC może być używany z produktami pakietu firm SAP są udostępniane wyłącznie przez Marketplace usługi SAP w: <https://support.sap.com/swdc>.

Jak w przypadku instalacji lokalnego, nie pobieraj aktualizacje dla serwera SAP ASE lub sterowniki ODBC i JDBC bezpośrednio z witryny sieci Web programu Sybase. Szczegółowe informacje na temat poprawek, które są obsługiwane do użytku z pakietu firm SAP produktów lokalnej i w maszyn wirtualnych Azure, zobacz poniższe uwagi SAP:

* [1590719]
* [1973241]
 
Ogólne informacje na temat uruchomionych pakietu firm SAP SAP ASE znajdują się w [usłudze SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Wskazówki dotyczące konfigurowania ASE SAP dla SAP dotyczące instalacji ASE SAP w maszyny wirtualne Azure

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktura wdrażania ASE SAP
Zgodnie z ogólnym opisem pliki wykonywalne SAP ASE powinny być znajduje się lub zainstalowany na dysku systemowym maszyn podstawowej wirtualny dysk twardy (dysk c:\). Zazwyczaj większość SAP ASE systemu i narzędzia bazy danych są nie naprawdę osobie całkowite obciążenie pracą SAP NetWeaver. W związku z tym system i narzędzia bazy danych (wzorca, model, saptools, sybmgmtdb, sybsystemdb) może pozostać na C:\drive także. 

Wyjątków może być tymczasowe bazę danych zawierającą wszystkie tabele pracy i tymczasowe tabele utworzone przez ASE SAP, które w przypadku niektórych ERP SAP i wszystkie obciążenia PC może wymagać większą ilość danych lub głośność operacje wejścia/wyjścia, która nie mieści się w oryginalnym maszyn wirtualnych podstawowej wirtualnego dysku twardego (dysk c:\).
 
W zależności od wersji SAPInst-SWPM użyte do zainstalowania systemu bazy danych może zawierać:

* Pojedynczy tymczasowe SAP ASE, który jest tworzony podczas instalowania SAP ASE
* Tymczasowe SAP ASE utworzone przez instalowania SAP ASE i dodatkowe saptempdb utworzone przez procedurę instalacji systemu SAP
* Tymczasowe SAP ASE utworzone przez instalowania SAP ASE i dodatkowe tymczasowe, który został utworzony ręcznie (np. następujące Uwaga systemu SAP [1752266]) do wymagań określonych tymczasowe ERP-PC

W przypadku ERP określonych lub wszystkich obciążenia Przepustowości jest to właściwe rozwiązanie, dotyczących wydajności, aby zachować urządzeń tymczasowe Ponadto utworzonych tymczasowe (przez SWPM lub ręcznie) na dysku niż C:\. Jeśli nie dodatkowych tymczasowe istnieje go zaleca się utworzenie (Uwaga systemu SAP [1752266]).

Dla takich układów Ponadto utworzonych tymczasowe należy przeprowadzić następujące czynności:

* Przenoszenie pierwszym urządzeniu tymczasowe pierwsze urządzenie SAP bazy danych
* Dodawanie urządzeń tymczasowe do wszystkich wirtualnych dysków twardych zawierające urządzeń SAP bazy danych

Konfiguracja ta pozwala tymczasowe albo wykorzystać więcej miejsca niż dysku systemowym jest możliwość udostępnienia. Jako jeden Sprawdź rozmiarów urządzenia tymczasowe na istniejących systemów, które są uruchamiane w lokalnej. Lub taka konfiguracja umożliwia liczby operacji i/o na SEKUNDĘ przed tymczasowe, której nie można przedstawić z dysku systemowym. Ponownie systemów, które są uruchomione w lokalnym można monitorować obciążenie We/Wy przed tymczasowe.

Nigdy nie umieszczaj wszystkich urządzeń SAP ASE na dysku D:\ maszyn wirtualnych. Dotyczy to również do tymczasowe, nawet jeśli obiekty na tymczasowe są tylko tymczasowe.

#### <a name="impact-of-database-compression"></a>Wpływ kompresji bazy danych
W konfiguracjach miejsce, w którym przepustowości we/wy może stać się ograniczanie czynniki każdej miary, zmniejszając operacji i/o na SEKUNDĘ mogą ułatwić aby rozciągnąć obciążenie pracą, które jedną można uruchamiać w scenariuszu IaaS, takich jak Azure. Dlatego zdecydowanie zaleca się upewnij się, że kompresji SAP ASE jest używany przed przekazaniem z istniejącą bazą danych SAP Azure.

Zalecenie wykonaj kompresję przed przekazaniem Azure, jeśli nie jest zaimplementowana znajduje się z kilku powodów:

* Ilość danych do przekazania Azure jest niższa
* Czas realizacji kompresji jest krótsza przy założeniu, że użyć można silniejsze sprzętu z więcej procesorów lub większej przepustowości we/wy lub mniej we/wy opóźnienie lokalnego
* Mniejszy rozmiar bazy danych może prowadzić do mniejszych koszty przydziału dysku

Kompresja danych i LOB działają w maszyny hostowana w maszyn wirtualnych Azure, jak lokalnego. Aby uzyskać więcej informacji na temat sprawdzania, jeśli kompresji jest już używana w istniejącej bazie danych SAP ASE Sprawdź, czy Uwaga systemu SAP [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Monitorowanie wystąpień bazy danych za pomocą DBACockpit
Dla systemów SAP, które używają SAP ASE jako platformy bazy danych DBACockpit jest dostępny, jako okna przeglądarki osadzony w transakcji DBACockpit lub Webdynpro. Jednak pełny zestaw funkcji monitorowania i zarządzania bazy danych jest dostępna w ramach Webdynpro stosowania DBACockpit tylko.

Jako z systemami lokalnego kilka czynności, aby włączyć wszystkie funkcje SAP NetWeaver używane przez Webdynpro stosowania DBACockpit. Wykonaj Uwaga systemu SAP [1245200] włączyć zastosowania webdynpros i generowanie wymagane z nich. Gdy postępując zgodnie z instrukcjami powyżej notatek także skonfigurujesz Menedżera komunikacji internetowej (dopasowanie kolorów) wraz z portów ma być używany dla połączeń http i https. Ustawieniem domyślnym dla protokołu http wygląda następująco:

> dopasowanie kolorów i server_port_0 = PROT = HTTP, PORT = 8000, PROCTIMEOUT = 600, limit czasu = 600
>
> dopasowanie kolorów i server_port_1 = PROT = HTTPS, PORT = $$ 443, PROCTIMEOUT = 600, limit czasu = 600

i łącza wygenerowane w transakcji DBACockpit będzie wyglądać podobnie do następującej:

> https://`<fullyqualifiedhostname`>: 44300/sap-bc-webdynpro-sap-dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000-sap-bc-webdynpro-sap-dba_cockpit

W zależności od czy i jak maszyna wirtualna Azure hostingu systemu SAP jest połączony za pośrednictwem witryny do witryny, wielu witryny lub ExpressRoute (między lokalne wdrożenie) należy upewnij się, że dopasowanie kolorów używanej w pełni kwalifikowana nazwa hosta, która może być przetłumaczona na komputerze miejsce, w którym chcesz otworzyć DBACockpit z. Zobacz Uwaga systemu SAP [773830] , aby dowiedzieć się, jak dopasowanie kolorów określa nazwa hosta w pełni kwalifikowana, w zależności od profilu i ustawić parametr dopasowanie kolorów i host_name_full jawnie w razie potrzeby.

Jeśli wdrożono maszyn wirtualnych w scenariuszu tylko do chmury bez połączenia między lokalnej między Azure i lokalnych, należy zdefiniować publiczny adres IP i domainlabel. Format nazwy DNS publicznej maszyn wirtualnych następnie będzie wyglądać podobnie do następującej:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Więcej szczegółów dotyczących nazwy DNS można znaleźć [tutaj][virtual-machines-azurerm-versus-azuresm].

Ustawienie parametru profilu SAP dopasowanie kolorów i host_name_full na nazwę DNS maszyn wirtualnych Azure łącza mogą wyglądać podobnie do:

> https://mydomainlabel.westeurope.cloudapp.NET:44300-sap-bc-webdynpro-sap-dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.NET:8000-sap-bc-webdynpro-sap-dba_cockpit

W tym przypadku należy upewnij się, że:

* Dodawanie reguły przychodzące do grupy zabezpieczeń sieci w Portal Azure dla portów TCP/IP umożliwia komunikowanie się za pomocą dopasowanie kolorów
* Dodawanie reguły przychodzące do konfiguracji zapory systemu Windows dla portów TCP/IP umożliwia komunikowanie się za pomocą dopasowanie kolorów

Automatyczne zaimportowane wszystkich poprawek dostępnych zalecane jest okresowo stosowanie zbioru korekty SAP notatki dotyczące wersji SAP:

* [1558958]
* [1619967]
* [1882376]

Dodatkowe informacje o panelu sterowania Zarejestrowana dla SAP ASE można znaleźć w następujących notatek SAP:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Wykonywanie kopii zapasowych i odzyskiwania zagadnienia związane z SAP ASE
Wdrażając SAP ASE w Twojej kopii zapasowej metodologii należy przejrzeć Azure. Nawet jeśli system nie jest wydajność systemu, SAP obsługiwanych przez SAP ASE musi być kopię zapasową bazy danych okresowo. Ponieważ Magazyn Azure zachowuje trzy obrazy, kopii zapasowej jest teraz mniej ważne w odniesieniu do wyrównywania awarię miejsca do magazynowania. Głównym celem utrzymania właściwy plan kopia zapasowa i przywracanie jest więcej, który można zadośćuczynienia błędów logicznych i ręczne, dostarczając punktu w funkcji odzyskiwania czasu. Aby celem jest albo użyj wykonywanie kopii zapasowych przywrócenia bazy danych do określonego punktu w czasie za pomocą kopii zapasowych w Azure zapełnić innego systemu, kopiując istniejącej bazy danych. Na przykład użytkownik może przekazać z poziomu 2 konfiguracji systemu SAP do konfiguracji systemu poziomu 3 tego samego systemu przywrócenia kopii zapasowej.

Wykonywanie kopii zapasowych i przywracanie bazy danych platformy Azure działa tak samo jak lokalnego. Zobacz uwagi SAP:

* [1588316]
* [1585981]

Aby uzyskać szczegółowe informacje na temat tworzenia konfiguracji zrzutu i planowania wykonywania kopii zapasowych. W zależności od usługi strategii i wymagań, możesz skonfigurować na dysku na jedną z istniejących wirtualnych dysków twardych lub dodać dodatkowe wirtualnego dysku twardego kopii zapasowej dokonuje zrzutu bazy danych i dziennika.  W celu zmniejszenia zagrożenia utratą danych w przypadku błędu zaleca się używanie wirtualnego dysku twardego miejsce, w którym znajduje się żadne urządzenie bazy danych.

Oprócz danych i LOB kompresji SAP ASE oferuje kompresji kopii zapasowej. Zajmować mniej miejsca z bazy danych i dziennika zrzuty zaleca się używanie kompresji kopii zapasowej. Uwaga systemu SAP [1588316] , aby uzyskać więcej informacji, zobacz. Kompresowanie kopii zapasowej jest również istotne w celu zmniejszenia ilości danych, które mają zostać przeniesione, jeśli zamierzasz pobrać kopie zapasowe lub wirtualnych dysków twardych zawierające kopii zapasowej zrzuty z maszyny wirtualnej Azure do lokalnego.

Nie należy używać dysku D:\ jako miejsca docelowego zrzutu bazy danych lub dziennika.

#### <a name="performance-considerations-for-backupsrestores"></a>Zagadnienia dotyczące wydajności do wykonywania kopii zapasowych i przywracanie
Jak wdrożeń zera wydajności tworzenia kopii zapasowych i przywracania jest uzależniony od ilości ile można odczytywać równolegle i przepustowość tych wielkości, co można. Ponadto użycie Procesora używane przez kompresji kopii zapasowej mogą być odtwarzane znaczącą rolę na maszyny wirtualne z wątkami Procesora tylko do 8. Dlatego jedną Załóżmy:

* Mniejsza liczba wirtualnych dysków twardych umożliwia przechowywanie urządzenia bazy danych mniejszych ogólną przepustowość w odczytu
* Mniejsza liczba procesorów wątki w Głosowa, bardziej poważne wpływ kompresja kopii zapasowej
* Cele mniej (pasek katalogów, wirtualnych dysków twardych) do zapisywania kopii zapasowej, mniejszy przepustowość

Aby zwiększyć liczbę elementów docelowych do zapisu czy istnieją dwie opcje, które mogą być używane i połączone w zależności od potrzeb:

* Rozkładanie wielkość docelowy kopii zapasowej na wielu wirtualnych dysków twardych zainstalowanych w celu poprawienia wydajności operacji i/o na SEKUNDĘ na rozłożone woluminu
* Tworzenie konfiguracji zrzutu na poziomie SAP ASE, który używa więcej niż jeden katalog docelowy napisać zrzutu do

Rozkładanie woluminu przez wielu zainstalowanego wirtualnych dysków twardych zostały omówione wcześniej w tym przewodniku. Aby uzyskać więcej informacji na temat korzystania z wielu katalogów w konfiguracji zrzutu SAP ASE zapoznaj się z dokumentacją w sp_config_dump procedura przechowywana, który jest używany do tworzenia konfiguracji zrzutu w [Centrum informacyjne Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Odzyskiwanie z Azure maszyny wirtualne

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikacja danych z serwerem replikacji programu Sybase SAP
Z ASE SAP SAP Sybase replikacji serwera (SRS) stanowi ciepłą wstrzymania rozwiązanie transakcje bazy danych do lokalizacji odległości asynchroniczne. 

Instalacji i działania SRS działa także funkcjonalny maszyny hostowana w usługach maszyn wirtualnych Azure tak samo jak lokalnego.

HADR ASE za pośrednictwem serwera replikacji SAP jest planowane z przyszłej wersji. Zostanie ono z i zwrócone dla platformy Microsoft Azure, w momencie, gdy jest on dostępny.

## <a name="specifics-to-sap-ase-on-linux"></a>Szczegóły do ASE SAP w systemie Linux

Począwszy od Microsoft Azure, możesz łatwo przeprowadzić migrację istniejącej aplikacji SAP ASE do maszyn wirtualnych Azure. ASE SAP w maszyny wirtualnej pozwala zmniejszyć całkowity koszt własność wdrażanie, zarządzanie i konserwacja popularności szerokość przez łatwe przeprowadzenie migracji tych aplikacji Microsoft Azure. Z ASE SAP w maszyn wirtualnych Azure Administratorzy i deweloperzy nadal można używać w tej samej sytuacji i narzędzi administracyjnych, które są dostępne w lokalnym.

Do wdrażania maszyny wirtualne Azure ważne jest, aby ustalić oficjalnym zwiększany, które można znaleźć tutaj: <https://azure.microsoft.com/support/legal/sla>

Informacje dotyczące zmiany rozmiaru SAP i listą certyfikowane SKU maszyn wirtualnych systemu SAP będzie dostępna w Uwaga systemu SAP [1928533]. Dodatkowe SAP w celu zmiany rozmiaru dokumentów do maszyn wirtualnych Azure można znaleźć tutaj <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> i tutaj <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Instrukcje i zalecenia dotyczące użycia magazyn Azure, wdrożenie systemu SAP maszyny wirtualne lub SAP w celu monitorowania dotyczą wdrożeń SAP ASE w połączeniu z aplikacji SAP podaną w całym pierwsze cztery rozdziały tego dokumentu.

Następujące dwie notatki SAP zawiera ogólne informacje o ASE Linux i ASE w chmurze:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Obsługa wersji ASE SAP 
SAP obecnie obsługuje ASE SAP wersji 16.0 do użytku z produktami pakietu firm SAP. Wszystkie aktualizacje dla SAP ASE server lub sterowniki JDBC i ODBC może być używany z produktami pakietu firm SAP są udostępniane wyłącznie przez Marketplace usługi SAP w: <https://support.sap.com/swdc>.

Jak w przypadku instalacji lokalnego, nie pobieraj aktualizacje dla serwera SAP ASE lub sterowniki ODBC i JDBC bezpośrednio z witryny sieci Web programu Sybase. Szczegółowe informacje na temat poprawek, które są obsługiwane do użytku z pakietu firm SAP produktów lokalnej i w maszyn wirtualnych Azure, zobacz poniższe uwagi SAP:

* [1590719]
* [1973241]
 
Ogólne informacje na temat uruchomionych pakietu firm SAP SAP ASE znajdują się w [usłudze SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Wskazówki dotyczące konfigurowania ASE SAP dla SAP dotyczące instalacji ASE SAP w maszyny wirtualne Azure

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktura wdrażania ASE SAP
Zgodnie z ogólnym opisem pliki wykonywalne SAP ASE należy znajduje się lub zainstalowany w systemie plików głównego maszyn wirtualnych (/sybase). Zazwyczaj większość SAP ASE systemu i narzędzia bazy danych są nie naprawdę osobie całkowite obciążenie pracą SAP NetWeaver. W związku z tym system i narzędzia bazy danych (wzorca, model, saptools, sybmgmtdb, sybsystemdb) może pozostać w systemie plików głównego także. 

Wyjątków może być tymczasowe bazę danych zawierającą wszystkie tabele pracy i tymczasowe tabele utworzone przez ASE SAP, które w przypadku niektórych ERP SAP i wszystkie obciążenia PC może wymagać większą ilość danych lub głośność operacje wejścia/wyjścia, która nie mieści się na oryginalnym maszyn wirtualnych systemu operacyjnego dysk.
 
W zależności od wersji SAPInst-SWPM użyte do zainstalowania systemu bazy danych może zawierać:

* Pojedynczy tymczasowe SAP ASE, który jest tworzony podczas instalowania SAP ASE
* Tymczasowe SAP ASE utworzone przez instalowania SAP ASE i dodatkowe saptempdb utworzone przez procedurę instalacji systemu SAP
* Tymczasowe SAP ASE utworzone przez instalowania SAP ASE i dodatkowe tymczasowe, który został utworzony ręcznie (np. następujące Uwaga systemu SAP [1752266]) do wymagań określonych tymczasowe ERP-PC

W przypadku ERP określonych lub wszystkich obciążenia Przepustowości jest to właściwe rozwiązanie, dotyczących wydajności, aby zachować urządzeń tymczasowe Ponadto utworzonych tymczasowe (przez SWPM lub ręcznie) w systemie osobny plik, który może być reprezentowany przez dysk pojedynczego Azure danych lub RAID Linux obejmujące wiele dysków Azure danych. Jeśli nie dodatkowe tymczasowe istnieje go zaleca się utworzenie (Uwaga systemu SAP [1752266]).

Dla takich układów Ponadto utworzonych tymczasowe należy przeprowadzić następujące czynności:

* Przenoszenie pierwszego katalogu tymczasowe do pierwszego systemu plików systemu SAP bazy danych
* Dodawanie katalogi tymczasowe do wszystkich wirtualnych dysków twardych zawierające plików bazy danych SAP

Konfiguracja ta pozwala tymczasowe albo wykorzystać więcej miejsca niż dysku systemowym jest możliwość udostępnienia. Jako jeden Sprawdź rozmiarów katalogu tymczasowe na istniejących systemów, które są uruchamiane w lokalnej. Lub taka konfiguracja umożliwia liczby operacji i/o na SEKUNDĘ przed tymczasowe, której nie można przedstawić z dysku systemowym. Ponownie systemów, które są uruchomione w lokalnym można monitorować obciążenie We/Wy przed tymczasowe.

Nigdy nie umieszczaj wszelkich katalogów SAP ASE do katalogu/mnt lub /mnt/resource maszyn wirtualnych. Dotyczy to również do tymczasowe, nawet jeśli obiekty na tymczasowe są tylko tymczasowe, ponieważ katalogu/mnt lub /mnt/resource jest domyślne maszyn wirtualnych Azure temp miejsca, która nie jest trwała. Więcej informacji na temat miejsca temp Azure maszyn wirtualnych można znaleźć w [tym artykule][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Wpływ kompresji bazy danych
W konfiguracjach miejsce, w którym przepustowości we/wy może stać się ograniczanie czynniki każdej miary, zmniejszając operacji i/o na SEKUNDĘ mogą ułatwić aby rozciągnąć obciążenie pracą, które jedną można uruchamiać w scenariuszu IaaS, takich jak Azure. Dlatego zdecydowanie zaleca się upewnij się, że kompresji SAP ASE jest używany przed przekazaniem z istniejącą bazą danych SAP Azure.

Zalecenie wykonaj kompresję przed przekazaniem Azure, jeśli nie jest zaimplementowana znajduje się z kilku powodów:

* Ilość danych do przekazania Azure jest niższa
* Czas realizacji kompresji jest krótsza przy założeniu, że użyć można silniejsze sprzętu z więcej procesorów lub większej przepustowości we/wy lub mniej we/wy opóźnienie lokalnego
* Mniejszy rozmiar bazy danych może prowadzić do mniejszych koszty przydziału dysku

Kompresja danych i LOB działają w maszyny hostowana w maszyn wirtualnych Azure, jak lokalnego. Aby uzyskać więcej informacji na temat sprawdzania, jeśli kompresji jest już używana w istniejącej bazie danych SAP ASE Sprawdź, czy Uwaga systemu SAP [1750510]. Zobacz też Uwaga systemu SAP [2121797] , aby uzyskać dodatkowe informacje dotyczące kompresji bazy danych.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Monitorowanie wystąpień bazy danych za pomocą DBACockpit
Dla systemów SAP, które używają SAP ASE jako platformy bazy danych DBACockpit jest dostępny, jako okna przeglądarki osadzony w transakcji DBACockpit lub Webdynpro. Jednak pełny zestaw funkcji monitorowania i zarządzania bazy danych jest dostępna w ramach Webdynpro stosowania DBACockpit tylko.

Jako z systemami lokalnego kilka czynności, aby włączyć wszystkie funkcje SAP NetWeaver używane przez Webdynpro stosowania DBACockpit. Wykonaj Uwaga systemu SAP [1245200] włączyć zastosowania webdynpros i generowanie wymagane z nich. Gdy postępując zgodnie z instrukcjami powyżej notatek także skonfigurujesz Menedżera komunikacji internetowej (dopasowanie kolorów) wraz z portów ma być używany dla połączeń http i https. Ustawieniem domyślnym dla protokołu http wygląda następująco:

> dopasowanie kolorów i server_port_0 = PROT = HTTP, PORT = 8000, PROCTIMEOUT = 600, limit czasu = 600
>
> dopasowanie kolorów i server_port_1 = PROT = HTTPS, PORT = $$ 443, PROCTIMEOUT = 600, limit czasu = 600

i łącza wygenerowane w transakcji DBACockpit będzie wyglądać podobnie do następującej:

> https://`<fullyqualifiedhostname`>: 44300/sap-bc-webdynpro-sap-dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000-sap-bc-webdynpro-sap-dba_cockpit

W zależności od czy i jak maszyna wirtualna Azure hostingu systemu SAP jest połączony za pośrednictwem witryny do witryny, wielu witryny lub ExpressRoute (między lokalne wdrożenie) należy upewnij się, że dopasowanie kolorów używanej w pełni kwalifikowana nazwa hosta, która może być przetłumaczona na komputerze miejsce, w którym chcesz otworzyć DBACockpit z. Zobacz Uwaga systemu SAP [773830] , aby dowiedzieć się, jak dopasowanie kolorów określa nazwa hosta w pełni kwalifikowana, w zależności od profilu i ustawić parametr dopasowanie kolorów i host_name_full jawnie w razie potrzeby.

Jeśli wdrożono maszyn wirtualnych w scenariuszu tylko do chmury bez połączenia między lokalnej między Azure i lokalnych, należy zdefiniować publiczny adres IP i domainlabel. Format nazwy DNS publicznej maszyn wirtualnych następnie będzie wyglądać podobnie do następującej:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Więcej szczegółów dotyczących nazwy DNS można znaleźć [tutaj][virtual-machines-azurerm-versus-azuresm].

Ustawienie parametru profilu SAP dopasowanie kolorów i host_name_full na nazwę DNS maszyn wirtualnych Azure łącza mogą wyglądać podobnie do:

> https://mydomainlabel.westeurope.cloudapp.NET:44300-sap-bc-webdynpro-sap-dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000-sap-bc-webdynpro-sap-dba_cockpit

W tym przypadku należy upewnij się, że:

* Dodawanie reguły przychodzące do grupy zabezpieczeń sieci w Portal Azure dla portów TCP/IP umożliwia komunikowanie się za pomocą dopasowanie kolorów
* Dodawanie reguły przychodzące do konfiguracji zapory systemu Windows dla portów TCP/IP umożliwia komunikowanie się za pomocą dopasowanie kolorów

Automatyczne zaimportowane wszystkich poprawek dostępnych zalecane jest okresowo stosowanie zbioru korekty SAP notatki dotyczące wersji SAP:

* [1558958]
* [1619967]
* [1882376]

Dodatkowe informacje o panelu sterowania Zarejestrowana dla SAP ASE można znaleźć w następujących notatek SAP:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Wykonywanie kopii zapasowych i odzyskiwania zagadnienia związane z SAP ASE
Wdrażając SAP ASE w Twojej kopii zapasowej metodologii należy przejrzeć Azure. Nawet jeśli system nie jest wydajność systemu, SAP obsługiwanych przez SAP ASE musi być kopię zapasową bazy danych okresowo. Ponieważ Magazyn Azure zachowuje trzy obrazy, kopii zapasowej jest teraz mniej ważne w odniesieniu do wyrównywania awarię miejsca do magazynowania. Głównym celem utrzymania właściwy plan kopia zapasowa i przywracanie jest więcej, który można zadośćuczynienia błędów logicznych i ręczne, dostarczając punktu w funkcji odzyskiwania czasu. Aby celem jest albo użyj wykonywanie kopii zapasowych przywrócenia bazy danych do określonego punktu w czasie za pomocą kopii zapasowych w Azure zapełnić innego systemu, kopiując istniejącej bazy danych. Na przykład użytkownik może przekazać z poziomu 2 konfiguracji systemu SAP do konfiguracji systemu poziomu 3 tego samego systemu przywrócenia kopii zapasowej.

Wykonywanie kopii zapasowych i przywracanie bazy danych platformy Azure działa tak samo jak lokalnego. Zobacz uwagi SAP:

* [1588316]
* [1585981]

Aby uzyskać szczegółowe informacje na temat tworzenia konfiguracji zrzutu i planowania wykonywania kopii zapasowych. W zależności od usługi strategii i wymagań, możesz skonfigurować na dysku na jedną z istniejących wirtualnych dysków twardych lub dodać dodatkowe wirtualnego dysku twardego kopii zapasowej dokonuje zrzutu bazy danych i dziennika.  W celu zmniejszenia zagrożenia utratą danych w przypadku błędu zaleca się używanie wirtualnego dysku twardego miejsce, w którym znajduje się żadnego katalogu i pliku bazy danych.

Oprócz danych i LOB kompresji SAP ASE oferuje kompresji kopii zapasowej. Zajmować mniej miejsca z bazy danych i dziennika zrzuty zaleca się używanie kompresji kopii zapasowej. Uwaga systemu SAP [1588316] , aby uzyskać więcej informacji, zobacz. Kompresowanie kopii zapasowej jest również istotne w celu zmniejszenia ilości danych, które mają zostać przeniesione, jeśli zamierzasz pobrać kopie zapasowe lub wirtualnych dysków twardych zawierające kopii zapasowej zrzuty z maszyny wirtualnej Azure do lokalnego.

Nie należy używać katalogu/mnt maszyn wirtualnych Azure temp miejsca lub /mnt/resource jako miejsca docelowego zrzutu bazy danych lub dziennika.

#### <a name="performance-considerations-for-backupsrestores"></a>Zagadnienia dotyczące wydajności do wykonywania kopii zapasowych i przywracanie
Jak wdrożeń zera wydajności tworzenia kopii zapasowych i przywracania jest uzależniony od ilości ile można odczytywać równolegle i przepustowość tych wielkości, co można. Ponadto użycie Procesora używane przez kompresji kopii zapasowej mogą być odtwarzane znaczącą rolę na maszyny wirtualne z wątkami Procesora tylko do 8. Dlatego jedną Załóżmy:

* Mniejsza liczba wirtualnych dysków twardych umożliwia przechowywanie urządzenia bazy danych mniejszych ogólną przepustowość w odczytu
* Mniejsza liczba procesorów wątki w Głosowa, bardziej poważne wpływ kompresja kopii zapasowej
* Mniejszej liczby elementów docelowych (Linux oprogramowania RAID, wirtualnych dysków twardych) do zapisywania kopii zapasowej, mniejszy przepustowość

Aby zwiększyć liczbę elementów docelowych do zapisu czy istnieją dwie opcje, które mogą być używane i połączone w zależności od potrzeb:

* Rozkładanie wielkość docelowy kopii zapasowej na wielu wirtualnych dysków twardych zainstalowanych w celu poprawienia wydajności operacji i/o na SEKUNDĘ na rozłożone woluminu
* Tworzenie konfiguracji zrzutu na poziomie SAP ASE, który używa więcej niż jeden katalog docelowy napisać zrzutu do

Rozkładanie woluminu przez wielu zainstalowanego wirtualnych dysków twardych zostały omówione wcześniej w tym przewodniku. Aby uzyskać więcej informacji na temat korzystania z wielu katalogów w konfiguracji zrzutu SAP ASE zapoznaj się z dokumentacją w sp_config_dump procedura przechowywana, który jest używany do tworzenia konfiguracji zrzutu w [Centrum informacyjne Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Odzyskiwanie z Azure maszyny wirtualne

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikacja danych z serwerem replikacji programu Sybase SAP
Z ASE SAP SAP Sybase replikacji serwera (SRS) stanowi ciepłą wstrzymania rozwiązanie transakcje bazy danych do lokalizacji odległości asynchroniczne. 

Instalacji i działania SRS działa także funkcjonalny maszyny hostowana w usługach maszyn wirtualnych Azure tak samo jak lokalnego.

HADR ASE za pośrednictwem serwera replikacji SAP nie jest obsługiwana w tym momencie. Może być sprawdzany przy użyciu i biuletynów za platformy Microsoft Azure w przyszłości.

## <a name="specifics-to-oracle-database-on-windows"></a>Szczegóły z bazą danych Oracle w systemie Windows
Od midyear 2013 oprogramowanie Oracle jest obsługiwany przez Oracle do uruchamiania w programie Microsoft Windows funkcji Hyper-V i Azure. Przeczytaj ten artykuł, aby uzyskać więcej szczegółowych informacji na ogólną pomoc techniczną Windows funkcji Hyper-V i Azure przez Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Po ogólną pomoc techniczną także jest obsługiwana scenariusz określonej aplikacji SAP używanie bazy danych programu Oracle. Szczegółowe informacje są nazywane w tej części dokumentu.

### <a name="oracle-version-support"></a>Obsługa wersji programu Oracle
Wszystkie szczegółowe informacje o wersji programu Oracle i odpowiednich wersjach systemu operacyjnego, które są obsługiwane w klastrze Oracle na Azure środowisku maszyn wirtualnych systemu SAP można znaleźć w następujących Uwaga systemu SAP [2039619]

Ogólne informacje o uruchamianiu pakietu firm SAP w bazie danych Oracle znajduje się na SCN: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Wskazówki dotyczące konfigurowania programu Oracle dla instalacji systemu SAP w Azure maszyny wirtualne

#### <a name="storage-configuration"></a>Konfiguracja magazynu
Tylko jedno wystąpienie programu Oracle przy użyciu dysków NTFS sformatowane jest obsługiwana. Wszystkie pliki bazy danych muszą być przechowywane w systemie plików NTFS oparte na dyskach wirtualnego dysku twardego. Te wirtualnych dysków twardych są instalowane do maszyn wirtualnych Azure i są oparte na magazyn obiektów BLOB platformy Azure strony (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Dowolny dysków sieciowych lub udziałach zdalnych, takich jak usługi Azure plików:
 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
**nie** są obsługiwane w przypadku plików bazy danych programu Oracle!

Przy użyciu wirtualnych dysków twardych Azure według magazyn obiektów BLOB strony Azure, zestawienia wprowadzone w tym dokumencie rozdziału [buforowanie maszyny wirtualne i wirtualnych dysków twardych] [bazami danych — przewodnik 2.1] i [magazyn Microsoft Azure] [bazami danych — przewodnik 2.3] dotyczą wdrożeń z bazy danych Oracle także.

Jak wyjaśniono wcześniej w ogólnej części dokumentu, istnieje przydziałów dla operacji i/o na SEKUNDĘ przepustowości dla Azure wirtualnych dysków twardych. Porównaj przydziałów w zależności od typu maszyn wirtualnych służą. Lista typów maszyn wirtualnych z ich przydziałów można znaleźć [w tym miejscu][virtual-machines-sizes]

Aby zidentyfikować obsługiwanych typów maszyn wirtualnych Azure, skorzystaj Uwaga systemu SAP [1928533]

Jak długo bieżącego przydziału operacji i/o na SEKUNDĘ na dysku spełnia wymagania, prawdopodobnie przechowywać wszystkie pliki bazy danych na jeden pojedynczy zainstalowanego Azure wirtualnego dysku twardego. 

Jeśli więcej operacji i/o na SEKUNDĘ są wymagane, zdecydowanie zalecane jest za pomocą puli miejsca do magazynowania okna (tylko dostępne w systemie Windows Server 2012 lub nowszy) lub rozkładanie systemu Windows dla systemu Windows 2008 R2 utworzyć jeden duży urządzenia logicznego przez wiele zainstalowanych dysków wirtualnego dysku twardego. Zobacz też rozdział — oprogramowanie RAID [bazami danych — przewodnik 2.2] tego dokumentu. Ta metoda upraszcza obciążenie administracyjne do zarządzania miejsca na dysku i pozwala uniknąć nakładu pracy w odniesieniu do ręcznego rozpowszechniania plików przez wielu zainstalowanego wirtualnych dysków twardych.

#### <a name="backup--restore"></a>Kopia zapasowa / Przywracanie
Do tworzenia kopii zapasowych / przywracanie funkcjonalności, SAP BR * narzędzia dla programu Oracle są obsługiwane w taki sam sposób jak na standardowe systemy operacyjne Windows Server i funkcji Hyper-V. Menedżer odzyskiwania Oracle (RMAN) jest również obsługiwane kopii zapasowych na dysku i Przywróć z dysku.

#### <a name="high-availability"></a>Wysoka dostępność
[komentarz]: <>  (łącze odwołuje się do ASM)
Straż danych Oracle jest obsługiwana wysokiej dostępności i operacji odzyskiwania. Szczegóły można znaleźć w [tym] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentacji.

#### <a name="other"></a>Inne
Inne tematy ogólne takie jak zestawy dostępność Azure lub SAP monitorowania stosowanie zgodnie z opisem w pierwsze trzy rozdziały tego dokumentu w przypadku wdrożeń maszyny wirtualne z bazy danych Oracle także.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Szczegóły dla bazy danych MaxDB SAP w systemie Windows

### <a name="sap-maxdb-version-support"></a>Obsługa wersji MaxDB SAP
SAP obecnie obsługuje MaxDB SAP wersji 7,9 do użytku z produktów opartych na SAP NetWeaver platformy Azure. Wszystkie aktualizacje dla SAP MaxDB server lub sterowniki JDBC i ODBC może być używany z produktami oparte na SAP NetWeaver są dostarczane wyłącznie za pośrednictwem witryny Marketplace usługi SAP w <https://support.sap.com/swdc>.
Ogólne informacje na temat uruchamiania SAP NetWeaver na SAP MaxDB można znaleźć w <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Obsługiwane typy wersji systemu Windows firmy Microsoft i maszyn wirtualnych Azure bazami danych MaxDB SAP
Aby znaleźć obsługiwanej wersji programu Microsoft Windows Azure, bazami danych MaxDB SAP, zobacz:

* [Macierz dostępność produktu SAP (PAM)][sap-pam]
* Uwaga systemu SAP [1928533]

Zdecydowanie zaleca się korzystać z najnowszych wersji systemu operacyjnego Microsoft Windows, która jest systemu Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>Dokumentacja MaxDB dostępne SAP
Uaktualniony wykaz dokumentacji SAP MaxDB można znaleźć w następujących Uwaga systemu SAP [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP — wskazówki dotyczące konfigurowania MaxDB dla instalacji systemu SAP w Azure maszyny wirtualne

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Konfiguracja magazynu
Azure magazynowania najważniejsze wskazówki dotyczące SAP MaxDB postępować zgodnie z zaleceniami ogólne, wymienione w rozdziału [struktury wdrożenia RDBMS] [bazami danych przewodnik-2].

> [AZURE.IMPORTANT] Podobnie jak inne bazy danych SAP MaxDB ma plików danych i dziennika. Jednak terminologia MaxDB SAP w celu poprawnego terminu jest "obrót" (nie "pliku"). Na przykład istnieje SAP MaxDB ilości danych i ilości dziennika. Nie należy mylić je razem z wielkości dysku systemu operacyjnego. 

Innymi słowy należy:

* Ustaw konto Azure miejsca do magazynowania, które zawiera SAP MaxDB danych i dziennika wielkość (to znaczy pliki) do **Lokalnego przechowywania zbędne (LRS)** jako określonych w rozdział [magazyn Microsoft Azure] [bazami danych — przewodnik 2.3].
* Oddzielenia ścieżkę ei SAP MaxDB ilości danych (to znaczy pliki) ścieżkę Jo wielkości dziennika (to znaczy plików). Oznacza to, że ilości danych SAP MaxDB (to znaczy pliki) musi być zainstalowany na dysku logicznego i ilości dziennika SAP MaxDB (to znaczy pliki) musi być zainstalowany na innym dysku logiczne.
* Ustaw odpowiednie pliku buforowania poszczególnych obiektów blob platformy Azure, w zależności od tego, czy możesz używać ich do SAP MaxDB danych lub dziennik wielkości (to znaczy plików) i używanie Azure standardowy lub miejsce do magazynowania Premium Azure, zgodnie z opisem w rozdziału [buforowanie maszyny wirtualne] [bazami danych — przewodnik 2.1].
* Jak długo bieżącego przydziału operacji i/o na SEKUNDĘ na dysku spełnia wymagania, jest możliwe do przechowywania wszystkich ilości danych na jednym zainstalowanego Azure wirtualnego dysku twardego, a także Zapisz wszystkie ilości dziennika bazy danych w innej pojedynczej zainstalowanego Azure wirtualnego dysku twardego.
* Jeśli więcej operacji i/o na SEKUNDĘ i/lub miejsca są wymagane, zdecydowanie zalecane jest za pomocą puli miejsca do magazynowania okno firmy Microsoft (tylko dostępne w systemie Microsoft Windows Server 2012 lub nowszy) lub rozkładanie Microsoft Windows dla programu Microsoft Windows 2008 R2 utworzyć jeden duży urządzenia logicznego przez wiele zainstalowanych dysków wirtualnego dysku twardego. Zobacz też rozdział — oprogramowanie RAID [bazami danych — przewodnik 2.2] tego dokumentu. Ta metoda upraszcza obciążenie administracyjne do zarządzania miejsca na dysku i pozwala uniknąć ręcznego rozpowszechniania plików na wielu zainstalowanego wirtualnych dysków twardych z wysiłku.
* Najwyższe wymagania operacji i/o na SEKUNDĘ można użyć magazyn Premium Azure, który jest dostępny na serii Zasadami i maszyny wirtualne serii GS.

![Konfiguracja odwołania Azure IaaS maszyn wirtualnych dla bazami danych MaxDB SAP][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Kopia zapasowa i przywracanie
Wdrażając SAP MaxDB do Azure, konieczne jest sprawdzenie Twojej kopii zapasowej metodologii. Nawet jeśli system nie jest wydajność systemu, SAP obsługiwanych przez SAP MaxDB musi być kopię zapasową bazy danych okresowo. Ponieważ Magazyn Azure zachowuje trzy obrazy, kopii zapasowej jest teraz mniej ważne w odniesieniu do ochrony systemu przed błąd pamięci i ważniejszych awariami operacyjne lub administracyjne. Głównym celem utrzymania pisane z wielkiej litery kopii zapasowych i przywracanie plan jest tak, aby można zadośćuczynienia pod kątem błędów logicznych lub ręcznie, dostarczając możliwości punktu w czasie odzyskiwania. Aby celem jest użyć kopie zapasowe przywrócenie bazy danych do określonego punktu w czasie lub za pomocą kopii zapasowych w Azure zapełnić innego systemu kopiując istniejącej bazy danych. Na przykład możesz można przesyłać z poziomu 2 konfiguracji systemu SAP konfiguracji systemu poziomu 3 tego samego systemu, przywracanie kopii zapasowej.

Wykonywanie kopii zapasowych i przywracanie bazy danych platformy Azure działa tak samo jak w przypadku systemów lokalnego, aby móc korzystać ze standardowej MaxDB SAP w celu tworzenia kopii zapasowych i przywracania narzędzia opisanych w jeden z wymienionych w Uwaga systemu SAP [767598]dokumentów dokumentacji SAP MaxDB. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Zagadnienia dotyczące wydajności kopii zapasowych i przywracanie
Tak jak we wdrożeniach zera wydajności i przywracania kopii zapasowych jest uzależniony od ilości ile można przeczytać w równolegle i przepustowość tych wielkości. Ponadto użycie Procesora używane przez kompresji kopii zapasowej można odtwarzać znaczącą rolę w maszyny wirtualne z wątkami Procesora do 8. Dlatego jedną Załóżmy:

* Mniejsza liczba wirtualnych dysków twardych służy do przechowywania urządzenia bazy danych, dolnej ogólną wydajność odczytu
* Mniejsza liczba procesorów wątki w Głosowa, bardziej poważne wpływ kompresja kopii zapasowej
* Mniejsza liczba elementów docelowych (katalogów pasek, wirtualnych dysków twardych) do zapisywania kopii zapasowej, dolnej przepustowość

Aby zwiększyć liczbę elementów docelowych do zapisu, istnieją dwie opcje, które są dostępne, prawdopodobnie w połączeniu, w zależności od potrzeb:

* Przeznaczenie osobnych wielkości kopii zapasowej
* Rozkładanie wielkość docelowy kopii zapasowej na wielu wirtualnych dysków twardych zainstalowanych w celu poprawienia wydajności operacji i/o na SEKUNDĘ na tym rozłożone woluminu
* O oddzielnych dedykowane dysku logicznego urządzenia:
    * SAP — ilości kopii zapasowej MaxDB (to znaczy plików)
    * SAP — MaxDB ilości danych (czyli plików)
    * SAP — wielkości dziennika MaxDB (to znaczy plików)

Rozkładanie woluminu przez wielu zainstalowanego wirtualnych dysków twardych został omówiony we wcześniejszej części rozdziału — oprogramowanie RAID [bazami danych — przewodnik 2.2] tego dokumentu. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Inne
Inne tematy ogólne takie jak zestawy dostępność Azure lub SAP monitorowania też ubiegać się zgodnie z opisem w pierwsze trzy rozdziały tego dokumentu w przypadku wdrożeń maszyny wirtualne w bazie danych SAP MaxDB.
Inne ustawienia specyficzne dla MaxDB SAP są niewidoczne dla maszyny wirtualne Azure i opisane w różne dokumenty wymienione w Uwaga systemu SAP [767598] te notatki SAP:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Szczegóły dla liveCache SAP w systemie Windows

### <a name="sap-livecache-version-support"></a>LiveCache SAP w celu obsługi wersji
Minimalną wersją systemu SAP liveCache obsługiwane w maszyn wirtualnych Azure jest **SAP LC/LCAPPS 10.0 SP 25** , łącznie z **liveCache 7.9.08.31** i **25 kompilacji LCA**, wydany **2 EhP SAP Menedżer sterowania usługami 7.0** lub nowszym.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Obsługiwane typy wersji systemu Windows firmy Microsoft i Azure maszyn wirtualnych systemu SAP liveCache bazami danych
Aby znaleźć obsługiwanej wersji programu Microsoft Windows liveCache SAP Azure, zobacz:

* [Macierz dostępność produktu SAP (PAM)][sap-pam]
* Uwaga systemu SAP [1928533]

Zdecydowanie zaleca się korzystać z najnowszych wersji systemu operacyjnego Microsoft Windows, która jest systemu Microsoft Windows 2012 R2. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP — liveCache wskazówki dotyczące konfigurowania dla instalacji systemu SAP w maszyny wirtualne Azure

#### <a name="recommended-azure-vm-types"></a>Polecane typy Azure maszyn wirtualnych
Jak SAP liveCache jest aplikacji, która wykonuje obliczenia duży, kwota i szybkości pamięci RAM i Procesora ma dużego wpływu na wydajność liveCache SAP. 

Dla typów Azure maszyn wirtualnych obsługiwanych przez SAP (Uwaga systemu SAP [1928533]) wszystkie zasoby Procesora wirtualnego przydzielono maszyn wirtualnych kopii przez dedykowane fizycznie zasobów Procesora monitor maszyny wirtualnej. Nie overprovisioning (i dlatego nie konkurencji dla zasobów Procesora) ma miejsce.

Podobnie wszystkie typy wystąpienie Azure maszyn wirtualnych obsługiwanych przez SAP, pamięci maszyn wirtualnych to 100% zamapowane pamięci fizycznej — na przykład overprovisioning (nadmierne zobowiązania), nie jest używane.

Z tej perspektywy zdecydowanie zaleca się nowy typ serii D lub maszyn wirtualnych Azure Zasadami serii (w połączeniu z magazyn Premium Azure), za pomocą mają 60% szybsze procesory niż serii A. Dla najwyższe obciążenie Procesora i pamięci RAM służy serii G i maszyny wirtualne serii GS (w połączeniu z magazyn Premium Azure) z najnowszych Intel® Xeon® E5 v3 rodzinie procesorów, której mają dwa razy pamięci i cztery razy stanie stałym dysk przechowywania (twarde SSD) i Zasadami serii.

#### <a name="storage-configuration"></a>Konfiguracja magazynu
Jak SAP liveCache jest oparty na technologii SAP MaxDB, Azure magazynie zalecane sposobami wymienionych dla SAP MaxDB w rozdziału [Konfiguracja magazynu] [bazami danych — przewodnik-8.4.1] dotyczą również SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Dedykowane Azure maszyn wirtualnych dla liveCache
Jak SAP liveCache intensywnie korzysta z power obliczeniowych, użycia wydajność zaleca do wdrożenia na dedykowane maszyny wirtualnej Azure. 
 
![Przeznaczony Azure maszyn wirtualnych do liveCache dla przypadków użycia wydajność][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Kopia zapasowa i przywracanie
Wykonywanie kopii zapasowych i przywracanie, łącznie z uwzględnieniem wydajności już zostały opisane w odpowiednich rozdziałów SAP MaxDB [kopia zapasowa i przywracanie] [bazami danych — przewodnik 8.4.2] i [zagadnienia dotyczące wydajności kopii zapasowych i przywracanie] [bazami danych — przewodnik-ppkt 8.4.3]. 

#### <a name="other"></a>Inne
Inne tematy ogólne są już opisane w odpowiednich rozdziałów [bazami danych — przewodnik-8.4.4] MaxDB SAP, [to]. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Szczegóły na serwerze zawartości SAP w systemie Windows
Rozwiązania SAP Server zawartości jest składnikiem oddzielnych, oparte na serwerze do przechowywania zawartości, takiej jak elektronicznych dokumenty w różnych formatach. Rozwiązania SAP Server zawartości jest dostarczany przez rozwoju technologii i ma być używany między platformami aplikacje SAP. Aplikacja jest zainstalowana na oddzielnego systemu. Typowa zawartość jest materiały szkoleniowe i dokumentów z magazynu wiedzy lub rysunki techniczne pochodzących z mySAP PLM systemu zarządzania dokumentami. 

### <a name="sap-content-server-version-support"></a>Obsługa wersji zawartości serwera SAP
Obsługuje obecnie SAP:

* **Rozwiązania SAP Server zawartości** z wersji **6.50 (lub nowszy)**
* **SAP — MaxDB wersji 7,9**
* **Microsoft IIS (Internet Information Server) wersji 8.0 (lub nowszy)**

Zdecydowanie zaleca się korzystanie z najnowszej wersji zawartości serwera SAP, czyli w momencie zapisywania tego dokumentu **6.50 SP4**i najnowszej wersji pakietu **Microsoft IIS 8,5**. 

Sprawdź najnowsze obsługiwanych wersji serwera zawartości SAP i Microsoft IIS w [Macierzy dostępność produktu SAP (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Obsługiwane typy Microsoft Windows i Azure maszyn wirtualnych systemu SAP Server zawartości
Aby dowiedzieć się, wersji systemu Windows SAP zawartości w obszarze Serwer Azure, zobacz:

* [Macierz dostępność produktu SAP (PAM)][sap-pam]
* Uwaga systemu SAP [1928533]

Zdecydowanie zaleca się korzystać z najnowszych wersji systemu Microsoft Windows, która w momencie zapisywania tego dokumentu jest **Windows Server 2012 R2**.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Wskazówki dotyczące konfigurowania zawartości serwera SAP dla instalacji systemu SAP w Azure maszyny wirtualne

#### <a name="storage-configuration"></a>Konfiguracja magazynu 
Po skonfigurowaniu systemu SAP Server zawartości do przechowywania plików w bazie danych SAP MaxDB wszystkie miejsca do magazynowania Azure najlepsze rozwiązania zalecenie wymienionych dla SAP MaxDB w rozdziału — miejsca do magazynowania konfiguracji [bazami danych — przewodnik-8.4.1] również są prawidłowe dla tego scenariusza rozwiązania SAP Server zawartości. 

Jeśli skonfigurujesz SAP Server zawartości do przechowywania plików w systemie plików, zaleca się używać dysku logicznego dedykowane. Używanie spacji miejsca do magazynowania umożliwia również zwiększyć rozmiar dysku logicznego i przepustowości operacji i/o na SEKUNDĘ, zgodnie z opisem w w rozdziału — oprogramowanie RAID [bazami danych — przewodnik 2.2]. 

#### <a name="sap-content-server-location"></a>Lokalizacja serwera zawartości SAP
Rozwiązania SAP Server zawartości musi być wdrożony w tym samym region Azure i miejsce, w którym wdrożenie systemu SAP VNET Azure. Mogą zdecydować, czy chcesz wdrożyć składniki rozwiązania SAP Server zawartości w dedykowanej maszyny Azure lub na tym samym maszyn wirtualnych miejsce, w którym jest uruchomiony systemu SAP. 
 
![Dedykowane Azure maszyn wirtualnych dla serwera zawartości SAP][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>Lokalizacja serwera pamięci podręcznej SAP
Rozwiązania SAP Server pamięci podręcznej to dodatkowe składnik oparte na serwerze umożliwia dostęp do lokalnie (buforowane) dokumenty. Rozwiązania SAP Server pamięci podręcznej buforowanie dokumentów z serwerem zawartości SAP. To aby zoptymalizować ruch sieciowy, jeśli masz dokumenty mają być pobierane więcej niż jeden raz z różnych lokalizacji. Ogólne reguła jest, że serwer pamięci podręcznej SAP musi być fizycznie zbliżony klienta, który uzyskuje dostęp do serwera pamięci podręcznej SAP. 

W tym miejscu masz dwie opcje:

1. **Klient jest systemu SAP wewnętrznej bazy danych** Jeśli systemu SAP wewnętrznej bazy danych jest skonfigurowany do dostępu do zawartości serwera SAP, systemu SAP jest klientem. Jak zarówno systemu SAP, jak i rozwiązania SAP Server zawartości wdrożonych w tym samym regionie Azure — w tym samym Azure centrum danych — są one fizycznie blisko siebie. Dlatego jest bez konieczności serwera dedykowanego pamięci podręcznej SAP. Klienci SAP interfejsu użytkownika (Graficznym SAP lub przeglądarki sieci web) systemu SAP uzyskują bezpośredni dostęp do, a SAP pobierany dokumentów z serwerem zawartości SAP.
1. **Klient jest lokalnego przeglądarki sieci web** Rozwiązania SAP Server zawartości można skonfigurować można uzyskać dostęp bezpośrednio z poziomu przeglądarki sieci web. W tym przypadku przeglądarki sieci web z lokalnych — jest klientem serwera zawartości SAP. Centrum danych lokalnych i Azure centrum danych znajdują się w różnych miejscach (najlepiej blisko siebie). Centrum danych lokalnych jest połączony z Azure za pośrednictwem Azure VPN witryny do witryny lub ExpressRoute. Chociaż w obu opcji bezpiecznego połączenia sieci VPN Azure, połączenie sieciowe witryny do witryny nie oferuje SLA sieci przepustowość i opóźnienie między centrum danych lokalnych i Azure centrum danych. Aby przyspieszyć dostęp do dokumentów, możesz wykonać jedną z następujących czynności:
    1. Zainstalowanie lokalnego serwera pamięci podręcznej SAP, Zamknij, aby w lokalnej sieci web przeglądarki (opcja na rysunku [this][dbms-guide-900-sap-cache-server-on-premises])
    1. Konfigurowanie Azure ExpressRoute, która oferuje szybkich i małym opóźnieniem dedykowane połączenie sieciowe między centrum danych lokalnych i Azure centrum danych.
 
![Opcja zainstalowania lokalnego serwera pamięci podręcznej SAP][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Kopia zapasowa / Przywracanie
Jeśli skonfigurujesz rozwiązania SAP Server zawartości do przechowywania plików w bazie danych SAP MaxDB zagadnienia wydajność i procedury tworzenia kopii zapasowych i przywracania już zostały opisane w rozdziału SAP MaxDB [kopia zapasowa i przywracanie] [bazami danych — przewodnik 8.4.2] i rozdziału [zagadnienia dotyczące wydajności kopii zapasowych i przywracanie] [bazami danych — przewodnik-ppkt 8.4.3]. 

Jeśli skonfigurujesz rozwiązania SAP Server zawartości do przechowywania plików w systemie plików, jedna opcja ma wykonać ręcznego tworzenie kopii zapasowych i przywracanie struktury całego pliku miejsce, w którym znajdują się dokumenty. Podobne do przywracania kopii zapasowej SAP MaxDB, zaleca się mają dedykowane woluminu w celu kopii zapasowej. 

#### <a name="other"></a>Inne
Inne ustawienia określone rozwiązania SAP Server zawartości są niewidoczne dla maszyny wirtualne Azure i opisanych w różnych dokumentów i SAP notatki:

* <https://Service.SAP.com/contentserver> 
* Uwaga systemu SAP [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Szczegóły do programu IBM DB2 dla LUW w systemie Windows
Przy użyciu programu Microsoft Azure możesz łatwo przeprowadzić migrację istniejącej aplikacji SAP z programu IBM DB2 Linux, UNIX i systemu Windows (LUW) do Azure maszyn wirtualnych. Z SAP na IBM DB2 dla LUW Administratorzy i deweloperzy nadal można używać w tej samej sytuacji i narzędzi administracyjnych, które są dostępne w lokalnym.
Ogólne informacje o uruchomionych IBM DB2 dla LUW pakietu firm SAP można znaleźć w sieci społeczności SAP (SCN) na <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Aby uzyskać dodatkowe informacje i aktualizacje dotyczące SAP na DB2 dla LUW Azure zobacz Uwaga systemu SAP [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX i obsługi wersji systemu Windows
SAP na IBM DB2 dla LUW w programie Microsoft Azure maszyn wirtualnych Services jest obsługiwane począwszy od wersji DB2 10.5.

Informacje o obsługiwanych produktach SAP i typy maszyn wirtualnych Azure można znaleźć w Uwaga systemu SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX i wskazówki dotyczące konfigurowania systemu Windows dla instalacji systemu SAP w Azure maszyny wirtualne

#### <a name="storage-configuration"></a>Konfiguracja magazynu
Wszystkie pliki bazy danych muszą być przechowywane w systemie plików NTFS oparte na dyskach wirtualnego dysku twardego. Te wirtualnych dysków twardych są instalowane do maszyn wirtualnych Azure i opierają się w magazynie obiektów BLOB platformy Azure strony (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Dowolny dysków sieciowych lub udziałach zdalnych, takich jak **nie** obsługiwane w przypadku plików bazy danych są następujące usługi Azure pliku: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Jeśli używasz wirtualnych dysków twardych Azure według magazyn obiektów BLOB strony Azure oświadczeń w tym dokumencie w rozdziału [struktury wdrożenia RDBMS] [bazami danych przewodnik-2] jest też zastosowane do wdrożenia z programu IBM DB2 LUW bazy danych. 

Jak wyjaśniono wcześniej w ogólnej części dokumentu, istnieje przydziałów dla operacji i/o na SEKUNDĘ przepustowości dla Azure wirtualnych dysków twardych. Porównaj przydziałów zależy od użytej maszyn wirtualnych. Lista typów maszyn wirtualnych z ich przydziałów można znaleźć [tutaj][virtual-machines-sizes].

Jak długo wystarcza bieżącego przydziału operacji i/o na SEKUNDĘ na dysku jest możliwe przechowywać pliki bazy danych na jeden pojedynczy zainstalowanego Azure wirtualnego dysku twardego. 

Wydajność zagadnienia również dotyczyć rozdziału "Danych bezpieczeństwa i wydajności zagadnienia dla bazy danych katalogów" SAP Przewodnik po instalacji.

Również służy puli miejsca do magazynowania systemu Windows (tylko dostępne w systemie Windows Server 2012 lub nowszy) lub rozkładanie systemu Windows dla systemu Windows 2008 R2 zgodnie z opisem w rozdziału — oprogramowanie RAID [bazami danych — przewodnik 2.2] ten dokument, aby utworzyć jeden duży urządzenia logicznego przez wiele zainstalowanych dysków wirtualnego dysku twardego.
W przypadku dysków zawierających DB2 ścieżek miejsca do magazynowania dla swojego katalogów sapdata i saptmp należy określić rozmiar sector dysk fizyczny 512 KB. Podczas korzystania z systemu Windows puli miejsca do magazynowania, należy utworzyć puli miejsca do magazynowania ręcznie za pośrednictwem interfejsu wiersza polecenia przy użyciu parametru "-LogicalSectorSizeDefault". Aby uzyskać szczegółowe informacje, zobacz <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Tworzenie kopii zapasowych i przywracanie
Funkcje tworzenia kopii zapasowych i przywracania IBM DB2 dla LUW jest obsługiwana w taki sam sposób, jak na standardowe systemy operacyjne Windows Server i funkcji Hyper-V.

Należy się upewnić, czy masz prawidłową bazą danych strategii wykonywania kopii zapasowych w miejscu. 

Tak jak we wdrożeniach zera tworzenie kopii zapasowych i przywracanie wydajność zależy od ilości ile można odczytywać równolegle i przepustowość tych wielkości, co można. Ponadto użycie Procesora używane przez kompresji kopii zapasowej mogą być odtwarzane znaczącą rolę na maszyny wirtualne z wątkami Procesora tylko do 8. Dlatego jedną Załóżmy:

* Mniejsza liczba wirtualnych dysków twardych umożliwia przechowywanie urządzenia bazy danych mniejszych ogólną przepustowość w odczytu
* Mniejsza liczba procesorów wątki w Głosowa, bardziej poważne wpływ kompresja kopii zapasowej
* Mniejsza liczba elementów docelowych (katalogów pasek, wirtualnych dysków twardych) do zapisywania kopii zapasowej, dolnej przepustowość

Aby zwiększyć liczbę elementów docelowych do zapisu, dwie opcje, które może być używane i połączone w zależności od potrzeb:

* Rozkładanie wielkość docelowy kopii zapasowej na wielu wirtualnych dysków twardych zainstalowanych w celu poprawienia wydajności operacji i/o na SEKUNDĘ na rozłożone woluminu
* Pisanie kopia zapasowa za pomocą więcej niż jeden katalog docelowy

#### <a name="high-availability-and-disaster-recovery"></a>Wysoką dostępność i odzyskiwanie
Klaster Microsoft Server (MSCS) nie jest obsługiwane.

DB2 szybkiej awarii (HADR) jest obsługiwana. Jeśli praca rozpoznawanie nazw w środowisku maszyn wirtualnych systemu konfiguracji HA, konfiguracji platformy Azure będzie różnić się od dowolnego Instalatora, w której jest wykonywane w lokalnej. Zależne tylko rozpoznawanie adresów IP nie jest zalecane.

Nie należy używać replikacji Geo sklepu Azure. Aby uzyskać więcej informacji, dotyczą rozdziału [magazyn Microsoft Azure] [bazami danych — przewodnik 2.3] i rozdziału [wysokiej dostępności i odzyskiwanie z maszyny wirtualne Azure] [bazami danych przewodnik-3].

#### <a name="other"></a>Inne
Inne tematy ogólne takie jak zestawy dostępność Azure lub SAP monitorowania stosowanie zgodnie z opisem w pierwsze trzy rozdziały tego dokumentu w przypadku wdrożeń maszyny wirtualne z programu IBM DB2 dla LUW także. 

Również dotyczyć rozdziału [ogólne SQL Server dla SAP na podsumowanie Azure] [bazami danych — przewodnik-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Szczegóły do programu IBM DB2 dla LUW w systemie Linux
Przy użyciu programu Microsoft Azure możesz łatwo przeprowadzić migrację istniejącej aplikacji SAP z programu IBM DB2 Linux, UNIX i systemu Windows (LUW) do Azure maszyn wirtualnych. Z SAP na IBM DB2 dla LUW Administratorzy i deweloperzy nadal można używać w tej samej sytuacji i narzędzi administracyjnych, które są dostępne w lokalnym. Ogólne informacje o uruchomionych IBM DB2 dla LUW pakietu firm SAP można znaleźć w sieci społeczności SAP (SCN) na <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Aby uzyskać dodatkowe informacje i aktualizacje dotyczące SAP na DB2 dla LUW Azure zobacz Uwaga systemu SAP [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX i obsługi wersji systemu Windows
SAP na IBM DB2 dla LUW w programie Microsoft Azure maszyn wirtualnych Services jest obsługiwane począwszy od wersji DB2 10.5.

Informacje o obsługiwanych produktach SAP i typy maszyn wirtualnych Azure można znaleźć w Uwaga systemu SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX i wskazówki dotyczące konfigurowania systemu Windows dla instalacji systemu SAP w Azure maszyny wirtualne

#### <a name="storage-configuration"></a>Konfiguracja magazynu
Wszystkie pliki bazy danych muszą być przechowywane w systemie plików, oparte na dyskach wirtualnego dysku twardego. Te wirtualnych dysków twardych są instalowane do maszyn wirtualnych Azure i opierają się w magazynie obiektów BLOB platformy Azure strony (<https://msdn.microsoft.com/library/azure/ee691964.aspx>).
Dowolny dysków sieciowych lub udziałach zdalnych, takich jak **nie** obsługiwane w przypadku plików bazy danych są następujące usługi Azure pliku:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Jeśli używasz wirtualnych dysków twardych Azure według magazyn obiektów BLOB strony Azure oświadczeń w tym dokumencie w rozdziału [struktury wdrożenia RDBMS] [bazami danych przewodnik-2] jest też zastosowane do wdrożenia z programu IBM DB2 LUW bazy danych.

Jak wyjaśniono wcześniej w ogólnej części dokumentu, istnieje przydziałów dla operacji i/o na SEKUNDĘ przepustowości dla Azure wirtualnych dysków twardych. Porównaj przydziałów zależy od użytej maszyn wirtualnych. Lista typów maszyn wirtualnych z ich przydziałów można znaleźć [tutaj][virtual-machines-sizes].

Jak długo wystarcza bieżącego przydziału operacji i/o na SEKUNDĘ na dysku jest możliwe przechowywać pliki bazy danych na jeden pojedynczy zainstalowanego Azure wirtualnego dysku twardego.

Wydajność zagadnienia również dotyczyć rozdziału "Bezpieczeństwa i wydajności zagadnienia dla bazy danych katalogi danych" SAP Przewodnik po instalacji.

Również służy LVM (logiczne menedżera woluminu) lub MDADM zgodnie z opisem w rozdziału — oprogramowanie RAID [bazami danych — przewodnik 2.2] ten dokument, aby utworzyć jeden duży urządzenia logicznego przez wiele zainstalowanych dysków wirtualnego dysku twardego.
W przypadku dysków zawierających DB2 ścieżek miejsca do magazynowania dla swojego katalogów sapdata i saptmp należy określić rozmiar sector dysk fizyczny 512 KB.

#### <a name="backuprestore"></a>Tworzenie kopii zapasowych i przywracanie
Funkcje tworzenia kopii zapasowych i przywracania IBM DB2 dla LUW jest obsługiwana w taki sam sposób, jak na standardowy Linux instalacji lokalnego.

Należy się upewnić, czy masz prawidłową bazą danych strategii wykonywania kopii zapasowych w miejscu.

Tak jak we wdrożeniach zera tworzenie kopii zapasowych i przywracanie wydajność zależy od ilości ile można odczytywać równolegle i przepustowość tych wielkości, co można. Ponadto użycie Procesora używane przez kompresji kopii zapasowej mogą być odtwarzane znaczącą rolę na maszyny wirtualne z wątkami Procesora tylko do 8. Dlatego jedną Załóżmy:

* Mniejsza liczba wirtualnych dysków twardych umożliwia przechowywanie urządzenia bazy danych mniejszych ogólną przepustowość w odczytu
* Mniejsza liczba procesorów wątki w Głosowa, bardziej poważne wpływ kompresja kopii zapasowej
* Mniejsza liczba elementów docelowych (katalogów pasek, wirtualnych dysków twardych) do zapisywania kopii zapasowej, dolnej przepustowość

Aby zwiększyć liczbę elementów docelowych do zapisu, dwie opcje, które może być używane i połączone w zależności od potrzeb:

* Rozkładanie wielkość docelowy kopii zapasowej na wielu wirtualnych dysków twardych zainstalowanych w celu poprawienia wydajności operacji i/o na SEKUNDĘ na rozłożone woluminu
* Pisanie kopia zapasowa za pomocą więcej niż jeden katalog docelowy

#### <a name="high-availability-and-disaster-recovery"></a>Wysoką dostępność i odzyskiwanie
DB2 szybkiej awarii (HADR) jest obsługiwana. Jeśli praca rozpoznawanie nazw w środowisku maszyn wirtualnych systemu konfiguracji HA, konfiguracji platformy Azure będzie różnić się od dowolnego Instalatora, w której jest wykonywane w lokalnej. Zależne tylko rozpoznawanie adresów IP nie jest zalecane.

Nie należy używać replikacji Geo sklepu Azure. Aby uzyskać więcej informacji, dotyczą rozdziału [magazyn Microsoft Azure] [bazami danych — przewodnik 2.3] i rozdziału [wysokiej dostępności i odzyskiwanie z maszyny wirtualne Azure] [bazami danych przewodnik-3].

#### <a name="other"></a>Inne
Inne tematy ogólne takie jak zestawy dostępność Azure lub SAP monitorowania stosowanie zgodnie z opisem w pierwsze trzy rozdziały tego dokumentu w przypadku wdrożeń maszyny wirtualne z programu IBM DB2 dla LUW także.

Również dotyczyć rozdziału [ogólne SQL Server dla SAP na podsumowanie Azure] [bazami danych — przewodnik-5.8].