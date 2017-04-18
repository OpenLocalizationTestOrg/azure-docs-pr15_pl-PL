1. W portalu z **wszystkie zasoby**kliknij przycisk **+ Dodaj**. W polu **wszystko** karta wyszukiwanie wpisz **bramy sieci lokalnej**, a następnie kliknij, aby wyszukać. Zwraca listę. Kliknij pozycję **bramy sieci lokalnej** , aby otworzyć karta, a następnie kliknij przycisk **Utwórz** , aby otworzyć karta **Tworzenie sieci lokalnej bramy** .

    ![Tworzenie bramy sieci lokalnej](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Na **Tworzenie sieci lokalnej bramy karta**określ **nazwę** obiektu bramy sieci lokalnej.
 
3. Określ prawidłowy publiczny **adres IP** dla urządzenia VPN lub Brama wirtualnej sieci, do którego chcesz się połączyć.<br>Jeśli ten sieci lokalnej reprezentuje lokalizacji lokalnego, to publiczny adres IP urządzenia VPN, którego chcesz się połączyć. Nie może być za translatora adresów Sieciowych, a musi być dostępne przy Azure.<br>Jeśli ten sieci lokalnej reprezentuje innym VNet, określisz publiczny adres IP, przypisany do bramy wirtualną sieć dla tego VNet.<br>

4. **Przestrzeń adresów** odwołuje się do zakresów adresów dla sieci, która reprezentuje tej sieci lokalnej. Możesz dodać wiele zakresów miejsca adresów. Upewnij się, że zakresy, wybrane tutaj nie pokrywają się z zakresami innych sieci, z którymi chcesz się połączyć.
 
5. **Subskrypcja**upewnij się, że poprawne subskrypcji jest widoczna.

6. **Grupa zasobów**wybierz grupę zasobów, do której chcesz użyć. Możesz utworzyć nową grupę zasobów lub wybrać jedną, które zostały już utworzone.

7. Dla **lokalizacji**zaznacz ten obiekt zostanie utworzony w lokalizacji. Być może zechcesz wybierz lokalizację usługi VNet znajduje się w, ale nie jest wymagane w tym celu.

8. Kliknij przycisk **Utwórz** , aby utworzyć bramę sieci lokalnej.
