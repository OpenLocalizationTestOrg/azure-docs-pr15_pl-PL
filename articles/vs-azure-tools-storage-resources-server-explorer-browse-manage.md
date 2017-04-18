<properties
   pageTitle="Przeglądanie i zarządzanie zasobami magazynowania w Eksploratorze serwera | Microsoft Azure"
   description="Przeglądanie i zarządzanie zasobami magazynowania w Eksploratorze serwera"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Przeglądanie i zarządzanie zasobami magazynowania w Eksploratorze serwera

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Omówienie
Po zainstalowaniu narzędzia Azure dla programu Microsoft Visual Studio, możesz wyświetlić z kont magazyn obiektów blob, kolejki i dane tabeli dla Azure. Węzeł magazyn Azure w Eksploratorze serwera są wyświetlane dane, konta emulatora magazynu lokalnego oraz innych kont Azure miejsca do magazynowania.

Aby wyświetlić Server Explorer w programie Visual Studio, na pasku menu, wybierz polecenie **Widok** **Eksploratora serwera**. Węzeł magazynu pokazuje wszystkie konta miejsca do magazynowania, istniejące w obszarze każdego Azure subskrypcji certyfikat, który masz połączenie. Jeśli nie widać konta miejsca do magazynowania, możesz dodać go, wykonując instrukcje [w dalszej części tego tematu](#add-storage-accounts-by-using-server-explorer).

Uruchamianie w Azure SDK 2.7, umożliwia także nowe Eksploratora chmury do wyświetlania i zarządzania zasobami Azure. Aby uzyskać więcej informacji, zobacz [Zarządzanie zasobami Azure w Eksploratorze chmury](./vs-azure-tools-resources-managing-with-cloud-explorer.md) .


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Wyświetlanie i zarządzanie zasobami magazynowania w programie Visual Studio

Eksplorator serwera automatycznie lista obiektów blob, kolejki i tabele na koncie emulatora miejsca do magazynowania. Konto emulatora miejsca do magazynowania jest widoczne w Eksploratorze serwera węźle miejsca do magazynowania jako węzeł **rozwoju** .

Aby wyświetlić zasoby konta emulatora miejsca do magazynowania, rozwiń węzeł **rozwoju** . Jeśli nie został uruchomiony emulator miejsca do magazynowania, gdy rozwiń węzeł **rozwoju** , będzie automatycznie uruchamiany. To może potrwać kilka sekund. Możesz nadal pracować w innych obszarach programu Visual Studio podczas uruchamiania emulatora miejsca do magazynowania.

Aby wyświetlić zasoby na koncie miejsca do magazynowania, rozwiń węzeł konta miejsca do magazynowania w Eksploratorze serwera. Są wyświetlane następujące węzły podrzędne:

- BLOB

- Kolejki

- Tabele

## <a name="work-with-blob-resources"></a>Praca z zasobami obiektów Blob

Węzeł obiektów blob Wyświetla listę kontenerów dla konta wybranego miejsca do magazynowania. Kontenery obiektów blob obiektów blob plikami i tych obiektów blob można organizować w folderach i podfolderach. Aby uzyskać więcej informacji, zobacz [jak za pomocą magazyn obiektów Blob z .NET](./storage/storage-dotnet-how-to-use-blobs.md) .

### <a name="to-create-a-blob-container"></a>Aby utworzyć kontener obiektów blob

1. Otwieranie menu skrótów dla węzła **obiektów blob** , a następnie wybierz **Tworzenie kontenera obiektów Blob**.

1. Wprowadź nazwę nowego kontenera w oknie dialogowym **Tworzenie kontenera obiektów Blob** , a następnie wybierz przycisk **Ok**

    >[AZURE.NOTE] Nazwa kontenera obiektów blob musi się zaczynać od liczby (0 – 9) lub małą literę (a-z).

### <a name="to-delete-a-blob-container"></a>Aby usunąć kontenera obiektów blob

- Otwieranie menu skrótów dla kontenera obiektów blob, który chcesz usunąć, a następnie wybierz pozycję **Usuń**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Aby wyświetlić listę elementów zawartych w kontenerze obiektów blob

- Otwieranie menu skrótów dla nazwy kontenera obiektów blob na liście, a następnie wybierz **Widok obiektów Blob kontener**.

    Podczas wyświetlania zawartości kontenera obiektów blob, pojawi się na karcie znana pod nazwą Widok kontenera obiektów blob.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    Za pomocą przycisków w prawym górnym rogu widoku kontenera obiektów blob, można wykonywać następujące operacje na obiektów blob:

    - Wprowadź wartość filtru i zastosować go

    - Odświeżenie listy obiektów blob w kontenerze

    - Przekazywanie pliku

    - Usuwanie obiektów blob

      >[AZURE.NOTE] Usuwanie pliku kontenera obiektów blob nie powoduje usunięcia pliku źródłowym. powoduje tylko usunięcie go z kontenera obiektów blob.

    - Otwieranie obiektów blob

    - Zapisywanie obiektu blob na komputerze lokalnym

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Tworzenie folderu lub podfolderu w kontenerze obiektów blob

1. Wybierz kontener obiektów blob w Eksploratorze serwera. W oknie kontenera kliknij przycisk **Przekaż obiektów Blob** .

    ![Przekazywanie pliku do folderu obiektów blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. W oknie dialogowym **Przekaż nowy plik** kliknij przycisk **Przeglądaj** , aby określić plik, który chcesz przekazać, a następnie wprowadź nazwę folderu w polu **Folder (opcjonalnie)** .

    Możesz dodać podfolderów w folderach kontenera, postępując zgodnie z procedurą. Jeśli nie określisz nazwę folderu, pliku zostaną przekazane na najwyższym poziomie kontenera obiektów blob. Plik pojawi się w określonym folderze w kontenerze.

    ![Folder dodany do kontenera obiektów blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Kliknij dwukrotnie folder lub naciśnij klawisz ENTER, aby wyświetlić zawartość folderu. Gdy wszystko będzie już w folderze kontenera, możesz przejść powrót do głównego kontenera, wybierając pozycję przycisku **Otwórz katalog nadrzędny** (Strzałka w górę).

### <a name="to-delete-a-container-folder"></a>Aby usunąć folder kontenera

 - Usuń wszystkie pliki w folderze

    >[AZURE.NOTE] Ponieważ foldery w kontenerach obiektów blob to foldery wirtualne, nie można utworzyć Opróżnij folder nie można usunąć folder, aby usunąć jej zawartość pliku. Musisz usunąć całą zawartość folderu, aby usunąć folder.

### <a name="to-filter-blobs-in-a-container"></a>Aby filtrować obiektów blob w kontenerze

Można filtrować obiektów blob, które są wyświetlane, określając Wspólny prefiks.

Na przykład w przypadku wprowadzenia prefiks `hello` w tekście filtru pole, a następnie wybierz pozycję **Wykonywanie** (****!) przycisk, są wyświetlane tylko obiektów blob, które zaczynają się od "Witaj".

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Pole filtru jest uwzględniana wielkość liter i nie obsługuje filtrowanie przy użyciu symboli wieloznacznych. Blob można filtrować tylko według prefiksu. Prefiks może zawierać ogranicznika, jeśli korzystasz z ogranicznika do organizowania obiektów blob w wirtualnej hierarchii. Na przykład filtrowanie prefiks HelloFabric / zwraca wszystkie obiekty BLOB rozpoczynające się od tego ciągu.

### <a name="to-download-blob-data"></a>Aby pobrać dane obiektów blob

- W **Eksploratorze serwera**Otwórz menu skrótów dla jednego lub więcej obiektów blob i następnie wybierz pozycję **Otwórz**, wybierz nazwę obiektów blob, a następnie wybierz przycisk **Otwórz** lub kliknij dwukrotnie nazwę obiektów blob.

    Informacje o postępie pobierania obiektów blob zostanie wyświetlone okno **Dziennik Azure** .

    To zostanie otwarty w edytorze domyślnych dla plików tego typu. Jeśli system operacyjny rozpoznaje typ pliku, plik zostanie otwarty w lokalnie zainstalowanej aplikacji; w przeciwnym razie zostanie wyświetlony monit, wybierz aplikację, która jest odpowiedni dla typ pliku to. Lokalny plik, który jest tworzona po pobraniu obiektów blob jest oznaczony jako tylko do odczytu.

    Dane BLOB są buforowane lokalnie i sprawdza, czy obiektów blob godzina ostatniej modyfikacji w usłudze obiektów Blob. Jeśli to została zaktualizowana od czasu ostatniego pobrania, będzie można pobrać ponownie; w przeciwnym razie to zostanie załadowana z dysku. Domyślnie obiektów blob są pobierane do katalogu tymczasowego. Aby pobrać obiektów blob do określonego katalogu, otwórz menu skrótów nazw zaznaczonych obiektów blob i wybierz polecenie **Zapisz jako**. Po zapisaniu obiektów blob w ten sposób plik obiektów blob nie jest otwarty, a plik lokalny jest tworzone atrybuty odczytu i zapisu.

### <a name="to-upload-blobs"></a>Aby przekazać obiektów blob

- Wybierz przycisk **Przekaż obiektów Blob** , gdy kontener jest otwarta do wyświetlania w widoku kontenera obiektów blob.

    Możesz wybrać jeden lub więcej plików, aby przekazać, a możesz przekazać pliki dowolnego typu. **Dziennik Azure** przedstawia postęp przekazywania. Aby uzyskać więcej informacji na temat pracy z danymi obiektów blob, Dowiedz się, [jak korzystać z usługi Magazyn obiektów Blob Azure w .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Aby wyświetlić dzienniki przeniesione do obiektów blob

- Jeśli używasz diagnostyki Azure do rejestrowania danych z usługi Azure aplikacji i dzienniki zostały przeniesione do swojego konta miejsca do magazynowania, zobaczysz kontenerach, które zostały utworzone przez Azure dla tych dzienników. Wyświetlanie dzienników w Eksploratorze serwera jest łatwym sposobem identyfikowanie problemów związanych z aplikacją, zwłaszcza jeśli są rozmieszczone w Azure. Aby uzyskać więcej informacji na temat diagnostyki Azure zobacz [Zbieranie rejestrowania danych przy użyciu narzędzia diagnostyczne Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Aby uzyskać adres URL dla obiektów blob

- Otwieranie menu skrótów obiektów blob, a następnie wybierz pozycję **Kopiuj adres URL**.

### <a name="to-edit-a-blob"></a>Aby edytować obiektów blob

- Zaznacz to, a następnie wybierz przycisk **Otwórz obiektów Blob** .

    Plik jest pobierana tymczasową lokalizację i otworzyć na komputerze lokalnym. Musisz ponownie przekazać to po wprowadzeniu zmian.

## <a name="work-with-queue-resources"></a>Praca z zasobami kolejki

Kolejek usług miejsca do magazynowania są obsługiwane przy użyciu konta z miejsca do magazynowania Azure i używać ich umożliwia do chmury usługi ról komunikowanie się ze sobą i z innymi usługami przy przekazywania mechanizmu komunikat. Możesz programowy dostęp kolejki za pośrednictwem usługi w chmurze i przez usługi sieci web dla klientów zewnętrznych. Kolejki można także przejść bezpośrednio za pomocą Eksploratora serwera w programie Visual Studio.

Podczas opracowywania usługi w chmurze, która korzysta z kolejek można Visual Studio umożliwia tworzenie kolejek i pracować z nimi interaktywnie podczas opracowywania i testowania kodu.

W Eksploratorze Server można wyświetlić kolejek na koncie miejsca do magazynowania, tworzenie i usuwanie kolejek, Otwórz kolejkę, aby wyświetlić swoje wiadomości i Dodaj wiadomości do kolejki. Po otwarciu kolejki do wyświetlania można wyświetlać poszczególne wiadomości, a w kolejce za pomocą przycisków w lewym górnym rogu można wykonywać następujące czynności:

- Odśwież widok kolejki

- Dodaj wiadomość do kolejki

- Usuwania z kolejki górnej wiadomości.

- Czyszczenie całej kolejki

Poniższa ilustracja przedstawia kolejki, która zawiera dwa wiadomości.

![Wyświetlanie kolejki](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Aby uzyskać więcej informacji na temat przestrzeni dyskowej usługi kolejek, zobacz [sposoby: za pomocą usługi Magazyn kolejki](http://go.microsoft.com/fwlink/?LinkID=264702). Aby informacji na temat usługi sieci web dla kolejek usług miejsca do magazynowania zobacz [Kolejki usługi pojęcia](http://go.microsoft.com/fwlink/?LinkId=264788). Aby uzyskać informacje dotyczące wysyłania wiadomości do miejsca do magazynowania kolejki usługi przy użyciu programu Visual Studio zobacz [Wysyłanie wiadomości do kolejki usługi miejsca do magazynowania](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Miejsca do magazynowania usług kolejek różni się od kolejek bus usług. Aby uzyskać więcej informacji na temat kolejek bus usług Zobacz kolejek Bus usług, tematy i subskrypcji.

## <a name="work-with-table-resources"></a>Praca z zasobami tabeli

Usługa magazynu tabel platformy Azure przechowuje dużych ilości danych strukturalnych. Usługa działa, że NoSQL magazynu danych, która przyjmuje uwierzytelnione połączenia z wewnątrz i na zewnątrz Azure chmury. Tabele Azure to idealne rozwiązanie w przypadku przechowywania danych strukturalnych, -relacyjne.

### <a name="to-create-a-table"></a>Aby utworzyć tabelę

1. W Eksploratorze serwera wybierz węzeł **tabel** konta miejsca do magazynowania, a następnie wybierz **Create Table**.

1. W oknie dialogowym **Tworzenie tabeli** wprowadź nazwę dla tabeli.

### <a name="to-view-table-data"></a>Aby wyświetlić dane w tabeli

1. W Eksploratorze Server otwórz węzeł **Azure** , a następnie otwórz węzeł **miejsca do magazynowania** .

1. Otwórz węzeł konta miejsca do magazynowania, który Cię interesuje, a następnie otwórz węzeł **tabele** , aby wyświetlić listę tabel dla konta miejsca do magazynowania.

1. Otwieranie menu skrótów dla tabeli, a następnie wybierz pozycję **Wyświetl tabelę**.

    ![Azure tabeli w Eksploratorze rozwiązań](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tabela jest zorganizowane według jednostek (jak pokazano w wierszach) i właściwości (jak pokazano w kolumnach). Na przykład na poniższej ilustracji przedstawiono podmioty wymienione w **Projektanta tabel**:

### <a name="to-edit-table-data"></a>Aby edytować dane w tabeli

1. W oknie **Projektanta tabel**Otwieranie menu skrótów dla jednostki (pojedynczy wiersz) lub właściwość (jednej komórki), a następnie wybierz pozycję **Edytuj**.

    ![Dodawanie lub edytowanie podmiot tabeli](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Jednostki w jednej tabeli nie są wymagane do ten sam zestaw właściwości (kolumny). Pamiętaj o następujących ograniczeń na wyświetlanie i edytowanie danych tabeli.
    - Nie można wyświetlać ani edytować dane binarne (byte[]) typu, ale można ją zapisać w tabeli.

    - Nie można edytować wartości **PartitionKey** lub **RowKey** , ponieważ Magazyn tabel platformy Azure nie obsługuje tej operacji.

    - Nie można utworzyć właściwość o nazwie sygnatury czasowej, usług magazynu Azure za pomocą właściwość o tej nazwie.

    - Jeśli zostanie wprowadzona wartość DateTime, należy wykonać formatu, który należy ustawienia regionalne i językowe komputera (na przykład MM/DD/rrrr hh: mm: [AM | Po Południu] dla języka angielskiego Stany Zjednoczone).

### <a name="to-add-entities"></a>Aby dodać jednostek

1. W oknie **Projektanta tabel**wybierz przycisk **Dodaj jednostki** , który znajduje się w pobliżu w prawym górnym rogu widoku tabeli.

    ![Dodawanie jednostki](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. W oknie dialogowym **Dodawanie jednostki** wprowadź wartości właściwości **PartitionKey** i **RowKey** .

    ![Dodawanie obiektu, okno dialogowe](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Wprowadź wartości starannie, ponieważ nie można ich zmienić po zamknięciu okna dialogowego, chyba że usuwanie jednostki i dodaj go ponownie.

### <a name="to-filter-entities"></a>Aby filtrować jednostek

Możesz dostosować zestaw jednostek, które pojawiają się w tabeli, jeśli korzystasz z Konstruktora kwerend.

1. Aby otworzyć Konstruktora kwerend, otwórz tabelę do wyświetlania.

1. Kliknij przycisk po prawej stronie na pasku narzędzi widoku tabeli.

    Zostanie wyświetlone okno dialogowe **Konstruktora kwerend** . Na poniższej ilustracji przedstawiono kwerendę, która jest tworzone w Konstruktorze kwerend.

    ![Konstruktora kwerend](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Po zakończeniu tworzenia kwerendy, zamknij okno dialogowe. Jako filtr usługi danych WCF formularzu tekstu kwerendy zostanie wyświetlony w polu tekstowym.

1. Aby uruchomić kwerendę, wybierz ikonę zielony trójkąt.

    Można również filtrować jednostki danych wyświetlanych w **Projektanta tabel** w przypadku wprowadzenia ciąg filtru usługi danych WCF bezpośrednio w polu Filtr. Tego typu ciąg jest podobna do klauzuli WHERE języka SQL, ale jest wysyłana do serwera jako żądanie HTTP. Aby uzyskać informacje dotyczące sposobu tworzenia ciągów filtr zobacz [Tworzenia ciągów filtru z projektanta tabel](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Na poniższej ilustracji przedstawiono przykład ciąg prawidłowego filtru:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Odświeżanie danych miejsca do magazynowania

Gdy Server Explorer nawiązania połączenia lub pobiera dane z konta miejsca do magazynowania, może upłynąć w górę minuty na zakończenie operacji. Jeśli nie możesz połączyć się, operacji mogą przekraczać limit czasu. Podczas pobierania danych, możesz nadal pracować w innych miejscach programu Visual Studio. Aby anulować operację, jeśli trwa zbyt długo, wybierz przycisk **Zatrzymaj odświeżanie** na pasku narzędzi Server Explorer.

#### <a name="to-refresh-blob-container-data"></a>Aby odświeżyć dane kontenera obiektów blob

- Wybierz węzeł **obiektów blob** poniżej **miejsca do magazynowania** i wybierz przycisk **Odśwież** na pasku narzędzi Server Explorer.

- Aby odświeżyć listę obiektów blob, która jest wyświetlana, kliknij przycisk **Wykonaj** .

#### <a name="to-refresh-table-data"></a>Aby odświeżyć dane w tabeli

- Wybierz węzeł **tabele** poniżej **miejsca do magazynowania** i wybierz przycisk **Odśwież** .

- Aby odświeżyć na liście obiektów wyświetlanych w **Projektanta tabel**, wybierz przycisk **Wykonywanie** na **Projektanta tabel**.

#### <a name="to-refresh-queue-data"></a>Aby odświeżyć dane w kolejce

- Wybierz węzeł **kolejek** , a następnie wybierz przycisk **Odśwież** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Aby odświeżyć wszystkie elementy na koncie miejsca do magazynowania

- Wybierz nazwę konta, a następnie wybierz przycisk **Odśwież** na pasku narzędzi programu Explorer serwera.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Dodawanie miejsca do magazynowania kont za pomocą Eksploratora serwera

Istnieją dwa sposoby dodawania konta miejsca do magazynowania za pomocą Eksploratora serwera. Można utworzyć nowe konto miejsca do magazynowania w ramach subskrypcji Azure lub można dołączać istniejącego konta miejsca do magazynowania.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Aby utworzyć nowe konto miejsca do magazynowania przy użyciu Eksploratora serwera

1. W Eksploratorze serwera Otwieranie menu skrótów dla węzła miejsca do magazynowania, a następnie wybierz Utwórz konto miejsca do magazynowania.

    ![Utwórz nowe konto Azure miejsca do magazynowania](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Wybierz lub wprowadź następujące informacje dotyczące nowego konta z miejsca do magazynowania w oknie dialogowym **Utwórz konto miejsca do magazynowania** .

    - Subskrypcja Azure, do którego chcesz dodać konto miejsca do magazynowania.

    - Nazwa, która ma być używany dla nowego konta miejsca do magazynowania.

    - Region lub grupa koligacji (na przykład zachód USA lub Azji Wschodniej).

    - Typ replikacji, którego chcesz użyć dla konta miejsca do magazynowania, takich jak Geo zbędne.

1. Wybierz pozycję **Utwórz**.

    Nowe konto miejsca do magazynowania zostanie wyświetlona na liście **miejsca do magazynowania** w Eksploratorze rozwiązań.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Aby dołączyć istniejące konto miejsca do magazynowania za pomocą Eksploratora serwera

1. W Eksploratorze serwera Otwieranie menu skrótów dla węzła Azure miejsca do magazynowania, a następnie wybierz **Dołączyć pamięci zewnętrznej**.

    ![Dodawanie istniejącego konta miejsca do magazynowania](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Wybierz lub wprowadź następujące informacje dotyczące nowego konta z miejsca do magazynowania w oknie dialogowym **Utwórz konto miejsca do magazynowania** .

    - Nazwa istniejącego konta miejsca do magazynowania, który chcesz dołączyć. Można wprowadzić nazwę lub wybierz ją z listy.

    - Klucz konta wybranego miejsca do magazynowania. Ta wartość jest zazwyczaj dostarczany po wybraniu konto miejsca do magazynowania. Jeśli chcesz Visual Studio do zapamiętania klucz konta miejsca do magazynowania, zaznacz pole klucza konta Zapamiętaj.

    - Protokół, który ma być używany do nawiązania połączenia z kontem miejsca do magazynowania, takich jak HTTP, HTTPS lub niestandardowe punktu końcowego. Aby uzyskać więcej informacji na temat niestandardowych punktów końcowych, zobacz [jak skonfigurować parametry połączenia](https://msdn.microsoft.com/library/azure/ee758697.aspx) .

### <a name="to-view-the-secondary-endpoints"></a>Aby wyświetlić pomocniczą punkty końcowe

- Jeśli utworzono konto miejsca do magazynowania przy użyciu opcji replikacji **Odczytu Geo zbędne** , możesz wyświetlić jego pomocniczej punkty końcowe. Otwórz menu skrótów dla nazwę konta, a następnie wybierz polecenie **Właściwości**.

    ![Punkty końcowe pomocniczej miejsca do magazynowania](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Aby usunąć konto miejsca do magazynowania Eksploratora serwera

- W Eksploratorze serwera Otwieranie menu skrótów dla nazwę konta, a następnie wybierz **Usuń**. Po usunięciu konta miejsca do magazynowania spowoduje usunięcie zapisanych najważniejsze informacje dla tego konta.

    >[AZURE.NOTE] Po usunięciu konta miejsca do magazynowania Eksploratora serwera nie wywiera wpływu na Twoje konto miejsca do magazynowania lub dane, które zawierają; po prostu usuwa odwołanie Eksploratora serwera. Aby trwale usunąć konto miejsca do magazynowania, użyj [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, że więcej informacji na temat korzystania z usług Azure miejsca do magazynowania, zobacz [Uzyskiwanie dostępu do usług magazynu Azure](https://msdn.microsoft.com/library/azure/ee405490.aspx).
