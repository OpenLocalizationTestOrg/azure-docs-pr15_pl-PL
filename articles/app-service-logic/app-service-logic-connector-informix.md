<properties
   pageTitle="Łącznik programu Informix przy użyciu programu Microsoft Azure aplikacji usługi | Microsoft Azure"
   description="Jak używać programu Informix łącznik z logiki aplikacji wyzwalacze i akcje"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="informix-connector"></a>Łącznik programu Informix
>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych.

Łącznik firmy Microsoft dla programu Informix jest aplikacji interfejsu API nawiązywania połączenia aplikacje za pośrednictwem usługi aplikacji Azure zasobów przechowywanych w bazie danych programu IBM Informix. Łącznik zawiera Client Microsoft nawiązywanie połączenia z komputerami serwerów programu Informix zdalnego przez połączenie sieciowe TCP/IP, łącznie z połączeniami Azure hybrydowych do lokalnych serwerów programu Informix przy użyciu przekazywania Bus usługi Azure. Łącznik obsługuje następujące operacje bazy danych:

- Zaznacz wiersze odczytu, używając
- Ankiety czytanie wierszy przy użyciu wybierz pozycję Liczba, a następnie wybierz pozycję
- Dodawanie wiersza jednej lub wielu wierszy (zbiorcze) za pomocą polecenia Wstaw
- Zmiany w jednym wierszu lub w wielu wierszach (zbiorcze) za pomocą aktualizacji
- Usuwanie jednego wiersza lub wiele wierszy (zbiorcze) przy użyciu DELETE
- Odczyt, zmiana wierszy przy użyciu kursora wybierz następuje aktualizacja miejsce, w którym bieżącego z KURSOREM
- Przeczytane w celu usunięcia wierszy przy użyciu kursora wybierz następuje aktualizacja miejsce, w którym bieżącego z KURSOREM
- Uruchomić procedurę parametry wejściowe i wyjściowe, zwraca wartość, zestaw wyników, za pomocą połączenia
- Polecenia niestandardowe i złożone operacji przy użyciu wybierz pozycję Wstaw, AKTUALIZUJ, Usuń

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje
Łącznik obsługuje wyzwalaczy aplikacji logika następujące akcje:

Wyzwalaczy | Akcje
--- | ---
<ul><li>Dane ankiety</li></ul> | <ul><li>Wstawianie zbiorcze</li><li>Wstawianie</li><li>Aktualizacjach zbiorczych</li><li>Aktualizacja</li><li>Nawiązywanie połączenia</li><li>Usuwanie zbiorcze</li><li>Usuwanie</li><li>Wybierz pozycję</li><li>Aktualizacja warunkowe</li><li>Opublikuj, aby zestawu</li><li>Usuń warunkowe</li><li>Zaznacz jedną osobę</li><li>Usuwanie</li><li>UPSERT do zestawu</li><li>Polecenia niestandardowe</li><li>Operacje złożonego</li></ul>


## <a name="create-the-informix-connector"></a>Tworzenie łącznika programu Informix
Łącznik aplikacji logika lub z usługi Azure Marketplace można zdefiniować jak w poniższym przykładzie:  

1. W startboard Azure wybierz pozycję **Marketplace**.
2. W karta **wszystko** wpisz **programu informix** w polu **Wyszukaj wszystko** , a następnie kliknij klawisz enter.
3. W polu Wyszukaj w okienku wszystko wyników, zaznacz **Łącznik programu Informix**.
4. W karta opis łącznika programu Informix wybierz pozycję **Utwórz**.
5. W karta pakiet łącznik programu Informix wprowadź nazwę (np. "InformixConnectorNewOrders"), planowanie usługi aplikacji i inne właściwości.
6. Wybierz **Ustawienia pakietu**, a następnie wprowadź następujące ustawienia pakietu.

    Nazwa | Wymagane |  Opis
