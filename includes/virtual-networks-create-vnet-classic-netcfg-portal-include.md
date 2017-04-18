## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Jak utworzyć VNet za pomocą pliku konfiguracji sieci w portalu Azure

Azure używa pliku xml do definiowania VNets wszystkich dostępnych do subskrypcji. Można pobrać ten plik i edytować go, aby zmodyfikować lub usunąć istniejących VNets i tworzyć nowych plików. W tym dokumencie możesz dowiedzieć się, jak pobrać ten plik, określane jako pliku konfiguracji (lub netcgf) sieciowego i edytowanie go, aby utworzyć nowy VNet. Sprawdź [Schemat konfiguracji Azure wirtualną sieć](https://msdn.microsoft.com/library/azure/jj157100.aspx) , aby dowiedzieć się więcej o pliku konfiguracji sieci.

Aby utworzyć VNet za pomocą pliku netcfg za pośrednictwem portalu Azure, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://manage.windowsazure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Przewiń w dół na liście usług, a następnie kliknij w **SIECIACH** , jak pokazano poniżej.

    ![Azure wirtualnych sieci](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. U dołu strony kliknij przycisk **EKSPORTUJ** , tak jak pokazano poniżej.

    ![Przycisk Eksportuj](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Na stronie **Eksportowanie konfiguracji sieci** Wybierz subskrypcję, do wyeksportowania konfigurację wirtualnej sieci z, a następnie kliknij przycisk znacznika wyboru w dolnym lewym górnym rogu strony.
5. Postępuj zgodnie z instrukcjami podanymi przeglądarki, aby zapisać plik **NetworkConfig.xml** . Upewnij się, możesz Uwaga miejsce, w którym plik jest zapisywany.
6. Otwórz plik został zapisany w kroku 5 powyżej za pomocą dowolnej aplikacji Edytor XML lub tekst, a poszukaj **<VirtualNetworkSites>** elementu. Jeśli masz już utworzony sieci każda sieć będzie wyświetlana jako własnej **<VirtualNetworkSite>** elementu.
7. Aby utworzyć wirtualną sieć opisaną w tym scenariuszu, Dodaj następujący kod XML tuż poniżej **<VirtualNetworkSites>** elementu:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Zapisz plik konfiguracji sieci.
9.  W portalu Azure na lewy górny róg strony kliknij przycisk **Nowy**, a następnie kliknij **Usług sieci**, a następnie kliknij **WIRTUALNĄ sieć**, a następnie kliknij pozycję **IMPORTUJ konfigurację** , jak pokazano na poniższej ilustracji.

    ![Importowanie konfiguracji](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Na stronie **Importowanie pliku konfiguracji sieci** kliknij pozycję **Przejdź do pliku...**, a następnie przejdź do folderu, który plik jest zapisany w kroku 8 powyżej, wybierz plik i kliknij przycisk **Otwórz**. Strony sieci web powinna wyglądać podobnie jak na poniższym rysunku. W prawym dolnym rogu strony kliknij przycisk strzałki, aby przejść do następnego kroku.

    ![Strona pliku konfiguracji sieci importu](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Na stronie **Tworzenie sieci** Zwróć uwagę, wpis dla do nowych VNet, jak pokazano na poniższej ilustracji.

    ![Tworzenie strony sieci](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Kliknij przycisk znacznika wyboru w prawym dolnym rogu strony, aby utworzyć VNet. Po kilku sekundach usługi VNet będą wyświetlane na liście dostępne VNets, jak pokazano na poniższej ilustracji.

    ![Nowa sieć wirtualna](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)