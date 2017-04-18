<properties
    pageTitle="Azure rejestracji jednokrotnej na protokole SAML | Microsoft Azure"
    description="W tym artykule opisano protokół pojedynczy znak na SAML w usłudze Active Directory platformy Azure"
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

# <a name="single-sign-on-saml-protocol"></a>Pojedynczy protokół SAML logowania jednokrotnego

W tym artykule opisano, jak SAML 2.0 uwierzytelniania zaproszenia i odpowiedzi, obsługiwanych przez usługi Azure Active Directory (Azure AD) dla rejestracji jednokrotnej.

Na poniższym diagramie Protocol (protokół) w tym artykule opisano jednej sekwencji logowania jednokrotnego. Usługa w chmurze (usługodawca) używa powiązanie przekierowania HTTP do przekazania `AuthnRequest` element (uwierzytelniania żądania), aby Azure AD (dostawcy tożsamości). Następnie Azure AD używa ogłoszenia HTTP wiązanie ogłaszać `Response` element, aby usługa w chmurze.

![Rejestracji jednokrotnej w przepływie pracy](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Aby zażądać uwierzytelnianie użytkownika, cloud services Wyślij `AuthnRequest` się Azure AD element. Przykładowe SAML 2.0 `AuthnRequest` może wyglądać tak:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| IDENTYFIKATOR | Wymagane | Azure AD używa ten atrybut do zapełnienia `InResponseTo` atrybut zwracane odpowiedzi. Identyfikator nie muszą zaczynać się od liczby dzięki wspólnej strategii jest dołączana ciąg like "identyfikator", aby Reprezentacja tekstowa identyfikator GUID. Na przykład `id6c1c178c166d486687be4aaf5e482730` jest prawidłowy identyfikator. |
| Wersja | Wymagane | Należy to **2.0**.|
| IssueInstant | Wymagane | Jest to ciąg daty i godziny przy użyciu wartość w czasie UTC i [przebiegu format ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD oczekuje wartości daty/godziny tego typu, ale nie są interpretowane lub użyj wartości z pola. |
| AssertionConsumerServiceUrl | opcjonalne | Jeśli podano musi być zgodna `RedirectUri` chmury usługi Azure AD. |
| ForceAuthn | opcjonalne | Jeśli podano należy to FAŁSZ. Inną wartość powoduje błąd.|
| IsPassive | opcjonalne | Jeśli podano należy to FAŁSZ. Inną wartość powoduje błąd. |  

Wszystkie inne `AuthnRequest` atrybutów, takich jak zgody, miejsce docelowe, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex i ProviderName są **ignorowane**.

Ignoruje także Azure AD `Conditions` element w `AuthnRequest`.

### <a name="issuer"></a>Wystawcy

`Issuer` Element `AuthnRequest` muszą dokładnie odpowiadać jedną **ServicePrincipalNames** w usłudze w chmurze w Azure AD. Zazwyczaj ustawienie **Aplikacji identyfikator URI** , który jest określony podczas rejestrowania aplikacji.

Zawierającej fragment SAML próbki `Issuer` element wygląda następująco:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Ten element formatu identyfikator nazw określonego w odpowiedzi na wezwania i jest opcjonalne w `AuthnRequest` elementy wysłane do Azure AD.

Próbki `NameIdPolicy` element wygląda następująco:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Jeśli `NameIDPolicy` jest podana, możesz umieścić jej opcjonalne `Format` atrybut. `Format` Atrybut może mieć tylko jedną z następujących wartości; inną wartość powoduje błąd.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory problemy roszczeń NameID jako identyfikator parowania.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory problemy roszczeń NameID w formacie adresu e-mail.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Wartość ta pozwala usługi Azure Active Directory, aby wybrać format roszczeń. Azure Active Directory problemów NameID jako identyfikator parowania.

Nie dołączaj `SPNameQualifer` atrybut. Ignoruje Azure AD `AllowCreate` atrybut.

### <a name="requestauthncontext"></a>RequestAuthnContext

`RequestedAuthnContext` Element określa metodę uwierzytelniania odpowiednią. Opcjonalne w `AuthnRequest` elementy wysłane do Azure AD. Azure AD obsługuje tylko jeden `AuthnContextClassRef` wartość: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Określanie zakresu

`Scoping` Element, który zawiera listę dostawców tożsamości, jest opcjonalne w `AuthnRequest` elementy wysłane do Azure AD.

Jeśli podano nie powinno obejmować `ProxyCount` atrybut, `IDPListOption` lub `RequesterID` element jako nie są obsługiwane.

### <a name="signature"></a>Podpis

Nie dołączaj `Signature` element `AuthnRequest` elementów, nie obsługuje Azure AD podpisane żądania uwierzytelniania.

### <a name="subject"></a>Temat

Ignoruje Azure AD `Subject` element `AuthnRequest` elementy.

## <a name="response"></a>Odpowiedź

Gdy wymagane jednokrotnej zakończy się pomyślnie, Azure AD wpisy odpowiedź dotycząca usługi w chmurze. Przykładowe odpowiedź na pomyślne próbę logowania jednokrotnego wygląda następująco:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Odpowiedź

`Response` Element zawiera wynik żądanie autoryzacji. Zestawy Azure AD `ID`, `Version` i `IssueInstant` wartości w `Response` elementu. Określa też następujące atrybuty:

- `Destination`: Podczas logowania jednokrotnego zakończy się pomyślnie, to jest ustawiona na `RedirectUri` dostawcy usług (usługa w chmurze).
- `InResponseTo`: To jest ustawiona na `ID` atrybut `AuthnRequest` element, który zainicjowane odpowiedź.

### <a name="issuer"></a>Wystawcy

Zestawy Azure AD `Issuer` element, aby `https://login.microsoftonline.com/<TenantIDGUID>/` miejsce, w którym <TenantIDGUID> jest identyfikator dzierżawy dzierżawy Azure AD.

Na przykład odpowiedź próbki z elementem wystawcy może wyglądać tak:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Stan

`Status` Element obejmuje sukcesu lub niepowodzenia logowania jednokrotnego. Zawiera `StatusCode` element, który zawiera kod lub zestawu kodów zagnieżdżonych reprezentujących stanu żądania. Zawiera również `StatusMessage` element, który zawiera niestandardowe komunikaty o błędach generowanych podczas procesu rejestracji.

<!-- TODO: Add a authentication protocol error reference -->

Poniżej przedstawiono odpowiedzi SAML nieudanej próbie logowania jednokrotnego.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Potwierdzenia

Oprócz `ID`, `IssueInstant` i `Version`, Azure AD zestawów następujące elementy w `Assertion` element odpowiedzi.

#### <a name="issuer"></a>Wystawcy

To jest ustawiona na `https://sts.windows.net/<TenantIDGUID>/`miejsce, w którym <TenantIDGUID> jest identyfikator dzierżawy dzierżawy Azure AD.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Podpis

Azure AD podpisuje potwierdzenia w odpowiedzi na pomyślne logowania jednokrotnego. `Signature` Element zawiera podpis cyfrowy, który usługa w chmurze służy do uwierzytelniania źródło, aby zweryfikować integralności potwierdzenia.

Aby wygenerować podpis cyfrowy, Azure AD używa klucz podpisywania w `IDPSSODescriptor` elementu jego dokument metadanych.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Temat

To ustawienie określa podmiotu, który jest temat oświadczeń do potwierdzenia. Zawiera `NameID` element, który reprezentuje uwierzytelnionego użytkownika. `NameID` Wartość jest identyfikatorem docelowej, który jest przekierowywany tylko dostawcę usługi odbiorców dla tokenu. Jest trwałych — można odwołany, ale nie jest przypisywana ponownie. Należy również nieprzezroczysty, ponieważ nie wyświetla jakiekolwiek założenia dotyczące użytkownika i nie można użyć jako identyfikator atrybut zapytania.

`Method` Atrybut `SubjectConfirmation` element zawsze jest ustawiona na `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Warunki

Ten element określa warunki, które Definiowanie dopuszczalnego wykorzystania potwierdzeń SAML.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` i `NotOnOrAfter` atrybuty określić przedział, w którym jest prawidłowy potwierdzenia.

- Wartość `NotBefore` atrybut jest równoważna lub nieco (mniej niż sekunda) później niż wartość `IssueInstant` atrybut `Assertion` elementu. Azure AD nie uwzględnia o różnicę czasu między sobą i usług w chmurze (usługodawca), a nie dodawać dowolną buforu do tej chwili.
- Wartość `NotOnOrAfter` atrybut jest 70 minut później niż wartość `NotBefore` atrybut.

#### <a name="audience"></a>Grupy odbiorców

Ta strona zawiera identyfikator URI, który identyfikuje określonej grupy odbiorców. Azure AD ustawia wartość tego elementu do wartości `Issuer` element `AuthnRequest` który zainicjowane logowania jednokrotnego. Aby ocenić `Audience` wartość, należy użyć wartości `App ID URI` , która została określona podczas rejestrowania aplikacji.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Jak `Issuer` wartość `Audience` wartość musi dokładnie odpowiadać jedną główne nazwy usługi, które reprezentuje usług w chmurze w Azure AD. Jednak jeśli wartość argumentu `Issuer` element nie jest wartością identyfikatora URI `Audience` wartość w odpowiedzi jest `Issuer` wartość prefiks `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Ta strona zawiera oświadczeniach o temat lub użytkownika. Poniższy fragment zawiera próbki `AttributeStatement` elementu. Wielokropek wskazuje, że element może zawierać wiele atrybutów i wartości atrybutu.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Roszczeń nazwa** : wartość `Name` atrybut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) jest głównej nazwy użytkownika uwierzytelnionego użytkownika, takie jak `testuser@managedtenant.com`.
- **Roszczeń ObjectIdentifier** : wartość `ObjectIdentifier` atrybut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) jest `ObjectId` reprezentowane przez uwierzytelnionego użytkownika w Azure AD obiektu katalogu. `ObjectId`niezmienne globalnie unikatowe, a ponownie użyć bezpiecznych identyfikator uwierzytelnionego użytkownika.

#### <a name="authnstatement"></a>AuthnStatement

Ten element potwierdza, że temat potwierdzenia został uwierzytelniony w określony sposób w określonym czasie.

- `AuthnInstant` Atrybut określa czas, w którym użytkownik uwierzytelniony z usługą Azure Active Directory.
- `AuthnContext` Element Określa kontekst uwierzytelniania używane do uwierzytelnienia użytkownika.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
