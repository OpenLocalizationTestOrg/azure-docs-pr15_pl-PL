<properties
   pageTitle="Sposób nawiązywania połączenia ze źródłami danych | Microsoft Azure"
   description="Artykule wyróżnianie sposób nawiązywania połączenia ze źródłami danych wykryte z wykazem danych Azure."
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
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Sposób nawiązywania połączenia ze źródłami danych

## <a name="introduction"></a>Wprowadzenie
**Wykaz danych Microsoft Azure** to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania dla źródła danych przedsiębiorstwa. Innymi słowy **Wykaz danych Azure** jest pomaga osobom odnajdowanie, opis i więcej korzyści z ich istniejących danych przy użyciu źródeł danych i organizacji Pomoc. Kluczowy element w tym scenariuszu korzysta z danych — po użytkownik zostanie odnaleziona źródła danych i rozumie celowi, następnym krokiem jest połączyć się ze źródłem danych, aby umieścić jej danych do użycia.

## <a name="data-source-locations"></a>Lokalizacja źródła danych
Podczas rejestrowania źródła danych **Wykazu danych Azure** otrzymuje metadanych o źródle danych. Metadane zawiera szczegółowe informacje o lokalizacji źródła danych. Szczegółowe informacje o lokalizacji różnią się ze źródła danych w źródle danych, ale zawsze będą zawierać informacji potrzebnych do połączenia. Na przykład lokalizację tabeli programu SQL Server zawiera nazwę serwera, nazwę bazy danych, nazwa schematu i nazwę tabeli, natomiast położenie raportu programu SQL Server Reporting Services zawiera nazwę serwera i ścieżkę do raportu. Inne typy źródeł danych ma lokalizacje, które odzwierciedlają strukturę i możliwości w źródłowym systemie.

## <a name="integrated-client-tools"></a>Narzędzia klienckie zintegrowane
Najprostszy sposób łączenia się ze źródłem danych jest użycie "Otwórz w...." menu w portalu **Azure wykazu danych** . W tym menu wyświetla listę opcji do łączenia się z zawartości zaznaczonych danych.
Podczas korzystania z widoku kafelków domyślnego, to menu jest dostępne na poszczególnych kafelków.

 ![Otwieranie tabeli programu SQL Server w programie Excel z danych fragmentu zawartości](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Podczas korzystania z widoku listy, menu jest dostępna w pasek wyszukiwania w górnej części okna portalu.

 ![Otwieranie raportu usług SQL Server Reporting Services w programie Report Manager z paska wyszukiwania](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Aplikacje klienckie obsługiwane
Podczas korzystania z "Otwórz w...." menu dla źródła danych w portalu Azure wykazu danych, z aplikacją kliencką poprawne musi być zainstalowany na komputerze klienckim.

| Otwieranie w aplikacji | Rozszerzenie pliku / protokół | Wersji obsługiwanych aplikacji |
| --- | --- | --- |
| Program Excel | odc | Program Excel 2010 lub nowszego |
| Program Excel (u góry 1000) | odc | Program Excel 2010 lub nowszego |
| Dodatek Power Query | xlsx | Excel 2016 lub Excel 2010 i Excel 2013 dodatek Power Query dla programu Excel zainstalowane
| Power BI Desktop | .pbix | Power BI Desktop lipca 2016 lub nowszy |
| Program SQL Server Data Tools | vsweb: / / | Visual Studio 2013 aktualizacji 4 lub nowszy z zainstalowane narzędzia programu SQL Server |
| Menedżer raportów | http:// | Zobacz [wymagania dotyczące przeglądarki dla usług SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Dane narzędziami
Opcje dostępne w menu zależy od typu danych składników majątku zaznaczonego. Oczywiście objęte nie wszystkie możliwe narzędzia "Otwórz w...." menu, ale jest nadal umożliwia łatwe łączenie się ze źródłem danych za pomocą dowolnego narzędzia klienta. Po zaznaczeniu zbiór danych w portalu **Wykazu danych Azure** pełną lokalizacji jest wyświetlana w okienku właściwości.

 ![Informacje o połączeniu dla tabeli programu SQL Server](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Szczegółowe informacje dotyczące połączenia będzie typ źródła danych różnić się zależnie od typu źródła danych, ale informacje zawarte w portalu zostaną wyświetlone wszystkie elementy, które trzeba połączyć się ze źródłem danych w dowolnym narzędziu do klienta. Użytkownicy mogą kopiować szczegóły połączenia dla źródła danych, które zostały odkryte, za pomocą **Wykazu danych Azure**, pozwalającego Praca z danymi w ich narzędzia.

## <a name="connecting-and-data-source-permissions"></a>Nawiązywanie połączenia z uprawnienia i danych źródłowych
Chociaż **Wykazu danych Azure** tworzy odnajdowania źródła danych, dostęp do danych sam pozostaje pod kontrolą danych źródłowych właściciel lub administrator. Odkrywanie źródła danych w **Wykazie danych Azure** nie daje użytkownikowi żadnych uprawnień dostępu do źródła danych, się.

Aby ułatwić użytkownikom, którzy wykrywania źródła danych, ale nie masz uprawnień dostępu do danych, użytkownicy mogą podać informacje w polu właściwości żądanie dostępu, gdy adnotacje źródła danych. Informacje przedstawione w tym łącza do procesu lub kontaktowej do uzyskania dostępu do źródła danych — są prezentowane razem z pakietem informacje o lokalizacji źródła danych w portalu.

 ![Informacje o połączeniu podanych instrukcji w żądania dostępu](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Podsumowanie
Rejestrowanie źródła danych dzięki **Wykazowi danych Azure** sprawia, że odnajdowania tych danych, kopiując strukturalne i opisową metadanych ze źródła danych w usłudze wykazu. Po źródła danych ma została zarejestrowana i wykryte, użytkowników może połączyć się ze źródłem danych z portalu **Wykazu danych Azure** "Otwórz w..." " menu lub przy użyciu narzędzi do ich danych wybranym.

## <a name="see-also"></a>Zobacz też
- [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md) samouczek krok po kroku szczegółowe informacje na temat sposobu łączenia się ze źródłami danych.
