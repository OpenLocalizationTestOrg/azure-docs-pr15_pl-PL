<properties
    pageTitle="Często zadawane pytania: Hasła Azure AD zarządzania | Microsoft Azure"
    description="Często zadawane pytania dotyczące zarządzania hasłami w Azure AD, włączając do resetowania hasła, rejestracji, raporty i zapisu do lokalnej usługi Active Directory."
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
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Zarządzanie hasłami — często zadawane pytania

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

Poniżej przedstawiono niektóre często zadawane pytania dla wszystkich czynności związanych z zarządzaniem hasła.

Jeśli okaże się z pytanie, na które nie znasz odpowiedzi lub szukasz pomocy dotyczącej konkretnego problemu, które są przeciwległych można znaleźć na poniżej Aby sprawdzić, czy firma Microsoft zostały ujęte ją już.  Jeśli firma Microsoft nie jest jeszcze nie martw się! Zachęcamy do zadawanie wszelkie pytania, masz nieuwzględnionych w tym miejscu na [forach Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) i firma Microsoft będzie wrócić do Ciebie jak możemy.

Niniejszy artykuł jest podzielony na następujące sekcje:

- [**Pytania dotyczące rejestracji resetowania hasła**](#password-reset-registration)
- [**Pytania na temat resetowania hasła**](#password-reset)
- [**Pytania na temat raportów zarządzania hasła**](#password-management-reports)
- [**Pytania na temat zapisu hasła**](#password-writeback)

## <a name="password-reset-registration"></a>Rejestracja do resetowania hasła
 - **P: czy użytkownicy zarejestrować własne dane resetowania hasła?**

 > **A:** Tak, pod warunkiem resetowania hasła jest włączona, i są one licencjonowane, ich przejdź do portalu rejestracji Resetowanie hasła pod adresem http://aka.ms/ssprsetup zarejestrować ich informacje uwierzytelniające do użycia w przypadku resetowania hasła. Użytkownicy będą mogli również zarejestrować, przechodząc do panelu dostęp u http://myapps.microsoft.com, klikając kartę profilu i klikając pozycję Rejestr dla opcji do resetowania hasła. Dowiedz się więcej na temat uzyskiwania skonfigurowane do resetowania, czytając sposób uzyskiwania skonfigurowany do resetowania hasła użytkowników hasła użytkowników.

 - **P: czy można zdefiniować dane resetowania hasła w imieniu moich użytkowników?**

 > **A:** Tak, możesz to zrobić za pomocą programu PowerShell lub DirSync lub za pośrednictwem portalu [Portalu zarządzania Azure](https://manage.windowsazure.com) lub Administrator pakietu Office. Dowiedz się więcej o tej funkcji na wpis w blogu poprawione prywatności za Azure AD MFA i numery telefonów resetowania hasła i odczytu Dowiedz się, jak dane są używane przez hasło zresetować.

 - **P: czy klient może synchronizować dane dla pytania zabezpieczeń z lokalnego?**

 > **A:** Nie, nie jest to możliwe dzisiaj, ale są możemy ustalając go.

 - **P: czy użytkownicy rejestrowania danych w taki sposób, aby inni użytkownicy nie widzą te dane**

 > **A:** Tak, gdy użytkownicy rejestrowania danych za pomocą portalu rejestracji Resetowanie hasła go elementy są zapisywane w polach uwierzytelniania prywatnych, które są widoczne tylko przez administratorów globalnej i siebie użytkownika. Dowiedz się więcej o tej funkcji na wpis w blogu ulepszona prywatności za Azure AD MFA i numery telefonów resetowania hasła i odczytu Dowiedz się, jak dane są używane przez hasło zresetować.

 - **P: czy użytkownicy muszą być zarejestrowane przed skorzystaniem resetowania hasła?**

 > **A:** Nie, jeśli zdefiniujesz wystarczających informacji uwierzytelniania w jego imieniu, użytkownicy nie będą mieć do rejestrowania. Resetowanie hasła będzie działać świetnie jak został prawidłowo sformatowany dane przechowywane w odpowiednich polach w katalogu. Aby uzyskać więcej informacji o przez odczyt Dowiedz się, jak dane są używane przez resetowania hasła.

 - **P: czy synchronizowanie lub zestawu pól uwierzytelniania wiadomości E-mail lub dodatkowego telefonu uwierzytelniania, telefon uwierzytelniania imieniu moich użytkowników?**

 > **A:** Nie są obecnie ale są uwzględniając, włączyć tę funkcję.

 - **P: jak portal rejestracji ustalić używanej opcji, aby wyświetlić moich użytkowników?**

 > **A:** Portal rejestracji resetowania hasła tylko pokazuje opcje czy włączono dla użytkowników usługi katalogowej Konfigurowanie karty w sekcji Resetowanie hasła użytkownika. Oznacza to, że jeśli nie włączysz, powiedzieć pytania dotyczące zabezpieczeń, następnie użytkownicy nie będą mogą rejestrować się w tej opcji.

 - **P: po uznaje użytkownika zarejestrowana?**

 > **A:** Użytkownik jest traktowany jako zarejestrowanych, kiedy dana osoba ma co najmniej N fragmentami informacje uwierzytelniania zdefiniowane, gdzie N to liczba z metod wymagane uwierzytelnienie ustawiany w [Portalu zarządzania Azure](https://manage.windowsazure.com). Aby uzyskać więcej informacji, zobacz Dostosowywanie zasad Resetowanie hasła użytkownika.


## <a name="password-reset"></a>Resetowanie hasła

 - **P: jak długo należy czekać do odbierania wiadomości e-mail, SMS lub rozmowy telefonicznej z Resetowanie hasła?**

 > **A:** Wiadomości e-mail, wiadomości SMS i telefony trwa w obszarze 1 minuty, przy użyciu normalna wielkość liter, 5-20 sekund. W tym okres nie otrzyma powiadomienie, sprawdź folder wiadomości-śmieci, którego numer / skontaktowanie się z poczty e-mail jest to można się spodziewać i że dane uwierzytelniania w katalogu jest poprawnie sformatowany. Aby dowiedzieć się więcej o formatowaniu numery telefonów i poczty e-mail adresów do użytku z Resetowanie hasła Zobacz Dowiedz się, jak dane są używane przez resetowania hasła.

 - **P: jakie języki są obsługiwane w przypadku resetowania hasła?**

 > **A:** Interfejs użytkownika do resetowania hasła wiadomości SMS i połączeń głosowych między komputerami są zlokalizowane w tym samym 40 języków, które są obsługiwane w usłudze Office 365. Znajdują się: arabski, bułgarski, chiński uproszczony, chiński tradycyjny, chorwacki, czeski, duński, holenderski, angielski, estoński, fiński, francuski, niemiecki, grecki, hebrajski, Hindi, węgierski, indonezyjski, włoski, japoński, kazachskim, koreański, łotewski, litewski, Malajski (Malezja), norweski (Bokmål), Polski, portugalski (Brazylia), portugalski (Portugalia), rumuński, rosyjski, serbski (łaciński), słowacki, słoweński, hiszpański, szwedzki, tajski, turecki, ukraiński i wietnamski.

 - **P:, które części środowiska resetowania hasła uzyskiwanie marki, gdy Ustawiam związane ze znakowaniem w mojej katalogu organizacji jest skonfigurować kartę?**

 > **A:** Portal resetowania hasła zostaną wyświetlone logo organizacji, a także umożliwia konfigurowanie kontaktu łącza administratora wskazywały własnego adresu e-mail lub adresu URL. Wszystkie wiadomości e-mail, która przesłane przez resetowania hasła będzie zawierać logo Twojej organizacji, kolory (w tym przypadku czerwony), nazwy w treści wiadomości e-mail i dostosować z nazwy. Zobacz przykład oznakowanych elementów poniżej. Aby dowiedzieć się więcej, przeczytaj Resetowanie hasła Dostosowywanie wyglądu i działania.

  ![][001]

 - **P: jak nauczyć moich użytkowników o miejsce docelowe, aby zresetować swoje hasło?**

 > **A:** Możesz wysłać użytkownikom bezpośrednio do https://passwordreset.microsoftonline.com lub polecić je do kliknięcia nie może uzyskać dostępu do połączenie z kontem znajdujące się na dowolnym szkoły lub identyfikator pracy ekran logowania. Użytkownik może swobodnie można publikować te łącza (lub tworzenie przekierowania adresu URL do nich) w dowolnym miejscu, które łatwo dostępny dla użytkowników.

 - **P: czy mogę używać tej strony z urządzenia przenośnego**

 > **A:** Tak, ta strona działa na urządzeniach przenośnych.

 - **P: czy możesz obsługuje odblokowywanie kont lokalną usługą active directory, gdy użytkownicy resetowanie haseł?**

 > **A:** Tak, kiedy użytkownik resetuje jej hasło i zapisu hasło zostało wdrożonym wszystkich wersjach systemu Azure AD Connect lub wersje Azure AD Sync 1.0.0485.0222 lub nowszy, konta użytkownika będzie odblokowywane automatycznie, gdy użytkownik pozwala zresetować swoje hasło.

 - **P: jak można zintegrować resetowania bezpośrednio do danego użytkownika logowania środowisko pulpitu hasła?**

 > **A:** Nie jest to możliwe dzisiaj. Jednak jeśli naprawdę potrzebujesz tej funkcji i jesteś klientem Azure AD Premium, można zainstalować Menedżer tożsamości Microsoft bez dodatkowych opłat i wdrażania rozwiązania Resetuj hasło lokalnego znalezionych tam rozwiązać to wymaganie.

 - **P: czy można ustawić pytania zabezpieczeń dla innych języków?**

 > **A:** Nie, nie jest to możliwe dzisiaj, ale są możemy ustalając go.

 - **P: jak wiele pytań możemy skonfigurować dla opcji uwierzytelniania pytania dotyczące zabezpieczeń?**

 > **A:** W [Portalu zarządzania Azure](https://manage.windowsazure.com)można skonfigurować maksymalnie 20 pytania niestandardowe zabezpieczeń.

 - **P: jak długo może być pytania dotyczące zabezpieczeń?**

 > **A:** Pytania dotyczące zabezpieczeń mogą być od 3 do 200 znaków.

 - **P: jak długo może być odpowiedzi na pytania dotyczące zabezpieczeń?**

 > **A:** Odpowiedzi może być 3-40 znaków.

 - **P: znajdują się zduplikowane odpowiedzi na pytania dotyczące zabezpieczeń odrzucone?**

 > **A:** Tak, firma Microsoft Odrzuć zduplikowanych odpowiedzi na pytania dotyczące zabezpieczeń.

 - **P: maja użytkownika zarejestrować więcej niż jeden z tego samego pytania zabezpieczeń?**

 > **A:** Nie, gdy użytkownik rejestruje konkretne pytanie, dana osoba nie może zarejestrować dla tego zapytania po raz drugi.

 - **P: czy można ustawić minimalny limit pytania dotyczące zabezpieczeń rejestracji i resetowanie?**

 > **A:** Tak, limit jeden można ustawić dla rejestracji i drugi resetowania. pytania dotyczące zabezpieczeń 3 do 5 mogą być wymagane na potrzeby rejestrowania i 3 do 5 może być wymagane na potrzeby resetowania.

 - **P: użytkownika została zarejestrowana więcej niż maksymalną liczbę wymaganych Resetowanie pytań, jak pytania dotyczące zabezpieczeń zaznaczenie podczas resetowania?**

 > **A:** Pytania dotyczące zabezpieczeń N wybierane są losowo poza łączna liczba pytań, które użytkownik został zarejestrowany, gdzie N jest minimalna liczba pytania wymaganego do resetowania hasła. Na przykład jeśli użytkownik ma 5 pytania dotyczące zabezpieczeń zarejestrowana, ale tylko 3 są wymagane do resetowania, 3 tych 5 zostaną wybranych losowo i prezentowane użytkownikowi podczas resetowania. Jeśli użytkownik uzyskuje problem odpowiedzi na pytania, aby zapobiec kucie pytanie powtarza procesu zaznaczenia.

 - **P: czy możesz uniemożliwić użytkownikom próby resetowania wielokrotnie w okresie krótkiej hasła?**

 > **A:** Tak, istnieje kilka funkcji wbudowanych w przypadku resetowania hasła. Użytkownicy mogą tylko spróbuj 5 prób resetowania hasła w ciągu godziny przed są zablokowane przez 24 godziny. Użytkownicy mogą tylko próbować sprawdzić poprawność numeru telefonu 5 razy w ciągu godziny przed są zablokowane przez 24 godziny. Użytkownicy mogą tylko spróbuj metodę uwierzytelniania pojedynczy 5 razy w ciągu godziny przed są zablokowane przez 24 godziny.

 - **P: w jakim są wiadomości e-mail i wiadomości SMS jednorazowy kod dostępu ważne?**

 > **A:** Istnienia sesji w przypadku resetowania hasła jest 105 minut. Oznacza to, że od początku hasło Resetowanie operacji, użytkownik ma 105 minut, aby zresetować swoje hasło. Wiadomości e-mail i wiadomości SMS jednorazowy kod dostępu są nieprawidłowe, po wygaśnięciu tego okresu.


## <a name="password-management-reports"></a>Zarządzanie hasłami raportów

 - **P: jak długo trwa danych wyświetlanych na raporty zarządzania hasło?**

 > **A:** Dane będą widoczne w raportach zarządzania hasło po 5-10 minut. Jej niektórych przypadkach może potrwać do godziny się pojawić.

 - **P: jak można filtrować raporty zarządzania hasło?**

 > **A:** Raporty zarządzania hasła można filtrować, klikając pozycję small lupę skrajnej prawej stronie etykiety kolumn, w kierunku górnej części raportu (zobacz zrzut ekranu). Jeśli chcesz bardziej rozbudowane filtrowanie, możesz pobrać raportu do programu excel i Utwórz tabelę przestawną.

  ![][002]

 - **P: jaka jest maksymalna liczba zdarzeń są przechowywane w raportach zarządzania hasło?**

 > **A:** Do 1000 hasło zdarzenia rejestracji resetowania hasła lub resetowanie są przechowywane w raporty zarządzania hasła.  Pracujemy nad Rozwiń ten numer, aby uwzględnić kolejnych zdarzeń.

 - **P: jak daleko ponownie hasło raporty zarządzania przejdź?**

 > **A:** Raporty zarządzania hasło Pokaż operacji wykonywanych w ciągu ostatnich 30 dni. Pracujemy obecnie analizuje sposobu wprowadzania to okres dłuższy czas. Teraz Jeśli chcesz zarchiwizować tych danych, możesz pobrać raportów okresowo i zapisywanie ich w innej lokalizacji.

 - **P: czy istnieje maksymalna liczba wierszy, które mogą być wyświetlane w raportach zarządzania hasło?**

 > **A:** Tak, maksymalnie 1000 wierszy może pojawić się na jednym z raportów zarządzania hasłami, czy są one wyświetlane w interfejsie użytkownika lub pobieranie. Pracujemy obecnie analizuje jak zwiększyć to ograniczenie.

 - **P: czy istnieje interfejs API dostępu do resetowania hasła lub rejestracji danych raportowania?**

 > **A:** Tak, można znaleźć w następujących dokumentacji, aby dowiedzieć się, jak można pobrać Resetowanie hasła raportowania strumienia danych.  [Dowiedz się, jak uzyskać dostęp do hasła programowo zresetować raportowania zdarzeń](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Hasło Wystornowaniem
 - **P: jak działa zapisu hasła w tle?**

 > **A:** Aby uzyskać szczegółowe wyjaśnienie, co się dzieje po włączeniu zapisu hasło, a także jak dane są wczytywane przez system z powrotem do środowiska lokalnego, zobacz [działa jak hasło wystornowania](active-directory-passwords-learn-more.md#how-password-writeback-works) . Zobacz [model zabezpieczeń zapisu hasła](active-directory-passwords-learn-more.md#password-writeback-security-model) w sposób hasło zapisu działa Aby dowiedzieć się, jak firma Microsoft upewnij się, że hasło zapisu jest usługą wysokim poziomie zabezpieczeń.

 - **P: jak długo zapisu hasło potrwa do pracy?  Czy istnieje opóźnienie synchronizacji like z synchronizacją mieszania hasła?**

 > **A:** Hasło zapisu jest błyskawiczne. Jest potok synchroniczne odpowiadającą istotnie inaczej niż synchronizacji mieszania haseł. Hasło zapisu umożliwia użytkownikom uzyskiwanie opinii czasu rzeczywistego o ich resetowania hasła lub zmienianie operacji. Średni czas potrzebny do pomyślnego zapisu hasła jest w obszarze 500 ms.

 - **P: jakie typy kont zapisu hasło działa dla?**

 > **A:** Hasło zapisu działa dla Federacji i d synchronizacji mieszania haseł użytkowników.

 - **P: czy zapisu hasło wymusić zasady haseł mojej domeny?**

 > **A:** Tak, zapisu hasła wymusza okres ważności hasła, historii, złożoność, filtry i innych ograniczeń, może być umieszczony w miejsce na haseł w domenie lokalnej.

 - **P: czy bezpiecznego zapisu hasło?  Jak mogę mieć pewność, że I nie będą naruszone uzyskiwanie przez hakera?**

 > **A:** Tak, zapisu hasło jest bardzo bezpieczne. Aby uzyskać więcej informacji o 4 warstw zabezpieczeń z zastosowane przez usługę zapisu hasła, wyewidencjonuj [model zabezpieczeń zapisu hasła](active-directory-passwords-learn-more.md#password-writeback-security-model) w działa jak hasło zapisu.




## <a name="links-to-password-reset-documentation"></a>Łącza do hasła Resetowanie dokumentacji
Poniżej znajdują się łącza do wszystkich stron dokumentacji Azure AD hasło:

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
* [**Działania**](active-directory-passwords-how-it-works.md) — Dowiedz się więcej o sześć różnych składników usługi i każdej czy
* [**Wprowadzenie**](active-directory-passwords-getting-started.md) — Dowiedz się, jak użytkownicy mogą zresetować i zmieniać swoje hasła w chmurze lub lokalnych
* [**Dostosowywanie**](active-directory-passwords-customize.md) — Dowiedz się, jak dostosować wygląd i sposób działania i zachowanie usługi do potrzeb danej organizacji
* [**Najważniejsze wskazówki**](active-directory-passwords-best-practices.md) — Dowiedz się, jak szybko wdrażanie i efektywne zarządzanie hasłami w organizacji
* [**Uzyskaj więcej informacji**](active-directory-passwords-get-insights.md) — Dowiedz się więcej o naszych zintegrowane funkcje raportowania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — Dowiedz się, jak szybko Rozwiązywanie problemów z usługą
* [**Aby uzyskać więcej informacji**](active-directory-passwords-learn-more.md) — głębokich przejdź do szczegółów technicznych opis działania usługi


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
