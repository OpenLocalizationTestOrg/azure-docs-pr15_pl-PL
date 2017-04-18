<properties
   pageTitle="Zabezpieczenia Azure automatyzacji | Microsoft Azure"
   description="Ten artykuł zawiera omówienie automatyzacji bezpieczeństwa i metody uwierzytelniania różnych dostępne w przypadku kont automatyzacji w automatyzacji Azure."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Automatyzacja zabezpieczeń, bezpiecznego automatyzacji" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure zabezpieczeń automatyzacji
Automatyzacja Azure umożliwia zautomatyzowanie przed zasobów Azure, lokalnego oraz z innymi dostawcami chmurze, takiej jak Amazon usług sieci Web (AWS).  Aby działań aranżacji do jego działania wymagane go musi mieć uprawnienia do bezpieczny dostęp do zasobów z minimalnymi prawami wymagane w subskrypcji.  
Ten artykuł obejmuje różne scenariusze uwierzytelniania obsługiwanych przez automatyzację Azure i procedurach pokazano, jak rozpocząć pracę w zależności od środowiska lub środowiskach, które są potrzebne do zarządzania.  

## <a name="automation-account-overview"></a>Omówienie kont automatyzacji
Po uruchomieniu automatyzacji Azure po raz pierwszy, musisz utworzyć co najmniej jedno konto automatyzacji. Konta automatyzacji umożliwiają wyodrębnienia zasobów automatyzacji (runbooks, elementy zawartości konfiguracji) z zasoby zawarte w innych kont automatyzacji. Konta automatyzacji umożliwia dzielenie zasobów na osobne środowiskach logiczne. Na przykład można użyć jednego konta rozwoju, innej produkcji oraz innej w środowisku lokalnym.  Konto Azure automatyzacji różni się od konto Microsoft lub konta utworzone w ramach subskrypcji Azure.

Zasoby automatyzacji dla każdego konta automatyzacji są skojarzone z pojedynczego obszaru Azure, ale konta automatyzacji umożliwia zarządzanie zasobami w dowolnym regionie. Główną przyczyną kont automatyzacji w różnych regionach będzie, jeśli masz zasady, które wymagają danych i zasoby, aby się odizolowane do określonego regionu.

>[AZURE.NOTE]Automatyzacja kont i zasobów, które zawierają są tworzone w portalu Azure, nie są dostępne w portalu klasyczny Azure. Jeśli chcesz zarządzać tych kont lub ich zasobów przy użyciu programu Windows PowerShell, należy użyć moduły Azure Menedżera zasobów.

Wszystkie zadania, które można wykonać przed zasobów za pomocą Menedżera zasobów Azure i Azure poleceń cmdlet w automatyzacji Azure musi uwierzytelnić Azure za pomocą uwierzytelniania opartego na poświadczeń tożsamości organizacyjnej usługi Azure Active Directory.  Na podstawie certyfikatu uwierzytelniania był oryginalny metody uwierzytelniania w trybie Zarządzanie usługą Azure, ale został on skomplikowane konfigurację.  Uwierzytelnianie Azure z użytkownikiem Azure AD został wprowadzane Wstecz w 2014, aby nie tylko uprościć proces Aby skonfigurować konto uwierzytelniania, ale również obsługiwana możliwość non interakcyjnie uwierzytelniania Azure za pomocą konta jednego użytkownika, których praca z Menedżera zasobów Azure i zasoby klasyczny.   

Obecnie, podczas tworzenia nowego konta automatyzacji w portalu Azure, automatycznie tworzy:

-  Uruchom jako konto, które tworzy nowy podmiot usługi Azure Active Directory, certyfikat i przypisuje kontrola dostępu oparta na rolach współautora (RBAC), która będzie używana do zarządzania zasobami Menedżera zasobów za pomocą runbooks.
-  Klasyczny konta Uruchom jako, przekazując certyfikatu zarządzania, która będzie używana do zarządzania usługą Azure czy klasyczny zasobów za pomocą runbooks.  

Kontrola dostępu oparta na rolach jest dostępna przy użyciu Menedżera zasobów Azure Aby udzielić dozwolone akcje do konta użytkownika Azure AD i Uruchom jako konto i uwierzytelniania wystawcy tej usługi.  Przeczytaj [Kontrola dostępu oparta na rolach w artykule automatyzacji Azure](../automation/automation-role-based-access-control.md) dalsze informacje pomagające w tworzeniu modelu zarządzania uprawnieniami automatyzacji.  

Runbooks uruchomionych pracownikiem działań aranżacji hybrydowych w centrum danych lub usług w AWS obliczeniowych nie można użyć tej samej metody, który jest zazwyczaj używany do uwierzytelniania runbooks Azure zasobów.  Jest tak, ponieważ te zasoby są uruchomione poza Azure i dlatego będzie wymagało własne poświadczeń zabezpieczeń zdefiniowane w automatyzacji do uwierzytelniania zasoby, które będą oni dostęp lokalnie.  

## <a name="authentication-methods"></a>Metody uwierzytelniania

W poniższej tabeli podsumowano różnych metod uwierzytelniania dla każdego środowiska obsługiwanych przez automatyzację Azure i artykuł z opisem sposobu ustawienia uwierzytelniania dla Twojej runbooks.

Metoda  |  Środowisko  | Artykuł
----------|----------|----------
Konto użytkownika Azure AD | Azure Menedżer zasobów i zarządzanie usługą Azure | [Typ poświadczeń uwierzytelniania Runbooks Azure AD konta](../automation/automation-sec-configure-aduser-account.md)
Uruchom Azure jako konta | Azure Menedżera zasobów | [Typ poświadczeń uwierzytelniania Runbooks konta Azure Uruchom jako](../automation/automation-sec-configure-azure-runas-account.md)
Azure klasyczny Uruchom jako konta | Zarządzanie usługą Azure | [Typ poświadczeń uwierzytelniania Runbooks konta Azure Uruchom jako](../automation/automation-sec-configure-azure-runas-account.md)
Uwierzytelnianie systemu Windows | Centrum danych lokalnych | [Uwierzytelnianie Runbooks hybrydowych działań aranżacji pracowników](../automation/automation-hybrid-runbook-worker.md)
Poświadczenia AWS | Amazon usług sieci Web | [Uwierzytelnianie Runbooks z usługami sieci Web Amazon (AWS)](../automation/automation-sec-configure-aws-account.md)



