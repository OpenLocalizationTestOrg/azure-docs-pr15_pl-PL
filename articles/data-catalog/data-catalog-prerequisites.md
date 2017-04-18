<properties
   pageTitle="Azure wymagania wstępne dotyczące wykazu danych | Microsoft Azure"
   description="Azure wstępnych wykazu danych — co jest potrzebne do dokumentu wprowadzenie do wykazu danych Azure."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure wymagania wstępne dotyczące wykazu danych

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Co należy rozpocząć pracę z wykazu danych Azure?

Istnieje kilka rzeczy, które musisz zajmować się przed możesz skonfigurować **Azure wykazu danych**. Nie martw się — nie trwa długo!

## <a name="azure-subscription"></a>Azure subskrypcji
Aby skonfigurować wykaz danych Azure, musi być właścicielem lub współwłaściciela Azure subskrypcji.

Subskrypcje Azure ułatwić organizowanie dostęp do chmury usługi zasoby takie jak Azure wykazu danych. One również pomocy kontrolowanie, jak zgłoszone jest użycie zasobu, dotyczy rozliczenie i opłacona. Każdej subskrypcji może mieć różnych konfiguracji rozliczeń i płatności, więc musisz różnych subskrypcjach i różnych planów przez dział, project, biuro regionalne i tak dalej. Co usługa w chmurze należy do subskrypcji, a musisz mieć subskrypcję przed rozpoczęciem konfigurowania Azure wykazu danych. Aby uzyskać więcej informacji, zobacz [Zarządzanie kontami, subskrypcji i role administracyjne](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Aby skonfigurować Azure wykazu danych, użytkownik zalogowany przy użyciu konta użytkownika usługi Azure Active Directory.

Azure Active Directory (Azure AD) umożliwia łatwe dla swojej firmy, tożsamości i dostęp zarówno w chmurze i lokalnych do zarządzania. Użytkownicy mogą jednej służbowego lub konta służbowego dla logowania jednokrotnego w chmurze dowolnego i lokalnej aplikacji sieci web. Wykaz danych Azure używa Azure AD do uwierzytelniania logowania jednokrotnego. Aby uzyskać więcej informacji, zobacz [Co to jest Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] [Azure portal](http://portal.azure.com/) umożliwia użytkownikom konto służbowe Zaloguj się przy użyciu osobistego Account Microsoft lub pracy usługi Azure Active Directory. Aby skonfigurować wykaz danych Azure za pomocą portalu Azure lub za pomocą [portalu wykazu danych](http://www.azuredatacatalog.com) użytkownik zalogowany przy użyciu konta usługi Azure Active Directory, osobistego konta.

## <a name="active-directory-policy-configuration"></a>Konfiguracja zasady w usłudze Active Directory

W niektórych sytuacjach użytkownicy mogą wystąpić sytuacja miejsce, w którym można zalogować się do portalu Azure wykazu danych, ale przy próbie Zaloguj się do narzędzia rejestracji źródła danych przez nich problemach komunikat o błędzie, który uniemożliwia logowanie. Zachowanie ten problem może wystąpić, tylko wtedy, gdy użytkownik znajduje się w sieci firmowej lub może wystąpić tylko wtedy, gdy użytkownik nawiązuje połączenie z spoza sieci firmowej.

Narzędzie do rejestracji źródła danych użyto uwierzytelniania formularzy, aby sprawdzić poprawność logowania użytkowników w usłudze Active Directory. Pomyślne logowanie uwierzytelniania formularzy musi być włączone w globalnej zasady uwierzytelniania przez administratora usługi Active Directory.

Globalne zasady uwierzytelniania umożliwia metody uwierzytelniania należy włączyć osobno dla sieci intranet i ekstranetu połączeń, jak pokazano poniżej. Błędy logowania może wystąpić, jeśli nie włączono uwierzytelnianie formularzy dla sieci, z której użytkownik nawiązuje połączenie.

 ![Zasady globalnej uwierzytelniania usługi Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Aby uzyskać więcej informacji zobacz [Konfigurowanie zasad uwierzytelniania](https://technet.microsoft.com/library/dn486781.aspx).
