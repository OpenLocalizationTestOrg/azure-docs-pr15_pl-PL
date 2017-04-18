<properties 
    pageTitle="Usługi Bus uwierzytelniania i autoryzacji | Microsoft Azure"
    description="Ogólne informacje o uwierzytelnianiu podpisu udostępnionych programu Access (SA)."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Bus usługi uwierzytelniania i autoryzacji

Aplikacje można uwierzytelnić Bus usługi Azure przy użyciu funkcji uwierzytelniania albo udostępnione podpis programu Access (SA) lub za pośrednictwem Azure Active Directory kontroli dostępu (nazywane także usługa Kontrola dostępu lub ACS). Udostępniane aplikacje umożliwia uwierzytelnianie podpisu dostęp do uwierzytelniania za pomocą klawisza dostępu skonfigurowane w obszarze nazw lub na jednostkę, z którymi są skojarzone określone prawa Bus usługi. Następnie za pomocą tego klucza do wygenerowania tokenu podpisu dostępu do udostępnionego, którego klienci mogą używać do uwierzytelniania usługi Bus.

>AZURE. WAŻNE skojarzeń zabezpieczeń zaleca się nad ACS, ponieważ tworzy ona schemat prosty, elastyczne i łatwy w użyciu uwierzytelniania dla usługi Bus. Aplikacje mogą używać skojarzeń zabezpieczeń w sytuacjach, w których nie potrzebujesz, zarządzanie pojęcia autoryzowanych "użytkownik."

## <a name="shared-access-signature-authentication"></a>Uwierzytelnianie za pomocą udostępnionego podpis programu Access

[Uwierzytelnianie skojarzeń zabezpieczeń](service-bus-sas-overview.md) umożliwia Udziel użytkownikowi dostępu do usług Bus zasoby z uprawnieniami. Uwierzytelnianie skojarzeń zabezpieczeń w Bus usługa obejmuje konfigurację klucz kryptograficzny skojarzone uprawnienia do zasobu usługi Bus. Klienci mogą uzyskać dostęp do tego zasobu przez przedstawienie tokenu skojarzeń zabezpieczeń składający się z wykorzystywane identyfikator URI zasobu i wygaśnięcia podpisanych skonfigurowanym kluczem.

Klucze można konfigurować dla skojarzeń zabezpieczeń dla nazw Bus usługi. Klucz dotyczy wszystkich jednostek wiadomości w tym nazw. Można również skonfigurować klawiszy na tematy i kolejek Bus usługi. Skojarzenia zabezpieczeń jest również obsługiwane na Bus usługi przekazuje.

Aby użyć skojarzeń zabezpieczeń, możesz skonfigurować obiektu [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) na przestrzeń nazw, kolejki lub tematu, który składa się z następujących czynności:

- *Nazwa klucza* , który identyfikuje reguły.

- *PrimaryKey* jest kluczem szyfrowania używany do logowania i sprawdzania poprawności tokenów skojarzeń zabezpieczeń.

- *Klucz pomocniczy* jest kluczem szyfrowania używany do logowania i sprawdzania poprawności tokenów skojarzeń zabezpieczeń.

- Przedstawiająca zbiór słuchać, Wyślij lub zarządzanie przyznane *uprawnienia* .

Reguły autoryzacji skonfigurowane na poziomie nazw może udzielić dostępu do wszystkich obiektów w obszarze nazw dla klientów z tokeny zalogowani przy użyciu odpowiedniego klucza. Maksymalnie 12 zezwolenia reguły można skonfigurować na przestrzeń nazw Bus usługi, kolejki lub temat. Domyślnie [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) wszystkie uprawnienia skonfigurowano dla każdej nazw po raz pierwszy zainicjowaniu obsługi administracyjnej.

Do uzyskiwania dostępu do obiektu, klient wymaga tokenu skojarzeń zabezpieczeń wygenerowanych przy użyciu określonej [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Token skojarzeń zabezpieczeń, jest generowany przy użyciu HMAC SHA256 do zasobu ciąg, który składa się z identyfikatora URI zasobu, do którego jest wniosek dostępu i wygaśnięcia kluczem szyfrowania skojarzone z regułą autoryzacji.

Obsługa uwierzytelniania skojarzeń zabezpieczeń usługi Bus jest uwzględniona w Azure .NET SDK w wersji 2.0 lub nowszy. Skojarzenia zabezpieczeń obsługuje [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Wszystkich interfejsów API, które przyjmują parametry połączenia jako parametr obsługują ciągi połączeń skojarzeń zabezpieczeń.

## <a name="acs-authentication"></a>Uwierzytelnianie ACS

Bus usługi uwierzytelniania za pośrednictwem ACS odbywa się za pośrednictwem dodatek "-sb" ACS nazw. Jeśli chcesz przestrzeń nazw ACS companion do utworzenia nazw Bus usługi nie można utworzyć obszaru nazw Bus usługi przy użyciu portalu klasyczny Azure; Musisz utworzyć nazw przy użyciu polecenia cmdlet programu PowerShell [AzureSBNamespace nowy](https://msdn.microsoft.com/library/azure/dn495165.aspx) . Na przykład:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Aby uniknąć tworzenia nazw ACS, Wydaj następujące polecenie:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Na przykład jeśli utworzysz nazw Bus usługi o nazwie **contoso.servicebus.windows.net**nazw ACS pomocniczym, o nazwie **contoso sb.accesscontrol.windows.net** jest obsługi administracyjnej automatycznie. Dla wszystkie przestrzenie nazw, które zostały utworzone przed sierpnia 2014 r towarzyszące nazw ACS również został utworzony.

Tożsamość usługi domyślne "właściciel" wszystkie uprawnienia jest obsługi administracyjnej domyślnie w tym obszarze nazw ACS pomocniczym. Można uzyskać szczegółowe kontrolki osobę Bus usług za pośrednictwem ACS przez skonfigurowanie relacji zaufania odpowiednie. Możesz skonfigurować tożsamością dodatkowe usługi Zarządzanie dostępem do obiektów Bus usługi.

Do uzyskiwania dostępu do obiektu, klient żąda tokenu SWT od ACS odpowiednie roszczeń prezentując swoje poświadczenia. SWT token musi następnie wysyłane jako część żądania do usługi Bus umożliwiające autoryzacji klienta dostępu do obiektu.

Obsługa uwierzytelniania ACS Bus usługi jest uwzględniona w Azure .NET SDK w wersji 2.0 lub nowszy. Uwierzytelnianie obsługuje [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). Wszystkich interfejsów API, które przyjmują parametry połączenia jako parametr obsługują ciągi połączeń ACS.

## <a name="next-steps"></a>Następne kroki

Kontynuuj czytania [udostępnione podpisu dostępu uwierzytelnianie za pomocą usługi Bus](service-bus-shared-access-signature-authentication.md) uzyskać więcej informacji o skojarzeń zabezpieczeń.

Aby uzyskać omówienie skojarzeń zabezpieczeń w Bus usługi zobacz [Udostępnione podpisy programu Access](service-bus-sas-overview.md).

Można znaleźć więcej informacji na temat tokenów ACS w [jak: Poproś ACS za pomocą protokołu ZAWIJANIE OAuth tokenu](https://msdn.microsoft.com/library/hh674475.aspx).



