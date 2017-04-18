<properties
   pageTitle="Znakowanie wskazówki dla aplikacji | Microsoft Azure"
   description="Kompleksowy przewodnik Deweloper zorientowane na zasoby dla usługi Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Znakowanie wskazówki dla aplikacji


W tym temacie omówiono znakowania wskazówek, które należy używać podczas tworzenia aplikacji z usługą Azure Active Directory (Azure AD). Poniższe wskazówki ułatwią bezpośredni klientów, gdy chcą za pomocą swojego konta służbowego, zarządzania w Azure AD lub osobistego konta dla zapisów i zaloguj się do aplikacji.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Konta osobiste a praca lub szkolnych kont od firmy Microsoft

Microsoft zarządza dwóch rodzajów kont użytkowników:

- **Konta osobiste** (dawniej znana jako identyfikator Windows Live ID). Te konta reprezentują relacje między *poszczególnych* użytkowników i firmy Microsoft i pozwalają uzyskać dostęp do urządzenia i usługi firmy Microsoft. Te konta są przeznaczone do użytku osobistego.

- **Konta służbowego.** Te konta są zarządzane przez firmę Microsoft w imieniu organizacjami używającymi usługi Azure Active Directory. Te konta są używane do zalogowania się do usługi Office 365 i usług innych firm od firmy Microsoft.

Microsoft konta służbowego zwykle są przypisywane do użytkowników (pracowników, uczniów lub studentów, federal pracowników) przez ich organizacji (firmy, szkoły, agencja rządowa). Te konta są format zarządzany bezpośrednio w chmurze, platformy Azure AD lub zsynchronizowanym z usługą Azure AD z katalogu lokalnego, takie jak Windows Server Active Directory. Microsoft jest *powiernik* konta służbowego, ale konta są własnością i kontrolowane przez organizację.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Odwołując się do konta Azure AD w aplikacji

Microsoft nie zapewnia użytkowników końcowych do Azure lub firmowe usługi Active Directory, a nie należy do Ciebie.

- Po zalogowaniu użytkowników należy używać możliwie logo i nazwa organizacji. To jest większa niż za pomocą ogólnego terminów, takich jak "organizacja".

- Gdy użytkownicy nie jest zalogowany, należy zapoznać się z ich kontami jako "Praca lub szkolnych kont" i używanie logo firmy Microsoft w celu przekazania, że te konta są zarządzane przez firmę Microsoft. Nie należy używać terminów, takich jak "enterprise konta", "konto firmowe" lub "konto firmowe", które pomyłki użytkownika.

## <a name="user-account-pictogram"></a>Piktogram konta użytkownika
We wcześniejszej wersji tych wytycznych zaleca się przy użyciu piktogram "niebieski znaczek". W zależności od użytkowników i deweloper opinii, teraz zalecamy użycie logo firmy Microsoft zamiast tego. Pomoże użytkownikom zrozumieć, że ich ponowne używanie konta, które korzystają z usługi Office 365 lub innego konta Microsoft usług biznesowych do zalogowania się do aplikacji.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Logowanie i zalogować się za pomocą Azure AD

Aplikacji mogą stanowić osobnych ścieżki zapisów i logowania i w poniższych sekcjach przedstawiono wskazówek wizualnych w obu przypadkach.

**Jeśli aplikacji obsługuje logowania użytkownika końcowego w górę (np. bezpłatnej wersji próbnej lub freemium modelu)**: można wyświetlić przycisk **logowania** , który umożliwia użytkownikom dostęp do aplikacji z ich konta służbowego lub ich osobistego konta. Azure AD pojawi się monit o zgody po raz pierwszy będą uzyskiwać dostęp do aplikacji.

**Jeśli aplikacji wymaga uprawnienia, które mogą przyznać tylko Administratorzy, lub jeśli aplikacji wymaga licencjonowania organizacji**: należy oddzielić nabycia administratora w logowania użytkownika. **Przycisk "Pobierz tej aplikacji"** przekieruje Administratorzy Zaloguj, a następnie poproś ich, aby wyrazić zgody w imieniu użytkowników w organizacji. Ma to dodane zaletą pomijanie instrukcjami zgody użytkowników końcowych do aplikacji.

