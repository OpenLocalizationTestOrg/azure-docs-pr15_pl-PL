<properties
    pageTitle="Azure CDN za pomocą CORS | Microsoft Azure"
    description="Dowiedz się, jak korzystać z udostępniania zasobu między Origin (CORS) Azure zawartości dostawy sieci (CDN) do."
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
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Azure CDN za pomocą CORS     

## <a name="what-is-cors"></a>Co to jest CORS?

CORS (krzyżowe Origin współużytkowanie zasobów) jest funkcja HTTP, która umożliwia aplikacja sieci web w obszarze jedną domenę dostęp do zasobów w innej domeny. W celu zmniejszenia możliwości atakami skryptów między witrynami, wszystkie przeglądarki sieci web nowoczesny zaimplementować ograniczenia zabezpieczeń znana pod nazwą [zasad samego pochodzenia](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Dzięki temu połączeń interfejsy API w domenie innej strony sieci web.  CORS udostępnia bezpieczny sposób, aby umożliwić jedną domenę (domeny pochodzenia) połączenie interfejsy API w innej domeny.
 
## <a name="how-it-works"></a>Jak to działa
1.  Przeglądarka przesyła żądanie opcji z nagłówkiem **pochodzenia** HTTP. Wartość tego nagłówka jest domeną, które obsługiwane strony nadrzędnej. Gdy strony z https://www.contoso.com próbuje uzyskać dostęp do danych użytkownika w domenie fabrikam.com, następujące nagłówek będzie można wysłać do fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  Serwer będzie działać z następujących czynności:
    - Nagłówek **Programu Access Control-Zezwalaj-początkowego** w odpowiedzi na wskazujące, które witryny pochodzenia są dozwolone. Na przykład:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Jeśli serwer nie zezwala na żądanie origin między strony błędu
    - Nagłówek **Programu Access Control-Zezwalaj-początkowego** przy użyciu symboli wieloznacznych, która zezwala na wszystkich domen:
        
        `Access-Control-Allow-Origin: *`
 
Złożone żądania HTTP jest żądanie "wstępnej" gotowe najpierw określ, czy jej uprawnień przed wysłaniem całego żądania.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Symbol wieloznaczny lub pojedynczy origin scenariuszy

CORS na Azure CDN będzie działać automatycznie z żadne dodatkowe czynności konfiguracyjne, gdy nagłówek **Programu Access Control-Zezwalaj-początkowego** jest ustawiona na symbol wieloznaczny (*) lub pojedynczy pochodzenia.  Sieci CDN będzie pamięci podręcznej pierwszej odpowiedzi i kolejne żądania będą używały ten sam nagłówek.
 
Jeśli już wprowadzono żądania CDN przed CORS ustawiany na swojej pochodzenia, będzie konieczne Wyczyść zawartość na zawartość punktu końcowego tak, aby ponownie załadować zawartości z nagłówkiem **Programu Access Control-Zezwalaj-początkowego** .
 
## <a name="multiple-origin-scenarios"></a>Wielu scenariuszy pochodzenia

Jeśli chcesz zezwolić określonej listy miejsc pochodzenia, aby było dozwolone dla CORS, co uzyskać nieco bardziej skomplikowane. Ten problem występuje, gdy CDN buforowanie nagłówka **Programu Access Control-Zezwalaj-początkowego** dla pierwszej pochodzenia CORS.  W przypadku innego źródła CORS kolejne żądanie, CDN będzie ilości pamięci podręcznej nagłówka **Programu Access Control-Zezwalaj-początkowego** , który nie odpowiada.  Istnieje kilka sposobów, aby rozwiązać ten problem.
 
### <a name="azure-cdn-premium-from-verizon"></a>Premium Azure CDN z Verizon

Najlepszym sposobem, aby włączyć tę jest używanie **Premium CDN Azure z Verizon**, która udostępnia niektóre funkcje zaawansowane. 
 
Musisz [utworzyć regułę](cdn-rules-engine.md) sprawdzić **pochodzenie** nagłówek na żądanie.  Jeśli jest prawidłowe origin regułę spowoduje ustawienie nagłówka **Programu Access Control-Zezwalaj-początkowego** wraz ze źródłem dostarczony z żądaniem.  Jeśli origin określony w nagłówku **pochodzenia** nie jest dozwolony, reguły należy pominąć nagłówek **Programu Access Control-Zezwalaj-początkowego** , co spowoduje przeglądarkę, aby odrzucić żądanie. 
 
Istnieją dwa sposoby, w tym aparat reguł.  W obu przypadkach nagłówka **Programu Access Control-Zezwalaj-początkowego** z serwera pochodzenia pliku całkowicie jest ignorowana, aparat reguł CDN całkowicie zarządza dozwolonych miejsc pochodzenia CORS.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Jeden wyrażeń regularnych z wszystkie prawidłowe miejsc pochodzenia
 
W tym przypadku utworzysz wyrażeń regularnych, które zawiera wszystkie miejsc pochodzenia, który chcesz zezwolić: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Sieci CDN Azure z Verizon** używa [Zgodne wyrażeń regularnych Perl](http://pcre.org/) jako jego aparat dla wyrażeń regularnych.  Narzędzie, takie jak [101 wyrażeń regularnych](https://regex101.com/) służy do sprawdzania poprawności do wyrażeń regularnych.  Należy zauważyć, że znak "/" jest ważna w wyrażeń regularnych, a nie należy wstawić, jednak ucieczce znaku najbardziej skuteczne rozwiązanie uznaje i oczekuje przy niektórych moduły regex.

Jeśli wyrażeń regularnych nie odpowiada, reguły zastąpi nagłówka **Programu Access Control-Zezwalaj-początkowego** (jeśli istnieją) względem punktu początkowego wraz ze źródłem, który wysłał żądanie.  Można również dodać dodatkowe nagłówki CORS, takich jak **Metody kontroli dostępu-Zezwalaj**.

![Przykład reguły z wyrażeń regularnych](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Żądanie nagłówka regułę dla każdego miejsca pochodzenia.

Zamiast wyrażeń regularnych zamiast tego można utworzyć regułę oddzielnych dla każdego miejsca pochodzenia, aby zezwolić na używanie **Żądanie symboli nagłówka** , [Uwzględnij warunku](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Podobnie jak w przypadku metody wyrażeń regularnych wyłącznie aparat reguł zestawów nagłówków CORS. 
  
![Przykładowe reguły bez wyrażeń regularnych](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] W przykładzie powyżej, użyj znaku wieloznacznego * informuje aparat reguł, aby dopasować HTTP i HTTPS.
 
### <a name="azure-cdn-standard"></a>Standardowe Azure CDN

Na standardowy CDN Azure profile tylko mechanizm umożliwiające wiele miejsc pochodzenia bez użycia pochodzenia symboli wieloznacznych jest użycie [pamięci podręcznej ciągu kwerendy](cdn-query-string.md).  Musisz ustawienie ciągu kwerendy dla punktu końcowego CDN, a następnie użyj ciągu kwerendy unikatowe dla żądań z każdej dozwolone domeny. Spowoduje to spowoduje CDN pamięci podręcznej oddzielny obiekt dla każdego ciągu kwerendy unikatowe. Ta metoda nie jest idealnym rozwiązaniem, jednak jako spowoduje wiele kopii tego samego pliku przechowywanych w pamięci podręcznej na CDN.  

