
<properties
    pageTitle="Interfejs API języka DocumentDB Java & SDK | Microsoft Azure"
    description="Wszystkie informacje o zestawu SDK wraz z datami wersji, emerytury i zmiany wprowadzone między poszczególnymi wersjami DocumentDB Java SDK i interfejsu API języka Java."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Interfejsy API DocumentDB i SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [POZOSTAŁE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>Interfejs API języka DocumentDB Java i zestaw SDK

<table>
<tr><td>**Pobierz zestaw SDK**</td><td>[Środowiska maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**Interfejs API dokumentacji**</td><td>[Interfejs API języka Java dokumentacji](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Współtworzenie SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Rozpoczynanie pracy**</td><td>[Rozpoczynanie pracy z Java SDK](documentdb-java-application.md)</td></tr>
<tr><td>**Bieżące środowisko uruchomieniowe obsługiwane**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Informacje o wersji

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Dodano obsługę BoundedStaleness spójności poziom.
  - Dodano obsługę bezpośredniej łączności dla OBSŁUGIWAŁ operacje dla zbiorów podzielone na partycje.
  - Stałe błąd w kwerendy bazy danych za pomocą SQL.
  - Błąd ustawiony w pamięci podręcznej sesji miejsce, w którym mogą być niepoprawna tokenu sesji.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Dodano obsługę się między partycją zapytania równolegle.
  - Dodano obsługę kwerend góry-ORDER BY dla zbiorów podzielone na partycje.
  - Dodano obsługę silnych spójności.
  - Dodano obsługę żądania nazwę podstawie podczas korzystania z bezpośredniej łączności.
  - Stałe identyfikatora pozostaną niezmienione przez wszystkich prób wezwanie.
  - Stałe błędu związane z pamięci podręcznej sesji podczas odtwarzania zbioru o takiej samej nazwie.
  - Dodano Wielokąt i typy obiektów LineString danych podczas zbioru indeksowania zasad ogrodzenia geo przestrzenna kwerend.
  - Stały problemy z dokumentem Java dla języka Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Błąd ustawiony w PartitionKeyDefinitionMap buforowanie zbiory jedną partycją i nie pozwalają wprowadzać dodatkowe pobierania podzielić żądania klucza.
  - Stałe błąd nie próbować po partition niepoprawne wartości klucza.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Dodano obsługę dla bazy danych w przypadku kont.
  - Dodano obsługę automatycznego ponawiania na ograniczonej żądania za pomocą opcji, aby dostosować maksymalna liczba ponownych prób i ponów próbę maksymalny czas oczekiwania.  Zobacz RetryOptions i ConnectionPolicy.getRetryOptions().
  - Przestarzałe IPartitionResolver podstawie podziału kodu niestandardowego. Użyj zbiory podzielone na partycje dla wyższych miejsca do magazynowania i przepustowość.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Obsługa zasad dodano ponów próbę ograniczania.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Dodano czas live (TTL) — Pomoc techniczna dla dokumentów.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Wdrożony [zbiory podzielone na partycje](documentdb-partition-data.md) i [poziomów wydajności zdefiniowane przez użytkownika](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[: 1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Błąd ustawiony w HashPartitionResolver do generowania wartości mieszania w małoendiański być zgodne z innymi SDK.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Dodawanie skrótu & zakres partycje rozpoznawania nazw ułatwiających z aplikacjami sharding u wielu partycje.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Wdrożenie Upsert. Nowe metody upsertXXX dodane do obsługi funkcji Upsert.
- Identyfikator Implementowanie podstawie Routing. Nie publicznej zmiany w interfejsie API, wewnętrznych wszystkie zmiany.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Aby wyświetlić numer wersji wyrównany innych SDK został pominięty wersji

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Obsługa geograficzne indeksu
- Sprawdzanie poprawności identyfikatora właściwości dla wszystkich zasobów. Identyfikatory zasobów nie mogą zawierać ?, /, # \, znaków lub na końcu spacją.
- Dodaje nowy nagłówek "indeks transformacja postępu" do ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Wykonuje zasad indeksowania w wersji 2

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- ZESTAW SDK GA

## <a name="release--retirement-dates"></a>Wersji i wycofywanie dat
Firma Microsoft udostępni powiadomienie co najmniej **12 miesięcy** przed emeryturę SDK było gładkie przejścia do nowszego nieobsługiwaną wersję.

Nowe funkcje i funkcje i optymalizacje tylko są dodawane do bieżącego zestawu SDK, co jest zalecane, zawsze uaktualnienie najnowszej SDK wersji najwcześniej, jak to możliwe.

Wszelkie żądanie DocumentDB przy użyciu wycofanych zestawu SDK zostaną odrzucone przez usługę.

> [AZURE.WARNING]
Wszystkie wersje SDK DocumentDB Azure dla języka Java przed wersji **1.0.0** zostanie wycofana na **29 lutego 2016**.

<br/>

| Wersja | Data wydania | Wycofywanie daty
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 października 2016 |---
| [1.9.0](#1.9.0) | 03 październik 2016 |---
| [1.8.1](#1.8.1) | 30 czerwca 2016 |---
| [1.8.0](#1.8.0) | 14 czerwca 2016 |---
| [1.7.1](#1.7.1) | 30 kwietnia 2016 |---
| [1.7.0](#1.7.0) | 27 kwietnia 2016 |---
| [1.6.0](#1.6.0) | 29 marca 2016 |---
| [: 1.5.1](#1.5.1) | 31 grudnia 2015 r. |---
| [1.5.0](#1.5.0) | 04 grudnia 2015 r. |---
| [1.4.0](#1.4.0) | 05 października 2015 r. |---
| [1.3.0](#1.3.0) | 05 października 2015 r. |---
| [1.2.0](#1.2.0) | 05 sierpnia 2015 r. |---
| [1.1.0](#1.1.0) | 09 lipca 2015 r. |---
| [1.0.1](#1.0.1) | 12 maj 2015 r. |---
| [1.0.0](#1.0.0) | 07 kwietnia 2015 r. |---
| 0.9.5-prelease | 09 marca 2015 r. | 29 lutego 2016
| 0.9.4-prelease | 17 lutego 2015 r. | 29 lutego 2016
| 0.9.3-prelease | 13 stycznia 2015 r. | 29 lutego 2016
| 0.9.2-prelease | 19 grudnia 2014 r. | 29 lutego 2016
| 0.9.1-prelease | 19 grudnia 2014 r. | 29 lutego 2016
| 0.9.0-prelease | 10 grudnia 2014 r. | 29 lutego 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat DocumentDB, zobacz strony usługi [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .
