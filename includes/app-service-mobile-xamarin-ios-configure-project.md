####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Konfigurowanie projektu iOS w Xamarin Studio

1. W Xamarin.Studio Otwórz **Info.plist**i aktualizowanie **Identyfikator pakietu** o identyfikatorze pakietu utworzonego wcześniej ze swojego nowego identyfikatora aplikacji.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Przewiń w dół do **Trybów tła** i zaznacz pole **Włącz trybów tła** i pole **zdalnych powiadomień** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Kliknij dwukrotnie projektu w panelu rozwiązania, aby otworzyć **Opcje programu Project**.

4.  **IOS podpisywania pakietu** w obszarze **Tworzenie**i wybierz przycisk odpowiednich **tożsamości** i **obsługi profilu** , po prostu skonfigurowano dla tego projektu. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Dzięki temu, że projekt używa nowego profilu dla podpisywanie kodu. Oficjalne urządzenia Xamarin inicjowania obsługi administracyjnej dokumentacji zobacz [Udostępnianie urządzeń Xamarin].

####<a name="configuring-the-ios-project-in-visual-studio"></a>Konfigurowanie projektu iOS w programie Visual Studio

1. W programie Visual Studio kliknij prawym przyciskiem myszy projektu, a następnie kliknij polecenie **Właściwości**.

2. Na stronach właściwości kliknij kartę **iOS aplikacji** i zaktualizować **identyfikatora** ID, który został utworzony wcześniej.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Na karcie **iOS podpisywania pakietu** zaznacz odpowiednie **tożsamości** i **obsługi profilu** , po prostu skonfigurowano dla tego projektu. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dzięki temu, że projekt używa nowego profilu dla podpisywanie kodu. Oficjalne urządzenia Xamarin inicjowania obsługi administracyjnej dokumentacji zobacz [Udostępnianie urządzeń Xamarin].

4. Kliknij dwukrotnie Info.plist, aby go otworzyć, a następnie włączyć **RemoteNotifications** w trybach tła. 



[Urządzenie Xamarin inicjowania obsługi administracyjnej]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/