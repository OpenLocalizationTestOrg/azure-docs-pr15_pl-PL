

1. Na komputerze Mac Uruchom **Dostęp do pęku kluczy**. Otwórz okno **Moje certyfikaty** w **kategorii** na pasku navigationn po lewej stronie. Znajdź certyfikat SSL pobranego w poprzedniej sekcji i ujawnić jego zawartość. Wybierz tylko certyfikat (nie należy zaznaczać klucz prywatny), a [go wyeksportować](https://support.apple.com/kb/PH20122?locale=en_US).

2. W [portalu Azure](https://portal.azure.com/), kliknij pozycję **Przeglądaj wszystkie** > **Aplikacji usług** > do wewnętrznej bazy danych aplikacji Mobile. W obszarze **Ustawienia**kliknij **Push usługi aplikacji**, a następnie kliknij swoją nazwę Centrum powiadomienie. Przejdź do **Usługi powiadomień Push firmy Apple** > **przekazywanie certyfikatu**. Przekaż plik .p12, wybranie poprawnego **trybu** (w zależności od tego, czy certyfikat klienta SSL z wcześniejszej jest produkcji lub piaskownicy). Zapisz zmiany.

Usługi jest skonfigurowany do pracy z powiadomień wypychanych w systemie iOS!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