--- | --- | ---
ConnectionString | Tak | Parametry połączenia klienta programu Informix (przykład "adres sieciowy = nazwa_serwera; Port sieciowy = 9089; Identyfikator użytkownika = nazwa użytkownika; Hasło = hasła; początkowego wykazu = nwind; domyślne schematu programu informix = ").
Tabele | Tak | Lista rozdzielanych przecinkami tabeli, widoku i alias nazwy wymagane dla operacji OData, a także do generowania dokumentacji swagger wraz z przykładami (np. "NEWORDERS").
Procedury | Tak | Lista rozdzielanych przecinkami nazw procedury i funkcji (np. "SPORDERID").
OnPremise | Brak | Wdrażanie przy użyciu przekazywania Bus usługi Azure lokalnego.
ServiceBusConnectionString | Brak | Parametry połączenia przekazywania Bus usługi Azure.
PollToCheckData | Brak | Wybierz pozycję Statystyka instrukcji, aby za pomocą wyzwalacza aplikacji logiczny (np. "wybierz pozycję Statystyka (\*) z NEWORDERS gdzie DATAWYSYŁKI IS NULL").
PollToReadData | Brak | Instrukcji SELECT do użytku z wyzwalacza aplikacji logiczny (np. "Wybierz \* z NEWORDERS miejsce, w którym DATAWYSYŁKI jest aktualizacja dla wartości NULL").
PollToAlterData | Brak | AKTUALIZACJI lub instrukcja DELETE do użytku z wyzwalacza aplikacji logiczny (np. "DATAWYSYŁKI Ustawianie NEWORDERS aktualizacji = BIEŻĄCEJ daty WHERE CURRENT OF &lt;kursor&gt;").

