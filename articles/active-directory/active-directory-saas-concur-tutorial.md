<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Concur | Microsoft Azure" 
    description="Dowiedz się, jak użyć Concur z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Samouczek: Azure Active Directory Integracja z Concur  


Celem tego samouczka jest pokazanie integracji Azure i Concur.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy w sekcji Concur

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Concur
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-concur-tutorial/IC769766.png "Scenariusz")

>[AZURE.NOTE] Konfiguracja subskrypcji Concur dla federacyjnych logowania jednokrotnego za pośrednictwem SAML jest oddzielnego zadania należy skontaktować się z Concur do wykonania.

##<a name="enabling-the-application-integration-for-concur"></a>Włączanie integracji aplikacji dla Concur

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Concur.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Concur, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Usługi Active Directory")

2.  Korzystając z **katalogu** listy, wybierz pozycję katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-concur-tutorial/IC700994.png "Aplikacje")

4.  Aby otworzyć **Galerię aplikacji**, kliknij pozycję **Dodaj aplikację**, a następnie kliknij **Dodawanie aplikacji dla swojej organizacji użyć**.

    ![Co chcesz zrobić?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Co chcesz zrobić?")

5.  W **polu wyszukiwania**wpisz **Concur**.

    ![Galeria aplikacji] (./media/active-directory-saas-concur-tutorial/IC721727.png "Galeria aplikacji")

6.  W okienku wyników wybierz **Concur**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Cząstkowe] (./media/active-directory-saas-concur-tutorial/IC721728.png "Cząstkowe")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Concur przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

>[AZURE.NOTE] Konfiguracja subskrypcji Concur dla federacyjnych logowania jednokrotnego za pośrednictwem SAML jest oddzielnego zadania należy skontaktować się z Concur do wykonania.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Concur **integracja aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-concur-tutorial/IC769767.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Concur** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-concur-tutorial/IC769768.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Cząstkowe adres URL logowania** wpisz swój concur dzierżawy logowania adres URL, a następnie kliknij przycisk **Dalej**: 

    ![Konfigurowanie Zaloguj adresu URL] (./media/active-directory-saas-concur-tutorial/IC769769.png "Konfigurowanie Zaloguj adresu URL")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Concur** wykonaj następujące czynności.

    ![Konfigurowanie Zaloguj adresu URL] (./media/active-directory-saas-concur-tutorial/IC769770.png "Konfigurowanie Zaloguj adresu URL")

    1.  Kliknij pozycję Pobierz metadanych, a następnie bezpieczne pliku danych na komputerze.
    2.  Skontaktuj się z zespołem pomocy technicznej Concur skonfigurowanie rejestracji Jednokrotnej dla dzierżawy.
    3.  Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .  

    >[AZURE.NOTE] Konfiguracja subskrypcji Concur dla federacyjnych logowania jednokrotnego za pośrednictwem SAML jest oddzielnego zadania należy skontaktować się z Concur do wykonania.

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Celem w tej sekcji jest konspektu, jak włączyć inicjowania obsługi administracyjnej konta użytkowników usługi Active Directory w celu Concur.

Aby umożliwić aplikacji w usłudze wydatków, musi być prawidłową konfigurację i korzystanie z nich profilu Administrator usługi sieci Web. Po prostu nie dodawaj rola administratora WS do istniejącego profilu administrator używanego T & E zadań administracyjnych.

Cząstkowe konsultantów lub administrator klienta musi utworzyć odrębnych profil administratora usługi sieci Web i administrator klienta, należy użyć tego profilu dla funkcji Administrator usług sieci Web (np. włączyć aplikacje). Te profile muszą być przechowywane w osobnych z administrator klienta dzienny T & E administrator profilu (Profil administratora T & E nie powinien mieć rolę WSAdmin).

Po utworzeniu profil ma być używany dla Włączanie aplikacji, wprowadź nazwę administrator klienta do pól profilu użytkownika. Spowoduje to przypisanie prawa własności do profilu. Po utworzeniu profilów klienta musisz zalogować się przy użyciu tego profilu kliknij przycisk "*Włącz*" dla aplikacji dla partnerów w menu usługi sieci Web.

Z następujących powodów ta akcja nie należy przeprowadzić z profilem, używanych do normalnego administracji T & E.

1.  Klient musi być, w którym kliknie przycisk "*Tak*" w oknie dialog, które jest wyświetlane po włączeniu aplikacji. Ten kliknij uznaje, że klient jest gotowa na podstawie partnera dostęp do danych, więc możesz lub Partner nie kliknij ten przycisk Tak.
2.  Jeśli administrator klienta, które włączono aplikacji przy użyciu administratora T & E profilu pozostawia firmy (uzyskując profilu jest zdezaktywowany), żadnych aplikacji włączone za pomocą profilu nie będzie działać do czasu włączenia aplikacji z innego profilu WS Admin aktywne. Jest to, dlatego użytkownik ma utworzyć odrębnych administrator WS profile.
3.  Jeśli administrator pozostaną firmy, nazwę skojarzonego do profilu Administrator WS mogą być zmieniane administratorowi zamienny, w razie potrzeby bez wpływania, że włączone aplikacji, ponieważ tego profilu nie jest konieczne zdezaktywowany

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **Concur** .

2.  Z menu **Administracja** wybierz **Usług sieci Web**.

    ![Concur dzierżawy] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur dzierżawy")

3.  Po lewej stronie w okienku **Usług sieci Web** wybierz **Włączyć aplikację partnera**.

    ![Włączanie aplikacji partnera] (./media/active-directory-saas-concur-tutorial/IC721730.png "Włączanie aplikacji partnera")

4.  Na liście **Włączanie aplikacji** wybierz **Usługi Azure Active Directory**, a następnie kliknij **Włącz**.

    ![Usługi Active Directory platformy Microsoft Azure] (./media/active-directory-saas-concur-tutorial/IC721731.png "Usługi Active Directory platformy Microsoft Azure")

5.  Kliknij przycisk **Tak,** aby zamknąć okno dialogowe **Potwierdzenia akcji** .

    ![Potwierdzić akcję] (./media/active-directory-saas-concur-tutorial/IC721732.png "Potwierdzić akcję")

6.  W portalu klasyczny Azure wybierz **Concur** z listy aplikacji, aby otworzyć stronę dialogową **Concur** .

7.  Aby otworzyć stronę okno dialogowe **Konfigurowanie przypisywanie użytkowników** , kliknij pozycję **Konfiguruj użytkownika inicjowania obsługi administracyjnej**.

8.  Wprowadź nazwę użytkownika i hasło administratora Concur, a następnie kliknij przycisk **Dalej**.

9.  Aby zakończyć konfigurację na stronie **potwierdzenia** kliknij przycisk **Zakończ** .

Możesz teraz utworzyć konto testowe, odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu Concur.
##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Aby przypisać użytkowników do Concur, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Concur **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-concur-tutorial/IC769771.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-concur-tutorial/IC767830.png "Tak")

Należy teraz Odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu Concur.

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
