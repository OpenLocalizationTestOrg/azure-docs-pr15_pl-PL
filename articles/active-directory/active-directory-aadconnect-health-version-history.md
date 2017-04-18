<properties
    pageTitle="Azure AD Connect kondycji Historia wersji"
    description="W tym dokumencie opisano tych wersjach ds Azure AD połączyć co został uwzględniony w tych wersjach."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect kondycji: Historia wersji wersji

Zespół usługi Azure Active Directory regularnie aktualizuje Azure AD kondycji nawiązywanie połączenia z nowych funkcji i funkcji. Ten artykuł zawiera listę oraz funkcje, które zostały wydane.

## <a name="october-2016"></a>Październik 2016
**Aktualizacja agenta:**
- Azure agent kondycji łączenie AD usług AD FS \(wersji 2.6.408.0\)
    1. Ulepszenia wykrywania adresów IP klienta w żądania uwierzytelniania
    2. Poprawki dotyczące alertów
- Azure agent kondycji łączenie AD w usługach AD DS (wersja 2.6.408.0)
    1. Poprawki dotyczące alertów.
- Azure agent kondycji łączenie AD do synchronizacji (wersja 2.6.353.0) wydane z Azure AD Connect wersji 1.1.281.0
    1. Podaj wymagane dane dla raportów o błędach synchronizacji
    2. Poprawki dotyczące alertów

**Nowe funkcje podglądu:**
- Łączenie raportów o błędach synchronizacji dla Azure AD

**Nowe funkcje:**
- Azure AD łączenie kondycja usług AD FS - pola adresu IP jest dostępna w raporcie o górnym 50 użytkowników przy użyciu nieprawidłowe nazwy użytkownika i hasła.

## <a name="july-2016"></a>Lipiec 2016

**Nowe funkcje podglądu:**

- [Azure AD Connect kondycji w usługach AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Stycznia 2016


**Aktualizacja agenta:**

- Azure agent kondycji łączenie AD usług AD FS (wersja 2.6.91.1512)


**Nowe funkcje:**

- [Narzędzia Connectivity test dla Azure AD łączenie agentów kondycji](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>Listopada 2015 r.


**Nowe funkcje:**

- Obsługa [Kontrola dostępu oparta na rolach](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Nowe funkcje podglądu:**

- [Azure AD łączenie kondycji do synchronizacji](active-directory-aadconnect-health-sync.md).

**Stały problemów:**

- Poprawki błędów podczas agenta rejestracji.

## <a name="september-2015"></a>Września 2015 r.

**Nowe funkcje:**

- Niewłaściwa nazwa_użytkownika hasło raportu usług AD FS
- Obsługa techniczna, aby skonfigurować nieuwierzytelnionych serwer Proxy HTTP
- Obsługa techniczna, aby skonfigurować agenta na Server core
- Poprawa alerty dotyczące usług AD FS
- Przekaż ulepszenia Azure AD łączenie Agent kondycji usług AD FS łączności i danych.


**Stały problemów:**

- Poprawki w wniosków zastosowania typu AD FS błędu.


## <a name="june-2015"></a>Czerwca 2015 r.

**Początkowy wersji Azure AD łączenie zdrowia dla usług AD FS i serwer Proxy usług AD FS.**

**Nowe funkcje:**

- Alerty monitorowania serwerów usług AD FS i serwer Proxy usług AD FS z powiadomień e-mail.
- Łatwy dostęp do usług AD FS topologii i desenie w polu liczniki wydajności AD FS.
- Trend w pomyślnego token wezwań na serwerach usług AD FS pogrupowane według aplikacji, metod uwierzytelniania, żądanie sieci lokalizacji itp.
- Trendy w żądań zakończonych niepowodzeniem na serwerach usług AD FS pogrupowane według aplikacji, błąd typy itp.
- Prostsze wdrażanie agenta przy użyciu poświadczeń administratora globalnego Azure AD.  




## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [Monitora sieci lokalnej tożsamości infrastruktury i synchronizacji usługi w chmurze](active-directory-aadconnect-health.md).
