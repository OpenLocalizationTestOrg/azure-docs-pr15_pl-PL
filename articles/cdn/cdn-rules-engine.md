<properties
    pageTitle="Zastępuje domyślne zachowanie HTTP w sieci CDN Azure za pomocą aparatu reguł | Microsoft Azure"
    description="Aparat reguł umożliwia dostosowywanie sposobu żądania HTTP są obsługiwane przez Azure CDN, takie jak blokowanie dostarczenie niektórych typów zawartości, opracowanie zasad przechowywania i modyfikować nagłówki HTTP."
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

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Zastępuje domyślne zachowanie HTTP za pomocą aparatu reguł

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Omówienie

Aparat reguł pozwala dostosować sposób obsługi żądania HTTP, takie jak blokowanie dostarczenie niektórych typów zawartości, definiowania zasad pamięci podręcznej i modyfikowanie nagłówków HTTP.  Ten samouczek przedstawi tworzenia reguły, która powoduje zmianę zachowania buforowania środków trwałych sieci CDN.  Istnieje także zawartość wideo dostępnych w sekcji "[Zobacz też](#see-also)".

## <a name="tutorial"></a>Samouczek

1. Z sieci CDN karta profilu kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-rules-engine/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Kliknij kartę **HTTP dużych** , a po nim **Aparat reguł**.

    Zostaną wyświetlone opcje dla nowej reguły.

    ![Nowe opcje reguły CDN](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] Kolejność, w której znajdują się wiele reguł ma wpływ na sposób obsługi. Kolejne reguły mogą zastąpić czynności określonych przez regułę poprzedniego.
    
3. Wprowadź nazwę w **nazwę i opis** pole tekstowe.

4. Określ typ żądania, które reguła ma dotyczyć.  Domyślnie warunek dopasowanie **zawsze** jest zaznaczone.  Będzie **zawsze** ten samouczek, należy więc pozostaw zaznaczony.

    ![Sieci CDN dopasowanie warunku](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Istnieje wiele typów dopasowania warunków dostępnych na liście rozwijanej.  Klikając niebieską ikonę informacyjne po lewej stronie warunek dopasowanie będzie wyjaśniono aktualnie zaznaczonego warunku szczegółowo.
    >
    >Aby uzyskać pełną listę warunków dopasowanie szczegóły zobacz [warunek zgodne aparat reguł i szczegóły funkcji](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Kliknij pozycję **+** przycisk obok **funkcji** , które można dodać nową funkcję.  Na liście rozwijanej po lewej stronie wybierz pozycję **Wieku Max wewnętrznych życie**.  W polu tekstowym wprowadź **300**.  Pozostaw pozostałe wartości domyślne.

    ![Funkcja sieci CDN](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Jako warunkom dopasowania, klikając ikonę informacyjny niebieski po lewej stronie nową funkcję wyświetli szczegółowe informacje o tej funkcji.  W przypadku **Wewnętrznych wieku Max życie**możemy są zastępowanie nagłówki **Kontroli pamięci podręcznej** i **wygasa** zasobu do kontrolki węzeł krawędzi CDN będzie odświeżania zawartości względem punktu początkowego.  Naszym przykładzie 300 sekund oznacza, że węzeł krawędzi CDN będzie pamięci podręcznej elementu 5 minut przed odświeżanie zawartości na podstawie pochodzenia.
    >
    >Aby uzyskać pełną listę funkcji szczegóły zobacz [warunek dopasowanie aparat reguł i szczegóły funkcji](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Kliknij przycisk **Dodaj** , aby zapisać nową regułę.  Nowa reguła jest teraz oczekujące na zatwierdzenie. Po zostały zatwierdzone, stan zmieni się z **Oczekiwanie XML** do **Aktywnego pliku XML**.

    >[AZURE.IMPORTANT] Reguły zmiany może potrwać do 90 propagowanie za pośrednictwem sieci CDN.

## <a name="see-also"></a>Zobacz też
* [Piątki azure: wydajne nowe funkcje Premium Azure CDN](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (wideo)
* [Warunek dopasowanie aparat reguł i szczegóły funkcji](https://msdn.microsoft.com/library/mt757336.aspx)