## <a name="visual-guidance-for-app-acquisition"></a>Wizualne wytyczne dotyczące nabycia aplikacji

Łącze "Pobierz aplikację" musisz utworzyć przekierowanie użytkownika do Azure AD udzielanie dostępu (Autoryzuj) strony, aby umożliwić administratorem organizacji, aby zezwolić aplikacji mają dostęp do danych organizacji, która jest obsługiwana przez firmę Microsoft. Szczegółowe informacje dotyczące żądania dostępu są omawiane w artykule [Integracji aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md) .

Po Administratorzy wyraża zgodę na aplikacji, mogą oni wybierać ją dodać do (dostępne z wafla i [https://portal.office.com/myapps](https://portal.office.com/myapps)) środowisko uruchamiania aplikacji usługi Office 365 swoich użytkowników. Jeśli chcesz ogłaszanie tej funkcji można używać terminów, takich jak "Dodaj tę aplikację do organizacji" oraz wyświetlanie przycisku tak jak poniżej:

![Typy aplikacji i scenariusze](./media/active-directory-branding-guidelines/add-to-my-org.png)

Jednak zaleca się, że zapisywać tekst objaśniający zamiast Polegaj na przycisków. Na przykład:
> *Jeśli już korzystasz z usługi Office 365 lub innych firm usług firmy Microsoft, możesz po prostu przyznać < your_app_name > dostęp do danych Twojej organizacji. Dzięki temu będzie użytkownikom dostęp do < your_app_name > ze swoich istniejących kont pracy.*


## <a name="visual-guidance-for-sign-in"></a>Wizualne wskazówki dotyczące logowania
Aplikacji powinien być wyświetlany znak przycisku, który przekierowuje użytkowników do logowania końcowego, która odpowiada protokół, którego używasz, aby zintegrować z usługą Azure Active Directory. W poniższej sekcji przedstawiono szczegółowe informacje na co powinna wyglądać podobnie do tego przycisku.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogram i "Zaloguj się przy użyciu programu Microsoft"
To skojarzenie logo firmy Microsoft i terminy "Microsoft with w znak" jednoznacznie reprezentuje Azure AD wśród innych dostawców tożsamości aplikacji może obsługiwać. Jeśli nie masz wystarczającej "Microsoft z w znak", to ok, aby skrócić go do "Logowania".

![Typy aplikacji i scenariusze](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Typy aplikacji i scenariusze](./media/active-directory-branding-guidelines/sign-in-light.png)

Za pomocą schemat kolorów ciemny przycisków.

![Typy aplikacji i scenariusze](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Typy aplikacji i scenariusze](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Znakowanie porady

**Należy** używać "konto służbowe" w połączeniu z przyciskiem "Microsoft z w znak" zapewnienie dodatkowe objaśnienia do pomocy użytkownikom końcowym rozpoznaje, czy może z niej korzystać. **Nie** używaj inne terminy, takie jak "enterprise konta", "konto firmowe" lub "konto firmowe".

**Nie** używaj "Office 365 identyfikator" lub "Azure". Usługi Office 365 jest również nazwę konsumenta oferowane przez firmę Microsoft, które nie używa Azure AD dla uwierzytelniania.

**Nie** należy zmieniać logo firmy Microsoft.

**Nie** uwidaczniają użytkowników końcowych do marek Azure lub usługi Active Directory. Ok jest jednak aby korzystać z tych terminów z projektantów, informatyków i administratorów.

## <a name="navigation-dos-and-donts"></a>Porady nawigacji

**WYKONAJ** umożliwiają użytkownikom Wyloguj się, a następnie przełącz się do innego konta użytkownika. Podczas gdy większość osób jedno konto osobiste z firmy Microsoft/Facebook i Google i Twitter, osoby często są skojarzone z więcej niż jednej organizacji. Obsługa wielu użytkowników zalogowany jest już wkrótce.
