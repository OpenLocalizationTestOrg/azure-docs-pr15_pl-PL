<properties
   pageTitle="MATLAB klastrów maszyn wirtualnych | Microsoft Azure"
   description="Umożliwia tworzenie klastrów MATLAB Distributed Computing serwerów do uruchamiania usługi obciążenia MATLAB równoległe obliczeniowych w środowisku maszyn wirtualnych systemu Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Tworzenie klastrów serwerów Distributed środowiska MATLAB na maszyny wirtualne Azure 

Należy utworzyć jeden lub kilka klastrów MATLAB Distributed Computing Server do uruchamiania usługi obciążenia MATLAB równoległe obliczeniowych w środowisku maszyn wirtualnych systemu Microsoft Azure. Zainstalować oprogramowanie serwera Distributed środowiska MATLAB na maszyn wirtualnych podstawowego obrazu oraz wdrażanie i Zarządzanie klastrem za pomocą szablonu Azure Szybki Start lub skrypt programu PowerShell Azure (dostępne na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)). Po wdrożeniu Połącz się z klastrem do uruchamiania usługi obciążenia. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Informacje o MATLAB i MATLAB Distributed przetwarzania danych serwera 

Platformy [MATLAB](http://www.mathworks.com/products/matlab/) jest zoptymalizowana pod kątem rozwiązywania problemów technicznych i naukowych. Użytkownikom MATLAB symulacji na dużą skalę i zadania przetwarzania danych może być przyspieszyć ich obciążeń obliczeniowych, wykorzystując klastrów obliczeń i usług siatki równolegle MathWorks przetwarzania danych produktów. [Równoległe Przybornik obliczania](http://www.mathworks.com/products/parallel-computing/) umożliwia użytkownikom MATLAB parallelize aplikacji i korzystać z wielu procesorów, GPU, obliczyć klastrów. [Serwer Distributed środowiska MATLAB](http://www.mathworks.com/products/distriben/) umożliwia użytkownikom MATLAB korzystać na wielu komputerach w klastrze. 


Za pomocą Azure maszyn wirtualnych, możesz utworzyć klastrów serwera Distributed środowiska MATLAB się te same mechanizmy można przesyłać równoległe pracy jako klastrów lokalnego, takich jak interakcyjne zadań, zadań, niezależnych zadań i zadań komunikacji. Za pomocą Azure w połączeniu z platformy MATLAB ma wiele zalet w porównaniu z inicjowania obsługi administracyjnej i przy użyciu tradycyjnego lokalnego sprzętowe: zakres maszyn wirtualnych o rozmiarze, tworzenie klastrów na żądanie, możesz zapłacić tylko dla zasobów obliczeń użytkowania i możliwość testowanie modeli w skali.  

## <a name="prerequisites"></a>Wymagania wstępne

* **Komputer kliencki** - musisz systemu Windows na komputerze klienckim można komunikować się z Azure a klastrem serwerów Distributed środowiska MATLAB po wdrożeniu. 

* **Azure programu PowerShell** — zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) ją zainstalować na komputerze klienckim użytkownika. 

* **Azure subskrypcji** — Jeśli nie masz subskrypcji, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) na kilka minut. W przypadku większych klastrów należy rozważyć, czy płatne subskrypcji lub innych opcji zakupu. 

* **Przydział rdzenie** - może być konieczne zwiększyć przydział core do wdrożenia dużych klaster lub więcej niż jeden klaster serwerów Distributed środowiska MATLAB. Aby zwiększyć przydziału, [Otwórz żądanie pomocy technicznej online klienta](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) bezpłatnie. 

* **Równoległe zestaw narzędzi obliczeniowych i licencje serwera Distributed środowiska MATLAB MATLAB** - skrypty przyjęto założenie, że [Menedżer licencji hostowanej MathWorks](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) jest używany dla wszystkich licencji.  

* **Oprogramowanie serwera Distributed środowiska MATLAB** - zostanie zainstalowany na maszyn wirtualnych, który będzie używany jako podstawa obraz maszyn wirtualnych klaster maszyny wirtualne. 


## <a name="high-level-steps"></a>Wysoki poziom czynności

Aby użyć Azure maszyn wirtualnych klastrów serwerów Distributed środowiska MATLAB, robiąc tak wysokiego poziomu są wymagane. Szczegółowe informacje znajdują się w dokumentacji towarzyszącej szablonu Szybki Start i skryptów na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Tworzenie podstawowego obrazu maszyn wirtualnych**  
    * Pobieranie i instalowanie oprogramowania serwera Distributed środowiska MATLAB na tym maszyn wirtualnych. 

    >[AZURE.NOTE]Ten proces może potrwać kilka godzin, ale musisz to zrobić, gdy dla każdej wersji MATLAB używasz.   
    
2. **Tworzenie klastrów jeden lub więcej**  
    * Aby utworzyć klaster z obrazu maszyn wirtualnych, należy użyć podany skrypt programu PowerShell lub szablon Szybki Start.   
    * Zarządzanie klastrów przy użyciu podany skrypt programu PowerShell, dzięki czemu można listy, wstrzymać, wznowić, a następnie usuń klastrów. 
 
## <a name="cluster-configurations"></a>Konfiguracje klastrów 

Obecnie skryptu tworzenia klaster i szablonu można utworzyć pojedynczy topologii serwerów Distributed środowiska MATLAB. Jeśli chcesz, należy utworzyć jeden lub kilka dodatkowych klastrów z każdy klaster o inną liczbę pracowników maszyny wirtualne, za pomocą różnych rozmiarach maszyn wirtualnych i tak dalej. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB klientem a klastrem platformy Azure 

Węzeł Klient MATLAB, węzeł MATLAB harmonogram zadań i węzły serwera Distributed środowiska MATLAB "pracownik" wszystkie skonfigurowano jako maszyny wirtualne Azure w sieci wirtualnej, jak pokazano na poniższym rysunku. 

![Klaster topologii](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Aby użyć klaster, łączenie przez pulpitu zdalnego do węzła klienta. Węzeł klient uruchamia klienta MATLAB. 

* Węzeł Klient ma udziału plików, które są dostępne dla wszystkich pracowników.

* Menedżer licencji hostowanej MathWorks jest używana do kontroli licencji dla każdego oprogramowania MATLAB. 

* Domyślnie jeden pracownik serwera Distributed środowiska MATLAB na core zostanie utworzona dla pracownika maszyny wirtualne, ale można określić dowolną liczbę. 


## <a name="use-an-azure-based-cluster"></a>Używanie klastrze oparte na platformie Azure 

Jako z innymi typami klastrów serwerów Distributed środowiska MATLAB, musisz utworzyć profil klaster MATLAB harmonogram zadań za pomocą Menedżera profilów klaster w kliencie programu MATLAB (na komputerze klienckim maszyn wirtualnych).

![Menedżer profilów klaster](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Następne kroki

* Aby uzyskać szczegółowe instrukcje wdrażanie i zarządzać nimi, klastrów serwerów Distributed środowiska MATLAB platformy Azure Zobacz repozytorium [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) zawierający szablony i skryptów. 

* Przejdź do [witryny MathWorks](http://www.mathworks.com/) szczegółowa dokumentacja MATLAB i MATLAB Distributed przetwarzania danych serwera.
