## <a name="peering-across-subscriptions"></a>Zaglądanie w subskrypcjach

W tym scenariuszu utworzysz zaglądanie między dwoma VNets należące do różnych subskrypcjach.

![krzyżowe scenariusz podrzędny](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Zaglądanie VNet zależy od kontrola dostępu oparta na rolach (RBAC) o zezwolenie. W scenariuszu krzyżowe subskrypcje należy najpierw udzielić odpowiednich uprawnień użytkownikom, którzy będą tworzenie peering łącza:

> [AZURE.NOTE] Jeśli użytkownik ma uprawnienia w obu subskrypcje, można pominąć step1-4 poniżej.
