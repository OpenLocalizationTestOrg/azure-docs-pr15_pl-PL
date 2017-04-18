<properties 
    pageTitle="Konfigurowanie parametrów połączenia z magazynem Azure | Microsoft Azure"
    description="Konfigurowanie parametrów połączenia z klientem Azure miejsca do magazynowania. Parametry połączenia zawiera informacje potrzebne do uwierzytelniania dostępu do konta miejsca do magazynowania z aplikacji w czasie rzeczywistym."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Konfigurowanie parametry połączenia Azure miejsca do magazynowania

## <a name="overview"></a>Omówienie

Parametry połączenia zawiera informacje uwierzytelniające, aby uzyskać dostęp do danych przy użyciu konta z miejsca do magazynowania Azure z aplikacji w czasie rzeczywistym. Możesz skonfigurować parametry połączenia do:

- Połącz ze emulatora Azure miejsca do magazynowania.
- Dostępu do konta magazynu platformy Azure.
- Dostęp do określonych zasobów w Azure za pomocą podpisu udostępniania (SA).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Przechowywanie ciąg połączenia

Aplikacja będzie konieczne dostęp do parametrów połączenia w czasie rzeczywistym w celu uwierzytelnienia żądania magazyn Azure. Masz kilka różnych opcji przechowywania ciąg połączenia:

- Do uruchamiania aplikacji na komputerze lub na urządzeniu, można przechowywać w ciągu połączenia `app.config `pliku lub `web.config` pliku. Dodawanie parametrów połączenia do sekcji **AppSettings** .
- Dla aplikacji działa usługa w chmurze Azure mogą zawierać ciąg połączenia w [pliku schematu (.cscfg) konfiguracji usługi Azure](https://msdn.microsoft.com/library/ee758710.aspx). Dodawanie parametrów połączenia do sekcji **appSettings** plik konfiguracyjny usługi.
- Za pomocą ciągu połączenia bezpośrednio w kodzie. Dla większości scenariuszy jednak zaleca się przechowywanie ciągu konfiguracji w pliku konfiguracji.

Przechowywanie ciąg połączenia w pliku konfiguracji ułatwia aktualizowanie parametry połączenia, aby przełączać się między emulatora miejsca do magazynowania i konto Azure magazynu w chmurze. Musisz edytować parametry połączenia, aby wskazywały swoim środowisku docelowym.

Klasy [Menedżer konfiguracji programu Microsoft Azure](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) umożliwia dostęp do ciągu połączenia w czasie wykonywania niezależnie od tego, w którym jest uruchomiona aplikacja.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Tworzenie parametrów połączenia do emulatora miejsca do magazynowania

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Aby uzyskać więcej informacji na temat emulatora miejsca do magazynowania, zobacz [Używanie Azure emulatora miejsca do magazynowania dla rozwoju i testowanie](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Tworzenie parametrów połączenia z klientem Azure miejsca do magazynowania

Aby utworzyć parametry połączenia z kontem Azure miejsca do magazynowania, użyj poniższych format ciągu połączenia. Wskazuje, czy chcesz utworzyć połączenie z kontem miejsca do magazynowania za pośrednictwem protokołu HTTPS (zalecane) lub HTTP, Zamień `myAccountName` z nazwą swojego konta miejsca do magazynowania i Zamień `myAccountKey` przy użyciu klucza dostępu do konta:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Na przykład ciąg połączenia będzie wyglądać podobnie do następujących parametrów połączenia przykładowe:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Magazyn Azure obsługuje HTTP i HTTPS w parametrach połączenia; Jednak przy użyciu protokołu HTTPS jest zalecany.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Tworzenie parametrów połączenia, korzystając z podpisu udostępniania

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Tworzenie parametrów połączenia do punktu końcowego jawne miejsca do magazynowania

Punkty końcowe usługi można określić jawnie w ciągu połączenia zamiast domyślnych punktów końcowych. Aby utworzyć ciąg połączenia, który określa punktu końcowego jawne, określ punktu końcowego usługi wykonane dla każdej usługi, w tym specyfikacji protocol (protokół HTTPS (zalecane) lub protokołu HTTP) w następującym formacie:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Jeden scenariusz miejsce, w którym możesz określić punkt końcowy jawne to, jeśli masz mapowane punkt końcowy magazyn obiektów Blob na domenę niestandardową. W takim przypadku możesz określić punkt końcowy niestandardowych dla magazyn obiektów Blob w ciągu połączenia i opcjonalnie określ punkty końcowe domyślne innej usługi, jeśli aplikacja z nich korzysta.

Poniżej przedstawiono przykłady ciągów prawidłowe połączenie, określających jawne punktu końcowego usługi obiektów Blob:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Wartość punktu końcowego, który jest wyświetlany w parametrach połączenia należy utworzyć żądanie URI z usługą obiektów Blob, a jego decyduje o tym formularzu dowolnego identyfikatorów, które są zwracane do kodu.

Należy zauważyć, że jeśli wybierzesz pominąć punktu końcowego usługi z ciągu połączenia, następnie nie będzie mógł korzystać z tego ciągu połączenia dostęp do danych w tej usłudze z kodu.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Tworzenie parametrów połączenia z sufiksem punktu końcowego

Aby utworzyć parametry połączenia dla usługi miejsca do magazynowania w regionach lub wystąpień sufiksy różnych punktu końcowego, takich jak Chiny Azure lub zarządzania Azure, użyj następującego formatu ciągu połączenia. Wskazuje, czy chcesz nawiązać połączenie z kontem miejsca do magazynowania za pośrednictwem protokołu HTTP lub HTTPS, Zamień `myAccountName` zamienić nazwę konta magazynu `myAccountKey` z klucz dostępu do konta i Zamień `mySuffix` z sufiksem identyfikatora URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Na przykład ciąg połączenia powinien wyglądać podobnie do następujący ciąg połączenia:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Podczas analizowania ciągu połączenia

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Następne kroki

- [Używanie emulatora Azure miejsca do magazynowania dla projektowania i testowania](storage-use-emulator.md)
- [Eksploratorów Azure miejsca do magazynowania](storage-explorers.md)
- [Korzystanie z podpisów udostępniania (SA)](storage-dotnet-shared-access-signature-part-1.md)
