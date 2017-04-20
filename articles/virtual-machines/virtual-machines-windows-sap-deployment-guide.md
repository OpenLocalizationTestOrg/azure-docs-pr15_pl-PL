<properties
   pageTitle="SAP NetWeaver w systemie Windows wirtualnych maszyn — Podręcznik wdrażania | Microsoft Azure"
   description="SAP NetWeaver w systemie Windows wirtualnych maszyn — Podręcznik wdrażania"
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

# <a name="sap-netweaver-on-windows-virtual-machines-vms--deployment-guide"></a>SAP NetWeaver w systemie Windows wirtualnych maszyn — Podręcznik wdrażania

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

[dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (SAP NetWeaver w systemie Windows wirtualnych maszyn — Podręcznik wdrażania bazami danych) [dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (buforowanie maszyny wirtualne i wirtualnych dysków twardych) [dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (oprogramowania RAID) [dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (magazyn Microsoft Azure) [dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (struktura wdrożenia RDBMS) [dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (wysokiej dostępności i odzyskiwanie z maszyny wirtualne Azure) [dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (program SQL Server 2012 z dodatkiem SP1 CU4 lub w nowszej wersji) [dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (program SQL Server 2012 z dodatkiem SP1 CU3 i starszych wersjach) [dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (za pomocą obrazów programu SQL Server poza Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (ogólne programu SQL Server dla SAP na podsumowanie Azure) [dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (szczegóły do programu SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Konfiguracja miejsca do magazynowania) [dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (kopia zapasowa i przywracanie) [dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (zagadnienia dotyczące wydajności kopii zapasowych i przywracanie) [dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (inne) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (SAP NetWeaver w systemie Windows wirtualnych maszyn — Podręcznik wdrażania) [deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP zasobów) [deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (wdrażanie maszyn wirtualnych z obraz niestandardowy) [deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (scenariusz 1: Wdrażanie maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace dla SAP) [deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenariusz 2: Wdrażanie maszyn wirtualnych z obraz niestandardowy dla SAP) [deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md# a9a60133-a763-4de8-8986-ac0fa33aa8c1 (scenariusz 3: przenoszenie maszyn wirtualnych ze źródeł lokalnych uogólniony wirtualnego dysku twardego Azure za pomocą SAP) [deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (wdrożenia scenariuszy z maszyny wirtualne dla systemu SAP w programie Microsoft Azure) [deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (wdrażanie Azure poleceń cmdlet) [deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (plik do pobrania i SAP w celu importowania odpowiednich poleceń cmdlet programu PowerShell) [deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (dołączanie maszyn wirtualnych do domeny lokalnej — tylko w systemie Windows) [deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md# 6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (pobieranie, instalowanie i Włącz agenta maszyn wirtualnych Azure) [deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure programu PowerShell) [deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (polecenie Azure) [deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurowanie Azure rozszerzony monitorowania rozszerzenie dla SAP) [deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (gotowość do wyszukania Azure rozszerzony monitorowania SAP) [wdrożenia — przewodnik 5,2]: Virtual-Machines-Windows-SAP-Deployment-Guide.MD#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (sprawdzanie kondycji konfiguracji infrastruktury monitorowania Azure) [deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (dodatkowo monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP)

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

[planning-guide]:virtual-machines-windows-sap-planning-guide.md (SAP NetWeaver na Windows wirtualnych maszyn — planowanie i przewodnik po implementacji) [planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (zasobów) [planning-guide-11]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (wysoka dostępność (HA) i odzyskiwanie danych (DR) dla SAP NetWeaver uruchomione na maszyn wirtualnych Azure) [planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (wysoką dostępność dla serwerów aplikacji SAP) [planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (przy użyciu Autostart wystąpień SAP) [planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (tylko do chmury - wdrożeń maszyn wirtualnych do Azure bez zależności w sieci lokalnej klienta) [planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (między lokalna – wdrożenia jednego lub wielu SAP maszyny wirtualne do Azure z wymaganiami jest w pełni zintegrowany z siecią lokalnego) [planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regiony) [planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 [planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (uaktualnianie domen) [planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088-(błędów domen) f9be - 4c 97-958a - 27996255c 665 (zestawach dostępność Azure) [planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (koncepcja Microsoft maszyn wirtualnych Azure) [planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (magazynowanie Premium Azure) [planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (przenoszenie maszyn wirtualnych ze źródeł lokalnych Azure za pomocą dysku uogólniony) [planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (wdrażanie maszyn wirtualnych z określonego obrazu klienta) [planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (przygotowanie do przenoszenia maszyn wirtualnych ze źródeł lokalnych Azure z uogólniony dysku) [planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (przygotowanie do wdrażania maszyn wirtualnych z określonego obrazu klienta dla SAP) [planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (przygotowywanie maszyny wirtualne z SAP dla Azure) [planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (różnicę między Azure dysku i obraz Azure) [planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (przekazywanie wirtualnego dysku twardego ze źródeł lokalnych Azure) [planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopiowanie dyski między kontami miejsca do magazynowania Azure) [planowania — przewodnik-5.5.1]: Virtual-Machines-Windows-SAP-Planning-Guide.MD#4efec401-91e0-40c0-8e64-f2dceadff646 (maszyn wirtualnych-wirtualnego dysku twardego struktura wdrożeniach systemu SAP) [planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (ustawienie automatycznej instalacji na dyskach dołączony) [planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (pojedynczy maszyn wirtualnych z programem SAP NetWeaver pokaz szkolenie scenariusz) [planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (pojęcia Cloud-Only wdrożenie wystąpień SAP) [planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure monitorowania rozwiązaniem SAP) [planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md# ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (magazynowanie Azure Premium)

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

Microsoft Azure umożliwia uzyskanie zasobów obliczeń i miejsca do magazynowania w czasie minimalnego bez cykli długiej zamówień. Azure maszyn wirtualnych umożliwia firmom wdrażanie klasycznego aplikacji, takich jak aplikacje do Azure oparte na SAP NetWeaver i rozszerzanie ich niezawodności i dostępności bez dalszych zasobów dostępnych lokalnego. Microsoft Azure umożliwia łączność między lokalnej, umożliwiający firmom aktywnie integrowanie maszyn wirtualnych Azure ich domeny lokalnej ich chmury prywatne i ich pozioma systemu SAP.

Ten dokument opisano krok po kroku, jak maszyna wirtualna Azure jest gotowa do wdrożenia aplikacji SAP NetWeaver podstawie. Przyjęto założenie, że informacje zawarte w [planowania i przewodnik po implementacji] [-przewodnik po planowaniu pod] jest znany. W przeciwnym razie odpowiednich dokumentu należy przeczytać najpierw.

Papier uzupełnia dokumentacji instalacji SAP i notatki SAP, reprezentujące podstawowego zasoby dotyczące instalacji i wdrożenia oprogramowania SAP na określonych platformach.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="introduction"></a>Wprowadzenie
Duża liczba firm na całym świecie korzystać z aplikacji SAP NetWeaver podstawie — najbardziej dobrze pakietu firm SAP — uruchamianie misji krytycznych procesów biznesowych. Kondycja systemu w związku z tym jest ważnych elementów zawartości i możliwość obsługi przedsiębiorstwa w przypadku awarii, w tym przypadki wydajności, staje się istotne wymagania.
Microsoft Azure udostępnia oprzyrządowania najwyższej platformy, aby zezwalały na wymagania obsługą wszystkich kluczowych aplikacji biznesowych. Ten przewodnik służy do sprawdzenia, czy maszyny wirtualnej Microsoft Azure przeznaczona dla wdrożenia oprogramowania SAP jest skonfigurowany tak, aby pomocy technicznej organizacji może skorzystać, niezależnie od sposobu maszyny wirtualnej otrzymuje utworzyć, jest to zrobione wylogowywanie się z usługi Azure Marketplace lub używając określonego obrazu klienta.
W ustawieniach następujących, wszystkie niezbędne kroki opisane szczegółowo.

## <a name="prerequisites-and-resources"></a>Wymagania wstępne i zasobów
### <a name="prerequisites"></a>Wymagania wstępne
Zanim zaczniesz, upewnij się, czy są spełnione warunki wstępne, które zostały opisane w kolejnych rozdziałów.

#### <a name="local-personal-computer"></a>Komputer lokalny
Konfiguracji systemu Azure maszyny wirtualnej wdrożenia oprogramowania SAP składa się z kilku kroków. Aby zarządzać maszyny wirtualne systemu Windows i Linux oraz maszyny wirtualne, należy używać skryptów programu PowerShell i portalu Microsoft Azure. Do tego komputera lokalnego osobiste z systemem Windows 7 lub nowszy jest konieczne. Jeśli chcesz tylko zarządzać maszyny wirtualne Linux i chcesz używać komputera Linux oraz dla tego zadania, również służy interfejs wiersza polecenia Azure (polecenie Azure).

#### <a name="internet-connection"></a>Połączenie z Internetem
Aby pobrać i uruchomić narzędzia potrzebne i skryptów, jest wymagane połączenie internetowe. Ponadto Microsoft Azure Virtual Machine uruchomiony Azure rozszerzony monitorowania rozszerzenia musi mieć dostęp do Internetu. W przypadku, gdy ten maszyn wirtualnych Azure jest częścią Azure wirtualnej sieci lub domeny lokalnej, upewnij się, że ustawienia serwera proxy odpowiednich są ustawione zgodnie z opisem w rozdziału [Konfigurowanie serwera Proxy] [ deployment-guide-configure-proxy] w tym dokumencie.

#### <a name="microsoft-azure-subscription"></a>Subskrypcja Microsoft Azure
Istnieje już konto Azure i zgodnie z poświadczeń logowania są znane.

#### <a name="topology-consideration-and-networking"></a>Topologia uwagę i sieci
Topologia i architektury wdrożenia SAP w Azure należy zdefiniować. Architektura w odniesieniu do:

* Magazyn Microsoft Azure konta może być używany
* Wirtualna sieć wdrożenie systemu SAP w
* Grupa zasobów do wdrożenia systemu SAP w
* Azure Region wdrożenie systemu SAP
* Konfiguracji systemu SAP (poziom 2 lub 3 warstwa)
* Całkowitą pojemność komputerze maszyn wirtualnych i liczby kolejne wirtualnych dysków twardych ma zostać zainstalowany do VM(s)
* Konfiguracji systemu SAP transportu i korekty

Azure klienci miejsca do magazynowania lub wirtualnych sieci Azure sposób należy zostały utworzone i już skonfigurowane. Jak utworzyć i skonfigurować je jest objęte [planowania i przewodnik po implementacji] [-przewodnik po planowaniu pod].

#### <a name="sap-sizing"></a>SAP w celu zmiany rozmiaru
* Przewidywane pracą SAP ustaleniu, np. przy użyciu SAP Quicksizer i wiadomo, zgodnie z numerem protokoły SAP 
* Powinny być znane wymaganych zasobów i pamięci użycie Procesora systemu SAP
* Powinny być znane wymagane operacji We/Wy na sekundę
* Znany jest wymagane przepustowości w ostatecznej komunikacji między różne maszyny wirtualne platformy Azure
* Wymagane przepustowości między aktywów lokalnego i Azure wdrożony SAP wiadomo, systemów

#### <a name="resource-groups"></a>Grupy zasobów
Grupy zasobów to nowa koncepcja, która zawiera wszystkie zasoby, które mają samego cyklu życia np są tworzone i usunięte w tym samym czasie. [W tym] artykule[ resource-group-overview] uzyskać więcej informacji dotyczących grup zasobów. 

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Zasoby SAP
Podczas pracy konfiguracji potrzebne są następujące zasoby:

* Uwaga systemu SAP [1928533]
    * na liście rozmiarów maszyn wirtualnych Azure, które są obsługiwane w przypadku wdrożenia oprogramowania SAP 
    * wydajność ważnych informacji na rozmiar maszyn wirtualnych Azure
    * obsługiwane oprogramowanie SAP i kombinacji systemu operacyjnego i bazy danych
* SAP — notatki [2015553] pozycje wymagania wstępne dotyczące są obsługiwane przez SAP podczas wdrażania oprogramowania SAP w programie Microsoft Azure.
* SAP — notatki [1999351] zawierający dodatkowe informacje dotyczące rozwiązywania problemów dla rozszerzony Azure monitorowania dla SAP.
* SAP — notatki [2178632] zawierające szczegółowe informacje na wszystkie dostępne metryki monitorowania SAP w programie Microsoft Azure. 
* SAP — notatki [1409604] zawierający wymagane wersję agenta hosta SAP dla systemu Windows Azure firmy Microsoft, wdrażając na nowy Menedżer zasobów Azure.
* SAP — notatki [2191498] zawierający wymagane wersję agenta hosta SAP Linux na Microsoft Azure, wdrażając na nowy Menedżer zasobów Azure.
* Uwaga systemu SAP [2243692] zawierający informacje dotyczące licencjonowania SAP w systemie Linux Azure
* Uwaga [1984787] zawierający ogólne informacje na temat SUSE LINUX Enterprise Server 12 SAP
* Uwaga [2002167] zawierający informacje ogólne Red funkcję Enterprise Linux SAP 7.x
* [SCN](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , który zawiera wszystkie wymagane notatek SAP Linux
* SAP — określonych poleceń cmdlet programu PowerShell, które są częścią [Azure programu PowerShell][azure-ps]
* SAP określone polecenie Azure, które są częścią [Polecenie Azure][azure-cli]
* [Portal Microsoft Azure][azure-portal]

[comment]: <> (Poziom poprawki MSSedusch zadania Dodaj ARM agenta hosta SAP w 1409604 Uwaga systemu SAP)
 
Prowadnice następujące kwestie tematu SAP w programie Microsoft Azure także:

* [SAP NetWeaver na Windows wirtualnych maszyn — planowanie i przewodnik po implementacji] [-przewodnik po planowaniu pod]
* [SAP NetWeaver na Windows wirtualnych maszyn — Podręcznik wdrażania (ten dokument)] [Podręcznik wdrażania]
* [SAP NetWeaver w systemie Windows wirtualnych maszyn — Podręcznik wdrażania bazami danych] [bazami danych — przewodnik]
* [SAP NetWeaver na Windows wirtualnych maszyn — Podręcznik wdrażania wysokiej dostępności][ha-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Wdrożeń programu maszyny wirtualne dla systemu SAP w programie Microsoft Azure
W tym rozdziału Dowiedz się różne sposoby rozmieszczania i jednej czynności dla każdego typu wdrożenia.

### <a name="deployment-of-vms-for-sap"></a>Wdrożenie maszyny wirtualne dla systemu SAP
Microsoft Azure oferuje wiele sposobów wdrażanie maszyny wirtualne i skojarzone dysków. Co jest ważne zrozumienie różnic od przygotowania maszyny wirtualne mogą się różnić zależy od sposobu wdrożenia. Ogólnie możemy wyglądać w następujących sytuacjach:

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Wdrażanie maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace
Chcesz wykonać firmy Microsoft lub strona 3 opisane obraz poza Azure Marketplace wdrażanie usługi maszyn wirtualnych. Po wdrożeniu programu maszyn wirtualnych w programie Microsoft Azure, należy wykonać tę samą wskazówek i narzędzi do zainstalowania oprogramowania SAP w swojej maszyn wirtualnych w sposób jak w środowisku lokalnym. Dotyczące instalowania oprogramowania SAP wewnątrz maszyn wirtualnych Azure, SAP i Microsoft zaleca się przekazywanie i przechowywanie nośnik instalacyjny SAP w wirtualnych dysków twardych Azure lub tworzenia maszyn wirtualnych Azure działa zgodnie z "serwer plików", który zawiera wszystkie niezbędne nośnik instalacyjny SAP.

[comment]: <> (Dlaczego należy zalecamy zarządzanie plikami, np. serwera plików lub wirtualnego dysku twardego zadania MSSedusch? Jest to tak różne ze źródeł lokalnych?)

Aby uzyskać więcej informacji, zobacz rozdział [scenariusz 1: Wdrażanie maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace dla SAP] [wdrożenia — przewodnik 3,2].

#### <a name="3688666f-281f-425b-a312-a77e7db2dfab"></a>Wdrażanie maszyn wirtualnych z obraz niestandardowy
Ze względu na określone wymagania dotyczące w odniesieniu do Twojej wersji systemu operacyjnego lub bazami danych dostarczonych obrazy z usługi Azure Marketplace może nie odpowiada Twoim potrzebom. Dlatego konieczne może być Tworzenie maszyn wirtualnych za pomocą własnego "private" obraz maszyn wirtualnych systemu operacyjnego-DB, który można wdrożyć po kilka razy.
Czynności, aby utworzyć obraz prywatne różnią się systemu Windows i Linux oraz obrazu.

___

> ![Systemu Windows][Logo_Windows] Systemu Windows
>
> Aby przygotować obraz systemu Windows, który może być używany do wdrożenia wiele maszyn wirtualnych, ustawienia systemu Windows (na przykład SID systemu Windows i nazwa hosta) musi być pobieranej uogólniony na maszyn wirtualnych lokalnego. Można to zrobić za pomocą programu sysprep, zgodnie z opisem w <https://technet.microsoft.com/library/cc721940.aspx>.
>
> ![Linux][Logo_Linux] Linux
>
> Aby przygotować obraz Linux, który może być używany do wdrożenia wiele maszyn wirtualnych, niektóre ustawienia Linux musi być pobieranej uogólniony na maszyn wirtualnych lokalnego. Można to zrobić przy użyciu waagent-deprovision zgodnie z opisem w [tym artykule] [ virtual-machines-linux-capture-image] lub w [tym artykule][virtual-machines-linux-agent-user-guide-command-line-options].

___

Możesz skonfigurować zawartości bazy danych za pomocą Menedżera świadczenia oprogramowania SAP zainstalować nowego systemu SAP, przywracanie kopii zapasowej bazy danych z wirtualnego dysku twardego podłączonego do komputera wirtualnych lub bezpośrednio przywracanie kopii zapasowej bazy danych z magazynu Azure, jeśli to obsługuje bazami danych. (zobacz [Guide][dbms-guide]) wdrożenia bazami danych. Jeśli masz już zainstalowany systemu SAP w swojej maszyn wirtualnych lokalnego (zwłaszcza w przypadku systemów poziom 2), możesz dopasować ustawienia systemu SAP po wdrożeniu maszyn wirtualnych Azure za pomocą procedury zmienić System obsługiwane przez SAP oprogramowania inicjowania obsługi administracyjnej Menedżera (Uwaga systemu SAP [1619720]). W przeciwnym razie możesz zainstalować oprogramowanie SAP później po wdrażania maszyn wirtualnych Azure.

Uzyskać więcej informacji, zobacz rozdział [Scenariusz 2: Wdrażanie maszyn wirtualnych z obraz niestandardowy dla SAP] [wdrożenia — przewodnik 3.3].

#### <a name="moving-a-vm-from-on-premises-to-microsoft-azure-with-a-non-generalized-disk"></a>Przenoszenie maszyn wirtualnych ze źródeł lokalnych do Microsoft Azure za pomocą dysku uogólniony
Planowanie przenieść określonego systemu SAP z lokalnego Microsoft Azure. Można to zrobić, przekazując wirtualny dysk twardy, zawierające system operacyjny, plików binarnych SAP i ewentualnego bazami danych plików binarnych oraz wirtualnych dysków twardych z plikami danych i dziennika Pozwalające Microsoft Azure. W przeciwnie do scenariusza opisanego w rozdziału [wdrażania maszyn wirtualnych z obraz niestandardowy] [wdrożenia — przewodnik-3.1.2] powyżej przechowujesz hostname, identyfikator SAP zabezpieczeń i kont użytkowników systemu SAP w maszyn wirtualnych Azure one zostały skonfigurowane w lokalnym środowisku. Dlatego uogólnianie system operacyjny nie jest to konieczne. Tym przypadku głównie zostaną zastosowane dla lokalnej między scenariuszy, gdzie część pozioma SAP jest uruchomiona lokalnego i części Microsoft Azure.

Uzyskać więcej informacji, zobacz rozdział [Scenariusz 3: przenoszenie maszyn wirtualnych ze źródeł lokalnych uogólniony wirtualnego dysku twardego Azure za pomocą SAP] [wdrożenia — przewodnik 3.4].

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenariusz 1: Wdrażanie maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace dla SAP
Microsoft Azure oferuje możliwość wdrażanie wystąpieniem maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace, która oferuje kilka standardowych obrazów systemu operacyjnego systemu Windows Server i różnych dystrybucji Linux. Użytkownik może również wdrażanie obraz zawierający SKU bazami danych przykład programu SQL Server. Szczegóły tych obrazów za pomocą SKU bazami danych można znaleźć w [bazami danych Podręcznik wdrażania] [bazami danych — przewodnik]

SAP w określonej kolejności kroków wdrażania maszyn wirtualnych wylogowywanie się z usługi Azure Marketplace będzie wyglądać:

![Schemat blokowy wdrażania maszyn wirtualnych systemu SAP przy użyciu obrazu maszyn wirtualnych z usługi Azure Marketplace][deployment-guide-figure-100]

Po schematu blokowego należy można wykonywać następujące czynności:

#### <a name="create-virtual-machine-using-the-azure-portal"></a>Tworzenie maszyn wirtualnych za pomocą portalu Azure
Najprostszym sposobem utworzenia nowej maszyny wirtualnej przy użyciu obrazu z usługi Azure Marketplace jest przez Azure Portal. Przejdź do <https://portal.azure.com/#create>. Wprowadź szukany typ systemu operacyjnego, który chcesz wdrożyć w polu wyszukiwania, np. systemu Windows, SLES lub RHEL i wybierz wersję. Upewnij się wybrać model wdrażania "Menedżera zasobów Azure" i kliknij przycisk Utwórz.

Kreator przeprowadzi Cię przez parametry wymagane do utworzenia maszyny wirtualnej oraz wszystkie wymagane zasoby, takie jak interfejsy lub innego konta miejsca do magazynowania. Niektóre z tych parametrów są:

1. Podstawowe informacje
    1. Nazwa: Nazwę zasobu, to znaczy maszyn wirtualnych nazwy
    1. Nazwa użytkownika i hasło i SSH klucz publiczny: Wprowadź nazwę użytkownika i hasło użytkownika, który jest tworzony podczas inicjowania obsługi administracyjnej. Dla maszyny wirtualnej Linux, można także wprowadzić klucz publiczny SSH, który ma być używany do logowania do komputera przy użyciu SSH.
    1. Subskrypcja: Wybierz subskrypcję, do której chcesz użyć do obsługi administracyjnej nowej maszyny wirtualnej.
    1. Grupa zasobów: Nazwa grupy zasobów. Można wstawić nazwę nowej grupy zasobów lub grupa zasobów, która już istnieje
    1. Lokalizacja: Wybierz lokalizację, w której powinny zostać wdrożone nowa maszyna wirtualna. Jeśli chcesz nawiązać maszyny wirtualnej sieci lokalnej, upewnij się wybrać lokalizację wirtualna sieć, która łączy Azure do sieci lokalnej. Aby uzyskać więcej informacji, zobacz rozdział [Microsoft Azure sieci] [ planning-guide-microsoft-azure-networking] w [przewodnika po planowaniu] [-przewodnik po planowaniu pod].
1. Rozmiar — Przeczytaj Uwaga systemu SAP [1928533] , aby uzyskać listę obsługiwanych typów maszyn wirtualnych. Również upewnij się wybrać typ poprawne, jeśli chcesz używać Premium miejsca do magazynowania. Nie we wszystkich typach maszyn wirtualnych obsługuje Premium miejsca do magazynowania. Zobacz rozdział [miejsca do magazynowania: magazyn Microsoft Azure i dyski danych] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] [Magazyn Premium Azure] i [planowania przewodnik azure-premium magazynowania] w [przewodnika po planowaniu] [-przewodnik po planowaniu pod] Aby uzyskać więcej informacji.
1. Ustawienia
    1. Konto miejsca do magazynowania: Można wybrać istniejące konto miejsca do magazynowania lub Utwórz nowy. Przeczytaj rozdział [magazyn Microsoft Azure] [bazami danych — przewodnik 2.3] [bazami danych przewodnika] [bazami danych — przewodnik] Aby uzyskać więcej informacji na temat typów innego miejsca do magazynowania. Należy zauważyć, że nie wszystkie typy magazynowania są obsługiwane uruchamiania aplikacji SAP.
    1. Wirtualnej sieci i podsieci: zaznacz wirtualnej sieci, który jest podłączony do sieci lokalnej, jeśli chcesz zintegrować maszyny wirtualnej sieci intranet.
    1. Publiczny adres IP: zaznacz adres publiczny adres IP, którego chcesz użyć, lub wprowadź parametry, aby utworzyć nowy adres publiczny adres IP. Publiczny adres IP umożliwia dostęp do komputera wirtualnych w Internecie. Upewnij się również utworzyć grupę zabezpieczeń sieci do filtrowania dostępu do komputera wirtualnych.
    1. Grupa zabezpieczeń sieci: Zobacz, [Co to jest grupa zabezpieczeń sieci (NSG)] [ virtual-networks-nsg] uzyskać więcej szczegółowych informacji.
    1. Monitorowanie: Możesz wyłączyć to ustawienie diagnostyki. Go będą dostępne automatycznie po uruchomieniu polecenia, aby włączyć Azure rozszerzony monitorowania, zgodnie z opisem w rozdziału [Konfigurowanie monitorowania][deployment-guide-configure-monitoring-scenario-1].
    1. Dostępność: Wybierz zestaw dostępność lub wprowadź odpowiednie parametry, aby utworzyć nowy zestaw dostępność. Aby uzyskać więcej informacji, zobacz rozdział [Azure dostępność zestawy] [planowania — przewodnik-3.2.3].
1. Podsumowanie: Sprawdź informacje podane na stronie podsumowania i kliknij przycisk OK.

Po zakończeniu pracy Kreator komputera wirtualnych zostanie wdrożony w wybranej grupie zasobów.

#### <a name="create-virtual-machine-using-a-template"></a>Tworzenie maszyn wirtualnych przy użyciu szablonu
Możesz również utworzyć wdrożeniu przy użyciu jednego z szablonów SAP opublikowanych w [repozytorium azure — Szybki Start szablony github][azure-quickstart-templates-github]. Możesz utworzyć maszyny wirtualnej za pomocą [Azure Portal][virtual-machines-windows-tutorial], [programu PowerShell] [ virtual-machines-ps-create-preconfigure-windows-resource-manager-vms] lub [Polecenie Azure] [ virtual-machines-linux-tutorial] ręcznie.

* [Szablon konfiguracji poziomu 2 (tylko jedna maszyna wirtualna)] [ sap-templates-2-tier-marketplace-image] Użyj tego szablonu, jeśli chcesz utworzyć przy użyciu tylko maszyn wirtualnych systemu poziomu 2.
* [Szablon konfiguracji poziomu 3 (wiele maszyn wirtualnych)] [ sap-templates-3-tier-marketplace-image] Użyj tego szablonu, jeśli chcesz utworzyć przy użyciu wielu maszyn wirtualnych systemu poziomu 3.

Po otwarciu jednego z szablonów powyżej Azure Portal przechodzi do panelu Edytuj parametry. Wprowadź następujące informacje:

* **sapSystemId**: identyfikator systemu SAP
* **osType**: system operacyjny chcesz wdrożyć np Windows Server 2012 R2, SLES 12 lub RHEL 7.2
    * Lista zawiera tylko wersje, które są obsługiwane przez SAP w programie Microsoft Azure
* **sapSystemSize**: rozmiar systemu SAP
    * Liczba protokoły SAP zapewni nowego systemu. Jeśli nie ma pewności ile protokoły SAP wymaga systemu, należy skontaktować się z partnerem technologii SAP lub systemu polega
* **systemAvailability**: (tylko szablon poziomu 3) dostępność systemu 
    * Wybierz pozycję HA konfiguracji, która nadaje się do instalacji HA. Serwer bazy danych w dwóch i dwa serwery ASCS zostanie utworzona.
* storageType: (tylko szablon poziomu 2) typ magazynu, który ma być używany 
    * Dla systemów większy zaleca się użycie pamięci Premium. Aby uzyskać więcej informacji na temat typów magazynu innego więcej 
        * [Magazynem platformy Microsoft Azure] [bazami danych — przewodnik-2.3] [bazami danych — przewodnik] [bazami danych przewodnika]
        * [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych][storage-premium-storage-preview-portal]
        * [Wprowadzenie do magazynu platformy Microsoft Azure][storage-introduction]
* **adminUsername** i **adminPassword**: nazwa użytkownika i hasło
    * Nowy użytkownik zostanie utworzony można używać do logowania się do komputera.
* **newOrExistingSubnet**: Określa, czy ma zostać utworzona nowa sieć wirtualna i podsieci, czy istniejącą podsieć powinien być używany. Jeśli masz już wirtualnej sieci, który jest podłączony do sieci lokalnej, zaznacz istniejący.
* **subnetId**: identyfikator podsieci, do której maszyn wirtualnych powinien być połączony z. Wybierz pozycję podsieci sieci VPN lub rozsyłania Express wirtualnych nawiązać maszyny wirtualnej sieci lokalnej. Identyfikator zazwyczaj wygląda /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Po wprowadzeniu wszystkich parametrów Wybierz subskrypcję i grupa zasobów, którego chcesz użyć. Możesz wybrać istniejącej grupy zasobów lub utworzyć nową, wybierając pozycję "+ nowe" w menu rozwijanym. Po utworzeniu nowej grupy zasobów masz wybierz region, w którym zostanie utworzone grupy zasobów i maszyny wirtualnej.

Przejrzyj warunki prawne, akceptując je i kliknij pozycję Utwórz.

Uwaga wdrożeniu agenta maszyn wirtualnych Azure domyślnie podczas korzystania z danego obrazu z Azure Marketplace.

#### <a name="configure-proxy-settings"></a>Konfigurowanie ustawień serwera Proxy
W zależności od konfiguracji sieci lokalnej mogą być wymagane do konfigurowania serwera proxy na komputerze wirtualnych, jeśli jest podłączony do sieci lokalnej za pośrednictwem sieci VPN lub rozsyłania Express. W przeciwnym razie maszyna wirtualna może nie móc uzyskać dostęp do Internetu i dlatego nie można pobrać wymaganego rozszerzenia lub zbieranie danych monitorowania. Zobacz rozdział [Konfigurowanie serwera Proxy] [ deployment-guide-configure-proxy] tego dokumentu.

#### <a name="join-domain-windows-only"></a>Dołączanie do domeny (tylko w systemie Windows)
W przypadku czy wdrożenia w Azure jest połączony z lokalnym AD-DNS za pośrednictwem Azure witryny do witryny lub rozsyłania Express (określany także jako lokalnej krzyżowe w [planowania i Guide][planning-guide]) implementacji oczekuje się, że maszyn wirtualnych dołączania do domeny lokalnej. Zagadnienia dotyczące tego kroku opisano w rozdziału [dołączanie maszyn wirtualnych do domeny lokalnej (tylko w systemie Windows)] [wdrożenia — przewodnik 4,3] tego dokumentu.

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurowanie monitorowania
Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP, zgodnie z opisem w rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5] tego dokumentu.

Sprawdź wymagania wstępne dotyczące monitorowania SAP wymagane minimalne wersji jądra SAP i agenta hosta SAP w zasoby wyświetlone w rozdziału — SAP zasoby [wdrożenia — przewodnik 2.2] tego dokumentu.

#### <a name="monitoring-check"></a>Monitorowanie wyboru
Sprawdź, czy monitorowanie działa zgodnie z opisem w rozdziału [sprawdza i rozwiązywanie problemów dotyczących zakończenia do końca monitorowania Instalatora systemu SAP Azure][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Publikowanie kroków wdrażania
Po utworzeniu maszyn wirtualnych, zostanie wdrożony i jest ona na zainstalowaniu wszystkie składniki oprogramowania niezbędne do maszyn wirtualnych. W związku z tym tego typu wdrożenia maszyn wirtualnych wymaga oprogramowania należy beeing zainstalowano już dostępne w programie Microsoft Azure w innych maszyn wirtualnych lub jako dysku, który można dołączyć. Lub możemy odnaleźć do lokalnej krzyżowe scenariuszy w przypadku połączeń do zasobów lokalnych (Instaluj akcje) danego.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenariusz 2: Wdrażanie maszyn wirtualnych z obraz niestandardowy dla SAP
Zgodnie z opisem w [planowania i przewodnik po implementacji] [-przewodnik po planowaniu pod] już w folderze uzyskać szczegółowe instrukcje istnieje sposób, aby przygotować i utworzyć obraz niestandardowy i za jej pomocą tworzyć wiele nowych maszyny wirtualne. Kolejności etapów schemat blokowy będzie wyglądać:
 
![Schemat blokowy wdrażania maszyn wirtualnych systemu SAP przy użyciu obrazu maszyn wirtualnych rynku prywatnego][deployment-guide-figure-300]

Po schematu blokowego należy można wykonywać następujące czynności:

#### <a name="create-virtual-machine"></a>Tworzenie maszyn wirtualnych
Do tworzenia wdrożeniu przy użyciu prywatne obraz systemu operacyjnego przez Azure Portal należy używać jednego z szablonów SAP opublikowany [repozytorium github azure — Szybki Start szablony][azure-quickstart-templates-github].
Możesz również utworzyć przy użyciu [programu PowerShell] maszyny wirtualnej[ virtual-machines-upload-image-windows-resource-manager] ręcznie. 

* [Szablon konfiguracji poziomu 2 (tylko jedna maszyna wirtualna)] [ sap-templates-2-tier-user-image] Użyj tego szablonu, jeśli chcesz utworzyć przy użyciu tylko maszyn wirtualnych i obrazu systemu operacyjnego system poziomu 2.
* [Szablon konfiguracji poziomu 3 (wiele maszyn wirtualnych)] [ sap-templates-3-tier-user-image] Jeśli chcesz utworzyć system poziomu 3 przy użyciu wielu maszyn wirtualnych i obrazu systemu operacyjnego za pomocą tego szablonu.

Po otwarciu jednego z szablonów powyżej Azure Portal przechodzi do panelu Edytuj parametry. Wprowadź następujące informacje:

* **sapSystemId**: identyfikator systemu SAP
* **osType**: chcesz wdrożyć systemu Windows i Linux oraz typ systemu operacyjnego
* **sapSystemSize**: rozmiar systemu SAP
    * Liczba protokoły SAP zapewni nowego systemu. Jeśli nie ma pewności ile protokoły SAP wymaga systemu, należy skontaktować się z partnerem technologii SAP lub systemu polega
* **systemAvailability**: (tylko szablon poziomu 3) dostępność systemu 
    * Wybierz pozycję HA konfiguracji, która nadaje się do instalacji HA. Serwer bazy danych w dwóch i dwa serwery ASCS zostanie utworzona.
* **storageType**: (tylko szablon poziomu 2) typ magazynu, który ma być używany 
    * Dla systemów większy zaleca się użycie pamięci Premium. Aby uzyskać więcej informacji na temat typów magazynu innego więcej 
        * [Magazynem platformy Microsoft Azure] [bazami danych — przewodnik-2.3] [bazami danych — przewodnik] [bazami danych przewodnika]
        * [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych][storage-premium-storage-preview-portal]
        * [Wprowadzenie do magazynu platformy Microsoft Azure][storage-introduction]
* **adminUsername** i **adminPassword**: nazwa użytkownika i hasło
    * Nowy użytkownik zostanie utworzony można używać do logowania się do komputera.
* **userImageVhdUri**: identyfikator URI prywatne OS obrazu wirtualnego dysku twardego np https://`<accountname`>.blob.core.windows.net/vhds/userimage.vhd
* **userImageStorageAccount**: nazwę konta magazynu prywatne obraz systemu operacyjnego przechowywania np `<accountname`> w tym przykładzie identyfikator URI powyżej
* **newOrExistingSubnet**: Określa, czy ma zostać utworzona nowa sieć wirtualna i podsieci, czy istniejącą podsieć powinien być używany. Jeśli masz już wirtualnej sieci, który jest podłączony do sieci lokalnej, zaznacz istniejący.
* **subnetId**: identyfikator podsieci, do której maszyn wirtualnych powinien być połączony z. Wybierz pozycję podsieci sieci VPN lub rozsyłania Express wirtualnych nawiązać maszyny wirtualnej sieci lokalnej. Identyfikator zazwyczaj wygląda /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Po wprowadzeniu wszystkich parametrów Wybierz subskrypcję i grupa zasobów, którego chcesz użyć. Możesz wybrać istniejącej grupy zasobów lub utworzyć nową, wybierając pozycję "+ nowe" w menu rozwijanym. Po utworzeniu nowej grupy zasobów masz wybierz region, w którym zostanie utworzone grupy zasobów i maszyny wirtualnej.

Przejrzyj warunki prawne, akceptując je i kliknij pozycję Utwórz.

#### <a name="install-vm-agent-linux-only"></a>Instalowanie agenta maszyn wirtualnych (tylko Linux)
Linux Agent musi być już zainstalowana obrazu użytkownika, jeśli chcesz używać szablonów powyżej. W przeciwnym razie wdrażanie zakończy się niepowodzeniem. Pobieranie i instalowanie agenta maszyn wirtualnych obrazu użytkownika, zgodnie z opisem w rozdziału [pobieranie, instalowanie i włączanie agenta maszyn wirtualnych Azure] [wdrożenia — przewodnik-4.4] tego dokumentu.
Jeśli nie korzystasz z powyższych szablonów, można również zainstalować agenta maszyn wirtualnych później.

#### <a name="join-domain-windows-only"></a>Dołączanie do domeny (tylko w systemie Windows)
W przypadku czy wdrożenia w Azure jest połączony z lokalnym AD-DNS za pośrednictwem Azure witryny do witryny lub rozsyłania Express (określany także jako lokalnej krzyżowe w [planowania i Guide][planning-guide]) implementacji oczekuje się, że maszyn wirtualnych dołączania do domeny lokalnej. Zagadnienia dotyczące tego kroku opisano w rozdziału [dołączanie maszyn wirtualnych do domeny lokalnej (tylko w systemie Windows)] [wdrożenia — przewodnik 4,3] tego dokumentu.

#### <a name="configure-proxy-settings"></a>Konfigurowanie ustawień serwera Proxy
W zależności od konfiguracji sieci lokalnej mogą być wymagane do konfigurowania serwera proxy na komputerze wirtualnych, jeśli jest podłączony do sieci lokalnej za pośrednictwem sieci VPN lub rozsyłania Express. W przeciwnym razie maszyna wirtualna może nie móc uzyskać dostęp do Internetu i dlatego nie można pobrać wymaganego rozszerzenia lub zbieranie danych monitorowania. Zobacz rozdział [Konfigurowanie serwera Proxy] [ deployment-guide-configure-proxy] tego dokumentu.

#### <a name="configure-monitoring"></a>Konfigurowanie monitorowania
Konfigurowanie Azure monitorowania rozszerzenie SAP, zgodnie z opisem w rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5] tego dokumentu.
Sprawdź wymagania wstępne dotyczące monitorowania SAP wymagane minimalne wersji jądra SAP i agenta hosta SAP w zasoby wyświetlone w rozdziału — SAP zasoby [wdrożenia — przewodnik 2.2] tego dokumentu.

#### <a name="monitoring-check"></a>Monitorowanie wyboru
Sprawdź, czy monitorowanie działa zgodnie z opisem w rozdziału [sprawdza i rozwiązywanie problemów dotyczących zakończenia do końca monitorowania Instalatora systemu SAP Azure][deployment-guide-troubleshooting-chapter].

### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenariusz 3: Przenoszenie maszyn wirtualnych ze źródeł lokalnych uogólniony wirtualnego dysku twardego Azure za pomocą systemu SAP
W tym scenariuszu jest adresowania w przypadku systemu SAP, po prostu jest przenoszony w obecnej formie i kształt z lokalnego Azure. Oznacza bez nazwy zmienianie systemu Windows i Linux oraz nazwa hosta i SAP SID lub mniej więcej tak jak odbywa się. W tym przypadku wirtualny dysk twardy nie jest używany jako obraz podczas wdrażania, ale bezpośrednio jest używany jako dysku systemu operacyjnego. W przypadku wdrożenia tym przypadku różni się od dwóch przypadkach byłego faktów agenta maszyn wirtualnych nie można automatycznie zainstalować podczas wdrażania. Dlatego Agent maszyn wirtualnych Azure muszą być pobrane od firmy Microsoft i musi być zainstalowany i włączone w maszyn wirtualnych ręcznie później. Po to zadanie zakończyło się pomyślnie, możesz nadal zainicjować hosta SAP w celu monitorowania rozszerzenia Azure i konfigurację. Aby uzyskać szczegółowe informacje o funkcji agenta maszyn wirtualnych Azure Sprawdź, czy w tym artykule:

[comment]: <> (Łącze Windows Update zadania MSSedusch poniżej) 

___

> ![Systemu Windows][Logo_Windows] Systemu Windows
>
> <http://blogs.msdn.com/b/wats/Archive/2014/02/17/bginfo-Guest-Agent-Extension-for-Azure-VMS.aspx>
>
> ![Linux][Logo_Linux] Linux
>
> [Przewodnik użytkownika Azure agenta Linux][virtual-machines-linux-agent-user-guide]

___

Przepływ pracy różne kroki wyglądają podobnie do:
 
![Schemat blokowy wdrażania maszyn wirtualnych systemu SAP za pomocą dysku maszyn wirtualnych][deployment-guide-figure-400]

Przy założeniu, że dysk nie jest już przekazane i zdefiniowane w Azure (patrz [planowania i Guide][planning-guide]) implementacji, wykonaj następujące czynności

#### <a name="create-virtual-machine"></a>Tworzenie maszyn wirtualnych
Aby utworzyć wdrożeniu przy użyciu prywatne dyskiem OS przez Azure Portal, za pomocą szablonu SAP opublikowany [repozytorium azure — Szybki Start szablony github][azure-quickstart-templates-github]. Możesz również utworzyć przy użyciu maszyn wirtualnych [programu PowerShell lub interfejsu wiersza polecenia Azure ręcznie.

* [Szablon konfiguracji poziomu 2 (tylko jedna maszyna wirtualna)][sap-templates-2-tier-os-disk]
    * Użyj tego szablonu, jeśli chcesz utworzyć przy użyciu tylko maszyn wirtualnych systemu poziomu 2.

Po otwarciu powyżej szablon Azure Portal przechodzi do panelu Edytuj parametry. Wprowadź następujące informacje:

* **sapSystemId**: identyfikator systemu SAP
* **osType**: chcesz wdrożyć systemu Windows i Linux oraz typ systemu operacyjnego
* **sapSystemSize**: rozmiar systemu SAP
    * Liczba protokoły SAP zapewni nowego systemu. Jeśli nie ma pewności ile protokoły SAP wymaga systemu, należy skontaktować się z partnerem technologii SAP lub systemu polega
* **storageType**: (tylko szablon poziomu 2) typ magazynu, który ma być używany 
    * Dla systemów większy zaleca się użycie pamięci Premium. Aby uzyskać więcej informacji na temat typów magazynu innego więcej 
        * [Magazynem platformy Microsoft Azure] [bazami danych — przewodnik-2.3] [bazami danych — przewodnik] [bazami danych przewodnika]
        * [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych][storage-premium-storage-preview-portal]
        * [Wprowadzenie do magazynu platformy Microsoft Azure][storage-introduction]
* **osDiskVhdUri**: identyfikator URI prywatne OS dysku np https://`<accountname`>.blob.core.windows.net/vhds/osdisk.vhd
* **newOrExistingSubnet**: Określa, czy ma zostać utworzona nowa sieć wirtualna i podsieci, czy istniejącą podsieć powinien być używany. Jeśli masz już wirtualnej sieci, który jest podłączony do sieci lokalnej, zaznacz istniejący.
* **subnetId**: identyfikator podsieci, do której maszyn wirtualnych powinien być połączony z. Wybierz pozycję podsieci sieci VPN lub rozsyłania Express wirtualnych nawiązać maszyny wirtualnej sieci lokalnej. Identyfikator zazwyczaj wygląda /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Po wprowadzeniu wszystkich parametrów Wybierz subskrypcję i grupa zasobów, którego chcesz użyć. Możesz wybrać istniejącej grupy zasobów lub utworzyć nową, wybierając pozycję "+ nowe" w menu rozwijanym. Po utworzeniu nowej grupy zasobów masz wybierz region, w którym zostanie utworzone grupy zasobów i maszyny wirtualnej.

Przejrzyj warunki prawne, akceptując je i kliknij pozycję Utwórz.

#### <a name="install-vm-agent"></a>Instalowanie agenta maszyn wirtualnych
Agent maszyn wirtualnych musi być już zainstalowana na dysku systemu operacyjnego, jeśli chcesz używać szablonów powyżej. W przeciwnym razie wdrażanie zakończy się niepowodzeniem. Pobieranie i instalowanie agenta maszyn wirtualnych w maszyn wirtualnych, zgodnie z opisem w rozdziału [pobieranie, instalowanie i włączanie agenta maszyn wirtualnych Azure] [wdrożenia — przewodnik-4.4] tego dokumentu.

Jeśli nie korzystasz z powyższych szablonów, można również zainstalować agenta maszyn wirtualnych później.

#### <a name="join-domain-windows-only"></a>Dołączanie do domeny (tylko w systemie Windows)
W przypadku czy wdrożenia w Azure jest połączony z lokalnym AD-DNS za pośrednictwem Azure witryny do witryny lub rozsyłania Express (określany także jako lokalnej krzyżowe w [planowania i Guide][planning-guide]) implementacji oczekuje się, że maszyn wirtualnych dołączania do domeny lokalnej. Zagadnienia dotyczące tego kroku opisano w rozdziału [dołączanie maszyn wirtualnych do domeny lokalnej (tylko w systemie Windows)] [wdrożenia — przewodnik 4,3] tego dokumentu.

#### <a name="configure-proxy-settings"></a>Konfigurowanie ustawień serwera Proxy
W zależności od konfiguracji sieci lokalnej mogą być wymagane do konfigurowania serwera proxy na komputerze wirtualnych, jeśli jest podłączony do sieci lokalnej za pośrednictwem sieci VPN lub rozsyłania Express. W przeciwnym razie maszyna wirtualna może nie móc uzyskać dostęp do Internetu i dlatego nie można pobrać wymaganego rozszerzenia lub zbieranie danych monitorowania. Zobacz rozdział [Konfigurowanie serwera Proxy] [ deployment-guide-configure-proxy] tego dokumentu.

#### <a name="configure-monitoring"></a>Konfigurowanie monitorowania
Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP, zgodnie z opisem w rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5] tego dokumentu.

Sprawdź wymagania wstępne dotyczące monitorowania SAP wymagane minimalne wersji jądra SAP i agenta hosta SAP w zasoby wyświetlone w rozdziału — SAP zasoby [wdrożenia — przewodnik 2.2] tego dokumentu.

#### <a name="monitoring-check"></a>Monitorowanie wyboru
Sprawdź, czy monitorowanie działa zgodnie z opisem w rozdziału [sprawdza i rozwiązywanie problemów dotyczących zakończenia do końca monitorowania Instalatora systemu SAP Azure][deployment-guide-troubleshooting-chapter].

### <a name="scenario-4-updating-the-monitoring-configuration-for-sap"></a>Scenariusz 4: Aktualizowanie monitorowania konfiguracji systemu SAP
Istnieją przypadki, w którym trzeba zaktualizować konfiguracji monitorowania:

* Wspólnego zespołu MS-SAP rozszerzone możliwości monitorowania i decyzję dodać więcej liczników lub usunąć niektóre liczniki. 
* Microsoft wprowadza nowej wersji podstawowej infrastruktury Azure przedstawiania danych monitorowania i Azure rozszerzony monitorowania rozszerzenie SAP dostosowuje się do tych zmian.
* Możesz dodać dodatkowe wirtualnych dysków twardych zainstalowany do maszyn wirtualnych usługi Azure lub usuwanie wirtualnego dysku twardego. W tym przypadku należy zaktualizować zbiór miejsca do magazynowania powiązane dane. Jeśli zmiana konfiguracji przez dodanie lub usunięcie punkty końcowe lub przypisywanie adresów IP do maszyny, nie wpłyną konfiguracji monitorowania.
* Możesz zmienić rozmiar maszyn wirtualnych usługi Azure np od A5 do dowolnego innego rozmiaru maszyn wirtualnych.
* Dodawanie nowego interfejsów do maszyn wirtualnych usługi Azure

Aby zaktualizować konfiguracji monitorowania, postępować w następujący sposób:

* Aktualizowanie monitorowania infrastruktury, wykonując czynności opisane szczegółowo rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5] tego dokumentu. Ponownie uruchom skrypt opisane w tym rozdziału wykryje, że konfiguracji monitorowania zostanie wdrożony i wykona niezbędne zmiany w celu monitorowania konfiguracji. 

___

> ![Systemu Windows][Logo_Windows] Systemu Windows
>
> Aktualizacji agenta maszyn wirtualnych Azure cichym nie jest wymagane. Agent maszyn wirtualnych automatycznie aktualizowana i nie wymaga ponownego uruchomienia maszyn wirtualnych.
>
> ![Linux][Logo_Linux] Linux
>
> Wykonaj czynności podane w [tym artykule] [ virtual-machines-linux-update-agent] zaktualizować Azure agenta Linux. 

___

## <a name="detailed-single-deployment-steps"></a>Szczegółowe kroki pojedynczy wdrożenia

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Rozmieszczanie poleceń cmdlet środowiska PowerShell Azure
* Przejdź do <https://azure.microsoft.com/downloads/>
* W sekcji "Narzędzia wiersza polecenia" jest sekcji o nazwie "Programu Windows PowerShell". Kliknięcie łącza "Zainstaluj".
* Menedżer pobierania programu Microsoft zostanie okno podręczne z pozycji kończące się literami .exe. Wybierz opcję "Uruchom".
* Podręczny będzie pojawiły się pytaniem, czy uruchomić Instalatora platformy sieci Web firmy Microsoft. Naciśnij klawisz tak
* Pojawi się ekran podobny do tego:
 
![Ekran instalacji dla poleceń cmdlet środowiska PowerShell Azure][deployment-guide-figure-500]
<a name="figure-5"></a>

* Naciśnij klawisz instalacji i zaakceptuj umowę licencyjną.

Często sprawdzać, czy zostały zaktualizowane poleceń cmdlet programu PowerShell. Zazwyczaj są dostępne aktualizacje w czasie co miesiąc. W tym celu najłatwiej postępuj zgodnie z instrukcjami instalacji, zgodnie z powyższym opisem w górę ekranu instalacji wyświetlane w [tym] [ deployment-guide-figure-5] rysunek. Na tym ekranie Data wydania poleceń cmdlet przedstawiono oraz numer wersji rzeczywiste. Jeśli nie określono inaczej, notatki SAP [1928533] lub [2015553], zaleca się do pracy z najnowszą wersją Azure poleceń cmdlet.

Bieżący zainstalowanej wersji cmdlet Azure na komputerze stacjonarnym i przenośnym można sprawdzić przy użyciu polecenia PS:

```powershell
Import-Module Azure
(Get-Module Azure).Version
```

Powinny być prezentowane wynik, jak pokazano poniżej, w [tym] [ deployment-guide-figure-6] rysunek.

![Wynik to sprawdzanie wersji polecenia cmdlet Azure PS][deployment-guide-figure-600]
<a name="figure-6"></a>

Jeśli wersja Azure polecenia cmdlet zainstalowany na komputerze i na komputerze przenośnym jest bieżąca, pierwszy ekran po uruchomieniu Instalatora platformy sieci Web firmy Microsoft będzie wyglądać nieco inne w porównaniu z przedstawiony w [tym] [ deployment-guide-figure-5] rysunek.

Pamiętaj, czerwone zakreślenie poniżej [rysunek] [ deployment-guide-figure-7] poniżej.
 
![Ekran instalacji dla Azure poleceń cmdlet wskazująca, że zainstalowano najnowszą wersję polecenia cmdlet Azure PS][deployment-guide-figure-700]
<a name="figure-7"></a>

Jeśli ekran wygląda jak [powyżej][deployment-guide-figure-7], wskazująca, że jest już zainstalowana najnowsza wersja Azure polecenia cmdlet, istnieje potrzeba kontynuowania instalacji. W takim przypadku możesz "opuścić" instalacji na tym etapie.

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Wdrażanie polecenie Azure
* Przejdź do <https://azure.microsoft.com/downloads/>
* W sekcji "Narzędzia wiersza polecenia" jest sekcji o nazwie "Azure interfejs wiersza polecenia". Kliknięcie łącza instalacji systemu operacyjnego.

Często sprawdzać, czy zostały zaktualizowane polecenie Azure. Zazwyczaj są dostępne aktualizacje w czasie co miesiąc. Najprostszym sposobem na tym jest postępuj zgodnie z instrukcjami instalacji zgodnie z powyższym opisem.

Bieżący zainstalowanej wersji Azure interfejsu wiersza polecenia na komputerze i na komputerze przenośnym można sprawdzić przy użyciu polecenia:

```
azure --version
```

Powinny być prezentowane wynik, jak pokazano poniżej, w [tym] [ deployment-guide-figure-azure-cli-version] rysunek.

![Wynik to polecenie Azure Sprawdzanie wersji][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Dołączanie do maszyn wirtualnych do domeny lokalnej (tylko w systemie Windows)
W przypadku miejsce, w którym wdrażanie SAP maszyny wirtualne do scenariusza lokalnej krzyżowe miejsce, w którym AD lokalnego i DNS jest używane do Azure, jest planowane sprzężone maszyny wirtualne w domenie lokalnej. Szczegółowe etapy dołączania maszyny do domeny lokalnej i dodatkowego oprogramowania musi być członkiem lokalnej domeny jest zależne klienta. Zazwyczaj dołączania do domeny lokalnej maszyny oznacza, że instalowania dodatkowego oprogramowania, takiego jak oprogramowanie do ochrony przed złośliwym oprogramowaniem lub różnych czynników kopii zapasowej lub monitorowania oprogramowania.

Ponadto należy upewnij się, w przypadku której Internet ustawienia serwera proxy wymuszeniem podczas dołączania do domeny, czy Windows Account(S-1-5-18) System lokalny w maszyn wirtualnych gość ma te ustawienia, a także. Easiest jest wymusić serwera proxy z zasad grupy dla domeny, której dotyczą systemów w domenie.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Pobieranie, instalowanie i włączanie agenta maszyn wirtualnych Azure
Poniższe czynności są konieczne, gdy maszyny dla SAP jest rozmieszczany z obrazów systemu operacyjnego niebędący uogólniony np nieprzetworzony dla systemu Windows. Nie jest wymagane do zainstalowania agenta dla maszyn wirtualnych wdrożony z Azure marketplace. Te obrazy już zawierać agenta Azure.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Systemu Windows

* Pobierz agenta Azure maszyn wirtualnych:
    * Pobierz pakiet Instalatora Azure maszyn wirtualnych agenta z: <https://go.microsoft.com/fwlink/?LinkId=394789>
    * Przechowywanie pakiet MSI agenta maszyn wirtualnych lokalnie na komputerze przenośnym lub na serwerze
* Instalowanie agenta Azure maszyn wirtualnych:
    * Nawiązywanie połączenia z wdrożonym maszyn wirtualnych Azure z usługi terminalowe (RDP)
    * Otwórz okno Eksploratora Windows na maszyn wirtualnych i otwórz katalog docelowy dla pliku MSI agenta maszyn wirtualnych
    * Przeciągnij i upuść plik Azure maszyn wirtualnych agenta Instalatora MSI z komputera przenośnego serwera lokalnego katalogu docelowego agenta maszyn wirtualnych w maszyn wirtualnych
    * Kliknij dwukrotnie plik MSI maszyn wirtualnych
    * Dla maszyn wirtualnych dołączony do domeny lokalnej, upewnij się, że ewentualne ustawienia serwera proxy Internet zastosować w maszyn wirtualnych dla konta systemu lokalnego (S-1-5-18), a także opisano w rozdziału [Konfigurowanie serwera Proxy][deployment-guide-configure-proxy]. Agent maszyn wirtualnych będzie działać w tym kontekście i musi mieć możliwość łączenia się Azure.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Zainstaluj agenta maszyn wirtualnych Linux przy użyciu następującego polecenia

- **SLES**

```
sudo zypper install WALinuxAgent
```
- **RHEL**

```
sudo yum install WALinuxAgent
```

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Konfigurowanie serwera Proxy
Instrukcje dotyczące konfigurowania serwera proxy różnią się w systemach Windows i Linux.

#### <a name="windows"></a>Systemu Windows
Te ustawienia należy używać do konta System lokalny dostęp do Internetu. Jeśli ustawienia serwera proxy nie są ustawione przez zasady grupy, można je skonfigurować dla konta System lokalny wykonaj poniższe czynności, aby go skonfigurować.

1.  Otwórz gpedit.msc
1.  Przejdź do konfiguracji komputera –> Szablony administracyjne -> składniki Windows -> programu Internet Explorer i włącz "serwer proxy ustawienia na komputerze (a nie na użytkownika)
1.  Otwórz Panel sterowania i przejdź do sieć i Internet -> Opcje internetowe
1.  Otwórz kartę połączenia, a następnie kliknij przycisk Ustawienia sieci LAN
1.  Wyłączanie "Automatycznie wykryj ustawienia"
1.  Włączanie "Użyj serwera proxy dla sieci LAN", a następnie wprowadź hosta serwera proxy oraz portu

#### <a name="linux"></a>Linux
Skonfiguruj poprawne serwer proxy w pliku konfiguracji agenta firmy Microsoft Azure gościa, w której znajduje się w /etc/waagent.conf. Można ustawić następujące parametry:

```
HttpProxy.Host=<proxy host e.g. proxy.corp.local>
HttpProxy.Port=<port of the proxy host e.g. 80>
```

Ponowne uruchomienie agenta po zmianie swoją konfigurację na

```
sudo service waagent restart
```

Ustawienia serwera proxy /etc/waagent.conf też ubiegać się o wymagane rozszerzenia maszyn wirtualnych. Jeśli chcesz zastosować repozytoria Azure, upewnij się, że ruch do tych repozytoria nie będzie za pośrednictwem sieci intranet lokalnego. Jeśli utworzono przekierowuje zdefiniowane przez użytkownika Włącz wymuszone Tunneling, upewnij się dodać trasę który kieruje ruch do repozytoria bezpośrednio do Internetu, a nie za pośrednictwem połączenia z witryny do witryny.

- **SLES** Należy dodać trasy dla adresów IP wymienionych w /etc/regionserverclnt.cfg. Przykład przedstawiono poniżej ekranu. 

- **RHEL** Należy dodać trasy dla adresów IP hostów wymienionych w /etc/yum.repos.d/rhui-load-balancers. Przykład przedstawiono poniżej ekranu.

Aby uzyskać więcej informacji na temat przekierowuje zdefiniowane przez użytkownika, zobacz [Ten artykuł][virtual-networks-udr-overview].

![Wymuszone Tunneling][deployment-guide-figure-50]

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurowanie Azure rozszerzonego rozszerzenie monitorowania dla SAP
Po maszyn wirtualnych jest gotowa, zgodnie z opisem w rozdziału [wdrożenia scenariuszy z maszyny wirtualne dla systemu SAP w programie Microsoft Azure] [wdrożenia przewodnik-3], Agent maszyn wirtualnych Azure jest zainstalowany na komputerze. Ważne następnym krokiem jest wdrożenie Azure rozszerzony monitorowania rozszerzenie SAP, który jest dostępny w repozytorium rozszerzenia Azure w globalnej centrach danych programu Microsoft Azure. Aby uzyskać więcej informacji, sprawdź, czy [planowania i przewodnik po implementacji] [planowania przewodnik-9.1]. 

Azure programu PowerShell lub interfejsu wiersza polecenia Azure służy do instalowania i konfigurowania Azure rozszerzony monitorowania rozszerzenie SAP. Przeczytaj rozdział — Azure PowerShell [wdrożenia — przewodnik-4.5.1] Jeśli chcesz zainstalować rozszerzenie w systemie Windows lub Linux maszyn wirtualnych przy użyciu komputera systemu Windows. Dotyczące instalowania rozszerzenie na maszyny Linux przy użyciu Linux pulpitu przeczytaj rozdział — Azure polecenie [wdrożenia — przewodnik 4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell dla Linux i maszyny wirtualne systemu Windows
Aby wykonać zadanie instalowania Azure rozszerzony monitorowania rozszerzenie SAP, wykonaj następujące czynności:

* Upewnij się, że zainstalowano najnowszą wersję pakietu Microsoft Azure programu PowerShell polecenia cmdlet. Zobacz rozdział [wdrażanie Azure poleceń cmdlet] [wdrożenia — przewodnik-4.1] tego dokumentu.  
* Uruchom następujące polecenie cmdlet programu PowerShell. Aby uzyskać listę dostępnych środowisk uruchomić polecenia Get-AzureRmEnvironment. Jeśli chcesz używać publicznej Azure środowiska jest AzureCloud. Azure w Chinach wybierz AzureChinaCloud.

```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>
    
    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

Po udostępniane dane konto i maszyny wirtualnej Azure, skrypt wdrożenia wymagane rozszerzenia i włączenie funkcji wymagane. To może potrwać kilka minut.
Przeczytaj [Ten artykuł w witrynie MSDN] [ msdn-set-azurermvmaemextension] uzyskać więcej informacji o AzureRmVMAEMExtension zestawu.
  
![Ekran wynik pomyślnym wykonaniu SAP określonych Azure polecenia cmdlet Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

Pomyślne uruchomienie zestawu AzureRmVMAEMExtension wykona wszystkie kroki niezbędne do skonfigurowania hosta monitorowania funkcji SAP. 

Dane wyjściowe, które powinny wprowadzić skrypt wygląda:

* Potwierdzenie, że konfiguracji monitorowania na podstawie wirtualny dysk twardy, (zawierający system operacyjny) oraz wszystkie dodatkowe wirtualnych dysków twardych zainstalowany do maszyn wirtualnych została skonfigurowana.
* Następnych dwóch wiadomości są Potwierdzenie konfiguracji metryki magazynowania dla konta określonego miejsca do magazynowania. 
* Jeden wiersz danych wyjściowych da stan na rzeczywista aktualizacja konfiguracji monitorowania.
* Inny będą widoczne Potwierdzanie czy konfiguracji został wdrożony lub zaktualizowane.
* Ostatni wiersz danych wyjściowych jest informacyjne, przedstawiający możliwość Przetestuj konfigurację monitorowania.
* Aby sprawdzić, czy wszystkie kroki Azure rozszerzony monitorowania pomyślnie wykonany i infrastruktury Azure znajdują się dane niezbędne, kontynuować Sprawdź gotowości Azure rozszerzony monitorowania rozszerzenie SAP, zgodnie z opisem w rozdziału [gotowości do wyszukania Azure rozszerzony monitorowania SAP] [wdrożenia przewodnik-5.1] w tym dokumencie. 
* Aby kontynuować, w ten sposób, zaczekaj 15-30 minut diagnostyki Azure uzyskuje odpowiednie dane zbierane.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Polecenie Azure dla maszyny wirtualne Linux

Aby wykonać zadanie instalowania Azure rozszerzony monitorowania rozszerzenie SAP przy użyciu interfejsu wiersza polecenia Azure, wykonaj następujące czynności:

1. Instalowanie polecenie Azure, zgodnie z opisem w [tym] [ azure-cli] artykuł
1. Zaloguj się za pomocą konta usługi Azure

    ```
    azure login
    ```
1. Przełącz do trybu Menedżer zasobów Azure

    ```
    azure config mode arm
    ```
1. Włącz Azure monitorowanie rozszerzone

    ```
    azure vm enable-aem <resource-group-name> <vm-name>
    ```  
1. Upewnij się, że Azure rozszerzony monitorowania jest aktywny w maszyn wirtualnych Linux Azure. Sprawdź, czy istnieje /var/lib/AzureEnhancedMonitor/PerfCounters pliku. Jeśli istnieje, Wyświetl informacje zgromadzone przez AEM z:

    ```
    cat /var/lib/AzureEnhancedMonitor/PerfCounters
    ```
    Następnie zostanie wyświetlony wynik, takich jak:
    
    ```
    2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
    2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
    …
    …
    ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Testy i rozwiązywanie problemów dotyczących monitorowania zakończenia do końca Instalatora systemu SAP Azure
Po wdrożeniu maszyn wirtualnych usługi Azure i konfigurowanie odpowiednich Azure infrastruktury monitorowania, sprawdź, czy wszystkie składniki Azure rozszerzony monitorowania działają w sposób pisane z wielkiej litery. 

W związku z tym, wykonywanie sprawdzanie gotowości Azure rozszerzony monitorowania rozszerzenie SAP zgodnie z opisem w rozdziału [gotowości do wyszukania Azure rozszerzony monitorowania SAP] [wdrożenia przewodnik 5.1]. Jeśli wynik ten test jest dodatni, zostanie wyświetlony wszystkie liczniki wydajności odpowiednich Azure monitorowania została skonfigurowana pomyślnie. W tym przypadku kontynuować instalacji agenta hosta SAP zgodnie z opisem w notatkach SAP wymienionych w rozdziału — SAP zasoby [wdrożenia — przewodnik 2.2] tego dokumentu. Jeśli wynik sprawdzanie gotowości wskazuje brak liczniki, przejdź wykonywania kontroli infrastruktury monitorowania Azure, zgodnie z opisem w rozdziału [Sprawdzanie kondycji Konfiguracja monitorowania infrastruktury Azure] [wdrożenia — przewodnik 5,2]. W przypadku jakichkolwiek problemów z konfiguracją monitorowania Azure sprawdzanie rozdziału [dalszego monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP] [wdrożenia — przewodnik-5.3] do dalszej pomocy dotyczącej rozwiązywania problemów.

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Sprawdzanie gotowości Azure rozszerzonego monitorowania SAP
Z ten test należy upewnić się, całkowicie dostarczane metryk, które będą wyświetlane w aplikacji SAP przez podstawowej infrastruktury Azure. 

#### <a name="execute-the-readiness-check-on-a-windows-vm"></a>Wykonaj sprawdzenie gotowości dla maszyn wirtualnych systemu Windows
Aby wykonać sprawdzanie gotowości logowania do Azure maszyny wirtualnej (konta administratora nie jest to konieczne) i wykonaj następujące czynności:

* Otwórz okno wiersza polecenia systemu Windows i przejdź do folderu instalacji rozszerzenia monitorowania Azure dla SAP C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop

Część wersji opisane w ścieżce do monitorowania rozszerzenia powyżej mogą się różnić. Jeśli zobaczysz wiele folderów monitorowania wersja rozszerzenia w folderze instalacji sprawdź konfigurację usługi Windows "AzureEnhancedMonitoring" i przejść do folderu, który został oznaczony jako "Ścieżkę do wykonywalny".
 
![Właściwości usługi uruchamiania Azure rozszerzony monitorowania rozszerzenie SAP][deployment-guide-figure-1000]

* W oknie wiersza polecenia bez parametrów, należy wykonać azperflib.exe.

> [AZURE.NOTE] Azperflib.exe jest uruchamiany w pętli i aktualizuje zebrane liczniki co 60 sekund. Aby zakończyć pętli, zamknij okno poleceń.

Jeśli nie zainstalowano Azure rozszerzony monitorowania rozszerzenia lub usługa "AzureEnhancedMonitoring" nie jest uruchomiona, rozszerzenie nie skonfigurowano poprawnie. W tym przypadku należy wykonać rozdziału [dalszego monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP] [wdrożenia — przewodnik-5.3] Aby uzyskać szczegółowe instrukcje jak ponownie rozmieścić rozszerzenia.

##### <a name="check-the-output-of-azperflibexe"></a>Zaznacz dane wyjściowe azperflib.exe
Wynik azperflib.exe pokazuje wszystkie wypełnione liczniki wydajności Azure SAP. U dołu listy liczników zebrane możesz znaleźć podsumowanie i wskaźnika kondycji, który wskazuje stan monitorowania Azure. 
 
![Wynik kontroli przez wykonywanie azperflib.exe wskazująca, że istnieje żadnych problemów][deployment-guide-figure-1100]
<a name="figure-11"></a>

Sprawdź wynik zwracany wyników ilość "Liczniki ogółem", który zgłoszone jako puste i sprawdzanie kondycji przedstawione powyżej ilustracji [powyżej][deployment-guide-figure-11].

Można interpretować wartości wyników w następujący sposób:

| Wartości Azperflib.exe wyników | Monitorowanie stanu gotowości Azure |
| ------------------------------|----------------------------------- |
| **Suma liczniki: puste** | Następujące liczniki Azure magazynowania 2 mogą być puste: <ul><li>Otwórz opóźnienie więcej miejsca do magazynowania MS serwera</li><li>Otwórz opóźnienie więcej miejsca do magazynowania MS E2E</li></ul>Wszystkie inne liczniki musi zawierać wartości. |
| **Sprawdzanie kondycji** | Tylko na OK, jeśli zwrotu stan jest wyświetlany przycisk OK |

Jeśli nie zwrócą wartości azperflib.exe pokazują, że wszystkie liczniki wypełnione są prawidłowo zwracane, postępuj zgodnie z instrukcjami lekarskie konfiguracji infrastruktury monitorowania Azure zgodnie z opisem w rozdziału [Sprawdzanie kondycji Konfiguracja monitorowania infrastruktury Azure] [wdrożenia — przewodnik 5,2] poniżej.

#### <a name="execute-the-readiness-check-on-a-linux-vm"></a>Wykonaj sprawdzenie gotowości dla maszyny Linux
Aby wykonać sprawdzanie gotowości, łączenie się z SSH do maszyny wirtualnej Azure i wykonaj następujące czynności:

* Zaznacz dane wyjściowe Azure rozszerzony monitorowania rozszerzenia
    * więcej /var/lib/AzureEnhancedMonitor/PerfCounters
        * Powinien podać Ci lista liczników wydajności. Plik nie powinna być pusta
    * cat /var/lib/AzureEnhancedMonitor/PerfCounters | GREP błędu
        * Powinna zwrócić jeden wiersz, gdzie błąd jest brak np 3; konfiguracji; Błąd; 0; 0; **Brak**; 0; 1456416792; tst servercs;
    * więcej /var/lib/AzureEnhancedMonitor/LatestErrorRecord
        * Powinna być pusta lub czy nie istnieje
* Jeśli pierwszy wyboru powyżej nie zakończyło się pomyślnie, należy wykonać testy dodatkowe:
    * Upewnij się, że waagent instalowania i uruchamiania
        * sudo ls-al/var/lib/waagent /
            * czy listy zawartości katalogu waagent
        * PS-ax | GREP waagent
            * należy wyświetlić jeden wpis podobny do "python /usr/sbin/waagent-demon"
    * Upewnij się, instalowania i uruchamiania rozszerzenia Linux Diagnostic
        * sudo Pokaż - c "ls-al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-*"
            * czy listy zawartości katalogu rozszerzenia diagnostyczne Linux
        * PS-ax | GREP diagnostyczne
            * należy wyświetlić jeden wpis podobny do "python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py-demon"
    * Upewnij się, instalowania i uruchamiania Azure rozszerzony monitorowania rozszerzenia
        * sudo Pokaż - c "ls-al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/"
            * czy listy zawartości katalogu rozszerzenia rozszerzony monitorowania Azure
        * PS-ax | GREP AzureEnhanced
            * mają się pojawić jeden wpis podobny do "python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py demon"
* Instalowanie agenta hosta SAP, zgodnie z opisem w Uwaga systemu SAP [1031096] i sprawdzić wynik saposcol
    * Wykonywanie /usr/sap/hostctrl/exe/saposcol -d
    * Wykonywanie zrzutu ccm
    * Sprawdź, czy PRAWDA jest metryczne "Virtualization_Configuration\Enhanced kontroli dostępu"
* Jeśli masz już zainstalowany serwer aplikacji SAP NetWeaver ABAP, otwórz transakcji ST06 i wyboru po włączeniu rozszerzonego monitorowania.

Jeśli dowolny z kontroli nad Fail (niepowodzenie), wykonaj rozdziału [dalszego monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP] [wdrożenia — przewodnik-5.3] Aby uzyskać szczegółowe instrukcje jak ponownie rozmieścić rozszerzenia.

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Sprawdzanie kondycji Azure monitorowania infrastruktury konfiguracji
Jeśli niektóre monitorowania danych nie są dostarczane poprawnie wskazywane przez badanie opisane w rozdziału [gotowości do wyszukania Azure rozszerzony monitorowania SAP] [wdrożenia przewodnik 5.1] powyżej, wykonanie polecenia cmdlet Test AzureRmVMAEMExtension, aby sprawdzić, czy bieżącą konfigurację infrastruktury Azure monitorowania i rozszerzenia monitorowania SAP jest poprawny.

Aby przetestować konfigurację monitorowania, wykonaj następującą kolejność:

* Upewnij się, że zainstalowano najnowszą wersję pakietu Microsoft Azure programu PowerShell polecenia cmdlet, zgodnie z opisem w rozdziału [wdrażanie Azure poleceń cmdlet] [wdrożenia — przewodnik-4.1] tego dokumentu.
* Uruchom następujące polecenie cmdlet programu PowerShell. Aby uzyskać listę dostępnych środowisk uruchomić polecenia Get-AzureRmEnvironment. Jeśli chcesz używać publicznej Azure środowiska jest AzureCloud. Azure w Chinach wybierz AzureChinaCloud.

```powershell
$env = Get-AzureRmEnvironment -Name <name of the environment>
Login-AzureRmAccount -Environment $env
Set-AzureRmContext -SubscriptionName <subscription name>
Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

* Po udostępniane dane konto i maszyny wirtualnej Azure, skrypt będzie Przetestuj konfigurację maszyny wirtualnej przez Ciebie.

 
![Ekran wprowadzania SAP określonych Azure polecenia cmdlet Test VMConfigForSAP_GUI][deployment-guide-figure-1200]

Po wprowadzeniu informacji o Twoje konto i maszyny wirtualnej Azure skrypt będzie Przetestuj konfigurację maszyny wirtualnej przez Ciebie.
 
![Wynik testu pomyślnego Azure monitorowania infrastruktury dla SAP][deployment-guide-figure-1300]

Upewnij się, co wyboru są oznaczane przycisk OK. Jeśli niektóre testy są ok, wykonaj polecenia cmdlet Aktualizuj zgodnie z opisem w rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5] tego dokumentu. Poczekaj innego 15 minut i sprawdzanie opisanego w rozdziału [gotowości do wyszukania Azure rozszerzony monitorowania SAP] [wdrożenia przewodnik 5.1] i [Sprawdzanie kondycji Konfiguracja monitorowania infrastruktury Azure] [wdrożenia — przewodnik 5,2] ponownie. Jeśli testy nadal wskazywać na problem z niektórych lub wszystkich liczników, przejdź do rozdziału [dalszego monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP] [wdrożenia — przewodnik 5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Dalsze monitorowania Azure infrastruktury Rozwiązywanie problemów związanych z SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Systemu Windows][Logo_Windows] Liczniki wydajności Azure nie są wyświetlane w ogóle
Kolekcja wskaźniki Azure jest wykonywane przez usługę systemu Windows "AzureEnhancedMonitoring". Jeśli usługa nie został zainstalowany poprawnie lub nie jest uruchomiony w swojej maszyn wirtualnych, nie wskaźniki mogą pochodzić wcale.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Katalog instalacji rozszerzenia Azure rozszerzony monitorowania jest pusty 

###### <a name="issue"></a>Problem
Katalog instalacji C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop jest pusta.

###### <a name="solution"></a>Rozwiązanie
Rozszerzenie nie jest zainstalowany. Sprawdź, jeśli jest to problem serwera proxy (zgodnie z opisem przed). Konieczne może być ponownie uruchomić komputer i/lub uruchom ponownie skryptu skrypt konfiguracji AzureRmVMAEMExtension zestawu

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Usługa Azure rozszerzony monitorowania nie istnieje. 

###### <a name="issue"></a>Problem
Usługa Windows "AzureEnhancedMonitoring" nie istnieje. Azperflib.exe: Wynik azperlib.exe zgłasza błąd, jak pokazano na [poniższym rysunku][deployment-guide-figure-14].
 
![Wykonanie azperflib.exe wskazuje, czy usługa Azure rozszerzony monitorowania rozszerzenia SAP nie jest uruchomiony][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Rozwiązanie
Jeśli usługa nie istnieje, jak pokazano na [ilustracji powyżej][deployment-guide-figure-14], Azure rozszerzenie monitorowania SAP nie został zainstalowany poprawnie. Ponownie wdróż rozszerzenia zgodnie z procedurą dla danego scenariusza wdrażania w rozdziału [wdrożenia scenariusze: maszyny wirtualne dla systemu SAP w programie Microsoft Azure] [wdrożenia przewodnik-3]. 

Po wdrożeniu rozszerzenia Sprawdź ponownie czy liczników wydajności Azure znajdują się w obrębie maszyn wirtualnych Azure po 1 godzina.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Usługa Azure rozszerzony monitorowania istnieje, ale nie można uruchomić 

###### <a name="issue"></a>Problem
Usługa Windows "AzureEnhancedMonitoring" istnieje i jest włączona, ale nie można uruchomić. Przejrzyj dziennik zdarzeń aplikacji, aby uzyskać więcej informacji.

###### <a name="solution"></a>Rozwiązanie
Nieprawidłowe konfiguracji. Ponowne włączanie monitorowania rozszerzenia dla maszyn wirtualnych, zgodnie z opisem w rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Systemu Windows][Logo_Windows] Brakuje niektórych liczniki wydajności Azure
Kolekcja wskaźniki Azure jest wykonywane przez usługę systemu Windows "AzureEnhancedMonitoring", który pobiera dane z różnych źródeł. Niektóre dane konfiguracyjne są zbierane lokalnie, wskaźniki są odczytywane z diagnostyki Azure i liczniki przestrzeni dyskowej są używane z usługi rejestrowania na poziomie subskrypcji miejsca do magazynowania.

Jeśli Rozwiązywanie problemów przy użyciu Uwaga systemu SAP [1999351] pomaga, uruchom ponownie skrypt konfiguracji AzureRmVMAEMExtension zestawu. Może być konieczne oczekiwania przez godzinę, ponieważ nie można utworzyć liczników analizy lub Diagnostyka miejsca do magazynowania, natychmiast po te funkcje są włączone. Jeśli problem nadal występuje, otwórz wiadomości obsługi klienta SAP na składniku BC-Otwórz-NT-AZR.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Liczniki wydajności Azure nie są wyświetlane w ogóle

Kolekcja wskaźniki Azure odbywa się przez deamon. Jeśli deamon nie jest uruchomiony, nie wskaźniki mogą pochodzić wcale.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Katalog instalacji rozszerzenia Azure rozszerzony monitorowania jest pusty 

###### <a name="issue"></a>Problem
Katalogu/var/biblioteki/waagent/nie zawiera podkatalogów dla rozszerzenia Azure rozszerzony monitorowania.

###### <a name="solution"></a>Rozwiązanie
Rozszerzenie nie jest zainstalowany. Sprawdź, jeśli jest to problem serwera proxy (zgodnie z opisem przed). Konieczne może być ponownie uruchomić komputer i/lub ponowne uruchamianie skryptu konfiguracji AzureRmVMAEMExtension zestawu

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Brakuje niektórych liczniki wydajności Azure

Kolekcja wskaźniki Azure jest wykonywane przez demon, który pobiera dane z różnych źródeł. Niektóre dane konfiguracyjne są zbierane lokalnie, wskaźniki są odczytywane z diagnostyki Azure i liczniki przestrzeni dyskowej są używane z usługi rejestrowania na poziomie subskrypcji miejsca do magazynowania.

Aby kompletne i aktualne listę znanych problemów, zobacz Uwaga systemu SAP [1999351] zawierający dodatkowe informacje dotyczące rozwiązywania problemów dla rozszerzony Azure monitorowania dla SAP.

Jeśli Rozwiązywanie problemów przy użyciu Uwaga systemu SAP [1999351] pomaga, uruchom ponownie skrypt konfiguracji zestawu AzureRmVMAEMExtension zgodnie z opisem w rozdziału [Konfigurowanie Azure rozszerzony monitorowania rozszerzenie SAP] [wdrożenia — przewodnik 4,5]. Może być konieczne oczekiwania przez godzinę, ponieważ nie można utworzyć liczników analizy lub Diagnostyka miejsca do magazynowania, natychmiast po te funkcje są włączone. Jeśli problem nadal występuje, Otwórz wiadomość pomocy technicznej klienta SAP na składnik BC-Otwórz-NT-AZR dla systemu Windows lub BC-Otwórz-LNX-AZR dla maszyny wirtualnej Linux.
