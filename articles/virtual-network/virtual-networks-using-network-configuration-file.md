<properties 
    pageTitle="Konfigurowanie sieci wirtualnej za pomocą pliku konfiguracji sieci" 
    description="Instrukcje dla eksportowanie oraz importowanie pliku konfiguracji sieci do portalu zarządzania Azure, aby można było utworzyć lub zmodyfikować wirtualnych sieci. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Konfigurowanie sieci wirtualnej za pomocą pliku konfiguracji sieci

Wirtualna sieć (VNet) można skonfigurować za pomocą portalu zarządzania Azure lub za pomocą pliku konfiguracji sieci.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Tworzenie i modyfikowanie pliku konfiguracji sieci 
Najprostszym sposobem na tworzenie pliku konfiguracji sieci jest eksportowanie ustawień sieci z istniejącej konfiguracji wirtualną sieć, następnie zmodyfikować plik zawierają ustawienia, które chcesz skonfigurować usługi wirtualnych sieci.

Aby edytować plik konfiguracji sieci, możesz może po prostu otwórz plik, wprowadź odpowiednie zmiany, a następnie zapisz plik. Aby wprowadzić zmiany w pliku konfiguracji sieci, można użyć dowolnego edytora *xml* . 

Wskazówki dotyczące [ustawień schematu pliku konfiguracji sieci](https://msdn.microsoft.com/library/azure/jj157100.aspx)ściśle należy wykonać. 

Azure uważa podsieci, która zawiera element znajdujący się na jako **używany**. Gdy podsieć jest używany, nie można modyfikować. Przed zmodyfikowaniem, Przenieś wszystkie elementy, które wdrożono aby różnych podsieci, która nie jest modyfikowany.   Zobacz [przenoszenie maszyn wirtualnych lub wystąpienia roli do innej podsieci](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Eksportowanie oraz importowanie ustawień wirtualną sieć za pomocą portalu zarządzania  
Można importować i eksportować ustawienia konfiguracji sieci zawarte w pliku konfiguracji sieci przy użyciu programu PowerShell lub portalu zarządzania. Poniższe instrukcje pomoże Ci eksportu i importu za pomocą portalu zarządzania. 

### <a name="to-export-your-network-settings"></a>Aby wyeksportować ustawienia sieci
Po wyeksportowaniu, wszystkie ustawienia sieci wirtualnych w ramach subskrypcji zostaną zapisane w pliku XML. 

1. Logowanie do **portalu zarządzania**.
2. W portalu zarządzania u dołu strony **sieci** kliknij przycisk **Eksportuj**. 
3. W oknie **Eksportuj konfigurację sieci** Sprawdź, czy wybrano subskrypcji, dla którego chcesz wyeksportować ustawienia sieci. Następnie kliknij znacznik wyboru w prawej dolnej. 
4. Gdy zostanie wyświetlony monit, Zapisz plik *NetworkConfig.xml* do lokalizacji, w dowolnym miejscu.


### <a name="to-import-your-network-settings"></a>Aby zaimportować ustawienia sieci

1. W **Portalu zarządzania**w okienku nawigacji w lewym dolnym rogu kliknij przycisk **Nowy**.
2. Kliknij pozycję **usługi sieciowe** -> **wirtualnej sieci** -> **Importuj konfigurację**.
3. Na stronie **Importuj plik konfiguracji sieci** przejdź do pliku konfiguracji sieci, a następnie kliknij strzałkę **następnego** .
4. Na stronie **Tworzenie sieci** pojawi się informacji wyświetlanych na ekranie, które części konfiguracji sieci zostaną zmienione lub utworzone. Jeśli zmiany poprawna do Ciebie, kliknij znacznik wyboru, aby przejść do aktualizacji lub tworzenie wirtualnych sieci. 