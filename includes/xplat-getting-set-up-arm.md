<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Za pomocą interfejsu wiersza polecenia Azure z Azure Menedżera zasobów (ARM)

Zanim będzie można używać polecenie Azure z poleceniami Menedżera zasobów i szablony do wdrożenia Azure zasobów i obciążenia przy użyciu grup zasobów, konieczne będzie konto z Azure (oczywiście). Jeśli nie masz konta, możesz uzyskać [bezpłatnej wersji próbnej platformy Azure tutaj](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Jeśli nie masz jeszcze konta usługi Azure, ale masz subskrypcję usługi subskrypcji MSDN, możesz pobrać bezpłatną Azure środków aktywując usługi [tutaj korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) — lub korzystając z bezpłatnego konta. Albo będzie działać dostępu Azure.

### <a name="step-1-verify-the-azure-cli-version"></a>Krok 1: Sprawdzić wersję polecenie Azure

Aby użyć polecenie Azure konieczne poleceń i szablony ARM, należy mieć co najmniej wersji 0.8.17. Aby sprawdzić swoją wersję, wpisz `azure --version`. Powinien zostać wyświetlony mniej więcej tak:

    $ azure --version
    0.8.17 (node: 0.10.25)

Jeśli musisz zaktualizować wersję polecenie Azure, zobacz [Polecenie Azure](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Krok 2: Weryfikowanie używają służbowe lub szkolne tożsamości z platformy Azure

Tryb poleceń ARM można używać tylko, jeśli korzystasz z [usługi Azure Active Directory dzierżawy](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) lub [Głównej nazwy usługi](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Te są także nazywane *identyfikatory organizacji*).

Aby sprawdzić, czy masz jedną, zaloguj się, wpisując `azure login` i używanie pracy lub szkole nazwy użytkownika i hasła po wyświetleniu monitu. Jeśli masz, zobacz podobną do następujących czynności:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Jeśli nie widzisz tego, musisz utworzyć nowej dzierżawy (lub głównej usługi) przy użyciu tożsamości usługi konta Microsoft. (Jest to często w przypadku osobistych subskrypcje MSDN lub bezpłatnej subskrypcji próbnej.) Aby utworzyć służbowe lub szkolne identyfikator z konta usługi Azure utworzone za pomocą identyfikatora Microsoft, zobacz [skojarzyć katalogiem Azure AD przy użyciu nowej subskrypcji Azure](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Jeśli uważasz, że identyfikator organizacji należy już masz, może być konieczne skontaktować się z osobą, która utworzyła konto usługi.

### <a name="step-3-choose-your-azure-subscription"></a>Krok 3: Wybierz subskrypcję Azure

Jeśli masz tylko jedną subskrypcję na koncie Azure, polecenie Azure domyślnie kojarzy się z tej subskrypcji. Jeśli masz więcej niż jedną subskrypcję, musisz Wybierz subskrypcję, do którego chcesz użyć, wpisując `azure account set <subscription id or name> true` miejsce, w którym _identyfikator subskrypcji lub nazwa_ jest identyfikator subskrypcji lub nazwę subskrypcji, którą chcesz pracować w bieżącej sesji.

Powinien zostać wyświetlony podobną do następujących czynności:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Krok 4: Uruchom polecenie usługi Azure w trybie ARM

Aby użyć trybu Azure zasobów zarządzania (ARM) z polecenie Azure, wpisz `azure config mode arm`. Powinien zostać wyświetlony podobną do następujących czynności:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Można przełączyć się ponownie wpisywać za pomocą poleceń zarządzania usługi Azure `azure config mode asm`.
