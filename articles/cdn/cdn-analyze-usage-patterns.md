<properties
    pageTitle="Analizowanie Azure CDN upodobania | Microsoft Azure"
    description="Można wyświetlać upodobania sieci CDN za pomocą następujące raporty: przepustowości, dane przesyłane, trafień, stany pamięci podręcznej, Współczynnik trafień pamięci podręcznej, przekazane dane IPV4 i IPV6."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-azure-cdn-usage-patterns"></a>Analizowanie upodobania Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Upodobania można wyświetlić dla sieci CDN za pomocą następujące raporty:

- Przepustowości
- Dane przesyłane
- Trafień
- Statusy pamięci podręcznej
- Współczynnik trafień pamięci podręcznej
- Przenoszone danych IPv4 i IPV6

## <a name="accessing-advanced-http-reports"></a>Uzyskiwanie dostępu do zaawansowanych raportów HTTP

1. Z sieci CDN karta profilu kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-reports/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Umieść wskaźnik myszy na karcie **analizy** , a następnie umieść wskaźnik myszy na menu wysuwane **Raporty podstawowe** .  Kliknij żądany raport w menu.

    ![Portal zarządzania CDN — menu Raporty podstawowe](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Przepustowości

Raport przepustowości składa się z wykresu i danych tabeli wskazująca wykorzystania przepustowości dla protokołu HTTP i HTTPS w określonym przedziale czasu. Możesz wyświetlić wykorzystania przepustowości przez wszystkie POP CDN lub określonego POP. Pozwala na wyświetlanie tych najwyższych wartościach ruchu i dystrybucji w sieci CDN pojawia się w MB/s.

- Zaznacz wszystkie węzły krawędzi Zobacz ruch ze wszystkich węzłów lub wybierz określonego regionu i węzła z listy rozwijanej.
- Wybierz zakres dat wyświetlanie danych dzisiaj ten tydzień/ten miesiąc, itd. lub wprowadź niestandardowe daty, a następnie kliknij przycisk "Przejdź", aby upewnić się, że zaznaczenie jest aktualizowana.
- Można wyeksportować i Pobierz dane, klikając ikonę arkusza programu excel, znajdujący się obok pozycji "Przejdź".

Raport jest aktualizowana co 5 minut.

![Raport przepustowości](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Dane przesyłane

Ten raport składa się z wykresu i danych tabeli wskazująca zastosowania ruch dla protokołu HTTP i HTTPS w określonym przedziale czasu. Możesz wyświetlić zastosowania ruch przez wszystkie POP CDN lub określonego POP. Pozwala na wyświetlanie tych najwyższych wartościach ruchu i dystrybucji w sieci CDN pojawia się w GB.

- Zaznacz wszystkie węzły krawędzi Zobacz ruchu z wszystkie notatki lub wybierz określonego regionu i węzła z listy rozwijanej.
- Wybierz zakres dat, aby wyświetlać dane dzisiaj ten tydzień/ten miesiąc, itd. lub wprowadź daty niestandardowe, a następnie kliknij "Przejdź", aby upewnić się, że zaznaczenie jest aktualizowana.
- Można wyeksportować i Pobierz dane, klikając ikonę arkusza programu excel, znajdujący się obok pozycji "Przejdź".

Raport jest aktualizowana co 5 minut.

![Dane przesyłane raportu](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Liczba (stan kody)

Ten raport zawiera opis rozkład kody stanu żądania dla zawartości. Każdego żądania dla zawartości wygeneruje kodem stanu HTTP. Kod stanu w tym artykule opisano obsługi krawędzi POP na żądanie. Na przykład kody stanu 2xx wskazują, że żądania został pomyślnie obsługiwane do klienta, podczas kodu stanu 4xx wskazuje, że wystąpił błąd. Aby uzyskać więcej informacji na temat kodu stanu HTTP zobacz [kody stanu](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Wybierz zakres dat wyświetlanie danych dzisiaj ten tydzień/ten miesiąc, itd. lub wprowadź niestandardowe daty, a następnie kliknij przycisk "Przejdź", aby upewnić się, że zaznaczenie jest aktualizowana.
- Można wyeksportować i Pobierz dane, klikając znajdujący się obok pozycji "Przejdź" arkusz programu excel.

![Raport trafień](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Statusy pamięci podręcznej

Ten raport zawiera opis rozkład trafień w pamięci podręcznej i Chybienia w pamięci podręcznej dla żądania klienta. Ponieważ największą wydajność pochodzi z trafień w pamięci podręcznej, możesz optymalizować szybkości dostarczania danych, minimalizując Chybienia w pamięci podręcznej i trafień w pamięci podręcznej wygasły. Chybienia w pamięci podręcznej można zmniejszyć przez skonfigurowanie serwera origin unikać przypisywanie nagłówków odpowiedzi "nie pamięci podręcznej", można uniknąć ciągu kwerendy pamięci podręcznej z wyjątkiem przypadku, gdy niezbędnych i przez uniknięcie kody-buforowalne odpowiedzi. Wygasłe pamięci podręcznej, którą można uniknąć trafień dokonując trwały przez max wieku tak długo, jak to możliwe, aby zminimalizować liczbę żądań z serwerem źródłowym.

![Raport statusy pamięci podręcznej](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Statusy głównej pamięci podręcznej obejmują:

- TCP_HIT: Dostarczono od krawędzi. Obiekt w pamięci podręcznej i nie przekroczył jego wieku max.
- TCP_MISS: Dostarczono pochodzenia. Obiekt nie znajdował się w pamięci podręcznej i odpowiedź była powrót do pochodzenia.
- TCP_EXPIRED _MISS: obsługiwane pochodzenia po ponownego sprawdzania poprawności wraz ze źródłem. Obiekt został w pamięci podręcznej, ale przekroczył jego wiek max. W wyniku ponownego sprawdzania poprawności wraz ze źródłem obiektu pamięci podręcznej zastąpiona nową odpowiedź na początku.
- TCP_EXPIRED _HIT: obsługiwane od krawędzi po ponownego sprawdzania poprawności wraz ze źródłem. Obiekt został w pamięci podręcznej, ale przekroczył jego wieku max. W wyniku ponownego sprawdzania poprawności na serwerze origin obiektu pamięci podręcznej jest za w niezmienionej postaci.

- Wybierz zakres dat wyświetlanie danych dzisiaj ten tydzień/ten miesiąc, itd. lub wprowadź niestandardowe daty, a następnie kliknij przycisk "Przejdź", aby upewnić się, że zaznaczenie jest aktualizowana.
- Można wyeksportować i Pobierz dane, klikając ikonę arkusza programu excel, znajdujący się obok pozycji "Przejdź".

### <a name="full-list-of-cache-statuses"></a>Pełna lista stanów pamięci podręcznej

- TCP_HIT — ten status jest podać, jeśli żądanie jest obsługiwana bezpośrednio z POP do klienta. Od razu udostępnieniu środka trwałego z POP, gdy jest buforowana na POP najbliżej klienta i został prawidłowy time to live lub pola TTL. TTL jest określona przez następujące nagłówków odpowiedzi:

    - Kontroli pamięci podręcznej: s-maxage
    - Kontroli pamięci podręcznej: Maksymalna liczba wieku
    - Wygasa

- TCP_MISS — ten status wskazuje najbliżej klienta POP nie został znaleziony wersja buforowana środka wymagane. Aktywów będzie wymagane z serwera pochodzenia lub serwera tarczą pochodzenia. Jeśli serwer pochodzenia lub serwer tarczą origin zwraca środka trwałego, będą przesyłane do komputera klienckiego i przechowywanych w pamięci podręcznej, zarówno na klienta i serwera granicznego. W przeciwnym razie kodu stanu innych niż 200 (np 403 Dostęp zabroniony, 404 — Nie znaleziono, itp.) zostaną zwrócone.

- _HIT TCP_EXPIRED — ten status jest zgłoszone, gdy żądanie, którego środka trwałego wygasłe TTL, na przykład po wygaśnięciu wieku max zasobu został obsługiwane bezpośrednio z POP do klienta.

    Wygasłe żądanie zwykle powoduje żądanie ponownego sprawdzania poprawności z serwerem źródłowym. Aby _HIT TCP_EXPIRED ma być wykonywana serwer pochodzenia muszą wskazywać nowszej wersji elementu nie istnieje. Taka sytuacja zazwyczaj zaktualizuje kontroli pamięci podręcznej i nagłówki wygasa tej zawartości.

- _MISS TCP_EXPIRED — ten status jest zgłaszany po nowsza wersja wygasłej zawartości pamięci podręcznej jest udostępniany z POP do klienta. Dzieje, gdy wygasła TTL buforowanych trwałego (np wygasł wieku max) i serwer pochodzenia zwraca nowsza wersja tego zasobu. Nowa wersja środka będzie udostępniania do klienta zamiast wersji pamięci podręcznej. Ponadto będą buforowane na serwerze granicznym i klienta.

- CONFIG_NOCACHE — ten stan wskazuje, że konfiguracji specyficzna dla klienta na nasz krawędzi POP można zapisać elementu z pamięci podręcznej.

- Brak — ten status wskazuje, że wyboru aktualności zawartości pamięci podręcznej nie została wykonana.

- _MISS TCP_ CLIENT_REFRESH — ten status jest zgłaszany po klienta HTTP (na przykład przeglądarka) wymusza krawędź POP do pobrania nowa wersja starych elementów zawartości z serwera pochodzenia.

    Domyślnie naszych serwerów zapobiega HTTP klienta wymuszania naszych serwerów krawędzi, aby pobrać nową wersję elementu z serwera pochodzenia.

- PARTIAL_HIT TCP_ — ten status jest zgłoszone, gdy żądania zakresu bajt powoduje trafienie trwałego częściowo pamięci podręcznej. Żądany zakres bajtów natychmiast jest udostępniany z POP do klienta.

- UNCACHEABLE — ten status jest zgłoszone, gdy nagłówki kontroli pamięci podręcznej i wygasa środka trwałego wskazują, że go nie buforowania przez klienta HTTP lub POP. Poniższe typy żądań są obsługiwane z serwera pochodzenia

## <a name="cache-hit-ratio"></a>Współczynnik trafień pamięci podręcznej

Raport zawiera wartość procentową żądań pamięci podręcznej, które zostało obsłużonych bezpośrednio z pamięci podręcznej.

Raport zawiera następujące informacje:

- Wymagane zawartość została buforowana na najbliżej żądającego POP.
- Żądanie zostało dostarczono bezpośrednio od krawędzi naszej sieci.
- Żądanie nie wymaga ponownego sprawdzania poprawności w serwerze źródłowym.

Raport nie zawiera:

- Żądania niedostępne z powodu kraju opcje filtrowania.
- Żądania dla elementów, których nagłówki wskazują, że nie powinny one buforowana. Na przykład kontroli pamięci podręcznej: prywatne, kontroli pamięci podręcznej: nie pamięci podręcznej lub pragmy: nagłówki pamięci podręcznej nie uniemożliwia środka trwałego z pamięci podręcznej.
- Żądania zakresu bajtów dla zawartości częściowo pamięci podręcznej.

Formuła jest następująca: (PRZEWAGA TCP_-(PRZEWAGA TCP_ + TCP_MISS)) * 100

- Wybierz zakres dat wyświetlanie danych dzisiaj ten tydzień/ten miesiąc, itd. lub wprowadź niestandardowe daty, a następnie kliknij przycisk "Przejdź", aby upewnić się, że zaznaczenie jest aktualizowana.
- Można wyeksportować i Pobierz dane, klikając ikonę arkusza programu excel, znajdujący się obok pozycji "Przejdź".


![Raport Stosunek trafień w pamięci podręcznej](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Przekazane dane IPv4 i IPV6

Ten raport zawiera dystrybucji zastosowania ruchu w porównaniu IPV4 protokołu IPV6.

![Przekazane dane IPv4 i IPV6](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Wybierz zakres dat, aby wyświetlać dane dzisiaj ten tydzień/ten miesiąc, itd., lub wprowadź daty niestandardowej.
- Następnie kliknij przycisk "Przejdź", aby upewnić się, że zaznaczenie jest aktualizowana.


## <a name="considerations"></a>Zagadnienia dotyczące

Raporty można tylko wygenerować w ciągu ostatnich 18 miesięcy.
