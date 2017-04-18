<properties
    pageTitle="Importowanie danych do wyszukiwania Azure za pomocą indeksatory w Azure Portal | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Za pomocą Kreatora importu danych wyszukiwania Azure w Azure Portal przeszukiwania danych z obiektów Blob platformy Azure miejsca do magazynowania, stroage tabeli bazy danych SQL i programu SQL Server na maszyny wirtualne Azure."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Importowanie danych do wyszukiwania Azure za pomocą portalu

Azure portal przewiduje kreatora **Importu danych** na pulpicie nawigacyjnym wyszukiwania Azure ładowania danych do indeksu. 

  ![Importowanie danych na pasku poleceń][1]

Wewnętrznie Kreator konfiguruje i wywołuje *indeksatora*kilka automatyzacji indeksowanie: 

- Nawiązywanie połączenia z zewnętrznym źródłem danych w bieżącej subskrypcji Azure
- Automatyczne tworzenie schematu indeks oparty na strukturze źródła danych
- Tworzyć dokumenty oparte na wierszy, pobrać ze źródła danych
- Przekazywanie dokumentów do indeks usługi wyszukiwania

Można wypróbować ten przepływ pracy przy użyciu przykładowych danych w DocumentDB. Aby uzyskać instrukcje, odwiedź witrynę [Wprowadzenie do wyszukiwania Azure w Azure Portal](search-get-started-portal.md) .

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Źródła danych obsługiwane przez Kreatora importu danych

Kreator importu danych obsługuje następujących źródeł danych: 

- Baza danych SQL Azure
- Program SQL Server danych relacyjnych na maszyn wirtualnych Azure
- Azure DocumentDB
- Magazyn obiektów Blob platformy Azure (w wersji preview)
- Magazyn tabel platformy Azure (w wersji preview)

Spłaszczoną zestawu danych jest wymagane dane wejściowe. Można zaimportować tylko z jednej tabeli, widoku bazy danych lub struktura odpowiednich danych. Struktura danych w tym należy utworzyć przed uruchomieniem kreatora.

Należy zauważyć, że kilka indeksatory nadal znajdują się w podglądzie, co oznacza, że definicja indeksatora jest przechowywana w wersji preview interfejsu API. Zobacz [Omówienie indeksatora](search-indexer-overview.md) , aby uzyskać więcej informacji i łącza.

## <a name="connect-to-your-data"></a>Łączenie z danymi

