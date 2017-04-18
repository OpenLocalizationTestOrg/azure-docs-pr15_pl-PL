Azure klientów można odblokować 25 000 bezpłatnych wiadomości e-mail każdego miesiąca. Te 25 000 bezpłatne miesięczny wiadomości e-mail będą umożliwiają dostęp do zaawansowanych raportów i analizy i [wszystkich interfejsów API][] (sieci Web, SMTP, zdarzeń, analizy i inne). Dla informacji na temat dodatkowych usług udostępnionych przez SendGrid zobacz stronę [SendGrid funkcje][] .

### <a name="to-sign-up-for-a-sendgrid-account"></a>Aby utworzyć konto SendGrid

1. Zaloguj się do [portalu zarządzania Azure][].

2. W dolnym okienku portalu zarządzania kliknij przycisk **Nowy**.

    ![polecenie Pasek — nowy][command-bar-new]

3. Kliknij pozycję **Marketplace**.

    ![Magazyn sendgrid][sendgrid-store]

4. W oknie dialogowym **Wybieranie aplikacji i usług** wybierz pozycję **SendGrid** , a następnie kliknij przycisk strzałki w prawo.

5. W oknie dialogowym **Personalizowanie aplikacji i usług** wybierz plan **SendGrid** , do którego chcesz uzyskać dostęp do.

6. Wprowadź nazwę identyfikującą usługi **SendGrid** w ustawieniach Azure, lub użyj wartości domyślnej **SendGridEmailDelivery.Simplified.SMTPWebAPI**. Nazwy musi należeć do przedziału od 1 do 100 znaków i zawierają tylko alfanumerycznych, kreski, kropki i znaki podkreślenia. Nazwa musi być unikatowa na liście elementów subskrypcją Sklepu Azure.

    ![Magazyn ekran 2][store-screen-2]

7. Wybierz wartość dla danego regionu; na przykład zachód US.

8. Kliknij strzałkę w prawo.

9. Na karcie **Przeglądanie zakupu** Przegląd planu i informacje o cenach i przejrzyj warunki prawne. Jeśli akceptujesz warunków, kliknij znacznik wyboru. Po kliknięciu przycisku znacznik wyboru konta SendGrid rozpocznie [proces inicjowania obsługi administracyjnej SendGrid].

    ![Magazyn ekranu-3][store-screen-3]

10. Po potwierdzeniu zakup nastąpi przekierowanie do pulpitu nawigacyjnego dodatki i zostanie wyświetlony komunikat **SendGrid zakupu**.

    ![sendgrid zakupieniu wiadomości][sendgrid-purchasing-message]

    Konta SendGrid jest zainicjowano obsługę administracyjną natychmiast i zostanie wyświetlony komunikat **zakupione pomyślnie SendGrid dodatek**. Teraz są tworzone swoje konto i poświadczenia. Możesz przystąpić do wysyłania wiadomości e-mail na tym etapie. 

    Aby zmodyfikować planu subskrypcji lub zobacz SendGrid, skontaktuj się z ustawień, kliknij nazwę tej usługi SendGrid, aby otworzyć SendGrid Marketplace pulpitu nawigacyjnego. 

    ![sendgrid — dodawanie na-pulpitu nawigacyjnego][sendgrid-add-on-dashboard]

    Aby wysłać wiadomości e-mail za pomocą SendGrid, musisz podać poświadczenia konta (nazwy użytkownika i hasła).

### <a name="to-find-your-sendgrid-credentials"></a>Aby znaleźć poświadczenia SendGrid ###

1. Kliknij pozycję **informacje o połączeniu**.

    ![informacje przycisk, sendgrid połączenie w-][sendgrid-connection-info-button]

2. W oknie dialogowym *informacje o połączeniu* Skopiuj **hasło** i nazwa użytkownika, aby użyć w dalszej części tego samouczka.

    ![sendgrid informacji o połączeniu][sendgrid-connection-info]

    Aby ustawić deliverability ustawienia poczty e-mail, kliknij przycisk **Zarządzaj** . Nastąpi przekierowanie do SendGrid Panel sterowania. 

    ![sendgrid – panel sterowania][sendgrid-control-panel]

    Aby uzyskać więcej informacji na wprowadzenie SendGrid zobacz [SendGrid wprowadzenie][].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[Funkcje SendGrid]: http://sendgrid.com/features
[Portal Azure zarządzania]: https://manage.windowsazure.com
[Wprowadzenie do SendGrid]: http://sendgrid.com/docs
[Proces inicjowania obsługi administracyjnej SendGrid]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[wszystkich interfejsów API]: https://sendgrid.com/docs/API_Reference/index.html

