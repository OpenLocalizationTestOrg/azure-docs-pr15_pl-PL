<properties 
   pageTitle="Wdrażanie maszyn wirtualnych przy użyciu statycznego publiczny adres IP przy użyciu portalu Azure w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć maszyny wirtualne przy użyciu statycznego publiczny adres IP przy użyciu portalu zure w Menedżerze zasobów"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Wdrażanie maszyn wirtualnych przy użyciu statycznego publiczny adres IP przy użyciu portalu Azure

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Tworzenie maszyn wirtualnych przy użyciu statycznego publiczny adres IP 

Aby utworzyć maszyn wirtualnych przy użyciu statycznego adresu IP publicznej w portalu Azure, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do [portalu Azure](https://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. W lewym górnym rogu portalu, kliknij przycisk **Nowy**>>**obliczyć**>**Systemu Windows Server 2012 R2 w centrum danych**.
3. Na liście **Wybierz model wdrażania** wybierz pozycję **Menedżer zasobów** , a następnie kliknij przycisk **Utwórz**.
4. W karta **podstawy** wprowadź informacje maszyn wirtualnych, tak jak pokazano poniżej, a następnie kliknij **przycisk OK**.

    ![Portal Azure — podstawy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. W karta **Wybierz rozmiar** kliknij pozycję **Standardowy A1** , tak jak pokazano poniżej, a następnie kliknij **Wybierz**.

    ![Portal Azure — Wybieranie rozmiaru](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. Karta **Ustawienia** kliknij pozycję **publiczny adres IP**, a następnie w karta **Tworzenie publiczny adres IP** w obszarze **przydziału**, kliknij **statyczne** tak jak pokazano poniżej. A następnie kliknij **przycisk OK**.

    ![Portal Azure — tworzenie publiczny adres IP](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. W karta **Ustawienia** kliknij **przycisk OK**.
8. Przejrzyj karta **Podsumowanie** , tak jak pokazano poniżej, a następnie kliknij **przycisk OK**.

    ![Portal Azure — tworzenie publiczny adres IP](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Zwróć uwagę, nowe kafelka w pulpitu nawigacyjnego.

    ![Portal Azure — tworzenie publiczny adres IP](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Po utworzeniu maszyn wirtualnych, tak jak pokazano poniżej zostanie wyświetlona karta **Ustawienia**

    ![Portal Azure — tworzenie publiczny adres IP](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)