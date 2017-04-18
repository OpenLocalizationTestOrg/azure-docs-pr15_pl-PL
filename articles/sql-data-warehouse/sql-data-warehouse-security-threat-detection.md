<properties
   pageTitle="Wprowadzenie do wykrywania zagrożenie magazynu danych SQL"
   description="Jak rozpocząć pracę z wykrywaniem zagrożenie"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Wprowadzenie do wykrywania zagrożenie

> [AZURE.SELECTOR]
- [Inspekcja](sql-data-warehouse-auditing-overview.md)
- [Wykrywanie zagrożenie](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Omówienie

Wykrywanie zagrożenie wykrywa działania anomalous bazy danych, wskazująca potencjalnych zagrożeniach do bazy danych. Wykrywanie zagrożenie znajduje się w wersji preview i jest obsługiwane dla magazynu danych SQL.

Wykrywanie zagrożenie zawiera nową warstwę zabezpieczeń, które umożliwia klientom wykrywanie i odpowiadanie na potencjalne zagrożenia występujące, dostarczając alertów zabezpieczeń anomalous działalności. Użytkowników można eksplorować podejrzane zdarzenia ustalenie, jeśli są one wynikiem próba dostęp, naruszenia lub wykorzystać dane w magazynie danych przy użyciu [Inspekcji magazynu danych SQL Azure](sql-data-warehouse-auditing-overview.md) .
Wykrywanie zagrożenie upraszcza na adres potencjalne zagrożenia do magazynu danych bez konieczności zabezpieczenia ekspertów lub zarządzać zabezpieczeniami zaawansowanymi systemów monitorowania.

Na przykład wykrywania zagrożenie wykrywa niektórych działań anomalous bazy danych, wskazująca potencjalne prób uruchomienie SQL. Uruchomienie SQL jest jednym z typowych problemów zabezpieczeń aplikacji sieci Web w Internecie, umożliwia ataki aplikacji opartych na danych. Ataki korzystać z aplikacji luk do dodania złośliwy instrukcji SQL w aplikacji pól naruszenie lub modyfikowania danych w bazie danych.


## <a name="set-up-threat-detection-for-your-database"></a>Konfigurowanie wykrywania zagrożenie dla bazy danych

1. Uruchamianie portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).

2. Przejdź do karta konfiguracji magazynu danych SQL, który chcesz monitorować. W karta Ustawienia wybierz pozycję **Inspekcja i wykrywania zagrożenie**.

    ![Okienko nawigacji][1]

3. W grupie **Inspekcja i wykrywania zagrożenie** karta konfiguracji Włączanie **na** inspekcji, co powoduje wyświetlenie ustawień wykrywania zagrożenie.

    ![Okienko nawigacji][2]

4. Włączanie wykrywania zagrożenie **Wł** .

5. Konfigurowanie listy wiadomości e-mail, które będą otrzymywać alerty zabezpieczeń w przypadku wykrycia czynności magazynowych anomalous danych.

6. Kliknij przycisk **Zapisz** w karta konfiguracji **wykrywania Inspekcja i zagrożenie** zapisać nową lub zaktualizowaną inspekcji i zagrożenie wykrywania zasad.

    ![Okienko nawigacji][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Eksplorowanie czynności magazynowych anomalous danych w przypadku wykrycia podejrzane zdarzenia

1. Otrzymasz powiadomienie e-mail w przypadku wykrycia działań anomalous bazy danych. <br/>
Wiadomość e-mail zawiera informacje dotyczące zdarzeń podejrzanych zabezpieczeń, w tym charakter anomalous działania, nazwę bazy danych, nazwa serwera i czas zdarzenia. Ponadto zawiera informacje dotyczące możliwych przyczyn i zalecane akcje badanie i łagodzenia zagrożenie do bazy danych.<br/>

    ![Okienko nawigacji][4]

2. W wiadomości e-mail kliknij łącze **Dziennik inspekcji SQL Azure** , uruchamianie Portal klasyczny Azure i wyświetlanie odpowiednich rekordów inspekcja pora podejrzane zdarzenia.

    ![Okienko nawigacji][5]

3. Kliknij rekordy inspekcji, aby wyświetlić więcej szczegółów na działania podejrzanych bazy danych, takich jak instrukcji SQL, adresów IP klienta i przyczyny awarii.

    ![Okienko nawigacji][6]

4. W karta Inspekcja rekordów, kliknij przycisk **Otwórz w programie Excel** , aby otworzyć wstępnie skonfigurowanego szablonu importowanie i uruchomić szczegółowego analizowania dziennik inspekcji pora podejrzanych zdarzeń w programie excel.<br/>
**Uwaga:** W programie Excel 2010 lub nowszy dodatek Power Query i ustawienie **Szybkie łączenie** jest wymagane

    ![Okienko nawigacji][7]

5. Aby skonfigurować ustawienie **Szybkie łączenie** — na karcie wstążki **POWER QUERY** , wybierz pozycję **Opcje** , aby wyświetlić okno dialogowe Opcje. Zaznacz sekcję prywatności i wybierz drugą opcję - "Ignoruj poziomy prywatności i potencjalnie zwiększenie wydajności":

    ![Okienko nawigacji][8]

6. Aby załadować dzienników inspekcji SQL, upewnij się, że parametry w obszarze Ustawienia karty są poprawnie skonfigurowane i wybierz na wstążce "Dane" i kliknij przycisk "Odśwież wszystko".

    ![Okienko nawigacji][9]

7. Wyniki są wyświetlane w arkuszu **Dzienników inspekcji SQL** , który umożliwia uruchamianie szczegółowego analizowania anomalous działania, które zostaną wykryte i ograniczanie wpływu zdarzeniem bezpieczeństwa w aplikacji.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
