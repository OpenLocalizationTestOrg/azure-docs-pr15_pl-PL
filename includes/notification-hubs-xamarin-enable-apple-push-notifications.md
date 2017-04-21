

Aby zarejestrować aplikacji dla powiadomienia wypychane za pośrednictwem firmy Apple wypychanych powiadomień usługi (APN), możesz utworzyć nowego certyfikatu wypychanych, identyfikator aplikacji i obsługi administracyjnej profilu dla projektu w portalu Deweloper firmy Apple. Identyfikator aplikacji będzie zawierać ustawienia konfiguracji, które umożliwiają aplikacji na wysyłanie i odbieranie powiadomień wypychanych. Te ustawienia będzie zawierać certyfikat powiadomień wypychanych wymagane do uwierzytelnienia z Apple wypychanych powiadomień usługi (nazwy APN) podczas wysyłania i odbierania powiadomień wypychanych. Aby uzyskać więcej informacji na temat tych koncepcji dokumentacji oficjalnym [Usługa powiadomień Push firmy Apple](http://go.microsoft.com/fwlink/p/?LinkId=272584) .


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Generowanie pliku żądania podpisania certyfikatu dla certyfikatów push

Te kroki przeprowadził Cię przez proces tworzenia żądania podpisania certyfikatu. To posłuży do generowania certyfikatu wypychanych może być używany z APN.

1. Na komputerze Mac Uruchom narzędzie dostęp do pęku kluczy. Można go było otworzyć z folderu **Narzędzia** lub **innego** folderu w konsoli uruchamianie.

2. Kliknij pozycję **Dostęp do pęku kluczy**, rozwiń **Asystenta certyfikat**, a następnie kliknij pozycję **żądania certyfikatu od urzędu certyfikacji...**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Wybierz swój **Adres E-mail użytkownika** i **Nazwa** , upewnij się, że jest zaznaczona opcja **zapisany na dysku** , a następnie kliknij przycisk **Kontynuuj**. Pozostaw puste pole **Adres E-mail urzędu certyfikacji** , nie jest wymagane.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Wpisz nazwę pliku certyfikatu podpisywania żądania obsługi w polu **Zapisz jako**, wybierz lokalizację, w **której**, a następnie kliknij przycisk **Zapisz**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    To zapisze plik CSR w wybranej lokalizacji. Lokalizacja domyślna to na pulpicie. Pamiętaj, że lokalizacji wybrany dla tego pliku.


####<a name="register-your-app-for-push-notifications"></a>Zarejestruj się w aplikacji dla powiadomienia wypychane

Utworzyć nowy identyfikator aplikacji jawne aplikacji z firmy Apple, a także skonfigurować powiadomienia wypychane.  

1. Przejdź do [portalu obsługi systemu iOS](http://go.microsoft.com/fwlink/p/?LinkId=272456) w Centrum deweloperów programu Apple, zaloguj się przy użyciu swojego Identyfikatora Apple, kliknij **identyfikatory**, a następnie kliknij **Identyfikatorów aplikacji**, a na koniec kliknij na **+** Zaloguj się do rejestrowania nowej aplikacji.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Zaktualizuj następujące trzy pola z nowej aplikacji, a następnie kliknij przycisk **Kontynuuj**:

    * **Nazwa**: wpisz opisową nazwę dla aplikacji w polu **Nazwa** w sekcji **Opis identyfikator aplikacji** .

    * **Identyfikator pakietu**: w sekcji **Jawne identyfikator aplikacji** wpisz **Identyfikator pakietu** w formularzu `<Organization Identifier>.<Product Name>` wymienione w [Przewodniku dystrybucji aplikacji](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Nazwa musi być zgodna co to jest również używana w programie project XCode, Xamarin lub Cordova dla aplikacji.

    * **Powiadomienia push**: Sprawdź opcję **Powiadomienia Push** w sekcji **Aplikacji usług** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Na stronie Potwierdzanie ekranie identyfikator aplikacji przejrzyj ustawienia, a po upewnieniu się je kliknij przycisk **Prześlij**

4.  Po przesłaniu nowy identyfikator aplikacji zostanie wyświetlony ekran **rejestracji ukończone** . Kliknij przycisk **Gotowe**.

5. W Centrum deweloperów w obszarze identyfikatory aplikacji Znajdź identyfikator aplikacji, która została właśnie utworzona i kliknij polecenie w danym wierszu. Kliknięcie polecenia w wierszu identyfikator aplikacji zostaną wyświetlone szczegóły aplikacji. Kliknij przycisk **Edytuj** u dołu.

6. Przewiń do dołu ekranu i kliknij przycisk **Utwórz certyfikat** w sekcji **Rozwoju Push certyfikat SSL**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Spowoduje to wyświetlenie Asystenta "Dodaj iOS certyfikatu".

    > [AZURE.NOTE] Ten samouczek z certyfikatem rozwoju. Ten sam proces jest używany podczas rejestrowania certyfikatu produkcji. Wystarczy sprawdzić, czy używać tego samego typu certyfikatu, podczas wysyłania powiadomień.

7. Kliknij pozycję **Wybierz plik**, przejdź do lokalizacji, w której zapisano obsługi dla certyfikatu push. Kliknij pozycję **Generuj**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Po utworzeniu certyfikatu przez portal kliknij przycisk **Pobierz** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    To do pobrania certyfikatu podpisywania i zapisuje go na komputerze w folderze pobrane.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Domyślnie pobrany plik certyfikatu rozwoju nosi nazwę **aps_development.cer**.

9. Kliknij dwukrotnie certyfikat pobrany wypychanych **aps_development.cer**. Spowoduje to zainstalowanie nowego certyfikatu w pęku kluczy, tak jak pokazano poniżej:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] Nazwa Twojej certyfikatu może być różna, ale będzie on poprzedzony **rozwoju firmy Apple iOS Push usług:**.

10. W programie Access Pęk kluczy kliknij prawym przyciskiem myszy nowy certyfikat wypychanych, która została właśnie utworzona w kategorii **Certyfikaty** . Kliknij przycisk **Eksportuj**, nadaj plikowi nazwę, wybierz odpowiedni format **.p12** , a następnie kliknij przycisk **Zapisz**.

    Należy pamiętać, nazwę pliku i lokalizację certyfikatu wypychanych .p12 wyeksportowane. Ma służyć do Włącz uwierzytelnianie za pomocą APN za pomocą przekazania go w portalu klasyczny Azure.



####<a name="create-a-provisioning-profile-for-the-app"></a>Tworzenie profilu obsługi administracyjnej aplikacji

1. Po powrocie do <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">obsługi portalu iOS</a>wybierz **Inicjowania obsługi administracyjnej profilów**, wybierz pozycję **Wszystkie**, a następnie kliknij **+** przycisk, aby utworzyć nowy profil. Zostanie uruchomiony Kreator **Dodaj iOS Provisiong profilu**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Wybierz typ profilu provisiong **iOS opracowywania aplikacji** w fazie **projektowania** i kliknij przycisk **Kontynuuj**.


3. Następnie wybierz identyfikator aplikacji właśnie utworzony na liście rozwijanej **Identyfikator aplikacji** i kliknij przycisk **Kontynuuj**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. Na ekranie **Zaznacz certyfikaty** wybrać certyfikat rozwoju używanymi do podpisywania kodu, a następnie kliknij przycisk **Kontynuuj**. To jest certyfikatu podpisywania nie certyfikatów push właśnie utworzonego.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Następnie wybierz pozycję **urządzenia** do badania i kliknij przycisk **Kontynuuj**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Na koniec wybierz nazwę profilu w polu **Nazwa profilu**, kliknij pozycję **Generuj**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
