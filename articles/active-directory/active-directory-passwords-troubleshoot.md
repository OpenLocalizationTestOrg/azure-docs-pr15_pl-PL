<properties
    pageTitle="Rozwiązywanie problemów: Zarządzanie Azure AD hasło | Microsoft Azure"
    description="Rozwiązywanie typowych problemów z etapów zarządzania Azure AD hasła, w tym Resetuj, zmienianie, zapisu, rejestracji, i jakie informacje mają być dołączone podczas szukasz pomocy."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Jak rozwiązywać problemy Zarządzanie hasłami

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

Jeśli występują problemy z usługą Zarządzanie hasła, tutaj znajdziesz pomoc. Większość problemów, które mogą pojawić się będzie możliwe z kilku prostych krokach rozwiązywania problemów, które zapoznać się poniżej rozwiązywać rozmieszczenia:

* [**Informacje, które mają być uwzględniane podczas potrzebujesz pomocy**](#information-to-include-when-you-need-help)
* [**Problemy z konfiguracją Zarządzanie hasłami w portalu zarządzania Azure**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Problemy z raportami zarządzania hasła w portalu zarządzania Azure**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Portal rejestracji Resetowanie problemów przy użyciu hasła**](#troubleshoot-the-password-reset-registration-portal)
* [**Problemy z hasłem Resetowanie portalu**](#troubleshoot-the-password-reset-portal)
* [**Problemy z zapisu hasła**](#troubleshoot-password-writeback)
  - [Kody błędów hasło zapisu dziennika zdarzeń](#password-writeback-event-log-error-codes)
  - [Problemy z łącznością zapisu hasła](#troubleshoot-password-writeback-connectivity)

Jeśli po wypróbowaniu poniższe kroki rozwiązywania problemów i nadal występowania problemów, można Zadaj pytanie na [forum Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) lub kontakt z pomocą techniczną i jak możemy zostaną wykonane przyjrzeć się problemu.

## <a name="information-to-include-when-you-need-help"></a>Informacje, które mają być uwzględniane podczas jest potrzebna pomoc

Jeśli nie rozwiąże problemu z poniższych porad, można skontaktować się naszych pracowników pomocy technicznej. Należy skontaktować się z nim zaleca zawiera następujące informacje:

 - **Ogólny opis błędu** — komunikat z jakiego dokładnie, czy użytkownik Zobacz?  Jeśli wystąpił żaden komunikat o błędzie, opisz nieoczekiwane działanie, które zauważysz, szczegóły.
 - **Strony** — które strony zostały na kiedy został wyświetlony komunikat o błędzie (Dołącz adres URL)?
 - **Data / Godzina / strefa czasowa** — jaki był dokładną datę i godzinę został wyświetlony komunikat o błędzie (obejmuje strefy czasowej)?
 - **Kod pomocy technicznej** — jaki był kod pomocy technicznej generowane, gdy zostanie wyświetlony błąd (to znaleźć, Odtwórz błąd, a następnie kliknij łącze obsługuje kod w dolnej części ekranu i wysyłanie z pracownikiem pomocy technicznej identyfikator GUID, którego wynikiem).
   - Jeśli jesteś na stronie bez używania kodu pomocy technicznej, u dołu, naciśnij klawisz F12 i wyszukiwanie SID i CID i przesuń tych dwóch wyników z pracownikiem pomocy technicznej.

    ![][001]

 - **Identyfikator użytkownika** — jaki był identyfikator użytkownika, który jest wyświetlony komunikat o błędzie (np.user@contoso.com)?
 - **Informacje o użytkowniku** — był użytkownika Federacji, zsynchronizowany, skrótu hasła tylko w chmurze?  Czy użytkownik ma przypisana licencja AAD Premium lub AAD podstawowe?
 - **Dziennik zdarzeń aplikacji** — Jeśli używasz zwrotne hasło i komunikat o błędzie jest w infrastrukturze lokalnych, należy zip w górę kopię w dzienniku zdarzeń aplikacji na serwerze narzędzie Azure AD Connect i wysyłanie wraz z Twoją prośbę.

W tym te informacje pomogą nam rozwiązać problem tak szybko, jak to możliwe.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Rozwiązywanie problemów z konfiguracji resetowania hasła w portalu zarządzania Azure
Jeśli wystąpi błąd podczas resetowania hasła konfigurowania, można je rozwiązać, wykonując poniższe kroki rozwiązywania problemów:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Błąd sprawy</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Jaki błąd czy użytkownika są widoczne?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rozwiązanie</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nie widzę sekcji <strong>Resetowanie hasła użytkownika </strong>na karcie <strong>Konfiguruj</strong> w portalu zarządzania Azure</p>
            </td>
            <td>
              <p>Sekcja <strong>Resetowanie hasła użytkownika </strong>nie jest widoczne na karcie <strong>Konfiguruj</strong> w portalu zarządzania Azure.</p>
            </td>
            <td>
              <p>To może wystąpić, jeśli nie masz licencję AAD Premium lub AAD podstawowe przypisane do administratora tej operacji. </p>
              <p>Aby rozwiązać ten problem, przypisywanie licencji AAD Premium lub AAD podstawowe do danego konta administratora, przechodząc na kartę <strong>licencje</strong> i spróbuj ponownie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Opcje konfiguracji w sekcji <strong>Resetowanie hasła użytkownika</strong> opisane w dokumentacji nie jest wyświetlane.</p>
            </td>
            <td>
              <p>W sekcji <strong>Resetowanie hasła użytkownika </strong>jest widoczny, ale tylko flagi, który pojawia się pod nim jest <strong>Włączona funkcja resetowania hasła użytkowników</strong> .</p>
            </td>
            <td>
              <p>Reszta interfejsu użytkownika pojawi się po przełączeniu flagę <strong>Użytkownicy dostęp do resetowania hasła</strong> , aby <strong>tak.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Opcja określonego konfiguracji nie jest wyświetlane.</p>
            </td>
            <td>
              <p>Na przykład nie widzę opcji <strong>Liczba dni przed użytkownik musi potwierdzić swoje dane kontaktowe</strong> podczas przewijania do sekcji <strong>Resetowanie hasła użytkownika</strong> (lub inne przykłady ten sam problem).</p>
            </td>
            <td>
              <p>Wiele elementów interfejsu użytkownika są ukryte, dopóki są potrzebne. Spróbuj włączanie wszystkich opcji na stronie, jeśli chcesz wyświetlić.</p>
              <p>Aby uzyskać więcej informacji o wszystkich kontrolek, które są dla Ciebie dostępne, zobacz <a href="active-directory-passwords-customize.md#password-management-behavior">Zarządzanie hasłami zachowanie</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nie widzę opcji Konfiguracja <strong>Napisać ponownie hasła do lokalnego</strong></p>
            </td>
            <td>
              <p>Opcja <strong>Napisać ponownie hasła do lokalnego</strong> nie jest widoczne na karcie <strong>Konfiguruj</strong> w portalu zarządzania Azure</p>
            </td>
            <td>
              <p>Ta opcja jest widoczna tylko, jeśli zostały pobrane narzędzie Azure AD Connect i skonfigurowany zapisu hasła. Po wykonaniu tej opcję, która pojawia się i umożliwia włączenie lub wyłączenie zapisu z chmury.</p>
              <p>Aby uzyskać więcej informacji o tym, jak to zrobić, zobacz <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Włączanie wystornowania hasła w narzędzie Azure AD Connect</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Rozwiązywanie problemów z raportami zarządzania hasła w portalu zarządzania Azure
W przypadku wystąpienia błędu korzystanie z raportów zarządzania hasła, można je rozwiązać, wykonując poniższe kroki rozwiązywania problemów:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Błąd sprawy</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Jakie błędów czy użytkownika są widoczne?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rozwiązanie</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nie widzę zgłoszenia zarządzania hasła</p>
            </td>
            <td>
              <p>Raporty <strong>aktywności do resetowania hasła</strong> i <strong>rejestracji aktywności do resetowania hasła</strong> nie są widoczne w obszarze Raporty <strong>Dziennika aktywności</strong> na karcie <strong>Raporty</strong> .</p>
            </td>
            <td>
              <p>To może wystąpić, jeśli nie masz licencję AAD Premium lub AAD podstawowe przypisane do administratora tej operacji. </p>
              <p>Aby rozwiązać ten problem, przypisywanie licencji AAD Premium lub AAD podstawowe do danego konta administratora, przechodząc na kartę <strong>licencje</strong> i spróbuj ponownie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Wielokrotne wyświetlanie rejestracji użytkownika</p>
            </td>
            <td>
              <p>Gdy użytkownik rejestruje alternatywny adres e-mail, telefonu komórkowego i pytania dotyczące zabezpieczeń, każdego pojawią się jako oddzielne wiersze, a nie jednego wiersza.</p>
            </td>
            <td>
              <p>Obecnie gdy rejestruje użytkownika, firma Microsoft nie przyjęto założenie, o będzie zarejestrowanie wszystkich obecnych na stronie rejestracji. W wyniku Pracujemy obecnie Zaloguj każda poszczególnych danych, który jest zarejestrowany jako osobne zdarzenia.</p>
              <p>Jeśli chcesz zagregować te dane, możesz pobrać raport i otwórz danych, jak w tabeli przestawnej programu excel do bardziej elastyczne.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Rozwiązywanie problemów z portalem rejestracji resetowania hasła
Jeśli wystąpi błąd podczas rejestrowania dla Resetowanie hasła użytkownika, można je rozwiązać, wykonując poniższe kroki rozwiązywania problemów:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Błąd sprawy</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Jakie błędów czy użytkownika są widoczne?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rozwiązanie</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Katalog nie jest włączona dla resetowania hasła</p>
            </td>
            <td>
              <p>Administrator nie włączył do korzystania z tej funkcji.</p>
            </td>
            <td>
              <p>Przełączanie flagę <strong>Włączona funkcja resetowania hasła użytkowników</strong> <strong>Tak</strong> i trafień <strong>zapisać</strong> na karcie portalu zarządzania Azure katalogu konfiguracji. Musi być Azure AD Premium lub podstawowe licencji przypisanej do administratora tej operacji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Użytkownik nie ma przypisana licencja AAD Premium lub AAD podstawowe</p>
            </td>
            <td>
              <p>Administrator nie włączył do korzystania z tej funkcji.</p>
            </td>
            <td>
              <p>Przypisywanie licencji Azure AD Premium lub Azure AD podstawowe użytkownikowi na karcie <strong>licencji</strong> w portalu zarządzania Azure. Musi być Azure AD Premium lub podstawowe licencji przypisanej do administratora tej operacji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Błąd podczas przetwarzania żądania</p>
            </td>
            <td>
              <p>Użytkownik widzi komunikat o błędzie z informacją:</p>
              <p>

              </p>
              <p>Błąd podczas przetwarzania żądania </p>
              <p>Podczas próby resetowania hasła.</p>
            </td>
            <td>
              <p>Może to być spowodowane problemami związanymi z wielu, ale ogólnie ten błąd jest powodowany przez albo to usługa awarii lub konfiguracji problem, który nie jest możliwe. </p>
              <p>Jeśli zostanie wyświetlony ten błąd, i jest wpływające na ochronę firmy, skontaktuj się z obsługą i firma Microsoft może pomóc JNW. Zobacz <a href="#information-to-include-when-you-need-help">informacje mają być uwzględniane podczas potrzebujesz pomocy</a> , aby zobaczyć, jakie należy podać do pracownika pomocy technicznej, aby ułatwić szybkie rozstrzygnięcie.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Rozwiązywanie problemów z portalem resetowania hasła
Jeśli wystąpi błąd podczas resetowania hasła dla użytkownika, można je rozwiązać, wykonując poniższe kroki rozwiązywania problemów:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Błąd sprawy</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Jaki błąd czy użytkownika są widoczne?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rozwiązanie</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Katalog nie jest włączona dla resetowania hasła</p>
            </td>
            <td>
              <p>Twoje konto nie jest włączona dla resetowania hasła</p>
              <p>Niestety, ale administrator nie skonfigurował konta do użytku z tej usługi. </p>
              <p>

              </p>
              <p>Jeśli chcesz, możemy skontaktuj się z administratorem w organizacji o zresetowanie hasła dla Ciebie.</p>
            </td>
            <td>
              <p>Przełączanie flagę <strong>Włączona funkcja resetowania hasła użytkowników</strong> <strong>Tak</strong> i trafień <strong>zapisać</strong> na karcie portalu zarządzania Azure katalogu konfiguracji. Musi być Azure AD Premium lub podstawowe licencji przypisanej do administratora tej operacji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Użytkownik nie ma przypisana licencja AAD Premium lub AAD podstawowe</p>
            </td>
            <td>
              <p>Podczas nie możemy resetowania hasła konta niebędących administratorami automatycznie, możemy skontaktować się administrator organizacji, aby zrobił to za Ciebie.</p>
            </td>
            <td>
              <p>Przypisywanie licencji Azure AD Premium lub Azure AD podstawowe użytkownikowi na karcie <strong>licencji</strong> w portalu zarządzania Azure. Musi być Azure AD Premium lub podstawowe licencji przypisanej do administratora tej operacji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Katalog została włączona funkcja resetowania hasła, ale użytkownik ma brakujące lub nieprawidłową postać informacje uwierzytelniające</p>
            </td>
            <td>
              <p>Twoje konto nie jest włączona dla resetowania hasła</p>
              <p>Niestety, ale administrator nie skonfigurował konta do użytku z tej usługi. </p>
              <p>

              </p>
              <p>Jeśli chcesz, możemy skontaktuj się z administratorem w organizacji o zresetowanie hasła dla Ciebie.</p>
            </td>
            <td>
              <p>Upewnij się, że użytkownik prawidłowo nawiązała dane kontaktowe w pliku w katalogu przed kontynuowaniem. Aby uzyskać informacje dotyczące konfigurowania informacje uwierzytelniające w katalogu, tak aby użytkownicy nie widzą ten błąd, zobacz <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">dane, które są używane przez resetowania hasła</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Katalog została włączona funkcja resetowania hasła, ale użytkownik ma tylko jednego elementu danych kontaktów na plik po zasady są ustawione na wymagają dwa kroki weryfikacji</p>
            </td>
            <td>
              <p>Twoje konto nie jest włączona dla resetowania hasła</p>
              <p>Niestety, ale administrator nie skonfigurował konta do użytku z tej usługi. </p>
              <p>

              </p>
              <p>Jeśli chcesz, możemy skontaktuj się z administratorem w organizacji o zresetowanie hasła dla Ciebie.</p>
            </td>
            <td>
              <p>Upewnij się, że użytkownik ma co najmniej dwa poprawnie skonfigurowanego metody kontaktu (np telefonu komórkowego i telefon w biurze) przed kontynuowaniem. Aby uzyskać informacje dotyczące konfigurowania informacje uwierzytelniające w katalogu, tak aby użytkownicy nie widzą ten błąd, zobacz <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">dane, które są używane przez resetowania hasła</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Katalog została włączona funkcja resetowania hasła i użytkownik jest poprawnie skonfigurowany, ale użytkownik nie może się z Tobą kontaktować </p>
            </td>
            <td>
              <p>Ojej!  Wystąpił nieoczekiwany błąd podczas kontaktowania się z Tobą.</p>
            </td>
            <td>
              <p>Może to być wynikiem błędów tymczasowej usługi lub niepoprawnie skonfigurowane dane kontaktowe, że firma Microsoft nie może wykryć prawidłowo. Jeśli użytkownik czeka 10 sekund, spróbuj ponownie i pojawi się łącze "Skontaktuj się z administratorem". Klikając ponownie spróbuj wyśle ponownie połączenia, dlatego kliknięcie "Skontaktuj się z administratorem" wyśle wiadomość e-mail z formularza do użytkownika, hasło lub administrator globalny (w tej kolejności pierwszeństwa) żąda resetowania do wykonania dla tego konta użytkownika hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Użytkownik nigdy nie odbiera wiadomości SMS w przypadku resetowania hasła lub rozmowy telefonicznej</p>
            </td>
            <td>
              <p>Użytkownik klika przycisk "tekst mnie" lub "Zadzwoń do mnie", a następnie nigdy nie odbiera niczego.</p>
            </td>
            <td>
              <p>Może to być spowodowane numeru telefonu nieprawidłową postać w katalogu. Upewnij się, numer telefonu jest w formacie "+ ccc xxxyyyzzzzXeeee". Aby dowiedzieć się więcej o formatowaniu telefonu numery do użytku z Resetowanie hasła się, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">jakie dane są używane przez resetowania hasła</a>.</p>
              <p>Jeśli jest wymagane rozszerzenie kierowane do danego użytkownika, należy zauważyć, w tym przypadku resetowania hasła nie obsługuje rozszerzenia, nawet jeśli użytkownik określi w katalogu (ich są usuwane połączenie jest wysłane). Spróbuj użyć liczby bez rozszerzenia lub integracji z rozszerzeniem numeru telefonu w PBX użytkownika.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Użytkownik nigdy nie odbiera wiadomości e-mail resetowania hasła</p>
            </td>
            <td>
              <p>Użytkownik kliknie przycisk "e-mailem", a następnie nigdy nie odbiera wszystko.</p>
            </td>
            <td>
              <p>Najbardziej typowych przyczyn tego problemu jest to, że wiadomość jest odrzucona przez filtr wiadomości-śmieci. Zaznacz konto wiadomości-śmieci, folderu wiadomości-śmieci lub usunięte elementy wiadomości e-mail.</p>
              <p>Upewnij się również sprawdzania prawo poczty e-mail wiadomości... wielu osobom masz bardzo podobne adresy e-mail i trafiać sprawdzanie złej Skrzynce odbiorczej wiadomości. Jeśli żadna z tych opcji, jest również możliwe, że adres e-mail w katalogu jest nieprawidłowo, sprawdź, upewnij się, że adres e-mail jest właściwą domenę i spróbuj ponownie. Aby dowiedzieć się więcej o formatowaniu wiadomości e-mail adresów do użytku z Resetowanie hasła Zobacz, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">jakie dane są używane przez resetowania hasła</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Można ustawić zasady resetowania hasła, ale gdy konta administratora jest używany w przypadku resetowania hasła, że zasady nie są stosowane</p>
            </td>
            <td>
              <p>Resetowanie haseł kont administratorów, zobacz te same opcje dostęp do resetowania hasła, poczty e-mail oraz telefonu komórkowego, niezależnie od tego, jakie zasady są ustawione na karcie <strong>Konfigurowanie</strong> , w sekcji <strong>Resetowanie hasła użytkownika</strong> .</p>
            </td>
            <td>
              <p>Opcje w sekcji <strong>Resetowanie hasła użytkownika</strong> na karcie <strong>Konfigurowanie</strong> mają zastosowanie tylko do użytkowników końcowych w organizacji.</p>
              <p>Microsoft zarządza i resetowania hasła administratora kontrolek zasad w celu zapewnienia najwyższego poziomu zabezpieczeń</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Uniemożliwia próby resetowania zbyt wiele razy w ciągu dnia hasła użytkownika</p>
            </td>
            <td>
              <p>Użytkownik widzi o błędzie z informacją:</p>
              <p>

              </p>
              <p>Użyj innej opcji.</p>
              <p>Próbowano Sprawdź swoje konto zbyt wiele razy w ostatniej 1 godz. Ze względów bezpieczeństwa będą musiały czekać 24 godzin przed możesz spróbować ponownie. </p>
              <p>Jeśli chcesz, możemy skontaktuj się z administratorem w organizacji o zresetowanie hasła dla Ciebie.</p>
            </td>
            <td>
              <p>Firma Microsoft zaimplementować mechanizm automatycznego ograniczania użytkownikom blok z próby resetowania hasła zbyt wiele razy w krótkim czasie. Dzieje się tak po:</p>
              <ol class="ordered">
                <li>
Użytkownik będzie próbował sprawdzić poprawność numeru telefonu 5 razy w ciągu godziny.<br\><br\></li>
                <li>
Użytkownik będzie próbował używać bramy pytania dotyczące zabezpieczeń 5 razy w ciągu godziny.<br\><br\></li>
                <li>
Użytkownik będzie próbował resetowania hasła dla tego samego konta użytkownika 5 razy w ciągu godziny.<br\><br\></li>
              </ol>
              <p>Aby rozwiązać ten problem, poinstruuj użytkownika, aby 24 godzin po ostatnia próba, a użytkownik będzie mógł zresetować swoje hasło.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Użytkownik widzi komunikat o błędzie podczas sprawdzania poprawności lub jej numer telefonu</p>
            </td>
            <td>
              <p>Podczas próby weryfikacji telefon ma zostać użyte jako metodę uwierzytelniania, zostanie wyświetlonych o błędzie z informacją:</p>
              <p>

              </p>
              <p>Numer telefonu niepoprawne określony.</p>
            </td>
            <td>
              <p>Ten błąd występuje, gdy podany numer telefonu jest niezgodny z numerem telefonu w pliku.</p>
              <p>Upewnij się, że użytkownik wprowadza pełny numer telefonu, łącznie z kodem obszar i kraju, podczas próby używania metody telefoniczny do resetowania hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Błąd podczas przetwarzania żądania</p>
            </td>
            <td>
              <p>Użytkownik widzi komunikat o błędzie z informacją:</p>
              <p>

              </p>
              <p>Błąd podczas przetwarzania żądania </p>
              <p>Podczas próby resetowania hasła.</p>
            </td>
            <td>
              <p>Może to być spowodowane problemami związanymi z wielu, ale ogólnie ten błąd jest powodowany przez albo to usługa awarii lub konfiguracji problem, który nie jest możliwe. </p>
              <p>Jeśli zostanie wyświetlony ten błąd, i jest wpływające na ochronę firmy, skontaktuj się z obsługą i firma Microsoft może pomóc JNW. Zobacz <a href="#information-to-include-when-you-need-help">informacje mają być uwzględniane podczas potrzebujesz pomocy</a> , aby zobaczyć, jakie należy podać do pracownika pomocy technicznej, aby ułatwić szybkie rozstrzygnięcie.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Rozwiązywanie problemów z zapisu hasła
Jeśli wystąpi błąd podczas Włączanie, wyłączanie lub przy użyciu zapisu hasła, można je rozwiązać, wykonując poniższe kroki rozwiązywania problemów:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Błąd sprawy</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Jaki błąd czy użytkownika są widoczne?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rozwiązanie</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Błędy ogólne ułatwiającej rozpoczęcie korzystania i uruchamiania</p>
            </td>
            <td>
              <p>W środowisku lokalnym z powodu błędu 6800 w dzienniku zdarzeń aplikacji na komputerze narzędzie Azure AD Connect uruchomić usługi resetowania hasła.</p>
              <p>

              </p>
              <p>Po ułatwiającej rozpoczęcie korzystania federacyjnych lub skrótu hasła synchronizowanych użytkowników nie można zresetować swoje hasło.</p>
            </td>
            <td>
              <p>Po włączeniu zapisu hasło aparatu synchronizacji nawiąże połączenie biblioteki zapisu w celu przeprowadzenia konfiguracji (ułatwiającej rozpoczęcie korzystania) przez rozmówcy chmury usługi ułatwiającej rozpoczęcie korzystania. Wszelkich błędów napotkanych podczas ułatwiającej rozpoczęcie korzystania lub podczas uruchamiania punktu końcowego WCF do zapisu hasła spowoduje błędy w dzienniku zdarzeń w dzienniku zdarzeń komputera narzędzie Azure AD Connect.</p>
              <p>Podczas ponownego uruchamiania usługi Synchronizacja Jeśli został skonfigurowany zapisu, punktu końcowego WCF zostanie uruchomiony w górę. Jednak jeśli uruchamiania punktu końcowego kończy się niepowodzeniem, firma Microsoft będzie po prostu dziennika zdarzeń 6800 i umożliwić uruchomienia usługi synchronizacji. Obecność to zdarzenie oznacza, że zapisu hasło punktu końcowego nie została uruchomiona w górę. Elementy wydarzenia (6800) oraz wpisy w dzienniku zdarzeń dziennika zdarzeń generowanie przez PasswordResetService składnika będzie wskazywać, dlaczego punkt końcowy nie można uruchomić w górę. Przejrzyj te błędy dziennika zdarzeń i spróbuj ponownie uruchomić narzędzie Azure AD Connect, jeśli zapisu hasło nie działa. Jeśli problem będzie nadal występował, spróbuj wyłączyć i ponownie włączyć zapisu hasła.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Gdy użytkownik będzie próbował resetowania hasła lub odblokowywanie konta z wystornowaniem hasła włączone, kończy się niepowodzeniem. Ponadto, zobacz zdarzenia w zawierającej Azure AD Connect dziennika zdarzeń: "Aparat synchronizacji zwrócony błąd hr = 800700CE, wiadomości = pliku lub wewnętrzny jest zbyt długa" po wystąpieniu operację odblokowania.
                            </p>
            </td>
            <td>
              <p>To może wystąpić, jeśli zostały uaktualnione ze starszych wersji Azure AD Connect lub DirSync. Uaktualnianie ze starszymi wersjami Azure AD Connect ustawić hasło 254 znaków dla konta Azure AD Management Agent (nowsze wersje będą ustawić hasło o długości 127 znaków). Takie długie hasła działają dla operacji łącznik AD importowania i eksportowania, ale nie są obsługiwane przez operację odblokowania.
                            </p>
            </td>
            <td>
              <p>[Znajdowanie konta usługi Active Directory](active-directory-aadconnect-accounts-permissions.md#active-directory-account) narzędzie Azure AD Connect i resetowanie hasła zawierać maksymalnie 127 znaków. Otwórz **Usługi synchronizacji** z Start menu. Przejdź do **łączników** i Znajdź **Łącznik usługi Active Directory**. Zaznacz go i kliknij polecenie **Właściwości**. Przejdź do strony **poświadczeń** i wprowadź nowe hasło. Wybierz **przycisk OK** , aby zamknąć stronę.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Wystąpił błąd podczas konfigurowania zapisu podczas instalacji narzędzie Azure AD Connect.</p>
            </td>
            <td>
              <p>W ostatnim kroku procesu instalacji narzędzie Azure AD Connect pojawi się komunikat o błędzie z informacją o tym, że nie można skonfigurować hasło wystornowania.</p>
              <p>

              </p>
              <p>Dziennik zdarzeń Azure AD łączenie aplikacji zawiera błąd 32009 z tekstem "Błąd podczas pobierania auth token".</p>
            </td>
            <td>
              <p>Ten błąd występuje w dwóch przypadkach:</p>
              <ul>
                <li class="unordered">
Po określeniu niepoprawnego hasła dla konta administratora globalnego, określonego na początku Azure AD Connect proces instalacji.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Próbujesz użyć użytkownik federacyjny dla konta administratora globalnego, określonego na początku Azure AD Connect proces instalacji.<br\><br\></li>
              </ul>
              <p>Aby naprawić ten błąd, upewnij się, że nie jesteś przy użyciu kont federacyjnych dla administratora globalnego określone na początku proces instalacji narzędzie Azure AD Connect i podane hasło jest poprawne.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Wystąpił błąd podczas konfigurowania zapisu podczas instalacji narzędzie Azure AD Connect.</p>
            </td>
            <td>
              <p>Dziennik zdarzeń komputera narzędzie Azure AD Connect zawiera błąd 32002 generowane przez PasswordResetService.</p>
              <p>

              </p>
              <p>Odczytuje komunikat o błędzie: "błąd podczas łączenia z ServiceBus, Dostawca tokenu nie może zapewnić tokenu zabezpieczeń..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>Przyczyny tego błędu jest usługi resetowania hasła działającej w środowisku lokalnym nie może nawiązać połączenia z punktu końcowego bus usługi w chmurze. Ten błąd jest zwykle zwykle spowodowany reguły zapory blokuje połączenia wychodzące do określonego adresu portu lub sieci web.</p>
              <p>

              </p>
              <p>Upewnij się, że ustawienia zapory umożliwiają połączenia wychodzące dla następujących elementów:</p>
              <ul>
                <li class="unordered">
Cały ruch przez port TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Połączenia wychodzące do <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Po zaktualizowaniu tych reguł, uruchom ponownie komputer Azure AD Connect i zapisu hasło powinien wznowić pracę.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Hasło wystornowania punktu końcowego lokalna nie jest dostępny</p>
            </td>
            <td>
              <p>Po zakończeniu pracy dla niektórych czasu federacyjnych lub skrótu hasła synchronizowanych użytkowników nie można zresetować swoje hasło.</p>
            </td>
            <td>
              <p>W niektórych przypadkach rzadkich zapisu hasła może się nie powieść ponowne uruchomienie, jeśli narzędzie Azure AD Connect rozpoczęła się ponownie. W tych przypadkach najpierw sprawdź, czy hasło zapisu jest wyświetlany jako włączone na prem. Można to zrobić za pomocą Kreatora Azure AD Connect lub programu powershell (zobacz HowTos sekcja powyżej). Jeśli ta funkcja zostanie wyświetlone są włączone, spróbuj Włączanie lub wyłączanie funkcji ponownie za pomocą interfejsu użytkownika lub programu PowerShell. Zobacz "krok 2: Włączanie wystornowania hasło na komputerze synchronizacji katalogów &amp; skonfiguruje reguły zapory" w <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">sposób, aby włączyć lub wyłączyć wystornowania hasła</a> , aby uzyskać więcej informacji o tym, jak to zrobić.</p>
              <p>

              </p>
              <p>Jeśli to nie rozwiąże problemu, spróbuj całkowite odinstalowanie i ponowne zainstalowanie Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Błędy uprawnień</p>
            </td>
            <td>
              <p>Federacji lub d synchronizacji mieszania haseł użytkowników, którzy próba zresetowania ich haseł zobacz komunikat o błędzie po przesłaniu hasła wskazuje, że wystąpił problem z usługi.</p>
              <p>

              </p>
              <p>Oprócz tego podczas operacji resetowania hasła, mogą być widoczne błąd dotyczący zarządzania agenta odmowa dostępu na dzienniki zdarzeń lokalnej.</p>
            </td>
            <td>
              <p>Jeśli widzisz tych błędów w dzienniku zdarzeń, upewnij się, czy konto AD MA (który został określony w Kreatorze podczas konfiguracji) ma uprawnienia do zapisu hasła.</p>
              <p>

              </p>
              <p>Należy ZAUWAŻYĆ, że po znajduje się to uprawnienie może potrwać do 1 godziny dotyczące uprawnień do ścieknie w dół za pośrednictwem sdprop zadania w tle na kontrolerze domeny. </p>
              <p>Do resetowania pracy hasła uprawnienia należy oznakować deskryptora zabezpieczeń w obiekcie użytkownika, którego hasło jest resetowanie. Do momentu te uprawnienia są wyświetlane na obiekcie użytkownika, w przypadku resetowania hasła będą nadal zakończyć się niepowodzeniem z odmową dostępu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Błąd podczas konfigurowania zapisu hasło z Kreatora konfiguracji Azure AD Connect </p>
            </td>
            <td>
              <p>Błąd "Nie można MA lokalizowanie" w Kreatorze podczas włączania/wyłączania zapisu hasła</p>
            </td>
            <td>
              <p>Istnieje znane błędów w wersji Azure AD Connect, które manifesty w następujących sytuacji:</p>
              <ol class="ordered">
                <li>
Konfigurowanie narzędzie Azure AD Connect dla dzierżawy abc.com (zweryfikowano domena) za pomocą poświadczenia. Powoduje AAD łącznik z nazwy "abc.com — AAD" tworzony.<br\><br\></li>
                <li>
Następnie następnie zmienisz poświadczenia AAD łącznika (za pomocą synchronizacji starego interfejsu użytkownika) (należy zauważyć, że pochodzi ona tej samej dzierżawy, ale inną nazwę domeny). <br\><br\></li>
                <li>
Teraz podczas próby włączać/wyłączać zapisu hasła. Kreator tworzenia nazwę łącznika przy użyciu poświadczenia, jako "abc.onmicrosoft.com — AAD" i przekazać do polecenia cmdlet wystornowania hasła. To zakończy się niepowodzeniem, ponieważ nie ma żadnego łącznika utworzone za pomocą tej nazwy.<br\><br\></li>
              </ol>
              <p>Ten problem usunięto w naszym najnowszej kompilacji. Jeśli masz starszy kompilacji jeden obejściem tego jest użyć polecenia cmdlet programu powershell, aby włączyć lub wyłączyć funkcję. Zobacz "krok 2: Włączanie wystornowania hasło na komputerze synchronizacji katalogów &amp; skonfiguruje reguły zapory" w <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">sposób, aby włączyć lub wyłączyć wystornowania hasła</a> , aby uzyskać więcej informacji o tym, jak to zrobić.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nie można zresetować hasło dla użytkowników w grupy specjalne, takie jak Administratorzy domeny i Enterprise Administratorzy itp.</p>
            </td>
            <td>
              <p>Federacji lub d synchronizacji mieszania haseł użytkowników, którzy są częścią grupy chronionego i próba zresetowania ich haseł zobacz komunikat o błędzie po przesłaniu hasła wskazuje, że wystąpił problem z usługi.</p>
            </td>
            <td>
              <p>Uprzywilejowanych użytkowników w usłudze Active Directory są chronione za pomocą AdminSDHolder. Zobacz <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> , aby uzyskać więcej informacji. </p>
              <p>

              </p>
              <p>Oznacza to, z parametrami zabezpieczeń tych obiektów okresowo są sprawdzane zgodnie z jednym AdminSDHolder określonych w, a zostaną zresetowane, jeśli są różne. Dodatkowe uprawnienia, które są wymagane do zapisu hasła w związku z tym nie ścieknie dla tych użytkowników. Może to powodować zapisu hasło nie działa dla tych użytkowników. W wyniku nie jest obsługiwana zarządzania hasła dla użytkowników, w tym grupom ponieważ dzieli model zabezpieczeń AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nie można odnaleźć Resetowanie operacji kończy się niepowodzeniem z obiektem</p>
            </td>
            <td>
              <p>Federacji lub d synchronizacji mieszania haseł użytkowników, którzy próba zresetowania ich haseł zobacz komunikat o błędzie po przesłaniu hasła wskazuje, że wystąpił problem z usługi.</p>
              <p>

              </p>
              <p>Oprócz tego podczas operacji resetowania hasła, może zostać wyświetlony komunikat o błędzie w dzienniki zdarzeń z usługi Azure AD Connect wskazująca błąd "Nie można odnaleźć obiektu".</p>
            </td>
            <td>
              <p>Ten błąd wskazuje, że aparat synchronizacji jest nie można odnaleźć obiektu użytkownika w obszar łączników AAD lub połączone MV albo obiekt miejsca łącznika AD. </p>
              <p>

              </p>
              <p>Aby rozwiązać ten problem, upewnij się, że użytkownik jest faktycznie zsynchronizowany z lokalna do AAD za pośrednictwem w bieżącym wystąpieniu programu narzędzie Azure AD Connect i sprawdzanie stanu obiektów łącznik spacji i MV. Upewnij się, że obiekt AD CS jest łącznik do obiektu MV za pomocą reguły "Microsoft.InfromADUserAccountEnabled.xxx".</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Resetowanie operacji kończy się niepowodzeniem z wielu eror znalezione dopasowania</p>
            </td>
            <td>
              <p>Federacji lub d synchronizacji mieszania haseł użytkowników, którzy próba zresetowania ich haseł zobacz komunikat o błędzie po przesłaniu hasła wskazuje, że wystąpił problem z usługi.</p>
              <p>

              </p>
              <p>Oprócz tego podczas operacji resetowania hasła, może zostać wyświetlony komunikat o błędzie w dzienniki zdarzeń z usługi Azure AD Connect wskazująca komunikat o błędzie "Znaleziono wiele maches".</p>
            </td>
            <td>
              <p>Oznacza to, że aparat synchronizacji wykrył, że obiekt MV jest połączony z więcej niż jeden obiektami usług AD CS za pośrednictwem "Microsoft.InfromADUserAccountEnabled.xxx". Oznacza to, że użytkownik ma konto włączone w więcej niż jeden las. </p>
              <p>

              </p>
              <p>W tym scenariuszu nie jest obecnie obsługiwane dla wystornowania hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Operacje hasło kończą się niepowodzeniem z powodu błędu konfiguracji.</p>
            </td>
            <td>
              <p>Operacje hasło kończą się niepowodzeniem z powodu błędu konfiguracji. Dziennik zdarzeń aplikacji zawiera błąd Azure AD Connect 6329 z tekstem: 0x8023061f (zakończone niepowodzeniem, ponieważ nie jest włączona synchronizacja hasła na ten Agent zarządzania operacji.)</p>
            </td>
            <td>
              <p>Dzieje się tak, jeżeli konfiguracji Azure AD Connect została zmieniona na dodawanie&nbsp;nowy las AD (lub do usunięcia i ponownego dodania istniejący las) <strong>po</strong> już została włączona funkcja zapisu hasła. Operacje hasła dla użytkowników w tych lasów nowo dodany zakończy się niepowodzeniem. Aby rozwiązać ten problem, wyłącz i ponownie włączyć funkcję zapisu hasła, po zakończeniu las zmiany konfiguracji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Pisanie ponownie hasła, które zostało zresetowane użytkowników działa poprawnie, ale zapisaniem haseł z powrotem zmienione przez użytkownika lub resetowanie dla użytkownika przez administratora kończy się niepowodzeniem.</p>
            </td>
            <td>
              <p>Podczas próby resetowania hasła w imieniu użytkownika za pomocą portalu zarządzania Azure, zostanie wyświetlony komunikat z informacją: "usługi resetowania hasła działającej w środowisku lokalnym nie obsługuje Administratorzy resetowanie haseł użytkowników. Uaktualnij do najnowszej wersji Azure AD Connect, aby rozwiązać ten problem."</p>
            </td>
            <td>
              <p>Dzieje się tak, gdy wersja aparatu synchronizacji nie obsługuje określonego operacji zapisu hasło, którego użyto. Wersji narzędzie Azure AD Connect później niż 1.0.0419.0911 obsługuje wszystkie operacje zarządzania hasła, w tym hasła Resetowanie wystornowania, wystornowania Zmień hasło i wystornowania resetowania hasła inicjowanych przez administratora za pomocą portalu zarządzania Azure.&nbsp; Wersje DirSync później niż 1.0.6862 obsługuje tylko wystornowania resetowania hasła. Aby rozwiązać ten problem, zaleca się zainstalowanie najnowszej wersji Azure AD Connect lub Azure Active Directory łączenie (Aby uzyskać więcej informacji, zobacz <a href="active-directory-aadconnect">Narzędzia integracji katalogu</a>) Aby rozwiązać ten problem i w pełni wykorzystać zapisu hasła w Twojej organizacji.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Kody błędów hasło zapisu dziennika zdarzeń
Podczas rozwiązywania problemów z zapisu hasło najlepiej do sprawdzenia tego dziennika zdarzeń aplikacji na komputerze narzędzie Azure AD Connect. Tego dziennika zdarzeń będzie zawierać wydarzenia z dwóch źródeł potrzebnych do zapisu hasła. Źródło PasswordResetService opisując działania i zagadnienia związane z operacji zapisu hasła. Źródło synchronizacja opisując działania i zagadnienia związane z Ustawianie hasła w środowisku usługi AD.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kod</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Nazwa / wiadomości</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Źródła</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Opis</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>PORĘCZENIE: MMS(4924) 0x80230619 — "ograniczenie zabezpiecza hasło przed zmianami do bieżącej określony".</p>
            </td>
            <td>
              <p>Synchronizacja</p>
            </td>
            <td>
              <p>To zdarzenie występuje, gdy usługa zapisu hasło próbuje Ustawianie hasła w katalogu lokalnym, który nie spełnia okres ważności hasła, historii, złożoności lub filtrowania wymagań domeny.</p>
              <ul>
                <li class="unordered">
Jeśli masz zainstalowaną minimalny okres ważności hasła i ostatnio zmieniono hasło w tym oknie czasu, nie można zmienić hasło ponownie, aż osiągnie ona określonym wieku w Twojej domenie. Podczas testowania, minimalny wiek powinna być równa 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jeśli masz włączone wymagania historii hasła, a następnie należy wybrać hasło, które nie został użyty w ostatnim N razy, gdzie N to ustawienie historii hasła. Jeśli wybierzesz hasło, które zostało użyte w ostatnim N razy, zostanie wyświetlona awarii w tym przypadku. Podczas testowania, Historia powinna być równa 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jeśli masz złożoności hasła, będą z nich wymuszane gdy użytkownik będzie próbował zmienianie lub resetowanie hasła.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jeśli masz włączone filtrów hasło, a użytkownik wybierze hasła, które nie spełnia kryteriów filtrowania, następnie Resetowanie lub zmienianie operacja nie powiedzie się.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Aparat synchronizacji zwrócony błąd hr = 80230402, wiadomości = próba pobrania obiektu nie powiodło się, ponieważ nie istnieją zduplikowane pozycje z tym samym kotwicy</p>
            </td>
            <td>
              <p>Synchronizacja</p>
            </td>
            <td>
              <p>To zdarzenie występuje po włączeniu tej samej nazwy użytkownika w wiele domen.  Na przykład jeśli trwa synchronizacja lasów konta i zasobów i mieć tej samej nazwy użytkownika, które są obecne i włączone w każdym, ten błąd może wystąpić.  </p>
              <p>Ten błąd może również wystąpić, jeśli korzystasz z atrybutem nieunikatowej kotwicy (na przykład alias lub UPN) i dwóch użytkowników udostępnianie tego samego atrybutu kotwicy.</p>
              <p>Aby rozwiązać ten problem, upewnij się, nie ma żadnych zduplikowanych użytkowników w swojej domenie i korzystasz z atrybutem kotwicy unikatowe dla każdego użytkownika.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że usługa lokalnego wykrywana żądanie federacyjnych resetowania hasła lub hasło mieszania synchronizacji d użytkownika pochodzące z chmury. To zdarzenie jest pierwsze zdarzenie każdego hasła resetowania operacji zapisu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że użytkownik wybrał nowe hasło podczas operacji resetowania hasła, możemy określić, że hasło spełnia wymagania firmy hasło i hasło zostały prawidłowo zapisane Wstecz w środowisku lokalnym AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że użytkownik wybrał hasło i hasło pomyślnie otrzymano w środowisku lokalnym, że wystąpił błąd podczas próby możemy ustawić hasło w środowisku lokalnym AD. Może się to zdarzyć z kilku powodów:</p>
              <ul>
                <li class="unordered">
Hasło użytkownika nie spełnia wiek, historii, złożoności lub filtrowanie wymagania dla domeny. Spróbuj zupełnie nowe hasło, aby rozwiązać ten problem.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Konto usługi MA nie ma odpowiednich uprawnień, aby ustawić nowe hasło dla danego konta użytkownika.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Konto użytkownika znajduje się w chronionym grupą, takie jak Administratorzy domeny lub przedsiębiorstwa, które nie zezwala na operacje Ustawianie hasła.<br\><br\></li>
              </ul>
              <p>Zobacz <a href="#troubleshoot-password-writeback">Rozwiązywanie problemów z zapisu hasła</a> , aby dowiedzieć się więcej o jakie inne situtions może spowodować ten błąd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie występuje po włączeniu zapisu hasło z Azure AD Connect i wskazuje, że firma Microsoft uruchamiana ułatwiającej rozpoczęcie korzystania organizacji zapisu hasło usługi sieci web.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza proces ułatwiającej rozpoczęcie korzystania zakończyło się pomyślnie i że funkcja zapisu hasło jest gotowy do użycia.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie wskazuje, że usługa lokalnego wykrywania żądania zmiany hasła federacyjnych lub synchronizacji haseł mieszania d użytkownika pochodzące z chmury. To zdarzenie jest pierwsze zdarzenie w każdej operacji zapisu zmiany hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że użytkownik wybrał nowe hasło podczas operacji zmiany hasła, możemy określić, że hasło spełnia wymagania firmy hasło i hasło zostały prawidłowo zapisane Wstecz w środowisku lokalnym AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że użytkownik wybrał hasło i hasło pomyślnie otrzymano w środowisku lokalnym, że wystąpił błąd podczas próby możemy ustawić hasło w środowisku lokalnym AD. Może się to zdarzyć z kilku powodów:</p>
              <ul>
                <li class="unordered">
Hasło użytkownika nie spełnia wiek, historii, złożoności lub filtrowanie wymagania dla domeny. Spróbuj zupełnie nowe hasło, aby rozwiązać ten problem.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Konto usługi MA nie ma odpowiednich uprawnień, aby ustawić nowe hasło dla danego konta użytkownika.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Konto użytkownika znajduje się w chronionym grupą, takie jak Administratorzy domeny lub przedsiębiorstwa, które nie zezwala na operacje Ustawianie hasła.<br\><br\></li>
              </ul>
              <p>Zobacz <a href="#troubleshoot-password-writeback">Rozwiązywanie problemów z zapisu hasła</a> , aby dowiedzieć się więcej o jakie inne sytuacje mogą powodować ten błąd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Usługa lokalnego wykryto żądanie federacyjnych resetowania hasła lub hasło mieszania synchronizacji d użytkownika pochodzące od administratora w imieniu użytkownika. To zdarzenie jest pierwsze zdarzenie w każdej operacji zapisu resetowania hasła inicjowanych przez administratora.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Administrator wybrane nowe hasło podczas operacji resetowania hasła inicjowanych przez administratora, możemy określić, że hasło spełnia wymagania dotyczące haseł firmy i hasło zostały prawidłowo zapisane Wstecz w środowisku lokalnym AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Administrator zaznaczone hasła w imieniu użytkownika i hasło pomyślnie otrzymano w środowisku lokalnym, ale wystąpił błąd podczas próby możemy ustawić hasło w środowisku lokalnym AD. Może się to zdarzyć z kilku powodów:</p>
              <ul>
                <li class="unordered">
Hasło użytkownika nie spełnia wiek, historii, złożoności lub filtrowanie wymagania dla domeny. Spróbuj zupełnie nowe hasło, aby rozwiązać ten problem.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Konto usługi MA nie ma odpowiednich uprawnień, aby ustawić nowe hasło dla danego konta użytkownika.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Konto użytkownika znajduje się w chronionym grupą, takie jak Administratorzy domeny lub przedsiębiorstwa, które nie zezwala na operacje Ustawianie hasła.<br\><br\></li>
              </ul>
              <p>Zobacz <a href="#troubleshoot-password-writeback">Rozwiązywanie problemów z zapisu hasła</a> , aby dowiedzieć się więcej o jakie inne situtions może spowodować ten błąd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie występuje po wyłączeniu zapisu hasło z Azure AD Connect i wskazuje, że firma Microsoft uruchamiana offboarding organizacji zapisu hasło usługi sieci web.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza proces offboarding zakończyło się pomyślnie i został pomyślnie wyłączony możliwości zapisu hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie wskazuje, że proces offboarding nie zakończyło się pomyślnie. Może to być spowodowane błąd uprawnień w chmurze lub lokalnego konta administratora określony podczas konfigurowania lub próbujesz ma być używana w chmurze federacyjnych administratora globalnego wyłączenie zapisu hasła. Aby rozwiązać ten problem, należy sprawdzić dotyczącymi administracji uprawnienia i że nie używasz dowolną Federacji konta podczas konfigurowania możliwości zapisu hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie wskazuje, że usługa zapisu hasła została pomyślnie uruchomiona i jest gotowy do akceptowania wezwań zarządzania hasło z chmury.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że usługę zapisu hasło zostało zatrzymane i żądań zarządzania hasło z chmury nie zostanie zrealizowane.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że pomyślnie pobrane możemy tokenu autoryzacji dla administratora globalnego określony podczas instalacji narzędzie Azure AD Connect, aby rozpocząć proces offboarding lub ułatwiającej rozpoczęcie korzystania.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza pomyślnie utworzonych klucza szyfrowania haseł, używanym do szyfrowania haseł z chmury były wysyłane do środowiska lokalnego.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza nieznany błąd podczas operacji zarządzania hasła. Przyjrzyj się tekst wyjątku zdarzenia, aby uzyskać więcej informacji. Jeśli masz problemy, spróbuj wyłączanie i ponowne włączanie zapisu hasła. Jeśli to nie pomoże, zawiera kopię dziennika zdarzeń oraz identyfikator śledzenia określony wykorzystywania do swojego pracownika pomocy technicznej.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie wskazuje wystąpił resetowania błędu łączenia się z chmurą hasło usługi i zazwyczaj występuje, gdy usługa lokalnego nie może połączyć się z usługą web resetowania hasła. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza wystąpił błąd podczas łączenia się wystąpienie bus Twojej dzierżawy usługi. Może się to zdarzyć, ponieważ blokują połączeń wychodzących w środowisku lokalnym. Sprawdź zapory, aby upewnić się, Zezwól na połączenia przez port TCP 443 i <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>i spróbuj ponownie. Jeśli nadal występują problemy, spróbuj wyłączanie i ponowne włączanie zapisu hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że dane wejściowe przekazywane do naszych interfejs API usługi sieci web jest nieprawidłowy. Spróbuj ponownie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że wystąpił błąd podczas odszyfrowywania hasło, które otrzymano z chmury. Może to być spowodowane odszyfrowywania klucza niezgodność między usługa w chmurze i środowiska lokalnego. Aby rozwiązać ten problem, wyłącz i ponownie włączyć zapisu hasła w środowisku lokalnym.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Podczas ułatwiającej rozpoczęcie korzystania możemy zapisać informacje specyficzne dla dzierżawy w pliku konfiguracji w środowisku lokalnym. To zdarzenie oznacza wystąpił błąd podczas zapisywania tego pliku lub że podczas uruchomienia usługi został błąd odczytu pliku. Aby rozwiązać ten problem, spróbuj wyłączania i ponownego włączania zapisu hasła, aby wymusić ponownie zapisać ten plik konfiguracyjny. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>PRZESTARZAŁE — to zdarzenie nie występuje w narzędzie Azure AD Connect, tylko bardzo wczesnym wersjach systemu DirSync, które obsługiwane wystornowania.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Podczas ułatwiającej rozpoczęcie korzystania wyślemy resetowania usług danych z chmury hasło lokalnego. Danych, który jest następnie zapisywane w pliku w pamięci przed wysłaniem do synchronizacji usługi bezpiecznego przechowywania tych informacji na dysku. To zdarzenie oznacza problem z zapisywanie lub aktualizowanie danych w pamięci. Aby rozwiązać ten problem, spróbuj wyłączania i ponownego włączania zapisu hasła, aby wymusić ponowne pisanie tej konfiguracji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie wskazuje, że Otrzymaliśmy nieprawidłową odpowiedź z usługi sieci web resetowania hasła. Aby rozwiązać ten problem, spróbuj wyłączania i ponownego włączania zapisu hasła.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że firma Microsoft nie można uzyskać zezwolenie token dla konta administratora globalnego określony podczas instalacji narzędzie Azure AD Connect. Ten błąd może być spowodowany nieprawidłowe Nazwa użytkownika lub hasło określone dla konta administratora globalnego lub Federacji ponieważ określony konta administratora globalnego. Aby rozwiązać ten problem, ponownie uruchom konfiguracji z właściwą nazwę użytkownika i hasło i upewnij się, administrator jest zarządzane (tylko do chmury lub synchronizacji haseł) konta.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza wystąpił błąd podczas generowania hasła klucza szyfrowania lub odszyfrowywania hasła otrzymywanej od usług w chmurze. Ten błąd może wskazuje wystąpił problem z środowiska. Przyjrzyj się szczegóły usługi Dziennik zdarzeń, aby dowiedzieć się więcej i rozwiązać ten problem. Można także spróbować wyłączanie i ponowne włączanie usług zapisu hasła, aby rozwiązać ten problem.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie określa, czy usługa lokalnego może nie poprawnie komunikować się z usługi sieci web resetowania hasła do rozpoczęcia procesu ułatwiającej rozpoczęcie korzystania. Może to być spowodowane reguły zapory lub wystąpił problem podczas uzyskiwania token uwierzytelniania dla dzierżawy usługi. Aby rozwiązać ten problem, upewnij się, że nie są blokuje połączenia wychodzące przez port TCP 443 i TCP 9350 9354 lub <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, a nie Federacji AAD konta administratora, którego używasz do wewnętrznego. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>PRZESTARZAŁE — to zdarzenie nie występuje w narzędzie Azure AD Connect, tylko bardzo wczesnym wersjach systemu DirSync, które obsługiwane wystornowania.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie określa, czy usługa lokalnego może nie poprawnie komunikować się z usługi sieci web resetowania hasła do rozpoczęcia procesu offboarding. Może to być spowodowane reguły zapory lub wystąpił problem podczas uzyskiwania tokenu autoryzacji dla dzierżawy usługi. Aby rozwiązać ten problem, upewnić się, że użytkownik nie blokują połączeń wychodzących na 443 lub <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>i konta administratora AAD używanym do offboard zewnętrzny. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że było próbować nawiązać wystąpienie bus Twojej dzierżawy usługi. W normalnych warunkach to należy nie będzie problemem, ale Jeśli zobaczysz to zdarzenie wiele razy, należy rozważyć, czy sprawdzanie połączenie sieciowe bus usługi, zwłaszcza jeśli jest to połączenie niskiej przepustowości lub długim czasem oczekiwania.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>W celu monitorowania kondycji usługi wystornowania hasła, wyślemy resetowania weryfikowania danych nasze hasło usługi sieci web co 5 minut. To zdarzenie oznacza, że wystąpił błąd podczas wysyłania te informacje dotyczące kondycji usługi sieci web w chmurze. Te informacje dotyczące kondycji nie zawiera danych OII lub osoby, a jest czysto weryfikowania oraz statystyk podstawowej usługi dzięki czemu firma Microsoft udostępnia informacje o stanie usługi w chmurze.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że został zwrócony przez AD nieznany błąd, przejrzyj dziennik zdarzeń serwer Azure AD Connect zdarzeń ze źródła synchronizacja, aby uzyskać więcej informacji na temat tego błędu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że użytkownik chce się Resetowanie lub zmienianie hasła nie odnaleziono w katalogu lokalnego. Ten błąd może wystąpić, gdy użytkownik został usunięty lokalnie, ale nie w chmurze, lub jeśli występuje problem z synchronizacją. Sprawdź dzienniki synchronizacji, a także ostatniej synchronizacji kilka uruchamianie szczegóły, aby uzyskać więcej informacji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Podczas resetowania hasła lub żądania zmiany pochodzi z chmury, firma Microsoft korzysta kotwicy chmury określony podczas procesu konfigurowania Azure AD Connect, aby dowiedzieć się, jak utworzyć łącze żądanie użytkownika w środowisku lokalnym. To zdarzenie oznacza znalezionych dwóch użytkowników w katalogu lokalnym z tym samym atrybutem kotwicy chmury. Sprawdź dzienniki synchronizacji, a także ostatniego kilka synchronizacji uruchamianie szczegóły, aby uzyskać więcej informacji.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że konta usługi zarządzania Agent nie ma odpowiednie uprawnienia w danym, aby ustawić nowe hasło konta. Upewnij się, czy konto MA w Las użytkownika ma resetowania i Zmień uprawnienia hasła na wszystkich obiektów w las.  Aby uzyskać więcej informacji o tym, jak wykonać w celu, zobacz <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">Krok 4: Konfigurowanie uprawnień usługi Active Directory</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że firma Microsoft próby Resetowanie lub zmienianie hasła dla konta, która została wyłączona w swojej siedzibie. Włącz konto i spróbuj ponownie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Zdarzenie oznacza, że firma Microsoft próby Resetowanie lub zmienianie hasła dla konta, które zostało zablokowane w środowisku lokalnym. Blokady mogą występować podczas próby zmiany lub resetowanie hasła operacji zbyt wiele razy w krótkim użytkownika. Odblokuj konto i spróbuj ponownie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie wskazuje, że użytkownik określić niepoprawne bieżące hasło, gdy wykonywanie hasła zmieniony operacji. Określ poprawne bieżące hasło i spróbuj ponownie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie występuje, gdy usługa zapisu hasło próbuje Ustawianie hasła w katalogu lokalnym, który nie spełnia okres ważności hasła, historii, złożoności lub filtrowania wymagania domeny.</p>
              <ul>
                <li class="unordered">
Jeśli masz zainstalowaną minimalny okres ważności hasła i ostatnio zmieniono hasło w tym przedziale czasu, nie można zmienić hasło ponownie, aż osiągnie ona określonym wieku w Twojej domenie. Podczas testowania, minimalny wiek powinna być równa 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jeśli masz włączone wymagania historii hasła, a następnie należy wybrać hasło, które nie został użyty w ostatnim N razy, gdzie N to ustawienie historii hasła. Jeśli wybierzesz hasło, które zostało użyte w ostatnim N razy, zostanie wyświetlona awarii w tym przypadku. Podczas testowania, Historia powinna być równa 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jeśli masz złożoności hasła, będą z nich wymuszane gdy użytkownik będzie próbował zmienianie lub resetowanie hasła.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jeśli masz włączone filtrów hasło, a użytkownik wybierze hasła, które nie spełnia kryteriów filtrowania, następnie Resetowanie lub zmienianie operacja nie powiedzie się.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>To zdarzenie oznacza, że pisania problem hasło z powrotem do katalogu lokalnego ze względu na problem z konfiguracją z usługą Active Directory. Sprawdź dziennik zdarzeń aplikacji na komputerze narzędzie Azure AD Connect wiadomości z usługi Synchronizacja, aby uzyskać więcej informacji, na jakie błędu. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>PRZESTARZAŁE — to zdarzenie nie występuje w narzędzie Azure AD Connect, tylko bardzo wczesnym wersjach systemu DirSync, które obsługiwane wystornowania.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>PRZESTARZAŁE — to zdarzenie nie występuje w narzędzie Azure AD Connect, tylko bardzo wczesnym wersjach systemu DirSync, które obsługiwane wystornowania.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>PRZESTARZAŁE — to zdarzenie nie występuje w narzędzie Azure AD Connect, tylko bardzo wczesnym wersjach systemu DirSync, które obsługiwane wystornowania.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Rozwiąż problemy z łącznością zapisu hasła

Jeśli występują przerwy usługi ze składnikiem wystornowania hasła: narzędzie Azure AD Connect, Oto niektóre Szybkie kroki, które można wykonać, aby rozwiązać ten problem:

 - [Uruchom ponownie Azure AD łączenie usługi synchronizacji](#restart-the-azure-AD-Connect-sync-service)
 - [Wyłącz i ponownie włączyć funkcję wystornowania hasła](#disable-and-re-enable-the-password-writeback-feature)
 - [Zainstaluj najnowszą wersję Azure AD Connect](#install-the-latest-azure-ad-connect-release)
 - [Rozwiązywanie problemów z zapisu hasła](#troubleshoot-password-writeback)

Firma Microsoft zaleca ogólnie, wykonaj następujące czynności w kolejności powyżej, aby odzyskać usługi w możliwie najszybszy sposób.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Uruchom ponownie Azure AD łączenie usługi synchronizacji
Ponowne uruchamianie usługi synchronizacji Azure łączenie AD może pomóc rozwiązać problemy z łącznością lub innych przejściowych problemów z usługą.

 1. Jako administrator kliknij przycisk **Start** na serwerze z **Azure AD Connect**.
 2. W polu wyszukiwania wpisz **"tekst services.msc"** , a następnie naciśnij klawisz **Enter**.
 3. Znajdź pozycję **Microsoft Azure AD Connect** .
 4. Kliknij prawym przyciskiem myszy pozycję usługi, kliknij polecenie **Uruchom ponownie**i poczekaj na zakończenie operacji.

    ![][002]

Ta procedura zostanie ponownie nawiązać połączenie z usług w chmurze i rozwiązywania wszelkich zakłóceń, które mogą występować.  Jeśli ponowne uruchomienie usługi synchronizacji nie rozwiąże problemu, zaleca się możesz wypróbować wyłączyć i ponownie włączyć funkcję zapisu hasła w następnym kroku.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Wyłącz i ponownie włączyć funkcję wystornowania hasła
Wyłączanie i ponowne włączanie funkcji wystornowania hasło ułatwiają rozwiązywanie problemów z łącznością.

 1. Jako administrator Otwórz **Narzędzie Azure AD Connect Kreatora konfiguracji**.
 2. W oknie dialogowym **Połącz Azure AD** wprowadź **poświadczenia administratora globalnego Azure AD**
 3. W oknie dialogowym **Łączenie w usługach AD DS** wprowadź **poświadczenia administratora w usługach domenowych AD**.
 4. W oknie dialogowym **jednoznacznie identyfikujący typ przepływu pracy użytkowników** kliknij przycisk **Dalej** .
 5. W oknie dialogowym **opcjonalne funkcje** wyczyść pole wyboru **hasło zapisu z powrotem** .

    ![][003]

 6. Kliknij przycisk **Dalej** do pozostałych stron okno dialogowe bez jakichkolwiek zmian, aż przejdziesz na stronę **gotowy do skonfigurowania** .
 7. Upewnij się, że na stronie Konfigurowanie to **hasło opcja Wstecz zapisu jako wyłączona** , a następnie kliknij zielony przycisk **Konfiguruj** , aby zatwierdzić wprowadzone zmiany.
 8. W oknie dialogowym **zakończone** Wyłącz opcję **Synchronizuj teraz** , a następnie kliknij przycisk **Zakończ** , aby zamknąć kreatora.
 9. Ponownie otwórz **Narzędzie Azure AD Connect Kreator konfiguracji**.
 10.    **Powtórz kroki 2 – 8**, z wyjątkiem zapewnić, że **zaznaczysz opcję wstecz zapisu hasło** **opcjonalne funkcje** ekranu, aby ponownie włączyć usługę.

    ![][004]

Ta procedura zostanie ponownie nawiązać połączenie z naszych usługi w chmurze i rozwiązywania wszelkich zakłóceń, które mogą występować.

Wyłączanie i ponowne włączanie funkcji zapisu hasła nie rozwiąże problemu, zaleca się spróbuj ponownie zainstalować narzędzie Azure AD Connect w następnym kroku.

### <a name="install-the-latest-azure-ad-connect-release"></a>Zainstaluj najnowszą wersję Azure AD Connect
Ponowne zainstalowanie pakietu narzędzie Azure AD Connect rozwiąże, że wszelkie problemy z konfiguracją, które mogą mieć wpływ na możliwość albo połączyć się z naszych usług w chmurze lub zarządzanie hasłami w środowisku lokalnym AD.
Firma Microsoft zaleca wykonywania tej czynności tylko po próbie dwa pierwsze kroki opisane powyżej.

 1. Pobierz najnowszą wersję Azure AD Connect [tutaj](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Ponieważ zainstalowano już Azure AD Connect, wystarczy przeprowadzić uaktualnienie w miejscu w celu zaktualizowania instalacji narzędzie Azure AD Connect do najnowszej wersji.
 3. Wykonywanie pobrany pakiet i postępuj zgodnie instrukcjami instrukcjami, aby zaktualizować komputer Azure AD Connect.  Nie dodatkowych czynności ręcznie są wymagane, chyba że został dostosowany w nowym polu synchronizacji reguł, w tym przypadku należy **zapasową tych przed kontynuowaniem uaktualnienia i ręczne zainstalowanie ich po zakończeniu**.

Ta procedura zostanie ponownie nawiązać połączenie z naszych usługi w chmurze i rozwiązywania wszelkich zakłóceń, które mogą występować.

Jeśli zainstalowanie najnowszej wersji serwer Azure AD Connect nie rozwiąże problemu, zaleca się spróbuj wyłączania i ponownego włączania wystornowania hasło jako ostatni krok po zainstalowaniu najnowszej synchronizacji QFE.

Jeśli to nie rozwiąże problemu, następnie zalecamy wykonanie przyjrzeć się [Rozwiązywanie problemów z wystornowania hasła](#troubleshoot-password-writeback) i [hasła Azure AD zarządzania — często zadawane pytania](active-directory-passwords-faq.md) , aby sprawdzić, jeśli problemu może omówiono.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Łącza do hasła Resetowanie dokumentacji
Poniżej znajdują się łącza do wszystkich stron dokumentacji Azure AD hasło:

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
* [**Działania**](active-directory-passwords-how-it-works.md) — zapoznaj się z sześciu różnych składników usługi i każdej czy
* [**Wprowadzenie**](active-directory-passwords-getting-started.md) — Dowiedz się, jak użytkownicy mogą zresetować i zmieniać swoje hasła w chmurze lub lokalnych
* [**Dostosowywanie**](active-directory-passwords-customize.md) — Dowiedz się, jak dostosować wygląd i sposób działania i zachowanie usługi do potrzeb danej organizacji
* [**Najważniejsze wskazówki**](active-directory-passwords-best-practices.md) — Dowiedz się, jak szybko wdrażanie i efektywne zarządzanie hasłami w organizacji
* [**Uzyskaj więcej informacji**](active-directory-passwords-get-insights.md) — Dowiedz się więcej o naszych zintegrowane funkcje raportowania
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — odpowiedzi na często zadawane pytania
* [**Aby uzyskać więcej informacji**](active-directory-passwords-learn-more.md) — głębokich przejdź do szczegółów technicznych opis działania usługi



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
