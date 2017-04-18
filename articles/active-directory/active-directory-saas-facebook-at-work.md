<properties
    pageTitle="Samouczek: Azure Active Directory Integracja z serwisem Facebook w miejscu pracy | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować rejestracji jednokrotnej między usługi Azure Active Directory i Facebook w miejscu pracy."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Samouczek: Azure Active Directory Integracja z serwisem Facebook w miejscu pracy

Celem tego samouczka jest pokazano, jak zintegrować Facebook w pracy z usługą Azure Active Directory (Azure AD).

Integrowanie Facebook w pracy z usługą Azure Active Directory zapewnia następujące korzyści: 

- Możesz sterować w Azure AD, kto ma dostęp do serwisu Facebook w miejscu pracy 
- Automatycznie obsługi administracyjnej konta użytkowników, którzy udzielono dostępu do serwisu Facebook w miejscu pracy
- Można umożliwić użytkownikom automatyczne pobieranie podpisane w Facebook w miejscu pracy (jednokrotną) z ich kontami Azure AD
- Możesz zarządzać kont użytkownika w jednej centralnej lokalizacji 

Jeśli chcesz wiedzieć więcej szczegółów dotyczących władz akredytacji bezpieczeństwa aplikacji Integracja z usługą Azure Active Directory, zobacz [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Wymagania wstępne 

Aby skonfigurować Azure AD Integracja z CS gwiazdek, potrzebne następujące elementy:

- Subskrypcję usługi Azure AD
- Włączone Facebook w pracy z logowaniu jednokrotnym

Aby przetestować czynności w ramach tego samouczka, należy zastosowaniu tych zaleceń:

- Nie należy używać środowisku produkcyjnym, chyba że jest to konieczne.
- Jeśli nie ma środowiska wersji próbnej usługi Azure AD, możesz uzyskać miesięcznego wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Dodawanie Facebook w pracy z galerii
Aby skonfigurować integrację Facebook w miejscu pracy w usłudze Azure Active Directory, musisz dodać Facebook w pracy z galerii do listy zarządzanych aplikacji władz akredytacji bezpieczeństwa.

**Aby dodać Facebook w pracy z galerii, wykonaj następujące czynności:**

1. W **portalu klasyczny Azure**, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**. 

    ![Usługi Active Directory][1]

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
    
    ![Aplikacje][3]

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Facebook w miejscu pracy**.

7. W okienku wyników wybierz pozycję **Facebook w miejscu pracy**, a następnie kliknij **Wykonano** w celu dodania aplikacji.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie Azure AD rejestracji jednokrotnej

W tej sekcji omówiono, jak umożliwić użytkownikom uwierzytelnienia serwisem Facebook w pracy przy użyciu swojego konta w Azure Active Directory, używanie federacyjnych na podstawie protokołu SAML.

**Aby skonfigurować Azure AD rejestracji jednokrotnej z serwisem Facebook w miejscu pracy, wykonaj następujące czynności:**

1.  Po dodaniu Facebook w miejscu pracy w portalu klasyczny Azure, kliknij pozycję **Konfigurowanie logowania jednokrotnego**.

2.  Na ekranie **Konfigurowanie adresu URL aplikacji** wprowadź adres URL w miejsce, w którym użytkownicy będą zalogować się do konta w serwisie Facebook w aplikacji pracy. Jest to pod adresem URL dzierżawy Praca z serwisu Facebook (przykład: https://example.facebook.com/). Po zakończeniu kliknij przycisk **Dalej**.

3.  W oknie przeglądarki innej witryny sieci web Zaloguj się do konta w serwisie Facebook w witrynie firmy pracy jako administrator.

4. Postępuj zgodnie z instrukcjami pod adresem URL, aby skonfigurować Facebook w pracy Azure AD jako dostawcy tożsamości: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Po wykonanych, wróć do okna przeglądarki z portalem klasyczny Azure, kliknij pole wyboru, aby potwierdzić, że procedura została ukończona, a następnie kliknij przycisk **Dalej** i **Zakończ**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automatycznie inicjowania obsługi administracyjnej użytkownikom Facebook w miejscu pracy

Azure AD obsługuje możliwość automatycznie Synchronizuj szczegóły konta użytkowników przydzielonych do serwisu Facebook w miejscu pracy. Ten automatyczne sychronization umożliwia Facebook w miejscu pracy, aby pobrać dane, należy zezwolić użytkownikom dostępu wyprzedzeniem przed ich próby zalogowania się po raz pierwszy. Go też Anuluj Inicjuje obsługę administracyjną użytkowników z serwisem Facebook w miejscu pracy po dostępu został odwołany w Azure AD.

Automatyczne inicjowania obsługi administracyjnej można skonfigurować, klikając pozycję **Konfiguruj konta inicjowania obsługi administracyjnej** w oknie Azure portalu klasyczny.

Aby uzyskać dodatkowe informacje dotyczące konfigurowania automatycznego inicjowania obsługi administracyjnej zobacz [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Przypisywanie użytkowników do serwisu Facebook w miejscu pracy

Ustanawianie AAD użytkownicy będą mogli zobaczyć Facebook pracujący na ich Panel dostępu one przypisane dostępu w portalu klasyczny Azure.

**Aby przypisać użytkowników do serwisu Facebook w miejscu pracy:**

1.  Na stronie początkowej serwisu Facebook w miejscu pracy w portalu klasyczny Azure kliknij polecenie **Przypisz konta**.

2.  W menu **Pokaż** Określ, czy chcesz przypisać użytkownikowi lub grupie serwisem Facebook w miejscu pracy, a następnie kliknij przycisk znacznika wyboru.

3.  W wyświetlonej liście Wybierz użytkowników lub grupy, do której chcesz przypisać Facebook w miejscu pracy.

4.  W stopce strony kliknij przycisk **Przypisz** .


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




