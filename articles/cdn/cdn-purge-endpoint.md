<properties
    pageTitle="Trwałe usuwanie punktu końcowego Azure CDN | Microsoft Azure"
    description="Dowiedz się, jak wyczyścić całą zawartość pamięci podręcznej z punkt końcowy CDN."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Trwałe usuwanie punktu końcowego Azure CDN

## <a name="overview"></a>Omówienie

Azure węzły krawędzi CDN będzie w pamięci podręcznej składników majątku do momentu wygaśnięcia zasobu time to live (TTL).  Po wygaśnięciu TTL środków trwałych, gdy klient żąda elementu z węzła krawędzi, węzeł krawędzi pobiera nową kopię zaktualizowanej zawartości do obsługi żądania klienta i Magazyn odświeżania pamięci podręcznej.

Czasami możesz czyszczenie pamięci podręcznej zawartości ze wszystkich węzłów krawędzi i wymusić je wszystkie do pobierania nowych trwałych zaktualizowane.  Może to być spowodowane aktualizacje do aplikacji sieci web lub do szybkiego aktywów aktualizacji, które zawiera niepoprawne informacje.

> [AZURE.TIP] Należy zauważyć, że czyszczenie tylko usuwa zawartość pamięci podręcznej na serwerach krawędzi CDN.  Dowolnego niższego pamięci podręcznej, takich jak serwery proxy i pamięci podręcznej przeglądarki lokalnej, może być nadal przytrzymaj pamięci podręcznej kopię pliku.  Należy pamiętać o tym, gdy ustawiony plik time to live.  Możesz wymusić niższego klienta do żądania najnowszej wersji pliku, nadając mu unikatową nazwę za każdym razem, możesz zaktualizować lub korzystając z [pamięci podręcznej ciągu kwerendy](cdn-query-string.md).  

Ten samouczek przeprowadzi Cię przez trwałe usuwanie elementów ze wszystkich węzłów krawędź punktu końcowego.

## <a name="walkthrough"></a>Instruktaż

1. W [Azure Portal](https://portal.azure.com)przejdź do profilu CDN zawierającej punkt końcowy, który chcesz usunąć.

2. Z sieci CDN karta profilu kliknij przycisk Wyczyść.

    ![Karta profil sieci CDN](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Zostanie wyświetlona karta Wyczyść.

    ![Karta Usuwanie CDN](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Na karta wyczyść wybierz adres usługi, którą chcesz usunąć z listy rozwijanej adresu URL.

    ![Czyszczenie formularza](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Karta Usuwanie można uzyskać również, klikając przycisk **Wyczyść** na CDN karta punktu końcowego.  W takim przypadku pole **adres URL** jest wstępnie wypełnione przy użyciu adresu tego określonego punktu końcowego usługi.

4. Wybierz pozycję jakie zasoby, które chcesz usunąć z węzłów krawędzi.  Jeśli chcesz wyczyścić wszystkie zasoby, kliknij przycisk **Wyczyść wszystkie** pola wyboru.  W przeciwnym razie wpisz pełną ścieżkę każdego środka chcesz wyczyścić (np `/pictures/kitten.png`) w polu tekstowym **ścieżka** .

    > [AZURE.TIP] Więcej pól tekstowych **ścieżka** pojawi się po wprowadzeniu tekstu pozwala utworzyć listę wielu zasobów.  Możesz usunąć elementy zawartości z listy, klikając przycisk wielokropka (...).
    >
    > Ścieżki muszą być względny adres URL, która mieści się następujące [wyrażenie](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  Dla **CDN Azure z Verizon** (Standard i Premium), gwiazdka (\*) może służyć jako symbol wieloznaczny (np `/music/*`).  Symbole wieloznaczne i **Czyszczenie wszystkich** nie są dozwolone z **CDN Azure z Akamai**.
    
5. Kliknij przycisk **Wyczyść** .

    ![Przycisk Wyczyść](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Usuwanie żądania potrwać około 2 – 3 minuty przetwarzania z **CDN Azure z Verizon** (Standard i Premium) i około 7 minut z **CDN Azure z Akamai**.  Azure CDN ma limit 50 równoczesne wyczyść żądania w dowolnym momencie. 

## <a name="see-also"></a>Zobacz też
- [Wstępnie ładowania zasobów dla punktu końcowego Azure CDN](cdn-preload-endpoint.md)
- [Azure odwołanie interfejsu API usługi REST CDN — czyszczenie lub wstępnie załadować punktu końcowego](https://msdn.microsoft.com/library/mt634451.aspx)
