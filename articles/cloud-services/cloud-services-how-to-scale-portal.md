<properties
    pageTitle="Automatyczne skalowanie usługi w chmurze w portalu | Microsoft Azure"
    description="Dowiedz się, jak korzystać z portalu skonfigurować automatyczne skali zasady chmury usługi sieci web rola lub pracownika w Azure."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Jak automatyczne skalowanie usługi w chmurze

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-scale-portal.md)
- [Portal Azure klasyczny](cloud-services-how-to-scale.md)

Warunki można ustawić w roli Pracownik usługi cloud wyzwalanie skalę lub pomniejszyć operacji. Warunki roli mogą oparte na Procesora, dysku lub obciążenia sieciowego roli. Można także ustawić conditation, na podstawie kolejki wiadomości lub metryki kilka innych Azure zasobu skojarzonego z subskrypcją usługi.

>[AZURE.NOTE] W tym artykule omówiono ról w sieci web i pracownik usługi w chmurze. Po utworzeniu bezpośrednio maszyny wirtualnej (klasyczny), jest ona hostowana w usłudze w chmurze. Można skalować standardowy maszyny wirtualnej kojarząc go z [zestawu dostępność](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) i ręcznie je włączyć lub wyłączyć.

## <a name="considerations"></a>Zagadnienia dotyczące

Przed skonfigurowaniem skalowania aplikacji, należy rozważyć następujące informacje:

- Skalowanie ma wpływ zastosowania podstawowych. Większe wystąpienia roli za pomocą rdzeni. Aplikacja tylko w granicach rdzenie można skalować dla subskrypcji. Na przykład jeśli Twoja subskrypcja ma limit dwudziestu rdzenie i uruchomić aplikację z dwóch średni o rozmiarze cloud services (łącznie cztery rdzenie), można tylko skalować innych wdrożeniach usługi w chmurze w górę w ramach subskrypcji przez rdzenie szesnastu. Aby uzyskać więcej informacji na temat rozmiarów, zobacz [Rozmiarów usługi w chmurze](cloud-services-sizes-specs.md) .

- Można skalować według wartości progowej kolejki wiadomości. Aby uzyskać więcej informacji o używaniu kolejki, zobacz [jak korzystać z usługi Magazyn kolejki](../storage/storage-dotnet-how-to-use-queues.md).

- Można również skalować inne zasoby skojarzonego z subskrypcją usługi.

- Aby włączyć wysoką dostępność aplikacji, należy upewnić się, wdrożenie z dwóch lub więcej wystąpień roli. Aby uzyskać więcej informacji zobacz [Umowy](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Miejsce, w którym znajduje się skala

Po zaznaczeniu usługi w chmurze, trzeba mieć widoczna karta usługi cloud.

1. Na chmurze usługi karta, na kafelku **ról i wystąpienia** wybierz nazwę usługi w chmurze.   
**Ważne**: Pamiętaj o kliknięciu chmury usługi roli, nie znajduje się poniżej roli wystąpienie roli.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Wybierz Kafelek **Skala** .

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatyczne skalowanie

Możesz skonfigurować ustawienia skali dla roli z dwóch trybów **Ręczne** i **Automatyczne**. Ręczne zgodnie z oczekiwaniami, ustaw bezwzględne liczbę wystąpień. Automatyczne umożliwia jednak do zestawu reguł, określające sposób i, jak dużo możesz należy skalowania.

Ustaw dla opcji **skali według** **harmonogramu i wydajności reguł**.

![Ustawienia skali usługi cloud z profilu i reguły](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Profil.
2. Dodawanie reguły dla profilu nadrzędnej.
3. Dodawanie innego profilu.

Wybierz pozycję **Dodaj profilu**. Profil określa, który ma być używany dla skali tryb: **zawsze** **cyklu**, **Stała data**.

Po skonfigurowaniu reguł i profil, wybierz ikonę **Zapisz** u góry.

#### <a name="profile"></a>Profil

Profil zestawów wartości minimalne i maksymalne wystąpienia skali, a także, kiedy ten zakres skali jest aktywny.

* **Zawsze**

    Ten zakres wystąpień dostępnych na bieżąco.  

    ![Usługa w chmurze, które zawsze skalowania](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Cykl**

    Wybierz pozycję Ustaw dni tygodnia przeskalować.

    ![Skala usługi cloud z harmonogramem cyklu](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Stała daty**

    Stały zakres dat przeskalować roli.

    ![Chmury usługi Skaluj z ustaloną datę](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Po skonfigurowaniu profil, wybierz przycisk **OK** u dołu karta profilu.

#### <a name="rule"></a>Reguły

Reguły są dodawane do profilu i reprezentują warunek, który spowoduje odpowiednią skalę. 

Wyzwalacza reguła jest oparty na metryki usługi cloud (użycie Procesora, aktywność dysku lub aktywności sieci), do którego można dodać wartości warunkowe. Ponadto możesz mieć wyzwalacza na podstawie kolejki wiadomości lub metrykę kilka innych Azure zasobu skojarzonego z subskrypcją usługi.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Po skonfigurowaniu regułę, kliknij przycisk **OK** u dołu karta reguły.

## <a name="back-to-manual-scale"></a>Powrót do ręcznego skali

Przejdź do [ustawień skali](#where-scale-is-located) i ustaw opcję **Skala przez** ****liczy wystąpienia, które można wprowadzić ręcznie.

![Ustawienia skali usługi cloud z profilu i reguły](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Spowoduje to usunięcie automatyczne skalowanie z roli, a następnie bezpośrednio możesz Ustaw licznik wystąpień. 

1. Opcja (ręcznego lub automatycznego) Skala.
2. Suwak wystąpienie roli, aby ustawić wystąpienia przeskalować do.
3. Wystąpienia roli, aby skalować.

Po skonfigurowaniu ustawień skali, wybierz ikonę **Zapisz** u góry.

