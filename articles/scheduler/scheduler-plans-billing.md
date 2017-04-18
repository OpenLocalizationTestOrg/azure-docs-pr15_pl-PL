<properties
 pageTitle="Plany i rozliczenia w harmonogramie Azure"
 description="Plany i rozliczenia w harmonogramie Azure"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Plany i rozliczenia w harmonogramie Azure

## <a name="job-collection-plans"></a>Plany gromadzenia zadania

Kolekcje zadania są fakturowanego jednostki w harmonogramie Azure. Kolekcje zadania zawiera liczbę zadań i okazać się trzy plany — wolny, Standard i Premium — opisane poniżej.

|**Planowanie zbioru zadania**|**Maksymalna liczba zadań na zbiór zadania**|**Cykl Max**|**Kolekcje Max zadania na subskrypcję**|**Ograniczenia**|
|:---|:---|:---|:---|:---|
|**Bezpłatne**|5 zadania na zbiór zadania|Raz na godzinę. Nie można uruchomić zadania częściej niż raz na godzinę|Subskrypcji jest dozwolona do 1 zbioru bezpłatne zadania|Nie można użyć [obiektu ruchu wychodzącego autoryzacji HTTP](scheduler-outbound-authentication.md)
|**Standardowe**|50 zadania na zbiór zadania|Raz na minutę. Nie można uruchomić zadania częściej niż raz na minutę|Subskrypcji jest dozwolona do 100 zbiory standardowe zadania|Dostęp do pełny zestaw funkcji harmonogramu|
|**P 10 Premium**|50 zadania na zbiór zadania|Raz na minutę. Nie można uruchomić zadania częściej niż raz na minutę|Subskrypcji jest dozwolone do 10 000 zbiorów zadania Premium p 10. Aby uzyskać więcej informacji, <a href="mailto:wapteams@microsoft.com">skontaktuj się z nami</a> .|Dostęp do pełny zestaw funkcji harmonogramu|
|**P20 Premium**|zadania 1000 na zbiór zadania|Raz na minutę. Nie można uruchomić zadania częściej niż raz na minutę|Subskrypcji jest dozwolone do 10 000 zbiorów zadania P20 Premium. Aby uzyskać więcej informacji, <a href="mailto:wapteams@microsoft.com">skontaktuj się z nami</a> .|Dostęp do pełny zestaw funkcji harmonogramu|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Aktualizacje i zmiany na starszą wersję planów zbioru zadania

Możesz uaktualnić lub starszą wersję zadania zbioru planu w dowolnym momencie między planami wolny, Standard i Premium. Jednak obniżenie do kolekcji bezpłatne zadania, na starszą wersję może zakończyć się niepowodzeniem z jednej z następujących przyczyn:

