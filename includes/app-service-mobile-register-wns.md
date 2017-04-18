
1. W Eksploratorze rozwiązań programu Visual Studio, kliknij prawym przyciskiem myszy projektu aplikacji ze Sklepu Windows, kliknij pozycję **Magazyn** > **Skojarzyć aplikacji ze sklepem...**.

    ![Kojarzenie aplikacji ze Sklepu Windows](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. W kreatorze kliknij przycisk **Dalej**, zaloguj się przy użyciu konta Microsoft, wpisz nazwę aplikacji w **minimalnej nową nazwę aplikacji**, a następnie kliknij pozycję **Minimalna**.

3. Po pomyślnym utworzeniu rejestracji aplikacji wybierz nową nazwę aplikacji, kliknij przycisk **Dalej**, a następnie kliknij, **Kojarzenie**. Spowoduje to dodanie wymaganych informacji o rejestracji ze Sklepu Windows manifest aplikacji.

7. Powtórz kroki od 1 do 3 dla projektu aplikacji ze Sklepu Windows Phone przy użyciu tę samą rejestrację utworzonego dla aplikacji ze Sklepu Windows.  

7. Przejdź do [Centrum deweloperów programu Windows](https://dev.windows.com/en-us/overview)logowania przy użyciu konta Microsoft, kliknij przycisk Nowy wpis aplikacji **Moje aplikacje**, a następnie rozwiń węzeł **usługi** > **powiadomienia Push**.

8. Na stronie **powiadomienia wypychane** kliknij **witrynę usług Live** w obszarze **Microsoft Azure Mobile aplikacje i usługi wypychanych powiadomień systemu Windows (WNS)**i zanotuj wartości **SID pakietu** i *bieżącą* wartość **Hasło aplikacji**. 

    ![Ustawienia aplikacji w Centrum deweloperów](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Tajny aplikacji i pakiet SID są poświadczenia zabezpieczeń ważne. Nie udostępniać te wartości osobie lub rozpowszechniać za pomocą aplikacji.
