<properties
   pageTitle="Dodawanie runbooks Azure automatyzacji do planów odzyskiwania | Microsoft Azure"
   description="W tym artykule opisano, jak Odzyskiwanie witryny Azure umożliwia teraz rozszerzanie planów odzyskiwania wykonywanie złożonych zadań podczas odzyskiwania Azure przy użyciu automatyzacji Azure"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Dodawanie runbooks Azure automatyzacji do odzyskiwania planów


Ten przewodnik opisuje, jak Odzyskiwanie witryny Azure można zintegrować z automatyzacji Azure zapewnienie rozszerzeń do planów odzyskiwania. Plany odzyskiwania można dodać akompaniament odzyskiwania maszyn wirtualnych chronione za pomocą Odzyskiwanie witryny Azure zarówno replikacji w chmurze pomocniczej i replikacji Azure scenariuszy. Pomagają także dokonując odzyskiwania **spójne dokładności**, **powtarzalnych**i **Automatyczne**. Jeśli błędy nad maszyn wirtualnych Azure Integracja z automatyzacji Azure rozszerza planów odzyskiwania i udostępni Ci możliwość wykonywania runbooks, umożliwiając zaawansowane automatyzację zadań.

Jeśli masz nie wiesz o automatyzacji Azure jeszcze, Utwórz konto [w tym miejscu](https://azure.microsoft.com/services/automation/) i pobieranie ich przykładowe skrypty [tutaj](https://azure.microsoft.com/documentation/scripts/). Więcej informacji o [Odzyskiwanie witryny Azure](https://azure.microsoft.com/services/site-recovery/) oraz jak dodać akompaniament odzyskiwania Azure za pomocą planów odzyskiwania [tutaj](https://azure.microsoft.com/blog/?p=166264).

W tym samouczku firma Microsoft będzie wyglądać w sposób można zintegrować runbooks automatyzacji Azure planów odzyskiwania. Firma Microsoft będzie zautomatyzować prostych zadań, które wcześniej wymagane ręczne i zobacz, jak do konwertowania odzyskiwania kroku wielu elementów na akcję odzyskiwania jednym kliknięciem. Firma Microsoft będzie również Przyjrzyj się jak można rozwiązać prosty skrypt, jeśli ulegnie problem.

## <a name="customize-the-recovery-plan"></a>Dostosowywanie planu odzyskiwania

1. Pozwól nam rozpocząć operning karta zasobu planu odzyskiwania. Możesz sprawdzić, czy plan odzyskiwania zawiera dwa maszyn wirtualnych do niej dodać odzyskiwania. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Kliknij przycisk Dostosuj, aby rozpocząć dodawanie działań aranżacji. Spowoduje to otwarcie planu odzyskiwania Dostosowywanie karta.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Kliknij prawym przyciskiem myszy grupę, do rozpoczęcia 1 i zaznacz, aby dodać akcję wpis Dodaj.

4. Wybierz, aby wybrać skrypt w nowej karta.

5. Nadaj nazwę skryptu "Witaj świecie".

6. Wybierz nazwę konta automatyzacji. Jest to konto Azure automatyzacji. Zauważ, że to konto można w dowolnej Azure geograficznych, ale musi znajdować się w tej samej subskrypcji jako magazynu Odzyskiwanie witryny.

7. Wybierz pozycję działań aranżacji z konta automatyzacji. Jest to skrypt, który zostanie uruchomiony w trakcie realizacji planu odzyskiwania po odzyskiwania pierwszej grupy.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Wybierz przycisk OK, aby zapisać skrypt. Spowoduje to dodanie skrypt do grupy akcji wpisu z grupy 1: rozpoczynanie.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Istotne punkty: Dodawanie skryptu

1. Możesz skrypt kliknij prawym przyciskiem myszy i wybierz opcję "Usuń krok" lub "skrypt aktualizacji".

2. Skrypt można uruchamiać na Azure podczas pracy awaryjnej ze źródeł lokalnych Azure i można uruchamiać na Azure jako skrypt podstawowego po stronie przed zamknięciem systemu, podczas awarii z platformy Azure do lokalnego.

3. Po uruchomieniu skrypt będzie ją wprowadzić kontekście planu odzyskiwania.

Poniżej przedstawiono przykładowy wygląd zmiennej kontekstu.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Poniższa tabela zawiera nazwę i opis dla każdej zmiennej w kontekście.

**Nazwa zmiennej** | **Opis**
---|---
RecoveryPlanName | Nazwa planu uruchomione. Ułatwia wykonywanie akcji na podstawie nazwy przy użyciu samej skryptu
FailoverType | Określa, czy tym przełączeniu jest test, planowanego lub niezaplanowane.
FailoverDirection | Określ, czy odzyskiwania jest podstawowy i pomocniczy
Identyfikator grupy | Określ liczbę grupy w ramach planu odzyskiwania, gdy jest uruchomiony planu
VmMap | Tablica maszyn wirtualnych w grupie
Klucz VMMap | Unikatowy klucz (GUID) dla każdego maszyn wirtualnych. Jest taki sam, jak identyfikator VMM maszyny wirtualnej w stosownych przypadkach.
RoleName | Nazwa maszyn wirtualnych Azure, który jest ich odzyskania
CloudServiceName | W obszarze który maszyny wirtualnej jest tworzona Azure nazwa usługi w chmurze.
CloudServiceName (w modelu wdrożenia Menedżera zasobów) | Nazwa grupy zasobów Azure, pod którym utworzono maszyny wirtualnej.


## <a name="using-complex-variables-per-recovery-plan"></a>Przy użyciu zmiennych zespolonej na planu odzyskiwania

Czasami działań aranżacji wymaga więcej informacji niż tylko RecoveryPlanContext. Istnieje mechanizm pierwszej klasie w celu przekazania parametru do działań aranżacji. Jednak jeśli ma być używany ten sam skrypt za pośrednictwem wiele planów odzyskiwania możesz użyć zmiennej kontekstu planowanie odzyskiwania "RecoveryPlanName" i za pomocą poniżej badawczych techniki można użyć zmiennej zespolonej automatyzacji Azure w działań aranżacji. W poniższym przykładzie pokazano, jak można utworzyć trzy różne zmiennej zespolonej trwałe i używanie ich w działań aranżacji na podstawie nazwy planu odzyskiwania.

Należy rozważyć, czy chcesz użyć w działań aranżacji 3 dodatkowe parametry. Pozwól nam kodowanie je do formularza JSON {"Var1": "testautomation", "Var2": "Niezaplanowane", "Var3": "PrimaryToSecondary"}

Umożliwia utworzenie nowego środka automatyzacji [AA zespolonej zmiennej](../automation/automation-variables.md#variable-types Complex variable) .
Nazwa zmiennej jako <RecoveryPlanName>— parametry.
W tym miejscu odwołanie umożliwia tworzenie [zespolonej zmiennej](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

W przypadku planów odzyskiwania różne nazwy zmiennej jako

1. recoveryPlanName1 > — Parametry
2. recoveryPlanName2 > — Parametry
3. recoveryPlanName3 > — Parametry

Teraz skryptu odwołują się do parametry jako

1. Uzyskaj nazwę RP z $rpname = zmienna $Recoveryplancontext
2. Pobieranie zawartości $paramValue = "$($rpname) parametry"
3. Użyj jako zmiennej zespolonej planu odzyskiwania, dzwoniąc Get-AzureAutomationVariable [-AutomationAccountName] <String> -nazwa $paramValue.

Na przykład aby uzyskać zmiennej zespolonej/parametr planu odzyskiwania SharepointApp, Utwórz zmiennej zespolonej Azure automatyzacji o nazwie "SharepointApp parametry".

Za pomocą go w planie odzyskiwania wyodrębnianie zmiennej z zawartości za pomocą instrukcji Get-AzureAutomationVariable [-AutomationAccountName] <String> [-nazwa] $paramValue. [Odwoływać, aby uzyskać więcej informacji](https://msdn.microsoft.com/library/dn913772.aspx)

Ten sam skrypt sposób mogą być używane dla różnych odzyskiwania planu przez przechowywanie określoną zmienną zespolonej planu na składniki majątku.

## <a name="sample-scripts"></a>Przykładowe skrypty

Repozytorium skryptów, które można zaimportować bezpośrednio do Twojego konta automatyzacji zobacz [repozytorium usługi OMS Kristian języka skryptów](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Skrypt w tym miejscu jest Menedżer zasobów Azure szablonu, który będzie wdrażane są wszystkie poniżej skryptów

* NSG

Działań aranżacji NSG przydzieli publiczne adresy IP do każdej maszyn wirtualnych w planie odzyskiwania i dołączanie ich karty wirtualnej sieci do umożliwiająca komunikacji domyślne grupy zabezpieczeń sieci

* PublicIP

Publiczny adres IP działań aranżacji przypisze publiczne adresy IP do każdej maszyn wirtualnych w planie odzyskiwania. Uzyskiwanie dostępu do urządzeń i aplikacji zależy od ustawień zapory w każdym gościa


* CustomScript

Działań aranżacji CustomScript przydzieli publiczne adresy IP do każdej maszyn wirtualnych w planie odzyskiwania i zainstalować rozszerzenie skryptu niestandardowego, która wprowadzi skrypt, które odwołują się do podczas wdrażania szablonu

* NSGwithCustomScript

NSGwithCustomScript, które działań aranżacji przypisze publiczne adresy IP do każdej maszyn wirtualnych w odzyskiwania Plan, zainstaluj niestandardowy skrypt przy użyciu rozszerzenia i łączenie karty wirtualnej sieci NSG zezwalające na stosowanie domyślne przychodzących i wychodzących komunikacji dla dostępu zdalnego

## <a name="additional-resources"></a>Dodatkowe zasoby

[Usługa Azure automatyzacji uruchamiane jako konto](../automation/automation-sec-configure-azure-runas-account.md)

[Omówienie Azure automatyzacji] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Omówienie Azure automatyzacji")

[Przykładowe skrypty Azure automatyzacji] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Przykładowe skrypty Azure automatyzacji")
