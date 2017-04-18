## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Wdrażanie szablonu ARM za pomocą kliknięcia do wdrożenia

Można ponownie użyć wstępnie zdefiniowanych przekazania szablonów ARM do repozytorium github obsługiwany przez firmę Microsoft i Otwórz do społeczności użytkowników. Te szablony można wdrożony bezpośrednio z github, lub pobrać i modyfikować tak, aby nie odpowiada Twoim potrzebom. Aby wdrożyć szablonu, który tworzy VNet z dwóch podsieci, wykonaj poniższe czynności.

1. Za pomocą przeglądarki przejdź do [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Przewiń w dół listę szablonów, a następnie kliknij przycisk **101-vnet 2 podsieci**. Zaznacz plik **README.md** , tak jak pokazano poniżej.

    ![Plik READEME.md w github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Kliknij pozycję **Wdrażanie Azure**. W razie potrzeby wprowadź poświadczenia logowania Azure. 
4. W karta **Parametry** wprowadź wartości, których chcesz użyć do utworzenia do nowych VNet, a następnie kliknij **przycisk OK**. Na poniższym rysunku przedstawiono wartości dla naszych scenariusza.

    ![Parametry szablonu ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Kliknij pozycję **Grupa zasobów** i wybierz grupę zasobów, aby dodać VNet do lub kliknij pozycję **Utwórz nowy** , aby dodać VNet do nowej grupy zasobów. Na poniższym rysunku przedstawiono zasobu ustawienia grupy dla nowej grupy zasobów o nazwie **TestRG**.

    ![Grupa zasobów](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. W razie potrzeby zmień ustawienia **subskrypcji** i **lokalizacji** usługi VNet.
6. Jeśli nie chcesz wyświetlać VNet jako kafelka w **Startboard**, wyłącz **Przypnij do Startboard**.
5. Kliknij pozycję **Leagl terminy**, zapoznaj się z warunkami i kliknij przycisk **Kup** , aby wyrazić. 
6. Kliknij przycisk **Utwórz** , aby utworzyć VNet.

    ![Przesyłając informacje kafelków wdrożenia w portalu preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Po zakończeniu rozmieszczania kliknij **TestVNet** > **wszystkie ustawienia** > **podsieci** , aby wyświetlić właściwości podsieci, tak jak pokazano poniżej.

    ![Tworzenie VNet w preview portal](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)