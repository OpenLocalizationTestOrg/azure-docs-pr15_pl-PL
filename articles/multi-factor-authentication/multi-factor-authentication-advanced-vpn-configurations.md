<properties
    pageTitle="Zaawansowanych scenariuszy z Azure uwierzytelnianie wieloskładnikowe i 3 przyjęcie sieci VPN"
    description="Ta strona zawiera informacje dotyczące konfiguracji ustawienia krok po kroku dla Azure MFA z 3 produktów firmy."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Zaawansowanych scenariuszy z Azure uwierzytelnianie wieloskładnikowe i 3 VPN przyjęcie
Uwierzytelnianie wieloskładnikowe Azure może służyć do bezproblemowo nawiązywania połączeń z różnych 3 rozwiązań sieci VPN firmy.  Ta opcja uwzględnia urządzenia Cisco® ASA VPN, Citrix NetScaler SSL VPN urządzenia i urządzenia Juniper sieci bezpiecznego dostępu-Pulse bezpiecznego połączenia bezpiecznego SSL VPN.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Urządzenie ASA VPN firmy Cisco i uwierzytelnianie wieloskładnikowe Azure
Uwierzytelnianie wieloskładnikowe Azure bezproblemowo zintegrować z urządzenia Cisco® ASA VPN zapewnia dodatkowe zabezpieczenia logowania Cisco AnyConnect® VPN dostępu do portalu.  Można to zrobić przy użyciu protokołu LDAP albo RADIUS.  Wybierz jedną z następujących czynności, aby pobrać prowadnice szczegółowe konfiguracji krok po kroku.

Przewodnik konfiguracji  | Opis
------------- | ------------- |
[Cisco ASA z sieci VPN Anyconnect i Azure konfiguracji MFA LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Bezproblemowa integracja urządzenia Cisco ASA VPN z MFA Azure za pomocą usługi LDAP|
[Cisco ASA z sieci VPN Anyconnect i Azure konfiguracji MFA RADIUS](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Bezproblemowa integracja z MFA Azure za pomocą RADIUS urządzenia Cisco ASA VPN

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe Citrix NetScaler SSL VPN i Azure
Uwierzytelnianie wieloskładnikowe Azure bezproblemowo zintegrować z urządzenia Citrix NetScaler SSL VPN zapewnia dodatkowe zabezpieczenia logowania Citrix NetScaler SSL VPN dostępu do portalu.  Można to zrobić przy użyciu protokołu LDAP albo RADIUS.  Wybierz jedną z następujących czynności, aby pobrać prowadnice szczegółowe konfiguracji krok po kroku.

Przewodnik konfiguracji  | Opis
------------- | ------------- |
[Citrix NetScaler SSL VPN i Azure konfiguracji MFA LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Bezproblemowa integracja z Citrix NetScaler SSL VPN z urządzenia Azure MFA za pomocą usługi LDAP|
[Citrix NetScaler SSL VPN i Azure konfiguracji MFA RADIUS](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Bezproblemowa integracja z MFA Azure za pomocą RADIUS urządzenia Citrix NetScaler SSL VPN

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Urządzenia juniper-Pulse bezpiecznego SSL VPN i uwierzytelnianie wieloskładnikowe Azure
Uwierzytelnianie wieloskładnikowe Azure bezproblemowo zintegrować z urządzenia Juniper-Pulse bezpiecznego SSL VPN zapewnia dodatkowe zabezpieczenia logowania Juniper-Pulse bezpiecznego SSL VPN dostępu do portalu.  Można to zrobić przy użyciu protokołu LDAP albo RADIUS.  Wybierz jedną z następujących czynności, aby pobrać prowadnice szczegółowe konfiguracji krok po kroku.

Przewodnik konfiguracji  | Opis
------------- | ------------- |
[Juniper-Pulse zabezpieczeń SSL VPN i Azure konfiguracji MFA LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Bezproblemowa integracja z Juniper-Pulse bezpiecznego SSL VPN z urządzenia Azure MFA za pomocą usługi LDAP|
[Juniper-Pulse zabezpieczeń SSL VPN i Azure konfiguracji MFA RADIUS](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Bezproblemowa integracja z MFA Azure za pomocą RADIUS urządzenia Juniper-Pulse bezpiecznego SSL VPN
