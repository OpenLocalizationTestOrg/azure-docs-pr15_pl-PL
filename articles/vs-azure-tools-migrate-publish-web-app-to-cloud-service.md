<properties
   pageTitle="Jak przeprowadzić migrację i publikowanie aplikacji sieci Web do usługi w chmurze Azure z programu Visual Studio | Microsoft Azure"
   description="Dowiedz się, jak przeprowadzić migrację i publikowanie aplikacji sieci web do usługi w chmurze Azure przy użyciu programu Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Jak: Migrowanie i publikowanie aplikacji sieci Web do usługi w chmurze Azure z programu Visual Studio

Podjęcie usług hostingu i skalowalność Azure, warto przeprowadzić migrację i publikowanie aplikacji sieci web do usługi w chmurze Azure. Możesz uruchomić aplikacji sieci web w Azure z minimalnymi zmian w istniejącej aplikacji.

>[AZURE.NOTE] Ten temat dotyczy wdrażanie usług w chmurze, nie do witryn sieci web. Aby uzyskać informacji na temat wdrażania witryn sieci web zobacz [Wdrażanie aplikacji sieci web w usłudze Azure aplikacji](./app-service-web/web-sites-deploy.md).

Aby uzyskać listę określone szablony obsługiwanych zarówno Visual C# i Visual Basic zobacz sekcję **Obsługiwane szablony projektów** w dalszej części tego tematu.

Należy najpierw włączyć aplikacji sieci web Azure z programu Visual Studio. Na poniższej ilustracji przedstawiono podstawowe etapy Publikowanie istniejącej aplikacji sieci web, dodając Azure projektu używanego do wdrożenia. Ten proces powoduje dodanie Azure projektu z roli sieci web do rozwiązania. Na podstawie typu projektu sieci web, które posiadasz, właściwości projektu dla zestawów są również aktualizowane Jeśli pakietu z wymaga dodatkowych zestawów do wdrożenia.

