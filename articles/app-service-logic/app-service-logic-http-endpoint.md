<properties
   pageTitle="Aplikacje logiczny jako żądanie punkty końcowe"
   description="Jak utworzyć i skonfigurować wyzwalanie punkty końcowe i używanie ich w aplikacji dla logiki Azure aplikacji usługi"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Aplikacje logiczny jako żądanie punkty końcowe

Logika aplikacje oryginalnie pozwala udostępnić synchroniczne końcowego HTTP jako wyzwalacz.  Za pomocą struktury żądanie punktów końcowych do wywołania logikę aplikacji jako zagnieżdżony przepływ pracy za pomocą akcji "przepływ pracy", w aplikacji logikę.

Istnieją 3 typy wyzwalaczy, które może odbierać żądania:

* Żądanie
* ApiConnectionWebhook
* HttpWebhook

W dalszej części artykułu użyjemy **żądanie** jako przykład, bez identyczne dotyczą wszystkich zasad innych typów 2 wyzwalaczy.

## <a name="adding-a-trigger-to-your-definition"></a>Dodawanie wyzwalacza do swojej definicji
Pierwszym krokiem jest dodanie wyzwalacza do definicji aplikacji usługi logiczny, który może odbierać przychodzących wezwań.  Możesz wyszukać w Projektancie dla "HTTP żądanie" dodać kartę wyzwalacza. Można zdefiniować treści żądania schematu JSON i projektanta wygeneruje tokeny ułatwiających analizowania i przekazywania danych z ręcznego wyzwalacza w przepływie pracy.  Najlepiej generowanie schematu JSON na podstawie próbki ładunku treści za pomocą narzędzi, takich jak [jsonschema.net](http://jsonschema.net) .

![Żądanie karty wyzwalacza][2]

Po zapisaniu do definicji aplikacji logika zwrotnego adresu URL będą generowane podobny do tego:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Ten adres URL zawiera klucz skojarzeń zabezpieczeń w parametry kwerendy używany do uwierzytelniania.

Możesz również wyświetlić tego punktu końcowego w portalu Azure:

![][1]

Lub, dzwoniąc:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Zabezpieczenia dla adresu URL wyzwalacza

Adresy URL zwrotnego aplikacji logika są generowane przy użyciu podpisu dostępu udostępnione.  Podpis jest przekazywany jako parametru zapytania i musi być zweryfikowany przed spowoduje uruchomienie aplikacji logicznych.  Jest generowany przez unikatową kombinację tajnego klucza na logiki aplikacji, nazwa wyzwalacza i wykonywania operacji.  Chyba że ktoś ma dostęp do klucza tajnego logiki aplikacji, może nie być w wygenerować podpisu.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Wywoływanie wyzwalacza aplikacji logika punktu końcowego

Po utworzeniu punkt końcowy wyzwalacza, może spowodować jego za pośrednictwem `POST` do pełnego adresu URL. Możesz umieścić dodatkowe nagłówki, a cała zawartość w treści.

Jeśli typ zawartości jest `application/json` będzie mógł właściwości odwołania z wewnątrz wezwanie. W przeciwnym razie jest traktowany jako całość binarne mogą być przekazywane do innych interfejsów API, ale nie można odwoływać się wewnątrz przepływ pracy bez konwertowania zawartości.  Na przykład po pomyślnym zakończeniu `application/xml` zawartości można użyć `@xpath()` do wyodrębniania xpath lub `@json()` Aby przekonwertować z pliku XML JSON.  Więcej informacji na temat pracy z zawartością typy [można znaleźć tutaj](app-service-logic-content-type.md)

Ponadto można określić schemat JSON w definicji. Ta opcja powoduje projektanta, aby wygenerować tokeny, które następnie można przekazać na stopnie.  Na przykład poniższy spowoduje, że `title` i `name` token dostępne w Projektancie:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Odwoływanie się do zawartości przychodzące żądanie

`@triggerOutputs()` Funkcji będzie wyjściowy zawartość przychodzące żądanie. Na przykład będzie wyglądał, takich jak:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Możesz użyć `@triggerBody()` skrótów, aby uzyskać dostęp do `body` właściwości specjalnie. 

## <a name="responding-to-the-request"></a>Odpowiadanie na żądanie

Dla niektórych żądań, które uruchamianie aplikacji logiczny warto odpowiedzieć na kilka zawartości do osoby wywołującej. Istnieje nowy typ akcji o nazwie **odpowiedź** , która służy do tworzenia kodu stanu, podstawowego i nagłówków odpowiedź. Notatki, że jeśli ma żaden kształt **odpowiedzi** punkt końcowy aplikacji logika *natychmiast* będą odpowiadanie za pomocą **202 zaakceptowane**.

![Akcja odpowiedź HTTP][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Odpowiedzi są następujące czynności:

| Właściwość | Opis |
| -------- | ----------- |
| statusCode | Kod stanu HTTP na odpowiedź na żądanie przychodzące. Może być cały kod nieprawidłowy stan, który zaczyna się od 2xx, 4xx lub 5xx. kody stanu 3xx nie są dozwolone. | 
| Treść | Obiekt treści, który może być ciągiem, JSON obiektu lub nawet binarny zawartości przez odwołanie w poprzednim kroku. | 
| nagłówki | Można zdefiniować dowolną liczbę nagłówków, które mają zostać uwzględnione w odpowiedzi | 

Wszystkie kroki w logiczny aplikacji, które są wymagane do odpowiedzi należy wykonać w ciągu *60 sekund* oryginalnego żądania do odbierania odpowiedzi **chyba że przepływ pracy jest wywoływana jako zagnieżdżonych aplikacji logicznych**. Po osiągnięciu żadnej akcji odpowiedzi w ciągu 60 sekund, a następnie przychodzące żądanie zostanie limit czasu i otrzymujesz odpowiedź **408 klienta limitu czasu** HTTP.  W przypadku zagnieżdżonych aplikacji logika nadrzędny logiki aplikacji będzie w dalszym ciągu oczekiwanie na odpowiedź, dopóki nie zakończy, bez względu na czas wystarczy.

## <a name="advanced-endpoint-configuration"></a>Konfiguracji zaawansowanej punktu końcowego

Logika aplikacji jest wbudowany w pomocy technicznej dla punktu końcowego bezpośredni dostęp i zawsze używaj `POST` metody, aby rozpocząć uruchamianie aplikacji logika. Aplikacja **Odbiornik HTTP** interfejsu API wcześniej również obsługiwane zmianę segmenty adresu URL i metoda HTTP. Możesz nawet Ustaw dodatkowe zabezpieczenia lub domenę niestandardową przez dodanie go do hosta aplikacji interfejsu API (aplikacji sieci Web hostowanej aplikacji interfejsu API). 

Ta funkcja jest dostępna za pośrednictwem **interfejsu API zarządzania**:
* [Zmienianie metody żądania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Zmienianie segmenty adresu URL żądania](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Konfigurowanie interfejsu API zarządzania domenami na karcie **Konfiguruj** w klasycznym portal Azure
* Konfigurowanie zasad do wyszukania uwierzytelnianie podstawowe (**wymagane łącze**)

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Podsumowanie migracji z 2014-12-01-Podgląd

|  2014-12-01-Podgląd | 2016-06-01 |
|---------------------|--------------------|
| Kliknij w aplikacji **Odbiornik HTTP** interfejsu API | Polecenie **wyzwalacza ręcznego** (nie interfejsu API aplikacja wymagane) |
| Ustawienie "*wysyła odpowiedzi automatyczne*" odbiornik HTTP | Zawierają akcji **odpowiedzi** lub nie w definicji przepływu pracy |
| Konfigurowanie uwierzytelniania OAuth lub basic | za pośrednictwem interfejsu API management |
| Konfigurowanie metodę HTTP | za pośrednictwem interfejsu API management |
| Konfigurowanie ścieżki względne | za pośrednictwem interfejsu API management |
| Dokumentacja dotycząca przychodzących treści za pomocą`@triggerOutputs().body.Content` | Odwołanie za pośrednictwem`@triggerOutputs().body` |
| Akcja **HTTP wysłać odpowiedź** na odbiornika protokołu HTTP | Polecenie **odpowiedź na żądanie HTTP** (nie interfejsu API aplikacja wymagane)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
