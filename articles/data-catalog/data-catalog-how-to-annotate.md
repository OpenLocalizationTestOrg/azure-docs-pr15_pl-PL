<properties
   pageTitle="Jak dodawać adnotacje źródeł danych | Microsoft Azure"
   description="Artykule wyróżnianie jak dodawać adnotacje danych składników majątku w wykazie danych Azure, w tym przyjazne nazwy, znaczniki, opisy i ekspertów."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Jak dodawać adnotacje źródeł danych

## <a name="introduction"></a>Wprowadzenie
**Wykaz danych Microsoft Azure** to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania dla źródła danych przedsiębiorstwa. Innymi słowy wykaz danych jest pomaga osobom odnajdowanie, opis i więcej korzyści z ich istniejących danych przy użyciu źródeł danych i organizacji Pomoc. Gdy źródła danych jest zarejestrowany z wykazu danych, metadanych jest kopiowana i indeksowania przez usługę, ale wątku nie kończą się tam. Wykaz danych umożliwia podanie własnych opisowy metadanych — takie jak opisy i znaczniki — uzupełnienie metadane wyodrębnionych ze źródła danych, a aby wprowadzić bardziej zrozumiałe źródła danych więcej osób.

## <a name="annotation-and-crowdsourcing"></a>Adnotacja i crowdsourcing
Wszyscy mają opinii. I to jest to użyteczna.
Wykaz danych rozpoznaje różnych użytkowników że perspektyw na źródła danych przedsiębiorstwa oraz że każdy z perspektywy te mogą być przydatne. Scenariusz:

* Administrator systemu zna umowy dotyczącej poziomu usług dla usługi lub serwerów obsługujących źródła danych.
* Administrator bazy danych zna harmonogramu wykonywania kopii zapasowych dla każdej bazy danych i windows dozwolonych przetwarzanie ETL.
* Właściciela systemu zna proces użytkownikom na żądanie dostępu do źródła danych.
* Zarządcy danych wie, jak mapowanie składniki majątku i atrybuty w źródle danych do modelu danych przedsiębiorstwa.
* Analityk wie, jak dane są używane w ramach procesów biznesowych, który obsługuje on.

Każdy z tych perspektyw jest przydatne, a wykazu danych używa crowdsourcing sposobem metadanych, umożliwiająca każdy z nich do rejestrowania i zapewnia pełną obrazu zarejestrowanych źródeł danych. Za pomocą portalu wykazu danych, każdy użytkownik może Dodawanie i edytowanie własnej adnotacje będąc możliwość wyświetlania adnotacje dostarczony przez innych użytkowników.

## <a name="different-types-of-annotations"></a>Różne rodzaje adnotacji
Wykaz danych obsługuje z adnotacjami następujących typów:

| Adnotacji     | Notatki                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Przyjazną nazwę  | Przyjazne nazwy mogą być podawane w poziomie zawartości danych, aby wprowadzić trwałe danych bardziej zrozumiały. Przyjazne nazwy są najbardziej użyteczne, gdy źródłowych nazwy obiektu jest niezrozumiała, wpisz skróconą lub w przeciwnym razie nie przydatności dla użytkowników.                                                                                                                            |
| Opis    | Opisy mogą być podawane w aktywa danych i atrybut-poziomy kolumny. Opisy są adnotacje dowolnych krótki tekst, które dobrze opisują użytkownika Perspektywa trwałego danych lub jego użytkowania.                                                                                                                                                              |
| Tagi (tagi użytkownika)          | Znaczniki mogą być podawane w aktywa danych i atrybut-poziomy kolumny. Tagi użytkownika są etykiety zdefiniowane przez użytkownika, które może służyć do kategoryzowanie danych elementy zawartości lub określonych atrybutów.                                                                                                                                                                                                    |
| Tagi (tagi słownik)          | Znaczniki mogą być podawane w aktywa danych i atrybut-poziomy kolumny. Tagi słownik są centralnie zdefiniowane słownik używane kategoryzowania aktywów dane lub atrybuty za pomocą typowych taksonomii firm. Aby uzyskać więcej informacji, zobacz [jak skonfigurować słownik firm podlega znakowania](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Ekspertów        | Ekspertów mogą być podawane w poziomie zawartości danych. Ekspertów identyfikowanie użytkowników lub grup z perspektywami ekspertów na danych i może służyć jako punkty kontaktowe dla użytkowników, którzy źródeł danych zarejestrowanych i pytań, na które nie są odbierane przez istniejących adnotacji.  |
| Żądanie dostępu | Żądanie dostępu do informacji mogą być podawane w poziomie zawartości danych. Te informacje są dla użytkowników, którzy wykrywania ich jeszcze nie masz uprawnień dostępu do źródła danych. Użytkowników można wprowadzić adres e-mail użytkownika lub grupy, która udziela dostępu, adres URL proces lub narzędzia, które użytkownicy muszą dostęp, lub można wprowadzić sam proces jako tekst. |
| Dokumentacja | Dokumentacja mogą być podawane w poziomie zawartości danych. Dokumentacja zawartości jest informacje tekstu sformatowanego zawierające łączy i obrazów, a które nakładają wszelkie informacje nie są przekazywane przez opisy i znaczników. |


## <a name="annotating-multiple-assets"></a>Adnotacje wielu zasobów
Podczas wybierania wielu zasobów danych w portalu wykazu danych, użytkownicy mogą dodawać adnotacje do wszystkich wybranych środków w jednej operacji. Adnotacje zostaną zastosowane do wszystkich wybranych środków, co ułatwia wybierz i podaj opis spójne i zestawy znaczników i ekspertów trwałych powiązanych danych.

> [AZURE.NOTE] Znaczniki i ekspertów również może być udostępniana podczas rejestrowania danych składniki majątku przy użyciu danych wykazu danych źródła narzędzia rejestracji.

Gdy Zaznaczanie wielu tabel i widoków tylko kolumny, że wszystkie zaznaczone dane, które są wspólne dla składników majątku pojawi się w portalu wykazu danych. Dzięki temu użytkowników w celu zapewnienia znaczniki i opisy wszystkich kolumn o takiej samej nazwie dla wszystkich wybranych środków.

## <a name="annotations-and-discovery"></a>Adnotacje i odnajdowanie
Tak, jak metadanych wyodrębnionych ze źródła danych podczas rejestrowania zostanie dodany do indeksu wyszukiwania wykazu danych, metadane dostarczane przez użytkownika są również indeksowane. Oznacza to, że nie tylko adnotacje ułatwić użytkownikom zrozumienie dane, które były odnajdowanie, adnotacje także ułatwiają użytkownikom odnajdowanie adnotowanych danych składniki majątku wyszukiwanie przy użyciu warunki, które miały znaczenie do nich.

## <a name="summary"></a>Podsumowanie
Rejestrowanie źródła danych dzięki wykazowi danych powoduje, że odnajdowania tych danych, kopiując strukturalne i opisową metadanych ze źródła danych w usłudze wykazu. Po zarejestrowaniu źródła danych, użytkownicy mogą podać adnotacje, aby ułatwić odnajdowanie i opis z poziomu portalu wykazu danych.

## <a name="see-also"></a>Zobacz też
- [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md) samouczek krok po kroku szczegółowe informacje na temat sposobu adnotowanie źródeł danych.
