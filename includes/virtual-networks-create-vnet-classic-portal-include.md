## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Jak utworzyć VNet w portalu Azure

Aby utworzyć VNet według tego scenariusza powyżej, wykonaj poniższe czynności.

1. Za pomocą przeglądarki, przejdź do http://manage.windowsazure.com i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.
2. Kliknij przycisk **Nowy** > **Usług SIECIOWYCH** > **WIRTUALNEJ sieci** > **Utworzyć niestandardowe** , jak pokazano na poniższej ilustracji.

    ![Tworzenie VNet w portalu](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Na stronie **Szczegóły wirtualna sieć** wpisz **nazwę** VNet, zaznacz jego **lokalizacji**, a następnie kliknij strzałkę w prawym dolnym rogu strony, aby przejść do kroku 2. Na poniższym rysunku przedstawiono ustawienia dla naszych scenariusza.

    ![Strona Szczegóły wirtualnej sieci](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Na stronie **serwery DNS i połączenia VPN** Określ nazwę i adres IP maksymalnie 9 serwerów DNS. Jeśli nie określisz serwera DNS, usługi VNet użyje wewnętrznych nazewnictwa rozdzielczość rozdzielczość dostarczony przez Azure. W naszym scenariuszu firma Microsoft nie skonfiguruje serwerów DNS.
5. Jeśli chcesz udostępnić punkt do witryny sieci VPN z VNet, należy włączyć pole wyboru **Konfiguruj VPN punkt do witryny** . Jeśli nie zostanie skonfigurowane VPN punktu do witryny, możesz dodać go do swojego VNet w dowolnym momencie po utworzeniu. W naszym scenariuszu firma Microsoft nie skonfiguruje połączenie VPN punktu do witryny.
6. Jeśli chcesz zapewnić witryny do witryny sieci VPN łączność z VNet i innym VNet lub sieci lokalnej, **Konfigurowanie sieci VPN witryny —** pole wyboru Włącz, a następnie określ, czy chcesz nawiązać połączenie przy użyciu **ExpressRoute** lub notatki i nazwę sieci. Jeśli nie zostanie skonfigurowane VPN witryny do witryny, możesz dodać je do swojego VNet w dowolnym momencie po utworzeniu. W naszym scenariuszu firma Microsoft nie skonfiguruje VPN witryny do witryny.
7. Kliknij strzałkę w prawym dolnym rogu strony, aby przejść do 3.w krok, który na poniższym rysunku przedstawiono ustawienia dla naszych scenariusza.

    ![Serwery DNS i VPN strony łączności](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Na stronie **Wirtualna sieć adres spacje** w obszarze **Uruchamianie IP**kliknij *10.0.0.0* , aby zmienić przestrzeń adresów VNet, a następnie wpisz początkowe przestrzeni adresów, którego chcesz użyć. W naszym scenariuszu wpisz *192.168.0.0*. 
9. W obszarze **CIDR (liczba adresów)** wybierz liczbę bitów maski podsieci. W naszym scenariuszu wybierz *16 (65536)*.
10. W obszarze **PODSIECI**kliknij przycisk *podsieci 1* i Zmień nazwę podsieci, jeśli to konieczne. Nasze scenariusza można ją zmienić do *serwera sieci Web*.

    >[AZURE.NOTE] Jeśli klikniesz poza pole tekstowe Nazwa podsieć nie będą mogli edytować nazwę, jeśli podsieci ponownie. Aby rozwiązać ten problem, należy usunąć podsieci, klikając przycisk X po jej prawej stronie, a następnie dodaj nowa podsieć, zgodnie z opisem w kroku 13 poniżej.

11. W obszarze **Uruchamianie adresów IP** dla pierwszej podsieci Określ początkowy adres IP dla podsieci. W naszym scenariuszu wpisz *192.168.1.0*.
12. W obszarze **CIDR (liczba adresów)** wybierz liczbę bitów dla maski podsieci pierwszą podsieć. W naszym scenariuszu wybierz *24 (256)*.
13. Kliknij pozycję **Dodawanie podsieci** , aby dodać nowy podsieci, jeśli to konieczne. W naszym scenariuszu Dodaj podsieci, a następnie powtórz kroki od 10 do 12, aby skonfigurować VNet, jak pokazano na poniższej ilustracji.

    ![Wirtualna sieć adres spacje strony](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Kliknij przycisk znacznika wyboru w prawym dolnym rogu strony, aby utworzyć VNet. Po kilku sekundach usługi VNet będą wyświetlane na liście dostępne VNets, jak pokazano na poniższej ilustracji.

    ![Nowa sieć wirtualna](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)