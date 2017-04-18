<properties
    pageTitle="Konfigurowanie Weryfikacja dwuetapowa konta służbowego"
    description="Gdy firmy konfiguruje uwierzytelnianie wieloskładnikowe Azure, pojawi do utworzenia konta na potrzeby weryfikacji dwuetapowej. Dowiedz się, jak go skonfigurować. "
    services="multi-factor-authentication"
    keywords="jak używać katalogu azure, usługi active directory w chmurze, samouczek usługi active directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Konfigurowanie konta na potrzeby weryfikacji dwuetapowej

Weryfikacja dwuetapowa jest na etapie zwiększenia bezpieczeństwa, które pomagają chronić swoje konto, zwiększając trudniejsze dla innych osób przerwać w. Jeśli czytasz w tym artykule, prawdopodobnie masz wiadomości e-mail od administratora służbowego o uwierzytelnianie wieloskładnikowe. Lub być może próbowano zalogować się i masz komunikat z pytaniem, możesz skonfigurować dodatkowe Weryfikacja. Jeśli jest to wielkość liter, **nie możesz zalogować się przed ukończeniem procesu automatycznego rejestrowania**.

Ten artykuł ułatwia konfigurowanie **konto służbowe**. Jeśli chcesz włączyć Weryfikacja dwuetapowa dla własny, osobistego konta Microsoft, zobacz [temat Weryfikacja dwuetapowa](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Określić, jak będzie używał uwierzytelnianie wieloskładnikowe

Weryfikacja dwuetapowa polega na monit dla dwóch rodzajów identyfikacji podczas logowania się. Najpierw prosimy o podanie nazwy użytkownika i hasła w zwykły sposób. Następnie możemy kontaktów, które telefonu, który jest znamy należy do Ciebie i potwierdzić, że próba logowania godny zaufania.  

Aby rozpocząć pracę z oprogramowaniem, spróbuj zalogować się do swojego konta, tak jak zwykle. Jeśli administrator skonfigurował konta na potrzeby weryfikacji dwuetapowej, zostanie wyświetlony monit o rozpocząć proces automatycznego rejestrowania. Rozpocząć ten proces, klikając pozycję **teraz skonfigurować.**

![Konfiguracja](./media/multi-factor-authentication-end-user-first-time/first.png)

Pierwsze pytanie w trakcie procesu rejestrowania ma jak kontakt z nim. Zapoznaj się z opcjom w tabeli, a następnie przejdź do etapów konfiguracji dla każdej metody za pomocą łącza.

| Metoda kontaktu | Opis |
| --- | --- |
[Aplikacji dla urządzeń przenośnych](#use-a-mobile-app-as-the-contact-method) | - **Otrzymywać powiadomienia w celu weryfikacji.** Ta opcja umieszcza powiadomienie do uwierzytelnienia aplikacji na telefonie smartphone lub tablecie. Wyświetlanie powiadomień, a jeśli jest godny zaufania, zaznacz **uwierzytelniania** w aplikacji. Swojej pracy lub szkole może wymagać wprowadzić kod PIN przed uwierzytelniania.<br>- **Za pomocą kodu weryfikacyjnego.** W tym trybie aplikacji uwierzytelnienia generuje kod weryfikacyjny, aktualizujące co 30 sekund. Wprowadź kod weryfikacyjny najnowsze interfejsu logowania.<br>Aplikacja Microsoft Authenticator jest dostępna dla [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Telefon komórkowy, połączenia lub tekstu](#use-your-mobile-phone-as-the-contact-method) | - **Rozmowa telefoniczna** umieści automatyczną rozmowy numer telefonu, podane. Odbierz połączenie, a następnie naciśnij klawisz # na klawiaturze telefonu do uwierzytelnienia.<br>- **Wiadomość tekstowa** kończy się wiadomość tekstowa zawierających kod weryfikacyjny. Po wyświetleniu monitu w tekście odpowiadania na wiadomość lub podaj kod weryfikacyjny Podany w interfejsie logowania. |  
[Rozmowa telefoniczna pakietu Office](#use-your-office-phone-as-the-contact-method) | Umieszcza automatyczną rozmowy numer telefonu, podane. Odebranie połączenia i naciska # na klawiaturze telefonu do uwierzytelnienia. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Używanie aplikacji dla urządzeń przenośnych jako metody kontaktu

Ta metoda wymaga zainstalować aplikację uwierzytelnienia na telefonie lub tablecie. Kroki opisane w tym artykule są oparte na aplikacji Microsoft Authenticator jest dostępna dla [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Z listy rozwijanej wybierz **aplikacji dla urządzeń przenośnych** .
2. Zaznacz **otrzymywać powiadomienia w celu weryfikacji** lub **kod weryfikacyjny Użyj**, a następnie wybierz pozycję **Ustawienia**.

    ![Dodatkowe zabezpieczenia weryfikacji ekranu](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Na telefonie lub tablecie, Otwórz aplikację i wybierz pozycję **+** w celu dodania konta. (Na urządzeniach z systemem Android, zaznacz trzy kropki, a następnie **Dodaj konto**).
4. Określ, czy chcesz dodać konto służbowe. Zostanie wyświetlona skanera kod QR na telefonie. Jeśli kamera nie działa poprawnie, możesz wybrać ręczne wprowadzanie informacji o firmie. Aby uzyskać więcej informacji zobacz [Dodawanie konta ręcznie](#add-an-account-manually).  
5. Skanowanie obrazów kod QR, wyświetloną z ekranu do konfigurowania aplikacji dla urządzeń przenośnych.  Wybierz pozycję **Gotowe** , aby zamknąć ekran kod QR.  

    ![Ekran kod QR](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Po zakończeniu aktywacji przez telefon, zaznacz **kontakt ja**.  Ten krok wyśle odbiorcy powiadomienie lub kod weryfikacyjny telefonu. Wybierz pozycję **Sprawdź**.  
7. Jeśli firma wymaga numeru PIN zatwierdzania weryfikacji logowania, wprowadź go.

    ![Pole służące do wprowadzania numeru PIN](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Po zakończeniu wprowadzania numeru PIN, wybierz pozycję **Zamknij**. W tym momencie weryfikacji powinien zakończyć się pomyślnie.
9. Zalecamy, wprowadź swój numer telefonu komórkowego, w przypadku, gdy utracisz dostęp do Twojej aplikacji dla urządzeń przenośnych. Określ swój kraj z listy rozwijanej, a następnie wprowadź numer swojego telefonu komórkowego w polu obok nazwy kraju. Wybierz przycisk **Dalej**.
10. W tym momencie zostanie wyświetlony monit, aby skonfigurować hasła aplikacji dla aplikacji innych niż przeglądarki, takich jak program Outlook 2010 lub starszym lub aplikacji macierzystych poczty e-mail na urządzeń firmy Apple. Jest tak, ponieważ niektóre aplikacje nie obsługują Weryfikacja dwuetapowa. Jeśli te aplikacje nie jest używana, kliknij przycisk **Gotowe** i pominąć pozostałe kroki.
11. Jeśli używasz te aplikacje kopiowanie hasło aplikacji opisane i wkleić go w aplikacji zamiast zwykłego hasło. W przypadku wielu aplikacji można używać tego samego hasła aplikacji. Aby uzyskać więcej informacji [pomocy hasła aplikacji].
12. Kliknij przycisk **Gotowe**.


### <a name="add-an-account-manually"></a>Ręczne dodawanie konta
Jeśli chcesz dodać konto do aplikacji dla urządzeń przenośnych ręcznie, zamiast korzystania z czytnika QR, wykonaj następujące kroki.

1. Wybierz przycisk **Ręczne wprowadzanie konta** .  
2. Wprowadź kod i adres URL, który znajdują się na tej samej stronie, pokazujący kodu kreskowego. Te informacje o przejdzie w polach **Kod** i **adres URL** w aplikacji dla urządzeń przenośnych.

    ![Konfiguracja](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Po zakończeniu aktywacji, zaznacz **kontakt ja**. W tym kroku wyśle odbiorcy powiadomienie lub kod weryfikacyjny telefonu. Wybierz pozycję **Sprawdź**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Za pomocą telefonu komórkowego jako metody kontaktu

1. Z listy rozwijanej wybierz **Telefonu uwierzytelniania** .  

    ![Konfiguracja](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Wybierz swój kraj z listy rozwijanej, a następnie wprowadź numer swojego telefonu komórkowego.
3. Wybierz metodę, którą chcesz za pomocą telefonu komórkowego — tekstu lub połączenia.
4. Zaznacz **kontakt ja** do weryfikacji numeru telefonu. W zależności od wybrany tryb możemy wysłać tekst lub połączenie. Postępuj zgodnie z instrukcjami na ekranie, a następnie wybierz pozycję **Weryfikuj**.
5. W tym momencie zostanie wyświetlony monit, aby skonfigurować hasła aplikacji dla aplikacji innych niż przeglądarki, takich jak program Outlook 2010 lub starszym lub aplikacji macierzystych poczty e-mail na urządzeń firmy Apple. Jest tak, ponieważ niektóre aplikacje nie obsługują Weryfikacja dwuetapowa. Jeśli te aplikacje nie jest używana, kliknij przycisk **Gotowe** i pominąć pozostałe kroki.
6. Jeśli używasz te aplikacje kopiowanie hasło aplikacji opisane i wkleić go w aplikacji zamiast zwykłego hasło. W przypadku wielu aplikacji można używać tego samego hasła aplikacji. Aby uzyskać więcej informacji [pomocy hasła aplikacji].
7. Kliknij przycisk **Gotowe**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Za pomocą telefonu pakietu office jako metody kontaktu

1. Z listy rozwijanej wybierz **Telefon w biurze**  

    ![Konfiguracja](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Pole numer telefonu jest automatycznie wypełniane swoje informacje kontaktowe firmy. Jeśli liczba jest nieprawidłowe lub brakuje, poproś administratora, aby wprowadzić zmiany.
4. Zaznacz **kontakt ja** do weryfikacji numeru telefonu i będzie nazywamy numeru. Postępuj zgodnie z instrukcjami na ekranie, a następnie wybierz pozycję **Weryfikuj**.
5. W tym momencie zostanie wyświetlony monit, aby skonfigurować hasła aplikacji dla aplikacji innych niż przeglądarki, takich jak program Outlook 2010 lub starszym lub aplikacji macierzystych poczty e-mail na urządzeń firmy Apple. Jest tak, ponieważ niektóre aplikacje nie obsługują Weryfikacja dwuetapowa. Jeśli te aplikacje nie jest używana, kliknij przycisk **Gotowe** i pominąć pozostałe kroki.
6. Jeśli używasz te aplikacje kopiowanie hasło aplikacji opisane i wkleić go w aplikacji zamiast zwykłego hasło. Za pomocą tego samego hasła aplikacji dla wielu aplikacji. Aby uzyskać więcej informacji zobacz [Co to są hasła aplikacji](multi-factor-authentication-end-user-app-passwords.md).
7. Kliknij przycisk **Gotowe**.

## <a name="next-steps"></a>Następne kroki

- Zmienianie opcji i [zarządzać ustawieniami na potrzeby weryfikacji dwuetapowej](multi-factor-authentication-end-user-manage-settings.md)
- Konfigurowanie [hasła aplikacji](multi-factor-authentication-end-user-app-passwords.md) urządzenie macierzyste aplikacje, które nie obsługują Weryfikacja dwuetapowa.
- Zapoznaj się z [aplikacji Microsoft uwierzytelnienia](multi-factor-authentication-microsoft-authenticator.md) uwierzytelniania szybko i bezpiecznie, nawet wtedy, gdy nie masz usługi komórki.
