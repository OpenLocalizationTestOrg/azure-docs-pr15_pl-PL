<properties 
   pageTitle="Rozpoczynanie pracy z magazynu Lake danych za pomocą interfejsu API usługi REST | Microsoft Azure" 
   description="Wykonywanie operacji na magazynu Lake danych przy użyciu interfejsów API pozostałych WebHDFS" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą interfejsów API REST

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

W tym artykule dowiesz się, jak za pomocą danych Lake sklepu pozostałych API i interfejsów API pozostałych WebHDFS wykonywanie Zarządzanie kontem, a także operacje plików Azure danych Lake magazynu. Magazyn Lake danych Azure udostępnia własnej API pozostałych operacji zarządzania kontem. Ponieważ magazynu Lake danych jest zgodny z HDFS i Hadoop ekosystemu, ją obsługuje jednak za pomocą interfejsów API pozostałych WebHDFS dla operacji systemu plików.

>[AZURE.NOTE] Szczegółowe informacje na temat interfejsu API usługi REST obsługę magazynu Lake danych, zobacz [Azure Data Lake sklepu pozostałych API Reference](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Wymagania wstępne

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Tworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji Azure AD służą do uwierzytelniania aplikacji magazynu Lake danych z usługą Azure Active Directory. Istnieją różne metody do uwierzytelniania Azure AD, które są **uwierzytelniania użytkowników końcowych** lub **do usługi**. Instrukcje i uzyskać więcej informacji na temat uwierzytelniania Zobacz [uwierzytelniania z magazynu Lake danych za pomocą usługi Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [zawinięcie](http://curl.haxx.se/). W tym artykule Umożliwia zwinięcie przedstawiają sposoby nawiązywania połączeń interfejsu API usługi REST do konta magazynu Lake danych.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak uwierzytelniania za pomocą usługi Azure Active Directory

Za pomocą dwóch metod do uwierzytelniania za pomocą usługi Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Uwierzytelnianie użytkownika końcowego (interakcyjne)

W tym scenariuszu aplikacji monituje użytkownika, aby zalogować się i wszystkie operacje są wykonywane w kontekście użytkownika. Interakcyjne uwierzytelniania, należy wykonać następujące czynności.

1. Za pomocą aplikacji przekierować użytkownika do następujący adres URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Identyfikator URI PRZEKIERUJ > musi być zakodowany do użycia w adresie URL. Tak, aby https://localhost, użyj `https%3A%2F%2Flocalhost`)

    Do tego samouczka można zamienić wartości symbol zastępczy w adresie URL powyżej i wklej je na pasku adresu przeglądarki sieci web. Nastąpi przekierowanie do uwierzytelniania za pomocą usługi Azure logowania. Po pomyślnym zalogowaniu odpowiedzi jest wyświetlany na pasku adresu przeglądarki. Odpowiedź będzie w następującym formacie:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Rejestrowanie kodu autoryzacji z odpowiedzi. Ten samouczek możesz skopiować kod autoryzacji na pasku adresu przeglądarki sieci web i przekazać je w wezwaniu na WPIS token punktu końcowego, tak jak pokazano poniżej:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] W tym przypadku \<identyfikatora URI PRZEKIERUJ > nie muszą być kodowane.

3. Odpowiedź jest obiektem JSON, który zawiera token dostępu (np. `"access_token": "<ACCESS_TOKEN>"`) i token odświeżania (np `"refresh_token": "<REFRESH_TOKEN>"`). Aplikacja używa token dostępu podczas uzyskiwania dostępu do magazynu Lake danych Azure i token odświeżania uzyskać inny token dostępu po wygaśnięciu token dostępu.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Po wygaśnięciu token dostępu, możesz przejąć tokenu dostępu przy użyciu tokenu odświeżania, tak jak pokazano poniżej:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Aby uzyskać więcej informacji o uwierzytelnianiu użytkowników interakcyjnych zobacz [Kod autoryzacji przyznać przepływu](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Aby usługa uwierzytelniania (non-interactive)

W tym scenariuszu własnej poświadczenia, aby wykonać operacje dostępne w aplikacji. W tym należy wygenerować żądanie POST, tak jak pokazano poniżej. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Wynik to żądanie będzie zawierać tokenu autoryzacji (oznaczona `access-token` w wyniku poniżej), która następnie przekaże z połączenia interfejsu API usługi REST. Zapisz ten token uwierzytelniania w pliku tekstowym; konieczne będzie to w dalszej części tego artykułu.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

W tym artykule wykorzystano **- interactive** podejście. Aby uzyskać więcej informacji na-interactive (połączeń do usługi) zobacz [Usługa do obsługi technicznej przy użyciu poświadczeń](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Tworzenie konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST połączenia zdefiniowane [w tym miejscu](https://msdn.microsoft.com/library/mt694078.aspx).

Użyj następującego polecenia zwinięcie. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

W powyższe polecenie Zamień \< `REDACTED` \> z token autoryzacji pobraniu wcześniej. Ładunku żądania dla tego polecenia znajduje się w pliku **input.json** , który jest przewidziane `-d` parametru powyżej. Zawartość pliku input.json wyglądać w następujący sposób:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Tworzenie folderów konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST WebHDFS połączenia zdefiniowane [w tym miejscu](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Użyj następującego polecenia zwinięcie. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

W powyższe polecenie Zamień \< `REDACTED` \> z token autoryzacji pobraniu wcześniej. To polecenie tworzy katalog o nazwie **mytempdir** w folderze głównym folderu konta magazynu Lake danych.

Odpowiedź tak należy sprawdzić, czy operacja zakończy się pomyślnie:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Lista folderów z konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST WebHDFS połączenia zdefiniowane [w tym miejscu](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Użyj następującego polecenia zwinięcie. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

W podanych polecenie Zamień \< `REDACTED` \> z token autoryzacji pobraniu wcześniej.

Odpowiedź tak należy sprawdzić, czy operacja zakończy się pomyślnie:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Przekazywanie danych do konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST WebHDFS połączenia zdefiniowane [w tym miejscu](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Przekazywanie danych przy użyciu interfejsu API usługi REST WebHDFS jest procesem dwuetapowym, zgodnie z poniższym opisem.

1. Przesyłanie żądania HTTP umieszczenie bez wysyłania danych pliku do przekazania. W następujące polecenie Zamień ** \<yourstorename >** z nazwą magazynu Lake danych.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Dane wyjściowe tego polecenia będzie można zawierać Przekierowanie tymczasowe adresu URL, takich jak przedstawionej poniżej.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Teraz należy przesłać kolejnego żądania HTTP umieszczenie dla adresu URL, na liście właściwości **lokalizacji** w odpowiedzi. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Wynik będzie podobny do następującego:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Odczyt danych z konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST WebHDFS połączenia zdefiniowane [w tym miejscu](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Odczytywanie danych z magazynu Lake danych konto jest procesem dwuetapowym.

* Najpierw przesyłanie żądania GET przed punkt końcowy `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Spowoduje to przywrócenie przesłanie dalej żądania GET do lokalizacji.
* Następnie przesłaniu żądania GET przed punkt końcowy `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Spowoduje to wyświetlenie zawartość pliku.

Jednak, ponieważ istnieje różnica w parametrów wejściowych między pierwszym i drugim krokiem, możesz za pomocą `-L` parametr w celu przesyłania pierwszego żądania. `-L`Opcja zasadniczo łączy dwa żądania w jedną i spowoduje zwinięcie ponowne wezwanie na nowe miejsce. Na koniec dane wyjściowe wszystkich połączeń wezwanie jest wyświetlane, takich jak pokazano poniżej. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Powinien zostać wyświetlony wynik podobny do następującego:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Zmienianie nazwy pliku z konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST WebHDFS połączenia zdefiniowane [w tym miejscu](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Aby zmienić nazwę pliku, użyj następującego polecenia zwinięcie. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Powinien zostać wyświetlony wynik podobny do następującego:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Usuwanie pliku z konta magazynu Lake danych

Operacja zależy interfejsu API usługi REST WebHDFS połączenia zdefiniowane [w tym miejscu](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Aby usunąć plik, użyj następującego polecenia zwinięcie. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Powinny zostać wyświetlone wyniki podobne do następujących:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Usuwanie konta magazynu Lake danych

Operacja jest oparty na interfejsu API usługi REST połączenia zdefiniowane [w tym miejscu](https://msdn.microsoft.com/library/mt694075.aspx).

Użyj następującego polecenia zwinięcie, aby usunąć konto magazynu Lake danych. Zamienianie ** \<yourstorename >** z nazwą magazynu Lake danych.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Powinny zostać wyświetlone wyniki podobne do następujących:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Zobacz też

- [Otwieranie aplikacji duży danych źródłowych zgodnych z magazynu Lake danych Azure](data-lake-store-compatible-oss-other-applications.md)
 
