<properties
    pageTitle="Tworzenie farmy serwerów programu SharePoint | Microsoft Azure"
    description="Szybkie tworzenie nowej farmy serwerów programu SharePoint 2013 lub 2016 programu SharePoint w Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>Tworzenie farmy serwerów programu SharePoint

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klasyczny model.

## <a name="sharepoint-2013-farms"></a>Farmy programu SharePoint 2013

Z portalu Microsoft Azure marketplace portalu można szybko utworzyć wstępnie skonfigurowane farmy programu SharePoint Server 2013. To pozwalają zaoszczędzić dużo czasu przeszukanie podstawowe lub wysokiej dostępności farmy programu SharePoint dla deweloperów/testowania lub jeśli użytkownik ocenia programu SharePoint Server 2013 jako rozwiązanie współpracy dla Twojej organizacji.

> [AZURE.NOTE] Usunięto element **Farmy serwerów programu SharePoint** Azure portal w Azure Marketplace. Zamieniono elementami **Farmie programu SharePoint 2013 non-HA** i **Farmie programu SharePoint 2013 HA** .

Podstawowe farmie programu SharePoint składa się z trzech maszyn wirtualnych w tej konfiguracji.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Za pomocą tej konfiguracji farmy uproszczone ustawienia aplikacji dla programu SharePoint lub ocenę po raz pierwszy programu SharePoint 2013.

Aby utworzyć podstawowe farmie programu SharePoint (3 serwer):

1. Kliknij [tutaj](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Kliknij pozycję **wdrożenie**.
3. W okienku **Farmie programu SharePoint 2013 non-HA** kliknij przycisk **Utwórz**.
4. Ustawienia dotyczące czynności okienka **Tworzenie farmie programu SharePoint 2013 non-HA** i kliknij przycisk **Utwórz**.

Farmie programu SharePoint wysokiej dostępności składa się z dziewięciu maszyn wirtualnych w tej konfiguracji.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Za pomocą tej konfiguracji farmy do przetestowania wyższą klienta obciążenia, wysoką dostępność zewnętrznej witryny programu SharePoint i SQL Server (AlwaysOn) Dostępność grupy farmy programu SharePoint. Za pomocą tej konfiguracji dla aplikacji programu SharePoint w środowisku wysokiej dostępności.

Aby utworzyć wysokiej dostępności farmie programu SharePoint (9 serwer):

1. Kliknij [tutaj](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Kliknij pozycję **wdrożenie**.
3. W okienku **Farmie programu SharePoint 2013 HA** kliknij przycisk **Utwórz**.
4. Określanie ustawień na siedem kroków okienka **Tworzenie farmie programu SharePoint 2013 HA** , a następnie kliknij przycisk **Utwórz**.

> [AZURE.NOTE] Nie można utworzyć **Farmie programu SharePoint 2013 non-HA** lub **Farmie programu SharePoint 2013 HA** z bezpłatną wersję próbną Azure.

Azure portal tworzy oba te farmach tylko do chmury wirtualnej sieci z Internetu witrynę internetową. Istnieje połączenie VPN lub ExpressRoute witryny powrót do sieci organizacji.

> [AZURE.NOTE] Podczas tworzenia podstawowego lub wysokiej dostępności farmy programu SharePoint za pomocą portalu Azure, nie można określić istniejącej grupy zasobów. Aby obejść to ograniczenie, należy utworzyć te farmach z programem PowerShell Azure. Aby uzyskać więcej informacji zobacz [Tworzenie programu SharePoint 2013 deweloperów/testowanie farmach z Azure programu PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>Farmy programu SharePoint 2016

Zobacz [Ten artykuł](https://technet.microsoft.com/library/mt723354.aspx) , aby uzyskać instrukcje dotyczące tworzenia następujące farmie programu SharePoint Server 2016 jednego serwera.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Zarządzanie farmy programu SharePoint

Można administrować serwerami tych farm za pośrednictwem połączenia pulpitu zdalnego. Aby uzyskać więcej informacji zobacz [Logowanie maszyn wirtualnych](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Z witryny programu SharePoint administracji centralnej można skonfigurować witryn Moja witryna, aplikacji programu SharePoint i innych funkcji. Aby uzyskać więcej informacji zobacz [Konfigurowanie programu SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Następne kroki

- Poznaj dodatkowe [konfiguracji programu SharePoint](https://technet.microsoft.com/library/dn635309.aspx) w usługach Azure infrastruktury.
