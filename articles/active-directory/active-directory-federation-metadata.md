<properties
    pageTitle="Azure AD Federacji metadanych | Microsoft Azure"
    description="W tym artykule opisano dokument metadanych Federacji publikującej usługi Azure Active Directory dla usług, które przyjmują tokeny usługi Azure Active Directory."
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


# <a name="federation-metadata"></a>Federacja metadanych

Azure Active Directory (Azure AD) publikuje Federacji dokumentu metadanych dla usług, które są skonfigurowane tak, aby zaakceptować tokenów zabezpieczających, które Azure AD problemów. Format dokumentu metadanych Federacji opisano w [sieci Web usług federacyjnych języka (federacyjnych) w wersji 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), która rozszerza [metadanych dla 2.0 OASIS zabezpieczeń potwierdzenia SAML (Markup Language)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Specyficzne dla dzierżawy i punkty końcowe metadanych niezależny od dzierżawy

Azure AD publikuje specyficzne dla dzierżawy i niezależny od dzierżawy punkty końcowe.

Specyficzne dla dzierżawy punkty końcowe są przeznaczone dla określonego dzierżawy. Metadane Federacji specyficzne dla dzierżawy zawierają informacje o dzierżawie, w tym informacje specyficzne dla dzierżawy wystawcy i punkt końcowy. Aplikacje, które ograniczyć dostęp do pojedynczej dzierżawy za pomocą punktów końcowych specyficzne dla dzierżawy.

Punkty końcowe niezależny od dzierżawy Podaj informacje, które są wspólne dla wszystkich Azure AD dzierżaw. Te informacje dotyczą dzierżaw hostowana u *login.microsoftonline.com* i udostępniane są dzierżaw. Niezależny od dzierżawy punkty końcowe są zalecane dla aplikacji wielu dzierżawy, ponieważ nie są skojarzone z dowolnego określonego dzierżawy.

## <a name="federation-metadata-endpoints"></a>Punkty końcowe metadanych Federacji

Azure AD publikuje metadane Federacji na `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Dla **punktów końcowych specyficzne dla dzierżawy** `TenantDomainName` może mieć jedną z następujących typów:

- Nazwa domeny zarejestrowane Azure AD dzierżawa, takich jak: `contoso.onmicrosoft.com`.
- Niezmienne dzierżawa identyfikator domeny, takich jak `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Dla **punktów końcowych niezależny od dzierżawy** `TenantDomainName` jest `common`. Ten dokument zawiera listę elementów Federacji metadanych, które są wspólne dla wszystkich dzierżaw Azure AD, które są obsługiwane w login.microsoftonline.com.

Na przykład punkt końcowy specyficzne dla dzierżawy mogą być `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Punkt końcowy niezależny od dzierżawy jest [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Możesz wyświetlać dokument metadanych Federacji, wpisując ten adres URL w przeglądarce.

## <a name="contents-of-federation-metadata"></a>Zawartość Federacji metadanych

W poniższej sekcji przedstawiono informacje wymagane przez usługi, które używają tokeny wydawany przez Azure AD.

### <a name="entity-id"></a>Identyfikator jednostki

`EntityDescriptor` Element zawiera `EntityID` atrybut. Wartość `EntityID` atrybut reprezentuje wystawcy, oznacza to, że usługa tokenu zabezpieczającego (STS) jaką wydane tokenu. Należy sprawdzić poprawność wystawcy po otrzymaniu tokenu.

Następujące metadane pokazuje przykład specyficzne dla dzierżawy `EntityDescriptor` element z `EntityID` elementu.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Możesz zastąpić identyfikator dzierżawy na punkt końcowy niezależny od dzierżawy swój identyfikator dzierżawy, aby utworzyć konkretną dzierżawy `EntityID` wartość. Wynik będzie taki sam, jak wystawcy tokenów. Strategia pozwala aplikacjom wielu dzierżawy sprawdzania poprawności wystawcy dla danej dzierżawy.

Następujące metadane pokazuje przykład niezależny od dzierżawy `EntityID` elementu. Pamiętaj, że `{tenant}` jest literałów, a nie symbolu zastępczego.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Certyfikaty podpisywania tokenu

Gdy usługa odbierze tokenu, który jest wydawany przez dzierżawy Azure AD, podpisu tokenu należy sprawdzić poprawność przy użyciu klucza podpisywania opublikowaną w dokumencie metadanych federacji. Metadane Federacji zawierają części publicznej certyfikatów, które dzierżaw za pomocą podpisywania tokenu. Bajtów nieprzetworzonych certyfikatu są wyświetlane w `KeyDescriptor` elementu. Certyfikat podpisywania tokenu jest właściwy dla podpisu tylko wtedy, gdy wartość `use` atrybut jest `signing`.

Dokument metadanych Federacji publikowane przez Azure AD może mieć wiele kluczy podpisywania, takich jak podczas Azure AD przygotowuje aktualizację certyfikatu podpisywania. Jeśli dokument metadanych Federacji zawiera więcej niż jeden certyfikat, usługa, która jest sprawdzanie poprawności tokenów powinien obsługiwać wszystkie certyfikaty w dokumencie.

Następujące metadane pokazuje przykład `KeyDescriptor` elementu przy użyciu klucza podpisywania.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

`KeyDescriptor` Element zostanie wyświetlony w dwóch miejscach w dokumencie metadanych federacji. w sekcji WS Federacji określonych i sekcji specyficzne dla SAML. Certyfikaty opublikowanych w obu sekcjach będą takie same.

W sekcji WS Federacji określonych czytnika metadanych federacyjnych czytać certyfikaty z `RoleDescriptor` element z `SecurityTokenServiceType` typu.

Następujące metadane pokazuje przykład `RoleDescriptor` elementu.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

W sekcji specyficzne dla SAML czytnika metadanych federacyjnych czytać certyfikaty z `IDPSSODescriptor` elementu.

Następujące metadane pokazuje przykład `IDPSSODescriptor` elementu.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Istnieją różnice w formacie certyfikaty specyficzne dla dzierżawy i niezależny od dzierżawy.

### <a name="ws-federation-endpoint-url"></a>Adres URL punktu końcowego federacyjnych

Metadane Federacji zawierają adres URL, który jest używana Azure AD dla pojedynczego logowania i pojedyncze wyrejestrowywania w protokole federacyjnych. Ten punkt końcowy pojawi się w `PassiveRequestorEndpoint` elementu.

Następujące metadane pokazuje przykład `PassiveRequestorEndpoint` elementu końcowego specyficzne dla dzierżawy.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Dla punktu końcowego niezależny od dzierżawy adresu URL federacyjnych pojawia się w punkt końcowy federacyjnych, jak pokazano w poniższym przykładzie.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>Adres URL punktu końcowego protokołu SAML

Metadane Federacji zawierają adres URL, który używa Azure AD dla pojedynczego logowania i pojedyncze wyrejestrowywania w protokole SAML 2.0. Poniższe punkty końcowe są wyświetlane w `IDPSSODescriptor` elementu.

Adresy URL logowania i wyrejestrowywania są wyświetlane w `SingleSignOnService` i `SingleLogoutService` elementy.

Następujące metadane pokazuje przykład `PassiveResistorEndpoint` dla dzierżawy określonego punktu końcowego.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Podobnie punkty końcowe dla punktów końcowych typowych protokołu SAML 2.0 są publikowane w metadanych Federacji niezależny od dzierżawy, jak pokazano w poniższym przykładzie.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
