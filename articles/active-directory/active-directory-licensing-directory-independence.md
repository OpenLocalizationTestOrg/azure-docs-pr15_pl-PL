<properties
   pageTitle="Dodawanie i zarządzanie nimi wielu katalogów usługi Azure Active Directory | Microsoft Azure"
   description="Instrukcje i najważniejsze wskazówki dotyczące dodawania i zarządzanie katalogów usługi Azure Active Directory wyjaśnieniem katalogi jako całkowicie niezależne zasoby"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Dodawanie i zarządzanie nimi wielu katalogów usługi Azure Active Directory

Azure usługi Active Directory (Azure AD) każdego katalogu jest zasobem całkowicie niezależne: partner, w pełni funkcjonalny i logicznie niezależnie od innych katalogów, którymi zarządzasz. Istnieje Brak relacji nadrzędny podrzędny między katalogów. Ten niezależności między katalogów zawiera niezależności zasobów, niezależności administracyjne i niezależności synchronizacji.

##<a name="resource-independence"></a>Niezależności zasobów

Tworzenie lub usuwanie zasobu z jednego katalogu, ma wpływu na wszystkich zasobów w innym katalogu, z wyjątkiem częściowego użytkowników zewnętrznych, opisane poniżej. Jeśli używasz domeny niestandardowej "contoso.com" z jednego katalogu nie można używać z dowolnego innego katalogu.

##<a name="administrative-independence"></a>Niezależności administracyjne

Jeśli użytkownik niebędący administratorem katalogu "Contoso" następnie tworzy katalog test "Test":
- Domyślnie użytkownik, który tworzy katalog jest dodane jako użytkownika zewnętrznego w tym nowego katalogu i przypisaną rolę administratora globalnego w katalogu.
- Administratorzy katalogu "Contoso" uprawnień nie bezpośredniego administracyjnych do katalogu "Test" chyba że administrator "Test" specjalnie otrzymuje je tych uprawnień. Administratorzy "Contoso" sterować dostępem do katalogu "Test" Jeśli im kontrolować konto użytkownika, który utworzył "Test".
- Jeśli zmienisz (Dodawanie lub usuwanie) roli administratora dla użytkownika z jednego katalogu zmiana nie wpływa na dowolnym rolę administratora, który użytkownik może mieć w innym katalogu.

##<a name="synchronization-independence"></a>Niezależności synchronizacji

Możesz skonfigurować każdego katalogu Azure AD niezależne mają zostać pobrane dane synchronizowane z jednego wystąpienia albo:
  - Narzędzie synchronizacji katalogów (DirSync) umożliwiające synchronizowanie danych z jeden las AD.
  - Azure łącznika usługi Active Directory dla Forefront Identity Manager, aby synchronizowanie danych z jedną lub więcej lasy lokalnego i/lub źródła danych nie Azure AD.

##<a name="add-an-azure-ad-directory"></a>Dodać katalog Azure AD

Aby dodać katalog Azure AD w portalu klasyczny Azure, zaznacz rozszerzenie usługi Azure Active Directory po lewej stronie, a następnie naciśnij przycisk **Dodaj**.

> [AZURE.NOTE]   W przeciwieństwie do innych Azure zasobów z katalogów nie są zasobami podrzędne Azure subskrypcji. Jeśli anulowanie lub zezwalanie na Azure subskrypcja wygaśnie, nadal można korzystać z katalogu danych przy użyciu programu PowerShell Azure, API wykresu Azure lub inne interfejsy takich jak Centrum administracyjne usługi Office 365. Można także skojarzyć innej subskrypcji z katalogu.

Ogólne omówienie problemów licencjonowania Azure AD i najważniejsze wskazówki, zobacz [Co to jest Azure Active Directory licencjonowania?](active-directory-licensing-what-is.md).
