## <a name="defining-a-backup-policy"></a>Definiowanie zasady tworzenia kopii zapasowych

Zasady tworzenia kopii zapasowych określa macierzy po wykonywane migawki danych i jak długo trwa migawki te są zachowywane. Podczas definiowania zasady tworzenia kopii zapasowych maszyn wirtualnych, mogą być uruchamiane *raz dziennie*zadania wykonywania kopii zapasowej. Podczas tworzenia nowych zasad jest stosowana do magazyn. Interfejs kopii zapasowej zasad wygląda następująco:

![Zasady kopii zapasowej](./media/backup-create-policy-for-vms/backup-policy.png)

Aby utworzyć zasady:

1. Wprowadź nazwę dla **Nazwa zasady**.

2. Migawki danych mogą być wykonywane w odstępach dzienny lub tygodniowy. Menu rozwijane **Częstotliwość kopia zapasowa** umożliwia wybieranie czy migawki danych są wykonywane codziennie lub co tydzień.

    - Jeśli wybierzesz liczbę dni, formant wyróżniony wybranie dnia dla migawki. Aby zmienić godzinę, usuń zaznaczenie pola wyboru godzinę, a następnie wybierz pozycję Nowy godzina.

    ![Dzienny zasady kopii zapasowej](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Jeśli wybierzesz interwał tygodniowy, użyj opcji wyróżniony zaznacz dni tygodnia i godzinę, aby wykonać migawkę. W menu dzień wybierz jeden lub kilka dni. W menu godzinę wybierz godzinę. Aby zmienić godzinę, usuń zaznaczenie pola wyboru zaznaczonego godzinę, a następnie wybierz pozycję Nowy godzina.

    ![Tygodniowy zasad kopii zapasowej](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Domyślnie zaznaczone są wszystkie opcje **Przechowywania zakresu** . Wyczyść pole wyboru limitów zakresu przechowywania, których chcesz użyć. Następnie określ interval(s) korzystać.

    Miesięcznych i rocznych zakresów przechowywania umożliwiają określenie migawki według przyrostu tygodniowy lub codziennie.

    >[AZURE.NOTE] Ochrona maszyny, zadanie kopii zapasowej uruchamiane raz dziennie. Przy uruchomieniu podczas tworzenia kopii zapasowej jest taki sam dla każdego zakresu przechowywania.

4. Po ustawieniu wszystkich opcji zasady, w górnej części karta kliknij przycisk **Zapisz**.

    Magazyn natychmiast dotyczy nowych zasad.
