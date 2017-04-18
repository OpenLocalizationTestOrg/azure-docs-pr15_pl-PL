<properties
   pageTitle="Logika aplikacje zawartości wpisz obsługi | Microsoft Azure"
   description="Zrozumieć, jak aplikacje logiczny dotyczy typów zawartości na projekt i środowisko uruchomieniowe"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Typ zawartości aplikacje logiki obsługi

Istnieje wiele typów zawartości, która może przechodzić za pośrednictwem aplikacji logika — w tym JSON, XML, prostym pliki i dane binarne.  Gdy wszystkie typy zawartości są obsługiwane, niektóre macierzyste zrozumie aparat aplikacje logiki i inne osoby mogą wymagać rzutowania lub konwersji stosownie do potrzeb.  Następujący artykuł będzie opisano, jak aparat obsługuje różne typy zawartości oraz sposób ich można poprawnie obsługi stosownie do potrzeb.

## <a name="content-type-header"></a>Typ zawartości nagłówka

Aby uruchomić prosty, Przyjrzyjmy się dwóch `Content-Types` które nie wymagają konwersji ani rzutowania korzystać w aplikacji logika — `application/json` i `text/plain`.

### <a name="applicationjson"></a>Aplikacja json

Aparat przepływu pracy zależy od `Content-Type` nagłówka z HTTP połączeń do ustalania obsługi właściwe.  Każdy wniosek o typie zawartości `application/json` są przechowywane i obsługiwane jako obiekt JSON.  Ponadto JSON zawartości można przeanalizować domyślnie bez określania jej wszelkie rzutowania.  Dlatego żądanie, które ma nagłówek typ zawartości `application/json ` następująco:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

można przeanalizować w przepływie pracy przy użyciu wyrażenia takiego jak `@body('myAction')['foo'][0]` do uzyskania wartości (w tym przypadku `bar`).  Nie dodatkowych rzutowanie jest potrzebny.  Jeśli pracujesz z danymi, który jest JSON, ale nie zawiera jeszcze nagłówka określony, możesz go ręcznie rzutowane na JSON przy użyciu `@json()` funkcji (na przykład: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Zwykły tekst

Podobne do `application/json`, wiadomości HTTP otrzymane z `Content-Type` nagłówek `text/plain` będą przechowywane w jego surowej formie.  Ponadto jeśli zawarte w kolejnych akcji bez rzutowania dowolnego żądania przejdzie z `Content-Type`: `text/plain` nagłówka.  Na przykład w pracy z plikiem prostym może zostać wyświetlony następujący zawartości w formacie HTTP:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Jeśli w następnej akcji możesz wysyłać je w treści wezwania na inny (`@body('flatfile')`), wniosek musi `text/plain` nagłówka typ zawartości.  Jeśli pracujesz z danymi, który jest zwykły tekst, ale nie zawiera jeszcze nagłówka określony, możesz go ręcznie rzutowane na tekst przy użyciu `@string()` funkcji (na przykład: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Kod xml i aplikacji i aplikacja/oktet strumienia i funkcje konwersji

Zawsze zachowuje aparat aplikacji logika `Content-Type` który otrzymano na żądanie HTTP lub odpowiedź.  Co to oznacza to, jeśli zawartość jest odebrana z `Content-Type` z `application/octet-stream`, obejmujący w kolejnych akcji dotyczącej nie rzutowania spowoduje żądanie wychodzących z `Content-Type`: `application/octet-stream`.  W ten sposób aparat może guaruntee dane nie zostaną utracone podczas przemieszczał się w całym przepływu pracy.  Jednak stan akcji (dane wejściowe i wyjściowe) są przechowywane w obiekcie JSON, jak przepływał całej przepływu pracy.  Oznacza to, że w celu zachowania niektórych typów danych, aparat zostanie przekonwertowana zawartość do ciąg binarny base64 zakodowany z odpowiednich metadanych, który zachowuje oba `$content` i `$content-type` — którego będą automatycznie konwertowane.  Można też ręcznie przekonwertować między typami zawartości przy użyciu wbudowanych w funkcjach konwertera:

* `@json()`-rzutuje danych`application/json`
* `@xml()`-rzutuje danych`application/xml`
* `@binary()`-rzutuje danych`application/octet-stream`
* `@string()`-rzutuje danych`text/plain`
* `@base64()`— Konwertuje ciąg base64 zawartości
* `@base64toString()`— Konwertuje ciąg zakodowany base64`text/plain`
* `@base64toBinary()`— Konwertuje ciąg zakodowany base64`application/octet-stream`
* `@encodeDataUri()`-kodowane ciąg w postaci tablicy bajtów dataUri
* `@decodeDataUri()`-dekoduje dataUri do tablicy bajtów

Na przykład w przypadku otrzymania żądania HTTP z `Content-Type`: `application/xml` programu:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Można zrzutowania, a później użyć z podobną do `@xml(triggerBody())`, lub w obrębie danej funkcji, takich jak `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Typy zawartości inne

Inne typy zawartości są obsługiwane i działa z aplikacją logiczny, ale może wymagać ręczne pobieranie treści wiadomości przez użytkownika `$content`.  Na przykład można powodujące zostały wyłączone z `application/x-www-url-formencoded` żądanie, która zawierała następująco:

```
CustomerName=Frank&Address=123+Avenue
```

ponieważ to nie zwykłego tekstu lub JSON będą przechowywane w działaniu jako:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Miejsce, w którym `$content` jest ładunku kodowane jako ciąg base64, aby zachować wszystkie dane.  Ponieważ nie istnieje obecnie natywnych funkcji dla danych formularza, nadal można używać tych danych w ramach przepływu pracy, ręcznie dostęp do danych przy użyciu funkcji, takich jak `@string(body('formdataAction'))`.  Jeśli miała Moje wezwanie wychodzącej musi mieć również `application/x-www-url-formencoded` nagłówek typ zawartości, może po prostu dodać go do treści akcji bez dowolnego rzutowania, takich jak `@body('formdataAction')`.  Jednak działa tylko w przypadku treści to jedyny parametr w `body` wprowadzania danych.  Jeśli spróbujesz wykonaj `@body('formdataAction')` w `application/json` żądanie zostanie wyświetlony błąd wykonania jako wysyła zakodowany treści.
