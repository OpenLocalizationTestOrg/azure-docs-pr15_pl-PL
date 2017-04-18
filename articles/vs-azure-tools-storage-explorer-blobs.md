<properties
    pageTitle="Zarządzanie zasobami magazyn obiektów Blob platformy Azure Eksplorator magazynu (wersja Preview) | Microsoft Azure"
    description="Zarządzanie kontenerami obiektów Blob platformy Azure i obiektów blob Eksplorator magazynu (wersja Preview)"
    services="storage"
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
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Zarządzanie zasobami magazyn obiektów Blob platformy Azure Eksplorator magazynu (wersja Preview)

## <a name="overview"></a>Omówienie

[Magazyn obiektów Blob platformy Azure](./storage/storage-dotnet-how-to-use-blobs.md) to usługa do przechowywania dużych ilości danych niestrukturalne, takie jak dane tekstowe lub binarne, które są dostępne z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS.
Magazyn obiektów Blob można użyć w celu udostępnienia danych publicznie do świata lub do przechowywania danych aplikacji prywatnie. W tym artykule dowiesz się, jak za pomocą Eksploratora magazynu (wersja Preview), aby praca z kontenerami obiektów blob i obiektów blob.

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, musisz następujące czynności:

- [Pobieranie i instalowanie Eksplorator magazynu (wersja preview)](http://www.storageexplorer.com)
- [Nawiązywanie połączenia z konta magazynu platformy Azure lub usługi](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Tworzenie kontenera obiektów blob

Wszystkie obiekty BLOB musi znajdować się w kontenerze obiektów blob, który jest po prostu logiczne grupowanie obiektów blob. Konta może zawierać dowolną liczbę kontenerów i każdego kontenera mogą zawierać dowolną liczbę obiektów blob.

Poniższe kroki przedstawiają sposób tworzenia obiektów blob kontenerze Eksplorator magazynu (wersja Preview).

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta miejsca do magazynowania, w którym chcesz utworzyć kontener obiektów blob.
1.  Kliknij prawym przyciskiem myszy **Obiektów Blob kontenery**, a - z menu kontekstowego — wybierz **Tworzenie kontenera obiektów Blob**.

    ![Tworzenie menu kontekstowego kontenerów obiektów blob][0]

1.  Pole tekstowe pojawi się pod folder **Containers obiektów Blob** . Wprowadź nazwę dla swojego kontenera obiektów blob. Zobacz sekcję [reguł nazewnictwa kontenera](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) dla listy reguł i ograniczeń na nazw obiektów blob kontenerów.

    ![Tworzenie pola tekstowego kontenerów obiektów Blob][1]

1.  Naciśnij klawisz **Enter** po zakończeniu tworzenia kontenera obiektów blob lub **Esc** , aby anulować. Po pomyślnym utworzeniu kontenera obiektów blob pojawi się w folderze **Kontenerów obiektów Blob** dla konta wybranego miejsca do magazynowania.

    ![Kontener obiektów blob utworzony][2]

## <a name="view-a-blob-containers-contents"></a>Wyświetlić zawartość kontenera obiektów blob

Kontenery obiektów blob zawierają obiektów blob i folderów (które może również zawierać obiektów blob).

Poniższe kroki pokazują jak wyświetlić zawartość kontenera obiektów blob w Eksploratorze magazynu (wersja Preview):

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob czy chcesz wyświetlić.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Kliknij prawym przyciskiem myszy kontener obiektów blob, który chcesz wyświetlić, a - z menu kontekstowego — wybierz **Otwórz Edytor kontenera obiektów Blob**.
Możesz również kliknąć dwukrotnie kontenera obiektów blob, który chcesz wyświetlić.

    ![Menu kontekstowego Edytor kontenera otwartych obiektów blob][19]

1.  W okienku głównym zostaną wyświetlone zawartości kontenera obiektów blob.

    ![Edytor kontenera obiektów blob][3]

## <a name="delete-a-blob-container"></a>Usuwanie kontenera obiektów blob

Kontenery obiektów blob można łatwo tworzyć i usunięte stosownie do potrzeb. (Można znaleźć w sekcji [Zarządzanie obiektami BLOB w kontenerze obiektów blob](./#managing-blobs-in-a-blob-container)przedstawiono, jak usunąć poszczególnych obiektów blob,).

Poniższe kroki pokazują jak usunąć kontenera obiektów blob w Eksploratorze magazynu (wersja Preview):

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob czy chcesz wyświetlić.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Kliknij prawym przyciskiem myszy kontener obiektów blob, który chcesz usunąć, a - z menu kontekstowego — wybierz polecenie **Usuń**.
Można również nacisnąć, **Usuń** , aby usunąć kontenera aktualnie zaznaczonych obiektów blob.

    ![Usuwanie menu kontekstowego kontenera obiektów blob][4]

1.  Wybierz pozycję **Tak** , aby okno dialogowe potwierdzenia.

    ![Usuwanie obiektów blob potwierdzenia kontenera][5]

## <a name="copy-a-blob-container"></a>Kopiowanie kontenera obiektów blob

Eksplorator magazynu (wersja Preview) umożliwia skopiowanie kontenera obiektów blob do Schowka, a następnie wklej kontenera obiektów blob na inne konto miejsca do magazynowania. (Aby wyświetlić jak skopiować poszczególnych obiektów blob, zajrzyj do sekcji [Zarządzanie obiektami BLOB w kontenerze obiektów blob](./#managing-blobs-in-a-blob-container).)

Poniższe kroki przedstawiają jak skopiować kontenera obiektów blob z jednego miejsca do magazynowania konta do innego.

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob czy chcesz skopiować.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Kliknij prawym przyciskiem myszy kontener obiektów blob, który chcesz skopiować, a - z menu kontekstowego — wybierz **Kontener obiektów Blob Kopiuj**.

    ![Menu kontekstowe Kopiowanie obiektów blob kontenera][6]

1.  Kliknij prawym przyciskiem myszy konta miejsca do magazynowania odpowiednie "docelowego", w którym chcesz wkleić kontenera obiektów blob, a - z menu kontekstowego — wybierz **Wklej kontenera obiektów Blob**.

    ![Menu kontekstowe kontenera obiektów blob wklejania][7]

## <a name="get-the-sas-for-a-blob-container"></a>Pobieranie skojarzeń zabezpieczeń dla kontenera obiektów blob

[Podpis udostępniania (SA)](./storage/storage-dotnet-shared-access-signature-part-1.md) udostępnia delegowanej zasobów na koncie miejsca do magazynowania.
Oznacza to, że możesz przyznać że klient ograniczone uprawnienia do obiektów na koncie miejsca do magazynowania przez określony czas i określony zestaw uprawnień, bez konieczności udostępniania kluczy dostępu do konta.

Poniższe kroki pokazują sposób tworzenia skojarzenia zabezpieczeń kontenera obiektów blob:

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob, dla którego chcesz uzyskać skojarzeń zabezpieczeń.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Kliknij prawym przyciskiem myszy kontener odpowiedniej obiektów blob, a - z menu kontekstowego — zaznacz **Uzyskać udostępnione podpis programu Access**.

    ![Uzyskiwanie menu kontekstowego skojarzenia zabezpieczeń][8]

1.  W oknie dialogowym **Podpis dostępu do udostępnionych** Określ zasady, daty rozpoczęcia i wygaśnięcia, strefa czasowa, a dostęp do poziomów, które mają dla zasobu.

    ![Opcje skojarzenia zabezpieczeń][9]

1.  Po zakończeniu określania opcji skojarzeń zabezpieczeń wybierz pozycję **Utwórz**.

1.  Druga okno dialogowe **Udostępnione podpisu dostępu** będzie wyświetlana zawierającym kontenera obiektów blob wraz z adresu URL i QueryStrings można uzyskać dostęp do zasobu miejsca do magazynowania.
Wybierz polecenie **Kopiuj** obok adresu URL mają zostać skopiowane do Schowka.

    ![Kopiowanie skojarzeń zabezpieczeń adresów URL][10]

1.  Po zakończeniu wybierz pozycję **Zamknij**.

## <a name="manage-access-policies-for-a-blob-container"></a>Zarządzanie zasadami dostępu dla kontenera obiektów blob

Poniższe kroki przedstawiają sposób zarządzania (Dodawanie i usuwanie) zasady kontenera obiektów blob dostępu:

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob zasady dostępu, których chcesz zarządzać.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Wybierz kontener odpowiedniej obiektów blob, a - z menu kontekstowego — wybierz **Zarządzanie zasadami dostępu**.

    ![Zarządzanie menu kontekstowego zasady dostępu][11]

1.  Okno dialogowe **Zasady dostępu** znajduje się lista wszelkie zasady dostępu utworzone dla kontenera zaznaczonych obiektów blob.

    ![Opcje zasad programu Access][12]        

1.  Wykonaj poniższe czynności w zależności od zadania zarządzania zasady dostępu:

    - **Dodaj nową zasadę dostępu** — wybierz pozycję **Dodaj**. Wygenerowany, okno dialogowe **Zasady dostępu** będzie wyświetlany zasady nowo dodany dostępu (z ustawienia domyślne).
    - **Edytować zasady dostępu** - wprowadź odpowiednie zmiany, a następnie wybierz przycisk **Zapisz**.
    - **Usuwanie zasad dostępu** — wybierz pozycję **Usuń** obok zasady dostępu, które chcesz usunąć.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Ustawianie poziomu dostępie dla kontenera obiektów blob

Domyślnie każdy kontener obiektów blob jest równa "Nie publicznego dostępu".

Poniższe kroki przedstawiają określania poziomu dostępie dla kontenera obiektów blob.

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob zasady dostępu, których chcesz zarządzać.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Wybierz kontener odpowiedniej obiektów blob, a - z menu kontekstowego — wybierz **Ustaw poziom dostępu publicznego**.

    ![Menu kontekstowe poziomu dostępie zestawu][13]

1.  W oknie dialogowym **Ustawianie poziomu dostępie kontenera** Określ poziom dostępu odpowiedniej.

    ![Ustawianie opcji poziomu dostępu publicznego][14]

1.  Wybierz opcję **Zastosuj**.

## <a name="managing-blobs-in-a-blob-container"></a>Zarządzanie obiektami BLOB w kontenerze obiektów blob

Po utworzeniu kontenera obiektów blob możesz przekazać obiektów blob do kontenera obiektów blob, Pobierz obiektów blob na komputerze lokalnym, otwieranie obiektów blob na komputerze lokalnym i wiele innych.

Poniższe kroki przedstawiają jak zarządzać obiektami BLOB (i foldery) w kontenerze obiektów blob.

1.  Otwórz Eksploratora magazynu (wersja Preview).
1.  W okienku po lewej stronie rozwiń konta magazynu zawierającego kontener obiektów blob pewno chcesz zarządzać.
1.  Rozwijanie konta magazynu **Obiektów Blob kontenerów**.
1.  Kliknij dwukrotnie kontenera obiektów blob, który chcesz wyświetlić.
1.  W okienku głównym zostaną wyświetlone zawartości kontenera obiektów blob.

    ![Kontener obiektów blob widoku][3]

1.  W okienku głównym zostaną wyświetlone zawartości kontenera obiektów blob.

1.  Wykonaj poniższe czynności w zależności od zadania, które chcesz wykonać:

    - **Przekazywanie plików do kontenera obiektów blob**

        1.  Na głównym okienko narzędzi wybierz pozycję **Przekaż**, a następnie polecenie **Przekaż** z menu rozwijanego.

            ![Przekazywanie plików menu][15]

        1.  W oknie dialogowym **przekazywanie plików** kliknij przycisk wielokropka (****...) po prawej stronie pola tekstowego **plików** zaznacz pliki, które chcesz przekazać.

            ![Przekazywanie plików opcji][16]

        1.  Określ typ **obiektów Blob**. [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) wyjaśniono: różnice między różne typy obiektów blob.

        1.  Opcjonalnie określ folder docelowy, do którego będzie można przekazać wybranych plików. Jeśli folder docelowy nie istnieje, zostanie on utworzony.

        1.  Wybierz pozycję **Przekaż**.

    - **Przekazywanie folderu do kontenera obiektów blob**

        1.  Na głównym okienko narzędzi wybierz pozycję **Przekaż**, a następnie polecenie **Przekaż Folder** z menu rozwijanego.

            ![Folder menu Przekaż][17]

        1.  W oknie dialogowym **folder przekazywania** kliknij przycisk wielokropka (****...) po prawej stronie pola tekstowego **Folder** , aby wybrać folder, którego zawartość chcesz przekazać.

            ![Przekazywanie aplet Opcje folderów][18]

        1.  Określ typ **obiektów Blob**. [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) wyjaśniono: różnice między różne typy obiektów blob.

        1.  Opcjonalnie określ folder docelowy, do którego będzie można przekazać zawartość zaznaczonego folderu. Jeśli folder docelowy nie istnieje, zostanie on utworzony.

        1.  Wybierz pozycję **Przekaż**.

    - **Pobierz obiektów blob na komputerze lokalnym**

        1.  Zaznaczanie obiektów blob, który chcesz pobrać.

        1.  Na pasku narzędzi głównego okienka wybierz pozycję **Pobieranie**.

        1.  W oknie dialogowym **Określ lokalizację zapisu pobranych obiektów blob** określić lokalizację, w której blob pobrany, a następnie nazwę, którą chcesz nadać mu.  

        1.  Wybierz przycisk **Zapisz**.

    - **Otwieranie obiektów blob na komputerze lokalnym**

        1.  Zaznaczanie obiektów blob, który chcesz otworzyć.

        1.  Na pasku narzędzi głównego okienka wybierz polecenie **Otwórz**.

        1.  To będą pobierane i otworzyć przy użyciu aplikacji skojarzonej z obiektów blob źródłowych typ pliku.

    - **Kopiowanie obiektów blob do Schowka.**

        1.  Zaznaczanie obiektów blob, który chcesz skopiować.

        1.  Na pasku narzędzi głównego okienka wybierz polecenie **Kopiuj**.

        1.  W okienku po lewej stronie przejdź do drugiego kontenera obiektów blob i kliknij dwukrotnie, aby go wyświetlić w okienku głównym.

        1.  Na pasku narzędzi głównego okienka wybierz pozycję **Wklej** , aby utworzyć kopię obiektów blob.

    - **Usuwanie obiektów blob**

        1.  Zaznaczanie obiektów blob, który chcesz usunąć.

        1.  Na pasku narzędzi głównego okienka wybierz pozycję **Usuń**.

        1.  Wybierz pozycję **Tak** , aby okno dialogowe potwierdzenia.

## <a name="next-steps"></a>Następne kroki

- Wyświetlanie [najnowszych Eksplorator magazynu (wersja Preview) informacje o wersji i klipy wideo](http://www.storageexplorer.com).
- Więcej sposobów [tworzenia aplikacji przy użyciu obiektów blob platformy Azure, tabele, kolejki i pliki](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png