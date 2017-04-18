<properties
    pageTitle="Interfejs API DocumentDB Node.js & SDK | Microsoft Azure"
    description="Wszystkie informacje o interfejsie Node.js API i zestawu SDK wraz z datami wersji, emerytury i zmiany wprowadzone między poszczególnymi wersjami DocumentDB Node.js SDK."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Interfejsy API DocumentDB i SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [POZOSTAŁE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>Interfejs API DocumentDB Node.js i zestaw SDK

<table>
<tr><td>**Pobierz zestaw SDK**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**Interfejs API dokumentacji**</td><td>[Interfejs API node.js dokumentacji](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**Instrukcje dotyczące instalacji zestawu SDK**</td><td>[Instrukcje dotyczące instalacji](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Współtworzenie SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Przykłady**</td><td>[Przykłady kodu node.js](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Samouczek Wprowadzenie Get**</td><td>[Rozpoczynanie pracy z Node.js SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Samouczek aplikacji sieci Web**</td><td>[Tworzenie aplikacji sieci web Node.js przy użyciu DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Bieżący obsługiwane platformy**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Informacje o wersji

###<a name="1.10.0"/>1.10.0</a>

- Dodano obsługę się między partycją zapytania równolegle.
- Dodano obsługę kwerend góry-ORDER BY dla zbiorów podzielone na partycje.

###<a name="1.9.0"/>1.9.0</a>

- Obsługa zasad dodano ponów próbę ograniczonej żądania. (Ograniczonej żądania otrzymają wezwanie stopa jest za duży wyjątek, kod błędu 429). Domyślnie DocumentDB prób dziewięciu razy dla każdego żądania po napotkaniu kod błędu 429 zachowaniu podczas retryAfter w nagłówku odpowiedź. Interwał ponawiania stały można teraz ustawić jako część właściwości RetryOptions w obiekcie ConnectionPolicy Jeśli chcesz zignorować czasu retryAfter zwrócony przez serwer między kolejnymi próbami. DocumentDB teraz zażądać czeka maksymalnie 30 sekund dla każdej z nich jest podejmowana ograniczenie (niezależnie od liczby powtórzeń) i zwraca odpowiedzi z kodem błędu 429. Teraz można także zastąpić we właściwości RetryOptions w obiekcie ConnectionPolicy.

- DocumentDB teraz zwraca wartość x-ms ograniczenia-ponów próbę count i x-ms-throttle-retry-wait-time-ms jako nagłówki odpowiedzi w każdej wezwaniu do oznaczania ograniczenia ponów próbę liczba i czas cummulative żądanie czas potrzebny między kolejnymi próbami.

- Został dodany klasy RetryOptions Uwidacznianie właściwość RetryOptions klasy ConnectionPolicy, który może być używany do zastępowania niektórych opcji domyślnych ponów próbę.

###<a name="1.8.0"/>1.8.0</a>

 - Dodano obsługę dla bazy danych w przypadku kont.

###<a name="1.7.0"/>1.7.0</a>

- Dodano obsługę dla funkcji czasu do Live(TTL) dokumentów.

###<a name="1.6.0"/>1.6.0</a>

- Wdrożony [zbiorów podzielone na partycje](documentdb-partition-data.md) i [poziomów wydajności zdefiniowane przez użytkownika](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- Stały błąd RangePartitionResolver.resolveForRead miejsce, w którym nie została powraca łącza ze względu na nieprawidłowe "concat" wyników.

###<a name="1.5.5"/>1.5.5</a>

- Stałe hashParitionResolver resolveForRead(): gdy żaden klucz partition dostarczonym została generowania wyjątku, zamiast powrócić listę wszystkich łączy zarejestrowanych.

###<a name="1.5.4"/>1.5.4</a>

- Rozwiązania problemu [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - zarezerwowane agenta HTTPS: uniknąć modyfikowania globalnej agenta na potrzeby DocumentDB. Za pomocą agenta dedykowane dla wszystkich żądań biblioteki.

###<a name="1.5.3"/>1.5.3</a>

- Rozwiązania problemów z [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - prawidłowo uchwyt kreski identyfikatory multimediów.

###<a name="1.5.2"/>1.5.2</a>

- Rozwiązania problemów [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter odbiornika pamięci ostrzeżenie.

###<a name="1.5.1"/>: 1.5.1</a>

- Poprawki wydać [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - Zmień nazwę folderu mieszania skrótu dla systemów uwzględniana wielkość liter.

### <a name="1.5.0"/>1.5.0</a>

- Obsługa sharding Implementowanie przez dodanie skrótu & zakres partition rozpoznawania nazw.

### <a name="1.4.0"/>1.4.0</a>

- Wdrożenie Upsert. Nowe metody upsertXXX na documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Pominięte, aby wyświetlić numery wersji wyrównany innych SDK.

### <a name="1.2.2"/>1.2.2</a>

- Pytania podziału projektuje opakowanie do nowego repozytorium.
- Zaktualizuj do pliku pakietu npm rejestru.

### <a name="1.2.1"/>1.2.1</a>

- Narzędzi identyfikator zależności Routing.
- Rozwiązania problemu [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - bieżącej właściwości jest w konflikcie z metody current().

### <a name="1.2.0"/>1.2.0</a>

- Dodano obsługę geograficzne indeksu.
- Sprawdzanie poprawności identyfikatora właściwości dla wszystkich zasobów. Identyfikatory zasobów nie może zawierać?, /, # & #47; & #47; znaków lub na końcu spacjami.
- Dodaje nowy nagłówek "indeks transformacja postępu" do ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>

- Wprowadza zasad indeksowania w wersji 2.

### <a name="1.0.3"/>1.0.3</a>

- Problem — nr 40 (https://github.com/Azure/azure-documentdb-node/issues/40) - wdrożonej eslint grunt konfiguracji i w podstawowych i zobowiązanie SDK.

### <a name="1.0.2"/>1.0.2</a>

- Problem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - opakowanie ze zobowiązania nie zawiera nagłówka z powodu błędu.

### <a name="1.0.1"/>1.0.1</a>

- Wdrożony możliwość wykonywania kwerend dotyczących konfliktów, dodając readConflicts, readConflictAsync i queryConflicts.
- Zaktualizowana dokumentacja interfejsu API.
- Emisja [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync błędu.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Wersji i wycofywanie dat
Firma Microsoft udostępni powiadomienie co najmniej **12 miesięcy** przed emeryturę SDK było gładkie przejścia do nowszego nieobsługiwaną wersję.

Nowe funkcje i funkcje i optymalizacje tylko są dodawane do bieżącego zestawu SDK, co jest zalecane, zawsze uaktualnienie najnowszej SDK wersji najwcześniej, jak to możliwe.

Wszelkie żądanie DocumentDB przy użyciu wycofanych zestawu SDK zostaną odrzucone przez usługę.

> [AZURE.WARNING]
Wszystkie wersje SDK DocumentDB Azure dla Node.js przed wersji **1.0.0** zostanie wycofana na **29 lutego 2016**.

<br/>

| Wersja | Data wydania | Wycofywanie daty
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 październik 2016 |---
| [1.9.0](#1.9.0) | 07 lipca 2016 |---
| [1.8.0](#1.8.0) | 14 czerwca 2016 |---
| [1.7.0](#1.7.0) | 26 kwietnia 2016 |---
| [1.6.0](#1.6.0) | 29 marca 2016 |---
| [1.5.6](#1.5.6) | 08 marca 2016 |---
| [1.5.5](#1.5.5) | 02 lutego 2016 |---
| [1.5.4](#1.5.4) | 01 lutego 2016 |---
| [1.5.2](#1.5.2) | 26 stycznia 2016 r. |---
| [1.5.2](#1.5.2) | 22 stycznia 2016 r. |---
| [: 1.5.1](#1.5.1) | 4 stycznia 2016 r. |---
| [1.5.0](#1.5.0) | 31 grudnia 2015 r. |---
| [1.4.0](#1.4.0) | 06 października 2015 r. |---
| [1.3.0](#1.3.0) | 06 października 2015 r. |---
| [1.2.2](#1.2.2) | 10 września 2015 r. |---
| [1.2.1](#1.2.1) | 15 sierpnia 2015 r. |---
| [1.2.0](#1.2.0) | 05 sierpnia 2015 r. |---
| [1.1.0](#1.1.0) | 09 lipca 2015 r. |---
| [1.0.3](#1.0.3) | 04 czerwca 2015 r. |---
| [1.0.2](#1.0.2) | 23 maj 2015 r. |---
| [1.0.1](#1.0.1) | 15 maj 2015 r. |---
| [1.0.0](#1.0.0) | 08 kwietnia 2015 r. |---
| 0.9.4-Prerelease | 06 kwiecień 2015 r. | 29 lutego 2016
| 0.9.3-Prerelease | 14 stycznia 2015 r. | 29 lutego 2016
| 0.9.2-Prerelease | 18 grudnia 2014 r. | 29 lutego 2016
| 0.9.1-Prerelease | 22 sierpnia 2014 r. | 29 lutego 2016
| 0.9.0-Prerelease | 21 sierpnia 2014 r. | 29 lutego 2016


## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat DocumentDB, zobacz strony usługi [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .
