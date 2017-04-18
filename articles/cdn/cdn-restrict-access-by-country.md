<properties
    pageTitle="Ograniczenia dostępu do zawartości sieci Azure CDN według krajów | Microsoft Azure"
    description="Dowiedz się, jak ograniczyć dostęp do zawartości sieci Azure CDN za pomocą funkcji filtrowania Geo."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>Ograniczenia dostępu do zawartości według krajów - Verizon

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Standardowe Akamai](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Omówienie

Gdy użytkownik zażąda tej zawartości, domyślnie, bez względu na miejsce, w którym użytkownik wprowadzone tego wniosek jest udostępniany zawartości. W niektórych przypadkach może być ograniczenia dostępu do zawartości według krajów. W tym temacie wyjaśniono, jak za pomocą funkcji **Filtrowania Geo** Aby skonfigurować usługę, aby zezwolić lub blokowanie dostępu według krajów.

> [AZURE.IMPORTANT] Produkty Verizon i Akamai zawierają te same funkcje filtrowania geo, ale różni się w interfejsie użytkownika. W tym dokumencie opisano interfejs usłudze **Azure CDN standardowy z Verizon** i **Premium CDN Azure z Verizon**. Aby geo filtrowania z **Azure CDN standardowy z Akamai**zobacz [ograniczenia dostępu do zawartości według krajów - Akamai](cdn-restrict-access-by-country-akamai.md).

Informacje zagadnienia, które dotyczą Konfigurowanie ten typ ograniczeń zobacz sekcję [Uwagi](cdn-restrict-access-by-country.md#considerations) na końcu tematu.  

>[AZURE.NOTE] Po skonfigurowaniu konfiguracji zostaną zastosowane do wszystkich **CDN Azure z Verizon** punkty końcowe w tym profilu Azure CDN.

![Filtrowanie kraju](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Krok 1: Definiowanie ścieżkę katalogu

Konfigurując filtr kraju, musisz określić względną ścieżkę do lokalizacji, do której użytkownicy będą dozwolone lub dostępu. Możesz zastosować kraju filtrowanie wszystkie pliki z "/" lub wybrane foldery, określając ścieżki katalogów.

Filtr ścieżkę katalogu przykładzie:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Krok 2: Definiowanie akcji: blokowanie lub zezwalanie

**Blok:** Użytkownicy z określonych krajów nie będą mieli dostęp do trwałych zażąda ścieżka cykliczne. Jeśli nie innego kraju opcje filtrowania zostały skonfigurowane dla tej lokalizacji, następnie wszyscy inni użytkownicy mogą uzyskiwać dostęp.

**Zezwalaj:** Tylko użytkownicy z określonych krajów mogą uzyskiwać dostęp do zasobów z tej ścieżki cykliczne.

##<a name="step-3-define-the-countries"></a>Krok 3: Definiowanie krajów

Wybierz kraje, które mają być blokowanie lub zezwalanie ścieżki. Aby uzyskać więcej informacji zobacz [CDN Azure z kody krajów Verizon](https://msdn.microsoft.com/library/mt761717.aspx).

Na przykład reguły blokowania /Photos/Strasburgu/będzie filtrować pliki w tym:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Kody krajów

Funkcja **Filtrowania Geo** używa kodów krajów do definiowania krajów, z których żądanie zostanie dozwolone lub blokowane w usłudze bezpiecznego katalogu. Kody krajów można znaleźć w [Sieci CDN Azure z kody krajów Verizon](https://msdn.microsoft.com/library/mt761717.aspx). Jeśli użytkownik określi "Unii Europejskiej" (Europa) lub "Aplikacji" (Azji i Pacyfiku) podzbiór adresów IP, które pochodzą z dowolnego kraju, w tym regiony zostaną zablokowane lub dozwolone.


##<a id="considerations"></a>Zagadnienia dotyczące

- Do 90 minut zmiany może potrwać do filtrowania konfiguracji kraju została uwzględniona.
- Ta funkcja nie obsługuje symboli wieloznacznych (na przykład "*").
- Kraj filtrowania konfiguracji skojarzone z ścieżkę względną będą stosowane lokalizacji do tej ścieżki.
- Tylko jedna reguła mogą być stosowane do tej samej ścieżki względne (nie można utworzyć wiele filtrów kraju, wskazujące na tej samej ścieżki względne. Folder może mieć jednak wiele filtrów kraju. Jest to spowodowane cykliczne rodzaj filtrów kraju. Innymi słowy podfolder folderu wcześniej skonfigurowane można przypisywać filtru innego kraju.
