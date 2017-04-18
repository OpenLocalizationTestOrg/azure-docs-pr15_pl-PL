## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Jak utworzyć klasyczny VNet w Azure portal

Aby utworzyć klasyczny VNet, oparte na powyższych scenariuszy, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://portal.azure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij przycisk **Nowy** > **sieci** > **wirtualnej sieci**, powiadomienie o już zawiera **Klasyczny**listy **Wybierz model wdrożenia** , a następnie kliknij przycisk **Utwórz**, jak pokazano na poniższej ilustracji.

    ![Tworzenie VNet w Azure portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Na karta **wirtualnej sieci** wpisz **nazwę** VNet, a następnie kliknij **przestrzeni adresów**. Skonfigurowanie ustawień adres miejsca dla VNet i jego pierwszego podsieci, a następnie kliknij **przycisk OK**. Na poniższym rysunku przedstawiono ustawienia blokowania CIDR dla naszych scenariusza.

    ![Karta miejsca adresu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Kliknij pozycję **Grupa zasobów** i wybierz grupę zasobów, aby dodać VNet do lub kliknij pozycję **Utwórz nową grupę zasobów** , aby dodać VNet do nowej grupy zasobów. Na poniższym rysunku przedstawiono zasobu ustawienia grupy dla nowej grupy zasobów o nazwie **TestRG**. Aby uzyskać więcej informacji dotyczących grup zasobów odwiedź stronę [Azure Omówienie Menedżera zasobów](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Tworzenie karta Grupa zasobów](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. W razie potrzeby zmień ustawienia **subskrypcji** i **lokalizacji** usługi VNet. 

6. Jeśli nie chcesz wyświetlać VNet jako kafelka w **Startboard**, wyłącz **Przypnij do Startboard**. 

7. Kliknij pozycję **Utwórz** i zwróć uwagę kafelków o nazwie **Tworzenie wirtualnych sieci** , jak pokazano na poniższej ilustracji.

    ![Tworzenie VNet w portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Poczekaj, aż VNet ma zostać utworzony, a gdy zostanie wyświetlony poniżej kafelków, kliknij go, aby dodać więcej podsieci.

    ![Tworzenie VNet w portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. **Konfiguracja** powinna być widoczna dla swojego VNet, tak jak pokazano poniżej. 

    ![Tworzenie VNet w portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Kliknij pozycję **podsieci** > **Dodaj**, wpisz **nazwę** i określ **zakres adresów (CIDR bloku)** dla danej podsieci, a następnie kliknij **przycisk OK**. Na poniższym rysunku przedstawiono ustawienia dla naszych bieżącego scenariusza.

    ![Tworzenie VNet w Azure portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)