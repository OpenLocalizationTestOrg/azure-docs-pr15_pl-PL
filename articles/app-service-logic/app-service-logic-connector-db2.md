<properties
   pageTitle="Łącznik DB2 przy użyciu programu Microsoft Azure aplikacji usługi | Microsoft Azure"
   description="Jak używać łącznik DB2 z logiki aplikacji wyzwalacze i akcje"
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

# <a name="db2-connector"></a>Łącznik DB2
>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych.

Łącznik firmy Microsoft dla DB2 to aplikacja interfejsu API nawiązywania połączenia aplikacje za pośrednictwem usługi aplikacji Azure zasobów przechowywanych w bazie danych programu IBM DB2. Łącznik zawiera Client Microsoft nawiązywanie połączenia z komputerami serwerów DB2 zdalnego przez połączenie sieciowe TCP/IP, w tym połączeń Azure hybrydowych do lokalnych serwerów DB2 przy użyciu przekazywania Bus usługi Azure łącznik obsługuje następujące operacje bazy danych:

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


## <a name="create-the-db2-connector"></a>Tworzenie łącznika DB2
Łącznik aplikacji logika lub z usługi Azure Marketplace można zdefiniować jak w poniższym przykładzie:  

1. W startboard Azure wybierz pozycję **Marketplace**.
2. W karta **wszystko** wpisz **db2** w polu **Wyszukaj wszystko** , a następnie kliknij klawisz enter.
3. W polu Wyszukaj w okienku wszystko wyników, zaznacz **Łącznik DB2**.
4. W karta opis łącznik DB2 wybierz pozycję **Utwórz**.
5. W karta pakiet łącznik DB2 wprowadź nazwę (np. "Db2ConnectorNewOrders"), planowanie usługi aplikacji i inne właściwości.
6. Wybierz **Ustawienia pakietu**, a następnie wprowadź następujące ustawienia pakietu:  

    Nazwa | Wymagane |  Opis
--- | --- | ---
ConnectionString | Tak | Parametry połączenia DB2 klienta (przykład "adres sieciowy = nazwa_serwera; Port sieciowy = 50000; Identyfikator użytkownika = nazwa użytkownika; Hasło = hasła; początkowego wykazu = próbki; Pakowanie zbioru = NWIND; domyślne schematu = NWIND ").
Tabele | Tak | Lista rozdzielanych przecinkami tabeli, widoku i alias nazwy wymagane dla operacji OData, a także do generowania dokumentacji swagger wraz z przykładami (np. "*NEWORDERS*").
Procedury | Tak | Lista rozdzielanych przecinkami nazw procedury i funkcji (np. "SPORDERID").
OnPremise | Brak | Wdrażanie przy użyciu przekazywania Bus usługi Azure lokalnego.
ServiceBusConnectionString | Brak | Parametry połączenia przekazywania Bus usługi Azure.
PollToCheckData | Brak | Wybierz pozycję Statystyka instrukcji, aby za pomocą wyzwalacza aplikacji logiczny (np. "wybierz pozycję Statystyka (\*) z NEWORDERS gdzie DATAWYSYŁKI IS NULL").
PollToReadData | Brak | Instrukcji SELECT do użytku z wyzwalacza aplikacji logiczny (np. "Wybierz \* z NEWORDERS miejsce, w którym DATAWYSYŁKI jest aktualizacja dla wartości NULL").
PollToAlterData | Brak | AKTUALIZACJI lub instrukcja DELETE do użytku z wyzwalacza aplikacji logiczny (np. "DATAWYSYŁKI Ustawianie NEWORDERS aktualizacji = BIEŻĄCEJ daty WHERE CURRENT OF &lt;kursor&gt;").

