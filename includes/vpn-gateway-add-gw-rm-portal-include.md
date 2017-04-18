1. W portalu przejdź do **nowej**. Wpisz "Brama wirtualnej sieci" w wyszukiwaniu. Znajdź **Brama wirtualnej sieci** w polu Wyszukaj w zwracanej i kliknij pozycję. Spowoduje to otwarcie karta **Tworzenie wirtualnych sieci bramy** .
2. Kliknij przycisk **Utwórz** w dolnej części karta **Brama wirtualnej sieci** . Zostanie otwarta karta **Tworzenie wirtualnych sieci bramy** . Wpisz wartości dla Centrum wirtualnej sieci.

    ![Tworzenie wirtualnych sieci bramy karta pola] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Tworzenie wirtualnych sieci bramy karta pola")

3. **Nazwa**: Nazwa bramy sieci. Nie jest taki sam, jak nazewnictwa podsieci bramy. Jest to nazwa tworzonego obiektu bramy.

4. **Typ bramy**: Wybierz **opcję VPN**. Bramy sieci VPN za pomocą typu bramy wirtualnej sieci **VPN**. 

5. **Typ VPN**: Wybierz odpowiedni typ VPN jest określona dla danej konfiguracji. Większości konfiguracji wymagają typu sieci VPN opartych na trasę.

6. **Jednostka SKU**: Wybierz bramy SKU z menu rozwijanego. SKU wymienione na liście rozwijanej zależy od wybranego typu VPN.

7. **Lokalizacja**: Dopasowywanie pola **lokalizację** wskaż miejsce, w którym znajduje się wirtualnej sieci.
 
8. Wybierz pozycję wirtualnej sieci, do którego chcesz dodać tej bramy. Kliknij **wirtualnej sieci** , aby otworzyć karta **Wybierz wirtualnej sieci** . Wybierz pozycję VNet. Jeśli nie widzisz swojego VNet, upewnij się, że pole **Lokalizacja** wskazuje region, w którym znajduje się wirtualnej sieci.

9. Wybierz pozycję publiczny adres IP. Kliknij pozycję **publiczny adres IP** , aby otworzyć karta **Wybierz publiczny adres IP** . Kliknij przycisk **+ Utwórz nowe** aby otworzyć **Tworzenie publicznej karta adres IP**. Wpisz nazwę swojego publiczny adres IP. Ta karta tworzy obiekt publiczny adres IP jakim publiczny adres IP zostanie dynamicznie przypisana.<br>Kliknij **przycisk OK** , aby zapisać zmiany w tym karta.

10. **Subskrypcja**: Sprawdź, czy poprawne subskrypcji jest zaznaczone.

11. **Grupa zasobów**: to ustawienie jest określona przez wirtualną sieć, wybrana. 

12. Nie dostosować **lokalizację** po określeniu poprzednie ustawienia.

13. Sprawdź ustawienia. Jeśli chcesz uzyskanie są wyświetlane na pulpicie nawigacyjnym, możesz wybrać **numeru Pin do pulpitu nawigacyjnego** w dolnej części karta.

14. Kliknij przycisk **Utwórz** , aby rozpocząć tworzenie bramy. Ustawienia będą sprawdzane, a zobaczysz "Wdrażanie wirtualnych sieci Brama" kafelka na pulpicie nawigacyjnym. Tworzenie bramy może potrwać do 45 minut. Może być konieczne odświeżenie strony portalu Aby sprawdzić stan zakończone.

    ![Wdrażanie wirtualnych Brama sieci] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Wdrażanie wirtualnych Brama sieci")

11. Po utworzeniu bramy, możesz wyświetlić adres IP, która została przypisana do niego analizując wirtualną sieć w portalu. Brama będzie wyświetlany jako podłączonego urządzenia. Możesz kliknąć urządzenie połączone (Centrum wirtualnej sieci), aby wyświetlić więcej informacji.



