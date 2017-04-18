<properties
    pageTitle="Odwołanie do protokołu Azure AD SAML | Microsoft Azure"
    description="W tym artykule omówiono profile logowania jednokrotnego i pojedynczy SAML Sign-Out w usługi Azure Active Directory."
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
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Jak używane są usługi Azure Active Directory protokołu SAML

Protokół zastosowania SAML 2.0 Azure Active Directory (Azure AD) aby umożliwić aplikacji zapewnić pojedynczy obsługi logowania jednokrotnego użytkownikom. Profile [Logowania jednokrotnego](active-directory-single-sign-on-protocol-reference.md) i SAML [pojedynczej wyrejestrowywania](active-directory-single-sign-out-protocol-reference.md) Azure AD wyjaśniono, jak SAML potwierdzeń, protokoły i powiązań są używane w usłudze dostawcy tożsamości.

Protokół SAML wymaga dostawcy tożsamości (Azure AD) i dostawca usług (aplikacja) do wymiany informacji o sobie.

Podczas rejestrowania aplikacji z usługą Azure Active Directory, Deweloper aplikacji rejestruje informacje związane z federacji z usługą Azure Active Directory. Obejmuje **Przekierowywać URI** i **URI metadanych** aplikacji.

Azure AD używa **Identyfikatora URI metadanych** usługi cloud do pobierania klucz podpisywania i wyrejestrowywanie identyfikator URI usługi w chmurze. Jeśli aplikacji nie obsługuje metadanych identyfikatora URI, deweloper musi kontakt z pomocą techniczną firmy Microsoft o podanie Wyloguj identyfikatora URI i klucz podpisywania.

Azure Active Directory udostępnia specyficzne dla dzierżawy i typowych (niezależny od dzierżawy) pojedynczego logowania jednokrotnego i pojedynczy wyrejestrowywania punkty końcowe. Te adresy URL reprezentują lokalizacje adresach — nie są tylko identyfikatory —, możesz przejść do punktu końcowego do odczytu metadanych.

 - Punkt końcowy specyficzne dla dzierżawy znajduje się w `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  <TenantDomainName> Symbol zastępczy oznacza nazwę domeny zarejestrowane lub identyfikator GUID TenantID dzierżawę Azure AD. Na przykład metadanych Federacji dzierżawie contoso.com znajduje się na: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Punkt końcowy niezależny od dzierżawy znajduje się w `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. W tym adres końcowy **typowych** zostanie wyświetlone, zamiast dzierżawy nazwy domeny lub identyfikatora.

Uzyskać informacji o dokumentach Federacji metadanych, które publikuje Azure AD zobacz [Metadanych Federacji](active-directory-federation-metadata.md).