- Kolekcja bezpłatne zadania już istnieje w subskrypcji
- Zadanie w zbiorze zadanie ma cyklu wyższą niż dozwolone dla zadań w zbiorach bezpłatne zadania. Maksymalna cyklu dozwolone w kolekcji bezpłatne zadania jest raz na godzinę
- Ma więcej niż 5 zadań w zbiorze zadania
- Zadanie w zbiorze zadanie ma akcję HTTP lub HTTPS, która korzysta z [protokołu HTTP ruchu wychodzącego autoryzacji obiektu](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Plany rozliczeń i Azure

Subskrypcji nie pobiera bezpłatnie zbiory zadania. Jeśli masz więcej niż 100 zbiory standardowe zadania (10 standardowe jednostki rozliczeń), jest lepsza transakcji mają wszystkie zbiory zadania w planie premium.

Jeśli masz jeden zbiór standardowe zadania i jednej kolekcji zadania premium, to rozliczonym jeden standardowy rozliczeń jednostki _i_ premium rozliczeń jednostki. Rozliczenia usługi Harmonogram na podstawie liczby zbiorów aktywnego zadania, które są ustawione na standardowy lub premii; to jest dokładniej w następnych dwóch sekcjach.

## <a name="standard-billable-units"></a>Standardowe jednostki fakturowanego

Standardowa jednostka fakturowanego może zawierać maksymalnie 10 zbiory standardowe zadania. Ponieważ zbioru standardowe zadania może mieć maksymalnie 50 zadania na zbiór zadania, standardowe jednostki rozliczeń umożliwia subskrypcji mieć maksymalnie 500 zadania — do wykonania zadania prawie 22 milionów na miesiąc.

Jeśli masz między 1 a 10 standardowe zadania, możesz zostanie wystawiona 1 standardowe jednostki rozliczeń. Jeśli masz między 11 i 20 zbiorów standardowe zadania, możesz zostanie wystawiona dla 2 standardowych jednostek rozliczeń. Jeśli masz między 21 i zbiory 30 standardowe zadania, zostanie wystawiona dla 3 standardowych jednostek rozliczeń i tak dalej.

## <a name="p10-premium-billable-units"></a>P 10 Jednostki fakturowanego Premium

Jednostka fakturowanego premium p 10 może zawierać do 10 000 zbiorów zadania premium p 10. Ponieważ zbioru p 10 premium zadania może mieć maksymalnie 50 zadania na zbiór zadania, jednostki premium rozliczeń umożliwia subskrypcji mieć maksymalnie 500 000 zadania — do wykonania zadania niemal 22 miliardów na miesiąc.

Jeśli masz między 1 do 10 000 zbiorów zadania premium, możesz zostanie wystawiona 1 jednostki rozliczeń premium p 10. Jeśli masz między 10,001 i premium 20 000 zbiorów zadania zostanie wystawiona 2 jednostek rozliczeń premium p 10 i tak dalej.

W związku z tym p 10 premium zadania zbiory mieć te same funkcje co zbiory standardowe zadania, ale pozwala rozbicie ceny w przypadku, gdy aplikacja wymaga wielu zbiorów zadania.

## <a name="p20-premium-billable-units"></a>P20 Jednostki fakturowanego Premium

Jednostka fakturowanego premium P20 może zawierać maksymalnie 5000 P20 premium zadania zbiorów. Ponieważ zbioru P20 premium zadania może mieć maksymalnie 1000 zadań na zbiór zadania, jednostki premium rozliczeń umożliwia subskrypcji mieć maksymalnie 5,000,000 zadania — do wykonania zadania niemal 220 miliardów na miesiąc.

P20 premium zadania zbiory zawiera takie same możliwości jako kolekcje zadania premium p 10, ale obsługuje również większe zadania liczb na zbiór zadania i większą liczbą całkowitą zadań ogólnego niż premium p 10, co umożliwia skalowalność więcej.

## <a name="billing-and-active-status"></a>Stan rozliczeń i aktywne

Kolekcje zadania są zawsze aktywne, chyba że subskrypcji całego wejścia w niektórych tymczasowe stanu wyłączenia z powodu problemów rozliczenia. Jedynym sposobem, aby upewnić się, że nie jest wystawiona zbioru zadania jest dowolnego ustawić go do planu _wolny_ lub usunąć zbiór zadania.

Mimo że można wyłączyć wszystkie zadania w obrębie zbioru zadania w jednej operacji, rozliczeń stanu zbioru zadania nie powoduje zmiany — zbioru zadania będą _nadal_ naliczane. Podobnie kolekcje pustego zadania są traktowane jako aktywne i będą naliczane.

## <a name="pricing"></a>Cennik

Ceny szczegóły, zobacz [Ceny harmonogram](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Zobacz też


 [Co to jest harmonogram?](scheduler-intro.md)

 [Azure pojęcia harmonogram, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Odwołanie interfejsu API usługi REST harmonogram Azure](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca Azure poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Azure Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Azure limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)

 [Azure uwierzytelniania ruchu wychodzącego harmonogramu](scheduler-outbound-authentication.md)
