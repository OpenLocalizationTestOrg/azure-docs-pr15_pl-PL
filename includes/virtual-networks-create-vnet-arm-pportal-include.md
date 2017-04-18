## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Jak utworzyć VNet w portalu Azure

Aby utworzyć VNet według tego scenariusza powyżej za pomocą portalu Azure Podgląd, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij przycisk **Nowy** > **sieci** > **wirtualnej sieci**, następnie kliknij pozycję **Menedżer zasobów** z listy **Wybierz model wdrożenia** , a następnie kliknij **Utwórz**, jak pokazano na poniższej ilustracji.

    ![Tworzenie VNet w Azure portal](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Na karta **Tworzenie wirtualnych sieci** Skonfiguruj ustawienia VNet, jak pokazano na poniższej ilustracji.

    ![Tworzenie wirtualnych sieci karta](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Kliknij pozycję **Grupa zasobów** i wybierz grupę zasobów, aby dodać VNet do lub kliknij pozycję **Utwórz nowy** , aby dodać VNet do nowej grupy zasobów. Na poniższym rysunku przedstawiono zasobu ustawienia grupy dla nowej grupy zasobów o nazwie **TestRG**. Aby uzyskać więcej informacji dotyczących grup zasobów odwiedź stronę [Azure Omówienie Menedżera zasobów](../articles/resource-group-overview.md#resource-groups).

    ![Grupa zasobów](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. W razie potrzeby zmień ustawienia **subskrypcji** i **lokalizacji** usługi VNet. 

6. Jeśli nie chcesz wyświetlać VNet jako kafelka w **Startboard**, wyłącz **Przypnij do Startboard**. 

7. Kliknij pozycję **Utwórz** i zwróć uwagę kafelków o nazwie **Tworzenie wirtualnych sieci** , jak pokazano na poniższej ilustracji.

    ![Tworzenie wirtualnych sieci kafelków](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Poczekaj, aż VNet ma zostać utworzony, a następnie w karta **wirtualnej sieci** , kliknij polecenie **wszystkie ustawienia** > **podsieci** > **Dodaj** jak pokazano poniżej.

    ![Dodawanie podsieci Azure portal](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Określ ustawienia podsieci podsieć *wewnętrznej bazy danych* , jak pokazano poniżej, a następnie kliknij **przycisk OK**. 

    ![Ustawienia podsieci](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Zwróć uwagę na liście podsieci, jak pokazano na poniższej ilustracji.

    ![Lista podsieci VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