7. Wybierz **przycisk OK**, a następnie wybierz pozycję **Utwórz**.
8. Po zakończeniu ustawienia pakietu wyglądać podobnie do następującej:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logika aplikacji z akcją łącznik DB2, aby dodać dane ##
Można zdefiniować akcję aplikacji logika dodać dane do tabeli DB2 za pomocą interfejsu API Insert lub opublikuj operacji OData jednostki. Na przykład można wstawić nowego rekordu zamówienia klientów, przetwarzania instrukcję SQL INSERT w odniesieniu do tabeli zdefiniowanej z kolumną tożsamości zwracanie wartości tożsamości lub zmieniono do aplikacji logiczny (Wybierz ORDID z KOŃCOWĄ tabelę (WSTAWIĆ do NWIND. WARTOŚCI NEWORDERS (IDKLIENTA NAZWAODBIORCY, SHIPADDR, MIASTOODBIORCY, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

> [AZURE.TIP] Połączenia DB2 "*Opublikuj, aby zestawu*" zwraca wartości kolumny tożsamości i zwraca "*Wstawianie interfejsu API*" dotyczy wierszy

1. Azure startboard, wybierz **+** (znak plus), **Web + Mobile**, a następnie **logiczny aplikacji**.
2. Wprowadź nazwę (np. "NewOrdersDb2"), planowanie usługi aplikacji, inne właściwości, a następnie wybierz pozycję **Utwórz**.
3. W startboard Azure wybierz logiki aplikacji, którą właśnie utworzony, **Ustawienia**, a następnie **Wyzwalacze i akcje**.
4. W karta wyzwalacze i Akcje wybierz **Tworzenie od podstaw** w aplikacji logika szablonów.
5. W panelu aplikacji interfejsu API wybierz **cyklu**, ustawić częstotliwość i interwału, a następnie **znacznik wyboru**.
6. W panelu aplikacji interfejsu API wybierz **Łącznik DB2**, rozwiń listę czynności, aby wybrać, **Wstawianie do NEWORDER**.
7. Rozwiń listę parametrów do wprowadź następujące wartości:  

    Nazwa | Wartość
--- | --- 
IDKLIENTA | 10042
NAZWAODBIORCY | Magazyn Kountry opóźnieniem K 
SHIPADDR | Tarasowej orkiestry 12
MIASTOODBIORCY | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Zaznacz **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie **Zapisz**.
9. Ustawienia powinna wyglądać następująco:  
![][3]

10. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**zaznacz element w pierwszej listy (Uruchom najnowszych). 
11. W karta **Uruchamianie aplikacji logiczny** zaznacz element **akcji** **db2connectorneworders**.
12. W karta **logiki aplikacji akcji** wybierz **Łącze danych wejściowych**. Łącznik DB2 używa danych wejściowych przetwarzania parametryczne instrukcję.
13. W karta **logiki aplikacji akcji** wybierz **LINK wyjściowe**. Danych wejściowych powinna wyglądać następująco:  
![][4]

#### <a name="what-you-need-to-know"></a>Co należy wiedzieć

- Łącznik obcina nazwy tabel DB2 po tworzące nazwy akcji aplikacji logicznych. Na przykład operacja **wstawiony NEWORDERS** jest obcinana wstawić **do NEWORDER**.
- Po zapisaniu aplikacji logika **Wyzwalacze i akcje**, aplikacja logiczny przetwarza tej operacji. Może wystąpić opóźnienie liczby sekund (np. 3 do 5 sekundach) przed logiki aplikacji przetwarza operację. Opcjonalnie można kliknąć **Uruchom teraz** podczas wykonywania operacji.
- Łącznik DB2 określa zestawu członkom atrybutów, na przykład zarówno składnik odpowiada kolumnie DB2 z domyślną lub wygenerowane kolumn (na przykład tożsamość). Logika aplikacji jest wyświetlana czerwona gwiazdka obok nazwy identyfikator zestawu członka, aby oznaczyć DB2 kolumn, które wymagają wartości. Nie należy wprowadzać wartości dla grupy ORDID, która odpowiada kolumnie tożsamości DB2. Możesz wprowadzić wartości dla innych uczestników opcjonalne (elementów, ORDDATE, REQDATE, SHIPID, FRACHT, SHIPCTRY), które odpowiadają kolumnom DB2 z wartościami domyślnymi. 
- Łącznik DB2 zwraca do aplikacji logika odpowiedź na wpis do zestawu zawierającego wartości dla kolumny tożsamości pochodzącej z SQLDARD DRDA (SQL obszar Odpowiedz danych) na gotowa instrukcja SQL INSERT. Serwer DB2 nie zwraca wstawione wartości dla kolumn z wartościami domyślnymi.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logika aplikacji z akcją łącznik DB2, aby dodać dane zbiorcze ##
Można zdefiniować akcję aplikacji logika dodać dane do tabeli DB2 za pomocą operacji Wstawianie zbiorcze interfejsu API. Na przykład można wstawić dwa nowe rekordy zamówienia klientów, przetwarzania za pomocą tablicy wartości wiersza tabeli zdefiniowanej zawierającej kolumnę identity instrukcję SQL INSERT zwracanie zmieniono do aplikacji logiczny (Wybierz ORDID z KOŃCOWĄ tabelę (WSTAWIĆ do NWIND. WARTOŚCI NEWORDERS (IDKLIENTA NAZWAODBIORCY, SHIPADDR, MIASTOODBIORCY, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

1. Azure startboard, wybierz **+** (znak plus), **Web + Mobile**, a następnie **logiczny aplikacji**.
2. Wprowadź nazwę (np. "NewOrdersBulkDb2"), planowanie usługi aplikacji, inne właściwości, a następnie wybierz pozycję **Utwórz**.
3. W startboard Azure wybierz logiki aplikacji, którą właśnie utworzony, **Ustawienia**, a następnie **Wyzwalacze i akcje**.
4. W karta wyzwalacze i Akcje wybierz **Tworzenie od podstaw** w aplikacji logika szablonów.
5. W panelu aplikacji interfejsu API wybierz **cyklu**, ustawić częstotliwość i interwału, a następnie **znacznik wyboru**.
6. W panelu aplikacji interfejsu API wybierz **Łącznik DB2**, rozwiń z listy operacji wybierz **Zbiorcze wstawiony nowy**.
7. Wprowadź wartość **wierszy** w postaci tablicy. Na przykład skopiuj i wklej następujący ciąg:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Zaznacz **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie **Zapisz**. Ustawienia powinna wyglądać następująco:  
![][6]

9. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**kliknij element w pierwszej listy (Uruchom najnowszych).
10. Karta **Uruchamianie aplikacji logiki** kliknij element **akcji** .
11. Karta **logiki aplikacji Akcja** kliknij **Łącze danych wejściowych**. Wydajność powinna wyglądać następująco:  
[][7]
12. Karta **logiki aplikacji Akcja** kliknij **Łącze wyjściowe**. Wydajność powinna wyglądać następująco:  
![][8]

#### <a name="what-you-need-to-know"></a>Co należy wiedzieć

- Łącznik obcina nazwy tabel DB2 podczas tworzące nazwy akcji aplikacji logicznych. Na przykład operacji **Zbiorczej wstawiony NEWORDERS** jest obcinana **zbiorcze**wstawić do nowej.
- Baza danych DB2 generowane przez pomijania kolumny tożsamości (np. ORDID), wartości Null kolumny (na przykład DATAWYSYŁKI) i kolumny z wartościami domyślnymi (ORDDATE, REQDATE, SHIPID, FRACHT, SHIPCTRY), wartości.
- Określając "dzisiaj" i "jutro", łącznik DB2 generuje "BIEŻĄCĄ datę" i "BIEŻĄCEJ daty + 1 dzień" funkcje (na przykład REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logika aplikacji z DB2 wyzwalacza łącznik do odczytu, zmienić lub usunąć dane ##
Można zdefiniować wyzwalacz aplikacji logika ankieta i odczytywanie danych z tabeli DB2 za pomocą operacji złożonych danych ankiety interfejsu API. Na przykład, przeczytaj lub więcej nowej kolejności rekordów klientów, zwraca rekordy do aplikacji logicznych. Ustawienia połączenia DB2 pakietu i aplikacji powinna wyglądać następująco:

    Ustawienia aplikacji | Wartość
--- | --- | ---
PollToCheckData | Wybierz pozycję Statystyka (\*) z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL
PollToReadData | Wybierz pozycję \* z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL aktualizacji
PollToAlterData | <no value specified>


Ponadto można zdefiniować wyzwalacza aplikacji logika ankieta, przeczytaj i zmiany danych w tabeli DB2 przy użyciu operacji złożonych danych ankiety interfejsu API. Na przykład, przeczytaj lub aktualizowanie więcej nowych rekordów zamówienia klientów z wartościami wiersza zwracanie wybranych (przed aktualizacją) rekordów do aplikacji logicznych. Ustawienia połączenia DB2 pakietu i aplikacji powinna wyglądać następująco:

    Ustawienia aplikacji | Wartość
--- | --- | ---
PollToCheckData | Wybierz pozycję Statystyka (\*) z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL
PollToReadData | Wybierz pozycję \* z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL aktualizacji
PollToAlterData | Aktualizacja NEWORDERS zestaw DATAWYSYŁKI = bieżąca data miejsce, w którym z bieżącym &lt;kursora&gt;


Ponadto można zdefiniować wyzwalacza aplikacji logika ankieta, przeczytaj i usuwanie danych z tabeli DB2 za pomocą operacji złożonych danych ankiety interfejsu API. Na przykład można przeczytać jedną lub więcej nowych rekordów zamówienia odbiorcy Usuń wiersze, zwraca rekordy zaznaczonego (przed usunięciem) do aplikacji logicznych. Ustawienia połączenia DB2 pakietu i aplikacji powinna wyglądać następująco:

    Ustawienia aplikacji | Wartość
--- | --- | ---
PollToCheckData | Wybierz pozycję Statystyka (\*) z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL
PollToReadData | Wybierz pozycję \* z NEWORDERS miejsce, w którym DATAWYSYŁKI ma wartość NULL aktualizacji
PollToAlterData | Usuń NEWORDERS miejsce, w którym z bieżącym &lt;kursora&gt;

W tym przykładzie logiki aplikacji będzie ankieta, odczytywanie, aktualizowanie, a następnie ponownie przeczytaj dane w tabeli DB2.

1. Azure startboard, wybierz **+** (znak plus), **Web + Mobile**, a następnie **logiczny aplikacji**.
2. Wprowadź nazwę (np. "ShipOrdersDb2"), planowanie usługi aplikacji, inne właściwości, a następnie wybierz pozycję **Utwórz**.
3. W startboard Azure wybierz logiki aplikacji, którą właśnie utworzony, **Ustawienia**, a następnie **Wyzwalacze i akcje**.
4. W karta wyzwalacze i Akcje wybierz **Tworzenie od podstaw** w aplikacji logika szablonów.
5. W panelu aplikacji interfejsu API wybierz **Łącznik DB2**, ustawianie częstotliwości i interwału, a następnie **znacznik wyboru**.
6. W panelu aplikacji interfejsu API wybierz **Łącznik DB2**, rozwiń listę czynności, aby wybrać, **Wybierz pozycję z NEWORDERS**.
7. Zaznacz **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie **Zapisz**. Ustawienia powinna wyglądać następująco:  
![][10]  
8. Kliknij, aby zamknąć karta **Wyzwalacze i akcje** , a następnie kliknij, aby zamknąć karta **Ustawienia** .
9. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**kliknij element w pierwszej listy (Uruchom najnowszych).
10. Karta **Uruchamianie aplikacji logiki** kliknij element **akcji** .
11. Karta **logiki aplikacji Akcja** kliknij **Łącze wyjściowe**. Wydajność powinna wyglądać następująco:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logika aplikacji z akcją łącznik DB2, aby usunąć dane ##
Można zdefiniować akcji aplikacji logiczny, aby usunąć dane z tabeli DB2 za pomocą interfejsu API Delete lub opublikuj operacji OData jednostki. Na przykład można wstawić nowego rekordu zamówienia klientów, przetwarzania instrukcję SQL INSERT w odniesieniu do tabeli zdefiniowanej z kolumną identyfikacji zwracanie wartości tożsamości lub zmieniono do aplikacji logiczny (Wybierz ORDID z KOŃCOWĄ tabelę (WSTAWIĆ do NWIND. WARTOŚCI NEWORDERS (IDKLIENTA NAZWAODBIORCY, SHIPADDR, MIASTOODBIORCY, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Tworzenie aplikacji logika usuwanie danych za pomocą łączników DB2 ##
Czy utworzenia nowej aplikacji logika z poziomu usługi Azure Marketplace, a następnie użyj łącznika DB2 jako akcji, aby usunąć zamówień klientów. Na przykład umożliwia DB2 łącznik warunkowe operację usuwania procesu instrukcję SQL DELETE (Usuń z NEWORDERS miejsce, w którym ORDID > = 10000).

1. W menu Centrum Azure **rozpocząć** tablicy kliknij **+** (znak plus), kliknij pozycję **Web + Mobile**, a następnie kliknij **logiczny aplikacji**. 
2. W karta **Tworzenie logiki aplikacji** wpisz **nazwę**, na przykład **RemoveOrdersDb2**.
3. Wybierz lub zdefiniuj wartości dla innych ustawień (plan usług, grupa zasobów).
4. Ustawienia powinna wyglądać w następujący sposób. Kliknij przycisk **Utwórz**:  
![][12]  
5. W karta **Ustawienia** kliknij **Wyzwalacze i akcje**.
6. W karta **Wyzwalacze i akcje** na liście **aplikacji logika szablony** kliknij pozycję **Utwórz od podstaw**.
7. W karta **Wyzwalacze i akcje** w panelu **Aplikacji interfejsu API** w grupie zasobów kliknij przycisk **Cykl**.
8. Na powierzchni projektowej logiki aplikacji kliknij element, **Cykl** , **Częstotliwość** i **interwału**, na przykład **dni** i **1**, a następnie kliknij **znacznik wyboru** , aby zapisać ustawienia elementu cyklu.
9. W karta **Wyzwalacze i akcje** w panelu **Aplikacji interfejsu API** należący do grupy zasobów kliknij **Łącznik DB2**.
10. Na powierzchni projektowej logiki aplikacji kliknij element akcji **Łącznik DB2** , kliknij wielokropek (****...), aby rozwinąć listę operacji, a następnie kliknij **warunkowe usunąć z N**.
11. Na DB2 łącznik czynności do wykonania wpisz **ORDID stronę 10000** **wyrażenie identyfikująca podzbiorem pozycji**.
12. Kliknij **znacznik wyboru** , aby zapisać ustawienia akcji, a następnie kliknij przycisk **Zapisz**. Ustawienia powinna wyglądać następująco:  
![][13]  
13. Kliknij, aby zamknąć karta **Wyzwalacze i akcje** , a następnie kliknij, aby zamknąć karta **Ustawienia** .
14. Na liście **Wszystkie jest uruchamiany** w obszarze **operacje**kliknij element w pierwszej listy (Uruchom najnowszych).
15. Karta **Uruchamianie aplikacji logiki** kliknij element **akcji** .
16. Karta **logiki aplikacji Akcja** kliknij **Łącze wyjściowe**. Wydajność powinna wyglądać następująco:  
![][14]

**Uwaga:** Projektant aplikacji logiki obcina nazwy tabeli. Na przykład operację **usunięcia warunkowe z NEWORDERS** jest obcinana do **usunięcia warunkowe z N**.


> [AZURE.TIP] Za pomocą następujących instrukcji SQL podczas tworzenia przykładowej tabeli i procedur składowanych. 

Przykładowa tabela NEWORDERS za pomocą poniższych stwierdzeń DB2 SQL DDL można utworzyć:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Można tworzyć za pomocą następujących instrukcji DB2 DDL procedura SPOERID przechowywane próbki:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Konfigurowanie środowiska hybrydowego (opcjonalnie)

> [AZURE.NOTE] Ten krok jest wymagany tylko wtedy, gdy używasz DB2 łącznik lokalnego za zaporą.

Usługa aplikacji używa Menedżer konfiguracji hybrydowych do bezpieczne łączenie się z systemem lokalnego. Jeśli łącznik używa lokalnego IBM DB2 serwera dla systemu Windows, wymagane jest hybrydowych Connection Manager.

Zobacz [Korzystanie z Menedżera połączeń hybrydowych](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Więcej możliwości korzystania z tego łącznika
Teraz, gdy łącznik jest tworzony, możesz dodać go do przepływu pracy firm przy użyciu aplikacji logicznych. Zobacz [Co to są aplikacje logika?](app-service-logic-what-are-logic-apps.md).

Tworzenie aplikacji interfejsu API za pomocą interfejsów API pozostałych. Zobacz [łączników i interfejsu API aplikacje odwołania](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Można także przeglądać wydajności statystyki i kontroli zabezpieczeń do łącznika. Zobacz [Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

