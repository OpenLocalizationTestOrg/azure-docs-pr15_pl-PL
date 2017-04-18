<properties
   pageTitle="Wdrażanie klastrze Usługa Azure kontenera | Microsoft Azure"
   description="Wdrażanie klastrze Usługa kontenera Azure za pomocą portalu Azure, polecenie Azure lub programu PowerShell."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Wdrażanie klastrze Usługa Azure kontenera

Azure kontenera Usługa udostępnia szybkie wdrażanie popularne kontenera Otwórz źródło klaster i aranżacji rozwiązań. Za pomocą usługi kontenera Azure, należy wdrożyć kontrolera domeny/systemu operacyjnego i Docker Swarm klastrów z szablonami Menedżera zasobów Azure lub Azure portal. Wdrażanie tych klastrów przy użyciu zestawów skali maszyn wirtualnych Azure, a klastrów skorzystać Azure ofert sieci i miejsca do magazynowania. Aby uzyskać dostęp do usługi kontenera Azure, potrzebujesz subskrypcji usługi Azure. Jeśli nie masz, następnie możesz zalogować do [bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Ten dokument przeprowadzi Cię przez wdrażanie klastrze Usługa kontenera Azure za pomocą [Azure portal](#creating-a-service-using-the-azure-portal), [Azure interfejs wiersza polecenia (polecenie)](#creating-a-service-using-the-azure-cli)i [modułu programu PowerShell usługi Azure](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Tworzenie usługi przy użyciu Azure portal

Zaloguj się do portalu Azure, wybierz pozycję **Nowy**i wyszukaj **Azure kontenera usługi**Azure Marketplace.

![Tworzenie wdrożenia 1](media/acs-portal1.png)  <br />

Wybierz **Usługę Azure kontenera**, a następnie kliknij przycisk **Utwórz**.

![Tworzenie wdrożenia 2](media/acs-portal2.png)  <br />

Wprowadź następujące informacje:

- **Nazwa użytkownika**: jest to nazwa użytkownika użyta dla konta na każdym maszyn wirtualnych i skali maszyn wirtualnych zestawy w klastrze Usługa Azure kontenera.
- **Subskrypcja**: Wybierz subskrypcję usługi Azure.
- **Grupa zasobów**: zaznacz istniejącej grupy zasobów lub Utwórz nowy.
- **Lokalizacja**: Wybierz Azure region do wdrożenia usługi kontenera Azure.
- **Klucz publiczny SSH**: Dodaj klucz publiczny będzie używany do uwierzytelniania maszyn wirtualnych usługi kontenera Azure. Jest ważne, że ten klucz zawiera nie podziały wierszy i zawiera prefiks "ssh-rsa" oraz 'username@domain' przyrostkowe. Powinna przypominać tabelę następujące czynności: **AAAAB3Nz ssh-rsa... <>...... UcyupgH azureuser@linuxvm **. Aby uzyskać instrukcje dotyczące tworzenia kluczy Secure Shell (SSH) zobacz artykuły [systemu Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) i [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) .

Gdy wszystko będzie już gotowe kontynuować, kliknij przycisk **OK** .

![Tworzenie wdrożenia 3](media/acs-portal3.png)  <br />

Wybierz typ aranżacji. Dostępne są następujące opcje:

- **Kontrolera domeny/systemu operacyjnego**: wdrożenie klastrze kontrolera domeny/systemu operacyjnego.
- **Swarm**: wdrożenie klastrze Docker Swarm.

Gdy wszystko będzie już gotowe kontynuować, kliknij przycisk **OK** .

![Tworzenie wdrożenia 4](media/acs-portal4.png)  <br />

Wprowadź następujące informacje:

- **Zliczanie wzorzec**: liczby wzorców w klastrze.
- **Liczba agenta**: dla Docker Swarm, będzie numer początkowy czynników w zestawie skali agenta. Kontrolera domeny i OS będzie numer początkowy czynników w zestawie skali prywatne. Ponadto zestaw publicznej skala jest tworzony, który zawiera wstępnie określoną liczbę agentów. W tym zestawie skali publicznej czynników jest określana przez liczbę wzorców zostały utworzone w klastrze — jednego publicznej agenta dla jednego wzorca i dwóch publicznej czynników dla trzech do pięciu wzorców.
- **Agent maszyn wirtualnych rozmiar**: rozmiar maszyn wirtualnych agenta.
- **Prefiks DNS**: świata unikatową nazwę, która będzie używana do prefiks kluczowych fragmentów pełni kwalifikowanych nazw domen w usłudze.

Gdy wszystko będzie już gotowe kontynuować, kliknij przycisk **OK** .

![Tworzenie wdrożenia 5](media/acs-portal5.png)  <br />

Po zakończeniu sprawdzania poprawności usługi, kliknij przycisk **OK** .

![Tworzenie wdrożenia 6](media/acs-portal6.png)  <br />

Kliknij przycisk **Utwórz** , aby rozpocząć proces wdrożenia.

![Tworzenie wdrożenia 7](media/acs-portal7.png)  <br />

Jeśli został wybrany przypinanie wdrożenia Portal Azure, zobaczysz stanu rozmieszczania.

![Tworzenie wdrożenia 8](media/acs-portal8.png)  <br />

Po zakończeniu rozmieszczania klaster usługa Azure kontener jest gotowa do użycia.

## <a name="create-a-service-by-using-the-azure-cli"></a>Tworzenie usługi przy użyciu interfejsu wiersza polecenia Azure

Aby utworzyć wystąpienie usługi kontenera Azure za pomocą wiersza polecenia, należy Azure subskrypcji. Jeśli nie masz, następnie możesz zalogować do [bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Można również muszą być [zainstalowaniu](../xplat-cli-install.md) i [skonfigurowaniu](../xplat-cli-connect.md) polecenie Azure.

Aby wdrożyć kontrolera domeny i OS lub Docker Swarm klaster, wybierz jedną z następujących szablonów z GitHub. Należy zauważyć, że oba te szablony są takie same, z wyjątkiem ustawieniem domyślnym orchestrator.

* [Szablon kontrolera domeny/systemu operacyjnego](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Szablon rój](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Następnie upewnij się, że polecenie Azure został połączony z subskrypcji usługi Azure. Możesz to zrobić przy użyciu następującego polecenia:

```bash
azure account show
```
Jeśli konto Azure nie jest zwracana, za pomocą następującego polecenia się zalogować polecenie Azure.

```bash
azure login -u user@domain.com
```

Następnie skonfiguruj narzędzia polecenie Azure za pomocą Menedżera zasobów Azure.

```bash
azure config mode arm
```

Tworzenie grupy zasobów Azure i klaster kontenera usługi przy użyciu następującego polecenia, gdzie:

- **RESOURCE_GROUP** to nazwa grupy zasobów, który ma być używany dla tej usługi.
- **Lokalizacja** jest Azure region, gdzie utworzone grupy zasobów i wdrażanie usługi kontenera Azure.
- **TEMPLATE_URI** jest lokalizacja pliku wdrożenia. Należy zauważyć, że musi to być plik nieprzetworzonych nie wskaźnik GitHub interfejsu użytkownika. Aby znaleźć ten adres URL, wybierz plik azuredeploy.json w GitHub, a następnie kliknij przycisk **Raw** .

> [AZURE.NOTE] Po uruchomieniu tego polecenia, powłoka wyświetli monit o wartości parametrów wdrożenia.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Podaj parametry szablonu

Ta wersja polecenia wymaga interakcyjne Definiowanie parametrów. Jeśli chcesz podać parametry, takie jak ciągu sformatowane JSON, możesz to zrobić przy użyciu `-p` przełączanie. Na przykład:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Można też kliknąć, można udostępnić plik JSON sformatowanych przy użyciu parametrów przy użyciu `-e` przełączyć:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Aby wyświetlić parametry przykładowy plik o nazwie `azuredeploy.parameters.json`, poszukaj dzięki szablonom usługa Azure kontener programu GitHub.

## <a name="create-a-service-by-using-powershell"></a>Tworzenie usługi przy użyciu programu PowerShell

Można także wdrożyć klastrze Usługa Azure kontenera przy użyciu programu PowerShell. Ten dokument jest oparty na wersji 1.0 [modułu programu PowerShell usługi Azure](https://azure.microsoft.com/blog/azps-1-0/).

Aby wdrożyć kontrolera domeny i OS lub Docker Swarm klaster, wybierz jedną z następujących szablonów. Należy zauważyć, że oba te szablony są takie same, z wyjątkiem ustawieniem domyślnym orchestrator.

* [Szablon kontrolera domeny/systemu operacyjnego](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Szablon rój](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Przed utworzeniem klaster w ramach subskrypcji Azure, upewnij się, że sesji programu PowerShell został podpisany Azure. W tym z `Get-AzureRMSubscription` polecenie:

```powershell
Get-AzureRmSubscription
```

Jeśli musisz zalogować się do Azure za pomocą `Login-AzureRMAccount` polecenie:

```powershell
Login-AzureRmAccount
```

Jeśli użytkownik jest instalowany do nowej grupy zasobów, należy najpierw utworzyć grupa zasobów. Aby utworzyć nową grupę zasobów, użyj `New-AzureRmResourceGroup` polecenie i określ region zasobów grupie nazwę i miejsce docelowe:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Po utworzeniu grupy zasobów, możesz utworzyć klaster przy użyciu następującego polecenia. Identyfikator URI odpowiedni szablon zostanie podana `-TemplateUri` parametru. Po uruchomieniu tego polecenia programu PowerShell wyświetli monit o wartości parametrów wdrożenia.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Podaj parametry szablonu

Użytkownicy zaznajomieni z programem PowerShell wiadomo, że można przechodzić między parametrów dostępnych dla polecenia cmdlet, wpisując znak minus (-), a następnie naciśnięcie klawisza TAB. To samo ma zastosowanie również z parametrami, które zostały zdefiniowane w szablonie. Po zainicjowaniu wpisz nazwę szablonu, polecenia cmdlet pobiera szablon, analizuje parametry i dynamicznie dodaje parametrów szablonu do polecenia. Dzięki temu bardzo łatwo, określ wartości parametrów szablonu. A Jeśli zapomnisz wartości parametru wymagane programu PowerShell zostanie wyświetlony monit dla wartości.

Poniżej przedstawiono pełną polecenie z parametrów dostępnych. Można podać własne wartości nazwy zasobów.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz klastrze działa, zobacz te dokumenty połączenia i szczegóły zarządzania:

- [Nawiązywanie połączenia z klastrem usługa Azure kontenera](container-service-connect.md)
- [Praca z usługi Azure kontenera i kontrolera domeny/systemu operacyjnego](container-service-mesos-marathon-rest.md)
- [Praca z usługi Azure kontenera i rój Docker](container-service-docker-swarm.md)
