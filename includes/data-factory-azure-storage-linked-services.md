## <a name="azure-storage-linked-service"></a>Usługi połączone Azure miejsca do magazynowania

**Usługi połączone magazyn Azure** umożliwia łączenie klienta z magazynu Azure factory Azure danych przy użyciu **klucz konta**. Zapewnia factory danych globalny dostęp do magazynu Azure. Poniższa tabela zawiera opis specyficzne dla usługi Magazyn Azure połączone elementy JSON.

| Właściwość | Opis | Wymagane |
| :-------- | :----------- | :-------- |
| Typ | Ustaw właściwości Typ: **AzureStorage** | Tak |
| connectionString | Podaj informacje wymagane do nawiązania Azure miejsca do magazynowania dla właściwości connectionString. | Tak |

Zobacz następujący artykuł instrukcje w widoku i kopii klucz konta magazynu platformy Azure: [Widok, kopiowanie i klawisze dostępu wyniku miejsca do magazynowania](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Przykład:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Usługi połączone skojarzeń zabezpieczeń Azure miejsca do magazynowania  
Podpis udostępniania (SA) udostępnia delegowanej zasoby na koncie miejsca do magazynowania. Oznacza to, że możesz przyznać że klient ograniczone uprawnienia do obiektów na koncie miejsca do magazynowania przez określony czas i od określonych uprawnień, bez konieczności udostępniania kluczy dostępu do konta. Jest identyfikator URI obejmujący w jego parametry kwerendy wszystkie informacje niezbędne do uwierzytelniony dostęp do zasobu miejsca do magazynowania. Dostęp do miejsca do magazynowania zasobów z skojarzeń zabezpieczeń, klienta musi tylko w celu przekazania w skojarzeń zabezpieczeń do odpowiedniego konstruktora lub metody. Aby uzyskać szczegółowe informacje dotyczące skojarzeń zabezpieczeń, zobacz [podpisów dostępu udostępnione: opis modelu skojarzeń zabezpieczeń](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Usługa skojarzeń zabezpieczeń miejsca do magazynowania Azure połączone umożliwia łączenie klienta z miejsca do magazynowania Azure factory Azure danych za pomocą udostępnionego podpis programu Access (SA). Dzięki temu factory danych ograniczone/czas — powiązanych z dostępu do określonego/wszystkich zasobów (obiektów blob kontener) w magazynie. Poniższa tabela zawiera opis specyficzne dla usługi skojarzeń zabezpieczeń miejsca do magazynowania Azure połączone elementy JSON. 

| Właściwość | Opis | Wymagane |
| :-------- | :----------- | :-------- |
| Typ | Ustaw właściwości Typ: **AzureStorageSas**  | Tak |
| sasUri | Określ udostępnione URI podpisu dostęp do zasobów magazynowania Azure przykład obiektów blob, kontener lub tabeli. Zobacz notatki poniżej, aby uzyskać szczegółowe informacje. | Tak | 


**Przykład:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Podczas tworzenia **Skojarzenia zabezpieczeń URI**, uwzględniając następujące czynności:  

- Azure Factory danych obsługuje tylko **Skojarzenia zabezpieczeń usług**, nie skojarzeń zabezpieczeń konta. Zobacz [Typy z udostępnionych dostępu podpisy](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) szczegółowe informacje na temat tych dwóch typów.
- Odczytu/zapisu odpowiednie **uprawnienia** należy ustawić na obiektów według używania usługi połączone (Odczyt, zapis, odczytu/zapisu) w firmie danych.
- **Czas wygaśnięcia** musi być odpowiednio ustawione. Upewnij się, że dostęp do obiektów magazyn Azure nie wygaśnie w okresie aktywnego procesu.
- Identyfikator URI powinny być tworzone poziomie kontenera prawo/obiektów blob lub tabelę zgodnie z potrzebami. Identyfikator Uri skojarzenia zabezpieczeń do obiektów blob platformy Azure umożliwia dostęp do tej konkretnej obiektów blob usługi Factory danych. Identyfikator Uri skojarzenia zabezpieczeń do kontenera obiektów blob platformy Azure umożliwia usługę Factory danych do iteracji BLOB w danym kontenerze. Jeśli chcesz udostępnić więcej mniej obiektów później lub aktualizowanie Identifier skojarzeń zabezpieczeń, należy pamiętać o aktualizowaniu usługi połączone z nowego identyfikatora URI.   
