<properties 
    pageTitle="Typ poświadczeń uwierzytelniania API pozostałych zaangażowania Mobile — Ręczna konfiguracja"
    description="W tym artykule opisano ręczne konfigurowanie uwierzytelniania dla interfejsów API pozostałych zaangażowania Mobile" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Typ poświadczeń uwierzytelniania API pozostałych zaangażowania Mobile — Ręczna konfiguracja

Jest to dokumentacja dodatku do [uwierzytelniony przez interfejsy API pozostałych zaangażowania Mobile](mobile-engagement-api-authentication.md). Upewnij się, że możesz czytać należy pobrać kontekstu. W takim innym sposobem wykonaj jednorazowego ustawienia dotyczące konfigurowania usługi uwierzytelniania dla API pozostałych zaangażowania Mobile za pomocą portalu Azure. 

>[AZURE.NOTE] Poniższe instrukcje są oparte na ten [Przewodnik usługi Active Directory](../resource-group-create-service-principal-portal.md) i dostosowane do co to jest wymagane na potrzeby uwierzytelniania dla interfejsów API zaangażowania Mobile. Aby zapoznać się ją, jeśli chcesz poznać kroki wymienione poniżej szczegółowo. 

1. Zaloguj się do konta Azure za pomocą [portalu klasyczny](https://manage.windowsazure.com/).

2. Wybierz pozycję **Usługi Active Directory** w okienku po lewej stronie.

     ![Wybierz pozycję usługi Active Directory][1]

3. Wybierz **Domyślny usługi Active Directory** w portalu usługi Azure. 

     ![Wybierz katalog][2]

    >[AZURE.IMPORTANT] Ta metoda działa tylko wtedy, gdy pracujesz w domyślnym usługi Active Directory dla Twojego konta i nie będzie działać, jeśli przeprowadzasz to w usłudze Active Directory, utworzonego na Twoim koncie. 

4. Aby wyświetlić aplikacje w katalogu, kliknij przycisk **aplikacje**.

     ![Wyświetl aplikacje][3]

5. Wybierz polecenie **Dodaj**. 

     ![Dodawanie aplikacji][4]

6. Wybierz polecenie **Dodaj aplikację, którą rozwija się w mojej organizacji**

     ![Nowa aplikacja][5]

6. Wpisz nazwę aplikacji i wybierz typ aplikacji jako **Interfejs API sieci WEB i/lub aplikacji sieci WEB** i kliknij przycisk Dalej.

     ![Nazwa aplikacji][6]

7. Można zapewnić wszelkie fikcyjna adresy URL **Logowania na adres URL** i **Aplikacji identyfikator URI**. Nie są używane w naszym scenariuszu i adresy URL, same nie są weryfikowane.  

     ![właściwości aplikacji][7]

8. Na końcu tego masz aplikację AAD o nazwie podanych wcześniej następująco. Jest to usługi **AD\_aplikacji\_nazwę** i zapisz go.  

     ![Nazwa aplikacji][8]

9. Kliknij nazwę aplikacji i kliknij pozycję **Konfiguruj**.

     ![Konfigurowanie aplikacji][9]

10. Zanotuj identyfikator klienta, który będzie używany jako **klienta\_identyfikator** dla swojego interfejsu API połączeń. 

     ![Konfigurowanie aplikacji][10]

11. Przewiń w dół do sekcji **klawiszy** i Dodaj klucz z najlepiej czas trwania 2 lat (wygaśnięcia) i kliknij przycisk **Zapisz**. 

     ![Konfigurowanie aplikacji][11]


12. Od razu skopiuj wartość, która jest wyświetlana dla klucza jest wyświetlana tylko teraz, a nie są przechowywane, więc nie będą wyświetlane kiedykolwiek ponownie. W przypadku utraty będzie mieć do wygenerowania nowego klucza. Są to **CLIENT_SECRET** połączeń interfejsu API. 

     ![Konfigurowanie aplikacji][12]

    >[AZURE.IMPORTANT] Ten klucz wygaśnie na końcu czasu trwania określona, upewnij się, że ją odnowić, gdy to nastąpi w przeciwnym razie uwierzytelnienie interfejsu API nie będą już działać. Możesz również usunąć i ponownie utworzyć ten klucz, jeśli uważasz, że jest już bezpieczne.
 
13. Kliknij przycisk **Widok punkty końcowe** teraz której zostanie otwarte okno dialogowe **Punkty końcowe aplikacji** . 

    ![][13]

14. W oknie dialogowym punkty końcowe aplikacji skopiuj punkt **Końcowy TOKEN OAUTH 2.0**. 

    ![][14]

15. Ten punkt końcowy będzie w następującym formacie GUID w adresie URL w przypadku usługi **TENANT_ID** , dlatego należy je zapisać: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Teraz możemy będzie kontynuować konfigurowanie uprawnień w tej aplikacji. W tym należy otworzyć [Azure portal](https://portal.azure.com). 

17. Kliknij pozycję **Grupy zasobów** i znajdowanie grupa zasobów **Zaangażowania Mobile** .  

    ![][15]

18. Kliknij pozycję Grupa zasobów **Zaangażowania Mobile** i przejdź do karta **Ustawienia** w tym miejscu. 

    ![][16]

19. Kliknij pozycję **użytkowników** w karta Ustawienia, a następnie kliknij na **Dodaj** , aby dodać użytkownika. 

    ![][17]

20. Polecenie **Wybierz rolę**

    ![][18]

21. Polecenie **właściciel**

    ![][19]

22. Wyszukaj nazwę aplikacji **AD\_aplikacji\_nazwę** w polu wyszukiwania. Nie zobaczysz to domyślnie tutaj. Po znalezieniu ją, zaznacz go i kliknij pozycję **Wybierz** u dołu karta. 

    ![][20]

23. Na karta **Dodawanie dostęp** pojawi się on jako **1 użytkownika, grupy 0**. Kliknij **przycisk OK** na to karta, aby potwierdzić zmiany. 

    ![][21]

Ukończono konfigurację AAD wymagane i możesz jest ustawiony na połączenie za pośrednictwem interfejsów API. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



