<properties
    pageTitle="Wprowadzenie do programu Eksplorator magazynu (wersja Preview) | Microsoft Azure"
    description="Zarządzanie zasobami magazynowania Azure Eksplorator magazynu (wersja Preview)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Wprowadzenie do programu Eksplorator magazynu (wersja Preview)

## <a name="overview"></a>Omówienie 

Eksplorator magazynu Microsoft Azure (wersja Preview) jest aplikację autonomicznej, która umożliwia łatwe pracę z danymi magazyn Azure systemu Windows, OS X i Linux. W tym artykule dowiesz się, różne sposoby nawiązywania połączeń i zarządzanie kontami usługi Azure miejsca do magazynowania.

![Eksplorator magazynu platformy Microsoft Azure (wersja Preview)][15]

## <a name="prerequisites"></a>Wymagania wstępne

- [Pobieranie i instalowanie Eksplorator magazynu (wersja preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Nawiązywanie połączenia z kontem miejsca do magazynowania lub usługi

Eksplorator magazynu (wersja Preview) zawiera tysięcy sposoby nawiązywania połączenia z kontami miejsca do magazynowania. Ta opcja uwzględnia nawiązywanie połączenia z magazynu kont skojarzonych z subskrypcji Azure łączenia się z konta magazynu i usług udostępnionych z innych subskrypcji Azure i nawet nawiązywanie i zarządzanie magazynu lokalnego za pomocą Azure emulatora miejsca do magazynowania:

- [Nawiązywanie połączenia z subskrypcji usługi Azure](#connect-to-an-azure-subscription) — zarządzanie zasobami magazynowania należące do subskrypcji usługi Azure.
- [Praca z rozwoju lokalnego magazynu](#work-with-local-development-storage) - Zarządzanie magazynu lokalnego za pomocą Azure emulatora miejsca do magazynowania. 
- [Dołączanie do magazynu zewnętrznych](#attach-or-detach-an-external-storage-account) — zarządzanie zasobami magazynowania należące do innej subskrypcji Azure za pomocą konta miejsca do magazynowania nazwę konta i klucza.
- [Dołącz miejsca do magazynowania konta przy użyciu skojarzenia zabezpieczeń](#attach-storage-account-using-sas) — zarządzanie zasobami magazynowania należące do innej subskrypcji Azure za pomocą skojarzenia zabezpieczeń.
- [Dołącz usługi przy użyciu skojarzenia zabezpieczeń](#attach-service-using-sas) — Zarządzanie usługą określonego miejsca do magazynowania (kontener obiektów blob, kolejki lub tabela) należących do innej subskrypcji Azure za pomocą skojarzenia zabezpieczeń.

## <a name="connect-to-an-azure-subscription"></a>Nawiązywanie połączenia z subskrypcji usługi Azure

> [AZURE.NOTE] Jeśli nie masz konta usługi Azure, możesz [Utwórz konto w bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywowania programu Visual Studio subskrybentów korzyści](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. W Eksploratorze magazynu (wersja Preview) wybierz pozycję **Ustawienia konta na platformie Azure**. 

    ![Ustawienia konta Azure][0]

1. W okienku po lewej stronie teraz zostaną wyświetlone wszystkie konta Microsoft, zarejestrowania do. Aby połączyć się z innym kontem, wybierz pozycję **Dodaj konto**, a następnie postępuj zgodnie okna dialogowe, aby zalogować się przy użyciu konta Microsoft, który jest skojarzony z co najmniej jedną aktywną subskrypcję Azure.

    ![Dodawanie konta][1]

1. Po pomyślnym zalogowaniu się przy użyciu konta Microsoft, okienku po lewej stronie zostanie wypełnione Azure subskrypcje związane z tym kontem. Wybierz subskrypcje Azure, z którymi chcesz pracować, a następnie wybierz pozycję **Zastosuj**. (Wybieranie przełącza **Wszystkie subskrypcje** , zaznaczając wszystkie lub żaden z wymienionych subskrypcje Azure).

    ![Wybierz pozycję subskrypcje Azure][3]

1. W okienku po lewej stronie teraz zostaną wyświetlone konta magazynu skojarzone z wybrane subskrypcje Azure.

    ![Wybrane subskrypcje Azure][4]

## <a name="work-with-local-development-storage"></a>Praca z lokalnym opracowywania miejsca do magazynowania

Eksplorator magazynu (wersja Preview) umożliwia pracę przed magazynu lokalnego za pomocą Azure emulatora miejsca do magazynowania. Dzięki temu można napisać kod i przetestować miejsca do magazynowania bez konieczności posiadania konta miejsca do magazynowania, wdrożony Azure (ponieważ przez Emulator miejsca do magazynowania Azure emulowane jest konto miejsca do magazynowania).

>[AZURE.NOTE] Azure emulatora miejsca do magazynowania jest obecnie obsługiwane tylko dla systemu Windows. 

1. W lewym okienku okna Eksploratora magazynu (wersja Preview), rozwiń **(lokalnych i załączone** > **Kont miejsca do magazynowania** > węzeł**(rozwój)** .

    ![Węzeł rozwoju lokalnego][21]

1. Jeśli Emulator miejsca do magazynowania Azure nie jest jeszcze zainstalowany, zostanie wyświetlony monit, aby to zrobić za pomocą paska informacyjnego. Jeśli jest wyświetlany pasek informacyjny, zaznacz pole wyboru **Pobierz najnowszą wersję**, a następnie zainstaluj emulatorze. 

    ![Pobierz monit emulatora miejsca do magazynowania Azure][22]

1. Po zainstalowaniu emulatorze będą mieli możliwość tworzenia i pracy z lokalnym obiektów blob, kolejki i tabel. Aby dowiedzieć się, jak pracować z każdego miejsca do magazynowania typu konta, wybierz odpowiednie łącze poniżej:

    - [Zarządzanie zasobami magazyn obiektów blob platformy Azure](./vs-azure-tools-storage-explorer-blobs.md)
    - Zarządzanie współużytkowanie zasobów miejsca do magazynowania plików Azure — *już wkrótce*
    - Zarządzanie zasobami magazynowania kolejki Azure — *już wkrótce*
    - Zarządzanie zasobami magazyn tabel platformy Azure — *już wkrótce*

## <a name="attach-or-detach-an-external-storage-account"></a>Dołączanie i odłączanie konta zewnętrznego miejsca do magazynowania

Eksplorator magazynu (wersja Preview) umożliwia dołączyć do kont zewnętrznych miejsca do magazynowania, tak aby magazyn kont można łatwo udostępniać. W tej sekcji wyjaśniono, jak dołączyć do (i odłączyć) konta zewnętrznego miejsca do magazynowania.

### <a name="get-the-storage-account-credentials"></a>Uzyskaj poświadczenia konta miejsca do magazynowania

W celu udostępnienia konta zewnętrznego miejsca do magazynowania, właściciel konta musi najpierw uzyskać poświadczenia — nazwę konta i klucza — dla konta, a następnie Udostępnij te informacje z osobą, którzy chcą mieć możliwość dołączania do tego konta (zewnętrznym). Uzyskiwanie magazynu poświadczeń konta można zrealizować przez Azure portal, wykonując następujące czynności: 

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
1.  Wybierz pozycję **Przeglądaj**.
1.  Wybierz pozycję **konta miejsca do magazynowania**.
1.  W karta **Miejsca do magazynowania konta** wybierz konto, odpowiednie miejsca do magazynowania.
1.  W karta **Ustawienia** konta wybranego miejsca do magazynowania wybierz **klawiszy dostępu**.

    ![Opcja klawiszy dostępu][5]
    
1.  W karta **klawisze dostępu** skopiuj wartości **Nazwę konta MAGAZYNU** i **klucz 1** do użytku podczas dołączania do rachunku miejsca do magazynowania. 

    ![Klawisze dostępu][6]

### <a name="attach-to-an-external-storage-account"></a>Dołączanie do konta usługi zewnętrzne miejsca do magazynowania
Aby dołączyć do konta usługi zewnętrzne miejsca do magazynowania, konieczne będzie nazwę konta i klucza. W sekcji *odczytać poświadczenia konta miejsca do magazynowania* wyjaśniono sposób uzyskiwania tych wartości z poziomu portalu Azure. Jednak pamiętać, że w portalu klucz konta nosi nazwę "klucza 1" tak w przypadku, gdy Eksplorator magazynu (wersja Preview) zapytanie dotyczące klucz konta, użytkownik będzie wprowadź (lub Wklej) wartość "klucza 1". 
 
1.  W Eksploratorze magazynu (wersja Preview) zaznacz pole wyboru **Połącz z magazynem Azure**.

    ![Nawiązywanie połączenia z Azure przechowywania danych][23]

1.  W oknie dialogowym **Nawiązywanie połączenia z magazynem Azure** określ klucz konta ("klucza 1" wartość z portalu Azure), a następnie wybierz przycisk **Dalej**.

    ![Nawiązywanie połączenia z okno dialogowe Azure miejsca do magazynowania][24] 

1.  W oknie dialogowym **Dołączanie zewnętrznych przestrzeni dyskowej** wprowadź nazwę konta magazynu w polu **Nazwa konta** , określ odpowiednie ustawienia, a następnie wybierz przycisk **Dalej** po zakończeniu. 

    ![Dołączanie okno dialogowe zewnętrznych miejsca do magazynowania][8]

1.  W oknie dialogowym **Podsumowanie połączenia** Sprawdź informacje. Jeśli chcesz zmienić jakiekolwiek właściwości, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. Po zakończeniu wybierz pozycję **Połącz**.

1.  Po nawiązaniu połączenia z tekstem **(zewnętrznym)** dołączone do nazwę konta magazynu będą wyświetlane konta zewnętrznego miejsca do magazynowania. 

    ![Wynik łączenie się z kontem zewnętrznych miejsca do magazynowania][9]

### <a name="detach-from-an-external-storage-account"></a>Odłączanie z konta zewnętrznego miejsca do magazynowania

1.  Kliknij prawym przyciskiem myszy konto pamięci zewnętrznej, dla którego chcesz odłączyć, a - z menu kontekstowego — wybierz pozycję **Odłącz**.

    ![Odłączyć od przechowywania danych][10]

1.  Gdy pojawi się okno komunikatu potwierdzenia, wybierz pozycję **Tak,** aby potwierdzić oderwania z konta zewnętrznego miejsca do magazynowania.

## <a name="attach-storage-account-using-sas"></a>Dołączanie miejsca do magazynowania do konta za pomocą skojarzenia zabezpieczeń

[Skojarzenia zabezpieczeń (udostępnione podpis programu Access)](storage/storage-dotnet-shared-access-signature-part-1.md) daje administrator subskrypcji usługi Azure możliwości do udzielania dostępu do konta miejsca do magazynowania tymczasowo bez konieczności ich poświadczenia Azure subskrypcji. 

Aby to zilustrować, załóżmy UserA jest administratorem subskrypcji usługi Azure i UserA chce zezwolić UserB uzyskiwania dostępu do konta miejsca do magazynowania przez ograniczony czas przy niektórych uprawnień:

1. UserA generuje skojarzeń zabezpieczeń (składający się z parametry połączenia dla konta miejsca do magazynowania) dla określonego czasu i odpowiednie uprawnienia.
1. UserA udostępnia skojarzeń zabezpieczeń z osobą, którzy chcą mieć dostęp do konta miejsca do magazynowania — UserB, w naszym przykładzie.  
1. UserB używa Eksplorator magazynu (wersja Preview), aby dołączyć do konta należącego do UserA przy użyciu podanych skojarzeń zabezpieczeń. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Pobieranie skojarzeń zabezpieczeń dla konta, które chcesz udostępnić

1.  W Eksploratorze magazynu (wersja Preview) kliknij prawym przyciskiem myszy konto miejsca do magazynowania, które będą udostępniane, a następnie-z menu kontekstowego — zaznacz **Uzyskać udostępnione podpis programu Access**.

    ![Uzyskiwanie opcji menu kontekstowego skojarzenia zabezpieczeń][13]

1. W oknie dialogowym **Podpis dostępu do udostępnionych** określić przedział czasu i uprawnienia, które chcesz dla konta, a następnie wybierz pozycję **Utwórz**.

    ![Okno dialogowe skojarzenia zabezpieczeń][14]
 
1. Druga **Podpisu dostępu udostępnione** zostanie wyświetlone okno dialogowe Wyświetlanie skojarzeń zabezpieczeń. Wybierz polecenie **Kopiuj** obok **Parametry połączenia** , aby skopiować go do Schowka. Wybierz pozycję **Zamknij** , aby zamknąć okno dialogowe.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Dołączanie do udostępnionej konto za pomocą skojarzenia zabezpieczeń

1.  W Eksploratorze magazynu (wersja Preview) zaznacz pole wyboru **Połącz z magazynem Azure**.

    ![Nawiązywanie połączenia z Azure przechowywania danych][23]

1.  W oknie dialogowym **Nawiązywanie połączenia z magazynem Azure** Określ parametry połączenia, a następnie wybierz przycisk **Dalej**.

    ![Nawiązywanie połączenia z okno dialogowe Azure miejsca do magazynowania][24] 

1.  W oknie dialogowym **Podsumowanie połączenia** Sprawdź informacje. Jeśli chcesz zmienić jakiekolwiek właściwości, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. Po zakończeniu wybierz pozycję **Połącz**.

1.  Po podłączeniu konta miejsca do magazynowania będą wyświetlane z tekstem (SA) dołączone do podanej nazwie konta.

    ![Powoduje dołączonych do konta przy użyciu skojarzenia zabezpieczeń][17]

## <a name="attach-service-using-sas"></a>Dołącz usługę za pomocą skojarzenia zabezpieczeń

Sekcję [Dołącz miejsca do magazynowania konta za pomocą skojarzenia zabezpieczeń](#attach-storage-account-using-sas) przedstawiono, jak administrator systemu Azure subskrypcji może udzielić tymczasowego dostępu do konta miejsca do magazynowania wygenerowanie (i udostępnianie) skojarzeń zabezpieczeń dla konta miejsca do magazynowania. Podobnie jest generowany skojarzeń zabezpieczeń dla określonej usługi (kontener obiektów blob, kolejki lub tabela) w ramach konta usługi miejsca do magazynowania.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Generowanie skojarzeń zabezpieczeń usługi, który chcesz udostępnić

W tym kontekście usługa może być kontenera obiektów blob, kolejki lub tabelę. W poniższych sekcjach opisano, jak wygenerować skojarzenia zabezpieczeń na liście usługi:

- [Pobieranie skojarzeń zabezpieczeń dla kontenera obiektów blob](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Pobieranie skojarzeń zabezpieczeń dla udziału plików — *już wkrótce*
- Pobieranie skojarzeń zabezpieczeń dla kolejki — *już wkrótce*
- Pobieranie skojarzeń zabezpieczeń dla tabeli — *już wkrótce*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Dołączanie do usługi udostępnionego konta przy użyciu skojarzenia zabezpieczeń

1.  W Eksploratorze magazynu (wersja Preview) zaznacz pole wyboru **Połącz z magazynem Azure**.

    ![Nawiązywanie połączenia z Azure przechowywania danych][23]

1.  W oknie dialogowym **Nawiązywanie połączenia z magazynem Azure** Podaj identyfikator URI skojarzenia zabezpieczeń, a następnie wybierz przycisk **Dalej**.

    ![Nawiązywanie połączenia z okno dialogowe Azure miejsca do magazynowania][24] 

1.  W oknie dialogowym **Podsumowanie połączenia** Sprawdź informacje. Jeśli chcesz zmienić jakiekolwiek właściwości, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. Po zakończeniu wybierz pozycję **Połącz**.

1.  Po podłączeniu serwisowych pojawi się w węźle **(Usługa SA)** .

    ![Wynik dołączaniu do usług udostępnionych za pomocą skojarzenia zabezpieczeń][20]

## <a name="search-for-storage-accounts"></a>Wyszukiwanie w przypadku kont miejsca do magazynowania

Jeśli masz długą listę kont miejsca do magazynowania, szybko odnaleźć konta określonego miejsca do magazynowania jest Użyj pola wyszukiwania w górnej części lewego okienka. 

Podczas wpisywania w polu wyszukiwania w okienku po lewej stronie zostaną wyświetlone jedynie miejsca do magazynowania konta pasujące do podanej wartości wyszukiwania, które wskazują. Na poniższym zrzucie ekranu przedstawiono przykład miejsce, w którym po przeszukaniu dla wszystkich kont miejsca do magazynowania, której nazwę konta magazynu zawiera tekst "tarcher".

![Wyszukiwanie konta miejsca do magazynowania][11]
    
Aby wyczyścić wyszukiwanie, wybierz przycisk **x** w polu wyszukiwania.

## <a name="next-steps"></a>Następne kroki
- [Zarządzanie zasobami magazyn obiektów blob platformy Azure Eksplorator magazynu (wersja Preview)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
