<properties 
   pageTitle="Zarządzanie serwery DNS używane przez sieć wirtualną (VNet)"
   description="Dowiedz się, jak dodawać i usuwać serwery DNS w sieci wirtualnych (vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Zarządzanie serwery DNS używane przez sieć wirtualną (VNet)

Możesz zarządzać listy serwerów DNS używanych w VNet w portalu zarządzania lub w pliku konfiguracji sieci. Możesz dodać maksymalnie 12 serwerów DNS dla każdego VNet. Określanie serwerów DNS, to należy upewnić się, że listy serwerów DNS w poprawnej kolejności w środowisku usługi. Listy serwerów DNS nie działają okrężny. Są one używane w ich określonej kolejności. Jeśli pierwszy serwer DNS na liście może się z Tobą, klient będzie używał tego serwera DNS, niezależnie od tego, czy serwer DNS działa poprawnie lub nie. Aby zmienić kolejność serwera DNS w sieci wirtualnej, usunąć serwery DNS z listy, a następnie dodaj je ponownie w kolejności, w jakiej chcesz.

>[AZURE.WARNING] Po zaktualizowaniu listy DNS, należy ponownie uruchomić maszyn wirtualnych znajduje się w sieci wirtualnej tak, aby były skompletowanie nowe ustawienia serwera DNS. Maszyn wirtualnych nadal będzie korzystać ich bieżącej konfiguracji do momentu ich ponownego uruchomienia.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Edytowanie listy serwerów DNS dla wirtualną sieć za pomocą portalu zarządzania

1. Zaloguj się do **portalu zarządzania**.

1. W okienku nawigacji kliknij pozycję **sieci**, a następnie kliknij nazwę wirtualnej sieci w kolumnie **Nazwa** .

1. Kliknij przycisk **Konfiguruj**.

1. **Serwery DNS**można skonfigurować następujące czynności:

    - **Aby zarejestrować serwer DNS nowego (Dodaj) —** Wystarczy wpisać w polach Nazwa i adres IP. Dodaje serwer DNS do listy serwerów DNS sieci wirtualnej, a także rejestruje serwera DNS Azure.

    - **Aby dodać serwera DNS, który został wcześniej zarejestrowany —** Jeśli serwer DNS jest już zarejestrowana z Azure, możesz go wybrać z listy wstępnie wypełnione.

    - **Aby usunąć serwera DNS z sieci lokalnej wirtualną —** Kliknij znak X obok serwera, który chcesz usunąć. Uwaga to tylko usuwa serwer z listy wirtualnej sieci. Serwer DNS pozostaje zarejestrowanych w Azure usługi wirtualnych sieci korzystać. Aby usunąć serwer DNS z subskrypcji, przejdź do strony **sieci -> serwerów DNS** .

    - **Aby zmienić porządek serwerów DNS —** Usuwanie wszystkich serwerów DNS, które są wyświetlane, a następnie dodaj je ponownie w kolejności, w jakiej chcesz. Pamiętaj, że nie jest listy DNS okrężny.

    - **Aby zmienić nazwę serwera DNS —** Wyróżnij serwera DNS na liście, a następnie wpisz nową nazwę. To będzie zarejestrować nowy serwer DNS w Azure, a także dodać go do listy serwerów DNS sieci wirtualnej. Stary serwer DNS i jego adres IP będzie w dalszym ciągu zarejestrowany Azure. Możesz usunąć go na stronie **Serwery DNS** , jeśli nie używasz go wirtualnych sieci.

1. Kliknij pozycję **Zapisz** u dołu strony do zapisania nowej konfiguracji serwerów DNS.

1. Uruchom ponownie maszyn wirtualnych znajduje się w wirtualnej sieci, aby umożliwić im zdobycie nowe ustawienia DNS.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Edytowanie listy serwerów DNS za pomocą pliku konfiguracji sieci

Aby edytować listy serwerów DNS za pomocą pliku konfiguracji sieci, będą najpierw wyeksportować ustawienia konfiguracji za pomocą portalu zarządzania. Następnie można edytować plik konfiguracji sieci i importować go ponownie za pomocą portalu zarządzania. Poniżej przedstawiono listę wysokiego poziomu czynności, aby zakończyć ten proces.

1. Ustawienia wirtualnej sieci wyeksportować do pliku konfiguracji sieci. Aby uzyskać więcej informacji i czynności, aby wyeksportować ustawienia konfiguracji sieci zobacz [Wirtualna sieć ustawienia eksportu do pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md).

1. Określ informacje o serwerze DNS dla wirtualnych sieci. Aby uzyskać więcej informacji na temat określania serwera DNS Zobacz [Określanie serwera DNS w pliku konfiguracji wirtualnych sieci](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Aby uzyskać dodatkowe informacje o plikach konfiguracji sieci zobacz [Azure wirtualna sieć konfiguracji schematu](https://msdn.microsoft.com/library/azure/jj157100.aspx) i [skonfigurować wirtualnej sieci przy użyciu pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md).

1. Importowanie pliku konfiguracji sieci. Aby uzyskać więcej informacji i czynności, aby zaimportować plik konfiguracji sieci zobacz [importowania pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md).

1. Uruchom ponownie maszyn wirtualnych znajduje się w wirtualnej sieci, aby umożliwić im zdobycie nowe ustawienia DNS.
