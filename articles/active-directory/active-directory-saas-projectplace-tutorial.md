<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Projectplace | Microsoft Azure" 
    description="Dowiedz się, jak użyć Projectplace z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Samouczek: Azure Active Directory Integracja z Projectplace
  
Celem tego samouczka jest pokazanie integracja Azure i Projectplace.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Projectplace logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do Projectplace będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy Projectplace (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Projectplace
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Scenariusz")
##<a name="enabling-the-application-integration-for-projectplace"></a>Włączanie integracji aplikacji dla Projectplace
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Projectplace.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Projectplace, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Projectplace**.

    ![Galeria aplikacji] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Projectplace**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Projectplace przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **Projectplace** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Konfigurowanie logować pojedynczy")

2.  Na stronie **jak chcesz użytkownikom logowanie do Projectplace** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Konfigurowanie logować pojedynczy")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Projectplace logowania na adres URL** wpisz adres URL dzierżawy ProjectPlace (np.: "*http://company.projectplace.com*"), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Projectplace** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych na komputerze.

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Konfigurowanie logować pojedynczy")

5.  Wyślij metadatafile do zespołu pomocy technicznej Projectplace.

    >[AZURE.NOTE] Pojedynczy Konfiguracja logowania jednokrotnego zawiera mają być wykonywane przez zespół pomocy technicznej Projectplace. Otrzymasz powiadomienie natychmiast po zakończeniu konfiguracji.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logować pojedynczy] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Konfigurowanie logować pojedynczy")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do Projectplace, musi być przygotowana do Projectplace.  
W przypadku Projectplace inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **Projectplace** jako administrator.

2.  Przejdź do **widoku Kontakty**, a następnie kliknij pozycję **Członkowie**.

    ![Osoby] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Osoby")

3.  Kliknij przycisk **Dodaj członka**.

    ![Dodawanie członków] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Dodawanie członków")

4.  W sekcji **Dodaj członka,** wykonaj następujące czynności:

    ![Nowych członków] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Nowych członków")

    1.  W polu tekstowym **Nowych członków** wpisz adres e-mail prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.
    2.  Kliknij przycisk **Wyślij**

        >[AZURE.NOTE] Aby właściciel konta usługi Azure Active Directory po wysłaniu wiadomości e-mail, łącznie z łączem do potwierdzenia konta staje się aktywny.
    
>[AZURE.NOTE]Można użyć dowolnego innego Projectplace użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Projectplace zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Aby przypisać użytkowników do Projectplace, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracja aplikacji **Projectplace **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).