<properties 
   pageTitle="Wyświetlanie i Zarządzanie alertami StorSimple | Microsoft Azure"
   description="W tym artykule opisano warunków alertów StorSimple i ważności, sposobu konfigurowania alertów i jak używać usługi Menedżera StorSimple do zarządzania alertami."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>Wyświetlanie i Zarządzanie alertami StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

Kartę **alerty** w usłudze Menedżer StorSimple umożliwia przeglądanie i wyczyść StorSimple urządzenia — alerty na podstawie w czasie rzeczywistym. Z poziomu tej karty można monitorować centralnie kondycją urządzenia StorSimple i ogólne rozwiązanie Microsoft Azure StorSimple.

Ten przewodnik opisuje typowych warunków alertów, poziomy ważności alertu i sposobu konfigurowania alertów. Ponadto zawiera alert podręczny wykaz tabel, które pozwalają szybko zlokalizować określonego alertu i odpowiednio odpowiadać.

![Strony alerty](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Typowe warunki alertów

Urządzenie StorSimple generuje alerty w odpowiedzi na szereg warunków. Poniżej przedstawiono najczęstsze typy alertów warunków:

- **Problemy ze sprzętem** — alerty te informują o stanie sprzętu. Pozwalają ustalić, czy aktualizacje oprogramowania są potrzebne, jeśli karty sieciowej występują problemy, czy występuje problem z jednym z dysków z danymi.

- **Problemy z łącznością** — te alerty występują w przypadku trudności z przesyłania danych. Problemów z komunikacją mogą występować podczas przekazywania danych do i z konta Azure miejsca do magazynowania lub z powodu braku łączności między urządzenia i usługi Menedżera StorSimple. Problemy dotyczące komunikacji przedstawiono kilka najtrudniejsze rozwiązać problem, ponieważ istnieje wiele punktów awarii. Zawsze najpierw należy sprawdzić połączenie sieciowe i dostęp do Internetu są dostępne przed kontynuowaniem było bardziej zaawansowane rozwiązywanie problemów. Aby uzyskać pomoc w rozwiązywaniu problemów przejdź do [Rozwiązywanie problemów z polecenia cmdlet Testuj połączenie](storsimple-troubleshoot-deployment.md).

- **Problemy z wydajnością** — te alerty są spowodowane przeprowadzając systemu nie jest optymalnie, takich jak kiedy jest przy dużym obciążeniu.

Ponadto może zostać wyświetlony alertów dotyczących zabezpieczeń, aktualizacji lub błędy zadań.

## <a name="alert-severity-levels"></a>Poziom ważności alertu

Alerty mają różne poziomy ważności, w zależności od wpływu, jaki ma sytuację alertów i potrzebę odpowiedź na wpis. Poziomy ważności są następujące:

- **Krytyczne** — ten alert znajduje się w odpowiedzi na warunku, który ma wpływ na pomyślne wydajność systemu. Aby zapewnić, że StorSimple ciągłość usługi są wymagane działania.

- **Ostrzeżenie dotyczące** — ten warunek może stać się krytyczne, gdy nie można rozpoznać. Należy Zbadaj sytuację i podejmować żadnych działań wymagane wyczyszczenie problem.

- **Informacje o** — ten alert zawiera informacje, które mogą być przydatne w śledzenia i zarządzania systemem.

## <a name="configure-alert-settings"></a>Konfigurowanie ustawień alertów

Możesz wybrać, czy chcesz otrzymywać powiadomienia pocztą e-mail alertu warunków dla każdego urządzenia StorSimple. Ponadto Adresaci alertów można określić, wprowadzając ich adresy e-mail w polu **innych adresatów wiadomości e-mail** , rozdzielając je średnikami.

>[AZURE.NOTE] Można wprowadzić maksymalnie 20 adresów e-mail na urządzeniu.

Po włączeniu powiadomień e-mail dla urządzenia, członków listy powiadomień otrzymają wiadomość e-mail z każdym alert krytyczny wystąpieniu. Wysyłane z *storsimple-alerts-noreply@mail.windowsazure.com* i opisując alert warunku. Adresaci mogą kliknij pozycję **Anuluj subskrypcję** , aby usunąć się z listy powiadomień e-mail.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Aby włączyć powiadomienia e-mail alertów dla urządzenia

1. Przejdź do pozycji **urządzenia** > **Konfigurowanie** urządzenia.

2. W obszarze **Ustawienia alertów**ustaw następujące opcje:

    1. W polu **Wyślij powiadomienie e-mail** wybierz pozycję **Tak**.

    2. W polu **Administratorzy usługi poczty E-mail** wybierz pozycję **Tak** , jeśli chcesz mieć administrator usługi i wszystkich administratorów współpracujących odbierać powiadomienia alertów.

    3. W polu **innych adresatów wiadomości e-mail** wpisz adresy e-mail wszystkich adresatów, którzy mają otrzymywać powiadomienia alertów. Wprowadź nazwy w formacie *someone@somewhere.com*. Oddzielanie adresy e-mail za pomocą średnikami. Można skonfigurować maksymalnie 20 adresów e-mail na urządzeniu. 

        ![Konfiguracja powiadomień alertów](./media/storsimple-manage-alerts/AlertNotify.png)

3. Aby wysłać powiadomienie e-mail test, kliknij ikonę strzałki obok przycisku **Wyślij wiadomość e-mail test**. Usługa Menedżera StorSimple będą wyświetlane komunikaty o stanie przekazuje powiadomienia test. 

4. Gdy zostanie wyświetlony następujący komunikat, kliknij **przycisk OK**. 

    ![Alerty testowanie wysłana wiadomość e-mail z powiadomieniem](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Jeśli nie można wysłać wiadomości z powiadomieniem o test, usługa Menedżera StorSimple wyświetli odpowiedni komunikat. Kliknij **przycisk OK**, poczekaj kilka minut i spróbuj ponownie wysłać wiadomość powiadomienie test. 

## <a name="view-and-track-alerts"></a>Wyświetlanie i śledzenie alertów

Pulpit nawigacyjny usługi Menedżera StorSimple pozwala szybko podglądać liczbę alertów na urządzeniach rozmieszczone według poziom ważności.

![Pulpit nawigacyjny alertów](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Kliknięcie przycisku poziom ważności otwarcia kartę **alerty** . Wyniki obejmują tylko alerty, które są zgodne z tym samym poziomie ważności.

![Raport alerty występujące w typ alertu](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Kliknięcie alertu na liście oferuje dodatkowe szczegóły alertu, łącznie z ostatniego Zgłoszono alert, liczba wystąpień alertu na urządzenie i zalecane działanie, aby rozwiązać alert. Jeśli jest to alert sprzętu, również określi składnika sprzętowego.

![Przykład alertu sprzętu](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Jeśli chcesz wysłać informacje do firmy Microsoft Support, możesz skopiować szczegóły alertu do pliku tekstowego. Po obserwowane zalecenia i rozpoznawania alert warunek lokalnego należy wyczyścić alert z urządzenia, wybierając kartę **alerty** alert i klikając pozycję **Usuń**. Aby wyczyścić wiele alertów, zaznacz każdy alert, kliknij wszystkich kolumn z wyjątkiem kolumny **Alert** , a po wybraniu można wyczyścić wszystkie alerty kliknij przycisk **Wyczyść** . Należy zauważyć, że niektóre alerty są automatycznie wyczyszczone, gdy problem zostanie rozwiązany lub gdy aktualizowana przez system alertu o nowe informacje.

Po kliknięciu przycisku **Wyczyść**będą mieć możliwość komentarz alertu i działania, które miały Aby rozwiązać ten problem. Niektóre zdarzenia zostanie usunięty przez system, jeśli inne zdarzenie zostanie wywołana przy użyciu nowych informacji. W takim przypadku zostanie wyświetlony poniższy komunikat.

![Komunikat alertu wyczyść pole wyboru](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sortowanie i recenzowanie alertów

Może być bardziej efektywne uruchamiać raporty na alerty, dzięki czemu będzie można przejrzeć i wyczyść je w grupach. Ponadto kartę **alerty** można wyświetlić alerty do 250. Po przekroczeniu tej liczby alertów, nie wszystkie alerty będą wyświetlane w widoku domyślnym. Można połączyć Dostosowywanie alerty są wyświetlane następujące pola:

- **Stan** — można wyświetlać alerty **aktywnej** lub **wyczyszczone** . Aktywne nadal jest alertów w systemie, gdy wyczyszczone alerty zostały albo ręcznie wyczyszczone przez administratora lub programowy wyczyszczone, ponieważ system zaktualizowane alert warunek przy użyciu nowych informacji.

- **Ważności** — można wyświetlać alerty wszystkie poziomy ważności (krytyczne, ostrzeżenie, informacje o) lub tylko niektórych ważności, takie jak tylko krytyczne alerty.

- **Źródła** — wyświetlaj alertów ze wszystkich źródeł lub ograniczyć alerty te, które pochodzą z usługi lub jedną lub wszystkie urządzenia.

- **Zakres czasu** —, określając **od** i **do** daty i sygnatury czasowe, możesz obejrzeć alerty w tym okresie, który Cię interesuje.

## <a name="alerts-quick-reference"></a>Podręczna karta informacyjna alertów

W poniższej tabeli wymieniono niektóre alerty Microsoft Azure StorSimple, które można napotkać, a także dodatkowe informacje i zalecenia w przypadku gdy dostępne. Alerty urządzenia StorSimple należą do jednego z następujących kategorii:

- [Alerty łączności chmury](#cloud-connectivity-alerts)

- [Klaster alertów](#cluster-alerts)

- [Alerty odzyskiwania po awarii](#disaster-recovery-alerts)

- [Alerty sprzętu](#hardware-alerts)

- [Alerty dotyczące błędów zadań](#job-failure-alerts)

- [Alerty lokalnie przypięty głośności](#locally-pinned-volume-alerts)

- [Alerty sieci](#networking-alerts)

- [Alerty wydajności](#performance-alerts)

- [Alerty zabezpieczeń](#security-alerts)

- [Obsługa alertów pakietu](#support-package-alerts)

- [Informacje o aktualizacjach](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Alerty łączności chmury

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Nie można nawiązać połączenie z <*Nazwa poświadczeń w chmurze*>.|Nie można połączyć z kontem miejsca do magazynowania.|Wygląda na to, być może wystąpił problem z połączeniami z urządzeniem. Uruchom `Test-HcsmConnection` polecenia cmdlet z interfejsu systemu Windows PowerShell dla StorSimple na urządzeniu, aby zidentyfikować i rozwiązać ten problem. Jeśli ustawienia są poprawne, ten problem może być przy użyciu poświadczeń konta miejsca do magazynowania, dla którego został dostarczony alert. W tym przypadku za pomocą `Test-HcsStorageAccountCredential` polecenia cmdlet, aby sprawdzić, czy są problemy, które można naprawić.<ul><li>Sprawdź ustawienia sieci.</li><li>Sprawdź swoje poświadczenia konta miejsca do magazynowania.</li></ul>|
|Firma Microsoft nie otrzymał weryfikowania na urządzeniu z systemem dla ostatniego <*numer*> min.|Nie można połączyć urządzenia.|Wygląda na to problem łączności z urządzeniem. Użyj `Test-HcsmConnection` polecenia cmdlet z interfejsu systemu Windows PowerShell dla StorSimple na urządzeniu, aby zidentyfikować i rozwiązać ten problem lub skontaktuj się z administratorem sieci.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Zachowanie StorSimple w przypadku niepowodzenia łączności chmury

Co się stanie, gdy łączność chmury kończy się niepowodzeniem, urządzenia StorSimple uruchomione w produkcji?

Jeśli łączności chmury nie powiedzie się na urządzeniu produkcji StorSimple, następnie w zależności od stanu urządzenia, następujące czynności mogą wystąpić: 

- **Dla danych lokalnie na urządzeniu**: przez pewien czas będzie nie zakłóceń i odczytuje będzie w dalszym ciągu zgłaszane. Liczba zaległych IOs zwiększa i przekracza limit, odczytywane można uruchomić kończy się niepowodzeniem. 

    W zależności od ilości danych na urządzeniu zapisywania również będzie dla pierwszych kilka godzin po przerwaniu występują w chmurze łączności. Zapisywania zostanie następnie spowolnić i ewentualnie zacznij kończy się niepowodzeniem, jeśli łączność chmurze jest zerwane przez kilka godzin. (Jest tymczasowego przechowywania na tym urządzeniu dla danych, który ma zostać przeniesiony do chmury. W tym obszarze jest opróżniany po wysłaniu danych. Jeśli połączenie nie powiedzie się, dane w tym obszarze magazynowania nie będzie zostać przeniesiony do chmury, a Jo zakończy się niepowodzeniem.)   

 
- **Dane w chmurze**: większości błędów łączności chmury, jest zwracany błąd. Po przywróceniu łączności IOs wznowienia bez konieczności uzupełnić online użytkownika. Sporadycznie w cichym może wymagać uzupełnić ponownie online z poziomu portalu klasyczny Azure. 
 
- **Na potrzeby migawek cloud w toku**: operacja zostanie wykonana ponownie kilka razy w ciągu 4-5 godzin i gdy łączność nie zostanie przywrócony, migawki chmury nie powiedzie się.


### <a name="cluster-alerts"></a>Klaster alertów

|Tekst alertu|Zdarzenia|Więcej informacji / polecane akcje|
|:---|:---|:---|
|Urządzenie nie powiodło się nad <*Nazwa urządzenia*>.|Urządzenie jest w trybie konserwacji.|Urządzenie nie powiodło się nad z powodu wprowadzania lub opuszczeniu trybu konserwacji. Jest to normalne i nie jest potrzebna. Po potwierdzenie alert, wyczyść je na stronie alerty.|
|Urządzenie nie powiodło się nad <*Nazwa urządzenia*>.|Po prostu zaktualizowano urządzenia sprzętowego lub oprogramowania.|Wystąpił trybie awaryjnym klaster z powodu aktualizacji. Jest to normalne i nie jest potrzebna. Po potwierdzenie alert, wyczyść je na stronie alerty.|
|Urządzenie nie powiodło się nad <*Nazwa urządzenia*>.|Kontroler został zamknięty lub ponownego uruchomienia.|Urządzenia nie powiodło się nad ponieważ active kontrolera został zamknięty lub ponowne uruchomienie przez administratora. Nie jest konieczne. Po potwierdzenie alert, wyczyść je na stronie alerty.|
|Urządzenie nie powiodło się nad <*Nazwa urządzenia*>.|Planowane awaryjnego.|Upewnij się, że jest to planowane trybie awaryjnym. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.|
|Urządzenie nie powiodło się nad <*Nazwa urządzenia*>.|Nieplanowanego przełączania awaryjnego.|StorSimple jest wbudowany automatyczne odzyskiwanie z niezaplanowane praca awaryjna. Jeśli zostanie wyświetlony dużej liczby tych alertów, skontaktuj się Support firmy Microsoft.|
|Urządzenie nie powiodło się nad <*Nazwa urządzenia*>.|Przyczyna inne/nieznany.|Jeśli zostanie wyświetlony dużej liczby tych alertów, skontaktuj się Support firmy Microsoft. Po problem został rozwiązany, Usuń ten alert na stronie alerty.|
|Usługi krytyczne urządzeń raportów statusu jako nie powiodło się.|Błąd usługi ścieżki danych. |Aby uzyskać pomoc, skontaktuj się z Support firmy Microsoft.|
|Wirtualny adres IP sieciowej <*danych #*> Raporty statusu jako nie powiodło się. |Przyczyna inne/nieznany. |Czasami tymczasowe warunki mogą powodować tych alertów. Jeśli jest to możliwe, następnie ten alert zostanie automatycznie wyczyszczone po pewnym czasie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft.|
|Wirtualny adres IP sieciowej <*danych #*> Raporty statusu jako nie powiodło się.|Nazwa interfejsu: <*danych #*> adres IP <IP address> nie można przełączyć online, ponieważ wykryto zduplikowany adres IP w sieci. |Upewnij się, że duplikat adresu IP został usunięty z sieci lub skonfigurowanie interfejsu przy użyciu innego adresu IP.|


### <a name="disaster-recovery-alerts"></a>Alerty odzyskiwania po awarii

|Tekst alertu|Zdarzenia|Więcej informacji / polecane akcje|
|:---|:---|:---|
|Operacje odzyskiwania nie można przywrócić wszystkie ustawienia dla tej usługi. Dane konfiguracji urządzenia są w stanie niespójności w przypadku niektórych urządzeń.|Niezgodność danych po awarii.|Zaszyfrowane dane w usłudze nie jest zsynchronizowany z którego na urządzeniu. Autoryzuj urządzenia <*Nazwa urządzenia*> za pomocą Menedżera StorSimple aby rozpocząć proces synchronizacji. Uruchamianie przy użyciu interfejsu systemu Windows PowerShell dla StorSimple `Restore-HcsmEncryptedServiceData` na urządzeniu <*Nazwa urządzenia*> polecenia cmdlet, dostarczając stare hasło jako dane wejściowe do tego polecenia cmdlet, aby przywrócić profil zabezpieczeń. Następnie uruchom `Invoke-HcsmServiceDataEncryptionKeyChange` polecenia cmdlet, aby zaktualizować klucza szyfrowania danych usługi. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.|


### <a name="hardware-alerts"></a>Alerty sprzętu

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Składnik sprzętowy <*identyfikator składnika*> raportów o stanie <*stanu*>.||Czasami tymczasowe warunki mogą powodować tych alertów. Jeśli tak, ten alert zostanie automatycznie czyszczone po pewnym czasie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft.|
|Kontroler pasywne działa poprawnie.|Pasywne kontrolera (pomocniczego) nie działa.|Urządzenie działa, ale jedną kontrolerów działa poprawnie. Spróbuj ponownie uruchomić ten kontroler. Jeśli problem nie został rozwiązany, skontaktuj się Support firmy Microsoft.|

### <a name="job-failure-alerts"></a>Alerty dotyczące błędów zadań

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Kopia zapasowa <*Identyfikator grupy głośność źródła*> nie powiodło się.|Zadania wykonywania kopii zapasowej nie powiodło się.|Problemy z łącznością może uniemożliwiać wykonywanie kopii zapasowej na ukończenie pomyślnie. W przypadku problemów z łącznością z nie osiągnięto maksymalną liczbę kopii zapasowych. Usuń wszelkie kopie zapasowe, które nie są już potrzebne i ponowić próbę. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.|
|Klonowanie <*element kopii zapasowej źródła identyfikatorów*> <*liczby kolejne głośność docelowy*> nie powiodło się.|Zadanie sklonuj nie powiodło się.|Odśwież listę kopii zapasowej, aby sprawdzić, czy wykonywanie kopii zapasowej jest nadal ważna. Jeśli kopia zapasowa jest prawidłowa, jest możliwe, że problemy z łącznością chmury uniemożliwiają pomyślne ukończenie operacji klonowanie. W przypadku problemów z łącznością z nie osiągnięto limit magazynowania. Usuń wszelkie kopie zapasowe, które nie są już potrzebne i ponowić próbę. Po miały odpowiednie kroki w celu rozwiązania problemu, wyczyść pole wyboru ten alert na stronie alerty.|
|Nie można przywrócić <*element kopii zapasowej źródła identyfikatorów*>.|Przywracanie zadania nie powiodło się.|Odśwież listę kopii zapasowej, aby sprawdzić, czy wykonywanie kopii zapasowej jest nadal ważna. Jeśli kopia zapasowa jest prawidłowa, jest możliwe, że problemy z łącznością chmury uniemożliwiają pomyślne wykonanie operacji przywracania. W przypadku problemów z łącznością z nie osiągnięto limit magazynowania. Usuń wszelkie kopie zapasowe, które nie są już potrzebne i ponowić próbę. Po miały odpowiednie kroki w celu rozwiązania problemu, wyczyść pole wyboru ten alert na stronie alerty.|

### <a name="locally-pinned-volume-alerts"></a>Alerty lokalnie przypięty głośności

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Nie można utworzyć <*Nazwa woluminu*> woluminu lokalnego.| Zadanie tworzenia woluminu nie powiodło się. <*Błędu wiadomości odpowiadające kodowi błędu nie powiodło się*>.|Problemy z łącznością może uniemożliwiać pomyślne ukończenie operacji tworzenia miejsca. Grubo zainicjowano lokalnie przypięty wielkości i proces tworzenia miejsca wiąże się przechodzeniu warstwowych wielkości w chmurze. W przypadku problemów z łącznością z nie może być wypróbowaniu wszystkich lokalnych miejsce na urządzeniu. Określ, jeśli istnieje miejsca na tym urządzeniu przed ponawianie tej operacji.|
|Lokalne woluminu <*Nazwa woluminu*> nie powiodło się.|Zadanie zmiany głośności nie powiodło się z powodu <*błędu wiadomości odpowiadające kodowi błędu nie powiodło się*>.|Problemy z łącznością może uniemożliwiać pomyślne ukończenie operacji rozszerzeń głośność. Grubo zainicjowano lokalnie przypięty wielkości i proces rozszerzania istniejący obszar obejmuje przechodzeniu warstwowych wielkości w chmurze. W przypadku problemów z łącznością z nie może być wypróbowaniu wszystkich lokalnych miejsce na urządzeniu. Określ, jeśli istnieje miejsca na tym urządzeniu przed ponawianie tej operacji.|
|Konwersja <*Nazwa woluminu*> woluminu nie powiodła się.|Zadanie konwersji głośność przekonwertować typu woluminu z lokalnie przypięta do tiered nie powiodło się.|Konwersja wielkości typu lokalnie przypięta do tiered może nie być wykonane. Upewnij się, czy nie uniemożliwia pomyślnym zakończeniu operacji problemy z łącznością. Rozwiązywanie problemów z łącznością znaleźć w [Rozwiązywanie problemów z polecenia cmdlet Test HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Oryginalny głośność lokalnie przypięty teraz został oznaczony jako warstwowych woluminu ponieważ część danych z woluminu lokalnie przypięty występują rozrzucone w chmurze podczas konwersji. Wielkość warstwowych wynikowy jest nadal zajmujące lokalne miejsca na urządzenie, którego nie można odzyskać dla przyszłych wielkości lokalny.<br>Rozwiązywanie problemów z łącznością, wyczyść alertu i konwertowanie tego woluminu to typ oryginalnego lokalnie przypięty głośność zapewnienie wszystkie dane są udostępniane lokalnie ponownie.|
|Konwersja <*Nazwa woluminu*> woluminu nie powiodła się.|Zadanie konwersji głośność przekonwertować typu głośność z tiered do lokalnie przypięte nie powiodło się.|Konwersja wielkości typu tiered do lokalnie przypięte nie można ukończyć. Upewnij się, czy nie uniemożliwia pomyślnym zakończeniu operacji problemy z łącznością. Rozwiązywanie problemów z łącznością znaleźć w [Rozwiązywanie problemów z polecenia cmdlet Test HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Oryginalny warstwowych wielkość oznaczona jako lokalnie przypięty woluminu jako część procesu konwersji w dalszym ciągu ma danych znajdujących się w chmurze, gdy grubo ustanawianie miejsca na urządzeniu dla tego woluminu już nie można odzyskać dla przyszłych lokalnej wielkości.<br>Rozwiązywanie problemów z łącznością, wyczyść alertu i konwertowanie tego woluminu to typ oryginalnego warstwowych głośność mieć pewność, że można odzyskać lokalnej przestrzeni grubo obsługi administracyjnej na urządzeniu.|
|Zbliża zużycie lokalnej przestrzeni dla lokalnego migawek <*Nazwa grupy głośności*>|Lokalne migawki zasady kopii zapasowej wkrótce może brakować miejsca i być unieważniane, aby uniknąć błędów zapisu hosta.|Częste migawek lokalne razem z pakietem pochodząca danych z dużą wielkości skojarzony z tą grupą kopii zapasowej zasad powodują lokalnej przestrzeni na tym urządzeniu szybko zużycie. Usuń wszelkie lokalne migawek, które nie są już potrzebne. Ponadto zaktualizuj planów migawkę lokalnych kopii zapasowej zasady do rzadziej migawek lokalne i upewnij się, regularnie wykonywane migawki chmury. Jeśli te akcje nie są wykonywane, lokalne miejsca do magazynowania dla migawek wkrótce może wyczerpaniu i system automatycznie usunie ich, aby upewnić się, że host zapisuje w dalszym ciągu zostanie przetworzony pomyślnie.|
|Masz zostały unieważniane lokalne migawek <*Nazwa grupy głośność*>.|Lokalne migawki <*Nazwa grupy głośność*> zostały unieważniane i usuwany, ponieważ zostały one przekraczają lokalne miejsce na urządzeniu.|Aby upewnić się, że to nie pojawia się ponownie w przyszłości, przejrzyj harmonogramów migawkę lokalnych kopii zapasowej zasady i usunąć wszelkie lokalne migawek, które nie są już potrzebne. Częste migawek lokalne razem z pakietem pochodząca danych z dużą wielkości skojarzony z tą grupą kopii zapasowej zasad może spowodować lokalnej przestrzeni na tym urządzeniu, aby szybko zużycia.|
|Nie można przywrócić <*element kopii zapasowej źródła identyfikatorów*>.|Zadanie przywracania nie powiodło się.|Jeśli masz lokalnie przypięte lub mix wielkości lokalnie przypięte i warstwowych w tych zasad kopii zapasowej odświeżanie listy kopii zapasowych w celu sprawdzenia, czy kopia zapasowa jest nadal ważna. Jeśli kopia zapasowa jest prawidłowa, jest możliwe, że problemy z łącznością chmury uniemożliwiają pomyślne wykonanie operacji przywracania. Wielkość lokalnie przypiętych, które zostały Trwa przywracanie jako część tej grupy migawki nie ma wszystkich danych pobierane na urządzeniu, a jeśli masz połączenie warstwowych i lokalnie przypięty w tej grupie migawki, nie będą w synchronizacji ze sobą. Aby pomyślnie ukończyć przywracanie, sporządzanie wielkość w tej grupie w trybie offline na hoście i spróbuj ponownie operacji przywracania. Należy zauważyć, że wszelkie zmiany w danych głośność, które były wykonywane podczas procesu przywracania zostaną utracone.|

### <a name="networking-alerts"></a>Alerty sieci

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Nie można uruchomić usługi StorSimple.|Błąd ścieżki danych |Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft.|
|Zduplikowany adres IP wykryto "Data0".| |System wykrył konflikt dla adresu IP "10.0.0.1". Zasób sieciowy "Data0" na tym urządzeniu *<device1>* jest w trybie offline. Upewnij się, że ten adres IP nie jest używany przez inny podmiot, w tym sieci. Aby rozwiązać problemy z siecią, przejdź do [Rozwiązywanie problemów z polecenia cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Aby uzyskać pomoc w rozwiązaniu tego problemu, skontaktuj się z administratorem sieci. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft. |
|Adres IP protokołu IPv4 (lub IPv6) "Data0" jest w trybie offline.| |Zasób sieciowy "Data0" z adresem IP "10.0.0.1." i długość "22" na tym urządzeniu prefiksu *<device1>* jest w trybie offline. Upewnij się, czy porty przełącznika, z którymi jest połączony ten interfejs działają. Aby rozwiązać problemy z siecią, przejdź do [Rozwiązywanie problemów z polecenia cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Alerty wydajności

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Załaduj urządzenia przekroczył <*Próg*>.|Mniejsza niż oczekiwany czas odpowiedzi.|Urządzenie raportów użycia przy dużym obciążeniu wejścia i wyjścia. Może to spowodować swojego urządzenia, aby nie działać prawidłowo powinien. Przejrzyj obciążenia podłączone do urządzenia i ustalić, czy istnieją może zostać przeniesione do innego urządzenia lub są już potrzebne.<br>Aby zrozumieć bieżący stan, przejdź do tematu [Używanie usługę Menedżer StorSimple monitorowanie urządzenia](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Alerty zabezpieczeń

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Rozpoczęcie sesji Support firmy Microsoft.|Innych firm dostęp do sesji pomocy technicznej.|Upewnij się, że ten dostępu. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.|
|Hasło dla <*element*> wygaśnie <*czas*>.|Zbliża się wygasania haseł.|Zmienianie własnego hasła, przed jej wygaśnięciem.|
|Informacje o konfiguracji zabezpieczeń brakujących <*identyfikator elementu*>.||Wielkość skojarzone z tym kontenerem głośności nie można użyć do replikacji konfiguracji StorSimple. Aby upewnić się, że dane są przechowywane bezpiecznie, zalecamy usunięcie kontenera głośności i wszelkie ilości skojarzone z tym kontenerem głośność. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.|
|<*Liczba*> prób logowania dla <*identyfikator elementu*> nie powiodło się.|Pozwala podjąć próbę wielu logowania.|Urządzenia mogą być w obszarze atakiem lub uwierzytelniony użytkownik jest próby nawiązania połączenia przy użyciu niepoprawnego hasła.<ul><li>Skontaktuj się autoryzowanych użytkowników i sprawdź, że te próby były z wiarygodnych źródła. Jeśli będzie nadal wyświetlany dużej liczby prób logowania nie powiodło się, rozważ wyłączenie zdalnego zarządzania i skontaktować się z administratorem sieci. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.</li><li>Sprawdź, czy usługi wystąpienia menedżera migawki są skonfigurowane poprawne hasło. Po miały odpowiednią akcję, Usuń ten alert na stronie alerty.</li></ul>Aby uzyskać więcej informacji przejdź do sekcji [Zmienianie hasła wygasły urządzenia](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Podczas zmiany klucza szyfrowania danych usługi wystąpił jeden lub więcej błędów.||Wystąpiły błędy podczas zmieniania klucza szyfrowania danych usługi. Po usunąć błędów, uruchom `Invoke-HcsmServiceDataEncryptionKeyChange` polecenia cmdlet z interfejsu systemu Windows PowerShell dla StorSimple na urządzeniu, aby zaktualizować usługę. Jeśli problem będzie nadal występował, skontaktuj się z pomocy technicznej firmy Microsoft. Po rozwiązać ten problem, Usuń ten alert na stronie alerty.|

### <a name="support-package-alerts"></a>Obsługa alertów pakietu

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Nie można utworzyć pakiet pomocy technicznej.|StorSimple nie może wygenerować pakietu.|Spróbuj ponownie tej operacji. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft. Po problem został rozwiązany, Usuń ten alert na stronie alerty.|

### <a name="update-alerts"></a>Informacje o aktualizacjach

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Po zainstalowaniu poprawki.|Ukończono aktualizację oprogramowania i oprogramowania układowego.|Poprawka została zainstalowana pomyślnie na urządzeniu.|
|Ręczne dostępne aktualizacje.|Powiadomienie o dostępnych aktualizacjach.|Przy użyciu interfejsu systemu Windows PowerShell StorSimple urządzenia do zainstalowania tych aktualizacji. <br>Aby uzyskać więcej informacji przejdź do [zaktualizować urządzenie serii 8000 StorSimple](storsimple-update-device.md).|
|Dostępne są nowe aktualizacje.|Powiadomienie o dostępnych aktualizacjach.|Możesz zainstalować następujące aktualizacje ze strony **konserwacji** lub przy użyciu interfejsu systemu Windows PowerShell dla StorSimple na urządzeniu. <br>Aby uzyskać więcej informacji przejdź do [zaktualizować urządzenie serii 8000 StorSimple](storsimple-update-device.md).|
|Nie można zainstalować aktualizacji.|Aktualizacje nie zostały pomyślnie zainstalowane.|System nie udało się instalowanie aktualizacji. Możesz zainstalować następujące aktualizacje ze strony **konserwacji** lub przy użyciu interfejsu systemu Windows PowerShell dla StorSimple na urządzeniu. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft. <br>Aby uzyskać więcej informacji przejdź do [zaktualizować urządzenie serii 8000 StorSimple](storsimple-update-device.md).|
|Nie można automatycznie sprawdzać dostępność nowych aktualizacji.|Automatyczne sprawdzanie nie powiodło się.|Możesz ręcznie sprawdzić dostępność nowych aktualizacji ze strony **konserwacji** .|
|Nowy agent programu Windows Update Agent jest niedostępna.|Powiadomienie o dostępnej aktualizacji.|Pobierz najnowszą wersję systemu Windows Update Agent i zainstaluj go z interfejsu programu Windows PowerShell.|
|Wersję oprogramowania układowego składnika <*identyfikator składnika*> nie jest zgodna z sprzętu.|Aktualizacje oprogramowania układowego nie zostały pomyślnie zainstalowane.|Kontakt z pomocą techniczną firmy Microsoft.|

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [StorSimple błędy i rozwiązywanie problemów z działającego urządzenia](storsimple-troubleshoot-operational-device.md).

