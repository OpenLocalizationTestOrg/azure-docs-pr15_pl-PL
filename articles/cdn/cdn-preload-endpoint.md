<properties
    pageTitle="Wstępnie ładowania zasobów dla punktu końcowego Azure CDN | Microsoft Azure"
    description="Dowiedz się, jak wstępnie ładowanie zawartości pamięci podręcznej na punkt końcowy CDN."
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

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Wstępnie ładowania zasobów dla punktu końcowego Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Domyślnie elementy zawartości najpierw pamięci podręcznej jako żądanie. Oznacza to, że pierwszy wniosek każdego regionu może potrwać dłużej, ponieważ serwery graniczne nie będą mieć zawartość przechowywanych w pamięci podręcznej i będzie konieczne przesyłanie dalej wezwania z serwerem źródłowym. Ładowanie wstępne zawartości pozwala uniknąć to pierwszy opóźnienie trafień.

Oprócz dostarczania lepiej klienta, ładowanie wstępne pamięci podręcznej aktywów można również zmniejszyć ruch sieciowy z serwera źródłowego.

> [AZURE.NOTE] Ładowanie wstępne zasoby są przydatne w przypadku dużych wydarzeń lub zawartości, która staje się jednocześnie dostępna dla dużej liczby użytkowników, takie jak nowa wersja filmu lub aktualizacji oprogramowania.

Ten samouczek przeprowadzi Cię przez ładowanie wstępne zawartości buforowanej we wszystkich węzłach krawędzi Azure CDN.

## <a name="walkthrough"></a>Instruktaż

1. W [Azure Portal](https://portal.azure.com)przejdź do profilu CDN zawierającej punkt końcowy, który chcesz załadować wstępnie.  Zostanie wyświetlona karta profilu.

2. Kliknij punkt końcowy na liście.  Zostanie wyświetlona karta punktu końcowego.

3. Z sieci CDN karta punktu końcowego kliknij przycisk Załaduj.

    ![Karta punktu końcowego sieci CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Zostanie wyświetlona karta ładowania.

    ![Karta obciążenie sieci CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Wpisz pełną ścieżkę każdego środka do załadowania (np `/pictures/kitten.png`) w polu tekstowym **ścieżka** .

    > [AZURE.TIP] Więcej pól tekstowych **ścieżka** pojawi się po wprowadzeniu tekstu pozwala utworzyć listę wielu zasobów.  Możesz usunąć elementy zawartości z listy, klikając przycisk wielokropka (...).
    >
    > Ścieżki muszą być względne adresu URL odpowiadającego następujące [wyrażenie](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Poszczególnych elementów zawartości musi mieć własną ścieżkę.  Istnieje bez obsługi funkcji symbol wieloznaczny wstępnie ładowania trwałych.

    ![Przycisk Załaduj](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Kliknij przycisk **Załaduj** .

    ![Przycisk Załaduj](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Istnieje ograniczenie liczby 10 obciążenie żądaniami minutę na CDN profil.

## <a name="see-also"></a>Zobacz też
- [Trwałe usuwanie punktu końcowego Azure CDN](cdn-purge-endpoint.md)
- [Azure odwołanie interfejsu API usługi REST CDN — czyszczenie lub wstępnie załadować punktu końcowego](https://msdn.microsoft.com/library/mt634451.aspx)
