Aplikacji usługi aplikacji dla urządzeń przenośnych używa [Koncentratory powiadomienie] w celu wysłania umieszcza, więc będą konfigurowane koncentratora powiadomienie dla twojej aplikacji dla urządzeń przenośnych.

1. W [Azure Portal], przejdź do **przeglądania** > **Usług aplikacji**, następnie kliknij pozycję do wewnętrznej bazy danych aplikacji. W obszarze **Ustawienia**kliknij pozycję **Push usługi aplikacji**.

2. Kliknij przycisk **Konfiguruj** , aby skonfigurować Centrum powiadomienie. Można utworzyć koncentratora lub łączenie z istniejącą.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Teraz masz połączenie koncentratora powiadomień do wewnętrznej bazy danych aplikacji Mobile. Później skonfigurujesz tego Centrum powiadomienie nawiązywania połączenia z systemem powiadamiania platformy (PNS), aby przekazać do urządzenia.

[Azure Portal]: https://portal.azure.com/
[Powiadomienie o koncentratory]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/