<properties
    pageTitle="Dodawanie łącznika DB2 w aplikacji logika | Microsoft Azure"
    description="Omówienie DB2 łącznik z parametrami interfejsu API usługi REST"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Wprowadzenie do łącznika DB2
Łącznik firmy Microsoft dla DB2 łączy aplikacje logiczny zasobów przechowywanych w bazie danych programu IBM DB2. Ten łącznik zawiera klienta firmy Microsoft można komunikować się z komputerów zdalnych serwera DB2 w sieci TCP/IP. Obejmuje chmury baz danych, takich jak IBM Bluemix dashDB lub IBM DB2 dla systemu Windows działa w Azure wirtualizacji i lokalnej bazy danych za pomocą bramy danych lokalnych. Zobacz [obsługiwane listy](connectors-create-api-db2.md#supported-db2-platforms-and-versions) programu IBM DB2 różnych wersji i platform (w tym temacie).

>[AZURE.NOTE] Tą wersją artykułu dotyczy aplikacji logiki ogólnodostępną (GA). 

Łącznik DB2 obsługuje następujące operacje bazy danych:

- Lista tabel bazy danych
- Odczyt przy użyciu wybierz jeden wiersz.
- Czytanie wszystkich wierszy za pomocą wybierz
- Dodawanie jednego wiersza za pomocą polecenia Wstaw
- Instrukcja ALTER za pomocą aktualizacji jeden wiersz.
- Usuwanie jednego wiersza przy użyciu DELETE

W tym temacie pokazano, jak używać łącznika w aplikacji dla logiki do procesu operacji bazy danych.

Aby dowiedzieć się więcej o aplikacjach logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Dostępne akcje
Łącznik DB2 obsługuje następujące akcje warunków logicznych w aplikacji:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- Usuwaniewierszy


## <a name="list-tables"></a>Lista tabel
Dotyczący tworzenia aplikacji logika dowolnej operacji składa się z wielu kroków wykonanych za pośrednictwem portalu Microsoft Azure.

W aplikacji logika akcja zostanie dodana do listy tabel w bazie danych DB2. Akcja powoduje, że łącznik przetwarzania instrukcję schematu DB2, takich jak `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Tworzenie aplikacji warunków logicznych
1.  **Azure rozpocząć tablicy**, wybierz **+** (znak plus), **Web + Mobile**, a następnie **Logiki aplikacji**.
2.  Wprowadź **nazwę**, na przykład `Db2getTables`, **subskrypcji**, **Grupa zasobów**, **lokalizację**i **Planowanie usługi aplikacji**. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**.

### <a name="add-a-trigger-and-action"></a>Dodawanie wyzwalacza i akcji
1.  W **Logiczny projektanta aplikacji**wybierz **Pusty LogicApp** listy **szablonów** .
2.  Na liście **wyzwalaczy** wybierz **cyklu**. 
3.  W **cyklu** wyzwalacza kliknij przycisk **Edytuj**, wybierz **Częstotliwość** listę rozwijaną zaznacz **dzień**, a następnie ustaw **Interwał** wpisywanie **7**.  
4.  Zaznacz pole wyboru **+ Nowy krok** , a następnie wybierz pozycję **Dodaj akcję**.
5.  Na liście **Akcje** wpisz `db2` w polu **Wyszukaj więcej akcji** okno do edycji, a następnie wybierz **DB2 — uzyskiwanie tabel (wersja Preview)**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  W okienku Konfiguracja **DB2 — uzyskiwanie tabel** zaznacz **pole wyboru** , aby umożliwić **Nawiązywanie połączenia za pomocą bramy danych lokalnych**. Należy zauważyć, że zmianie ustawienia z chmury do lokalnego.
    - Wpisz wartość dla **serwera**, w formie adres lub alias numer portu dwukropek. Na przykład wpisz `ibmserver01:50000`.
    - Wpisz wartość dla **bazy danych**. Na przykład wpisz `nwind`.
    - Wybierz wartość dla **uwierzytelniania**. Na przykład wybierz **podstawowy**.
    - Wpisz wartość dla **nazwy użytkownika**. Na przykład wpisz `db2admin`.
    - Wpisz wartość **hasła**. Na przykład wpisz `Password1`.
    - Wybierz wartość dla **bramy**. Na przykład zaznacz **datagateway01**.
7. Wybierz pozycję **Utwórz**, a następnie wybierz przycisk **Zapisz**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Zaznaczanie elementu w pierwszej listy (Uruchom ostatnio) w karta **Db2getTables** , na liście **Wszystkie jest uruchamiany** w obszarze **Podsumowanie**.
9.  W karta **Uruchamianie aplikacji logika** wybierz pozycję **Uruchom szczegóły**. Na liście **akcji** wybierz pozycję **Get_tables**. Zobacz wartość **stanu**powinny być **powiodło się**. Wybierz **łącze wartości wejściowych** w celu wyświetlenia danych wejściowych. Wybierz pozycję **Wyświetla łącze**i wyświetlanie wyjściowe; obejmującą listy tabel.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Tworzenie połączenia
Ten łącznik obsługuje połączenia do baz danych lokalnych i w chmurze przy użyciu następujących właściwości połączenia. 

Właściwość | Opis
--- | ---
Serwer | Wymagane. Przyjmuje wartość ciągu, która reprezentuje adresu TCP/IP lub alias w formacie IPv4 lub IPv6, a następnie (rozdzielany znakami dwukropek) numer portu TCP/IP. 
bazy danych | Wymagane. Przyjmuje wartość ciągu, która reprezentuje DRDA relacyjne bazy danych nazwa (RDBNAM). Ciąg 16-bajtowe akceptuje DB2 dla z/OS (baza danych jest znany jako IBM DB2 lokalizacji z/OS). DB2 w i5-OS akceptuje ciąg 18-bajtowa (baza danych jest zwana IBM DB2 dla i relacyjne bazy danych). DB2 dla LUW akceptuje ciąg 8-bajtowe.
uwierzytelnianie | Opcjonalnie. Akceptuje wartości elementu listy, Basic lub systemu Windows (kerberos). 
Nazwa użytkownika | Wymagane. Przyjmuje wartość ciągu. DB2 dla z/OS akceptuje ciąg 8-bajtowe. DB2 dla i akceptuje ciągu 10-bajtowa. DB2 Linux lub UNIX akceptuje ciąg 8-bajtowe. DB2 dla systemu Windows akceptuje ciągu 30-bajtowa.
hasło | Wymagane. Przyjmuje wartość ciągu.
Brama | Wymagane. Akceptuje wartości elementu listy, reprezentującą bramy danych lokalnych zdefiniowane do aplikacji logicznych w grupie miejsca do magazynowania.  

## <a name="create-the-on-premises-gateway-connection"></a>Tworzenie lokalnego połączenia bramy
Ten łącznik dostęp do bazy danych DB2 lokalnego przy użyciu bramy lokalnej. Zobacz Tematy bramy, aby uzyskać więcej informacji. 

1. W okienku konfiguracji **bramy** zaznacz **pole wyboru** , aby umożliwić **Nawiązywanie połączenia za pomocą bramy**. Należy zauważyć, że zmianie ustawienia z chmury do lokalnego.
2. Wpisz wartość dla **serwera**, w formie adres lub alias numer portu dwukropek. Na przykład wpisz `ibmserver01:50000`.
3. Wpisz wartość dla **bazy danych**. Na przykład wpisz `nwind`.
4. Wybierz wartość dla **uwierzytelniania**. Na przykład wybierz **podstawowy**.
5. Wpisz wartość dla **nazwy użytkownika**. Na przykład wpisz `db2admin`.
6. Wpisz wartość **hasła**. Na przykład wpisz `Password1`.
7. Wybierz wartość dla **bramy**. Na przykład zaznacz **datagateway01**.
8. Wybierz pozycję **Utwórz** , aby kontynuować. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Utwórz połączenie z chmurą
Ten łącznik dostęp do bazy danych w chmurze DB2. 

1. W okienku konfiguracji **bramy** pozostaw **pole wyboru** wyłączone (Niekliknięte) **Połącz przez bramy**. 
2. Wpisz wartość **Nazwa połączenia**. Na przykład wpisz `hisdemo2`.
3. Wpisz wartość **Nazwa serwera DB2**w formularzu adres lub alias numer portu dwukropek. Na przykład wpisz `hisdemo2.cloudapp.net:50000`.
3. Wpisz wartość dla **nazwy bazy danych DB2**. Na przykład wpisz `nwind`.
4. Wpisz wartość dla **nazwy użytkownika**. Na przykład wpisz `db2admin`.
5. Wpisz wartość **hasła**. Na przykład wpisz `Password1`.
6. Wybierz pozycję **Utwórz** , aby kontynuować. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Uzyskiwanie zdalnego dostępu do wszystkich wierszy za pomocą wybierz
Można zdefiniować działania aplikacji logicznych w celu pobrania wszystkich wierszy w tabeli DB2. To powoduje, że łącznik przetwarzania instrukcji DB2 SELECT, takich jak `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Tworzenie aplikacji warunków logicznych
1.  **Azure rozpocząć tablicy**, wybierz **+** (znak plus), **Web + Mobile**, a następnie **Logiki aplikacji**.
2.  Wprowadź **nazwę**, na przykład `Db2getRows`, **subskrypcji**, **Grupa zasobów**, **lokalizację**i **Planowanie usługi aplikacji**. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**.

### <a name="add-a-trigger-and-action"></a>Dodawanie wyzwalacza i akcji
1.  W **Logiczny projektanta aplikacji**wybierz **Pusty LogicApp** listy **szablonów** .
2.  Na liście **wyzwalaczy** wybierz **cyklu**. 
3.  W **cyklu** wyzwalacza kliknij przycisk **Edytuj**, wybierz **Częstotliwość** listę rozwijaną zaznacz **dzień**, a następnie wybierz **Interwał** wpisywanie **7**. 
4.  Zaznacz pole wyboru **+ Nowy krok** , a następnie wybierz pozycję **Dodaj akcję**.
5.  Na liście **Akcje** wpisz `db2` w polu **Wyszukaj więcej akcji** okno do edycji, a następnie wybierz **DB2 — uzyskiwanie wierszy (wersja Preview)**.
6. W działaniu **Uzyskiwanie wierszy (wersja Preview)** wybierz pozycję **Zmień połączenie**.
7. W okienku konfiguracji **połączenia** wybierz pozycję **Utwórz nowy**. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. W okienku konfiguracji **bramy** pozostaw **pole wyboru** wyłączone (Niekliknięte) **Połącz przez bramy**.
    - Wpisz wartość **Nazwa połączenia**. Na przykład wpisz `HISDEMO2`.
    - Wpisz wartość w polu **nazwy serwera DB2**w formularzu adres lub alias numer portu dwukropek. Na przykład wpisz `HISDEMO2.cloudapp.net:50000`.
    - Wpisz wartość dla **nazwy bazy danych DB2**. Na przykład wpisz `NWIND`.
    - Wpisz wartość dla **nazwy użytkownika**. Na przykład wpisz `db2admin`.
    - Wpisz wartość **hasła**. Na przykład wpisz `Password1`.
9. Wybierz pozycję **Utwórz** , aby kontynuować.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. Na liście **Nazwa tabeli** wybierz **strzałkę w dół**, a następnie wybierz **obszar**.
11. Opcjonalnie wybierz pozycję **Pokaż opcje zaawansowane** , aby określić opcje kwerendy.
12. Wybierz przycisk **Zapisz**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Zaznaczanie elementu w pierwszej listy (Uruchom ostatnio) w karta **Db2getRows** , na liście **Wszystkie jest uruchamiany** w obszarze **Podsumowanie**.
14. W karta **Uruchamianie aplikacji logika** wybierz pozycję **Uruchom szczegóły**. Na liście **akcji** wybierz pozycję **Get_rows**. Zobacz wartość **stanu**powinny być **powiodło się**. Wybierz **łącze wartości wejściowych** w celu wyświetlenia danych wejściowych. Wybierz pozycję **Wyświetla łącze**i wyświetlanie wyjściowe; którego powinny zawierać listę wierszy.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Dodawanie jednego wiersza za pomocą polecenia Wstaw
Można zdefiniować akcji aplikacji logiczny, aby dodać jeden wiersz w tabeli DB2. Ta akcja powoduje, że łącznik przetwarzania instrukcji DB2 INSERT, takich jak `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Tworzenie aplikacji warunków logicznych
1.  **Azure rozpocząć tablicy**, wybierz **+** (znak plus), **Web + Mobile**, a następnie **Logiki aplikacji**.
2.  Wprowadź **nazwę**, na przykład `Db2insertRow`, **subskrypcji**, **Grupa zasobów**, **lokalizację**i **Planowanie usługi aplikacji**. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**.

### <a name="add-a-trigger-and-action"></a>Dodawanie wyzwalacza i akcji
1.  W **Logiczny projektanta aplikacji**wybierz **Pusty LogicApp** listy **szablonów** .
2.  Na liście **wyzwalaczy** wybierz **cyklu**. 
3.  W **cyklu** wyzwalacza kliknij przycisk **Edytuj**, wybierz **Częstotliwość** listę rozwijaną zaznacz **dzień**, a następnie wybierz **Interwał** wpisywanie **7**. 
4.  Zaznacz pole wyboru **+ Nowy krok** , a następnie wybierz pozycję **Dodaj akcję**.
5.  Na liście **Akcje** wpisz **db2** w polu edycji **wyszukać więcej akcji** , a następnie zaznacz **DB2 - wiersza wstawiania (wersja Preview)**.
6. W działaniu **Uzyskiwanie wierszy (wersja Preview)** wybierz pozycję **Zmień połączenie**. 
7. W okienku konfiguracji **połączenia** wybierz połączenie. Na przykład zaznacz **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na liście **Nazwa tabeli** wybierz **strzałkę w dół**, a następnie wybierz **obszar**.
9. Wprowadź wartości dla wszystkich wymaganych kolumn (zobacz czerwona gwiazdka). Na przykład wpisz `99999` **AREAID**wpisz `Area 99999`i wpisz `102` dla **REGIONID**. 
10. Wybierz przycisk **Zapisz**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Zaznaczanie elementu w pierwszej listy (Uruchom ostatnio) w karta **Db2insertRow** , na liście **Wszystkie jest uruchamiany** w obszarze **Podsumowanie**.
12. W karta **Uruchamianie aplikacji logika** wybierz pozycję **Uruchom szczegóły**. Na liście **akcji** wybierz pozycję **Get_rows**. Zobacz wartość **stanu**powinny być **powiodło się**. Wybierz **łącze wartości wejściowych** w celu wyświetlenia danych wejściowych. Wybierz pozycję **Wyświetla łącze**i wyświetlanie wyjściowe; obejmującą nowy wiersz.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Zdalny dostęp za pomocą wybierz jeden wiersz.
Można zdefiniować akcję aplikacji logiki do pobierania jednego wiersza w tabeli DB2. Ta akcja powoduje, że łącznik przetwarzania instrukcję DB2 wybierz miejsce, w którym, takich jak `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Tworzenie aplikacji warunków logicznych
1.  **Azure rozpocząć tablicy**, wybierz **+** (znak plus), **Web + Mobile**, a następnie **Logiki aplikacji**.
2.  Wprowadź **nazwę** (np. "**Db2getRow**"), **subskrypcji**, **Grupa zasobów**, **lokalizację**i **Planowanie usługi aplikacji**. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**.

### <a name="add-a-trigger-and-action"></a>Dodawanie wyzwalacza i akcji
1.  W **Logiczny projektanta aplikacji**wybierz **Pusty LogicApp** listy **szablonów** . 
2.  Na liście **wyzwalaczy** wybierz **cyklu**. 
3.  W **cyklu** wyzwalacza kliknij przycisk **Edytuj**, wybierz **Częstotliwość** listę rozwijaną zaznacz **dzień**, a następnie wybierz **Interwał** wpisywanie **7**. 
4.  Zaznacz pole wyboru **+ Nowy krok** , a następnie wybierz pozycję **Dodaj akcję**.
5.  Na liście **Akcje** wpisz **db2** w polu edycji **wyszukać więcej akcji** , a następnie wybierz pozycję **DB2 — uzyskiwanie wierszy (wersja Preview)**.
6. W działaniu **Uzyskiwanie wierszy (wersja Preview)** wybierz pozycję **Zmień połączenie**. 
7. W okienku konfiguracji **połączenia** wybierz istniejące połączenie. Na przykład zaznacz **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na liście **Nazwa tabeli** wybierz **strzałkę w dół**, a następnie wybierz **obszar**.
9. Wprowadź wartości dla wszystkich wymaganych kolumn (zobacz czerwona gwiazdka). Na przykład wpisz `99999` dla **AREAID**. 
10. Opcjonalnie wybierz pozycję **Pokaż opcje zaawansowane** , aby określić opcje kwerendy.
11. Wybierz przycisk **Zapisz**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Zaznaczanie elementu w pierwszej listy (Uruchom ostatnio) w karta **Db2getRow** , na liście **Wszystkie jest uruchamiany** w obszarze **Podsumowanie**.
13. W karta **Uruchamianie aplikacji logika** wybierz pozycję **Uruchom szczegóły**. Na liście **akcji** wybierz pozycję **Get_rows**. Zobacz wartość **stanu**powinny być **powiodło się**. Wybierz **łącze wartości wejściowych** w celu wyświetlenia danych wejściowych. Wybierz pozycję **Wyświetla łącze**i wyświetlanie wyjściowe; które powinno obejmować wiersza.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Zmienianie jeden wiersz, za pomocą aktualizacji
Można zdefiniować akcji aplikacji logiczny, aby zmienić jednego wiersza w tabeli DB2. Ta akcja powoduje, że łącznik przetwarzania instrukcja DB2 UPDATE, takich jak `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Tworzenie aplikacji warunków logicznych
1.  **Azure rozpocząć tablicy**, wybierz **+** (znak plus), **Web + Mobile**, a następnie **Logiki aplikacji**.
2.  Wprowadź **nazwę**, na przykład `Db2updateRow`, **subskrypcji**, **Grupa zasobów**, **lokalizację**i **Planowanie usługi aplikacji**. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**.

### <a name="add-a-trigger-and-action"></a>Dodawanie wyzwalacza i akcji
1.  W **Logiczny projektanta aplikacji**wybierz **Pusty LogicApp** listy **szablonów** .
2.  Na liście **wyzwalaczy** wybierz **cyklu**. 
3.  W **cyklu** wyzwalacza kliknij przycisk **Edytuj**, wybierz **Częstotliwość** listę rozwijaną zaznacz **dzień**, a następnie wybierz **Interwał** wpisywanie **7**. 
4.  Zaznacz pole wyboru **+ Nowy krok** , a następnie wybierz pozycję **Dodaj akcję**.
5.  Na liście **Akcje** wpisz **db2** w polu edycji **wyszukać więcej akcji** , a następnie zaznacz **DB2 — aktualizacja wiersza (wersja Preview)**.
6. W działaniu **Uzyskiwanie wierszy (wersja Preview)** wybierz pozycję **Zmień połączenie**. 
7. W okienku konfiguracji **połączenia** wybierz Zaznacz istniejącego połączenia. Na przykład zaznacz **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na liście **Nazwa tabeli** wybierz **strzałkę w dół**, a następnie wybierz **obszar**.
9. Wprowadź wartości dla wszystkich wymaganych kolumn (zobacz czerwona gwiazdka). Na przykład wpisz `99999` **AREAID**wpisz `Updated 99999`i wpisz `102` dla **REGIONID**. 
10. Wybierz przycisk **Zapisz**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Zaznaczanie elementu w pierwszej listy (Uruchom ostatnio) w karta **Db2updateRow** , na liście **Wszystkie jest uruchamiany** w obszarze **Podsumowanie**.
12. W karta **Uruchamianie aplikacji logika** wybierz pozycję **Uruchom szczegóły**. Na liście **akcji** wybierz pozycję **Get_rows**. Zobacz wartość **stanu**powinny być **powiodło się**. Wybierz **łącze wartości wejściowych** w celu wyświetlenia danych wejściowych. Wybierz pozycję **Wyświetla łącze**i wyświetlanie wyjściowe; obejmującą nowy wiersz.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Usuwanie jednego wiersza przy użyciu DELETE
Można zdefiniować akcję logiki aplikacji, aby usunąć jeden wiersz w tabeli DB2. Ta akcja powoduje, że łącznik przetwarzania instrukcji usunąć DB2, takich jak `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Tworzenie aplikacji warunków logicznych
1.  **Azure rozpocząć tablicy**, wybierz **+** (znak plus), **Web + Mobile**, a następnie **Logiki aplikacji**.
2.  Wprowadź **nazwę**, na przykład `Db2deleteRow`, **subskrypcji**, **Grupa zasobów**, **lokalizację**i **Planowanie usługi aplikacji**. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**.

### <a name="add-a-trigger-and-action"></a>Dodawanie wyzwalacza i akcji
1.  W **Logiczny projektanta aplikacji**wybierz **Pusty LogicApp** listy **szablonów** . 
2.  Na liście **wyzwalaczy** wybierz **cyklu**. 
3.  W **cyklu** wyzwalacza kliknij przycisk **Edytuj**, wybierz **Częstotliwość** listę rozwijaną zaznacz **dzień**, a następnie wybierz **Interwał** wpisywanie **7**. 
4.  Zaznacz pole wyboru **+ Nowy krok** , a następnie wybierz pozycję **Dodaj akcję**.
5.  Na liście **Akcje** zaznacz **db2** w polu edycji **wyszukać więcej akcji** , a następnie wybierz **DB2 — usuwanie wiersza (wersja Preview)**.
6. W działaniu **Uzyskiwanie wierszy (wersja Preview)** wybierz pozycję **Zmień połączenie**. 
7. W okienku konfiguracji **połączenia** wybierz istniejące połączenie. Na przykład zaznacz **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na liście **Nazwa tabeli** wybierz **strzałkę w dół**, a następnie wybierz pozycję **obszar**.
9. Wprowadź wartości dla kolumn wszystkie wymagane (zobacz czerwona gwiazdka). Na przykład wpisz `99999` dla **AREAID**. 
10. Wybierz przycisk **Zapisz**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Zaznaczanie elementu w pierwszej listy (Uruchom ostatnio) w karta **Db2deleteRow** , na liście **Wszystkie jest uruchamiany** w obszarze **Podsumowanie**.
12. W karta **Uruchamianie aplikacji logika** wybierz pozycję **Uruchom szczegóły**. Na liście **akcji** wybierz pozycję **Get_rows**. Zobacz wartość **stanu**powinny być **powiodło się**. Wybierz **łącze wartości wejściowych** w celu wyświetlenia danych wejściowych. Wybierz pozycję **Wyświetla łącze**i wyświetlanie wyjściowe; obejmującą usuniętego wiersza.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Szczegóły techniczne

## <a name="actions"></a>Akcje
Akcja jest czynność wykonaną przez przepływ pracy zdefiniowane w aplikacji logicznych. Łącznik bazy danych DB2 obejmuje następujące czynności. 

|Akcja|Opis|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Pobiera pojedynczy wiersz z tabeli DB2|
|[GetRows](connectors-create-api-db2.md#get-rows)|Pobiera wiersze z tabeli DB2|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Wstawia nowy wiersz w tabeli DB2|
|[Usuwaniewierszy](connectors-create-api-db2.md#delete-row)|Usuwa wiersza z tabeli DB2|
|[GetTables](connectors-create-api-db2.md#get-tables)|Pobiera tabel z bazy danych DB2|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Aktualizowanie istniejącego wiersza w tabeli DB2|

### <a name="action-details"></a>Szczegóły akcji

W tej sekcji Zobacz szczegółowe informacje na temat każdej akcji, w tym wszystkie wymagane lub opcjonalne właściwości wprowadzania i dowolne odpowiednie dane wyjściowe skojarzone z łącznik.

#### <a name="get-row"></a>Uzyskiwanie wierszy 
Pobiera pojedynczy wiersz z tabeli DB2.  

| Nazwa właściwości| Nazwa wyświetlana |Opis|
| ---|---|---|
|Tabela * | Nazwa tabeli |Nazwa tabeli DB2|
|Identyfikator * | Identyfikator wiersza |Unikatowy identyfikator wiersza do pobrania|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
Element

| Nazwa właściwości | Typ danych |
|---|---|
|ItemInternalId|ciąg|


#### <a name="get-rows"></a>Uzyskiwanie wierszy 
Pobiera wiersze z tabeli DB2.  

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Tabela *|Nazwa tabeli|Nazwa tabeli DB2|
|$skip|Pomiń zliczanie|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Uzyskiwanie maksymalna liczba|Maksymalna liczba wpisów do pobierania (domyślny = 256)|
|$filter|Filtrowanie kwerendy|Kwerenda filtru ODATA, aby ograniczyć liczbę wpisów|
|$orderby|Uporządkuj według|Kwerenda orderBy ODATA do określania kolejności pozycji|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
ItemsList

| Nazwa właściwości | Typ danych |
|---|---|
|wartość|Tablica|


#### <a name="insert-row"></a>Wstaw wiersz 
Wstawia nowy wiersz w tabeli DB2.  

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Tabela *|Nazwa tabeli|Nazwa tabeli DB2|
|element *|Wiersz|Wiersz, aby wstawić do określonej tabeli w DB2|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
Element

| Nazwa właściwości | Typ danych |
|---|---|
|ItemInternalId|ciąg|


#### <a name="delete-row"></a>Usuwanie wiersza 
Usuwa wiersza z tabeli DB2.  

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Tabela *|Nazwa tabeli|Nazwa tabeli DB2|
|Identyfikator *|Identyfikator wiersza|Unikatowy identyfikator wiersza do usunięcia|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
Brak.

#### <a name="get-tables"></a>Pobieranie tabel 
Pobiera tabel z bazy danych DB2.  

Nie ma żadnych parametrów dla tego połączenia. 

##### <a name="output-details"></a>Szczegóły wyników 
TablesList

| Nazwa właściwości | Typ danych |
|---|---|
|wartość|Tablica|

#### <a name="update-row"></a>Aktualizowanie wierszy 
Aktualizacje istniejących wierszy w tabeli DB2.  

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Tabela *|Nazwa tabeli|Nazwa tabeli DB2|
|Identyfikator *|Identyfikator wiersza|Unikatowy identyfikator wiersza do aktualizacji|
|element *|Wiersz|Wiersz z zaktualizowane wartości|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników  
Element

| Nazwa właściwości | Typ danych |
|---|---|
|ItemInternalId|ciąg|


### <a name="http-responses"></a>Odpowiedzi HTTP

Podczas połączenia z innych działań, może zostać wyświetlony określone odpowiedzi. W poniższej tabeli przedstawiono odpowiedzi i ich opisy:  

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="supported-db2-platforms-and-versions"></a>Obsługiwane platformy DB2 i wersji
Ten łącznik obsługuje następujących platformach IBM DB2 i wersjami, jak również produkty zgodne IBM DB2 (np. dashDB IBM Bluemix), które obsługują SQL Menedżera programu Access (SQLAM) Distributed relacyjne bazy danych architektura DRDA () w wersji 10 i 11:

- IBM DB2 w z/OS 11.1
- IBM DB2 w z/OS 10.1
- IBM DB2 dla i 7.3
- IBM DB2 dla i 7.2
- IBM DB2 dla i 7.1
- IBM DB2 LUW 11
- IBM DB2 dla LUW 10.5


## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Poznaj dostępne łączniki w aplikacjach logiki w naszej [listy interfejsów API](apis-list.md).

