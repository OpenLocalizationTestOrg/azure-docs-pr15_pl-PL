<properties
    pageTitle="Tworzenie osób uzyskujących .NET HDInsight-interactive uwierzytelniania | Microsoft Azure"
    description="Dowiedz się, jak utworzyć-interactive uwierzytelniania aplikacji .NET HDInsight."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Tworzenie-interactive uwierzytelniania aplikacji .NET HDInsight

Można wykonywać aplikacji .NET Azure HDInsight tożsamością aplikacji (non-interactive) lub z tożsamością zalogowany użytkownik aplikacji (interakcyjne). Przykładowe interakcyjnych aplikacji zobacz [Przesyłanie gałęzi-świnka-Sqoop zadań przy użyciu zestawu SDK usługi HDInsight .NET](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). W tym artykule pokazano, jak utworzyć-interactive uwierzytelniania aplikacji .NET nawiązać połączenie z usługi HDInsight Azure i Prześlij zadanie gałęzi.

Z poziomu aplikacji .NET potrzebne są:

- Identyfikator subskrypcji Azure dzierżawy
- Identyfikator klienta aplikacji katalogu Azure
- klucz tajny aplikacji Azure katalogu.  

Proces główny obejmuje następujące kroki:

2. Tworzenie aplikacji usługi Azure Directory.
2. Przypisywanie ról do aplikacji AD.
3. Opracować aplikację klienta.


##<a name="prerequisites"></a>Wymagania wstępne

- Klaster HDInsight. Możesz utworzyć jeden zgodnie z instrukcjami zawartymi w [Wprowadzenie — samouczek](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 




## <a name="create-azure-directory-application"></a>Tworzenie aplikacji katalogu Azure 
Podczas tworzenia aplikacji usługi Active Directory, faktycznie tworzy zarówno aplikacji i usługą kapitału. Można wykonywać aplikacji w obszarze tożsamości aplikacji.

Obecnie portalu klasyczny Azure należy użyć do utworzenia nowej aplikacji usługi Active Directory. Ta możliwość zostaną dodane do portalu Azure w nowszej wersji. Można również wykonać następujące czynności za pomocą programu PowerShell Azure lub polecenie Azure. Aby uzyskać więcej informacji na temat przy użyciu programu PowerShell lub interfejsu wiersza polecenia z głównej usługi zobacz [Usługa uwierzytelniania kapitału przy użyciu Menedżera zasobów Azure](../resource-group-authenticate-service-principal.md).

**Aby utworzyć aplikację katalogu Azure**

1.  Zaloguj się do [portalu klasyczny Azure]( https://manage.windowsazure.com/).
2.  Wybierz pozycję **Usługi Active Directory** w okienku po lewej stronie.

    ![Azure klasyczny portalu usługi active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Wybierz katalog, który ma być używany do tworzenia nowej aplikacji. Jest ona istniejącego wzornika.
4.  U góry listy istniejących aplikacji kliknij pozycję **aplikacje** .
5.  Kliknij przycisk **Dodaj** od dołu w celu dodania nowej aplikacji.
6.  Wprowadź **nazwę**, wybierz **aplikację sieci Web i/lub interfejs API sieci Web**, a następnie kliknij przycisk **Dalej**.

    ![Nowa aplikacja usługi azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Wprowadź **adres URL logowania jednokrotnego** i **Aplikacji identyfikator URI**. Dla **Adresu URL logowania na**przekazać go do witryny, który opisuje aplikacji. Obecność w witrynie sieci web nie jest sprawdzany. Dla aplikacji identyfikator URI Podaj identyfikator URI, który identyfikuje aplikację. A następnie kliknij przycisk **Zakończ**.
Wystarczy kilka minut, aby utworzyć aplikację.  Po utworzeniu aplikacji portalu przedstawia stronę szybkie Glace nowej aplikacji. Nie zamykaj portalu. 

    ![nowe właściwości aplikacji usługi azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Aby uzyskać application client Identyfikator i klucz tajny**

1.  Na stronie aplikacji AD nowo utworzonego w górnym menu kliknij przycisk **Konfiguruj** .
2.  Tworzenie kopii **Identyfikator klienta**. Należy go w aplikacji .NET.
3.  W obszarze **klawiszy**kliknij listę rozwijaną **Wybierz czas trwania** i wybierz **rok 1** lub **2 lata**. Nie będą wyświetlane wartości klucza do momentu zapisania konfiguracji.
4.  Kliknij pozycję **Zapisz** u dołu strony. Gdy zostanie wyświetlony klucz tajny, Utwórz kopię klucza. Należy go w aplikacji .NET.

##<a name="assign-ad-application-to-role"></a>Przypisywanie AD aplikacji do roli

Należy przypisać aplikacji do [roli](../active-directory/role-based-access-built-in-roles.md) udzielenia uprawnień umożliwiających wykonywanie akcji. Możesz ustawić zakresu na poziomie subskrypcji, grupa zasobów lub zasobu. Uprawnienia są dziedziczone niższych poziomów zakresu (na przykład dodawania aplikacji do roli czytnika dla grupy zasobów oznacza, że można przeczytać grupa zasobów i wszystkie zasoby, które zawiera). W tym samouczku będzie Ustaw zakres na poziomie grupy zasobów.  Ponieważ portalu klasyczny Azure nie obsługuje grup zasobów, tej części ma zostać wykonane z portalu usługi Azure. 

**Aby dodać rolę właściciela do aplikacji AD**

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
2.  W lewym okienku kliknij pozycję **Grupa zasobów** .
3.  Kliknij grupę zasobów, która zawiera klaster HDInsight miejsce, w którym spowoduje uruchomienie kwerendy gałęzi w dalszej części tego samouczka. Jeśli ma zbyt wiele grup zasobów, możesz użyć filtru.
4.  Kliknij pozycję **dostęp** z karta klaster.

    ![Ikona chmury i thunderbolt = Szybki Start](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Kliknij pozycję **Dodaj** z karta **użytkowników** .
6.  Postępuj zgodnie z instrukcjami, aby dodać roli **właściciela** do aplikacji AD utworzonej w ostatniej procedury. Po zakończeniu go pomyślnie, zobaczysz są aplikacji wymienionych w karta użytkowników z roli właściciela.


##<a name="develop-hdinsight-client-application"></a>Opracowywanie aplikacji klienckiej HDInsight

Tworzenie aplikacji konsoli C# .net postępując zgodnie z instrukcjami znaleziony w [zadaniach przesyłanie Hadoop HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Następnie należy zastąpić metodę GetTokenCloudCredentials następujące czynności:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Aby pobrać identyfikator dzierżawy przy użyciu programu PowerShell:

    Get-AzureRmSubscription

Lub polecenie Azure:

    azure account show --json

      
## <a name="see-also"></a>Zobacz też

- [Przesyłanie zadania Hadoop w HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Tworzenie aplikacji usługi Active Directory i wystawcy usługi za pomocą portalu](../resource-group-create-service-principal-portal.md)
- [Uwierzytelnianie wystawcy usługi przy użyciu Menedżera zasobów Azure](../resource-group-authenticate-service-principal.md)
- [Kontrola dostępu oparta na rolach Azure](../active-directory/role-based-access-control-configure.md)
