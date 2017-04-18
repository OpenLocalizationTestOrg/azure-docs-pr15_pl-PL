<properties
    pageTitle="Publikowanie aplikacji z serwera Proxy aplikacji Azure AD | Microsoft Azure"
    description="Publikowanie aplikacji lokalnej w chmurze za pomocą serwera Proxy aplikacji Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikowanie aplikacji przy użyciu serwera Proxy aplikacji Azure AD

Serwer Proxy aplikacji Azure AD ułatwia obsługuje pracowników zdalnych przez opublikowanie lokalnej aplikacji są dostępne w Internecie. W tej chwili należy masz już [włączone serwera Proxy aplikacji w portalu klasyczny Azure](active-directory-application-proxy-enable.md). W tym artykule opisano czynności, aby opublikować aplikacje, które są uruchomione w Twojej sieci lokalnej i bezpieczny dostęp zdalny z spoza sieci. Po ukończeniu tego artykułu będzie gotowa do konfigurowania aplikacji informacji spersonalizowanych lub wymagania dotyczące zabezpieczeń.

> [AZURE.NOTE] Serwer Proxy aplikacji jest funkcję, która jest dostępna tylko po uaktualnieniu do Premium lub Basic edition usługi Azure Active Directory. Aby uzyskać więcej informacji zobacz [wersji usługi Azure Active Directory](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>Publikowanie aplikacji przy użyciu Kreatora

1. Zaloguj się jako administrator w [portalu klasyczny Azure](https://manage.windowsazure.com/).
2. Przejdź do usługi Active Directory i wybierz katalog, w którym włączono serwera Proxy aplikacji.

    ![Usługa Active Directory - ikony](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w dolnej części ekranu

    ![Dodawanie aplikacji](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Wybierz pozycję **Publikuj aplikację, która będzie dostępny spoza sieci**.

    ![Publikowanie aplikacji, która będzie dostępny spoza sieci](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Wprowadź następujące informacje na temat aplikacji:

    - **Nazwa**: Przyjazna nazwa aplikacji. Musi być unikatowa w katalogu.
    - **Wewnętrzny adres URL**: adres łącznika serwera Proxy aplikacji korzysta z dostępu do aplikacji z wewnątrz sieci prywatnej. Można udostępnić określonej ścieżki na serwerze wewnętrznej bazy danych, aby opublikować, gdy reszta serwera jest wycofanych. W ten sposób można publikowanie różnych witryn na tym samym serwerze i nadaj każdy z nich własne zasady nazwę i access.

        > [AZURE.TIP] Jeśli publikujesz ścieżkę, upewnij się, że zawiera wszystkie niezbędne obrazy, skrypty i arkusze stylów dla aplikacji. Na przykład jeśli aplikacji znajduje się na https://yourapp/app i używa obrazy znajdujące się w https://yourapp/media, następnie należy opublikować https://yourapp/ jako ścieżkę katalogu.

    - **Metoda było wstępnie uwierzytelnić**: jak serwer Proxy aplikacji weryfikuje użytkowników przed nadanie im dostępu do aplikacji. Wybierz jedną z opcji z menu rozwijanego.

        - Azure Active Directory: Serwer Proxy aplikacji przekierowuje użytkowników, zaloguj się przy użyciu Azure AD, która uwierzytelnia ich uprawnień do katalogu i aplikacji.
        - Przekazywanie: Użytkownicy nie mają do uwierzytelnienia dostępu do aplikacji.

    ![Właściwości aplikacji](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Aby zakończyć działanie kreatora, kliknij znacznik wyboru u dołu ekranu. Aplikacja jest teraz zdefiniowane w Azure AD.


## <a name="assign-users-and-groups-to-the-application"></a>Przypisywanie użytkowników i grup do aplikacji

Aby użytkownikom dostęp do aplikacji opublikowanych należy przypisywać je pojedynczo lub w grupach. (Należy pamiętać o przypisanie sobie dostęp, zbyt.) Wymaga to, że każdy użytkownik licencji dla Azure podstawowe lub wyższą. Można przypisywać licencje, pojedynczo lub grup. Aby uzyskać więcej informacji, zobacz [Przypisywanie użytkowników do aplikacji](active-directory-applications-guiding-developers-assigning-users.md) . 

W przypadku aplikacji, które wymagają wstępnego uwierzytelniania dzięki temu uprawnienia do korzystania z tej aplikacji. W przypadku aplikacji, które nie wymagają wstępne uwierzytelnienie użytkownicy nadal można przypisywać do aplikacji tak, aby była wyświetlana na liście aplikacji, takich jak Moje aplikacje.

1. Po zakończeniu działania kreatora Dodawanie aplikacji, zostanie wyświetlona strona Szybki Start dla aplikacji. Aby zarządzać, kto ma dostęp do aplikacji, wybierz pozycję **Użytkownicy i grupy**.

    ![Szybki start dla aplikacji serwera Proxy przypisywanie użytkowników — zrzut ekranu](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Wyszukiwanie określonych grup w katalogu lub wyświetlanie wszystkich użytkowników. Aby wyświetlić wyniki wyszukiwania, kliknij znacznik wyboru.

    ![Wyszukiwanie grup lub użytkowników — zrzut ekranu](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Zaznacz danego użytkownika lub grupy, którą chcesz przypisać do tej aplikacji, a następnie kliknij polecenie **Przypisz**. Zostanie wyświetlony monit o potwierdzenie tej akcji.

> [AZURE.NOTE] W przypadku aplikacji zintegrowane uwierzytelnianie systemu Windows można przypisać tylko użytkownicy i grupy, które są synchronizowane z usługi Active Directory w lokalnej. Użytkownicy, którzy Zaloguj się przy użyciu konta Microsoft i goście nie można przypisać w przypadku aplikacji opublikowanych z serwera Proxy Azure Active Directory aplikacji. Upewnij się, zaloguj się przy użyciu poświadczeń, które są częścią tej samej domeny jako aplikację, którą publikujesz użytkowników.

## <a name="test-your-published-application"></a>Testowanie opublikowanych aplikacji

Po opublikowaniu aplikacji, możesz je przetestować, przechodząc do adresu URL, który można opublikować. Upewnij się, że będzie dostępny, poprawne renderowanie i wszystko działa zgodnie z oczekiwaniami. Jeśli masz problemy lub komunikatu o błędzie, wypróbuj [Podręcznik rozwiązywania problemów](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurowanie aplikacji

Można zmodyfikować opublikowanych aplikacji lub skonfigurowanie zaawansowanych opcji na stronie Konfigurowanie. Na tej stronie aplikacji można dostosować, zmieniając nazwy lub przekazywanie logo. Można również zarządzać reguły dostępu, takich jak metody wstępne uwierzytelnienie lub uwierzytelnianie wieloskładnikowe.

![Konfiguracja zaawansowana](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Po opublikowaniu aplikacji przy użyciu Azure Active Directory serwer Proxy aplikacji, były widoczne na liście aplikacji w Azure AD i można zarządzać nimi.

Wyłączenie serwera Proxy aplikacji usług po opublikowaniu aplikacje są już dostępne spoza sieci prywatnej. Nie powoduje to usunięcia aplikacji.

Aby wyświetlić aplikacji i upewnij się, że jest ona dostępna, kliknij dwukrotnie nazwę aplikacji. Jeśli usługa serwera Proxy aplikacji jest wyłączona, a aplikacja nie jest dostępna, w górnej części ekranu zostanie wyświetlony komunikat z ostrzeżeniem.

Aby usunąć aplikację, wybierz aplikację na liście, a następnie kliknij przycisk **Usuń**.

## <a name="next-steps"></a>Następne kroki

- [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
- [Praca z aplikacjami pamiętać o oświadczeniach](active-directory-application-proxy-claims-aware-apps.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
