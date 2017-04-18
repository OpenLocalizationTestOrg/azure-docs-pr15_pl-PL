<properties 
    pageTitle="Omówienie X12 i pakiet integracji przedsiębiorstwa | Microsoft Azure aplikacji usługi | Microsoft Azure" 
    description="Dowiedz się, jak używać X12 umów do tworzenia aplikacji warunków logicznych" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Integracja przedsiębiorstwa z X12 

>[AZURE.NOTE]Ta funkcja obejmuje X12 strony logiczny aplikacji. Aby uzyskać informacje na EDIFACT kliknij [tutaj](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Tworzenie X12 umowy 
Aby można było wymieniać X12 wiadomości, należy utworzyć X12 umowy i zapisać go na swoim koncie integracji. Poniższe kroki przeprowadzi Cię przez proces tworzenia X12 umowy.

### <a name="heres-what-you-need-before-you-get-started"></a>Poniżej opisano, co jest potrzebne przed rozpoczęciem
- [Integracja konta](./app-service-logic-enterprise-integration-accounts.md) zdefiniowanego w ramach subskrypcji Azure  
- Co najmniej dwa [partnerów](./app-service-logic-enterprise-integration-partners.md) już określonych na koncie integracji  

>[AZURE.NOTE]Podczas tworzenia umowę, zawartości w pliku umowy musi odpowiadać typ umowy.    


Po wprowadzeniu [utworzone konto integracji](./app-service-logic-enterprise-integration-accounts.md) i [dodane partnerów](./app-service-logic-enterprise-integration-partners.md), można utworzyć X12 umowy, wykonując następujące czynności:  

### <a name="from-the-azure-portal-home-page"></a>Na stronie Azure głównej portalu

Po zalogowaniu się do [portalu Azure](http://portal.azure.com "Azure portal"):  
1. Z menu po lewej stronie wybierz pozycję **Przeglądaj** .  

>[AZURE.TIP]Łącze **Przejdź** nie jest widoczne, konieczne może być najpierw rozwinąć menu. W tym celu kliknąć łącze **Pokaż menu** , który znajduje się w lewym górnym rogu menu zwinięte.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Wpisz *integracji* w polu wyszukiwania filtr, a następnie wybierz pozycję **Konta integracji** z listy wyników.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. W kartę **Konta integracji** , która zostanie otwarta w górę wybierz konto integracji, w którym ma zostać utworzony umowę. Jeśli nie widzisz integracji konta list, [utworzyć pierwszy](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Wybierz Kafelek **umów** . Jeśli nie widzisz umów kafelka, dodaj go najpierw.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Wybierz przycisk **Dodaj** na kartę umowy, która zostanie otwarta.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Wprowadź **nazwę** dla umowy, a następnie wybierz **Typ umowy**, **Hosta partnera**, **Tożsamości hosta**, **Partnera gościa**, **Tożsamości gościa**w kartę umowy, która zostanie otwarta.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Po ustawieniu odbierania ustawienia właściwości, kliknij przycisk **OK**  
Przejdź:  
8. Wybierz **Ustawienia odbierania** Konfigurowanie sposobu obsługi wiadomości otrzymywane przy użyciu tej Umowy.  
9. Kontrolka ustawień odbierania jest podzielony na poniższych sekcjach, w tym identyfikatory, potwierdzenia, schematy, koperty, numery kontroli, reguły sprawdzania poprawności i ustawienia wewnętrzne. Konfigurowanie tych właściwości zgodnie z umową wraz z partnerem będzie wymianę wiadomości z. Oto widok decyduje, skonfigurować je według sposobu niniejszej Umowy do identyfikowania i obsługi wiadomości przychodzących:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Wybierz przycisk **OK** , aby zapisać ustawienia.  

### <a name="identifiers"></a>Identyfikatory

|Właściwość|Opis |
|---|---|
|ISA1 (kwalifikator autoryzacji)|Z listy rozwijanej wybierz wartość kwalifikator autoryzacji.|
|ISA2|Opcjonalnie. Wprowadź wartość informacji autoryzacji. Jeśli wprowadzona dla ISA1 wartość jest równa 00, wprowadź co najmniej jeden znak alfanumeryczny i maksymalnie 10.|
|ISA3 (zabezpieczeń kwalifikator)|Z listy rozwijanej wybierz wartość kwalifikator zabezpieczeń.|
|ISA4|Opcjonalnie. Wprowadź wartość informacji zabezpieczeń. Jeśli wprowadzona dla ISA3 wartość jest równa 00, wprowadź co najmniej jeden znak alfanumeryczny i maksymalnie 10.|

### <a name="acknowledgments"></a>Potwierdzenia 

|Właściwość|Opis |
|----|----|
|TA1 poprawnie|Zaznacz to pole wyboru, aby zwrócić techniczne potwierdzenia (TA1) do nadawcy wymiany. Te potwierdzenia są wysyłane do nadawcy wymiany na podstawie ustawień Wyślij umowę.|
|FA oczekiwaniami|Zaznacz to pole wyboru, aby zwrócić funkcjonalności potwierdzenia (FA) do nadawcy wymiany. Następnie określ, czy potwierdzeń 997 lub 999 według wersji schematu, którą pracujesz. Te potwierdzenia są wysyłane do nadawcy wymiany na podstawie ustawień wysyłanie umowy.|
|Dołączanie pętli AK2-IK2|Zaznaczenie tego pola wyboru Włącz generowanie AK2 pętli w funkcjonalności potwierdzenia dla zestawów zaakceptowanych transakcji. Uwaga: To pole wyboru jest włączone tylko wtedy, gdy jest zaznaczone pole wyboru FA poprawnie.|

### <a name="schemas"></a>Schematy

Wybierz schemat dla każdego typu transakcji (ST1) i aplikacji nadawcy (GS2). Proces odbierania rozkłada wiadomości przychodzącej dopasowując wartości ST1 i GS2 w przychodzących wiadomości z wartości, które Ustaw tutaj i schemat przychodzących wiadomości ze schematem, ustaw tutaj.

|Właściwość|Opis |
|----|----|
|Wersja|Wybierz pozycję X12 wersji|
|Typ transakcji (ST01)|Wybierz typ transakcji|
|Nadawca aplikacji (GS02)|Wybierz aplikację nadawcy|
|Schematu|Wybierz plik schematu, który chcesz nam. Pliki schematów znajdują się na Twoim koncie integracji.|

### <a name="envelopes"></a>Koperty

|Właściwość|Opis |
|----|----|
|Użycie ISA11|To pole umożliwia określenie separator w zestawie transakcji:</br></br>Wybierz pozycję standardowy identyfikator umożliwia zapis dziesiętny: "." zamiast Zapis dziesiętny przychodzących dokumentu w Edytuj otrzymają procesu.</br></br>Wybierz separator cyklu, aby określić separator dla powtarzających się wystąpień elementu proste dane lub struktura powtarzających się danych. Na przykład (^) jest zazwyczaj używany jako separator cyklu. Schematy HIPAA można używać tylko (^).|

### <a name="control-numbers"></a>Numery kontroli

|Właściwość|Opis |
|----|----|
|Nie zezwalaj na duplikaty wymiany numer formantu|Zaznacz tę opcję, aby zablokować skrzyżowania zduplikowane. Po zaznaczeniu portalu usługi BizTalk sprawdza numer formantu wymiany (ISA13) otrzymanej wymiany nie pasuje numer kontrolny wymianę. Jeśli zostanie wykryta dopasowanie, proces Odbierz nie przetwarza wymiany.<br/>Jeśli użytkownik wybrał niezezwalanie na numery kontroli zduplikowanych wymiany, można określić liczbę dni, w których sprawdzanie jest wykonywane przez nadanie odpowiednią wartość podczas sprawdzania zduplikowanych ISA13 co n dni.|
|Nie zezwalaj na duplikaty numerów kontroli grupy|Zaznacz tę opcję, aby zablokować skrzyżowania z grupy zduplikowane numery kontroli.|
|Nie zezwalaj na duplikaty liczba transakcji zestawu sterowania|Zaznacz tę opcję, aby zablokować skrzyżowania z numerami sterowania zestaw transakcji zduplikowane.|

### <a name="validations"></a>Sprawdzanie poprawności

|Właściwość|Opis |
|----|----|
|Typ wiadomości|Typ wiadomości grupy użytkowników, takie jak zamówienia zakupu 850 lub potwierdzenia implementacji 999.|
|Edytuj sprawdzania poprawności|Edytuj poprawności na typy danych zdefiniowane przez właściwości Edytuj schematu, ograniczeń długości, elementy puste dane i separatory końcowych.|
|Rozszerzone sprawdzania poprawności|Jeśli typ danych nie jest grupy użytkowników, sprawdzanie poprawności znajduje się na wymagania elementu danych i dozwolone cyklu, wyliczenia i sprawdzanie poprawności danych elementu długość (min i max).|
|Zezwalaj na zera wiodące i końcowe|Dodatkowe miejsce i zero znaki wiodące i końcowe są zachowywane. Nie są usuwane.|
|Końcowe zasad separatora|Umożliwia generowanie końcowe separatory na wymiany odebrane. Opcje obejmują NotAllowed, opcjonalnie i obowiązkowe.|

### <a name="internal-settings"></a>Ustawienia wewnętrzne

|Właściwość|Opis |
|----|----|
|Konwertowanie formatu dziesiętnego dorozumianych Nn się opierać 10 wartości liczbowych|Konwertuje liczbę grupy użytkowników, którą określono w formacie Nn na podstawie 10 wartość liczbową pośrednie XML w portalu usługi BizTalk.|
|Utwórz puste znaczniki XML, jeśli są dozwolone separatory końcowe|Zaznacz to pole wyboru, aby uwzględnić puste znaczniki XML końcowe separatory nadawca wymiany.|
|Łączenia we wsady przetwarzania przychodzącego|Podziel wymiany jako zestawy transakcji - zawieszenia zestawy transakcji wystąpi błąd: analizuje każdej transakcji ustawić w wymiany do innego dokumentu XML, stosując odpowiednie koperty do zestawu transakcji. Po wybraniu tej opcji Jeśli transakcji jeden lub więcej zestawów w wymiany nie sprawdzanie poprawności, a następnie usług BizTalk zawiesza te zestawy transakcji. </br></br>Podziel wymiany jako zestawy transakcji - zawieszenia wymiany wystąpi błąd: analizuje każdej transakcji Ustawianie w wymiany do innego dokumentu XML, stosując odpowiednie koperty. Przy użyciu tej opcji Jeśli co najmniej jednej transakcji zestawów w wymiany nie sprawdzanie poprawności, a następnie usług BizTalk zawiesza całego wymiany.</br></br>Zachowaj wymiany — zawieszenia zestawy transakcji w przypadku błędu: pozostaną niezmienione tworzenia dokumentu XML całego wymiany wsadowej wymiany. Po wybraniu tej opcji Jeśli onAe lub więcej transakcji zestawów w wymiany nie sprawdzanie poprawności, a następnie usług BizTalk zawiesza tylko te transakcji zestawy, pozostawiając procesu wszystkie zestawy transakcji.</br></br>Zachowaj wymiany — zawieszenia wymiany wystąpi błąd: pozostaną niezmienione tworzenia dokumentu XML całego wymiany wsadowej wymiany. Przy użyciu tej opcji Jeśli co najmniej jednej transakcji zestawów w wymiany nie sprawdzanie poprawności, a następnie usług BizTalk zawiesza całego wymiany.</br></br>|

Umowy jest gotowy do obsługi przychodzących wiadomości, które są zgodne z wybranego schematu.

Aby skonfigurować ustawienia, które obsługują wiadomości wysyłanych do partnerów:  
11. Wybierz **Ustawienia wysyłania** do konfigurowania sposobu obsługi wiadomości wysyłane za pośrednictwem niniejszej Umowy.  

Przycisk Ustawienia wysyłania jest podzielony na poniższych sekcjach, w tym identyfikatory, potwierdzenia, schematy, kopert, numery kontroli, zestawy znaków i separatory i sprawdzanie poprawności. 

Oto widok kontrolować następujące elementy. Wybierz odpowiednie ustawienia według sposobu obsługi wiadomości wysyłanych do partnerów za pośrednictwem niniejszej Umowy:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Wybierz przycisk **OK** , aby zapisać ustawienia.  

### <a name="identifiers"></a>Identyfikatory
|Właściwość|Opis |
|----|----|
|Kwalifikator autoryzacji (ISA1)|Z listy rozwijanej wybierz wartość kwalifikator autoryzacji.|
|ISA2|Wprowadź wartość informacji autoryzacji. Jeśli ta wartość jest równa 00, wprowadź co najmniej jeden znak alfanumeryczny i maksymalnie 10.|
|Kwalifikator zabezpieczeń (ISA3)|Z listy rozwijanej wybierz wartość kwalifikator zabezpieczeń.|
|ISA4|Wprowadź wartość informacji zabezpieczeń. Jeśli ta wartość jest inne niż 00 dla pola tekstowego wartości (ISA4), wprowadź co najmniej jedną wartość alfanumeryczny i maksymalnie 10.|

### <a name="acknowledgment"></a>Potwierdzanie
|Właściwość|Opis |
|----|----|
|TA1 poprawnie|Zaznacz to pole wyboru, aby zwrócić techniczne potwierdzenia (TA1) do nadawcy wymiany. To ustawienie określa, że partnera hosta, który wysyła wiadomość żądania potwierdzenia od partnera gościa w umowie. Te potwierdzenia oczekuje przez partnera hosta na podstawie ustawień odbieranie układu.|
|FA oczekiwaniami|Zaznacz to pole wyboru, aby powrócić funkcjonalności potwierdzenia (FA) do nadawcy wymiany, a następnie określ, czy potwierdzeń 997 lub 999 według wersji schematu, którą pracujesz. Te potwierdzenia oczekuje przez partnera hosta na podstawie ustawień odbieranie układu.|
|Wersja FA|Wybierz wersję FA|

### <a name="schemas"></a>Schematy
|Właściwość|Opis |
|----|----|
|Wersja|Wybierz pozycję X12 wersji|
|Typ transakcji (ST01)|Wybierz typ transakcji|
|SCHEMATU|Wybierz schemat, aby użyć. Schematy znajdują się na Twoim koncie integracji. Aby uzyskać dostęp do schematów, najpierw połączyć konta integracji aplikacji logicznych.|

### <a name="envelopes"></a>Koperty
|Właściwość|Opis |
|----|----|
|Użycie ISA11|To pole umożliwia określenie separator w zestawie transakcji:</br></br>Wybierz pozycję standardowy identyfikator umożliwia zapis dziesiętny: "." zamiast Zapis dziesiętny przychodzące dokumentu w Edytuj otrzymają procesu.</br></br>Wybierz separator cyklu, aby określić separator dla powtarzających się wystąpień elementu proste dane lub struktura powtarzających się danych. Na przykład (^) jest zazwyczaj używany jako separator cyklu. Schematy HIPAA można używać tylko (^).</br>|
|Separator cyklu|Wprowadź separator cyklu|
|Numer wersji Control (ISA12)|Wybierz wersję standardowy X12 jest używana przez Portal usług BizTalk do wygenerowania wymiany poczty wychodzącej.|
|Wskaźnik użycia (ISA15)|Wprowadź czy kontekście wymiany informacji (I), dane produkcji (P) lub testowanie danych (T). Odbieranie Edytuj planowana podkreśla tej właściwości do kontekstu.|
|Schematu|Możesz wprowadzić jak portalu usługi BizTalk generuje GS i ST segmentów zakodowany X12 wymiany, wysyłające do nich Wyślij.</br></br>Możesz skojarzyć wartości GS1, GS2 GS3, GS4, GS5, GS7 i GS8 elementami danych z wartościami transakcji typu i elementów danych wersji i wydania. Gdy portalu usługi BizTalk Określa, że wiadomość XML zawiera wartości Typ transakcji i elementy wersji i wydania w wierszu siatki, a następnie go wypełnia elementami danych GS1, GS2 GS3, GS4, GS5, GS7 i GS8 w kopercie wymiany poczty wychodzącej z wartościami z tym samym wierszu siatki. Wartości Typ transakcji i elementy wersji i wydania musi być unikatowa.</br></br>Opcjonalnie. Dla GS1 wybierz wartość funkcjonalności kodu z listy rozwijanej.</br></br>Wymagane. GS2 wprowadź wartość alfanumeryczny nadawcy aplikacji z co najmniej dwa znaki i maksymalnie 15 znaków.</br></br>Wymagane. GS3 wprowadź wartość alfanumeryczny odbiorcy aplikacji z co najmniej dwa znaki i maksymalnie 15 znaków.</br></br>Opcjonalnie. Aby uzyskać GS4 zaznacz CCYYMMDD lub rrmmdd.</br></br>Opcjonalnie. Dla GS5 wybierz hh: mm, HHMMSS lub HHMMSSdd.</br></br>Opcjonalnie. Dla GS7 wybierz wartość dla właściwej agencji z listy rozwijanej.</br></br>Opcjonalnie. GS8 wprowadź wartość alfanumeryczny dokumentu zidentyfikować co najmniej jeden znak o wysokości 12 znaków.</br></br>**Uwaga**: te wartości do portalu usługi BizTalk wprowadzane w polach GS wymiany tworzenia Jeśli transakcji wpisz i elementy wersji i wydania w tym samym wierszu jest dopasowanie kontakty skojarzone z wymiany.|

### <a name="control-numbers"></a>Numery kontroli
|Właściwość|Opis |
|----|----|
|Wymiany numer formantu (ISA13)|Wymagane. Wprowadź zakres wartości numer formantu wymiany używane przez Portal usługi BizTalk do generowania wymiany poczty wychodzącej. Wprowadź wartość liczbową z zakresu od 1 i maksymalnie 999999999.|
|Numer sterowania grupy (GS06)|Wymagane. Wprowadź zakres liczb, które portalu usługi BizTalk należy użyć numer sterowania grupy. Wprowadź wartość liczbową z co najmniej jeden znak i maksymalnie trzynaście znaków.|
|Numer sterowania zestaw transakcji (ST02)|Numer kontrola Ustawianie transakcji (ST02) wprowadź wybrany zakres wartości liczbowe w polach środkowym wymagane i wartości alfanumeryczne opcjonalne prefiks i sufiks. Maksymalna długość wszystkimi czterema polami jest trzynaście znaków.|
|Prefiks|Aby określić zakres liczb sterowania zestaw transakcji używane w potwierdzenie, wprowadź wartości w pola numer (ST02) formantu ACK. Wprowadź wartość liczbową dla środka dwa pola i alfanumeryczny wartość (Jeśli to konieczne) w polach prefiksu i sufiksu. Inicjał drugiego pola są wymagane i zawierają wartości minimalnej i maksymalnej liczby kontroli; Prefiks i sufiks są opcjonalne. Maksymalna długość dla wszystkich trzech pól jest trzynaście znaków.|
|Sufiks|Aby określić zakres liczb sterowania zestaw transakcji używane w potwierdzenie, wprowadź wartości w pola numer (ST02) formantu ACK. Wprowadź wartość liczbową dla środka dwa pola i alfanumeryczny wartość (Jeśli to konieczne) w polach prefiksu i sufiksu. Inicjał drugiego pola są wymagane i zawierają wartości minimalnej i maksymalnej liczby kontroli; Prefiks i sufiks są opcjonalne. Maksymalna długość dla wszystkich trzech pól jest trzynaście znaków.|

### <a name="character-sets-and-separators"></a>Zestawy znaków i separatory
Inne niż zestawu znaków, można wprowadzić inny zestaw ograniczniki może być używany dla każdego typu wiadomości. Jeśli nie określono zestaw znaków dla schematu danej wiadomości, jest używany domyślny zestaw znaków.

|Właściwość|Opis |
|----|----|
|Zestaw ma być używany znaków|Wybierz pozycję X12 zestawu znaków do sprawdzania poprawności właściwości, które można wprowadzić umowę.</br></br>**Uwaga**: BizTalk portalu usługi używa tego ustawienia, aby sprawdzić poprawność wartości wprowadzone dla właściwości powiązanych umowy. Potok Odbierz lub Wyślij planowana ignoruje tej właściwości zestawu znaków, podczas wykonywania przetwarzanie wykonawczego.|
|Schematu|Symbol wybierz pozycję (+) i wybierz schemat z listy rozwijanej. Wybrany schemat zaznacz separatory ustawiona na używanie:</br></br>Separator elementów składnik — wprowadzenie pojedynczego znaku do oddzielania elementów danych.</br></br>Separator elementów danych — wprowadzenie pojedynczego znaku do oddzielania elementów proste dane w elementach danych.</br></br></br></br>Znak zastępczy — zaznacz to pole wyboru, jeśli ładunek, który zawiera dane znaki są również używane jako części, danych lub separatory składnik. Następnie należy wprowadzić znak zastępczy. Podczas generowania ruchu wychodzącego X12 wiadomości, wszystkie wystąpienia znaków separatora ładunku, które dane są zastępowane określony znak.</br></br>Segmenty element końcowy — wprowadź pojedynczy znak, aby wskazać koniec segmentu grupy użytkowników.</br></br>Sufiks — wybierz znak, który jest używany z identyfikatorem segmentu. Jeśli sufiks, element danych element końcowy części może być pusty. Jeśli element końcowy części jest puste, należy wyznaczyć sufiks.|

### <a name="validation"></a>Sprawdzanie poprawności
|Właściwość|Opis |
|----|----|
|Typ wiadomości|Wybranie tej opcji umożliwia sprawdzanie poprawności Odbiorca wymiany. To uwierzytelnienie poprawności Edytuj dane transakcji zestawu elementów, sprawdzanie poprawności typów danych, długość ograniczenia i elementami danych puste i końcowe separatory.|
|Edytuj sprawdzania poprawności||
|Rozszerzone sprawdzania poprawności|Wybranie tej opcji umożliwia rozszerzonego poprawności skrzyżowania otrzymane od tego nadawcy wymiany. Ta opcja uwzględnia sprawdzania poprawności długość pola, Opcjonalność i liczbę powtórzeń oprócz sprawdzania poprawności typ danych XSD. Możesz włączyć sprawdzanie poprawności rozszerzenia bez włączania Edytuj sprawdzanie poprawności, lub odwrotnie.|
|Zezwalaj na zera wiodące / końcowe|Wybranie tej opcji określa, że wymiany Edytuj otrzymane od strony nie sprawdzania poprawności Jeśli element danych w wymiany Edytuj nie jest zgodna z jego długość wymagania z powodu lub końcowe spacje, ale jest zgodna z wymogu długość, gdy są usuwane.|
|Separator końcowe|Wybranie tej opcji umożliwia określenie wymiany Edytuj otrzymane od strony niepowodzenie sprawdzania poprawności, jeśli element danych w wymiany Edytuj nie jest zgodna z wymogu długość ze względu na zera wiodące (lub końcowe) lub końcowe spacje, ale jest zgodna z wymogu długość, gdy są usuwane.</br></br>Wybierz pozycję niedozwolone, jeśli nie chcesz umożliwić końcowe ograniczniki i separatory w wymiany otrzymane od tego nadawcy wymiany. Jeśli wymiany zawiera końcowych ograniczniki i separatory, jest uznany za nieważny.</br></br>Wybierz opcjonalne, aby zaakceptować skrzyżowania z lub bez końcowych ograniczniki i separatory.</br></br>Wybierz pozycję obowiązkowe, jeśli otrzymanej wymiany musi zawierać końcowe ograniczniki i separatory.|

Po zakończeniu wybierz **przycisk OK** na otwieranie karty:  
13. Wybierz Kafelek **umów** na karta Integracja konta, a zobaczysz umowę dodane na liście.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Dowiedz się więcej
- [Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack")  
