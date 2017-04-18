
<properties
    pageTitle="Podłącz za pomocą Azure AD kondycji z usług AD FS | Microsoft Azure"
    description="Jest to strona Azure AD łączenie kondycji sposobów monitorowania sieci lokalnej infrastruktury usług AD FS."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Podłącz za pomocą Azure AD kondycji z usług AD FS
Poniższą dokumentację dotyczy monitorowania infrastruktury programu AD FS z Azure AD łączenie kondycją. Aby uzyskać informacje dotyczące monitorowania Azure AD Connect (synchronizacja) z Azure AD łączenie kondycją zobacz [Przy użyciu Azure AD łączenie kondycji do synchronizacji](active-directory-aadconnect-health-sync.md). Ponadto aby uzyskać informacje dotyczące monitorowania usług domenowych Active Directory z Azure AD łączenie zdrowia, zobacz [Przy użyciu Azure AD łączenie zdrowia z usługami AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Alerty dotyczące usług AD FS
Sekcja Azure AD łączenie alerty dotyczące kondycji zawiera listę aktywne alertów. Każdy alert zawiera odpowiednie informacje, kroki rozwiązywania i łącza do powiązanych dokumentacji.

Możesz kliknąć dwukrotnie alert aktywny ani rozwiązany, otworzyć nowe kart z dodatkowymi informacjami, czynności, które można wykonać, aby rozpoznawać alert i łącza odpowiednią dokumentację. Możesz także wyświetlić danych historycznych na alerty, które zostały rozwiązane w przeszłości.

![Azure AD Connect portalu kondycji](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Analizy użycia usług AD FS
Azure AD łączenie kondycji analizy użycia analizuje ruch uwierzytelniania serwerów federacyjnych. Dwukrotne kliknięcie pola analizy użycia, aby otworzyć karta analizy użycia, które przedstawiono kilka metryki i grupy.

>[AZURE.NOTE] Aby użyć analizy użycia z usług AD FS, należy się upewnić, że inspekcja usług AD FS jest włączona. Aby uzyskać więcej informacji zobacz [Włączanie inspekcja usług AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Azure AD Connect Portal zdrowia](./media/active-directory-aadconnect-health/report1.png)

Aby wybrać dodatkowe metryki, określ przedział czasu lub zmienić zgrupowania, kliknij prawym przyciskiem myszy wykres analizy użycia i wybierz opcję Edytuj wykres. Następnie można określić przedział czasu, wybierz inny jednostki metryczne i zmienić grupowania. Można wyświetlać rozdzielania ruchu uwierzytelniania na podstawie innego "metryk" i grupowanie Każda metryka zgodnie z parametrami odpowiednich "Grupuj według" opisane w poniższej tabeli:

| Metryka | Grupowanie według | Oznacza to, jakie grupowania i dlaczego jest przydatne? |
| ------ | -------- | -------------------------------------------- |
| Zwraca sumę: Liczba żądań przetwarzanych przez usługi federacyjnej | Wszystkie | Statystyka całkowita liczba żądań bez grupowania. |
|  | Aplikacji | Całkowita liczba żądań oparte na wybranych uzależnioną z grup. Aby dowiedzieć się, jaka aplikacja odbiera ile procent sumy ruch przydaje się grupy. |
|  | Serwer | Całkowita liczba żądań oparte na serwerze przetwarzania żądania z grup. Grupy jest przydatna do zrozumienia dystrybucji obciążenia całkowitym ruchu. |
|  | Dołączanie do obszaru roboczego | Całkowita liczba żądań według tego, czy są pochodzące z urządzenia, które są połączone w miejscu pracy (znanej) z grup. Aby dowiedzieć się, jeśli zasoby są dostępne za pomocą urządzenia, które są nieznane infrastruktury tożsamości przydaje się grupy. |
|  | Metody uwierzytelniania | Całkowita liczba żądań na podstawie metody uwierzytelniania używany do uwierzytelniania z grup. Grupy jest przydatna do zrozumienia typowe metody uwierzytelniania, który jest używany do uwierzytelniania. Poniżej przedstawiono metody uwierzytelniania możliwe <ol> <li>Zintegrowane uwierzytelnianie systemu Windows (Windows)</li> <li>Uwierzytelnienie (formularze) oparte na formularzach</li> <li>Logowania jednokrotnego (pojedynczy znak na)</li> <li>X509 certyfikatu uwierzytelniania (certyfikat)</li> <br>Jeśli serwerów federacyjnych otrzymają wezwanie z logowania jednokrotnego plików Cookie, żądanie jest liczony jako logowania jednokrotnego (rejestracji jednokrotnej). W takich przypadkach Jeśli plik cookie jest prawidłowy, użytkownik nie zostanie poproszony o podanie poświadczeń i otrzymuje Bezproblemowa dostęp do aplikacji. To zachowanie jest typowe, jeśli masz wiele jednostek uzależnionej chroniony przez serwerów federacyjnych. |
|  | Lokalizacji sieciowej | Całkowita liczba żądań na podstawie lokalizacji sieci użytkownika z grup. Może być albo intranetu lub ekstranetu. Grupy jest przydatne dowiedzieć się, jaki procent ruchu pochodzi z intranetu i ekstranetu. |
| Całkowita liczba żądań nie powiodło się: Żądań przetwarzanych przez usługi federacyjnej nie powiodło się liczba. <br> (Ta metryka jest dostępna tylko na usług AD FS dla systemu Windows Server 2012 R2)| Typ błędu | Pokazuje liczbę błędów na podstawie wstępnie zdefiniowanego błędu typów. Grupy jest przydatne poznać typowych błędów. <ul><li>Nieprawidłowa nazwa użytkownika lub hasło: błędy spowodowane Nieprawidłowa nazwa użytkownika lub hasło.</li> <li>"Ekstranetu blokady": awarii z powodu żądania otrzymane od użytkownika, które zostało zablokowane z ekstranetu </li><li> "Wygasła hasło": awarii z powodu logowania się przy użyciu hasła użytkowników.</li><li>"Wyłączone konta": awarii z powodu logowania przy użyciu wyłączonego konta użytkowników.</li><li>"Urządzenia uwierzytelniania": awarii z powodu nie służą do uwierzytelniania za pomocą urządzenia uwierzytelniania użytkowników.</li><li>"Certyfikatu uwierzytelniania użytkowników": awarii z powodu użytkowników nie można uwierzytelnić ze względu na nieprawidłowy certyfikat.</li><li>"MFA": awarii z powodu użytkownika nie uwierzytelnianie za pomocą uwierzytelnianie wieloskładnikowe.</li><li>"Innych poświadczeń": "Wydawania autoryzacji": awarii z powodu autoryzacji błędy.</li><li>"Delegowanie wydania": awarii z powodu błędów delegowanie wydania.</li><li>"Token akceptacji": awarii z powodu ADFS odrzucając tokenu od dostawcy tożsamości innych firm.</li><li>"Protokół": awarii z powodu błędów Protocol (protokół).</li><li>"Informacja Brak danych": przechwytywać wszystkie. Wszelkie inne błędy, które nie pasują do określonych kategorii.</li> |
|  | Serwer | Błędy oparte na serwerze. Aby zrozumieć rozkład błędu na serwerach przydaje się grupy. Rozkład normalny może być wskaźnikiem serwera w stanie uszkodzony. |
|  | Lokalizacji sieciowej | Błędy oparte na lokalizacji sieciowej żądań (intranet w porównaniu z ekstranetu). Grupy, warto opis typu żądania, które działają nieprawidłowo. |
|  | Aplikacji | Grup błędy, w zależności od aplikacji docelowej (uzależnioną). Grupy jest przydatne dowiedzieć się, której aplikacji docelowej jest wyświetlane większość liczba błędów. |
| Średnia liczba unikatowych użytkowników aktywny w systemie liczba użytkowników: | Wszystkie | Ta metryka zawiera liczbę średnia liczba użytkowników przy użyciu usługa federacyjna w wycinku zaznaczonego czasu. Użytkownicy nie są zgrupowane. <br>Średnia zależy od przedział czasu zaznaczone. |
|  | Aplikacji | Grup średnia liczba użytkowników, w zależności od aplikacji docelowej (uzależnioną). Grupy jest przydatne dowiedzieć się, ile użytkownicy korzystają z której aplikacji. |


## <a name="performance-monitoring-for-ad-fs"></a>Monitorowanie wydajności dla usług AD FS
Azure AD łączenie kondycji monitorowanie wydajności zawiera monitorowania informacje dotyczące metryki. Zaznaczając pole monitorowania, zostanie wyświetlona karta nowy z szczegółowe informacje na temat metryki.


![Azure AD Connect portalu kondycji](./media/active-directory-aadconnect-health/perf1.png)


Po zaznaczeniu opcji filtru w górnej części karta, można filtrować według serwera, aby wyświetlić metryki poszczególnych serwerów. Aby zmienić metryki, kliknij prawym przyciskiem myszy wykres monitorowania w obszarze karta monitorowania i wybierz opcję Edytuj wykres. Następnie z nowego kartę, która zostanie otwarta w górę, można z listy rozwijanej wybierz dodatkowe metryki i określić przedział czasu dla wyświetlania danych o wydajności.

## <a name="reports-for-ad-fs"></a>Raporty dotyczące usług AD FS
Azure AD łączenie Health zapewnia raportów dotyczących działania i wydajności usług AD FS. Te raporty pomogą Administratorzy lepszy wgląd w działania na serwerach usług AD FS.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Pierwszych 50 użytkowników o niepowodzeniu nazwy użytkownika i hasła logowania

Typowe przyczyny na żądanie nie powiodło się uwierzytelniania na serwerze usług AD FS jest żądanie przy użyciu poświadczeń nieprawidłowe, oznacza to, że problem nazwy użytkownika i hasła. Zwykle dzieje się użytkowników ze względu na złożone hasła, zapomnianego hasła lub literówek.

Istnieją inne powodów, dla których może powodować nieoczekiwane liczba żądań są obsługiwane przez serwery usług AD FS, takich jak: Wygaśnięcie pamięci podręcznej poświadczeń i poświadczenia aplikacji lub złośliwy użytkownik próby zalogować się do konta z seriami znanego haseł. Te dwa przykłady są prawidłowe powodów, dla których może spowodować wzrost żądania.

Azure AD łączenie kondycji dla usług ADFS zawiera raport o górnym 50 użytkowników z prób logowania zakończonych niepowodzeniem z powodu Nieprawidłowa nazwa użytkownika lub hasło. Ten raport uzyskuje się przez przetwarzania generowanych przez wszystkie serwery usług AD FS farm zdarzeń inspekcji

![Azure AD Connect portalu kondycji](./media/active-directory-aadconnect-health-adfs/report1a.png)

W tym raporcie masz łatwy dostęp do następujących rodzajów informacji:

- Całkowita liczba żądań zakończonych niepowodzeniem przy użyciu nazwy użytkownika i hasła problem w ciągu ostatnich 30 dni
- Średnia liczba użytkowników, których nie powiodło się nieprawidłowe nazwy użytkownika i hasła logowania na dzień.


Kliknięcie tej części przejście do kartę raport główny, która zawiera dodatkowe informacje. Ta karta zawiera wykres popularnych informacje, aby ustalić planu bazowego o żądania z problem nazwy użytkownika i hasła. Ponadto zawiera listę góry 50 użytkowników o liczbie niepomyślne.

Wykres zawiera następujące informacje:

- Całkowita liczba logowania nie powiodło się ze względu na nieprawidłowe Nazwa użytkownika i hasło na podstawie-dniowego.
- Całkowita liczba unikatowych użytkowników, które nie zostały logowania na podstawie-dniowego.
- Adres IP klienta z ostatniego żądania

![Azure AD Connect portalu kondycji](./media/active-directory-aadconnect-health-adfs/report3a.png)

Raport zawiera następujące informacje:

| Elementu raportu | Opis
| ------ | -------- |
|Identyfikator użytkownika| Zawiera identyfikator użytkownika, który został użyty. Ta wartość jest co użytkownik wpisał, która w niektórych przypadkach jest nieprawidłowy użytkownika identyfikator używany.|
|Niepomyślne| Pokazuje całkowita liczba niepomyślne danego identyfikatora określonego użytkownika. Tabeli są sortowane od najwięcej niepomyślne w kolejności malejącej.|
|Ostatni błąd| Wyświetla sygnaturę czasową, gdy wystąpił ostatni błąd.
|Ostatni błąd IP | Zawiera adres IP klienta z najnowszych nieprawidłowe żądanie.|



>[AZURE.NOTE] Ten raport jest automatycznie aktualizowana co dwie godziny przy użyciu nowych informacji zbieranych w tym czasie. W wyniku prób logowania w ciągu ostatnich dwóch godzin nie zostaną uwzględnione w raporcie.



## <a name="related-links"></a>Łącza pokrewne

* [Azure AD Connect kondycji](active-directory-aadconnect-health.md)
* [Azure AD Connect instalacji agenta kondycji](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect operacje kondycji](active-directory-aadconnect-health-operations.md)
* [Przy użyciu Azure AD łączenie kondycji dla synchronizacji](active-directory-aadconnect-health-sync.md)
* [Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kondycji — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kondycji Historia wersji](active-directory-aadconnect-health-version-history.md)
