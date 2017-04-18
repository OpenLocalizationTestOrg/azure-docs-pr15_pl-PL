<properties
    pageTitle="Rozpoczynanie pracy z wykazem danych | Microsoft Azure"
    description="Samouczek zakończenia do końca prezentowanie scenariusze i możliwości wykazu danych Azure."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Rozpoczynanie pracy z wykazem danych Azure
Wykaz danych Azure to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania trwałych danych przedsiębiorstwa. Aby uzyskać szczegółowe omówienie zobacz [Co to jest Azure wykaz danych](data-catalog-what-is-data-catalog.md).

Ten samouczek ułatwia rozpoczęcie pracy z wykazem danych Azure. W ramach tego samouczka należy wykonać następujące procedury:

| Procedura | Opis |
| :--- | :---------- |
| [Obsługa administracyjna wykaz danych](#provision-data-catalog) | W tej procedurze obsługi administracyjnej lub ustawienie Azure wykazu danych. Możesz wykonać ten krok, tylko wtedy, gdy katalogu nie została ustawiona przed. Możesz mieć tylko jeden wykazu danych dla organizacji (Microsoft Azure Active Directory domena), nawet jeśli istnieje wiele subskrypcji skojarzone z kontem usługi Azure. |
| [Zarejestruj się w danych składników majątku](#register-data-assets) | W tej procedurze rejestrowania danych składniki majątku z przykładowej bazy danych AdventureWorks2014 dzięki wykazowi danych. Rejestracja jest proces Wyodrębnianie klucza strukturalnych metadanych, takie jak nazwy, typy i lokalizacje ze źródła danych i kopiowanie metadane do katalogu. Źródła danych i aktywa dane pozostaną miejsce, w którym są one, ale metadanych jest używany przez wykaz aby były łatwo odnajdowania i zrozumienia. |
| [Poznawanie danych składników majątku](#discover-data-assets) | W tej procedury można użyć portal Azure wykazu danych do znalezienia aktywów danych, które zostały zarejestrowane w poprzednim kroku. Po zarejestrowaniu źródła danych dzięki wykazowi danych Azure metadanych jest indeksowane przez usługę, tak, aby użytkownicy mogą łatwo wyszukiwać dane, które są potrzebne. |
| [Dodawanie adnotacji danych składników majątku](#annotate-data-assets) | W tej procedurze podasz adnotacje (informacje, takie jak opisy, znaczniki, dokumentacji lub ekspertów) trwałych danych. Ta informacja uzupełnia metadanych wyodrębnionych ze źródła danych, a aby wprowadzić bardziej zrozumiałe źródła danych więcej osób. |
| [Nawiązywanie połączenia z danych składników majątku](#connect-to-data-assets) | W tej procedurze możesz otworzyć danych składniki majątku w narzędziach klienta zintegrowane (takie jak programy Excel i SQL Server Data Tools) i narzędzia zintegrowanego (program SQL Server Management Studio). |
| [Zarządzanie danych składników majątku](#manage-data-assets) | W tej procedurze możesz skonfigurować zabezpieczeń trwałych danych. Wykaz danych nie udzielanie dostępu do samych danych. Właściciel źródła danych kontroluje dostęp do danych. <br/><br/> Wykaz danych można odkryć źródeł danych i wyświetlanie **metadanych** związanych z źródła zarejestrowane w katalogu. Czasami może występować sytuacjach, jednak źródeł danych powinien być widoczny tylko dla określonych użytkowników i członkowie określonych grup. Za pomocą wykazu danych przejmowanie praw własności aktywów danych zarejestrowanych w wykazie i kontrolować widoczność środków trwałych, których jesteś właścicielem, w przypadku następujących scenariuszy. |
| [Usuwanie danych zasobów](#remove-data-assets) | W tej procedurze się, jak usunąć składniki majątku danych z wykazu danych. |  

## <a name="tutorial-prerequisites"></a>Samouczki wymagania wstępne

### <a name="azure-subscription"></a>Azure subskrypcji
Aby skonfigurować wykaz danych Azure, musi być właścicielem lub współwłaściciela Azure subskrypcji.

Subskrypcje Azure ułatwiają organizowanie dostęp do chmury usługi zasoby takie jak Azure wykazu danych. One również pomocy kontrolowanie, jak zgłoszone jest użycie zasobu, dotyczy rozliczenie i opłacona. Każdej subskrypcji może mieć różne ustawienia rozliczeń i płatności, więc musisz różnych subskrypcjach i różnych planów przez dział, project, biuro regionalne i tak dalej. Co usługa w chmurze należy do subskrypcji, a musisz mieć subskrypcję przed rozpoczęciem konfigurowania Azure wykazu danych. Aby uzyskać więcej informacji, zobacz [Zarządzanie kontami, subskrypcji i role administracyjne](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Jeśli nie masz subskrypcji, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="azure-active-directory"></a>Azure Active Directory
Aby skonfigurować wykaz danych Azure, muszą być zalogowani przy użyciu konta usługi Azure Active Directory (Azure AD). Musisz być właścicielem lub współwłaściciela Azure subskrypcji.  

Azure AD umożliwia łatwe dla swojej firmy, tożsamości i dostęp zarówno w chmurze i lokalnych do zarządzania. Za pomocą pojedynczego służbowego lub konta służbowego do zalogowania się do dowolnej aplikacji sieci web w chmurze lub lokalnego. Wykaz danych Azure używa Azure AD do uwierzytelniania logowania. Aby uzyskać więcej informacji, zobacz [Co to jest Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Konfiguracja zasad usługi Azure Active Directory

Mogą wystąpić sytuacja, gdzie można zalogować się do portalu Azure wykazu danych, ale przy próbie Zaloguj się do narzędzia rejestracji źródła danych, napotykasz komunikat o błędzie, który uniemożliwia logowanie. Ten błąd może wystąpić, gdy korzystasz z sieci firmowej lub podczas nawiązywania połączenia z spoza sieci firmowej.

Narzędzie do rejestracji użyto *uwierzytelniania formularzy* , aby sprawdzić poprawność dodatki logowania użytkownika przed usługi Azure Active Directory. Dla pomyślnego logowania administrator usługi Azure Active Directory należy włączyć uwierzytelnianie formularzy w *zasady globalnej uwierzytelniania*.

Z możliwością globalnej uwierzytelniania możesz włączyć uwierzytelnianie osobno dla sieci intranet i ekstranetu połączeń, jak pokazano na poniższej ilustracji. Błędy logowania może wystąpić, jeśli nie włączono uwierzytelnianie formularzy dla sieci, z której nawiązujesz połączenie.

 ![Azure zasady globalnej uwierzytelniania usługi Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Aby uzyskać więcej informacji zobacz [Konfigurowanie uwierzytelniania zasady](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Obsługa administracyjna wykazu danych
Można dodawać tylko jeden wykaz danych dla organizacji (domena usługi Azure Active Directory). W związku z tym jeśli właściciel lub współwłaściciela subskrypcji usługi Azure należący do tej domeny usługi Azure Active Directory został już utworzony wykaz, nie będzie mógł tworzyć wykaz ponownie, nawet jeśli masz wiele subskrypcji Azure. Aby sprawdzić, czy wykaz danych została utworzona przez użytkownika w domenie usługi Azure Active Directory, przejdź do [strony głównej wykazu danych usługi Azure](http://azuredatacatalog.com) i sprawdź, czy możesz zobaczyć wykazu. Jeśli utworzono już wykaz dla Ciebie, Pomiń poniższej procedury i przejdź do następnej sekcji.    

1. Przejdź do [strony usługi wykazu danych](https://azure.microsoft.com/services/data-catalog) , a następnie kliknij pozycję **wprowadzenie**.

    ![Azure wykazu danych — strona główna marketingowych](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Zaloguj się przy użyciu konta użytkownika, które jest właściciel lub współwłaściciela Azure subskrypcji. Zostanie wyświetlona strona następujących po zalogowaniu się.

    ![Azure wykazu danych — Obsługa administracyjna wykazu danych](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Określ **nazwę** dla wykazu danych, z **subskrypcji** , której chcesz użyć i **lokalizacji** katalogu.
4. Rozwiń **ceny** i wybierz wykaz danych Azure **edition** (wolny lub standardowe).
    ![Azure wykazu danych — wybierz edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Rozwiń **Wykaz użytkowników** i kliknij przycisk **Dodaj** , aby dodać użytkowników, którzy wykazu danych. Są automatycznie dodawane do tej grupy.
    ![Wykaz danych Azure — użytkowników](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Rozwiń **Wykaz administratorów** i kliknij przycisk **Dodaj** , aby dodać dodatkowych administratorów dla wykazu danych. Są automatycznie dodawane do tej grupy.
    ![Wykaz danych Azure — Administratorzy](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Kliknij pozycję **Utwórz wykaz** tworzenie wykazu danych dla Twojej organizacji. Zostanie wyświetlona strona główna dla wykazu danych po jego utworzeniu.
    ![Wykaz danych Azure — Relacja utworzona](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Znajdowanie wykazu danych w portalu Azure
1. Na osobnej karcie w przeglądarce sieci web lub w sieci web w osobnym oknie przeglądarki przejdź do [portalu Azure](https://portal.azure.com) i zaloguj się przy użyciu tego samego konta, którego użyto do utworzenia wykazu danych w poprzednim kroku.
2. Wybierz pozycję **Przeglądaj** , a następnie kliknij pozycję **Wykazu danych**.

    ![Azure wykazu danych — przeglądanie Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) Zobacz wykazu danych została utworzona.

    ![Azure wykazu danych — wykazu widoku listy](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Kliknij pozycję wykaz, który został utworzony. Zobacz Karta **Wykazu danych** w portalu.

    ![Azure wykazu danych — karta w portalu ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Można wyświetlić właściwości wykaz danych i zaktualizuj je. Na przykład kliknij **poziom ceny** i zmień wersji.

    ![Azure wykazu danych — ceny warstwy](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works przykładowej bazy danych
W tym samouczku rejestrowania danych składników majątku (tabel) z przykładowej bazy danych AdventureWorks2014 dla aparatu bazy danych programu SQL Server, ale możesz użyć dowolnego źródła danych obsługiwane, jeśli chcesz pracować z danymi, które jest znany i istotne dla danej roli. Aby uzyskać listę obsługiwanych źródeł danych zobacz [obsługiwane źródła danych](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Instalowanie Adventure Works 2014 OLTP bazy danych
Baza danych Adventure Works obsługuje standard online scenariuszy przetwarzanie transakcji dla producenta fikcyjny rowerowy (Adventure Works cykli), które zawiera produktów, sprzedaży i zakupu. W tym samouczku rejestrowania informacji o produktach do wykazu danych Azure.

Aby zainstalować bazy danych przykładowej firmy Adventure Works:

1. Pobierz [przygodowy Works 2014 pełnej bazy danych Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) w witrynie CodePlex.
2. Aby przywrócić bazę danych na komputerze, postępuj zgodnie z instrukcjami w [kopii zapasowej bazy danych przy użyciu programu SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx)lub, wykonując następujące czynności:
    1. Otwórz program SQL Server Management Studio i nawiązywanie połączenia z aparatu bazy danych programu SQL Server.
    2. Kliknij prawym przyciskiem myszy **baz danych** , a następnie kliknij pozycję **Przywróć bazę danych**.
    3. W obszarze **Przywróć bazę danych**kliknij opcję **urządzenia** **źródła** , a następnie kliknij przycisk **Przeglądaj**.
    4. W obszarze, **Zaznacz urządzeń kopii zapasowej**kliknij przycisk **Dodaj**.
    5. Przejdź do folderu, w której możesz plik **AdventureWorks2014.bak** , wybierz plik i kliknij **przycisk OK** , aby zamknąć okno dialogowe **Odszukaj plik kopii zapasowej** .
    6. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Wybieranie urządzeń kopii zapasowej** .    
    7. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Przywróć bazę danych** .

Możesz teraz zarejestrować aktywów danych z bazy danych przykładowej firmy Adventure Works przy użyciu Azure wykazu danych.

## <a name="register-data-assets"></a>Zarejestruj się w danych składników majątku

W tym wykonywania umożliwia narzędzie do rejestracji zarejestrować aktywów danych z bazy danych Adventure Works w katalogu. Rejestracja jest proces Wyodrębnianie klucza strukturalnych metadanych, takie jak nazw, typów i lokalizacji ze źródłem danych i zasoby, które zawiera i kopiowanie metadane do katalogu. Źródła danych i aktywa dane pozostaną miejsce, w którym są one, ale metadanych jest używany przez wykaz aby były łatwo odnajdowania i zrozumienia.

### <a name="register-a-data-source"></a>Zarejestruj się źródła danych

1.  Przejdź do [strony głównej Azure wykazu danych](https://azuredatacatalog.com) , a następnie kliknij przycisk **Publikuj dane**.

    ![Wykaz danych Azure — publikowanie danych przycisk](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Kliknij przycisk **Uruchom aplikację** na pobieranie, instalowanie i uruchamianie narzędzia rejestracji na komputerze.

    ![Wykaz danych Azure — przycisk Uruchom](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Na stronie **powitalnej** kliknij przycisk **Zaloguj** i wprowadź poświadczenia.    

    ![Azure wykazu danych — strona powitalna](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Na stronie **Wykazu danych usługi Microsoft Azure** kliknij **Programu SQL Server** i **Następny**.

    ![Azure wykazu danych — źródeł danych](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Wprowadzanie właściwości połączenia programu SQL Server dla **AdventureWorks2014** (Zobacz przykład poniżej) i kliknij przycisk **POŁĄCZ**.

    ![Azure wykazu danych — ustawienia połączenia programu SQL Server](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Zarejestruj się w metadanych do środka danych. W tym przykładzie rejestrowania obiektów **Produkcji/produktu** z nazw AdventureWorks produkcji:

    1. W drzewie **Hierarchii Server** rozwiń **AdventureWorks2014** i kliknij **produkcji**.
    2. Wybierz pozycję **produktu**, **ProductCategory**, **ProductDescription**i **ProductPhoto** za pomocą klawiszy Ctrl + kliknięcie.
    3. Kliknij przycisk **przeniesienie zaznaczonego strzałkę** (**>**). Ta akcja powoduje przeniesienie wszystkich zaznaczonych obiektów na liście **obiektów, które mają zostać zarejestrowane** .

        ![Azure samouczek wykazu danych — przeglądanie i Zaznaczanie obiektów](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Wybierz pozycję **Dołącz Podgląd** , aby dołączyć Podgląd migawek danych. Migawka zawiera maksymalnie 20 rekordy z każdej tabeli i jest kopiowana do wykazu.
    5. Wybranie **Profilu danych zawiera** obejmują migawkę statystyki obiekt profilu danych (na przykład: minimalną, maksymalną i średniej wartości dla kolumny, liczba wierszy).
    6. W polu **Dodawanie znaczników** wprowadź **adventure works cykli**. Spowoduje to dodanie znaczników wyszukiwania dla tych zasobów danych. Znaczniki to doskonały sposób, aby ułatwić użytkownikom znajdowanie źródła danych zarejestrowane.
    7. Określ nazwę **ekspertów** na tych danych (opcjonalnie).

        ![Azure samouczek wykaz danych — obiekty rejestracji](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Kliknij przycisk **ZAREJESTRUJ**. Wykaz danych Azure rejestruje z zaznaczonych obiektów. W tym wykonywania są rejestrowane zaznaczonych obiektów z Adventure Works. Narzędzie do rejestracji wyodrębnia metadane z zasobów danych i kopiuje te dane do usługi Azure wykazu danych. Dane pozostaną miejsce, w którym obecnie znajduje się i pozostaje ona w obszarze Sterowanie administratorów i zasad bieżącego systemu.

        ![Azure wykazu danych — zarejestrowanych obiektów](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Aby wyświetlić obiekty źródła danych zarejestrowane, kliknij **Widok portalu**. W portal Azure wykazu danych upewnij się, czy jest widoczny wszystkie cztery tabele i bazy danych w widoku siatki.

        ![Obiekty w portalu wykazu danych Azure ](media/data-catalog-get-started/data-catalog-view-portal.png)


W tym wykonywania zarejestrowany obiekty z bazy danych przykładowej firmy Adventure Works tak, aby łatwo ich wykryte przez użytkowników w organizacji. Do wykonywania następnego możesz dowiedzieć się, jak do znalezienia aktywów zarejestrowane dane.

## <a name="discover-data-assets"></a>Poznawanie danych składników majątku
Odnajdowanie w wykazie danych Azure używa dwa podstawowe mechanizmy: wyszukiwanie i filtrowanie.

Wyszukiwanie ma być intuicyjny i zaawansowanych. Domyślnie wyszukiwane terminy są dopasowywane dowolnej właściwości w wykazie, łącznie z adnotacjami użytkownika.

Filtrowanie jest przeznaczony do uzupełnienia wyszukiwania. Możesz wybrać określonych cech, takich jak ekspertów, typ źródła danych, typ obiektu i znaczników do wyświetlania aktywów dopasowanych danych oraz ograniczyć wyniki wyszukiwania do aktywa dopasowane.

Przy użyciu kombinacji wyszukiwania i filtrowania, możesz szybko przejść źródła danych, które zostały zarejestrowane w Azure wykazu danych, aby odkryć aktywów danych, które są potrzebne.

W tym wykonywania portal Azure wykaz danych służy do wykrywania trwałe danych, które zarejestrowane w poprzednim wykonywania. Zobacz [informacje dotyczące składni wyszukiwanie w wykazie danych](https://msdn.microsoft.com/library/azure/mt267594.aspx) , aby uzyskać szczegółowe informacje o składni wyszukiwania.

Poniżej przedstawiono kilka przykładów za odkrycie danych składników majątku w katalogu.  

### <a name="discover-data-assets-with-basic-search"></a>Poznawanie zasobami danych za pomocą podstawowego wyszukiwania
Podstawowe wyszukiwanie ułatwia wyszukiwanie wykaz przy użyciu wyszukiwanych terminów. Wyniki są wszelkich aktywów, które są zgodne z dowolnej właściwości z jedną lub więcej z warunkami określonymi.

1. Kliknij kartę **Narzędzia główne** , w portalu wykaz danych Azure. Po zamknięciu przeglądarki sieci web, przejdź do [strony głównej Azure wykazu danych](https://www.azuredatacatalog.com).
2. W polu wyszukiwania wpisz `cycles` i naciśnij klawisz **ENTER**.

    ![Azure wykazu danych — wyszukiwanie tekst podstawowy](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Upewnij się, że widoczne wszystkie cztery tabele i bazy danych (AdventureWorks2014) w wynikach. Możesz przełączać się między **widoku siatki** i **widoku listy** , klikając przyciski na pasku narzędzi, jak pokazano na poniższej ilustracji. Zwróć uwagę, że słowo kluczowe wyszukiwania jest wyróżniona w wynikach wyszukiwania, ponieważ **włączona**jest opcja **wyróżnienia** . Można również określić liczbę **wyników na stronie** , w wynikach wyszukiwania.

    ![Azure wykazu danych — wyniki wyszukiwania tekst podstawowy](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    Panel **Wyszukiwanie** jest po lewej stronie i panelu **Właściwości** jest po prawej stronie. W panelu **wyszukiwania** można zmienić kryteria wyszukiwania i filtrować wyniki. Panel **Właściwości** Wyświetla właściwości zaznaczonego obiektu w siatkę lub listę.

4. Kliknij **produkt** w wynikach wyszukiwania. Kliknij **Podgląd**, **kolumny**, **Danych profilu**i **dokumentacji** karty, lub kliknij strzałkę, aby rozwinąć dolnym okienku.  

    ![Wykaz danych Azure — dolnym okienku](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    Na karcie **Podgląd** można zobaczyć podgląd danych w tabeli **Product** .  
5. Kliknij kartę **kolumny** , aby znaleźć szczegółowe informacje o kolumnach (takie jak **Nazwa** i **Typ danych**) w zawartości danych.
6. Kliknij kartę **Profilu danych** , aby wyświetlić profilowanie danych (na przykład: liczba wierszy, rozmiar danych lub minimalną wartość w kolumnie) w zawartości danych.
7. Umożliwia filtrowanie wyników przy użyciu **filtrów** po lewej stronie. Na przykład kliknij **tabelę** **Typ obiektu**, a zostaną wyświetlone tylko tabele cztery, a nie bazy danych.

    ![Azure wykaz danych — filtrowanie wyników wyszukiwania](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Poznawanie zasobami danych za pomocą właściwości zakresu
Określanie zakresu właściwość ułatwia odnajdywanie miejsce, w którym wyszukiwany termin jest dopasowany do określonej właściwości elementów danych.

1. Wyczyść filtr **tabeli** w obszarze **Typ obiektu** w **filtrach**.  
2. W polu wyszukiwania wpisz `tags:cycles` i naciśnij klawisz **ENTER**. Zobacz [informacje dotyczące składni wyszukiwanie w wykazie danych](https://msdn.microsoft.com/library/azure/mt267594.aspx) wszystkich właściwości, które umożliwiają wyszukiwanie w wykazie danych.
3. Upewnij się, że widoczne wszystkie cztery tabele i bazy danych (AdventureWorks2014) w wynikach.  

    ![Wykaz danych — właściwości zakresu wyników wyszukiwania](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Zapisywanie wyszukiwania
1. W okienku **Wyszukiwanie** w sekcji **Bieżącego wyszukiwania** wprowadź nazwę funkcji wyszukiwania, a następnie kliknij przycisk **Zapisz**.

    ![Azure wykaz danych — zapisywanie wyszukiwania](media/data-catalog-get-started/data-catalog-save-search.png)
2. Upewnij się, zapisane wyszukiwanie widoczne w obszarze **Zapisane wyszukiwania**.

    ![Azure wykaz danych — zapisane wyszukiwania](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Wybierz jeden z akcje, które można wykonać na zapisanego wyszukiwania (**Zmienianie nazwy**, **Usuwanie**, **Zapisz jako domyślne** wyszukiwania).

    ![Azure wykaz danych — zapisane opcje wyszukiwania](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Operatory logiczne
Można rozszerzyć lub zawęzić kryteria wyszukiwania z operatorów logicznych.

1. W polu wyszukiwania wpisz `tags:cycles AND objectType:table`, a następnie naciśnij klawisz **ENTER**.
2. Upewnij się, że zostaną wyświetlone tylko tabele (nie bazy danych) w wynikach.  

    ![Wykaz danych Azure — operatorów logicznych w wyszukiwaniu](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Grupowanie w nawiasach
Grupując w nawiasy, można grupować części kwerendy w celu uzyskania logiczne oddzielnie, zwłaszcza wraz z operatorów logicznych.

1. W polu wyszukiwania wpisz `name:product AND (tags:cycles AND objectType:table)` i naciśnij klawisz **ENTER**.
2. Upewnij się, że jest wyświetlany tylko tabelę **produktów** w wynikach wyszukiwania.

    ![Wykaz danych Azure — wyszukiwanie grupowania](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Operatory porównania
Operatory porównania można użyć porównania innych niż równości dla właściwości, które mają typów danych liczbowych i Data.

1. W polu wyszukiwania wpisz `lastRegisteredTime:>"06/09/2016"`.
2. Wyczyść filtr **tabeli** w obszarze **Typ obiektu**.
3. Naciśnij klawisz **ENTER**.
4. Upewnij się, czy jest widoczny w tabelach **produktu**, **ProductCategory**, **ProductDescription**i **ProductPhoto** i AdventureWorks2014 bazę danych, z którą zarejestrowane w wynikach wyszukiwania.

    ![Wykaz danych Azure — porównanie wyników wyszukiwania](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Zobacz, [jak do znalezienia danych składniki majątku](data-catalog-how-to-discover.md) uzyskać szczegółowe informacje na temat odkrywania danych składniki majątku i [Dokumentacja składni wyszukiwanie w wykazie danych](https://msdn.microsoft.com/library/azure/mt267594.aspx) dla składni wyszukiwania.

## <a name="annotate-data-assets"></a>Dodawanie adnotacji danych składników majątku
W tym wykonywania portal Azure wykazu danych służy do adnotacji (Dodaj informacje, takie jak opisy, znaczniki lub ekspertów) danych składniki majątku wcześniej zarejestrowany w katalogu. Adnotacje uzupełnienia wzbogacanie strukturalnych metadane wyodrębnionych ze źródła danych podczas rejestrowania i sprawia, że danych składniki majątku znacznie ułatwia odnajdowanie i opis.

W tym wykonywania Dodawanie adnotacji zbiór danych single (ProductPhoto). Możesz dodać do środka danych ProductPhoto przyjazną nazwę i opis.  

1.  Przejdź do [strony głównej wykazu danych Azure](https://www.azuredatacatalog.com) i wyszukiwania z `tags:cycles` znajdowanie danych składniki majątku zarejestrowano.  
2. Kliknij pozycję **ProductPhoto** w wynikach wyszukiwania.  
3. Wprowadź **Opis** **obrazów produktów** **Przyjazną nazwę** i **fotografii produktu dla materiałów marketingowych** .

    ![Azure wykazu danych — opis ProductPhoto](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **Opis** pomaga osobom odnaleźć i zrozumieć, dlaczego i jak używać zawartości zaznaczonych danych. Możesz również dodać więcej znaczników i wyświetlać kolumny. Teraz możesz wypróbować wyszukiwanie i filtrowanie w celu wykrywania składników majątku danych przy użyciu opisową metadanych, które zostały dodane do katalogu.

Możesz również wykonać poniższe czynności na tej stronie:

- Dodawanie ekspertów trwałego danych. Kliknij przycisk **Dodaj** w obszarze **ekspertów** .
- Dodawanie znaczników na poziomie zestawu danych. Kliknij przycisk **Dodaj** w obszarze **znaczniki** . Znacznik może być znacznika użytkownika lub znacznika słownik. Standard Edition wykazu danych zawiera słownik firm, ułatwiające administratorom wykazu określenie taksonomii centralnej firm. Następnie wykazu użytkownicy mogą dodawać adnotacje zasobami danych za pomocą słownik. Aby uzyskać więcej informacji zobacz, [jak skonfigurować słownik firm podlega znakowania](data-catalog-how-to-business-glossary.md)
- Dodawanie znaczników na poziomie kolumny. Kliknij przycisk **Dodaj** w obszarze **znaczniki** dla kolumny, którą chcesz umieścić adnotacje.
- Dodawanie opisu na poziomie kolumny. Wprowadź **Opis** dla kolumny. Możesz także wyświetlić metadane opis wyodrębnionych ze źródła danych.
- Dodaj informacje **żądania dostępu** , które przedstawiono, jak na żądanie dostępu do elementu danych.

    ![Azure wykaz danych — Dodawanie znaczników i opisy](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Wybierz kartę **dokumentacji** i dokumentacja dotycząca elementu danych. Z dokumentacją Azure wykazu danych służy do wykazu danych jako repozytorium zawartości utworzyć pełną narracji środków trwałych danych.

    ![Azure wykazu danych — karta dokumentacji](media/data-catalog-get-started/data-catalog-documentation.png)


Można również dodać adnotację do wielu zasobów danych. Na przykład można wybrać wszystkie dane środki trwałe zarejestrowana i określić ekspertem dla nich.

![Azure wykazu danych — Dodawanie adnotacji wielu zasobów danych](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Wykaz danych Azure obsługuje źródeł tłum podejście do adnotacji. Dowolny użytkownik wykaz danych można dodać znaczniki (użytkownika lub słownik), opis i inne metadane, dzięki czemu każdy użytkownik z perspektywy na zbiór danych i jego zastosowania mają tej perspektywy przechwycony i dostępne dla innych użytkowników.

Zobacz, [jak dodawać adnotacje danych składniki majątku](data-catalog-how-to-annotate.md) szczegółowe informacje dotyczące adnotacje aktywów danych.

## <a name="connect-to-data-assets"></a>Nawiązywanie połączenia z danych składników majątku
W tym wykonywania możesz otworzyć środkami danych klienta zintegrowane narzędzia (Excel) i-zintegrowane (program SQL Server Management Studio) przy użyciu informacji o połączeniu.

> [AZURE.NOTE] Należy pamiętać, że wykaz danych Azure nie umożliwiają dostęp do źródła danych rzeczywistych — po prostu ułatwi odnajdowanie i jej opis. Podczas łączenia się ze źródłem danych, z aplikacją kliencką, wybrane używa poświadczeń systemu Windows lub monituje o podanie poświadczeń w razie potrzeby. Jeśli nie wcześniej przyznano mu dostępu do źródła danych, musisz mieć udzielony dostęp, zanim będzie można połączyć.

### <a name="connect-to-a-data-asset-from-excel"></a>Nawiązywanie połączenia z zasobów danych z programu Excel

1. Wybierz **produkt** z wyników wyszukiwania. Kliknij przycisk **Otwórz w** na pasku narzędzi, a następnie kliknij przycisk **programu Excel**.

    ![Wykaz danych Azure — nawiązywanie połączenia z zasobów danych](media/data-catalog-get-started/data-catalog-connect1.png)
2. Kliknij przycisk **Otwórz** w oknie podręcznym plik do pobrania. Ten może być różna w zależności od przeglądarki.

    ![Azure wykazu danych — pobrany plik połączenia w programie Excel](media/data-catalog-get-started/data-catalog-download-open.png)
3. W oknie **Hasło zabezpieczeń programu Microsoft Excel** kliknij przycisk **Włącz**.

    ![Wykaz danych Azure — podręcznego zabezpieczeń programu Excel](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Zachowaj wartości domyślne w oknie dialogowym **Importowanie danych** , a następnie kliknij **przycisk OK**.

    ![Azure wykaz danych — program Excel importowanie danych](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Wyświetlanie źródła danych w programie Excel.

    ![Azure wykazu danych — produktu tabeli w programie Excel](media/data-catalog-get-started/data-catalog-connect2.png)

W tym wykonywania połączony z danych składniki majątku wykryte za pomocą wykazu danych Azure. Portal Azure wykaz danych umożliwia nawiązanie połączenia bezpośrednio przy użyciu aplikacji klienckich zintegrowany z **otwartym w** menu. W przypadku nawiązywania połączenia z dowolną aplikacją, wybrane za pomocą połączenia informacje o lokalizacji, zawarte w metadanych zawartości. Na przykład SQL Server Management Studio umożliwia połączenia z bazą danych AdventureWorks2014 dostępu do danych w aktywa danych zarejestrowane w tym samouczku.

1. Otwórz **program SQL Server Management Studio**.
2. W oknie dialogowym **połączenie z serwerem** wprowadź nazwę serwera w okienku **Właściwości** w portal Azure wykazu danych.
3. Aby uzyskać dostęp do zawartości danych za pomocą uwierzytelniania odpowiednią i poświadczenia. Jeśli nie masz programu access, użyj informacji w polu **Żądanie dostępu** , aby uzyskać go.

    ![Azure wykazu danych — żądania dostępu](media/data-catalog-get-started/data-catalog-request-access.png)

Kliknij przycisk **Parametry połączenia w widoku** do wyświetlania i skopiuj parametry połączenia ADF.NET, ODBC i OLEDB do Schowka w celu użycia w aplikacji.

## <a name="manage-data-assets"></a>Zarządzanie danych składników majątku
W tym kroku zobaczysz konfigurowania zabezpieczeń trwałych danych. Wykaz danych nie udzielanie dostępu do samych danych. Właściciel źródła danych kontroluje dostęp do danych.

Za pomocą wykazu danych, aby odnaleźć źródła danych i wyświetlić metadane związane z źródła zarejestrowane w wykazie. Czasami może występować sytuacjach, jednak miejsce, w którym źródła danych należy będzie widoczny tylko do określonych użytkowników lub członkowie określonych grup. W przypadku następujących scenariuszy wykaz danych umożliwia przejmowanie praw własności aktywów zarejestrowane dane w katalogu, a następnie kontrolki widoczność aktywów własności.

> [AZURE.NOTE] Funkcje zarządzania opisane w tym wykonywania dostępnych tylko w standardowej wersji z Azure wykazie danych, a nie w bezpłatnej wersji.
W wykazie danych Azure można przejmowanie praw własności aktywów danych, dodawanie Współtworzenie właścicieli do danych składniki majątku i ustawić widoczności danych składniki majątku.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Przejmowanie praw własności aktywów danych, a także ograniczać widoczności

1. Przejdź do [strony głównej wykaz danych Azure](https://www.azuredatacatalog.com). W polu tekstowym **Wyszukaj** wprowadź `tags:cycles` i naciśnij klawisz **ENTER**.
2. Kliknij odpowiedni element na liście wyników, a następnie kliknij przycisk **Własność** na pasku narzędzi.
3. W sekcji **Zarządzanie** w panelu **Właściwości** kliknij **Własność**.

    ![Azure wykazu danych — przejmowanie praw własności](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Aby ograniczyć widoczność, wybierz pozycję **Właściciele i tych użytkowników** w sekcji **widoczność** , a następnie kliknij przycisk **Dodaj**. Wprowadź adresy e-mail użytkowników w polu tekstowym, a następnie naciśnij klawisz **ENTER**.

    ![Azure wykazu danych — ograniczenia dostępu](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Usuwanie danych zasobów

W tym wykonywania umożliwia portal Azure wykaz danych usunąć Podgląd danych z aktywów zarejestrowane dane i usuwanie danych składniki majątku z wykazu.

W wykazie danych Azure można usunąć środka trwałego poszczególnych lub usuwanie wielu zasobów.

1. Przejdź do [strony głównej wykaz danych Azure](https://www.azuredatacatalog.com).
2. W polu tekstowym **Wyszukaj** wprowadź `tags:cycles` i naciśnij klawisz **ENTER**.
3. Zaznacz element na liście wyników, a następnie kliknij polecenie **Usuń** na pasku narzędzi jak pokazano na poniższej ilustracji:

    ![Azure wykazu danych — Usuń element siatki](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Jeśli korzystasz z widoku listy, pole wyboru jest po lewej stronie elementu, jak pokazano na poniższej ilustracji:

    ![Azure wykazu danych — usuwanie elementu listy](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Możesz również zaznaczyć wielu zasobów danych i usuń je, jak pokazano na poniższej ilustracji:

    ![Azure wykaz danych — usuwanie wielu zasobów danych](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Domyślne zachowanie wykazu jest umożliwienie dowolnego użytkownika do rejestrowania każdego źródła danych i zezwolić każdemu użytkownikowi usuwanie aktywów danych, która została zarejestrowana. Funkcje zarządzania zawarte w standardowej wersji programu Azure wykazu danych zapewniają dodatkowe opcje pobierania własności aktywów ograniczanie którzy mogą one znaleźć składników majątku i ograniczanie, kto może usuwać elementy zawartości.


## <a name="summary"></a>Podsumowanie

W tym samouczku zbadane się podstawowe możliwości wykazu danych Azure, w tym rejestrowanie, adnotacje wykrywanie i zarządzanie zasobami danych przedsiębiorstwa. Teraz, gdy koniec samouczka nadszedł czas na rozpoczęcie pracy. Można rozpocząć dzisiaj przez proces rejestracji źródła danych, które możesz razem ze swoim zespołem zależne i zapraszanie współpracowników do za pomocą wykazu.

## <a name="references"></a>Odwołania

- [Rejestrowanie danych składników majątku](data-catalog-how-to-register.md)
- [Jak do znalezienia danych składników majątku](data-catalog-how-to-discover.md)
- [Jak dodawać adnotacje danych składników majątku](data-catalog-how-to-annotate.md)
- [Jak umożliwia dokumentowanie zasobów danych](data-catalog-how-to-documentation.md)
- [Jak utworzyć połączenie ze danych składników majątku](data-catalog-how-to-connect.md)
- [Jak zarządzać danych składników majątku](data-catalog-how-to-manage.md)
