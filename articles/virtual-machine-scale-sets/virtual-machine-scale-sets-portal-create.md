<properties
    pageTitle="Tworzenie maszyn wirtualnych skali zestawu za pomocą portalu Azure | Microsoft Azure"
    description="Wdrażanie zestawów skali za pomocą portalu Azure."
    keywords="zestawy skali maszyn wirtualnych" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Tworzenie maszyn wirtualnych skali zestawu za pomocą portalu Azure

W tym samouczku pokazano, jak łatwo jest tworzenie zestawu skali maszyn wirtualnych w ciągu kilku minut, przy użyciu Azure portal. Jeśli nie masz subskrypcji usługi Azure, przed rozpoczęciem należy utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) .

## <a name="choose-the-vm-image-from-the-marketplace"></a>Wybierz obraz maszyn wirtualnych z witryny marketplace

Z poziomu portalu można łatwo rozmieścić skalę zestaw z CentOS, CoreOS, Debian, otwórz Suse, Red funkcję Enterprise Linux, SUSE Linux Enterprise Server, serwer Ubuntu lub obrazy systemu Windows Server.

Najpierw przejdź do [portalu Azure](https://portal.azure.com) w przeglądarce sieci web. Kliknij pozycję `New`, wyszukaj `scale set`, a następnie wybierz pozycję `Virtual machine scale set` wpis:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Tworzenie maszyny wirtualnej Linux

Teraz możesz korzystać z domyślnych ustawień i szybko utworzyć maszyny wirtualnej.

* Na `Basics` karta, wprowadź nazwę zestawu Skala. Ta nazwa zostaje podstawy Kwalifikowaną równoważenia obciążenia przed zestawu skali, dlatego należy się upewnić, że nazwa jest unikatowa we wszystkich Azure.

* Wybierz pozycję system operacyjny odpowiedniej wpisz, wprowadź nazwę odpowiedniej użytkownika i wybierz typ uwierzytelniania, które wolisz. Jeśli wybierzesz hasła, musi to być co najmniej 12 znaków długa i spełniają trzy z czterech następujące wymagania złożoności: znak jeden małe litery, wielkie jeden znak jedną liczbę i jeden znak specjalny. Zobacz więcej informacji na temat [wymagań nazwy użytkownika i hasła](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Jeśli wybierzesz `SSH public key`należy upewnić się, że tylko Wklej w klucz publiczny, nie klucz prywatny:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Wprowadź nazwę grupy zasobem i lokalizacji, a następnie kliknij pozycję `OK`.

* Na `Virtual machine scale set service settings` karta: Wprowadź etykiety nazwy żądanej domeny (podstawę nazwy FQDN równoważenia obciążenia przed zestawu skali). Etykieta musi być unikatowa we wszystkich Azure.

* Wybierz żądany system operacyjny obrazu dysku, Zliczanie wystąpień i rozmiarów.

* Włączanie lub wyłączanie autoscale i konfigurowanie Jeśli włączono tę funkcję:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Na `Summary` kliknij pozycję Karta po zakończeniu sprawdzania poprawności `OK`.

* Ponadto na `Purchase` karta, kliknij przycisk `Purchase` zacząć Skala Ustaw wdrożenia.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Nawiązywanie połączenia z maszyn wirtualnych w zestawie skali

Po wdrożeniu zestawu skali przejdź do `Inbound NAT Rules` kartę równoważenia obciążenia dla zestawu skali:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Można nawiązać poszczególnych maszyn wirtualnych w skali zestaw przy użyciu tych reguł translatora adresów Sieciowych. Na przykład dla zestawu skali systemu Windows, jeśli istnieje reguła translatora adresów Sieciowych na porcie przychodzących 50 000 możesz można połączyć na komputerze za pośrednictwem RDP na `<load-balancer-ip-address>:50000`. Dla zestawu skali Linux można połączyć za pomocą polecenia `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Następne kroki

Dokumentację na temat instalacji zestawów skali z interfejsu wiersza polecenia można znaleźć w [dokumentacji](./virtual-machine-scale-sets-cli-quick-create.md).

Dokumentacji na temat instalacji zestawów skali z programu PowerShell zobacz [Niniejsza dokumentacja](./virtual-machine-scale-sets-windows-create.md).

Dokumentację dotyczącą Wdrażanie zestawów skali z programu Visual Studio, zobacz [Niniejsza dokumentacja](./virtual-machine-scale-sets-vs-create.md).

Dokumentacji ogólne zapoznaj się z [zestawów stronie Omówienie dokumentacji Skala](./virtual-machine-scale-sets-overview.md).

Aby uzyskać ogólne informacje zapoznaj się z [Strona początkowa głównym dla zestawów Skala](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