![Publikowanie aplikacji sieci Web Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] **Konwertowanie**, **Konwersja na projekt usługi Cloud Azure** polecenia są wyświetlane tylko dla projektu sieci web w rozwiązaniu. Na przykład polecenia nie są dostępne w projekcie programu Silverlight w rozwiązaniu.
Podczas tworzenia pakietu usługi lub publikowanie aplikacji Azure mogą pojawić się ostrzeżenia lub błędy. Te ostrzeżeń i błędów może ułatwić rozwiązywanie problemów, przed wdrożeniem Azure. Na przykład może pojawi się ostrzeżenie o zestawie brak. Aby uzyskać więcej informacji na temat traktuje ostrzeżenia jako błędy, zobacz [Konfigurowanie Azure chmury usługi programu Project przy użyciu programu Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Jeśli tworzenie aplikacji, uruchom go lokalnie, przy użyciu emulatora obliczeń lub opublikować go Azure, może zostać wyświetlony następujący błąd w oknie **Lista błędów** : **określonej path, nazwę pliku lub oba są zbyt długie**. Ten błąd występuje, ponieważ długość nazwy w pełni kwalifikowana Azure projektu jest zbyt długa. Długość nazwy projektu, łącznie z pełną ścieżką nie może być więcej niż 146 znaków. Na przykład jest to nazwa pełny projekt, tym ścieżka Azure projektu, który jest tworzony dla aplikacji Silverlight: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Może być konieczne przeniesienie rozwiązanie do innego katalogu krótszej ścieżce w celu zmniejszenia długość nazwy projektu w pełni kwalifikowana.

Aby przeprowadzić migrację i publikowanie aplikacji sieci web Azure z programu Visual Studio, wykonaj następujące kroki.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Włączanie aplikacji sieci Web do wdrożenia Azure

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Aby umożliwić aplikacji sieci web do wdrożenia Azure

1. Aby umożliwić aplikacji sieci web do wdrożenia Azure, otwórz menu skrótów dla projektu sieci web w rozwiązaniu i wybierz polecenie Dodaj projekt wdrożenia Azure.

    Wykonywane są następujące akcje:

    - Azure projektu o nazwie `<name of the web project>.Azure` jest dodawany do rozwiązania dla aplikacji.

    - Role sieci web dla projektu sieci web jest dodawany do tego projektu Azure.

    - **Kopii lokalnej** właściwość jest ustawiona na PRAWDA dla dowolnych zestawów, które są wymagane do MVC MVC 2, MVC 3, 4 i Silverlight aplikacji biznesowych. Spowoduje to dodanie tych zestawów pakietu z używaną do wdrożenia.

  >[AZURE.IMPORTANT] Jeśli masz inne zestawy lub pliki, które są wymagane do tej aplikacji sieci web, należy ręcznie ustawić właściwości dla tych plików. Aby uzyskać informacje dotyczące ustawiania tych właściwości zobacz sekcję **Obejmują pliki w pakiecie usługi** w dalszej części tego artykułu.

  >[AZURE.NOTE] Jeśli rola sieci web dla projektu w określonej sieci web już istnieje w projekcie Azure w rozwiązanie, **Konwertowanie**, **Konwersja na projekt usługi Cloud Azure** nie jest wyświetlana w menu skrótów dla tego projektu sieci web.

  Jeśli masz wiele projektów sieci web w aplikacji sieci web i chcesz utworzyć role w sieci web dla każdego projektu sieci web, należy wykonać czynności opisane w tej procedurze dla każdego projektu sieci web. Spowoduje to utworzenie oddzielnych Azure projektów dla poszczególnych ról w sieci web. Każdy projekt sieci web można opublikować oddzielnie. Alternatywnie możesz ręcznie dodać kolejnej roli sieci web w istniejącym projekcie Azure w aplikacji sieci web. W tym celu należy otworzyć menu skrótów dla folderu **role** w projekcie Azure, wybierz pozycję **Dodaj**, a następnie **Projektu roli sieci Web w rozwiązanie**, wybierz projekt, aby dodać rolę sieci web, a następnie kliknij przycisk **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Baza danych SQL Azure za pomocą aplikacji

Jeśli masz parametry połączenia dla aplikacji sieci web, korzystającego z bazy danych programu SQL Server w środowisku lokalnym, należy zmienić ten ciąg połączenia, aby użyć wystąpienie bazy danych SQL Azure obsługującego zamiast tego.

>[AZURE.IMPORTANT] Twoja subskrypcja należy włączyć użytkownikowi korzystania z bazy danych SQL. Jeśli uzyskujesz dostęp do subskrypcji z [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885), można określić, jakie usługi oferuje subskrypcji. Poniższe instrukcje dotyczą wydaną [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885). Jeśli używasz [Azure portal](http://portal.microsoft.com), przejdź do następnej procedury.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Aby użyć wystąpienie bazy danych SQL w ról w sieci web dla ciągu połączenia

1. Aby utworzyć wystąpienie bazy danych SQL w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885), wykonaj czynności opisane w artykule: [Tworzenie serwera bazy danych SQL](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Podczas konfigurowania reguły zapory wystąpienia bazy danych SQL musi zaznacz pole wyboru **Zezwalaj na inne usługi Azure dostępu do tego serwera** .

1. Aby utworzyć wystąpienie bazy danych SQL służących do ciągu połączenia, wykonaj czynności opisane w następnej sekcji w następujący artykuł: [Tworzenie bazy danych SQL](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Aby skopiować ADO.NET parametry połączenia dla ciągu połączenia, wykonaj następujące czynności w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Wybierz przycisk **bazy danych** , a następnie otwórz węzeł subskrypcję, do której został użyty do utworzenia wystąpienia bazy danych SQL.

  1. Aby wyświetlić dostępne wystąpienia bazy danych SQL, wybierz węzeł **Bazy danych SQL** .

  1. Aby wyświetlić właściwości bazy danych, wybierz bazę danych. Zostanie wyświetlony widok **Właściwości** .

      >[AZURE.NOTE] Wyświetlanie **Właściwości** nie jest wyświetlany, może być konieczne otworzyć przy użyciu linii podziału.

  1. Aby wyświetlić parametry połączenia, wybierz przycisk wielokropka (...) obok widoku.

    Zostanie wyświetlone okno dialogowe **Parametry połączenia** .

  1. Aby skopiować ADO.NET parametry połączenia, zaznacz tekst i wybierz klawisze Ctrl + C.

  1. Aby zamknąć okno dialogowe, wybierz przycisk **Zamknij** .

1. Aby zamienić parametry połączenia w pliku web.config, aby użyć tego wystąpienia bazy danych SQL, otwórz plik web.config, zaznacz istniejący wpis parametry połączenia, a następnie wybierz klawisze Ctrl + V. ADO.NET parametry połączenia dla danego wystąpienia bazy danych SQL zastępuje istniejących parametrów połączenia.

1. Należy również dodać parametr `MultipleActiveResultSets=True` do parametry połączenia. Parametry połączenia powinien mieć następujący format:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Opcjonalnie) Alternatywna metoda ze zmianą parametry połączenia bezpośrednio w pliku web.config jest dodawanie sekcji do jednego z plików transformacji web.config, w zależności od konfiguracji kompilacji, która umożliwia tworzenie pakietu usługi. Otwórz plik Web.Debug.Config lub plik Web.Release.Config. Dodaj następującą sekcję do tego pliku:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Zapisz plik, który został zmodyfikowany i ponowne publikowanie aplikacji.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Aby użyć wystąpienie bazy danych SQL za pomocą portalu klasyczny Azure

1. W [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885)wybierz węzeł bazy danych SQL.

  - Jeśli pojawi się wystąpienie bazy danych SQL, który ma być używany, wybierz go otworzyć.

  - Jeśli nie utworzono wystąpienia, wybierz odpowiednie łącze, a następnie utwórz wystąpienie.

1. Po otwarciu lub Utwórz wystąpienie bazy danych wybierz link **Parametry połączenia** .

1. U dołu strony wybierz pozycję łącze do skonfigurowania ustawień zapory i zaakceptować domyślne wartości lub skonfigurować wartości, które są potrzebne.

1. Skopiuj parametry połączenia ADO.NET, wklej je do pliku web.config na starych parametry połączenia dla lokalnej bazy danych i upewnij się dodać `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Publikowanie aplikacji sieci Web Azure

### <a name="to-publish-a-web-application-to-azure"></a>Publikowanie aplikacji sieci Web Azure

1. Aby przetestować aplikację w rozwoju lokalnego środowiska przy użyciu Azure obliczyć emulatora, otwórz menu skrótów dla Azure projektu do ról w sieci web i wybierz pozycję **Ustaw jako projekt startowy**. Następnie wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: **F5**).

    Zostanie wyświetlone okno dialogowe **Uruchom środowisko debugowania Azure** i uruchamiania aplikacji w przeglądarce. Aby uzyskać szczegółowe informacje na temat uruchamiania każdego typu aplikacji sieci web w emulatorze obliczeń zobacz tabelę w tej sekcji.

1. Aby skonfigurować usługi aplikacji publikowanie Azure, musi mieć konto Microsoft i subskrypcji usługi Azure. Wykonaj czynności podane w temacie w celu skonfigurowania usług: [Przygotowywanie do publikowania i wdrożyć aplikację Azure z programu Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Aby opublikować aplikacji sieci web z Azure, otwórz menu skrótów dla projektu sieci web i wybierz pozycję **Publikuj Azure**.

    Zostanie wyświetlone okno dialogowe **Publikowanie aplikacji Azure** i Visual Studio rozpoczęcie procesu wdrażania. Aby uzyskać więcej informacji na temat opublikować aplikację zobacz sekcję **Publikowanie aplikacji Azure z programu Visual Studio** [publikowania usługi w chmurze za pomocą narzędzi Azure](vs-azure-tools-publishing-a-cloud-service.md).

    >[AZURE.NOTE] Można również publikować aplikacji sieci web z Azure programu project. Aby to zrobić, otwórz menu skrótów dla Azure projektu i wybierz pozycję **Publikuj**.

1. Aby wyświetlić postęp wdrożenia, możesz wyświetlić okno **Dziennik działań Azure** . Dziennik ten jest automatycznie wyświetlany podczas uruchamiania procesu wdrażania. Możesz rozwinąć pozycji w dzienniku aktywności, aby wyświetlić szczegółowe informacje, jak pokazano na poniższej ilustracji:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Opcjonalnie) Aby anulować proces wdrażania, otwórz menu skrótów dla pozycji wiersza w dzienniku aktywności i wybierz pozycję **Anuluj i Usuń**. To zatrzymanie procesu wdrażania i usuwa środowiska rozmieszczania z Azure.

    >[AZURE.NOTE] Aby usunąć to środowisko rozmieszczania, po jej wdrożeniu, należy użyć [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Opcjonalnie) Po uruchomieniu wystąpienia do roli mają Visual Studio automatycznie wyświetlana środowiska wdrażania w węźle **Obliczyć Azure** w **Chmurze Eksploratora** lub **Eksploratora serwera**. W tym miejscu możesz wyświetlać stan wystąpienia poszczególnych ról.

    Na poniższej ilustracji przedstawiono wystąpienia roli w **Eksploratorze Server** , gdy są one nadal w stanie inicjowanie:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Aby uzyskać dostęp do aplikacji po wdrożeniu, wybierz strzałkę obok pozycji wdrożenia po wyświetleniu stan **wykonane** w **Azure dziennik**. Spowoduje to wyświetlenie adresu URL aplikacji sieci web Azure. Zobacz szczegółowe informacje dotyczące sposobu uruchamiania określonego typu aplikacji sieci web z platformy Azure poniższej tabeli.

    W poniższej tabeli przedstawiono szczegółowe informacje dotyczące tworzenie aplikacji sieci web określonych na podstawie Azure lub uruchamiania lub debugowania aplikacji sieci web lokalnie, przy użyciu emulatora obliczyć Azure:

  	|Typ aplikacji sieci Web|Uruchom debugowania lokalnie, przy użyciu emulatora obliczeń|Uruchamianie platformy Azure|
  	|---|---|---|
  	|Aplikacja sieci Web programu ASP.NET|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Wybierz polecenie hiperłącze adres URL wyświetlany na karcie **wdrożenia** **Azure dziennik** ładowanie strony startowej w przeglądarce.|
  	|Aplikacja sieci Web programu ASP.NET MVC 2|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Wybierz polecenie hiperłącze adres URL wyświetlany na karcie **wdrożenia** **Azure dziennik** ładowanie strony startowej w przeglądarce.|
  	|Aplikacja sieci Web programu ASP.NET MVC 3|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Wybierz polecenie hiperłącze adres URL wyświetlany na karcie **wdrożenia** **Azure dziennik** ładowanie strony startowej w przeglądarce.|
  	|Aplikacja sieci Web programu ASP.NET MVC 4|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Wybierz polecenie hiperłącze adres URL wyświetlany na karcie **wdrożenia** **Azure dziennik** ładowanie strony startowej w przeglądarce.|
  	|Aplikacja sieci Web programu ASP.NET puste|Strona aspx należy dodać w aplikacji ustawiany jako stronę początkową projektu sieci web. Następnie na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Jeśli masz strona aspx domyślne w aplikacji, wybierz polecenie hiperłącze adres URL wyświetlany na karcie **wdrożenia** **Azure dziennik** i ta strona jest pobierana w przeglądarce. Jeśli masz strona aspx różnych, musisz przejść do tej strony w następującym formacie dla adresu url:`<url for deployment>/<name of page>.aspx`|
  	|Aplikacji Silverlight|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Należy przejść do odpowiedniej strony w następującym formacie dla adresu url aplikacji:`<url for deployment>/<name of page>.aspx`|
  	|Program Silverlight aplikacji biznesowych|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Należy przejść do odpowiedniej strony w następującym formacie dla adresu url aplikacji:`<url for deployment>/<name of page>.aspx`|
  	|Aplikacji Silverlight nawigacji|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Należy przejść do odpowiedniej strony w następującym formacie dla adresu url aplikacji:`<url for deployment>/<name of page>.aspx`|
  	|Aplikacja usługi WCF|Konieczne jest ustawienie pliku svc jako strony startowej projektu Usługa WCF. Następnie na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Należy przejść do pliku svc aplikacji w następującym formacie dla adresu url:`<url for deployment>/<name of service file>.svc`|
  	|Aplikacja usługi WCF przepływu pracy|Konieczne jest ustawienie pliku svc jako strony startowej projektu Usługa WCF. Następnie na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Należy przejść do pliku svc aplikacji w następującym formacie dla adresu url:`<url for deployment>/<name of service file>.svc`|
  	|Jednostki dynamiczne programu ASP.NET|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Musisz zaktualizować parametry połączenia (zobacz następną sekcję). Należy przejść do odpowiedniej strony w następującym formacie dla adresu url aplikacji:`<url for deployment>/<name of page>.aspx`|
  	|Dynamiczne ASP.NET Linq danych SQL|Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** (klawiatury: Wybierz klawisz **F5** .).|Należy wykonać czynności opisane w tej procedurze: bazy danych platformy SQL Azure za pomocą aplikacji (zobacz wcześniejszych sekcję w tym temacie). Należy przejść do odpowiedniej strony w następującym formacie dla adresu url aplikacji:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualizowanie parametry połączenia dla obiektów dynamiczne programu ASP.NET

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Aby zaktualizować parametry połączenia dla obiektów dynamiczne programu ASP.NET

1. Aby utworzyć bazę danych platformy SQL Azure, który może być używany dla aplikacji sieci web ASP.NET podmioty dynamiczne, wykonaj kroki procedury **używania bazy danych platformy SQL Azure aplikacji** wcześniej w tym temacie.

1. Dodawanie tabel i pól, których potrzebujesz dla tej bazy danych w [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Parametry połączenia dla tego typu aplikacji ma następujący format w pliku web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Zaktualizuj wartość *connectionString* przy użyciu parametrów połączenia ADO.NET dla bazy danych platformy SQL Azure w następujący sposób:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Aby zapisać plik web.config zmiany dokonane w parametrach połączenia na pasku menu wybierz pozycję **plik**, **Zapisz web.config**.

## <a name="supported-project-templates"></a>Szablony projektów obsługiwane

Aby opublikować aplikacji sieci web Azure, aplikacji należy użyć jednego z szablonów programu project dla C# lub Visual Basic, który jest wymieniony w poniższej tabeli.

|Grupa szablon projektu|Szablon projektu|
|---|---|
|W sieci Web|Aplikacja sieci Web programu ASP.NET|
|W sieci Web|Aplikacja sieci Web programu ASP.NET MVC 2|
|W sieci Web|Aplikacja sieci Web programu ASP.NET MVC 3|
|W sieci Web|Aplikacja sieci Web programu ASP.NET MVC4|
|W sieci Web|Aplikacja sieci Web programu ASP.NET puste|
|W sieci Web|Aplikacja pustego sieci Web ASP.NET MVC 2|
|W sieci Web|Aplikacja sieci Web ASP.NET dynamiczne obiekty danych|
|W sieci Web|Dynamiczne ASP.NET Linq danych do aplikacji sieci Web SQL|
|Program Silverlight|Aplikacji Silverlight|
|Program Silverlight|Program Silverlight aplikacji biznesowych|
|Program Silverlight|Aplikacji Silverlight nawigacji|
|WCF|Aplikacja usługi WCF|
|WCF|Aplikacja usługi WCF przepływu pracy|
|Przepływ pracy|Aplikacja usługi WCF przepływu pracy|

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji dotyczących publikowania zobacz [Przygotowanie do publikowania lub rozmieszczanie aplikacji Azure za pomocą programu Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Także zajrzeć [Ustawienie w górę o nazwie poświadczeń uwierzytelniania](vs-azure-tools-setting-up-named-authentication-credentials.md).
