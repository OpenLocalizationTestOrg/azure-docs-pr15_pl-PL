<properties
    pageTitle="Azure DevTest Labs — często zadawane pytania | Microsoft Azure"
    description="Odpowiedzi na typowe pytania Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs — często zadawane pytania

Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania dotyczące Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Ogólne
- [Co zrobić, jeśli Moja nie ma odpowiedzi na pytanie w tym miejscu?](#what-if-my-question-isnt-answered-here)
- [Dlaczego warto używać Azure DevTest Labs?](#why-should-i-use-azure-devtest-labs) 
- [Co oznacza "obaw, Samoobsługowe"?](#what-does-quotworry-free-self-servicequot-mean)
- [Jak używać Azure DevTest Labs?](#how-can-i-use-azure-devtest-labs) 
- [Jak mam wystawiona dla Azure DevTest Labs?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Zabezpieczenia 
- [Jakie są poziomy zabezpieczeń w laboratoriach DevTest Azure?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Jak utworzyć role, aby umożliwić użytkownikom wykonywanie określonych zadań?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>Integracja CI/dysk CD i automatyzacji 
 
- [Azure DevTest Labs Integracja z mojej toolchain CI/dysk CD?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Maszyn wirtualnych 
 
- [Dlaczego nie widzę niektórych maszyny wirtualne w karta maszyn wirtualnych Azure, wyświetlany w Azure DevTest Labs?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Jaka jest różnica między formułami a niestandardowych obrazów?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Jak utworzyć wiele maszyny wirtualne z tego samego szablonu jednocześnie?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Jak przenieść moje istniejące maszyny wirtualne Azure do mojej ćwiczenia Azure DevTest Labs?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Do mojego maszyny wirtualne można dołączyć wiele dysków?](#can-i-attach-multiple-disks-to-my-vms) 
- [Jak zautomatyzować proces przekazywania plików wirtualnego dysku twardego w celu utworzenia niestandardowych obrazów?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Jak można zautomatyzować proces usuwania wszystkich maszyny wirtualne w mojej ćwiczenia](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Artefakty 
 
- [Co to są artefakty?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Konfiguracja ćwiczenia 
 
- [Jak utworzyć ćwiczenia z szablonu Menedżera zasobów Azure?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Dlaczego moje maszyny wirtualne są tworzone w różnych grup zasobów z nazwami dowolnego? Można zmienić nazwę lub modyfikowanie grupy zasobów?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Ile labs można utworzyć w obszarze tej samej subskrypcji?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Ile maszyny wirtualne można utworzyć na ćwiczenia?](#how-many-vms-can-i-create-per-lab)
- [Jak udostępniać bezpośredni link do mojej ćwiczenia?](#how-do-i-share-a-direct-link-to-my-lab)
- [Co to jest konto Microsoft?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Rozwiązywanie problemów 
 
- [Moje artefaktu nie powiodła się podczas tworzenia maszyn wirtualnych. Jak rozwiązać go](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Dlaczego nie jest Mój istniejącą wirtualna sieć zapisywania poprawnie?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Co zrobić, jeśli Moja nie ma odpowiedzi na pytanie w tym miejscu?
Jeśli na liście nie ma swoje pytanie, powiedz nam, a pomożemy Ci odpowiedzi.

- Zadaj pytanie w [wątku Disqus](#comments) na końcu często zadawanych Pytaniach i współpracować z pamięci podręcznej Azure zespołu i inni członkowie społeczności dotyczący tego artykułu.
- Aby uzyskać dostęp do większej liczby osób, Zadaj pytanie na [forum Azure DevTest Labs MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)i współpracować z Azure DevTest Labs zespołu i inni członkowie społeczności.
- Aby wprowadzić żądanie funkcji, przesłać żądania i pomysły [Azure DevTest Labs użytkownika głosu](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Dlaczego warto używać Azure DevTest Labs? 
Azure DevTest Labs można zapisać zespołu czasu i środków. Deweloperów można tworzyć własne środowiska za pomocą kilku różnych podstaw i szybko wdrażania i konfigurowania aplikacji przy użyciu artefakty. Za pomocą niestandardowych obrazów i formuł, maszyn wirtualnych można zapisać jako szablonów i łatwo odtworzyć. Ponadto labs oferuje kilka zasad można konfigurować, umożliwiające administratorom ćwiczenia zmniejszyć odpadów i zarządzanie nimi środowiskach zespołu. Te zasady obejmują automatyczne zamknięcia, próg koszt maksymalna maszyny wirtualne na użytkownika i maksymalne rozmiary maszyn wirtualnych. Aby uzyskać więcej informacji na temat wyjaśnienie Azure DevTest Labs zapoznanie się z [omówieniem](devtest-lab-overview.md) lub zapoznaj się z [klipy wideo](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Co oznacza "obaw, Samoobsługowe"?
"Samodzielnego obaw," oznacza, że deweloperów i testerów tworzyć własne środowiska stosownie do potrzeb, a administratorzy mają bezpieczeństwo i pewność, że Azure DevTest Labs ułatwia minimalizowanie odpady i kontrolować koszt. Administratorzy mogą określać rozmiarów maszyn wirtualnych, które są dozwolone, maksymalna liczba maszyny wirtualne, a kiedy pracę i zamknij maszyny wirtualne. Azure DevTest Labs również ułatwia monitorowanie kosztów i ustawianie alertów śledzić sposobu użycia zasobów w ćwiczenia. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Jak używać Azure DevTest Labs? 
Azure DevTest Labs jest przydatna w dowolnym momencie wymagają standardowego lub Testuj środowiskach i chcesz odtworzyć szybkie i/lub zarządzać nimi przy użyciu oszczędność zasady. 

Oto kilka scenariuszy, które klientów za pomocą Azure DevTest Labs za: 

- Zarządzanie środowiskach deweloperów i testowych w jednym miejscu, wykorzystując zasady, aby zmniejszyć koszty i niestandardowych obrazów do udostępnienia tworzy przez zespół.
- Projektowanie przy użyciu niestandardowych obrazów do zapisywania stanu dysku etapach opracowywania aplikacji.
- Śledzenie kosztów względem postępu. 
- Tworzenie środowisk testowych masy badań assurance jakości.
- Za pomocą artefakty i formuł, można łatwo skonfigurować i odtworzyć aplikacji w różnych środowiskach. 
- Rozpowszechnianie maszyny wirtualne dla hackathons (test lub deweloperów współpracy), a następnie łatwo Anuluj inicjowania obsługi administracyjnej ich po zakończeniu wydarzenia. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Jak mam wystawiona dla Azure DevTest Labs? 
Azure DevTest Labs to bezpłatna usługa, co oznacza, że tworzenie labs i konfigurowanie zasad, szablony i artefaktów jest wolne. Płatność tylko w przypadku Azure zasoby używane w Twojej labs, takich jak maszyn wirtualnych, kont miejsca do magazynowania i wirtualnych sieci. Aby uzyskać więcej informacji na koszty zasobów ćwiczenia przeczytaj o [Azure DevTest Labs ceny](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Jakie są poziomy zabezpieczeń w laboratoriach DevTest Azure?  
Zabezpieczenia programu access jest określona przez [Azure Role-Based kontroli dostępu (RBAC)](../active-directory/role-based-access-built-in-roles.md). Aby dowiedzieć się, jak działa dostęp, ułatwia opis różnic między uprawnień, roli i zakresu zdefiniowane przez RBAC.

- **Uprawnień** — uprawnienia są zdefiniowane dostęp do określonej akcji. Na przykład uprawnień może być dostęp do odczytu do wszystkich wirtualnych komputerów. 
- **Rola** - roli to zestaw uprawnień, które można grupować i przypisana do użytkownika. Na przykład "właściciel subskrypcji" ma dostęp do wszystkich zasobów w ramach subskrypcji. 
- **Zakres** — zakres jest poziom w hierarchii Azure zasobów. Na przykład zakres może być grupa zasobów lub pojedynczy ćwiczenia lub całej subskrypcji. 
 
W zakresie Azure DevTest Labs, dostępne są dwa typy ról w celu zdefiniowania uprawnień użytkownika: ćwiczenia właściciela i ćwiczenia użytkownika.

- **Właściciel ćwiczenia** - właściciel ćwiczenia ma dostęp do dowolnych zasobów w ćwiczenia. W związku z tym mogą modyfikować zasady, czytanie i pisanie wszelkie maszyny wirtualne, zmienianie wirtualnej sieci i itd. 
- **Ćwiczenia użytkownika** - użytkownika ćwiczenia można wyświetlić wszystkie zasoby ćwiczenia, takich jak maszyny wirtualne, zasady i wirtualnych sieci, ale nie mogą modyfikować zasady lub dowolny maszyny wirtualne utworzone przez innych użytkowników. Istnieje także możliwość tworzenia niestandardowych ról w Azure DevTest Labs i możesz dowiedzieć się, jak zrobić w artykule [Udziel uprawnień użytkownika do określonych ćwiczenia zasad](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Ponieważ zakresy hierarchicznych, gdy użytkownik ma uprawnienia w określonego zakresu, automatycznie otrzymują uprawnienia w każdym zakresie niższego poziomu kategorii. Na przykład jeśli użytkownik jest przypisany do roli właścicielem subskrypcji, następnie mają dostęp do wszystkich zasobów w subskrypcji. Do tych zasobów należą wszystkich maszyn wirtualnych, wszystkie wirtualnych sieci i wszystkie laboratoria. W związku z tym właścicielem subskrypcji automatycznie dziedziczy roli właściciela ćwiczenia. Przeciwieństwem jest PRAWDA. Tylko właściciel ćwiczenia ma dostęp do ćwiczenia, czyli zakres niższym niż poziom subskrypcji. W związku z tym właściciel ćwiczenia nie jest widoczny w środowisku maszyn wirtualnych systemu lub wirtualnych sieci lub wszystkie zasoby, które są poza ćwiczenia. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Jak utworzyć rolę, aby umożliwić użytkownikom wykonywanie określonych zadań?
Pełna artykuł na temat sposobu tworzenia niestandardowych ról i przypisać uprawnienia do tej roli można znaleźć tutaj. Oto przykładowy skrypt, który tworzy roli "DevTest Labs zaawansowane użytkownik", który ma uprawnienia do rozpoczynania i kończenia wszystkie maszyny wirtualne w ćwiczenia:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Azure DevTest Labs Integracja z mojej toolchain CI/dysk CD? 
Jeśli używasz programu VSTS jest [rozszerzenie Azure DevTest Labs zadania](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , które umożliwia automatyzację planowaną wersji w laboratoriach DevTest Azure. Zastosowanie tego numeru wewnętrznego, należą:

- Tworzenie i wdrażanie maszyny automatycznie i konfigurowanie go za pomocą najnowszej kompilacji za pomocą kopiowania plików Azure lub programu PowerShell VSTS zadań. 
- Po przetestowaniu w celu odtworzenia błędu na tym samym maszyn wirtualnych w celu dalszego zbadania automatycznie przechwytuje stan maszyny. 
- Usuwanie maszyn wirtualnych na końcu potoku wersji, gdy nie jest już potrzebna. 

Następujące wpisów w blogu zawiera wskazówki i informacji na temat korzystania z rozszerzeniem programu VSTS:
 
- [Azure Labs DevTest — rozszerzenie programu VSTS](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Wdrażanie nowych maszyn wirtualnych w istniejących AzureDevTestLab z programu VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Za pomocą programu VSTS wersji zarządzania wdrożeniach ciągły do AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Dla innych toolchains CI-CD wszystkich opisanych powyżej scenariuszy, które można osiągnąć poprzez rozszerzenie zadania programu VSTS może się podobnie, stosując wdrażanie [szablonów Menedżera zasobów Azure](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) za pomocą [poleceń cmdlet środowiska PowerShell Azure](../resource-group-template-deploy.md) i [SDK .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Za pomocą [Pozostałych interfejsy API dla DevTest Labs](http://aka.ms/dtlrestapis) integracji z usługi toolchain.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Dlaczego nie widzę niektórych maszyny wirtualne w karta maszyn wirtualnych Azure, wyświetlany w Azure DevTest Labs?
Po utworzeniu maszyny w laboratoriach DevTest Azure uprawnień otrzymuje dostęp do tego maszyn wirtualnych. Jesteś możliwość wyświetlania go karta labs i karta **maszyn wirtualnych** . Użytkownicy w roli DevTest Labs mogą wyświetlać wszystkie maszyn wirtualnych utworzonych w ćwiczenia za pomocą karta **wszystkich maszyn wirtualnych** ćwiczenia. Jednak użytkownicy w roli DevTest Labs nie udzielono automatycznie dostęp do odczytu do zasobów maszyn wirtualnych, tworzone przez inne osoby. W związku z tym te maszyny wirtualne nie są wyświetlane w **środowisku maszyn wirtualnych systemu** karta. 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Jaka jest różnica między formułami a niestandardowych obrazów? 
Obraz niestandardowy jest wirtualnego dysku twardego (wirtualnego dysku twardego), natomiast formuła jest obraz, który można skonfigurować dodatkowe ustawienia, które można zapisać i odtworzyć. Obraz niestandardowy mogą być lepiej, jeśli chcesz szybko utworzyć kilka środowiskach z tego samego obrazu podstawowych, niezmienne. Formuła może być lepiej, jeśli chcesz odtworzyć konfigurację usługi maszyn wirtualnych z najnowszych bitów, virtual podsieci lub określonego rozmiaru. Aby uzyskać więcej informacji na temat wyjaśnienie zapoznaj się z artykułem [Opis niestandardowych obrazów i formuł w laboratoriach DevTest](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Jak utworzyć wiele maszyny wirtualne z tego samego szablonu jednocześnie? 
[Rozszerzenie programu VSTS zadania](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) lub [wygenerować szablonu Menedżera zasobów Azure](devtest-lab-add-vm-with-artifacts.md#save-arm-template) można używać podczas tworzenia maszyn wirtualnych i [Wdrażanie szablonu Menedżera zasobów Azure z programu Windows PowerShell](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Jak przenieść moje istniejące maszyny wirtualne Azure do mojej ćwiczenia Azure DevTest Labs? 
Firma Microsoft projektuje rozwiązanie bezpośrednio przenieść maszyny wirtualne Azure DevTest Labs, ale obecnie można kopiować do istniejącego maszyny wirtualne do Azure DevTest Labs w następujący sposób: 

1. Kopiowanie pliku wirtualnego dysku twardego z istniejących maszyn wirtualnych za pomocą tego [skryptu programu Windows PowerShell](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [Tworzenie niestandardowego obrazu](devtest-lab-create-template.md) wewnątrz usługi Azure DevTest Labs ćwiczenia. 
1. Tworzenie maszyn wirtualnych w ćwiczenia z obraz niestandardowy 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Do mojego maszyny wirtualne można dołączyć wiele dysków? 
Dołączanie wielu dysków do maszyny wirtualne jest obsługiwana.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Jak zautomatyzować proces przekazywania plików wirtualnego dysku twardego w celu utworzenia niestandardowych obrazów? 
Dostępne są dwie opcje:

- Kopiowanie lub przekazywanie plików wirtualnego dysku twardego do miejsca do magazynowania konta skojarzonego z ćwiczenia umożliwia [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) .
- [Eksplorator magazynu usługi Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) to aplikacja autonomicznej z systemem Windows, OSX i Linux.   
 
Aby znaleźć konta miejsca do magazynowania docelowego skojarzonego z Twojej ćwiczenia, wykonaj następujące czynności:

1. Zaloguj się do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Wybierz pozycję **Grupy zasobów** z poziomu panelu po lewej stronie. 
1. Znajdź i zaznacz grupa zasobów, skojarzony z ćwiczenia. 
1. Na karta **Przegląd** wybierz jedno z kont miejsca do magazynowania. 
1. Zaznaczanie **obiektów blob**.
1. Poszukaj przekazywania na liście. Jeśli nie istnieje, powróć do kroku nr 4 i spróbuj innego konta miejsca do magazynowania.
1. Użyj **adresu URL** jako miejsce docelowe kopii polecenia AzCopy.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Jak można zautomatyzować proces usuwania wszystkich maszyny wirtualne w mojej ćwiczenia

Oprócz usuwania maszyny wirtualne z Twojej ćwiczenia w portalu Azure, możesz usunąć wszystkie maszyny wirtualne w swojej ćwiczenia za pomocą skryptu programu PowerShell. W poniższym przykładzie po prostu zmodyfikuj wartości parametru pod komentarzem **wartości w celu zmiany** . Można podjąć `subscriptionId`, `labResourceGroup`, i `labName` wartości z karta ćwiczenia w portalu Azure. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Co to są artefakty? 
Artefakty są dostosowania elementów, które mogą być używane do wdrożenia usługi najnowszą bitów lub narzędziami deweloperów na maszyny. Są dołączone do maszyn wirtualnych podczas tworzenia za pomocą kilku kliknięć proste, a po maszyn wirtualnych jest obsługi administracyjnej, artefaktów wdrażanie i konfigurowanie usługi maszyn wirtualnych. Istnieją różne artefakty istniejącego w naszym [publicznej repozytorium Github](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), ale możesz również łatwo [tworzyć własne artefakty](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Jak utworzyć ćwiczenia z szablonu Menedżera zasobów Azure? 
Udostępniono można wdrażać jako [repozytorium Github szablonów Menedżera zasobów Azure ćwiczenia](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) - lub zmodyfikować, aby utworzyć niestandardowe szablony dla swojego labs. Każda z tych szablonów zawiera łącze, które można kliknąć, aby wdrożyć ćwiczenia jako-znajduje się w obszarze własne Azure subskrypcji lub dostosować szablon i [Wdrażanie przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Dlaczego moje maszyny wirtualne są tworzone w różnych grup zasobów z nazwami dowolnego? Można zmienić nazwę lub modyfikowanie grupy zasobów? 
Grup zasobów są tworzone w ten sposób, aby Azure DevTest Labs radzić uprawnienia i dostęp do maszyn wirtualnych. Gdy maszyn wirtualnych można przenieść do innej grupy zasobów z nazwą odpowiedniej, zrobić nie jest to zalecane. Pracujemy obecnie nad poprawy tego środowiska umożliwia uzyskania większej elastyczności.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Ile labs można utworzyć w obszarze tej samej subskrypcji? 
Istnieje nie określonego ograniczenia liczby labs, które można tworzyć na subskrypcję. Jednak zasoby używane są ograniczone na subskrypcję. Więcej informacji o [limity i przydziałami nałożone na Azure subskrypcje](../azure-subscription-service-limits.md) oraz [jak zwiększyć te limity](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Ile maszyny wirtualne można utworzyć na ćwiczenia? 
Nie ma żadnych określonego ograniczenia liczby maszyny wirtualne, które można tworzyć na ćwiczenia. Jednak obecnie ćwiczenia obsługuje tylko około 40 maszyny wirtualne uruchomiony w tym samym czasie w magazynie standardowy i 25 maszyny wirtualne równoczesne działanie w magazynie premium. Pracujemy obecnie nad na zwiększenie te limity. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Jak udostępniać bezpośredni link do mojej ćwiczenia?

Aby udostępnić łącze do użytkowników ćwiczenia można wykonać poniższą procedurę.

1. Przejdź do ćwiczenia w portalu Azure.
2. Skopiuj adres URL ćwiczenia z poziomu przeglądarki i udostępnij go innym użytkownikom ćwiczenia. 

>[AZURE.NOTE] Jeśli użytkownicy ćwiczenia są użytkowników zewnętrznych z [MSA konta](#what-is-a-microsoft-account) , a nie należą do usługi Active directory firmy, ich może zostać wyświetlony komunikat o błędzie podczas nawigowania do podanego łącza. Jeśli otrzymają komunikat o błędzie, poinstruuj ich, kliknij jej nazwę w prawym górnym rogu Azure portal i wybierz katalog, w którym istnieje ćwiczenia z **katalogu** części menu.

### <a name="what-is-a-microsoft-account"></a>Co to jest konto Microsoft?

Konto Microsoft jest dostępne dla niemal wszystko, co zrobić z urządzeń firmy Microsoft i usług. Jest to adres e-mail i hasło, którego używasz, aby zalogować się do programu Skype, Outlook.com, OneDrive, Windows Phone i Xbox LIVE — i oznacza to, że plików, zdjęć, kontaktów i ustawień może Cię obserwują na dowolnym urządzeniu. 

>[AZURE.NOTE] Konto Microsoft umożliwia się o nazwie "Windows Live ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Moje artefaktu nie powiodła się podczas tworzenia maszyn wirtualnych. Jak rozwiązać go 
Aby dowiedzieć się, jak uzyskać dzienniki dotyczące usługi nie powiodło się artefaktu odwołują się do wpisu w blogu [Jak rozwiązywać problemy z kończy się niepowodzeniem artefaktów w AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - napisane przez jeden z naszych specjaliści MVP w dziedzinie. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Dlaczego nie jest Mój istniejącą wirtualna sieć zapisywania poprawnie?  
Jedną z możliwości to, że nazwy wirtualną sieć zawiera kropki. Jeśli tak, spróbuj Usuwanie okresów lub zastępowanie łączniki, a następnie spróbuj ponownie zapisać wirtualnej sieci.
