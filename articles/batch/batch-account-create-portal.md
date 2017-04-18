<properties
    pageTitle="Utwórz konto Azure partii | Microsoft Azure"
    description="Dowiedz się, jak utworzyć konto Azure partii w portal Azure, aby uruchomić na dużą skalę równoległe obciążenie pracą w chmurze"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Tworzenie konta partii Azure za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Zarządzanie partii .NET](batch-management-dotnet.md)

Dowiedz się, jak utworzyć konto Azure partii w [Azure portal][azure_portal]i gdzie można znaleźć ważne właściwości konta, takich jak klawiszami dostępu i konta adresy URL. Omówimy również partii ceny i łączenie konta magazynu platformy Azure do swojego konta partii, dzięki czemu można użyć [pakietów aplikacji](batch-application-packages.md) i [utrzymują dane wyjściowe i zadań](batch-task-output.md).

## <a name="create-a-batch-account"></a>Tworzenie konta partii

1. Zaloguj się do [portalu Azure][azure_portal].

2. Kliknij przycisk **Nowy** > **obliczyć** > **usługi partii**.

    ![Partii na rynku][marketplace_portal]

3. Zostanie wyświetlona karta **Nowego konta partię** . Zobacz elementy ** do *e* poniżej dla opisy poszczególnych elementów karta.

    ![Tworzenie konta partii][account_portal]

    . **Nazwa konta**: unikatową nazwę dla Twojego konta partię. Ta nazwa musi być unikatowa w obszarze Azure, konto jest tworzony (zobacz poniższą *lokalizacji* ). Go może zawierać tylko małe litery, cyfry i musi być 3-24 znaków.

    b. **Subskrypcja**: subskrypcji, w której chcesz utworzyć konto partię. Jeśli masz tylko jedną subskrypcję, jest zaznaczona domyślnie.

    c. **Grupa zasobów**: istniejący zasób grupy dla nowego konta w partii lub opcjonalnie możesz utworzyć nowy.

    d. **Lokalizacja**: region Azure, w której chcesz utworzyć konto partię. Tylko regiony obsługiwane przez subskrypcję i grupa zasobów są wyświetlane jako opcje.

    e. **Konto miejsca do magazynowania** (opcjonalnie): konto miejsca do magazynowania **uniwersalny** skojarzyć (łącza) do nowego konta w partii. Aby uzyskać więcej informacji, zobacz temat [konta połączone magazyn Azure](#linked-azure-storage-account) poniżej.

4. Kliknij przycisk **Utwórz** , aby utworzyć konto.

  Portalu wskazuje, że jest **Wdrażanie** konta, a po zakończeniu, pojawi się powiadomienie **wdrożeń zakończyła się pomyślnie** , w *powiadomieniach*.

## <a name="view-batch-account-properties"></a>Wyświetlanie właściwości konta partii

Po utworzeniu konta możesz otworzyć **Karta konta partii** dostęp do jej ustawienia i właściwości. Przy użyciu menu po lewej stronie karta konta partii są dostępne wszystkie ustawienia konta i właściwości.

![Karta konta partii w Azure portal][account_blade]

* **Adres URL konta partii**: tworzenie [partii rozwoju interfejsy API](batch-technical-overview.md#batch-development-apis) potrzebne adresu URL konta do zarządzania zasobami i uruchamiania zadań przy użyciu konta. Adres URL konta partii ma następujący format:

    `https://<account_name>.<region>.batch.azure.com`

![Adres URL konta partii w portalu][account_url]

* **Klawisze dostępu**: aplikacji muszą również klawisz dostępu podczas pracy z zasobami na koncie partię. Aby wyświetlić lub ponownie wygenerować klawiszy dostępu konta partię, wprowadź `keys` w menu po lewej stronie pola **wyszukiwania** na karta konta partię, a następnie wybierz **klawiszy**.

    ![Klucze konta partii w Azure portal][account_keys]

## <a name="pricing"></a>Cennik

Konta partii są dostępne tylko w jednej "bezpłatne warstwie," co oznacza, że nie są pobierane dla samo konto partię. Są naliczane źródłowych zasobów Azure obliczeń, które zajmują rozwiązanie partię, a dla zasobów zużywanej przez inne usługi uruchomienie usługi obciążenia. Na przykład jest naliczany dla węzłów komputerowe w Twojej pul i danych są przechowywane w magazynie Azure jako dane wejściowe i wyjściowe dla zadań. Podobnie jeśli używasz funkcji [pakietów aplikacji](batch-application-packages.md) partii, są naliczane dla zasobów magazyn Azure używany do przechowywania pakiety aplikacji. Zobacz [ceny partii] [ batch_pricing] uzyskać więcej informacji.

## <a name="linked-azure-storage-account"></a>Połączone konta magazynu platformy Azure

Jak wspomniano wcześniej, konto magazynowania **uniwersalny** można połączyć (opcjonalnie) do konta w partii. Funkcja [pakietów aplikacji](batch-application-packages.md) partii używa magazyn obiektów blob w połączonych uniwersalny konta miejsca do magazynowania, podobnie jak biblioteki [.NET konwencje plik wsadowy](batch-task-output.md) . Te funkcje opcjonalne pomóc w wdrażania aplikacji, uruchomienia zadań partię, a utrzymuje dane, które oferują.

Partii obecnie obsługuje *tylko* typ konta magazynu **uniwersalny** zgodnie z opisem w kroku 5, [Utwórz konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account)w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). Gdy konto magazyn Azure Połącz się ze swoim kontem partię, czy łącze *tylko* być kontem miejsca do magazynowania **uniwersalny** .

![Tworzenie konta miejsca do magazynowania "Uniwersalny"][storage_account]

Zaleca się utworzenie konta miejsca do magazynowania w trybie wyłączności przez konta partię.

>[AZURE.WARNING] Zachowaj ostrożność podczas ponownego generowania kluczy dostępu połączony klient miejsca do magazynowania. Ponownie wygenerować tylko jeden klucz konta miejsca do magazynowania i kliknij pozycję **Klawiszy synchronizacji** połączonych karta konta miejsca do magazynowania. Poczekaj pięć minut, aby umożliwić klawisze propagowanie węzły komputerowe w Twojej pul Generuj, a następnie zsynchronizować inny klawisz, w razie potrzeby. Jeśli w tym samym czasie ponownie wygenerować oba przyciski, węzły obliczeń nie będzie można zsynchronizować albo klucz i utracą dostęp do konta miejsca do magazynowania.

  ![Ponowne generowanie kluczy konta miejsca do magazynowania][4]

## <a name="batch-service-quotas-and-limits"></a>Przydziały usługi partii i ograniczenia

Należy pamiętać, że podobnie jak w przypadku subskrypcji usługi Azure i innych usług Azure niektórych [przydziałów i ograniczenia](batch-quota-limit.md) dotyczą partii kont. Bieżący przydziałów dla konta partii są wyświetlane w portalu na koncie **Właściwości**.

![Partia konta przydziały w Azure portal][quotas]

Jak są projektowanie i skalowanie wewnętrzne obciążeń pracą usługi partii, należy pamiętać o tych przydziałów. Na przykład jeśli puli użytkownika nie jest osiągnięcie docelowej liczby węzłów obliczeń, wybrane, może być osiągnięto limit przydziału core dla Twojego konta partię.

Należy również zauważyć, że nie masz uprawnienia dla jednego konta partii dla subskrypcji Azure. Uruchamianie wielu zadań partii z jednego konta partii lub rozpowszechnianie programu obciążenia między wieloma kontami partii w tej samej subskrypcji, ale w różnych regionach Azure.

Wiele z tych przydziałów można zwiększyć po prostu z prośbę o pomoc techniczną produktu bezpłatne przesłane w portalu Azure. Aby uzyskać szczegółowe informacje dotyczące żądania przyrosty zasobów, zobacz [przydziałów i limity dotyczące usługi Azure partię](batch-quota-limit.md) .

## <a name="other-batch-account-management-options"></a>Inne opcje zarządzania kontem partii

Oprócz korzystania Azure portal, można także tworzyć i zarządzać kontami partii z następujących czynności:

* [Partii apletów poleceń programu PowerShell](batch-powershell-cmdlets-get-started.md)
* [Polecenie Azure](../xplat-cli-install.md)
* [Zarządzanie partii .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Następne kroki

* Zobacz [Omówienie funkcji partii Azure](batch-api-basics.md) dowiedzieć się więcej o wydajności partii i funkcje. Artykuł w tym artykule omówiono podstawowy zasobów partię, takich jak pule, węzły obliczeń, zadania i zadania i omówiono funkcje usługi, które umożliwiają wykonywanie obciążenie pracą obliczeń na dużą skalę.

* Podstawowe opracowywania aplikacji z obsługą partii przy użyciu [Biblioteka klienta .NET partii](batch-dotnet-get-started.md)informacje. [Artykuł wprowadzający](batch-dotnet-get-started.md) przeprowadzi Cię przez działającą aplikację do wykonywania obciążenie pracą w wielu węzłów obliczeń jest używana usługa partii, która zawiera obciążenie pracą pliku tymczasowego i pobierania za pomocą magazyn Azure.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Ponowne generowanie kluczy konta miejsca do magazynowania"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