1. Zaloguj się do [Azure Portal](https://portal.azure.com) i otwieranie usługi pulpitu nawigacyjnego. Na pasku szybkiego dostępu do pokazania istniejących usług w bieżącej subskrypcji, możesz kliknąć **usług wyszukiwania** . 

2. Kliknij polecenie **Importuj dane** na pasku poleceń do slajdu, otwórz karta importowanie danych.  

3. Kliknij przycisk **Połącz dane** do określania definicji źródła danych używanego przez indeksatora. W przypadku źródeł danych wewnątrz subskrypcji kreatora można zazwyczaj wykrywać i przeczytaj informacje o połączeniu, minimalizując ogólne wymagania dotyczące konfiguracji.

| | |
|--------|------------|
|**Źródło danych** | Jeśli masz już indeksatory zdefiniowane w usłudze wyszukiwania, możesz wybrać istniejących definicji źródła danych do innego importu.|
|**Baza danych SQL Azure** | Na stronie lub przy użyciu parametrów połączenia ADO.NET można określić nazwę usługi, poświadczeń użytkownika bazy danych przy użyciu uprawnień odczytu i nazwę bazy danych. Wybierz opcję Parametry połączenia do wyświetlania lub dostosowywanie właściwości. <br/><br/>Należy określić tabelę lub widok, który zawiera zestaw wierszy na stronie. Ta opcja jest dostępna po pomyślnym połączenie, które zapewnia listy rozwijanej, dzięki czemu można wprowadzić zaznaczenia.|
|**Program SQL Server na Azure maszyn wirtualnych** | Określ nazwę usługi w pełni kwalifikowane, identyfikator użytkownika i hasło i bazy danych jako parametry połączenia. Aby użyć tego źródła danych, musisz wcześniej zainstalowano certyfikat w magazynie lokalnej, które są szyfrowane połączenie. <br/><br/>Należy określić tabelę lub widok, który zawiera zestaw wierszy na stronie. Ta opcja jest dostępna po pomyślnym połączenie, które zapewnia listy rozwijanej, dzięki czemu można wprowadzić zaznaczenia.
|**DocumentDB** |Wymagania dotyczące zawierać konta, bazy danych i pobieranie. Wszystkie dokumenty w zbiorze zostaną uwzględnione w indeksie. Można zdefiniować kwerendy Spłaszcz lub filtrowanie wierszy lub wykrywanie zmienione dokumenty danych kolejne operacje odświeżania.|
|**Magazyn obiektów Blob platformy Azure** | Wymagania dotyczące to konto miejsca do magazynowania i kontenera. Opcjonalnie nazw obiektów blob obserwować wirtualnych konwencji nazewnictwa na potrzeby grupowania, można określić katalog wirtualny część nazwy jako folder w kontenerze. Aby uzyskać więcej informacji, zobacz [Magazyn obiektów Blob indeksowania (wersja preview)](search-howto-indexing-azure-blob-storage.md) . |
|**Magazyn tabel platformy Azure** | Wymagania dotyczące to konto miejsca do magazynowania i nazwę tabeli. Opcjonalnie można określić kwerendę, aby pobrać podzbiór tabel. Aby uzyskać więcej informacji, zobacz [Magazyn tabel indeksowania (wersja preview)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Dostosowywanie docelowej indeksu

Indeks wstępny jest zazwyczaj wnioskowane na podstawie zestawu danych. Dodawanie, edytowanie lub usuwanie pola do wykonania w schemacie. Ponadto można ustawić atrybuty na poziomie pola do określenia jego zachowań kolejnych wyszukiwania.

1. **Dostosuj docelowej indeks**Określ nazwę i **klucz** użyty do jednoznacznie identyfikować każdy dokument. Klucz musi być ciągiem. Jeśli wartości pól zawierać spacji lub kresek Pamiętaj ustawić opcje zaawansowane w **Importowanie danych** , aby pominąć weryfikację tych znaków.

2. Przeglądanie i poprawianie pozostałe pola. Nazwa pola i typ są zwykle wypełnione automatycznie. Możesz zmienić typ danych.

3. Możesz ustawić atrybuty indeksu dla każdego pola:

 - Uwzględnianej podczas pobierania wyników zwraca pole w wynikach wyszukiwania.
 - Podatne pozwala pola odwoływać się do wyrażenia filtru.
 - Uwzględnianą w sortowaniu umożliwia pola do użycia w sortowania.
 - Facetable umożliwia pola dla nawigacji aspektowej.
 - Z możliwością wyszukiwania umożliwia wyszukiwanie pełnotekstowe.
  
4. Kliknij kartę **analizatora** , jeśli chcesz określić analizatora język, na poziomie pola. W tej chwili można określić tylko programy do analizowania języka. Za pomocą analizatora niestandardowej lub analizatora nie języka, takie jak słów kluczowych, deseniu i tak dalej, będzie wymagało kodu.

 - Kliknij pozycję **z możliwością wyszukiwania** , aby wyznaczyć przeszukiwania pełnego tekstu w polu i włączyć analizatora listy rozwijanej.
 - Wybierz pozycję analizatora, które mają. Aby uzyskać szczegółowe informacje, zobacz temat [Tworzenie indeksu dla dokumentów w wielu językach](search-language-support.md) .

5. Kliknij przycisk **Suggester** , aby włączyć sugestie dotyczące kwerend dynamiczne na wybranych pól.


## <a name="import-your-data"></a>Importowanie danych

1. **Importowanie danych**i podaj nazwę dla indeksowania. Odwoływanie się, że produkt Kreatora importu danych jest indeksatora. Później Jeśli chcesz go wyświetlać lub edytować, trzeba będzie go wybrać z portalu, a nie musisz ponownie uruchomić kreatora. 

2. Określ harmonogram, opartym na strefę czasową regionalnych, w którym jest obsługi administracyjnej usługi.

3. Ustaw opcje zaawansowane, aby określić progi na czy indeksowanie można nadal Jeśli dokument zostanie usunięte. Ponadto można określić, czy pól **kluczy** mogą zawierać spacji i ukośniki.  

## <a name="edit-an-existing-indexer"></a>Edytowanie istniejącego indeksowania

Na pulpicie nawigacyjnym usługi kliknij dwukrotnie na kafelku indeksatora slajdów się z listą wszystkich indeksatory utworzone dla subskrypcji. Kliknij dwukrotnie jeden indeksatory uruchomienia, Edytuj lub usuń go. Można zastąpić innym istniejący indeks, zmienianie źródła danych i ustawianie opcji progi błąd podczas indeksowania.

## <a name="edit-an-existing-index"></a>Edytować istniejący indeks

W polu Wyszukaj Azure strukturalnych aktualizacje indeksu wymaga odbudowanie indeksu i składa się z usuwanie indeksu, ponowne tworzenie indeksu i ponowne załadowanie danych. Strukturalne aktualizacje obejmują zmiana typu danych i zmiana nazwy lub usuwanie pola.

Zmiany, które nie wymagają przebudowanie obejmuje dodanie nowego pola, zmieniając wyników profile, zmiana suggesters lub zmiana język programy do analizowania. Aby uzyskać więcej informacji, zobacz [Aktualizowanie indeksu](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Następny krok

Przejrzyj te łącza, aby dowiedzieć się więcej o indeksatory:

- [Indeksowanie baza danych SQL Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indeksowanie DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Magazyn obiektów Blob indeksowania (wersja preview)](search-howto-indexing-azure-blob-storage.md)
- [Magazyn tabel indeksowania (wersja preview)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

