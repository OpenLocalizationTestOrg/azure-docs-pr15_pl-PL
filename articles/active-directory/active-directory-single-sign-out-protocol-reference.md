<properties
    pageTitle="Azure pojedynczy Wyloguj protokołu SAML | Microsoft Azure"
    description="W tym artykule opisano pojedynczy protokół SAML Sign-Out w usłudze Active Directory platformy Azure"
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


# <a name="single-sign-out-saml-protocol"></a>Pojedynczy SAML wyrejestrowywania Protocol (protokół)

Azure Active Directory (Azure AD) obsługuje SAML 2.0 web jednego profilu wyrejestrowywania przeglądarki. Dla pojedynczego wyrejestrowywania działają prawidłowo, Azure AD musisz zarejestrować jego adres URL metadanych podczas rejestrowania aplikacji. Azure AD otrzymuje adres URL Wyloguj i klucz podpisywania usługi w chmurze z metadanych. Azure AD przy użyciu klucza podpisywania weryfikowanie podpisu LogoutRequest przychodzących i przekierować użytkowników, gdy są one wylogowano się za pomocą LogoutURL.

Jeśli po zarejestrowaniu aplikacji usług w chmurze nie obsługuje punkt końcowy metadanych, deweloper musi kontakt z pomocą techniczną firmy Microsoft o podanie adresu URL Wyloguj i klucza podpisywania.

Ten diagram przedstawia przepływ pracy procesu Azure AD pojedynczy wyrejestrowywania.

![Pojedynczy Wyloguj przepływu pracy](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Wysyła usługa w chmurze `LogoutRequest` wiadomość do Azure AD oznaczającą zakończenia sesji. Poniższy fragment prezentuje próbki `LogoutRequest` elementu.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

`LogoutRequest` Elementu wysyłane do Azure AD wymaga następujące atrybuty:

- `ID`: Identyfikuje wyrejestrowywania wezwanie. Wartość `ID` nie powinna rozpoczynać się od liczby. Typowe sesji ćwiczeń jest dołączyć **id** Aby reprezentacja tekstowa identyfikator GUID.

- `Version`: Ustaw wartość tego elementu do **2.0**. Ta wartość jest wymagana.

- `IssueInstant`: `DateTime` Ciąg zawierający wartość koordynowanie Universal Time (UTC) i [Formatowanie przebiegu ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD oczekuje wartości tego typu, ale nie jest wymusza.

- `Consent`, `Destination`, `NotOnOrAfter` i `Reason` atrybuty są ignorowane, jeśli znajdują się w `LogoutRequest` elementu.

### <a name="issuer"></a>Wystawcy

`Issuer` Element `LogoutRequest` muszą dokładnie odpowiadać jedną **ServicePrincipalNames** w usłudze w chmurze w Azure AD. Zazwyczaj ustawienie **Aplikacji identyfikator URI** , który jest określony podczas rejestrowania aplikacji.

### <a name="nameid"></a>NameID

Wartość `NameID` elementu musi dokładnie odpowiadać `NameID` użytkownika, który jest Trwa wyrejestrowywanie.
## <a name="logoutresponse"></a>LogoutResponse

Wysyła Azure AD `LogoutResponse` w odpowiedzi na `LogoutRequest` elementu. Poniższy fragment prezentuje próbki `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Zestawy Azure AD `ID`, `Version` i `IssueInstant` wartości w `LogoutResponse` elementu. Określa też `InResponseTo` elementu na wartość `ID` atrybut `LogoutRequest` który tych odpowiedzi.

### <a name="issuer"></a>Wystawcy

Azure AD ustawia tę wartość na `https://login.microsoftonline.com/<TenantIdGUID>/` miejsce, w którym <TenantIdGUID> jest identyfikator dzierżawy dzierżawy Azure AD.

Ma być obliczona wartość `Issuer` element, użyj wartości **Aplikacji identyfikator URI** podany podczas rejestrowania aplikacji.

### <a name="status"></a>Stan

Azure AD przy użyciu `StatusCode` element `Status` element, aby wskazać powodzenie lub niepowodzenie wyrejestrowywania. Podczas próby wyrejestrowywania nie powiedzie się, `StatusCode` element może również zawierać niestandardowe komunikaty o błędach.
