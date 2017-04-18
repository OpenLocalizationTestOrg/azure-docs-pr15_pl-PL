<properties 
    pageTitle="Firma Firma łączników i aplikacje interfejsu API w aplikacjach logiczny | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć i skonfigurować grupy użytkowników, EDIFACT AS2 i TPM łączników; Architektura microservices" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Firma Firma łączników i aplikacje interfejsu API

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logika aplikacji zawiera wiele BizTalk interfejsu API aplikacjami, które są niezbędne do integracji środowiska. Te aplikacje interfejsu API są oparte na pojęcia i narzędzia używane w obrębie serwera BizTalk, ale są teraz dostępne jako część aplikacji logicznych. 

Jedną z kategorii te aplikacje interfejsu API są te aplikacje interfejsu API między firmami (B2B). Za pomocą te aplikacje interfejsu API B2B, możesz łatwo dodawać partnerów, Tworzenie umów i wykonaj wszystko jako użytkownik będzie lokalnego przy użyciu grupy użytkowników, AS2 i EDIFACT.  

Te aplikacje interfejsu API B2B oferują możliwości "Wyzwalacza" i "Działania". Wyzwalacz powoduje uruchomienie nowego wystąpienia według określonego zdarzenia, takie jak nadejście X12 wiadomości od partnera. Akcja jest wynik, jak po otrzymaniu X12 wiadomości, a następnie wysłać wiadomości przy użyciu AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Co to jest firm do Business Connector lub aplikacji interfejsu API
Funkcja między firmami (B2B) obejmuje istniejących, gotowych aplikacji interfejsu API, umożliwiająca innej firmy, podziałów, aplikacji i tak dalej komunikowanie się za pomocą AS2, Edytuj i EDIFACT. 

Aplikacje interfejsu API B2B obejmują: 

Łącznik lub interfejsu API aplikacji | Opis
--- | ---
Zarządzanie Trading partnera BizTalk | Aplikacja interfejsu API, która umożliwia utworzenie relacji między firmami (B2B) przy użyciu partnerów i umów. Te relacje wykorzystanie AS2, EDIFACT i X12 Protocol (protokół).<br/><br/>Aplikacja interfejsu API TPM jest podstawowy wymagania łącznik AS2 i X12 lub EDIFACT interfejsu API aplikacje. 
Łącznik AS2 | Łącznik, który odbiera i wysyła wiadomości przy użyciu transportu AS2. Łącznik protokoły transportowe danych bezpieczeństwa i niezawodności w Internecie.
BizTalk EDIFACT | Aplikacja interfejsu API, który odbiera i wysyła wiadomości przy użyciu EDIFACT. EDIFACT również często nosi nazwę NZ/EDIFACT (ONZ/elektronicznej danych wymiany dla administracji, handlu i transportu) i powszechnie używane w branży.
BizTalk X12 | Interfejs API aplikację, która odbiera i wysyła wiadomości przy użyciu X12 Protocol (protokół). X12 również często nosi nazwę ASC X 12 (akredytowane standardy Komitet X12) i powszechnie używane w branży. 


Te aplikacje interfejsu API może wykonać różne grupy użytkowników wiadomości zadania. Na przykład przy użyciu łącznika AS2, można bezpieczne odbieranie i wysyłanie wiadomości różnego typu (płaskie pliku grupy użytkowników, XML, i tak dalej) do klienta, dzielenia w firmie, takich jak zasobów ludzkich lub każda osoba, która korzysta z AS2. 

Możesz tworzyć możliwie jak najwięcej aplikacje interfejsu API a łatwe tworzenie. Można również ponownie użyć pojedynczej aplikacji interfejsu API w wielu scenariuszy lub przepływów pracy.

Można to zrobić bez pisania kodów. Rozpocznijmy pracę. 


## <a name="requirements-to-get-started"></a>Wymagania, aby rozpocząć pracę
Podczas tworzenia aplikacji API B2B, istnieje kilka wymaganych zasobów. Te elementy muszą zostać utworzone przez Ciebie, można było korzystać w innych aplikacjach interfejsu API. Tych wymagań należą: 

