<properties
    pageTitle="Ustawianie alertów dla subskrypcje usługi Microsoft Azure rozliczenia | Microsoft Azure"
    description="W tym artykule opisano, jak możesz skonfigurować alerty na rachunku Azure, można uniknąć niespodzianki rozliczeń."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Ustawianie alertów rozliczeń dla subskrypcje usługi Microsoft Azure

Czy chodzi o ile masz wydatków każdego miesiąca dla subskrypcji Azure? Jeśli jesteś administratorem konta dla subskrypcji usługi Azure umożliwia usługi alertów rozliczenia Azure Tworzenie niestandardowych rozliczeń alertów, które ułatwiają monitorowanie i zarządzanie aktywnością rozliczeń dla kont usługi Azure.

Ta usługa jest usługą podglądu, najpierw musisz wykonać zależy od logowania dla niego. Odwiedź [stronę funkcji podglądu](https://account.windowsazure.com/PreviewFeatures) w portalu zarządzania konto Azure, aby włączyć tę funkcję.

## <a name="set-the-alert-threshold-and-email-recipients"></a>Ustawianie alertów adresatów próg i poczty e-mail

Po otrzymaniu potwierdzenia wiadomości e-mail, że usługa rozliczeń jest włączona dla subskrypcji można znaleźć [na stronie subskrypcje](https://account.windowsazure.com/Subscriptions) w portalu konta. Kliknij subskrypcję, którą chcesz monitorować, a następnie kliknij pozycję **alerty**.

![][Image1]

Następnie kliknij pozycję **Dodaj Alert** , aby utworzyć pierwsza — można ustawić liczbę pięciu rozliczeń alerty na subskrypcję, z różnych próg i do maksymalnie dwóch adresatów wiadomości e-mail dla każdego alertu.

![][Image2]

Po dodaniu alertu nadać mu unikatową nazwę, wybierz pozycję wydatków próg i wybierz miejsce, w którym będą wysyłane alerty adresy e-mail. Podczas konfigurowania progu, możesz wybrać **Rozliczenia całkowita** lub **Pieniężnych kredytowej** na liście **Alertów dla** . Dla wszystkich rozliczeń alert zostanie wysłany po wydatków subskrypcji przekracza próg. Dla kredyt pieniężnych alert zostanie wysłany po środków pieniężnych upuść poniżej limitu. Środki pieniężne dotyczą zazwyczaj bezpłatnej wersji próbnych i subskrypcje związane z kontami w witrynie MSDN.

![][Image3]

Azure obsługuje dowolnego adresu e-mail, ale nie zweryfikować, że adres e-mail działa, więc dokładnie literówek.

## <a name="check-on-your-alerts"></a>Sprawdzanie alertów

Po skonfigurowaniu alertów Centrum konta wyświetlane są one i sprawdzić, ile więcej można skonfigurować. Dla każdego alert Zobacz Data i godzina wysłania go, czy jest alertu dla rozliczenia całkowita lub pieniężnych faktury i możesz skonfigurować limit. Format daty i godziny koordynowanie Universal Time 24-godzinnego (UTC) a datą rrrr mm-dd format. Kliknij znak plus alertu na liście, aby ją edytować, lub kliknij przycisk Kosza może go usunąć.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
