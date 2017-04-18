Po jest propagowana rekordy dla nazwy domeny, musisz kojarzenie ich z aplikacji sieci Web. Wykonaj następujące czynności, aby włączyć nazw domen przy użyciu przeglądarki sieci web.

> [AZURE.NOTE] Może upłynąć trochę czasu dla rekordów TXT utworzonych w poprzednich krokach propagowanie w systemie DNS. Nazwa domeny nie można dodać do aplikacji sieci web, aż rekord TXT jest propagowana. Jeśli korzystasz z rekordu A, nie możesz dodać nazwę domeny rekordu A do aplikacji sieci web do momentu jest propagowana rekord TXT, utworzony w poprzednim kroku.
>
> Za pomocą usług, takich jak <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> Aby sprawdzić, czy rekord TXT jest dostępny.

1. Otwórz [Azure Portal](https://portal.azure.com)w przeglądarce.

2. Na karcie **Aplikacje sieci Web** kliknij nazwę aplikacji sieci web, a następnie wybierz **domen niestandardowych**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. W karta **domen niestandardowych** kliknij przycisk **Dodaj nazwa hosta**.
    
4. Wprowadzanie nazw domen, który chcesz skojarzyć tę aplikację sieci web za pomocą pola **Nazwa hosta** .

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Kliknij przycisk **Sprawdzanie poprawności**.

7.  Po kliknięciu Azure **sprawdzania poprawności** będzie rozpoczynanie weryfikacji domeny przepływu pracy. To sprawdza własności domeny, a także sukces dostępności i raport nazwa hosta lub szczegółowe błąd wskazowki porady na temat poprawienia błędu.    

W tym momencie powinno być możliwe wprowadź nazwę domeny niestandardowej w przeglądarce i zobacz, że pomyślnie przejście do aplikacji sieci web.
