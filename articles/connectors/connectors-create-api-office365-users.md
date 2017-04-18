<properties
    pageTitle="Dodawanie łącznika użytkownicy usługi Office 365 w aplikacjach logiczny | Microsoft Azure"
    description="Omówienie łącznik użytkownicy usługi Office 365 z parametrami interfejsu API usługi REST"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Wprowadzenie do łącznika użytkownicy usługi Office 365

Połącz, aby użytkownicy usługi Office 365 uzyskanie profile, Wyszukaj użytkowników i innych elementów. 

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych.

Z użytkownikiem usługi Office 365 możesz wykonać następujące czynności:

- Tworzenie usługi przepływu biznesowych na podstawie danych, które otrzymujesz z użytkownikiem usługi Office 365. 
- Używanie akcji, które uzyskać bezpośredni podwładni, Uzyskaj przez menedżera profilu użytkownika i nie tylko. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład pobieranie osoby bezpośredni podwładni, a następnie wykonać te informacje i aktualizowanie bazy danych platformy SQL Azure. 

Aby dodać operację w aplikacjach logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik użytkownicy usługi Office 365 ma następujące akcje dostępne. Istnieją wyzwalacze nie.

| Wyzwalaczy | Akcje|
| --- | --- |
|Brak | <ul><li>Uzyskiwanie manager</li><li>Pobierz mój profil</li><li>Uzyskiwanie bezpośredni podwładni</li><li>Uzyskiwanie profilu użytkownika</li><li>Wyszukiwanie użytkowników</li></ul>|

Wszystkie łączniki obsługuje danych w formatach XML i JSON. 


## <a name="create-a-connection-to-office-365-users"></a>Tworzenie połączenia z użytkownikiem usługi Office 365

Po dodaniu tego łącznika do aplikacji logika należy Zaloguj się do konta usługi Office 365 dla użytkowników i zezwolić aplikacji logicznych w celu nawiązywania połączenia z kontem.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Po utworzeniu połączenia wprowadź właściwości użytkownicy usługi Office 365, takie jak identyfikator użytkownika. **Odwołanie interfejsu API usługi REST** w tym temacie opisano następujące właściwości.

>[AZURE.TIP] Za pomocą tego samego połączenia użytkownicy usługi Office 365 w innych aplikacjach logicznych.


## <a name="office-365-users-rest-api-reference"></a>Odwołanie interfejsu API usługi REST użytkowników usługi Office 365
Dotyczy wersji: 1.0.

### <a name="get-my-profile"></a>Pobierz mój profil 
Pobiera profilu dla bieżącego użytkownika.  
```GET: /users/me``` 

Nie ma żadnych parametrów dla tego połączenia.

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="get-user-profile"></a>Uzyskiwanie profilu użytkownika 
Pobiera profilu określonego użytkownika.  
```GET: /users/{userId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Nazwa użytkownika|ciąg|tak|Ścieżka|Brak|Główne nazwiska lub adresu e-mail identyfikator użytkownika|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="get-manager"></a>Uzyskiwanie manager 
Pobiera profilu użytkownika dla Menedżera określonego użytkownika.  
```GET: /users/{userId}/manager``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Nazwa użytkownika|ciąg|tak|Ścieżka|Brak|Główne nazwiska lub adresu e-mail identyfikator użytkownika|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|



### <a name="get-direct-reports"></a>Uzyskiwanie bezpośredni podwładni 
Uzyskaj bezpośredni podwładni.  
```GET: /users/{userId}/directReports``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Nazwa użytkownika|ciąg|tak|Ścieżka|Brak|Główne nazwiska lub adresu e-mail identyfikator użytkownika|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|



### <a name="search-for-users"></a>Wyszukiwanie użytkowników 
Pobiera wyszukiwania wyniki profile użytkowników.  
```GET: /users``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|searchTerm|ciąg|Brak|kwerendy|Brak|Ciąg wyszukiwania (dotyczy: wyświetlanie imię, imię, nazwisko, poczty, poczta pseudonimu i główna nazwa użytkownika)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|



## <a name="object-definitions"></a>Definicje obiektów

#### <a name="user-user-model-class"></a>Użytkownika: Klasy modelu użytkownika

|Nazwa właściwości | Typ danych |Wymagane
|---|---|---|
|DisplayName|ciąg|Brak|
|Imię|ciąg|Brak|
|Nazwisko|ciąg|Brak|
|Poczta|ciąg|Brak|
|MailNickname|ciąg|Brak|
|TelephoneNumber|ciąg|Brak|
|AccountEnabled|wartość logiczna|Brak|
|Identyfikator|ciąg|tak
|UserPrincipalName|ciąg|Brak|
|Dział|ciąg|Brak|
|Stanowisko|ciąg|Brak|
|mobilePhone|ciąg|Brak|


## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

Wróć do [listy interfejsów API](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
