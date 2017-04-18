<properties
    pageTitle="Azure Auth usług AD przy użyciu OAuth2.0 | Microsoft Azure"
    description="W tym artykule opisano, jak za pomocą wiadomości HTTP zaimplementować uwierzytelnianie usług za pomocą przepływu udzielanie poświadczeń klienta OAuth2.0."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="service-to-service-calls-using-client-credentials"></a>Usługa obsługi technicznej przy użyciu poświadczeń klienta

OAuth 2.0 klienta poświadczeń udzielanie przepływ pozwala usługi sieci web ( *poufne klienta*) za pomocą własnej poświadczeń uwierzytelniania podczas nawiązywania połączeń z innej usługi sieci web, zamiast personifikacji użytkownika. W tym scenariuszu klient jest zazwyczaj usługi sieci web warstwy środkowej, demon usługi lub witryny sieci web.

## <a name="client-credentials-grant-flow-diagram"></a>Diagram przepływu udzielić poświadczeń klienta

Poniższy diagram wyjaśniono, jak poświadczenia klienta przyznania przepływu działa w usłudze Azure Active Directory (Azure AD).

![Klient OAuth2.0 poświadczenia przepływ przyznania](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Z aplikacją kliencką uwierzytelnia do punktu końcowego wydawania token Azure AD i żądania token dostępu.
2. Punktu końcowego wydawania token Azure AD problemy token dostępu.
3. Token dostępu jest używany do uwierzytelniania zabezpieczonego zasobów.
4. Dane z zabezpieczonego zasobu są zwracane do aplikacji sieci web.

## <a name="register-the-services-in-azure-ad"></a>Rejestruj usług Azure AD

Zarejestruj zarówno połączeń usługi i odbierania w usłudze Azure Active Directory (Azure AD). Aby uzyskać szczegółowe instrukcje zobacz [Dodawanie, aktualizowanie i usuwanie aplikacji](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Żądanie Token dostępu

Aby zażądać token dostępu, za pomocą HTTP POST do dzierżawy określonego punktu końcowego Azure AD.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Żądania token dostępu do usługi

Dostęp do usługi żądania tokenu zawiera następujące informacje.

| Parametr | | Opis |
|-----------|------|------------|
| response_type | Wymagane | Umożliwia określenie typu żądanej odpowiedzi. W przepływie udzielanie poświadczeń klienta wartość musi być **client_credentials**.|
| client_id | Wymagane | Określa identyfikator klienta Azure AD połączeń usługi sieci web. Aby znaleźć identyfikator klienta aplikacji wywołującej, w portalu zarządzania Azure, kliknij **Usługi Active Directory**, kliknij katalog, kliknij aplikację, a następnie kliknij przycisk **Konfiguruj**.|
| client_secret | Wymagane |  Wprowadź klucz zarejestrowane połączeń usługi sieci web w Azure AD. Aby utworzyć klawisza, w portalu zarządzania Azure, kliknij pozycję **Usługi Active Directory**, kliknij katalog, kliknij aplikację, a następnie kliknij **Konfiguruj**. |
| zasób | Wymagane | Wprowadź identyfikator URI identyfikator aplikacji odbierania usługi sieci web. Znajdowanie identyfikatora URI identyfikator aplikacji, w portalu zarządzania Azure, kliknij **Usługi Active Directory**, kliknij katalog, kliknij aplikację, a następnie kliknij przycisk **Konfiguruj**. |

## <a name="example"></a>Przykład

Następujące HTTP POST żąda token dostępu usługi sieci web https://service.contoso.com/. `client_id` Identyfikuje żądające token dostępu usługi sieci web.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Odpowiedź Token dostępu do usługi

Odpowiedź sukcesu zawiera odpowiedź JSON OAuth 2.0 z poniższych parametrów.

| Parametr   | Opis |
|-------------|-------------|
|access_token |Token dostępu wymagane. Połączeń usługi sieci web umożliwia ten token uwierzytelniania odbierania usługi sieci web. |
|access_type  | Wskazuje typ tokenu wartość. Tylko typ, która obsługuje Azure AD jest **okaziciela**. Aby uzyskać więcej informacji na temat tokeny okaziciela zobacz [Framework autoryzacji 2.0 OAuth: zastosowania Token okaziciela (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Jak długo trwa token dostępu jest prawidłowy (w sekundach).|
|expires_on   |Czas wygaśnięcia token dostępu. Data jest reprezentowana jako liczbę sekund od 1970-01-01T0:0:0Z UTC czasu wygaśnięcia. Ta wartość jest używana do określenia czasu trwania pamięci podręcznej tokenów. |
|zasób     | Identyfikator URI identyfikator aplikacji odbierania usługi sieci web. |

## <a name="example"></a>Przykład

W poniższym przykładzie pokazano sukcesu odpowiedź na żądanie token dostępu do usługi sieci web.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Zobacz też

* [OAuth 2.0 w Azure AD](active-directory-protocols-oauth-code.md)
