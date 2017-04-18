Po jest propagowana rekordy dla nazwy domeny, należy sprawdzić, czy niestandardowej nazwy domeny można uzyskać dostęp do aplikacji sieci web w usłudze Azure aplikacji przy użyciu przeglądarki.

> [AZURE.NOTE] Może upłynąć trochę czasu dla swojego CNAME propagowanie w systemie DNS. Za pomocą usług, takich jak <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> CNAME jest dostępna.

Jeśli nie zostało już dodane aplikacji sieci web jako punkt końcowy Menedżer ruchu, musisz to zrobić, zanim rozpoznawanie nazw działa jako trasy nazwę domeny niestandardowej do Menedżera ruch. Menedżer ruchu następnie kieruje do aplikacji sieci web. Dodawanie aplikacji sieci web jako punkt końcowy w swoim profilu Menedżer ruchu, skorzystaj z informacji w oknie [Dodawanie lub usuwanie punktów końcowych](../articles/traffic-manager/traffic-manager-endpoints.md) .

> [AZURE.NOTE] Jeśli aplikacji sieci web nie jest wyświetlana podczas dodawania punktu końcowego, upewnij się, że nie jest skonfigurowana do trybu plan **standardowej** aplikacji usługi. Praca z menedżerem ruchu, należy użyć **standardowego** trybu dla aplikacji sieci web.

1. Otwórz [Azure Portal](https://portal.azure.com)w przeglądarce.

1. Na karcie **Aplikacje sieci Web** kliknij nazwę aplikacji sieci web, wybierz pozycję **Ustawienia**, a następnie wybierz pozycję **domen niestandardowych**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. W karta **domen niestandardowych** kliknij przycisk **Dodaj nazwa hosta**.
    
1. Wprowadzanie nazwy domeny Menedżer ruchu chcesz skojarzyć tę aplikację sieci web za pomocą pola **Nazwa hosta** .

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Kliknij przycisk **Sprawdzanie poprawności** , aby zapisać konfigurację nazwę domeny.

7.  Po kliknięciu Azure **sprawdzania poprawności** będzie rozpoczynanie weryfikacji domeny przepływu pracy. To sprawdza własności domeny, a także sukces dostępności i raport nazwa hosta lub szczegółowe błąd wskazowki porady na temat poprawienia błędu.    

8.  Po sprawdzeniu poprawności **hostname Dodaj** przycisk stanie się aktywny, i będzie mógł przypisywanie nazwa hosta. Teraz przejść do niestandardowej nazwy domeny w przeglądarce. Powinien zostać wyświetlony swojej pracy aplikacji przy użyciu niestandardowej nazwy domeny. 

    Po zakończeniu konfiguracji niestandardowej nazwy domeny zostaną zapisane w sekcji **nazw domen** aplikacji sieci web.

Na tym etapie można wpisać nazwę nazwy domeny Menedżer ruchu w przeglądarce i zobacz, że pomyślnie przejście do aplikacji sieci web.