Wymaganie | Opis
--- | ---
Baza danych SQL Azure | Są przechowywane elementy B2B tym partnerów, schematy, certyfikaty i agreeements. Każda z aplikacji interfejsu API B2B wymaga własnej bazy danych SQL Azure. <br/><br/>**Uwaga** Skopiuj parametry połączenia z tą bazą danych.<br/><br/>[Tworzenie bazy danych programu Azure SQL](../sql-database/sql-database-get-started.md)
Kontener magazyn obiektów Blob platformy Azure | Po włączeniu archiwizacji AS2, właściwości wiadomości sklepy. Jeśli nie potrzebujesz wiadomości AS2 archiwizacji kontenera magazynu nie jest potrzebna. <br/><br/>**Uwaga** Jeśli jest włączana, archiwizowanie, skopiuj parametry połączenia z tym magazynem obiektów Blob.<br/><br/>[Informacje o kontach Azure miejsca do magazynowania](../storage/storage-create-storage-account.md)
Usługa Bus Namespace i jego wartości klucza | Przechowuje X12 i EDIFACT tworzeniu partii danych. Jeśli nie potrzebujesz, tworzeniu partii, nazw Bus usługa nie jest potrzebna.<br/><br/>**Uwaga** Jeśli jest włączana tworzeniu partii, skopiuj następujące wartości.<br/><br/>[Tworzenie Namespace Bus usługi](http://msdn.microsoft.com/library/azure/hh690931.aspx)
Wystąpienia TPM | W przypadku wystąpienia BizTalk Trading partnera zarządzania TPM jest wymagane do utworzenia łącznik AS2 i X12 lub EDIFACT interfejsu API aplikacji. Podczas tworzenia aplikacji API TPM, tworzysz wystąpienia TPM. <br/><br/>**Uwaga** Znasz nazwy aplikacji interfejsu API TPM. 


## <a name="create-the-api-apps"></a>Tworzenie aplikacji interfejsu API
Aplikacje interfejsu API B2B można tworzyć przy użyciu Azure portal lub interfejsów API pozostałych. 


### <a name="create-the-api-apps-using-rest-apis"></a>Tworzenie aplikacji interfejsu API za pomocą interfejsów API REST
[Zapoznaj się z dokumentacją na sposób użycia interfejsów API pozostałych.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Tworzenie aplikacji interfejsu API B2B w portalu Azure
W portalu Azure można utworzyć B2B interfejsu API aplikacje podczas tworzenia aplikacji logiczny, aplikacje sieci Web lub aplikacji Mobile. Lub możesz utworzyć je przy użyciu własnej karta. Obie strony są łatwe, więc zależy preferencji dotyczących i wymagań. Niektórzy użytkownicy preferują uprzednie utworzenie wszystkie aplikacje interfejsu API B2B z ich określone właściwości. Następnie tworzyć logiki aplikacji i aplikacje sieci Web Apps i Mobile i dodawanie aplikacji interfejsu API B2B została utworzona.  

Poniższe czynności tworzenie aplikacji interfejsu API B2B za pomocą karta interfejsu API aplikacje.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>Tworzenie BizTalk Trading aplikacje interfejsu API zarządzania (TPM) partnera

> [AZURE.NOTE] W przypadku wystąpienia BizTalk Trading partnera zarządzania TPM jest wymagane do utworzenia łącznik AS2 i X12 lub EDIFACT interfejsu API aplikacji. Podczas tworzenia aplikacji API TPM, tworzysz wystąpienia TPM.

Poniższe kroki utworzenie wystąpienia TPM:

1. W portalu Azure Startboard (strona główna) wybierz pozycję **Marketplace**. **Aplikacje interfejsu API** zawiera listę wszystkich istniejących aplikacji interfejsu API i łączników. Można również **wyszukiwania** dla określonej aplikacji interfejsu API B2B.
2. Wybierz pozycję **BizTalk Trading zarządzania partnera**. W nowej karta wybierz pozycję **Utwórz**. 
3. Wprowadź właściwości: 

    Właściwość | Opis
--- | ---
Nazwa | Wpisz dowolną nazwę wystąpienia TPM. Na przykład nazwie *AccountsPayableTPM*.
Ustawienia dotyczące pakietu | Wprowadź ADO.NET **Parametry połączenia bazy danych** do bazy danych SQL Azure została utworzona. <br/><br/>Skopiuj parametry połączenia, hasło nie jest dodawany do parametry połączenia. Należy wprowadzić hasło w parametrach połączenia.
Plan usług aplikacji | Lista plan płatności. Jeśli potrzebujesz więcej lub mniej zasobów można ją zmienić.
Cennik warstwy | Właściwość tylko do odczytu, zawierająca listę kategorii cen w ramach subskrypcji usługi Azure. 
Grupa zasobów | Utwórz nowy lub użyj istniejącej grupy. Wszystkie aplikacje interfejsu API i łączników logiki aplikacji, aplikacje sieci Web i aplikacji Mobile muszą być w tej samej grupy zasobów. <br/><br/>[Korzystanie z grup zasobów](../azure-resource-manager/resource-group-overview.md) wyjaśniono tej właściwości. 
Subskrypcji | Właściwość tylko do odczytu, która zawiera listę bieżącej subskrypcji.
Lokalizacja | Położenie geograficzne obsługującego usługi Azure. 
Dodawanie do Startboard | Zaznacz tę opcję, aby dodać aplikację B2B interfejsu API do do prawej burty (strona główna).

4. Wybierz polecenie **Utwórz**. 

Po utworzeniu aplikacji interfejsu API TPM (TPM wystąpienia), następnie można utworzyć łącznik AS2 i/lub X12 lub EDIFACT interfejsu API aplikacji. 


#### <a name="create-the-as2-connector"></a>Tworzenie łącznika AS2

1. W portalu Azure Startboard (strona główna) wybierz pozycję **Marketplace**. **Aplikacje interfejsu API** zawiera listę wszystkich istniejących aplikacji interfejsu API i łączników. Można również **wyszukiwania** dla określonej aplikacji interfejsu API B2B.
2. Zaznacz **Łącznik AS2**. W nowej karta wybierz pozycję **Utwórz**. 
3. Wprowadź właściwości: 

    Właściwość | Opis
--- | ---
Nazwa | Wpisz dowolną nazwę łącznika AS2. Na przykład nazwie *AS2Connector*.
Ustawienia dotyczące pakietu | Wprowadź ustawienia określone do tej aplikacji interfejsu API, takie jak nazwa wystąpienia TPM. <br/><br/>Zobacz [Dodawanie ustawienia pakietu AS2](#AddAS2Conn) w tym temacie dla określonej właściwości. 
Plan usług aplikacji | Lista planu płatności. Jeśli potrzebujesz więcej lub mniej zasobów można ją zmienić.
Cennik warstwy | Właściwość tylko do odczytu, zawierająca listę kategorii cen w ramach subskrypcji usługi Azure. 
Grupa zasobów | Utwórz nowy lub użyj istniejącej grupy. [Korzystanie z grup zasobów](../azure-resource-manager/resource-group-overview.md) wyjaśniono tej właściwości. 
Subskrypcji | Właściwość tylko do odczytu, która zawiera listę bieżącej subskrypcji.
Lokalizacja | Położenie geograficzne obsługującego usługi Azure. 
Dodawanie do Startboard | Zaznacz tę opcję, aby dodać aplikację B2B interfejsu API do do prawej burty (strona główna).

    **<a name="AddAS2Conn"></a>Łącznik AS2 ustawienia dotyczące pakietu**

    Właściwość | Opis
--- | --- 
Parametry połączenia bazy danych | Wprowadź ADO.NET parametry połączenia z bazą danych SQL Azure została utworzona. Skopiuj parametry połączenia, hasło nie jest dodawany do parametry połączenia. Należy wprowadzić hasło w parametrach połączenia przed wklejeniem.
Włączanie archiwizacji dla wiadomości przychodzących | Opcjonalnie. Włączenie tej właściwości do przechowywania właściwości wiadomości przychodzących wiadomości AS2 otrzymane od partnera. 
Parametry połączenia magazyn obiektów Blob platformy Azure  | Wprowadź parametry połączenia w kontenerze magazyn obiektów Blob platformy Azure utworzony. Po włączeniu archiwizacji zakodowany i zdekodowaną wiadomości są przechowywane w tym kontenerze miejsca do magazynowania.
Nazwa wystąpienia TPM | Wprowadź nazwę **Zarządzania Trading partnera BizTalk** aplikacji interfejsu API wcześniej utworzone. Po utworzeniu łącznika AS2 ten łącznik wykonuje tylko umów AS2 w ramach tego konkretnego wystąpienia TPM.

4. Wybierz polecenie **Utwórz**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>Tworzenie aplikacji interfejsu API X12 lub EDIFACT

1. W portalu Azure Startboard (strona główna) wybierz pozycję **Marketplace**. **Aplikacje interfejsu API** zawiera listę wszystkich istniejących aplikacji interfejsu API i łączników. Można również **wyszukiwania** dla określonej aplikacji interfejsu API B2B.
2. Wybierz pozycję **BizTalk X 12** lub **BizTalk EDIFACT**. W nowej karta wybierz pozycję **Utwórz**. 
3. Wprowadź właściwości: 

    Właściwość | Opis
--- | ---
Nazwa | Wprowadź nazwę dowolnej aplikacji interfejsu API B2B. Na przykład nazwie *EDI850APIApp*.
Ustawienia dotyczące pakietu | Wprowadź ustawienia określone do tej aplikacji interfejsu API, takie jak nazwa wystąpienia TPM. <br/><br/>Zobacz [X12 lub ustawienia pakietu EDIFACT](#AddX12) w tym temacie dla określonej właściwości. 
Plan usług aplikacji | Lista planu płatności. Jeśli potrzebujesz więcej lub mniej zasobów można ją zmienić.
Cennik warstwy | Właściwość tylko do odczytu, zawierająca listę kategorii cen w ramach subskrypcji usługi Azure. 
Grupa zasobów | Utwórz nowy lub użyj istniejącej grupy. [Korzystanie z grup zasobów](../azure-resource-manager/resource-group-overview.md) wyjaśniono tej właściwości. 
Subskrypcji | Właściwość tylko do odczytu, która zawiera listę bieżącej subskrypcji.
Lokalizacja | Położenie geograficzne obsługującego usługi Azure. 
Dodawanie do Startboard | Zaznacz tę opcję, aby dodać aplikację B2B interfejsu API do do prawej burty (strona główna).

    **<a name="AddX12"></a>Ustawienia dotyczące pakietu aplikacji interfejsu API X12 i EDIFACT**  

    Właściwość | Opis
--- | --- 
Parametry połączenia bazy danych | Wprowadź ADO.NET parametry połączenia z bazą danych SQL Azure została utworzona. Skopiuj parametry połączenia, hasło nie jest dodawany do parametry połączenia. Należy wprowadzić hasło w parametrach połączenia przed wklejeniem.
Usługa Bus Namespace | Wprowadzanie nazw usługi Bus, utworzony. Wymagane tylko wtedy, gdy tworzeniu partii jest włączona. 
Nazwa udostępnionego klucza dostępu usługi Bus Namespace | Wprowadzanie nazw usługi Bus klawisz dostępu została utworzona. Wymagane tylko wtedy, gdy tworzeniu partii jest włączona. 
Wartość udostępnionego klucza dostępu usługi Bus Namespace | Wprowadzanie nazw usługi Bus wartość klawisz dostępu, która została utworzona. Wymagane tylko wtedy, gdy tworzeniu partii jest włączona. 
Nazwa wystąpienia TPM | Wprowadź nazwę **Zarządzania Trading partnera BizTalk** aplikacji interfejsu API wcześniej utworzone. Po utworzeniu X12 lub aplikacji interfejsu API EDIFACT tej aplikacji interfejsu API wykonuje tylko umów X12/EDFIACT w ramach tego konkretnego wystąpienia TPM.

4. Wybierz polecenie **Utwórz**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Dodawanie partnerów, umów, certyfikaty i schematów 
W portalu usługi Azure Otwórz aplikację interfejsu API TPM. W sekcji **składniki** Dodaj partnerów, umów, certyfikaty i schematów. 

Można również dodać umów do łączników AS2, X12 aplikacje interfejsu API i aplikacje interfejsu API EDIFACT. 


## <a name="monitor-your-api-apps"></a>Monitorowanie aplikacji interfejsu API
W portalu usługi Azure Otwórz aplikację interfejsu API TPM. W sekcji **operacji** można wyświetlić zarządzania różnych operacji. Na przykład można wykonywać następujące czynności:

- Zdarzenia informacyjne widoku i błędów
- Wyświetlanie użycia i wątku liczba pamięci procesu roboczego (w3wp)
- Wyświetlanie dzienników serwera sieci web i aplikacji

W [monitorze aplikacji logiczny](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Dodawanie aplikacji interfejsu API do aplikacji 
Usługa aplikacji Microsoft Azure udostępnia typy innej aplikacji, które mogą zawierać te aplikacje interfejsu API B2B. Można utworzyć nową lub dodać istniejącej aplikacji interfejsu API B2B logiki aplikacji, aplikacje Mobile lub aplikacji sieci Web. 

W aplikacji po prostu zaznaczenie aplikacje interfejsu API B2B z galerii automatycznie dodaje ją do aplikacji.  

> [AZURE.IMPORTANT] Aby dodać łączniki i aplikacje interfejsu API poprzednio utworzone, Utwórz logiki aplikacji, aplikacje Mobile lub aplikacji sieci Web w tej samej grupy zasobów. 

Poniższe kroki dodać aplikacje interfejsu API B2B do aplikacji Mobile, aplikacji sieci Web lub logiczny aplikacji: 

1. W portalu Azure Startboard (strona główna) przejdź do **sklepu Office**i wyszukaj logiczny, telefon komórkowy lub aplikacji sieci Web. 

    W przypadku tworzenia nowej aplikacji, wyszukaj aplikacji Mobile, aplikacji sieci Web lub aplikacji logiczny. Wybierz aplikację, a w nowej karta, wybierz pozycję **Utwórz**. [Tworzenie aplikacji logika](app-service-logic-create-a-logic-app.md) przedstawiono kroki. 

2. Otwórz aplikację i wybierz pozycję **Wyzwalacze i akcje**. 

3. W **galerii**wybierz aplikację interfejsu API B2B automatycznie dodaje do aplikacji. Można także tworzyć nowej aplikacji interfejsu API B2B.

    > [AZURE.IMPORTANT] Łącznik AS2 i X12 EDIFACT interfejsu API aplikacje wymagają wystąpienie TPM. Tak, jeśli tworzysz nowe aplikacje interfejsu API B2B, najpierw utworzyć aplikację interfejsu API TPM, a następnie Utwórz łącznik AS2, X12 interfejsu API aplikacji lub EDIFACT interfejsu API aplikacji. 

4. Wybierz **przycisk OK** , aby zapisać zmiany. 

>[AZURE.NOTE] Aby rozpocząć pracę z Azure logiki aplikacji przed utworzeniem konta dla konta Azure, [Spróbuj logiczny aplikacji](https://tryappservice.azure.com/?appservice=logic). Możesz od razu utworzyć aplikację logiki krótkotrwałe starter. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="more-b2b-resources"></a>Więcej zasobów B2B

[Tworzenie procesu B2B](app-service-logic-create-a-b2b-process.md)<br/>
[Tworzenie umowę partnerów handlowych](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Co to są łączników i BizTalk interfejsu API aplikacje](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Informacje o aplikacji logiki i aplikacjach sieci Web
[Co to są logiczny aplikacje?](app-service-logic-what-are-logic-apps.md)<br/>
[Witryny internetowe i aplikacje sieci Web w usłudze Azure aplikacji](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Więcej łączników

[Łączniki i interfejsu API listy aplikacji](app-service-logic-connectors-list.md)<br/><br/>
[Co to są łączników i BizTalk interfejsu API aplikacje](app-service-logic-what-are-biztalk-api-apps.md) 
