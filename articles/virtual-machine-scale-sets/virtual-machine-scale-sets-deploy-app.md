<properties
    pageTitle="Wdrażanie aplikacji na zestawach skali maszyn wirtualnych | Microsoft Azure"
    description="Wdrażanie aplikacji na zestawach skali maszyn wirtualnych"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Wdrażanie aplikacji na zestawach skali maszyn wirtualnych

Uruchamiania aplikacji na Ustawianie skali maszyn wirtualnych jest zazwyczaj wdrożona w jeden z trzech sposobów:

- Instalowanie nowego oprogramowania na obrazie platformy w czasie rozmieszczania. Obraz platformy w tym kontekście jest obraz systemu operacyjnego z Azure Marketplace, takich jak Ubuntu 16.04, Windows Server 2012 R2 itp.

Możesz zainstalować nowe oprogramowanie na obrazie platformy [Rozszerzenia maszyn wirtualnych](../virtual-machines/virtual-machines-windows-extensions-features.md). Rozszerzenie maszyn wirtualnych to oprogramowanie jest uruchamiany po wdrożeniu maszyny. Możesz uruchomić dowolny kod, który chcesz w czasie rozmieszczania rozszerzenia skryptu niestandardowego. [Poniżej](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) przedstawiono przykładowy szablon Menedżera zasobów Azure z dwoma rozszerzeniami maszyn wirtualnych: Linux niestandardowy skrypt rozszerzenia do zainstalowania Apache i PHP i diagnostyczne rozszerzenia do emisji dane dotyczące wydajności używane przez Azure Autoscale.

Zaletą tej metody jest mają poziom odstęp między kodzie aplikacji i systemu operacyjnego i aplikacji można zachować osobno. Oczywiście, oznacza, że to również bardziej ruchomych elementów i czas wdrażania maszyn wirtualnych może być dłużej, jeśli istnieje wiele skrypt, aby pobrać i skonfigurować.

**W przypadku przekazania informacji poufnych polecenia niestandardowy skrypt rozszerzenie (na przykład hasła), należy określić `commandToExecute` w `protectedSettings` atrybut niestandardowy skrypt rozszerzenia zamiast `settings` atrybut.**

- Tworzenie niestandardowego obrazu maszyn wirtualnych zawierający zarówno system operacyjny, jak i aplikacji w jednym wirtualnego dysku twardego. W tym miejscu zestaw skali składa się z zestawu maszyny wirtualne kopiowane z obrazu utworzone przez użytkownika, które należy zachować. Ta metoda wymaga dodatkowych konfiguracji w czasie rozmieszczania maszyn wirtualnych. Jednak w `2016-03-30` wersji zestawy skali maszyn wirtualnych (i wcześniejszymi wersjami) dysków systemu operacyjnego maszyny wirtualne w zestawie skali są ograniczone do konta jednego miejsca do magazynowania. W związku z tym mogą zawierać maksymalnie 40 maszyny wirtualne w zestawie skali, zamiast 100 maszyn wirtualnych na skali Ustawianie limitu z obrazami platformy. Zobacz [Omówienie projektu Ustawianie skali](./virtual-machine-scale-sets-design-overview.md) uzyskać więcej szczegółowych informacji.

- Wdrażanie platformy lub obraz niestandardowy, który zasadniczo jest host kontenera i zainstalować aplikację jako jeden lub więcej kontenerów, które można zarządzać za pomocą narzędzia do zarządzania orchestrator lub konfiguracji. I zaletą tej metody jest mają pobieranej infrastruktury chmury z warstwy aplikacji, a następnie można zachować ich oddzielnie.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Co się dzieje, gdy Ustawianie skali maszyn wirtualnych skale się?

