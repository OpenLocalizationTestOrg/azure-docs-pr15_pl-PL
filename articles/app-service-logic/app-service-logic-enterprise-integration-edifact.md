<properties 
    pageTitle="Integracja przedsiębiorstwa z EDIFACT | Microsoft Azure" 
    description="Dowiedz się, jak używać umów EDIFACT do tworzenia aplikacji logiki" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Integracja przedsiębiorstwa z EDIFACT 

> [AZURE.NOTE] Tej stronie opisano funkcje EDIFACT logiczny aplikacji. Aby uzyskać informacje na X12 kliknij [tutaj](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Utwórz umowę EDIFACT 
Przed przystąpieniem do wymiany wiadomości EDIFACT, musisz utworzyć umowę EDIFACT i przechowywaniu go na koncie integracji. Poniższe kroki przeprowadzi Cię przez proces tworzenia umowę EDIFACT.

### <a name="heres-what-you-need-before-you-get-started"></a>Poniżej opisano, co jest potrzebne przed rozpoczęciem
- [Integracja z programem konta](./app-service-logic-enterprise-integration-accounts.md) zdefiniowane w ramach subskrypcji Azure  
- Co najmniej dwa [partnerów](./app-service-logic-enterprise-integration-partners.md) już określonych na koncie integracji  

>[AZURE.NOTE]Podczas tworzenia umowę, zawartości w wiadomości możesz będą otrzymywać/Wyślij do i z partnera musi odpowiadać typ umowy.    


Po wprowadzeniu [utworzone konto integracji](./app-service-logic-enterprise-integration-accounts.md) i [dodane partnerów](./app-service-logic-enterprise-integration-partners.md)umowę EDIFACT można utworzyć, wykonując następujące czynności:  

### <a name="from-the-azure-portal-home-page"></a>Na stronie Azure głównej portalu

Po zalogowaniu się do [portalu Azure](http://portal.azure.com "Azure portal"):  
1. Z menu po lewej stronie wybierz pozycję **Przeglądaj** .  

>[AZURE.TIP]Łącze **Przejdź** nie jest widoczne, konieczne może być najpierw rozwinąć menu. W tym celu kliknąć łącze **Pokaż menu** , znajdującego się w lewym górnym rogu menu zwinięte.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. Wpisz *integracji* w polu wyszukiwania filtr, a następnie wybierz pozycję **Konta integracji** z listy wyników.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. W kartę **Konta integracji** , która zostanie otwarta w górę wybierz konto integracji, w którym ma zostać utworzony umowę. Jeśli nie widzisz integracji konta list, [utworzyć pierwszy](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Wybierz Kafelek **umów** . Jeśli nie widzisz umów kafelka, dodaj go najpierw.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Wybierz przycisk **Dodaj** na kartę umowy, która zostanie otwarta.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Wprowadź **nazwę** dla umowy, a następnie wybierz **Typ umowy** EDIFACT, **Hosta partnera**, **Tożsamości hosta**, **Partnera gościa**, **Tożsamości gościa**w kartę umowy, która zostanie otwarta.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Po ustawieniu właściwości umowę, wybierz pozycję **Ustawienia odbierania** Konfigurowanie sposobu obsługi wiadomości otrzymywane przy użyciu tej Umowy.  
8. Kontrolka ustawień odbierania jest podzielony na następujące sekcje, w tym identyfikatory, potwierdzenia, schematy, numery kontroli, sprawdzanie poprawności, ustawienia wewnętrzne i przetwarzanie wsadowe. Konfigurowanie tych właściwości zgodnie z umową wraz z partnerem będzie wymianę wiadomości z. Oto widok decyduje, skonfigurować je według sposobu niniejszej Umowy do identyfikowania i obsługi wiadomości przychodzących:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Wybierz przycisk **OK** , aby zapisać ustawienia.  

### <a name="identifiers"></a>Identyfikatory

|Właściwość|Opis |
|---|---|
|UNB6.1 (odwołanie adresatów hasło)|Wprowadź alfanumeryczny wartość z zakresu od 1 do 14 znaków.|
|UNB6.2 (odwołanie adresatów kwalifikator)|Wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie dwa znaki.|

### <a name="acknowledgments"></a>Potwierdzenia 

|Właściwość|Opis |
|----|----|
|Odebranie komunikatu (CONTRL)|Zaznacz to pole wyboru, aby zwrócić techniczne potwierdzenia (CONTRL) do nadawcy wymiany. Potwierdzanie są wysyłane do nadawcy wymiany na podstawie ustawień wysyłanie umowy.|
|Potwierdzenia (CONTRL)|Zaznacz to pole wyboru, aby zwrócić funkcjonalności potwierdzenia (CONTRL) do nadawcy wymiany Potwierdzanie są wysyłane do nadawcy wymiany na podstawie ustawień wysyłanie umowy.|

### <a name="schemas"></a>Schematy

|Właściwość|Opis |
|----|----|
|UNH2.1 (TYP)|Wybierz typ zestawu transakcji.|
|UNH2.2 (WERSJA)|Wprowadź numer wersji komunikatu. (Minimum, o jeden znak, maksimum, trzy znaki).|
|UNH2.3 (WERSJA RELEASE)|Wprowadź numer wersji wiadomości. (Minimum, o jeden znak, maksimum, trzy znaki).|
|UNH2.5 (SKOJARZONY KOD PRZYDZIELONYCH)|Wprowadź kod przydzielone. (Maksymalnie sześć znaków. Musi być alfanumerycznych).|
|UNG2.1 (IDENTYFIKATOR NADAWCY APLIKACJI)|Wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie 35 znaków.|
|UNG2.2 (KWALIFIKATOR KODU NADAWCY APLIKACJI)|Wprowadź wartość alfanumeryczne z maksymalnie czterech znaków.|
|SCHEMATU|Wybierz wcześniej przekazane schemat, który ma być używany z konta skojarzonego integracji.|

### <a name="control-numbers"></a>Numery kontroli

|Właściwość|Opis |
|----|----|
|Nie zezwalaj na duplikaty wymiany numer formantu|Zaznacz to pole wyboru, aby zablokować skrzyżowania zduplikowane. Po zaznaczeniu akcji dekodowanie EDIFACT sprawdza numer formantu wymiany (UNB5) otrzymane wymiany nie ma odpowiednika numer formantu poprzednio przetwarzanych wymiany. Jeśli zostanie wykryta dopasowania, a następnie wymiany nie jest przetwarzana.
|Wyszukaj zduplikowane UNB5 co (dni)|Jeśli użytkownik wybrał niezezwalanie na numery kontroli zduplikowanych wymiany, można określić liczbę dni, w których sprawdzanie jest wykonywane przez podanie odpowiednich wartości dla opcji **Wyszukaj zduplikowane UNB5 co (dni)** .|
|Nie zezwalaj na duplikaty numerów kontroli grupy|Zaznacz to pole wyboru, aby zablokować skrzyżowania z grupy zduplikowane numery kontroli (UNG5).|
|Nie zezwalaj na duplikaty liczba transakcji zestawu sterowania|Zaznacz to pole wyboru, aby zablokować skrzyżowania liczbami sterowania zestawu zduplikowanych transakcji (UNH1).|
|Numer sterowania potwierdzenia EDIFACT|Aby wyznaczyć zestaw transakcji numery umożliwia potwierdzenie, wprowadź wartość prefiksu, zakres numery i sufiks.|

### <a name="validations"></a>Sprawdzanie poprawności

|Właściwość|Opis |
|----|----|
|Typ wiadomości|Określ typ wiadomości. Zakończeniu każdego wiersza sprawdzania poprawności innego zostaną automatycznie dodane. Jeśli zasady nie zostaną określone, wiersz oznaczone jako domyślny jest używana do sprawdzania poprawności.|
|Edytuj sprawdzania poprawności|Zaznacz to pole wyboru w celu sprawdzenia poprawności Edytuj na typy danych zdefiniowane przez właściwości Edytuj schematu, ograniczeń długości, elementy puste dane i separatory końcowych.|
|Rozszerzone sprawdzania poprawności|Zaznacz to pole wyboru, aby włączyć rozszerzonego weryfikację skrzyżowania otrzymane od tego nadawcy wymiany (XSD). Ta opcja uwzględnia sprawdzania poprawności długość pola, Opcjonalność i liczbę powtórzeń oprócz sprawdzania poprawności typ danych XSD.|
|Zezwalaj na zera wiodące i końcowe|Wybierz opcję **Zezwalaj** , aby umożliwić wiodące i końcowe zera; **NotAllowed** nie umożliwia interlinii i końcowe zera lub **Przycinanie** przycięcia wiodące i końcowe wyzerowania.|
|Przycinanie zera wiodące i końcowe|Zaznacz to pole wyboru, aby przyciąć dowolnego zer wiodących lub końcowych|
|Końcowe zasad separatora|Wybierz pozycję **Niedozwolone** , jeśli nie chcesz umożliwić końcowe ograniczniki i separatory w wymiany otrzymane od tego nadawcy wymiany. Jeśli wymiany zawiera końcowych ograniczniki i separatory, jest uznany za nieważny. Wybierz **opcjonalne** , aby zaakceptować skrzyżowania z lub bez końcowych ograniczniki i separatory. Zaznaczenie **obowiązkowe** otrzymanej wymiany musi zawierać końcowe ograniczniki i separatory.|

### <a name="internal-settings"></a>Ustawienia wewnętrzne

|Właściwość|Opis |
|----|----|
|Utwórz puste znaczniki XML, jeśli są dozwolone separatory końcowe|Zaznacz to pole wyboru, aby uwzględnić puste znaczniki XML końcowe separatory nadawca wymiany.|
|Łączenia we wsady przetwarzania przychodzącego|Są następujące opcje:</br></br>**Podziel wymiany jako zestawy transakcji - zawieszenia zestawy transakcji w przypadku błędu**: analizuje każdej transakcji Ustawianie w wymiany do innego dokumentu XML, stosując odpowiednie koperty do zestawu transakcji. Użyj tej opcji Jeśli co najmniej jeden zestaw transakcji w wymiany niepowodzenie sprawdzania poprawności, następnie te zestawy transakcji są zawieszone. Podziel wymiany jako zestawy transakcji - zawieszenia wymiany wystąpi błąd: analizuje każdej transakcji Ustawianie w wymiany do innego dokumentu XML, stosując odpowiednie koperty. Przy użyciu tej opcji Jeśli co najmniej jednej transakcji zestawów w wymiany nie sprawdzanie poprawności, a następnie całą wymiany zostanie zawieszone.</br></br>**Zachowaj wymiany - zawiesić zestawów transakcji w przypadku błędu**: pozostaną niezmienione tworzenia dokumentu XML całego wymiany wsadowej wymiany. Użyj tej opcji Jeśli co najmniej jeden zestaw transakcji w wymiany niepowodzenie sprawdzania poprawności, następnie te zestawy transakcji zawieszenia podczas przetwarzania wszystkie zestawy transakcji.</br></br>**Zachowaj wymiany - zawieszenia wymiany wystąpi błąd**: pozostaną niezmienione tworzenia dokumentu XML całego wymiany wsadowej wymiany. Przy użyciu tej opcji Jeśli co najmniej jednej transakcji zestawów w wymiany nie sprawdzanie poprawności, a następnie całą wymiany zostało zawieszone.|

Umowy jest gotowy do obsługi wiadomości przychodzących, które są zgodne z wybranych ustawień.

Aby skonfigurować ustawienia, które obsługują wiadomości wysyłanych do partnerów:  
10. Wybierz **Ustawienia wysyłania** do konfigurowania sposobu obsługi wiadomości wysyłane za pośrednictwem niniejszej Umowy.  

Przycisk Ustawienia wysyłania jest podzielony na poniższych sekcjach, w tym identyfikatory, potwierdzenia, schematy, koperty, zestawy znaków i separatory, numery kontroli i sprawdzanie poprawności. 

Oto widok kontrolować następujące elementy. Wybierz odpowiednie ustawienia według sposobu obsługi wiadomości wysyłanych do partnerów za pośrednictwem niniejszej Umowy:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Wybierz przycisk **OK** , aby zapisać ustawienia.  

### <a name="identifiers"></a>Identyfikatory
|Właściwość|Opis |
|----|----|
|UNB1.2 (wersja składni)|Wybierz wartość z przedziału od **1** do **4**.|
|UNB2.3 (odwrotnej routingu adres nadawcy)|Wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie 14 znaków.|
|UNB3.3 (adres adresata routingu odwrotnej)|Wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie 14 znaków.|
|UNB6.1 (odwołanie adresatów hasło)|Wprowadź wartość alfanumeryczne z co najmniej jeden i maksymalnie 14 znaków.|
|UNB6.2 (odwołanie adresatów kwalifikator)|Wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie dwa znaki.|
|UNB7 (identyfikator aplikacji odwołanie)|Wprowadź wartość alfanumerycznych z co najmniej jeden znak i maksymalnie 14 znaków|

### <a name="acknowledgment"></a>Potwierdzanie
|Właściwość|Opis |
|----|----|
|Odebranie komunikatu (CONTRL)|Zaznacz to pole wyboru, jeśli hostowanej partnera przewiduje odbieranie do odbierania technical potwierdzenia (CONTRL). To ustawienie określa, że hostowanej u partnera, który wysyła wiadomość, prosząc o potwierdzenie partnera gościa.|
|Potwierdzenia (CONTRL)|Zaznacz to pole wyboru, jeśli partner hostowanej przewiduje otrzyma funkcjonalności (CONTRL). To ustawienie określa, że hostowanej u partnera, który wysyła wiadomość, prosząc o potwierdzenie partnera gościa.|
|Generowanie BL1-BL4 pętli dla zestawów zaakceptowanych transakcji|Jeśli zdecydujesz się żądania potwierdzenia funkcjonalności, zaznacz to pole wyboru, aby wymusić Generowanie BL1-BL4 pętli w funkcjonalności potwierdzenia CONTRL dla zestawów zaakceptowanych transakcji.|

### <a name="schemas"></a>Schematy
|Właściwość|Opis |
|----|----|
|UNH2.1 (TYP)|Wybierz typ zestawu transakcji.|
|UNH2.2 (WERSJA)|Wprowadź numer wersji komunikatu.|
|UNH2.3 (WERSJA RELEASE)|Wprowadź numer wersji wiadomości.|
|SCHEMATU|Wybierz schemat, aby użyć. Schematy znajdują się na Twoim koncie integracji. Aby uzyskać dostęp do schematów, najpierw połączyć konta integracji aplikacji logicznych.|

### <a name="envelopes"></a>Koperty
|Właściwość|Opis |
|----|----|
|UNB8 (kod priorytetu przetwarzania)|Wprowadź wartość alfabetycznie, której nie ma więcej niż jeden znak.|
|UNB10 (Umowa komunikacji)|Wprowadź wartość alfanumerycznych z co najmniej jeden znak i maksymalnie 40 znaków.|
|UNB11 (Test wskaźnik)|Zaznacz to pole wyboru, aby wskazać, że wymiany wygenerowane jest testowymi danymi|
|Stosowanie UNA segmentu (Porada ciąg usługi)|Zaznacz to pole wyboru, aby wygenerować segment UNA wymiany do wysłania.|
|Stosowanie segmentów UNG (grupa funkcja nagłówek)|Zaznacz to pole wyboru, aby utworzyć segmenty grupowania w nagłówku grupy funkcjonalnej w wiadomości wysłanych do partnera gościa. Aby utworzyć segmenty UNG używane są następujące wartości:</br></br>**UNG1**wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie sześć znaków.</br></br>**UNG2.1**wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie 35 znaków.</br></br>**UNG2.2**wprowadź wartość alfanumeryczne z maksymalnie czterech znaków.</br></br>**UNG3.1**wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie 35 znaków.</br></br>**UNG3.2**wprowadź wartość alfanumeryczne z maksymalnie czterech znaków.</br></br>**UNG6**wprowadź wartość alfanumeryczne z co najmniej jeden i maksymalnie trzy znaki.</br></br>**UNG7.1**wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie trzy znaki.</br></br>**UNG7.2**wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie trzy znaki.</br></br>**UNG7.3**wprowadź wartość alfanumeryczne z co najmniej 1 znak i maksymalnie 6 znaków.</br></br>**UNG8**wprowadź wartość alfanumeryczne z co najmniej jeden znak i maksymalnie 14 znaków.|

### <a name="character-sets-and-separators"></a>Zestawy znaków i separatory
Inne niż zestawu znaków, można wprowadzić inny zestaw ograniczniki może być używany dla każdego typu wiadomości. Jeśli nie określono zestaw znaków dla schematu danej wiadomości, jest używany domyślny zestaw znaków.

|Właściwość|Opis |
|----|----|
|UNB1.1 (identyfikator systemowy)|Wybierz znak EDIFACT Ustawianie stosowane do wymiany poczty wychodzącej.|
|Schematu|Z listy rozwijanej wybierz schemat. Jako każdy wiersz jest zakończone zostanie dodany nowy wiersz. Wybrany schemat zaznacz separatory ustawiona na używanie:</br></br>**Separator elementów składnik** — wprowadzenie pojedynczego znaku do oddzielania elementów danych.</br></br>**Separator elementów danych** — wprowadzenie pojedynczego znaku do oddzielania elementów proste dane w elementach danych.</br></br></br></br>**Znak zastępczy** — zaznacz to pole wyboru, jeśli ładunek, który zawiera dane znaki są również używane jako części, danych lub separatory składnik. Następnie należy wprowadzić znak zastępczy. Generowanie wiadomości wychodzących EDIFACT, wszystkie wystąpienia znaków separatora danych ładunku są zastępowane określony znak.</br></br>**Element końcowy części** — wprowadzenie pojedynczego znaku, aby wskazać koniec segmentu grupy użytkowników.</br></br>**Sufiks** — wybierz znak, który jest używany z identyfikatorem segmentu. Jeśli sufiks, element danych element końcowy części może być pusty. Jeśli element końcowy części jest puste, należy wyznaczyć sufiks.|

### <a name="control-numbers"></a>Numery kontroli
|Właściwość|Opis |
|----|----|
|UNB5 (wymiany numer formantu)|Wprowadź prefiks, zakres wartości numer kontrolny wymianę i sufiks. Wartości te są używane do wygenerowania wymiany poczty wychodzącej. Prefiks i sufiks są opcjonalne; wymagany jest numer kontrolki. Numer formantu są zwiększane dla każdej nowej wiadomości; Prefiks i sufiks zmieniają się.|
|UNG5 (grupowanie numer formantu)|Wprowadź prefiks, zakres wartości numer kontrolny wymianę i sufiks. Te wartości są używane do wygenerowania liczby sterowania grupy. Prefiks i sufiks są opcjonalne; wymagany jest numer kontrolki. Numer formantu są zwiększane dla każdej nowej wiadomości, aż do osiągnięcia maksymalnej wartości; Prefiks i sufiks zmieniają się.|
|UNH1 (numer nagłówka wiadomości)|Wprowadź prefiks, zakres wartości numer kontrolny wymianę i sufiks. Te wartości są używane do generowania numeru odwołania nagłówka wiadomości. Prefiks i sufiks są opcjonalne; wymagany jest numer odwołania. Numery odwołań są zwiększane dla każdej nowej wiadomości; Prefiks i sufiks zmieniają się.|

### <a name="validations"></a>Sprawdzanie poprawności
|Właściwość|Opis |
|----|----|
|Typ wiadomości|Wybranie tej opcji umożliwia sprawdzanie poprawności Odbiorca wymiany. To uwierzytelnienie poprawności Edytuj dane transakcji zestawu elementów, sprawdzanie poprawności typów danych, długość ograniczenia i elementami danych puste i szkolenia separatory.|
|Edytuj sprawdzania poprawności|Zaznacz to pole wyboru w celu sprawdzenia poprawności Edytuj na typy danych zdefiniowane przez właściwości Edytuj schematu, ograniczeń długości, elementy puste dane i separatory końcowych.|
|Rozszerzone sprawdzania poprawności|Wybranie tej opcji umożliwia rozszerzonego poprawności skrzyżowania otrzymane od tego nadawcy wymiany. Ta opcja uwzględnia sprawdzania poprawności długość pola, Opcjonalność i liczbę powtórzeń oprócz sprawdzania poprawności typ danych XSD. Możesz włączyć sprawdzanie poprawności rozszerzenia bez włączania Edytuj sprawdzanie poprawności, lub odwrotnie.|
|Zezwalaj na zera wiodące i końcowe|Wybranie tej opcji określa, że wymiany Edytuj otrzymane od strony nie sprawdzania poprawności Jeśli element danych w wymiany Edytuj nie jest zgodna z jego długość wymagania z powodu lub końcowe spacje, ale jest zgodna z wymogu długość, gdy są usuwane.|
|Przycinanie zera wiodące i końcowe|Wybranie tej opcji spowoduje przycinanie zera wiodące i końcowe.|
|Separator końcowe|Wybranie tej opcji umożliwia określenie wymiany Edytuj otrzymane od strony niepowodzenie sprawdzania poprawności, jeśli element danych w wymiany Edytuj nie jest zgodna z wymogu długość ze względu na zera wiodące (lub końcowe) lub końcowe spacje, ale jest zgodna z wymogu długość, gdy są usuwane.</br></br>Wybierz pozycję **Niedozwolone** , jeśli nie chcesz umożliwić końcowe ograniczniki i separatory w wymiany otrzymane od tego nadawcy wymiany. Jeśli wymiany zawiera końcowych ograniczniki i separatory, jest uznany za nieważny.</br></br>Wybierz **opcjonalne** , aby zaakceptować skrzyżowania z lub bez końcowych ograniczniki i separatory.</br></br>Zaznaczenie **obowiązkowe** otrzymanej wymiany musi zawierać końcowe ograniczniki i separatory.|

Po zakończeniu wybierz **przycisk OK** na otwartej karta:  
12. Wybierz Kafelek **umów** na karta Integracja konta, a zobaczysz umowę dodane na liście.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Dowiedz się więcej
- [Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack")  
