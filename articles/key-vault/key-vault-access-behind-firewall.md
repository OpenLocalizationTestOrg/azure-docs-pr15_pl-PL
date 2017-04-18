<properties
    pageTitle="Dostęp do magazynu klucz za zaporą | Microsoft Azure"
    description="Dowiedz się, jak uzyskać dostęp do magazynu klucz z aplikacją za zaporą"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Uzyskiwanie dostępu do magazynu klucz za zaporą
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>P: mojej aplikacji klienckiej klucza magazynu musi być za zaporą, jakie porty i hosts i adresów należy otwierać umożliwiający dostęp do kluczowych magazynu?

Aby uzyskać dostęp do klucza magazynu aplikacji klienckiej klucza magazynu musi być mieli dostęp do wielu punkty końcowe dla różnych funkcji.

- Uwierzytelnianie (za pośrednictwem Azure Active Directory)
- Zarządzanie klucza magazynu (obejmująca tworzenie/odczytu/aktualizacji i usuwania, a także zasady dostępu ustawienie) za pomocą Menedżera zasobów Azure
- Dostęp i zarządzanie obiektów (klucze i hasła) przechowywanych w klucza magazynu, samej, pokazując klucza magazynu określonego punktu końcowego (np. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

W zależności od konfiguracji i środowisko istnieją pewne różnice.   

## <a name="ports"></a>Porty

Cały ruch do klucza magazynu dla wszystkich funkcji 3 (dostęp płaszczyzny uwierzytelniania, zarządzania i danych) omówiono HTTPS: porcie 443. Jednak w przypadku CRL, będzie od czasu do czasu ruch HTTP (port 80). Klienci, którzy obsługują OCSP nie powinny osiągnięcia listy odwołań certyfikatów, ale czasami może osiągnąć [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Uwierzytelnianie

Aplikacji klienckiej klucza magazynu musisz uzyskać dostęp do punktów końcowych usługi Azure Active Directory dla uwierzytelniania. Punkt końcowy używane w zależności od konfiguracji dzierżawy AAD i typ kapitału — główna użytkownika, wystawcy usługi i typ konta, na przykład Account Microsoft lub identyfikatora organizacji.  

| Typ kapitału | Port punktu końcowego: |
|----------------|---------------|
| Za pomocą Account Microsoft użytkownika<br> (np.user@hotmail.com) | **Globalne:**<br> Login.microsoftonline.com:443<br><br> **Chiny Azure:**<br> Login.chinacloudapi.CN:443<br><br>**Azure Rządu Stanów Zjednoczonych:**<br> us.microsoftonline.com:443 logowania<br><br>**Azure Niemcy:**<br> Login.microsoftonline.de:443<br><br> oraz <br>Login.Live.com:443   |
| Kwota kapitału usługi-użytkownika za pomocą Identyfikatora organizacji AAD (np.user@contoso.com) | **Globalne:**<br> Login.microsoftonline.com:443<br><br> **Chiny Azure:**<br> Login.chinacloudapi.CN:443<br><br>**Azure Rządu Stanów Zjednoczonych:**<br> us.microsoftonline.com:443 logowania<br><br>**Azure Niemcy:**<br> Login.microsoftonline.de:443 |
| Kwota kapitału użytkownika/usługi przy użyciu Identyfikatora + usług ADFS organizacji lub innych federacyjnych punktu końcowego (np.user@contoso.com) | Wszystkie powyższe punkty końcowe na identyfikator organizacji oraz ADFS lub innych federacyjnych punkty końcowe |

Istnieją inne możliwe scenariusze złożonych. Zajrzyj do [Azure Active Directory uwierzytelniania przepływ](/documentation/articles/active-directory-authentication-scenarios/), [Integracji aplikacji z usługą Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) i [Active Directory protokoły](https://msdn.microsoft.com/library/azure/dn151124.aspx) Aby uzyskać dodatkowe informacje.  

## <a name="key-vault-management"></a>Zarządzanie klucza magazynu

Key Management magazynu (OBSŁUGIWAŁ i ustawienie zasad programu access) z aplikacją kliencką klucza magazynu musi uzyskać dostęp do Menedżera zasobów Azure punktu końcowego.  

| Typ operacji | Port punktu końcowego: |
|----------------|---------------|
| Operacje płaszczyzny sterowania magazynu klucza<br> za pomocą Menedżera zasobów Azure | **Globalne:**<br> Management.Azure.com:443<br><br> **Chiny Azure:**<br> Management.chinacloudapi.CN:443<br><br> **Azure Rządu Stanów Zjednoczonych:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Niemcy:**<br> Management.microsoftazure.de:443 |
| Wykres Azure Active Directory interfejsu API | **Globalne:**<br> Graph.Windows.NET:443<br><br> **Chiny Azure:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure Rządu Stanów Zjednoczonych:**<br> Graph.Windows.NET:443<br><br> **Azure Niemcy:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Operacje klucza magazynu

Dla wszystkich klucza magazynu zarządzania obiektu (klucze i hasła) i szyfrowania operacje klient klucza magazynu musi uzyskać dostęp do punktu końcowego klucza magazynu. W zależności od lokalizacji magazynu usługi klucza różni się sufiks DNS punktu końcowego. Punkt końcowy klucza magazynu jest formatu: < nazwa magazynu >. < region--sufiks dns konkretnego-> zgodnie z opisem w poniższej tabeli.  

| Typ operacji | Port punktu końcowego: |
|----------------|---------------|
| Klucz magazynu operacji, takich jak cryptographic symulacji klawiszy, utworzony/odczytu/aktualizowanie i usuwanie kluczy i hasła, ustawić ani uzyskać znaczników i inne atrybuty klucza magazynu obiektów (klucze i hasła)     | **Globalne:**<br> &lt;Nazwa magazynu&gt;. vault.azure.net:443<br><br> **Chiny Azure:**<br> &lt;Nazwa magazynu&gt;. vault.azure.cn:443<br><br> **Azure Rządu Stanów Zjednoczonych:**<br> &lt;Nazwa magazynu&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Niemcy:**<br> &lt;Nazwa magazynu&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>Zakresy adresów IP

Usługa magazynu klucza korzysta z kolei inne Azure zasoby, takie jak PaaS infrastruktury, w związku z tym nie jest możliwe zapewnienie określonego zakresu adresów IP zawierające punkty końcowe usługi klucza magazynu w dowolnym momencie. Jednak jeśli zapory obsługuje tylko adres IP zakresów następnie można znaleźć w dokumencie [Zakresów adresów IP firmy Microsoft Azure centrum danych](https://www.microsoft.com/download/details.aspx?id=41653) .   Do uwierzytelniania i tożsamości (Azure Active Directory) aplikacja muszą mieć możliwość nawiązać punkty końcowe opisany w [uwierzytelniania i adresy tożsamości](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Następne kroki

- Jeśli masz pytania dotyczące magazynu klucza, odwiedź [Fora magazynu klucza Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
