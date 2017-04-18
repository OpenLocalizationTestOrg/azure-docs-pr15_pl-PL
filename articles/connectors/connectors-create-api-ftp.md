<properties
pageTitle="Dowiedz się, jak używać łącznika FTP w aplikacjach logiki | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Podłącz do serwera FTP do zarządzania plikami. Można wykonywać różne akcje, takie jak przekazywania, aktualizowanie, pobieranie i usuwanie plików w serwera FTP."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/22/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-ftp-connector"></a>Wprowadzenie do łącznika FTP

Łącznik FTP umożliwia monitorowanie, zarządzanie i tworzenie plików na serwerze FTP. 

Aby użyć [dowolny łącznik](./apis-list.md), najpierw należy utworzyć aplikację logicznych. Można rozpocząć, [tworząc aplikację logiczny, który jest teraz](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-ftp"></a>Nawiązywanie połączenia z FTP

Przed aplikacji logiki uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenie* z usługą. [Połączenia](./connectors-overview.md) umożliwia łączność aplikacji logiki i innej usługi.  

### <a name="create-a-connection-to-ftp"></a>Tworzenie połączenia z FTP

>[AZURE.INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]

## <a name="use-a-ftp-trigger"></a>Użyj wyzwalacza FTP

Wyzwalacz to zdarzenie, które może służyć do uruchamiania przepływu pracy, zdefiniowane w aplikacji logicznych. [Dowiedz się więcej o wyzwalaczy](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.IMPORTANT]Łącznik FTP wymaga serwer FTP, który jest dostępny z Internetu i jest skonfigurowany do pracy w trybie PASYWNE. Również łącznik FTP jest **niezgodny z niejawne FTPS (FTP przez SSL)**. Łącznik FTP obsługuje tylko jawne FTPS (FTP przez SSL).  

W tym przykładzie I procedurach pokazano, jak za pomocą wyzwalacza **FTP - podczas dodawania lub modyfikowania pliku** zainicjować przepływ pracy logiki aplikacji, po dodaniu pliku lub modyfikacji na serwerze FTP. Na przykład przedsiębiorstwa można użyć tego wyzwalacza monitorowanie folder FTP dla nowych plików, które reprezentują zamówienia od klientów.  Aby pobrać zawartość kolejność dalszego przetwarzania i magazynowania w bazie danych zamówień można używać akcji łącznik FTP takich jak **Pobieranie zawartości pliku** .

1. W polu wyszukiwania wprowadź *ftp* w Projektancie aplikacji logiczny, a następnie wybierz wyzwalacz **FTP - podczas dodawania lub modyfikowania pliku**   
![Obraz wyzwalacza FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
Otwiera kontroli **podczas dodawania lub modyfikowania pliku**  
![Obraz wyzwalacza FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
- Wybierz pozycję **...** znajdującej się po prawej stronie formantu. Spowoduje to otwarcie folderu selektora formantu  
![Obraz wyzwalacza FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
- Wybierz pozycję **>** (Strzałka w prawo) i Przeglądaj, aby znaleźć folder, w którym chcesz monitorować nowych lub zmienionych plików. Wybierz folder i zwróć uwagę, że folder zostanie wyświetlona w formancie **folderu** .  
![Obraz wyzwalacza FTP 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   


W tym momencie aplikacji logiczny został skonfigurowany z wyzwalacz, który rozpocznie Uruchom wyzwalaczy i akcje w ramach przepływu pracy po modyfikacji lub utworzony w określonym folderze FTP pliku. 

>[AZURE.NOTE]Dla aplikacji logiki do działać musi zawierać co najmniej jeden wyzwalacz i akcji. Wykonaj kroki opisane w następnej sekcji, aby dodać akcję.  



## <a name="use-a-ftp-action"></a>Za pomocą akcji FTP

Akcja jest czynność wykonaną przez przepływ pracy zdefiniowane w aplikacji logicznych. [Dowiedz się więcej o akcje](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

Teraz, gdy dodano wyzwalacz, wykonaj poniższe czynności, aby dodać akcję, która będzie mieć zawartość pliku nowe lub zmodyfikowane, znaleziony przez wyzwalacz.    

1. Wybierz pozycję **+ Nowy krok** Dodawanie akcji, aby uzyskać zawartość pliku na serwerze FTP  
- Wybierz łącze **Dodaj akcję** .  
![Obraz akcji FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
- Wprowadź *FTP* , aby wyszukać wszystkie operacje dotyczące FTP.
- Wybierz akcję do wykonania po nowej lub zmodyfikowanej plik znajduje się w folderze FTP **FTP — Pobierz zawartość pliku** .      
![Obraz akcji FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
Zostanie wyświetlona kontrolkę **Pobieranie zawartości pliku** . **Uwaga**: pojawi się monit Aby autoryzować aplikacji logiki do uzyskiwania dostępu do konta serwera FTP, jeśli jeszcze nie tego wcześniej.  
![Obraz akcji FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
- Zaznacz kontrolkę **pliku** (odstęp znajdujących się poniżej **plik***). Tutaj możesz użyć dowolnej z różne właściwości z pliku nowej lub zmodyfikowanej znajdujące się na serwerze FTP.  
- Wybierz opcję **zawartość pliku** .  
![Obraz akcji FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
-  Kontrolka zostanie zaktualizowany, wskazująca, że akcja **FTP — Pobierz zawartość pliku** otrzymają *pliku zawartości* pliku nowe lub zmienione na serwerze FTP.      
![Obraz akcji FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
- Zapisz swoją pracę, a następnie dodaj pliku do folderu FTP testowania przepływu pracy.    

W tym momencie aplikacji logiczny został skonfigurowany z wyzwalacza folderu na serwerze FTP oraz inicjowania przepływu pracy, gdy znajdzie nowego pliku lub zmodyfikowanego pliku na serwerze FTP. 

Aplikacja logiczny również skonfigurowano z akcją Pobierz zawartość pliku nowe lub zmodyfikowane.

Można teraz dodawać innej akcji, takich jak akcji [Programu SQL Server — wstawić wiersz](./connectors-create-api-sqlazure.md#insert-row) , aby wstawić zawartość pliku nowej lub zmodyfikowanej w tabeli bazy danych SQL.  

## <a name="technical-details"></a>Szczegóły techniczne

Poniżej przedstawiono szczegółowe informacje dotyczące wyzwalacze, akcje i odpowiedzi, które obsługuje to połączenie:

## <a name="ftp-triggers"></a>Wyzwalacze FTP

FTP występują następujące trigger(s):  

|Wyzwalacza | Opis|
|--- | ---|
|[Podczas dodawania lub modyfikowania pliku](connectors-create-api-ftp.md#when-a-file-is-added-or-modified)|Operacja uaktywnia przepływu pliku zostanie dodany bądź zmodyfikowane w folderze.|


## <a name="ftp-actions"></a>Akcje FTP

FTP występują następujące działania:


|Akcja|Opis|
|--- | ---|
|[Pobierz plik metadanych](connectors-create-api-ftp.md#get-file-metadata)|Operacja otrzymuje metadanych dla pliku.|
|[Aktualizowanie plików](connectors-create-api-ftp.md#update-file)|Operacja aktualizacji pliku.|
|[Usuwanie pliku](connectors-create-api-ftp.md#delete-file)|Operacja usuwa plik.|
|[Pobieranie metadanych pliku przy użyciu ścieżki](connectors-create-api-ftp.md#get-file-metadata-using-path)|Operacja otrzymuje metadanych przy użyciu ścieżki pliku.|
|[Pobieranie zawartości pliku przy użyciu ścieżki](connectors-create-api-ftp.md#get-file-content-using-path)|Operacja otrzymuje zawartości przy użyciu ścieżki pliku.|
|[Pobieranie zawartości pliku](connectors-create-api-ftp.md#get-file-content)|Operacja pobiera zawartość pliku.|
|[Tworzenie pliku](connectors-create-api-ftp.md#create-file)|Operacja tworzy plik.|
|[Skopiuj plik](connectors-create-api-ftp.md#copy-file)|Operacja kopiuje plik na serwerze FTP.|
|[Lista plików w folderze](connectors-create-api-ftp.md#list-files-in-folder)|Operacja otrzymuje listę plików i podfolderów w folderze.|
|[Lista plików w folderze głównym](connectors-create-api-ftp.md#list-files-in-root-folder)|Operacja otrzymuje listę plików i podfolderów w folderze głównym.|
|[Wyodrębnianie folderu](connectors-create-api-ftp.md#extract-folder)|Operacja wyodrębnia plik archiwum do folderu (przykład: zip).|
### <a name="action-details"></a>Szczegóły akcji

Oto szczegóły akcji i wyzwalaczy dla tego łącznika, wraz z ich odpowiedziami:



### <a name="get-file-metadata"></a>Pobierz plik metadanych
Operacja pobiera metadane dla pliku. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator *|Plik|Wybierz plik|

* Wskazuje, że właściwość jest wymagane

#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="update-file"></a>Aktualizowanie plików
Operacja aktualizacji pliku. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator *|Plik|Wybierz plik|
|Treść *|Zawartość pliku|Zawartość pliku|

* Wskazuje, że właściwość jest wymagane

#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="delete-file"></a>Usuwanie pliku
Operacja usuwa plik. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator *|Plik|Wybierz plik|

* Wskazuje, że właściwość jest wymagane




### <a name="get-file-metadata-using-path"></a>Pobieranie metadanych pliku przy użyciu ścieżki
Operacja otrzymuje metadanych przy użyciu ścieżki pliku. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Ścieżka *|Ścieżka pliku|Wybierz plik|

* Wskazuje, że właściwość jest wymagane

#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="get-file-content-using-path"></a>Pobieranie zawartości pliku przy użyciu ścieżki
Operacja otrzymuje zawartości przy użyciu ścieżki pliku. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Ścieżka *|Ścieżka pliku|Wybierz plik|

* Wskazuje, że właściwość jest wymagane




### <a name="get-file-content"></a>Pobieranie zawartości pliku
Operacja pobiera zawartość pliku. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator *|Plik|Wybierz plik|

* Wskazuje, że właściwość jest wymagane




### <a name="create-file"></a>Tworzenie pliku
Operacja tworzy plik. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|ścieżkafolderu *|Ścieżka folderu|Wybierz folder|
|Nazwa *|Nazwa pliku|Nazwa pliku|
|Treść *|Zawartość pliku|Zawartość pliku|

* Wskazuje, że właściwość jest wymagane

#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="copy-file"></a>Skopiuj plik
Operacja kopiuje plik na serwerze FTP. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|źródło *|Adres url źródła|Adres URL z plikiem źródłowym|
|miejsce docelowe *|Ścieżka pliku docelowego|Ścieżka pliku docelowego, w tym nazwa pliku docelowego|
|Zastąp|Czy zastąpić?|Zastępuje pliku docelowego, jeśli ma wartość "true"|

* Wskazuje, że właściwość jest wymagane

#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="when-a-file-is-added-or-modified"></a>Podczas dodawania lub modyfikowania pliku
Operacja uaktywnia przepływu pliku zostanie dodany bądź zmodyfikowane w folderze. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator folderu *|Folder|Wybierz folder|

* Wskazuje, że właściwość jest wymagane




### <a name="list-files-in-folder"></a>Lista plików w folderze
Operacja otrzymuje listę plików i podfolderów w folderze. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator *|Folder|Wybierz folder|

* Wskazuje, że właściwość jest wymagane



#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="list-files-in-root-folder"></a>Lista plików w folderze głównym
Operacja otrzymuje listę plików i podfolderów w folderze głównym. 


Nie ma żadnych parametrów dla tego połączenia

#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|




### <a name="extract-folder"></a>Wyodrębnianie folderu
Operacja wyodrębnia plik archiwum do folderu (przykład: zip). 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|źródło *|Ścieżka pliku archiwum źródła|Ścieżka do pliku archiwum|
|miejsce docelowe *|Ścieżka folderu docelowego|Ścieżka do folderu docelowego|
|Zastąp|Czy zastąpić?|Zastępuje plików docelowych, jeśli ma wartość "true"|

* Wskazuje, że właściwość jest wymagane



#### <a name="output-details"></a>Szczegóły wyników

BlobMetadata


| Nazwa właściwości | Typ danych |
|---|---|---|
|Identyfikator|ciąg|
|Nazwa|ciąg|
|DisplayName|ciąg|
|Ścieżka|ciąg|
|Ostatnia modyfikacja|ciąg|
|Rozmiar|Liczba całkowita|
|Typ nośnika|ciąg|
|IsFolder|wartość logiczna|
|ETag|ciąg|
|FileLocator|ciąg|



## <a name="http-responses"></a>Odpowiedzi HTTP

Akcje i wyzwalaczy powyżej może zwracać jedną lub więcej z poniższych kodów stanu HTTP: 

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd.|
|domyślne|Operacja nie powiodła się.|







## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)