Po dodaniu co najmniej jeden maszyny wirtualne skali Ustaw, zwiększając możliwości — ręcznie lub za pośrednictwem autoscale — aplikacja zostanie automatycznie zainstalowana. Na przykład jeśli skonfigurowano skali ma rozszerzenia definicja, są uruchamiane w nowych maszyn wirtualnych każdym razem, gdy zostanie utworzona. Jeśli zestaw skala jest oparty na obraz niestandardowy, wszelkie nowe maszyn wirtualnych jest kopię obraz niestandardowy źródła. Ustaw odpowiednią skalę maszyny wirtualne są hosts kontenera, a następnie może być kod uruchamiania, aby załadować kontenerów rozszerzenia skrypt niestandardowy, czy rozszerzenie zainstalować agenta rejestruje orchestrator klaster (na przykład usługa kontenera Azure).

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Jak zarządzać aktualizacje aplikacji w zestawach skali maszyn wirtualnych?

Aktualizacje aplikacji w zestawach skali maszyn wirtualnych wykonaj trzy główne sposoby z trzech powyższej metody wdrażania aplikacji:

* Uaktualnianie rozszerzenia maszyn wirtualnych. Wszystkie rozszerzenia maszyn wirtualnych, które są definiowane dla zestawu maszyn wirtualnych skali są wykonywane każdym wdrożeniu nowych maszyn wirtualnych istniejących maszyn wirtualnych jest reimaged lub rozszerzenie maszyn wirtualnych jest aktualizowana. Jeśli musisz zaktualizować aplikację bezpośrednio aktualizowanie aplikacji do rozszerzenia jest podejście rentowny — po prostu zaktualizowania definicji rozszerzenia. Najprostszy sposób w tym celu jest przez zmianę fileUris, aby wskazywały nowego oprogramowania.

* Podejście niezmienne obraz niestandardowy. Gdy użytkownik wypieków (kwesta) aplikacji (lub części aplikacji) do obrazu maszyn wirtualnych możesz skoncentrować się na tworzenie potok zaufanego, aby zautomatyzować tworzenie, testowanie i wdrażania obrazów. Możesz projektować architektury ułatwiające szybkie zamiana zestaw etapowej skali do produkcji. Przykładem tej metody jest [sterownik Azure Spinnaker](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Pakującego i Terraform obsługują także Azure Menedżera zasobów, aby zdefiniować obrazów "jako kod" i je tworzyć w Azure, następnie za pomocą wirtualny dysk twardy w zestawie Skala. Jednak zrobić tak staje się problemów obrazy Marketplace, gdzie skryptów rozszerzenia i niestandardowe stają się ważniejszych od bitów z witryny Marketplace nie zmieniać bezpośrednio.

* Aktualizowanie kontenerów. Na przykład abstrakcyjne zarządzania cyklu życia aplikacji do poziomu powyżej infrastruktury chmury przez włókienka aplikacje i składniki aplikacji w kontenerach i zarządzanie tych kontenerów za pośrednictwem orchestrators kontenera i Menedżera konfiguracji, takie jak Kuchmistrz-Puppet.

Skala Ustaw maszyny wirtualne są zamieniane na stałe podłożem kontenerów i jest wymagane tylko rzadkie zabezpieczeń i aktualizacji związanych z systemem operacyjnym. Jak wspomniano, usługa Azure kontener jest dobrym przykładem korzystających z tej metody i tworzenie usługi wokół niego.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Jak wdrożeniem aktualizacji systemu operacyjnego domen aktualizacji?

Załóżmy, że chcesz zaktualizować obrazu systemu operacyjnego przy zachowaniu uruchomiony Ustawianie skali maszyn wirtualnych. Jednym ze sposobów jest, aby zaktualizować maszyn wirtualnych obrazów jeden maszyn wirtualnych jednocześnie. Aby to zrobić za pomocą programu PowerShell lub interfejsu wiersza polecenia Azure. Istnieją osobne polecenia, aby zaktualizować modelu Ustawianie skali maszyn wirtualnych (jak konfigurację jest definiowana) i problemu połączenia "ręcznego uaktualnienia" w poszczególnych maszyny wirtualne.

[Poniżej](https://github.com/gbowerman/vmsstools) przedstawiono przykładowy skrypt Python, który pozwala zautomatyzować proces aktualizacji domeny jedna aktualizacja Ustawianie skali maszyn wirtualnych jednocześnie. (Ostrzeżenie: więcej dowodu koncepcji niż zaostrzony rozwiązanie gotowe do użycia produkcyjnej jest — warto dodać kilka itp sprawdzania błędów.).
