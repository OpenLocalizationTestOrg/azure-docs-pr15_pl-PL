<properties
    pageTitle="Sterowanie Premium CDN Azure z Verizon pamięci podręcznej, zachowanie żądania z ciągi kwerend | Microsoft Azure"
    description="Azure ciągu kwerendy sieci CDN pamięci podręcznej kontrolek sposobu uzyskania pliki buforowane, które zawierają ciągi kwerend."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Sterowanie buforowania zachowanie żądania CDN za pomocą kwerendy ciągów - Premium

> [AZURE.SELECTOR]
- [Standardowe](cdn-query-string.md)
- [Premium Azure CDN z Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Omówienie

Buforowanie kontrolek sposobu uzyskania pliki buforowane, które zawierają ciągi kwerend ciągu kwerendy.

> [AZURE.IMPORTANT] Produkty Standard i Premium CDN zapewniają ten sam ciąg kwerendy pamięci podręcznej funkcji, ale różni się w interfejsie użytkownika.  W tym dokumencie opisano interfejs usługi **Azure CDN Premium z Verizon**.  W pamięci podręcznej ciągu kwerendy z **Azure CDN standardowe z Akamai** i **Azure CDN standardowy z Verizon**, zobacz [Kontrolowanie zachowania buforowania żądania sieci CDN za pomocą ciągi kwerend](cdn-query-string.md).

Dostępne są trzy tryby:

- **pamięć podręczna standardowe**: jest to tryb domyślny.  Węzeł krawędzi CDN przejdzie ciągu kwerendy z żądającego źródło na pierwsze żądanie i pamięci podręcznej elementu.  Wszystkie kolejne żądania dla tego zasobu, które są obsługiwane w węźle krawędzi zignoruje ciągu kwerendy do momentu wygaśnięcia zawartości pamięci podręcznej.
- **nie pamięci podręcznej**: W tym trybie żądania z ciągi kwerend nie są buforowane w węźle krawędzi CDN.  Węzeł krawędzi pobiera elementu bezpośrednio z punktu początkowego i przekazuje je do żądającego z każdego żądania.
- **unikatowe pamięci podręcznej**: w tym trybie traktuje każde żądanie z ciągu kwerendy jako unikatowe zasób ze swojej własnej pamięci podręcznej.  Na przykład odpowiedzi od punktu początkowego za żądanie *foo.ashx?q=bar* czy przechowywanych w pamięci podręcznej w węźle krawędzi i zwracane dla kolejnych pamięci podręcznej z tego samego ciągu kwerendy.  Wniosek o *foo.ashx?q=somethingelse* będzie buforowana jako osobne zasób ze swoim czasie na żywo.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Zmienianie ustawienia profilów CDN premium buforowania ciągu kwerendy

1. Z sieci CDN karta Profil kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-query-string-premium/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Umieść wskaźnik myszy na karcie **Dużych HTTP** , a następnie umieść wskaźnik myszy na menu wysuwane **Ustawienia pamięci podręcznej** .  Kliknij polecenie w **pamięci podręcznej w ciągu kwerendy**.

    Zostaną wyświetlone opcje buforowania ciągu kwerendy.

    ![Opcje buforowania ciągu kwerendy sieci CDN](./media/cdn-query-string-premium/cdn-query-string.png)

3. Po dokonaniu wyboru, kliknij przycisk **Aktualizuj** .


> [AZURE.IMPORTANT] Zmiany w ustawieniach mogą nie być widoczne natychmiast czas rejestracji propagowanie za pośrednictwem sieci CDN.  Profilów <b>CDN Azure z Verizon</b> propagowanie zakończy się zazwyczaj w ciągu 90 minut, ale w niektórych przypadkach może trwać dłużej.
