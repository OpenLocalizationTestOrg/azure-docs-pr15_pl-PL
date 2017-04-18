<properties
    pageTitle="Automatyczne skalowanie usługi w chmurze w portalu | Microsoft Azure"
    description="(klasyczny) Dowiedz się, jak korzystać z portalu klasyczny skonfigurować automatyczne skali zasady chmury usługi sieci web rola lub pracownika w Azure."
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

Na stronie skali Azure portalu klasyczny ręcznie można skalować usługi sieci web rola lub pracownika, lub możesz włączyć automatyczne skalowanie na podstawie obciążenie Procesora lub kolejki wiadomości.

>[AZURE.NOTE] W tym artykule omówiono ról w sieci web i pracownik usługi w chmurze. Po utworzeniu bezpośrednio maszyny wirtualnej (klasyczny), jest ona hostowana w usłudze w chmurze. Niektóre z tych informacji dotyczy to następujących typów maszyn wirtualnych. Skalowanie zestawu dostępność maszyn wirtualnych rzeczywistości po prostu kończy ich włączanie i wyłączanie funkcji zgodnie z regułami skali, które można skonfigurować. Aby uzyskać więcej informacji o środowisku maszyn wirtualnych systemu i zestawy dostępności zobacz [Zarządzanie dostępność maszyn wirtualnych](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Przed skonfigurowaniem skalowania aplikacji, należy rozważyć następujące informacje:

- Skalowanie ma wpływ zastosowania podstawowych. Większe wystąpienia roli za pomocą rdzeni. Aplikacja tylko w granicach rdzenie można skalować dla subskrypcji. Na przykład jeśli Twoja subskrypcja ma limit dwudziestu rdzenie i uruchomić aplikację z dwóch średni o rozmiarze cloud services (łącznie cztery rdzenie), można tylko skalować innych wdrożeniach usługi cloud w górę w ramach subskrypcji przez rdzenie szesnastu. Aby uzyskać więcej informacji na temat rozmiarów, zobacz [Rozmiarów usługi w chmurze](cloud-services-sizes-specs.md) .

- Należy utworzyć kolejkę i skojarzyć z roli przed można skalować aplikacji opartej na progu wiadomości. Aby uzyskać więcej informacji zobacz [jak korzystać z usługi Magazyn kolejki](../storage/storage-dotnet-how-to-use-queues.md).

- Można skalować zasoby, które są połączone z usługi w chmurze. Aby uzyskać więcej informacji na temat łączenia zasobów, zobacz [jak: łączenie zasobu do usługi w chmurze](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Aby włączyć wysoką dostępność aplikacji, należy upewnić się, wdrożenie z dwóch lub więcej wystąpień roli. Aby uzyskać więcej informacji zobacz [Świadczeniu](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Planowanie skalowania

Domyślnie wszystkie role nie są zgodne określonego harmonogramu. W związku z tym zmienić ustawienia dotyczą wszystkie dni i godziny wszystkie w ciągu roku. Jeśli chcesz, możesz skonfigurować ręczne i automatyczne skalowanie:

- Dni tygodnia
- Dni weekendowe
- Nocy tydzień
- Mornings tydzień
- Określone daty
- Zakresy dat

Jest to conigured w [portal Azure klasyczny](https://manage.windowsazure.com/)  
**Usług w chmurze** > **\[usługi w chmurze\]** > **skali** > **\[produkcji lub tymczasowego\] ** strony.

Kliknij przycisk **Konfigurowanie uruchomień** dla każdej roli, którą chcesz zmienić.

![Automatyczne skalowanie chmury usługi zgodnie z harmonogramem][scale_schedules]



## <a name="manual-scale"></a>Ręczne skali

Na stronie **skali** ręcznie można zwiększyć lub zmniejszyć liczbę uruchomione wystąpienia w usłudze w chmurze. Jeśli nie utworzono serii rozłożonych w czasie, to jest skonfigurowany do każdego harmonogram, który został utworzony lub cały czas.

1. W [portal Azure klasycznym](https://manage.windowsazure.com/)kliknij **Usług w chmurze**, a następnie kliknij nazwę usługi w chmurze otworzyć pulpitu nawigacyjnego.

    > [AZURE.TIP] Jeśli nie widać usługi w chmurze, może być konieczne zmiany z **produkcji** do **przemieszczania** lub odwrotnie.

2. Kliknij pozycję **Skala**.

3. Wybierz pozycję Harmonogram, aby zmienić opcje skalowania. Domyślnie *nr zaplanowanych terminach* Jeśli masz nie harmonogramów zdefiniowanych.

4. Znajdź sekcję **skali przez jednostki metryczne** i wybierz pozycję **Brak**. Jest to ustawienie domyślne dla wszystkich ról.

5. Każda rola w usłudze w chmurze ma suwak dotyczące zmieniania liczby wystąpień korzystać.

    ![Ręczne skalowanie rolę usługi chmury][manual_scale]

    Jeśli potrzebujesz więcej wystąpień, może być konieczne zmienić [rozmiar maszyn wirtualnych usługi w chmurze](cloud-services-sizes-specs.md).

6. Kliknij przycisk **Zapisz**.  
Wystąpienia roli dodane lub usunięte w zależności od ustawień.

>[AZURE.TIP] Gdy zostanie wyświetlony ![][tip_icon] Przenieś wskaźnik myszy i można uzyskać pomoc dotyczącą ustawienie określone.


## <a name="automatic-scale---cpu"></a>Automatyczne Skala - Procesora

To skale, jeśli średnia procentowe użycie Procesora powyżej lub poniżej określonego progi; Rola wystąpienia są tworzone lub usuwane.

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/)kliknij **Usług w chmurze**, a następnie kliknij nazwę usługi w chmurze do otwierania pulpitu nawigacyjnego.

    > [AZURE.TIP] Jeśli nie widać usługi w chmurze, może być konieczne zmiany z **produkcji** do **przemieszczania** lub odwrotnie.

2. Kliknij pozycję **Skala**.

3. Wybierz pozycję Harmonogram, aby zmienić opcje skalowania. Domyślnie *nr zaplanowanych terminach* Jeśli masz nie harmonogramów zdefiniowanych.

4. Znajdź sekcję **skali przez jednostki metryczne** i wybierz pozycję **Procesora**.

5. Teraz można skonfigurować wartości minimalne i maksymalne zakres wystąpień role, docelowej użycie Procesora (wyzwalać skalę w górę) i ile wystąpień przeskalować w górę i w dół przez.

![Skalowanie rolę usługi cloud przez obciążenie procesora][cpu_scale]

>[AZURE.TIP] Gdy zostanie wyświetlony ![][tip_icon] Przenieś wskaźnik myszy i można uzyskać pomoc dotyczącą ustawienie określone.





## <a name="automatic-scale---queue"></a>Automatyczne skali — kolejki

To jest automatycznie skalowany Jeśli liczba wiadomości w kolejce powyżej lub poniżej określonej wartości progowej; Rola wystąpienia są tworzone lub usuwane.

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/)kliknij **Usług w chmurze**, a następnie kliknij nazwę usługi w chmurze do otwierania pulpitu nawigacyjnego.

    > [AZURE.TIP] Jeśli nie widać usługi w chmurze, może być konieczne zmiany z **produkcji** do **przemieszczania** lub odwrotnie.

2. Kliknij pozycję **Skala**.

3. Znajdź sekcję **skali przez jednostki metryczne** i wybierz pozycję **Procesora**.

4. Teraz można skonfigurować wartości minimalne i maksymalne zakres wystąpień role, kolejki i liczby wiadomości w kolejce przetwarzania dla każdego wystąpienia i ile wystąpień przeskalować w górę i w dół przez.

![Skalowanie rolę usługi cloud w kolejce wiadomości][queue_scale]

>[AZURE.TIP] Gdy zostanie wyświetlony ![][tip_icon] Przenieś wskaźnik myszy i można uzyskać pomoc dotyczącą ustawienie określone.


## <a name="scale-linked-resources"></a>Skalowanie połączonych zasobów

Gdy przeskalować roli, jest korzystne przeskalować bazy danych, która także używa aplikacji. Jeśli baza danych jest łącze do usługi w chmurze, masz dostęp do ustawień skalowania dla tego zasobu, klikając na odpowiednie łącze.

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/)kliknij **Usług w chmurze**, a następnie kliknij nazwę usługi w chmurze otworzyć pulpitu nawigacyjnego.

    > [AZURE.TIP] Jeśli nie widać usługi w chmurze, może być konieczne zmiany z **produkcji** do **przemieszczania** lub odwrotnie.

2. Kliknij pozycję **Skala**.

3. Znajdź sekcję **połączonych zasobów** i kliknięciu skali **Zarządzanie dla tej bazy danych**.

    > [AZURE.NOTE] Jeśli nie widać sekcji **zasoby połączone** , prawdopodobnie nie masz żadnych połączonych zasobów.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
