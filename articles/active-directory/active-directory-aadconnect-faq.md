<properties
    pageTitle="Azure AD Connect: Często zadawane pytania dotyczące | Microsoft Azure"
    description="Ta strona zawiera często zadawane pytania dotyczące Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Connect — często zadawane pytania

## <a name="general-installation"></a>Ogólne instalacji
**P: instalacji działa Jeśli Azure AD administrator globalny ma 2FA włączone?**  
Z kompilacjach z lutego 2016 jest to obsługiwane.

**P: czy istnieje sposób, aby zainstalować narzędzie Azure AD Connect nienadzorowanego?**  
Jest obsługiwana tylko zainstalować narzędzie Azure AD Connect przy użyciu Kreatora instalacji. Instalacja nienadzorowanego i cichym nie jest obsługiwana.

**P: Mam las, w którym nie skontaktować się z jedną domenę. Jak zainstalować narzędzie Azure AD Connect**  
Z kompilacjach z lutego 2016 jest to obsługiwane.

**P: agent kondycji usług AD DS działa na core serwera?**  
Wartość Tak. Po zainstalowaniu agenta można ukończyć proces rejestracji przy użyciu następujące polecenia programu PowerShell: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Sieci
**P: Mam urządzenia sieci, zapory lub czegoś innego, który ogranicza maksymalny czas połączenia może pozostać otwarte w mojej sieci. Jak długo Moje próg limit czasu po stronie klienta należy podczas korzystania z Azure AD Connect?**  
Łączność między serwera, w którym zainstalowano narzędzie Azure AD Connect klienta i usługi Azure Active Directory, wszystkie oprogramowania sieciowego, fizycznymi lub pozostałej zawartości, który ogranicza maksymalny czas połączenia może pozostać otwarte należy użyć progu co najmniej 5 minut (300 sekund). Dotyczy to również wszystkie poprzednio wydane narzędzia synchronizacji Identity firmy Microsoft.

**P: są obsługiwane przez (pojedynczej etykiety domen)?**  
Nie, Azure AD Connect nie obsługuje lokalnego lasów i domen przy użyciu drugiego poziomu.

**P: są "kropkowaną" NetBios o nazwie obsługiwane?**  
Nie, Azure AD Connect nie obsługuje lokalnego lasów i domen miejsce, w którym nazwa NetBios zawiera okres "." w polu Nazwa.

## <a name="federation"></a>Federacja
**P: co zrobić, jeśli otrzymuję wiadomości e-mail, które monit o odnowieniu certyfikatu usługi Office 365**  
Za pomocą wskazówek, który jest opisane w tym temacie [odnawiać certyfikaty](active-directory-aadconnect-o365-certs.md) dotyczące odnowienia certyfikatu.

**P: Mam "Automatyczne aktualizowanie uzależnioną" ustawieniem uzależnioną usługi Office 365. Czy muszę podejmować żadnych działań po Moje token certyfikatu podpisywania automatycznie przesuwany nad?**  
Za pomocą wskazówek, które zawarto w artykule [Odnawianie certyfikatów](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Środowisko
**P: Zmień nazwę serwera, po zainstalowaniu Azure AD Connect jest obsługiwana?**  
Wartość nie. Zmiana nazwy serwera spowoduje, że aparat synchronizacji, aby nie można nawiązać połączenia z bazą danych SQL i usługa nie będzie można uruchomić.

## <a name="identity-data"></a>Dane tożsamości
**P: głównej nazwy użytkownika (userPrincipalName) atrybut Azure AD jest niezgodna UPN lokalna – Dlaczego?**  
Zobacz następujące artykuły:

- [Nazwy użytkowników w usłudze Office 365, Azure lub usługi nie są zgodne lokalnej głównej lub identyfikator logowania alternatywny](https://support.microsoft.com/en-us/kb/2523192)
- [Zmiany nie są synchronizowane przez narzędzie Azure Active Directory Sync po zmianie UPN konta użytkownika, aby użyć innej domeny federacyjnych](https://support.microsoft.com/en-us/kb/2669550)

Można również skonfigurować Azure AD umożliwia aparat synchronizacji, aby zaktualizować userPrincipalName, zgodnie z opisem w [Narzędzie Azure AD Connect funkcje usługi synchronizacji](active-directory-aadconnectsyncservice-features.md).

## <a name="custom-configuration"></a>Konfiguracja niestandardowa
**P: gdzie opisano polecenia cmdlet programu PowerShell dla Azure AD Connect**  
Z wyjątkiem poleceń cmdlet opisane w tej witrynie innych poleceń cmdlet programu PowerShell dostępny w narzędzie Azure AD Connect nie są obsługiwane do użycia przez klientów.

* *P: czy mogę używać "serwer eksportu Server importowanie" znaleziony w *Menedżerze usługi synchronizacji* przenosić konfiguracji między servers? **  
Wartość nie. Ta opcja nie pobierze wszystkie ustawienia konfiguracji i nie może być używana. Zamiast tego należy użyć Kreatora tworzenia konfiguracji podstawowej na drugi serwer i generowanie skryptów programu PowerShell, aby przenieść dowolny reguły niestandardowej między serwerami przy użyciu edytora reguły synchronizacji. Zobacz [Przenoszenie Konfiguracja niestandardowa z aktywne na serwerze](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Rozwiązywanie problemów
**P: jak można uzyskać pomoc dotyczącą Azure AD Connect?**

[Przeszukiwanie bazy wiedzy Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Wyszukiwanie Microsoft Knowledge Base (KB) technicznych rozwiązań typowych problemów fix podział o obsługę Azure AD Connect.

[Fora usługi Active Directory platformy Microsoft Azure](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Można przeszukiwać i przeglądać dla techniczne pytania i odpowiedzi ze społeczności lub zadawać własne pytania, klikając [tutaj](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect działu obsługi klienta](https://manage.windowsazure.com/?getsupport=true)

- Uzyskiwanie pomocy technicznej za pośrednictwem portalu Azure za pomocą tego łącza.