7. Wybierz **przycisk OK**, a następnie wybierz pozycję **Utwórz**.
8. Po zakończeniu ustawienia pakietu wyglądać podobnie do następującej:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Logika aplikacji z akcją łącznik programu Informix, aby dodać dane ##
Można zdefiniować akcji aplikacji logiki, aby dodać dane do tabeli programu Informix za pomocą interfejsu API Insert lub opublikuj operacji OData jednostki. Na przykład można wstawić nowego rekordu zamówienia klientów, przetwarzania instrukcję SQL INSERT w odniesieniu do tabeli zdefiniowanej z kolumną tożsamości zwracanie wartości tożsamości lub zmieniono do aplikacji logiczny (Wybierz ORDID z KOŃCOWĄ tabelę (wartości WSTAWIĆ do NEWORDERS (IDKLIENTA NAZWAODBIORCY, SHIPADDR, MIASTOODBIORCY, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

> [AZURE.TIP] Połączenie programu Informix "*Opublikuj, aby zestawu*" zwraca wartości kolumny tożsamości i zwraca "*Wstawianie interfejsu API*" dotyczy wierszy

1. Azure startboard, wybierz **+** (znak plus), **Web + Mobile**, a następnie **logiczny aplikacji**.
2. Wprowadź nazwę (np. "NewOrdersInformix"), planowanie usługi aplikacji, inne właściwości, a następnie wybierz pozycję **Utwórz**.
3. W startboard Azure wybierz logiki aplikacji, którą właśnie utworzony, **Ustawienia**, a następnie **Wyzwalacze i akcje**.
4. W karta wyzwalacze i Akcje wybierz **Tworzenie od podstaw** w aplikacji logika szablonów.
5. W panelu aplikacji interfejsu API wybierz **cyklu**, ustawić częstotliwość i interwału, a następnie **znacznik wyboru**.
6. W panelu aplikacji interfejsu API wybierz **Łącznik programu Informix**, rozwiń listę czynności, aby wybrać, **Wstawianie do NEWORDER**.
7. Rozwiń listę parametrów do wprowadź następujące wartości:  

    Nazwa | Wartość
--- | --- 
IDKLIENTA | 10042
SHIPID | 10000
NAZWAODBIORCY | Magazyn Kountry opóźnieniem K 
SHIPADDR | Tarasowej orkiestry 12
MIASTOODBIORCY | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Zaznacz **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie **Zapisz**.
9. Ustawienia powinna wyglądać następująco:  
![][3]  
10. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**zaznacz element w pierwszej listy (Uruchom najnowszych). 
11. W karta **Uruchamianie aplikacji logiczny** zaznacz element **akcji** **informixconnectorneworders**.
12. W karta **logiki aplikacji akcji** wybierz **Łącze danych wejściowych**. Łącznik programu Informix używa danych wejściowych przetwarzania parametryczne instrukcję.
13. W karta **logiki aplikacji akcji** wybierz **LINK wyjściowe**. Danych wejściowych powinna wyglądać następująco:  
![][4]

#### <a name="what-you-need-to-know"></a>Co należy wiedzieć

- Łącznik obcina nazwy tabel programu Informix po tworzące nazwy akcji aplikacji logicznych. Na przykład operacja **wstawiony NEWORDERS** jest obcinana wstawić **do NEWORDER**.
- Po zapisaniu aplikacji logika **Wyzwalacze i akcje**, aplikacja logiczny przetwarza tej operacji. Może wystąpić opóźnienie liczby sekund (np. 3 do 5 sekundach) przed logiki aplikacji przetwarza operację. Opcjonalnie można kliknąć **Uruchom teraz** podczas wykonywania operacji.
- Łącznik programu Informix określa członków zestawu z atrybutami, na przykład zarówno składnik odpowiada kolumnie Informix z domyślną lub wygenerowane kolumn (na przykład tożsamość). Logika aplikacji jest wyświetlana czerwona gwiazdka obok nazwy identyfikator zestawu członka, do oznaczania Informix kolumn, które wymagają wartości. Nie należy wprowadzać wartość dla elementu ORDID, odnoszącą się do kolumny tożsamości programu Informix. Możesz wprowadzić wartości dla innych uczestników opcjonalne (elementów, ORDDATE, REQDATE, SHIPID, FRACHT, SHIPCTRY), które odpowiadają kolumnom programu Informix z wartościami domyślnymi. 
- Łącznik programu Informix zwraca do aplikacji logika odpowiedź na wpis do zestawu zawierającego wartości dla kolumny tożsamości pochodzącej z SQLDARD DRDA (SQL obszar odpowiedź danych) na gotowa instrukcja SQL INSERT. Serwer programu Informix nie zwraca wstawionego wartości dla kolumn z wartościami domyślnymi.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Logika aplikacji z akcją łącznik programu Informix dodać dane zbiorcze ##
Można zdefiniować akcję aplikacji logika dodać dane do tabeli programu Informix przy użyciu operacji Wstawianie zbiorcze interfejsu API. Na przykład można wstawić dwa nowe rekordy zamówienia klientów, przetwarzania za pomocą tablicy wartości wiersza tabeli zdefiniowanej zawierającej kolumnę identity instrukcję SQL INSERT zwracanie zmieniono do aplikacji logiczny (Wybierz ORDID z KOŃCOWĄ tabelę (wartości WSTAWIĆ do NEWORDERS (IDKLIENTA NAZWAODBIORCY, SHIPADDR, MIASTOODBIORCY, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

1. Azure startboard, wybierz **+** (znak plus), **Web + Mobile**, a następnie **logiczny aplikacji**.
2. Wprowadź nazwę (np. "NewOrdersBulkInformix"), planowanie usługi aplikacji, inne właściwości, a następnie wybierz pozycję **Utwórz**.
3. W startboard Azure wybierz logiki aplikacji, którą właśnie utworzony, **Ustawienia**, a następnie **Wyzwalacze i akcje**.
4. W karta wyzwalacze i Akcje wybierz **Tworzenie od podstaw** w aplikacji logika szablonów.
5. W panelu aplikacji interfejsu API wybierz **cyklu**, ustawić częstotliwość i interwału, a następnie **znacznik wyboru**.
6. W panelu aplikacji interfejsu API wybierz **Łącznik programu Informix**, rozwiń z listy operacji wybierz **Zbiorcze wstawiony nowy**.
7. Wprowadź wartość **wierszy** w postaci tablicy. Na przykład skopiuj i wklej następujący ciąg:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
    ```
        
8. Zaznacz **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie **Zapisz**. Ustawienia powinna wyglądać następująco:  
![][6]

9. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**kliknij element w pierwszej listy (Uruchom najnowszych).
10. Karta **Uruchamianie aplikacji logika** kliknij element **akcji** .
11. Karta **logiki aplikacji Akcja** kliknij **Łącze danych wejściowych**. Wydajność powinna wyglądać następująco:  
[][7]
12. Karta **logiki aplikacji akcji** kliknij **Łącze wyjściowe**. Wydajność powinna wyglądać następująco:  
![][8]

#### <a name="what-you-need-to-know"></a>Co należy wiedzieć

- Łącznik obcina nazwy tabel programu Informix po tworzące nazwy akcji aplikacji logicznych. Na przykład operacji **Zbiorczej wstawiony NEWORDERS** jest obcinana **zbiorcze**wstawić do nowej.
- Baza danych programu Informix może być uwzględniana wielkość liter, nazwy tabel i kolumn. Na przykład nazwy kolumn tablicy operacji Wstawianie zbiorcze może być konieczne należy określić rozpoczynających się małą literą ("IDKlienta") zamiast wielkich liter ("IDKLIENTA").
- Baza danych programu Informix generowane przez pomijania kolumny tożsamości (np. ORDID), wartości Null kolumny (na przykład DATAWYSYŁKI) i kolumny z wartościami domyślnymi (ORDDATE, REQDATE, SHIPID, FRACHT, SHIPCTRY), wartości.
- Określając "dzisiaj" i "jutro", łącznik programu Informix generuje "BIEŻĄCĄ datę" i "BIEŻĄCEJ daty + 1 dzień" funkcje (na przykład REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Logika aplikacji przy użyciu programu Informix wyzwalacza łącznik do odczytu, zmienić lub usunąć dane ##
Można zdefiniować wyzwalacz aplikacji logika ankieta i odczytywanie danych z tabeli programu Informix przy użyciu operacji złożonych danych ankiety interfejsu API. Na przykład, przeczytaj lub więcej nowej kolejności rekordów klientów, zwraca rekordy do aplikacji logicznych. Ustawienia połączenia programu Informix pakietu i aplikacji powinna wyglądać następująco:

    Ustawienia aplikacji | Wartość
--- | --- | ---
PollToCheckData | Wybierz pozycję Statystyka (\*) z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL
PollToReadData | Wybierz pozycję \* z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL aktualizacji
PollToAlterData | <no value specified>


Ponadto można zdefiniować wyzwalacza aplikacji logiki ankieta, przeczytaj i zmiany danych w tabeli programu Informix przy użyciu operacji złożonych danych ankiety interfejsu API. Na przykład, przeczytaj lub aktualizowanie więcej nowych rekordów zamówienia klientów z wartościami wiersza zwracanie wybranych (przed aktualizacją) rekordów do aplikacji logicznych. Ustawienia połączenia programu Informix pakietu i aplikacji powinna wyglądać następująco:

    Ustawienia aplikacji | Wartość
--- | --- | ---
PollToCheckData | Wybierz pozycję Statystyka (\*) z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL
PollToReadData | Wybierz pozycję \* z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL aktualizacji
PollToAlterData | Aktualizacja NEWORDERS zestaw DATAWYSYŁKI = bieżąca data miejsce, w którym z bieżącym &lt;kursora&gt;


Ponadto można zdefiniować wyzwalacza aplikacji logika ankieta, przeczytaj i usuwanie danych z tabeli programu Informix przy użyciu operacji złożonych danych ankiety interfejsu API. Na przykład można przeczytać jedną lub więcej nowych rekordów zamówienia klientów Usuń wiersze, zwraca rekordy zaznaczonego (przed usunięciem) do aplikacji logicznych. Ustawienia połączenia programu Informix pakietu i aplikacji powinna wyglądać następująco:

    Ustawienia aplikacji | Wartość
--- | --- | ---
PollToCheckData | Wybierz pozycję Statystyka (\*) z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL
PollToReadData | Wybierz pozycję \* z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL aktualizacji
PollToAlterData | Usuń NEWORDERS miejsce, w którym z bieżącym &lt;kursora&gt;

W tym przykładzie logiki aplikacji będzie ankieta, odczytywanie, aktualizowanie, a następnie ponownie przeczytaj dane w tabeli programu Informix.

1. Azure startboard, wybierz **+** (znak plus), **Web + Mobile**, a następnie **logiczny aplikacji**.
2. Wprowadź nazwę (np. "ShipOrdersInformix"), planowanie usługi aplikacji, inne właściwości, a następnie wybierz pozycję **Utwórz**.
3. W startboard Azure wybierz logiki aplikacji, którą właśnie utworzony, **Ustawienia**, a następnie **Wyzwalacze i akcje**.
4. W karta wyzwalacze i Akcje wybierz **Tworzenie od podstaw** w aplikacji logika szablonów.
5. W panelu aplikacji interfejsu API zaznacz **Łącznik Informix**, ustawić częstotliwość i interwału, a następnie **Zaznacz pole**.
6. W panelu aplikacji interfejsu API wybierz **Łącznik Informix**, rozwiń listę czynności, aby wybrać, **Wybierz pozycję z NEWORDERS**.
7. Zaznacz **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie **Zapisz**. Ustawienia powinna wyglądać następująco:  
![][10]
8. Kliknij, aby zamknąć karta **Wyzwalacze i akcje** , a następnie kliknij, aby zamknąć karta **Ustawienia** .
9. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**kliknij element w pierwszej listy (Uruchom najnowszych).
10. Karta **Uruchamianie aplikacji logika** kliknij element **akcji** .
11. Karta **logiki aplikacji akcji** kliknij **Łącze wyjściowe**. Wydajność powinna wyglądać następująco:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Logika aplikacji z akcją łącznik programu Informix, aby usunąć dane ##
Można zdefiniować akcji aplikacji logiczny, aby usunąć dane z tabeli programu Informix za pomocą interfejsu API Delete lub opublikuj operacji OData jednostki. Na przykład można wstawić nowego rekordu zamówienia klientów, przetwarzania instrukcję SQL INSERT w odniesieniu do tabeli zdefiniowanej z kolumną tożsamości zwracanie wartości tożsamości lub zmieniono do aplikacji logiczny (Wybierz ORDID z KOŃCOWĄ tabelę (wartości WSTAWIĆ do NEWORDERS (IDKLIENTA NAZWAODBIORCY, SHIPADDR, MIASTOODBIORCY, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Tworzenie aplikacji logiki usuwanie danych przy użyciu łącznika programu Informix ##
Czy utworzenia nowej aplikacji logika z poziomu usługi Azure Marketplace, a następnie użyj łącznika programu Informix jako akcji, aby usunąć zamówień klientów. Na przykład, można użyć programu Informix łącznik warunkowe operację usuwania przetwarzania instrukcję SQL DELETE (Usuń z NEWORDERS miejsce, w którym ORDID > = 10000).

1. W menu Centrum Azure **rozpocząć** tablicy kliknij **+** (znak plus), kliknij pozycję **Web + Mobile**, a następnie kliknij **logiczny aplikacji**. 
2. W karta **Tworzenie logiki aplikacji** wpisz **nazwę**, na przykład **RemoveOrdersInformix**.
3. Wybierz lub zdefiniuj wartości dla innych ustawień (plan usług, grupa zasobów).
4. Ustawienia powinna wyglądać w następujący sposób. Kliknij przycisk **Utwórz**:  
![][12]
5. W karta **Ustawienia** kliknij **Wyzwalacze i akcje**.
6. W karta **Wyzwalacze i akcje** na liście **aplikacji logika szablony** kliknij pozycję **Utwórz od podstaw**.
7. W karta **Wyzwalacze i akcje** w panelu **Aplikacji interfejsu API** w grupie zasobów kliknij przycisk **Cykl**.
8. Na powierzchni projektowej logiki aplikacji kliknij element, **Cykl** , **Częstotliwość** i **interwału**, na przykład **dni** i **1**, a następnie kliknij **znacznik wyboru** , aby zapisać ustawienia elementu cyklu.
9. W karta **Wyzwalacze i akcje** w panelu **Aplikacji interfejsu API** w grupie zasobów kliknij **Łącznik programu Informix**.
10. Na powierzchni projektowej logiki aplikacji kliknij element akcji **Łącznik programu Informix** , kliknij wielokropek (****...), aby rozwinąć listę operacji, a następnie kliknij **warunkowe usunąć z N**.
11. Na programu Informix łącznik czynności do wykonania, wpisz **ordid stronę 10000** **wyrażenie identyfikująca podzbiorem pozycji**.
12. Kliknij **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie kliknij przycisk **Zapisz**. Ustawienia powinna wyglądać następująco:  
![][13]
13. Kliknij, aby zamknąć karta **Wyzwalacze i akcje** , a następnie kliknij, aby zamknąć karta **Ustawienia** .
14. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**kliknij element w pierwszej listy (Uruchom najnowszych).
15. Karta **Uruchamianie aplikacji logiki** kliknij element **akcji** .
16. Karta **logiki aplikacji Akcja** kliknij **Łącze wyjściowe**. Wydajność powinna wyglądać następująco:  
![][14]

**Uwaga:** Projektant aplikacji logiki obcina nazwy tabeli. Na przykład operację **usunięcia warunkowe z NEWORDERS** jest obcinana **warunkowe**usuwanie z N.


> [AZURE.TIP] Za pomocą następujących instrukcji SQL podczas tworzenia przykładowej tabeli i procedur składowanych. 

Możesz utworzyć przykładowej tabeli NEWORDERS za pomocą poniższych stwierdzeń Informix SQL DDL:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


Można tworzyć za pomocą następujących instrukcji DDL programu Informix procedura SPORDERID przechowywane próbki:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Konfigurowanie środowiska hybrydowego (opcjonalnie)

> [AZURE.NOTE] Ten krok jest wymagany tylko wtedy, gdy używasz DB2 łącznik lokalnego za zaporą.

Usługa aplikacji używa Menedżer konfiguracji hybrydowych do bezpieczne łączenie się z systemem lokalnego. Jeśli łącznik używa lokalnego IBM DB2 serwera dla systemu Windows, wymagane jest hybrydowych Connection Manager.

Zobacz [Korzystanie z Menedżera połączeń hybrydowych](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Więcej możliwości korzystania z tego łącznika
Teraz, gdy łącznik jest tworzony, możesz dodać go do przepływu pracy firm przy użyciu aplikacji logicznych. Zobacz [Co to są aplikacje logika?](app-service-logic-what-are-logic-apps.md).

Tworzenie aplikacji interfejsu API za pomocą interfejsów API pozostałych. Zobacz [łączników i interfejsu API aplikacje odwołania](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Można także przeglądać wydajności statystyki i kontroli zabezpieczeń do łącznika. Zobacz [Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


