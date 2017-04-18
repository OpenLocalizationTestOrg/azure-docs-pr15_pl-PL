<properties 
    pageTitle="Interfejs API DocumentDB Python & SDK | Microsoft Azure" 
    description="Wszystkie informacje o interfejsie Python API i zestawu SDK wraz z datami wersji, emerytury i zmiany wprowadzone między poszczególnymi wersjami DocumentDB Python SDK." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Interfejsy API DocumentDB i SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [POZOSTAŁE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>Interfejs API DocumentDB Python i zestaw SDK

<table>
<tr><td>**Pobierz zestaw SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**Interfejs API dokumentacji**</td><td>[Interfejs API Python dokumentacji](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**Instrukcje dotyczące instalacji zestawu SDK**</td><td>[Instrukcje dotyczące instalacji Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Współtworzenie SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Rozpoczynanie pracy**</td><td>[Rozpoczynanie pracy z Python SDK](documentdb-python-application.md)</td></tr>
<tr><td>**Bieżący obsługiwane platformy**</td><td>[Python 2.7](https://www.python.org/downloads/) i [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Informacje o wersji

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Dodano obsługę Python 3.5.
- Dodano obsługę buforowanie połączeń za pomocą modułu żądania.
- Dodano obsługę spójności sesji.
- Dodano obsługę kwerend góry-ORDERBY dla zbiorów podzielone na partycje.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Obsługa zasad dodano ponów próbę ograniczonej żądania. (Ograniczonej żądania otrzymają wezwanie stopa jest za duży wyjątek, kod błędu 429). Domyślnie DocumentDB prób dziewięciu razy dla każdego żądania po napotkaniu kod błędu 429 zachowaniu podczas retryAfter w nagłówku odpowiedź. Interwał ponawiania stały można teraz ustawić jako część właściwości RetryOptions w obiekcie ConnectionPolicy Jeśli chcesz zignorować czasu retryAfter zwrócony przez serwer między kolejnymi próbami. DocumentDB teraz zażądać czeka maksymalnie 30 sekund dla każdej z nich jest podejmowana ograniczenie (niezależnie od liczby powtórzeń) i zwraca odpowiedzi z kodem błędu 429. Teraz można także zastąpić we właściwości RetryOptions w obiekcie ConnectionPolicy.

- DocumentDB teraz zwraca wartość x-ms ograniczenia-ponów próbę count i x-ms-throttle-retry-wait-time-ms jako nagłówki odpowiedzi w każdej wezwaniu do oznaczania ograniczenia ponów próbę liczba i czas cummulative żądanie czas potrzebny między kolejnymi próbami.

- Usunięte klasy RetryPolicy i odpowiadające im właściwości (retry_policy) dostępne w klasie document_client i zamiast tego wprowadzone klasy RetryOptions Uwidacznianie właściwości RetryOptions w klasie ConnectionPolicy, który może być używany do zastępowania niektórych opcji domyślnych ponów próbę.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Dodano obsługę dla bazy danych w przypadku kont.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Dodano obsługę dla funkcji czasu do Live(TTL) dokumentów.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Poprawki związane z podziału po stronie serwera umożliwia znaków specjalnych w ścieżce partitionkey.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Wdrożony [zbiorów podzielone na partycje](documentdb-partition-data.md) i [poziomów wydajności zdefiniowane przez użytkownika](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Dodawanie skrótu i zakresu partycje rozpoznawania nazw ułatwiających z aplikacjami sharding u wielu partycje.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Wdrożenie Upsert. Nowe metody UpsertXXX dodane do obsługi funkcji Upsert.
- Identyfikator Implementowanie podstawie Routing. Nie publicznej zmiany w interfejsie API, wewnętrznych wszystkie zmiany.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Obsługa geograficzne indeks.
- Sprawdzanie poprawności identyfikatora właściwości dla wszystkich zasobów. Identyfikatory zasobów nie mogą zawierać ?, /, # \, znaków lub na końcu spacjami.
- Dodaje nowy nagłówek "indeks transformacja postępu" do ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Wprowadza zasad indeksowania w wersji 2.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Obsługuje połączenia serwera proxy.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Daty wersji i wycofywanie
Firma Microsoft udostępni powiadomienie co najmniej **12 miesięcy** przed emeryturę SDK było gładkie przejścia do nowszego nieobsługiwaną wersję.

Nowe funkcje i funkcje i optymalizacje tylko są dodawane do bieżącego zestawu SDK, co jest zalecane, zawsze uaktualnienie najnowszej SDK wersji najwcześniej, jak to możliwe. 

Wszelkie żądanie DocumentDB przy użyciu wycofanych zestawu SDK zostaną odrzucone przez usługę.

> [AZURE.WARNING]
Wszystkie wersje SDK DocumentDB Azure dla Python przed wersji **1.0.0** zostanie wycofana na **29 lutego 2016**. 

<br/>

| Wersja | Data wydania | Wycofywanie daty 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 września 2016 |---
| [1.9.0](#1.9.0) | 07 lipca 2016 |---
| [1.8.0](#1.8.0) | 14 czerwca 2016 |---
| [1.7.0](#1.7.0) | 26 kwietnia 2016 |---
| [1.6.1](#1.6.1) | 08 kwietnia 2016 |---
| [1.6.0](#1.6.0) | 29 marca 2016 |---
| [1.5.0](#1.5.0) | 03 stycznia 2016 r. |---
| [1.4.2](#1.4.2) | 06 października 2015 r. |---
| [1.4.1](#1.4.1) | 06 października 2015 r. |---
| [1.2.0](#1.2.0) | 06 sierpnia 2015 r. |---
| [1.1.0](#1.1.0) | 09 lipca 2015 r. |---
| [1.0.1](#1.0.1) | 25 maj 2015 r. |---
| [1.0.0](#1.0.0) | 07 kwietnia 2015 r. |---
| 0.9.4-prelease | 14 stycznia 2015 r. | 29 lutego 2016
| 0.9.3-prelease | 09 grudnia 2014 r. | 29 lutego 2016
| 0.9.2-prelease | 25 listopada 2014 r. | 29 lutego 2016
| 0.9.1-prelease | 23 września 2014 r. | 29 lutego 2016
| 0.9.0-prelease | 21 sierpnia 2014 r. | 29 lutego 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat DocumentDB, zobacz strony usługi [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 
