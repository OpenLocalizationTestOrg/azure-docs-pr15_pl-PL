<properties
    pageTitle="Partii przydziały usługi i ograniczenia | Microsoft Azure"
    description="Dowiedz się więcej o domyślne partii Azure przydziały, limity i ograniczeń, i zwiększanie jak zażądać przydziału"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Przydziały i limity dotyczące usługi Azure partii

Zgodnie z innymi usługami Azure, istnieją ograniczenia określonych zasobów skojarzonego z usługą partię. Wiele z tych ograniczeń są stosowane przez Azure poziomie subskrypcji lub konto domyślne przydziały. W tym artykule omówiono te ustawienia domyślne, i jak można zażądać przydziału zwiększa.

Jeśli zamierzasz uruchomienie produkcji obciążenia w partii może być konieczne zwiększenie jednej lub większej liczby przydziałów powyżej domyślny. Jeśli chcesz podnieść normę, możesz otworzyć online [żądania obsługi klienta](#increase-a-quota) bezpłatnie.

>[AZURE.NOTE] Przydział jest limit kredytu, nie gwarancję wydajność. Jeśli potrzebują dużej pojemności skontaktuj się z obsługą Azure.

## <a name="subscription-quotas"></a>Subskrypcja przydziałów
**Zasób**|**Domyślny Limit**|**Limit maksymalny**
---|---|---
Konta partii w rozbiciu na regiony na subskrypcję | 1 | 50

## <a name="batch-account-quotas"></a>Partia konta przydziałów
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Inne limity
**Zasób**|**Limit maksymalny**
---|---
[Równoczesne zadania](batch-parallel-node-tasks.md) na węzeł obliczeń | 4 x liczby rdzeni węzeł
[Aplikacje](batch-application-packages.md) dla każdego konta partii        | 20
Pakiety aplikacji dla aplikacji  | 40
Rozmiar pakietu aplikacji (wszystkie)       | Około 195GB<sup>1</sup>

Limit magazynowania azure <sup>1</sup> dla maksymalny obiektów blob rozmiar bloku

## <a name="view-batch-quotas"></a>Wyświetlanie przydziałów partii

Wyświetlanie partii przydziałami konta w [Azure portal][portal].

1. Wybierz pozycję **konta partii** w portalu, a następnie wybierz konto partii, który Cię interesuje.

2. Wybierz pozycję **Właściwości** na rachunku partii karta menu

3. Karta właściwości wyświetla **przydziałów** zastosowanym do konta partii

    ![Partia konta przydziałów][account_quotas]

## <a name="increase-a-quota"></a>Zwiększanie przydziału

Wykonaj poniższe czynności, aby zażądać normę zwiększyć za pomocą [Azure portal][portal].

1. Wybierz Kafelek **pomocy + pomocy technicznej** na pulpicie nawigacyjnym portalu lub znak zapytania (****?) w prawym górnym rogu portalu.

2. Wybierz pozycję **Nowy obsługuje żądania** > **podstawy**.

3. Na karta **— informacje podstawowe** :

    . **Typ problemu** > **przydziału**

    b. Wybierz subskrypcję.

    c. **Typ przydziału** > **partii**

    d. **Planowanie obsługi** > **Obsługa przydziałów — dostępny**

    Kliknij przycisk **Dalej**.

4. Na karta **problemu** :

    . Wybieranie **ważności** zgodnie z [biznesowych][support_sev].

    b. **Szczegóły**Określ każdego przydziału, który chcesz zmienić, nazwę konta partii i nowy limit.

    Kliknij przycisk **Dalej**.

5. Na karta **informacje kontaktowe** :

    . Wybierz **Preferowany metoda kontaktu**.

    b. Sprawdź i wprowadź wymagane dane kontaktowe.

    Kliknij przycisk **Utwórz** , aby wysłać żądanie pomocy technicznej.

Po przesłaniu żądania obsługi Azure obsługi będzie skontaktować się z Tobą. Zauważ, że wykonywania żądania może potrwać do 2 dni robocze.

## <a name="related-topics"></a>Tematy pokrewne

* [Tworzenie konta partii Azure za pomocą portalu Azure](batch-account-create-portal.md)

* [Omówienie funkcji partii Azure](batch-api-basics.md)

* [Azure subskrypcji i limity dotyczące usługi, przydziałów i ograniczeń](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
