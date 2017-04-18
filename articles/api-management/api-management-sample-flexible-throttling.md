<properties
    pageTitle="Zaawansowane żądanie ograniczania za pomocą usługi Zarządzanie interfejsu API platformy Azure"
    description="Dowiedz się, jak tworzyć i stosować elastyczne przydziałów i tempa ograniczania zasady zarządzania interfejsu API Azure."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Zaawansowane żądanie ograniczania za pomocą usługi Zarządzanie interfejsu API platformy Azure

Możliwość ograniczania przychodzących żądań jest kluczową rolę Azure interfejsu API zarządzania. Albo przez kontrolowanie stawkę żądania lub Suma żądania i przekazania danych, zarządzania interfejsu API umożliwia dostawców interfejsu API w celu chronienia ich interfejsów API z abuse i utworzyć wartości dla różnych poziomów produktu interfejsu API.

## <a name="product-based-throttling"></a>Ograniczanie produktu podstawie
Data końcowa ograniczania stawek funkcje zostały ograniczone do występujące w danej subskrypcji produktu (zasadniczo klucz), zdefiniowane w portalu zarządzania interfejsu API programu publisher. To jest przydatne w przypadku interfejsu API dostawcy stosowanie limity na deweloperów, którzy zalogowali się za pomocą ich interfejsu API, jednak go nie pomoże, na przykład w ograniczania indywidualnych użytkowników końcowych interfejsu API. Jest to możliwe, że dla pojedynczego użytkownika dewelopera aplikacji, aby wykorzystać całego przydziału, a następnie uniemożliwić innych klientów projektanta korzystania z aplikacji. Ponadto kilku klientów, którzy mogą wygenerować dużej liczby żądań mogą ograniczyć dostęp do rzadkie użytkowników.

## <a name="custom-key-based-throttling"></a>Niestandardowe klucza ograniczania zależności
Nowe zasady [stopa limit przez klucz](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [przydziału według klucza](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zapewniają znacznie bardziej elastyczne rozwiązanie do kontroli ruchu. Tych zasad umożliwia definiowanie wyrażeń w celu identyfikowania klawiszy, używane do śledzenia zastosowania ruch. Easiest przedstawiono sposób to sprawdza się z przykładem. 

## <a name="ip-address-throttling"></a>Ograniczanie adres IP
Następujących zasad ograniczyć adres IP jednego klienta do połączenia tylko 10, co minutę z łącznie 1 000 000 połączeń i 10 000 kilobajtów przepustowości na miesiąc. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

W przypadku wszystkich klientów w Internecie użycia unikatowy adres IP, może to być skuteczny sposób ograniczenia użycia przez użytkownika. Jednak jest bardzo prawdopodobieństwo, że wielu użytkowników będzie udostępniania jeden publiczny adres IP ze względu na ich dostęp do Internetu przez urządzenie NAT. Mimo to dla nieuwierzytelnionych dostęp do interfejsów API `IpAddress` może być najlepszym rozwiązaniem.

## <a name="user-identity-throttling"></a>Ograniczanie tożsamość użytkownika
Jeśli jest uwierzytelniony użytkownik końcowy, a następnie ograniczania klucz może zostać wygenerowany na podstawie informacji jednoznacznie określa, że użytkownik.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

W tym przykładzie firma Microsoft wyodrębnić nagłówek uwierzytelnienia, konwertować, aby `JWT` obiekt i użyj temat tokenu, aby zidentyfikować użytkownika i użyć jej jako tempa ograniczania klucza. Jeśli tożsamość użytkownika są przechowywane w `JWT` zgodnie z jedną z tych roszczeń następnie że wartości można używać w tym miejscu.

## <a name="combined-policies"></a>Zasady Scalonej
Chociaż nowe zasady ograniczania zapewniają większą kontrolę, niż istniejące zasady ograniczania, jest nadal wartość łączenie obie możliwości. Ograniczenie przez klucza subskrypcji produktu ([Ogranicz szybkość połączenia przez subskrypcji](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [ustawić przydział użycia przez subskrypcji](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) jest to doskonały sposób na włączanie monetizing interfejsu API przez ładowania według poziomów użycia. Bardziej precyzyjne formant lub kolorów można ograniczyć przez użytkownika uzupełnia i zapobiega obniżeniu środowisko innego zachowanie jednego użytkownika. 

## <a name="client-driven-throttling"></a>Klient zmiennych ograniczania
Klucz ograniczania zdefiniowane przy użyciu [wyrażenie zasad](https://msdn.microsoft.com/library/azure/dn910913.aspx), to dostawcy interfejs API, który jest wybór sposobu występujące ograniczania. Deweloper może być jednak chcesz zachować kontrolę nad jak ich ocenić limit własne klientów. To może zostać włączona przez dostawcę interfejsu API wprowadzając niestandardowy nagłówek umożliwia dewelopera aplikacji klienckiej na komunikowanie się klucz API.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Dzięki temu aplikacji klienckiej dewelopera określić, jak mają być tworzenie tempa ograniczania klucza. Przy odrobinie ingenuity Deweloper klienta można utworzyć swoje własne warstwy stawki przydzielając zestawów kluczy do użytkowników i obracanie użycia klucza.

## <a name="summary"></a>Podsumowanie
Azure zarządzania interfejsu API udostępnia stopa i oferty ograniczania zarówno ochrony i dodawać wartości do interfejsu API usługi. Nowe zasady ograniczania z regułami zakresu niestandardowe pozwalają bardziej precyzyjne lub kolorów kontrolę nad tych zasad, aby umożliwić klientom tworzenie jeszcze lepiej aplikacji. W przykładach w tym artykule przedstawiono stosowania tych zasad przez tempa produkcji ograniczania kluczy z adresów IP klienta, tożsamość użytkownika i klienta wygenerowane wartości. Istnieją jednak wielu części wiadomości, które mogą zostać użyte, takie jak agenta użytkownika, fragmenty ścieżkę adresu URL, rozmiar wiadomości.

## <a name="next-steps"></a>Następne kroki
Podaj swoją opinię w wątku Disqus dla tego tematu. Jest doskonałe otrzymywać informacje o innych potencjalnych wartości klucza, które zostały logiczne wybór w swojej scenariuszach.

## <a name="watch-a-video-overview-of-these-policies"></a>Obejrzyj klip wideo pokazujący tych zasad
Aby uzyskać więcej informacji na zasady [Stawka limit przez klucz](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [przydziału według klucza](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) , omówione w tym artykule, należy w poniższym klipie wideo.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
