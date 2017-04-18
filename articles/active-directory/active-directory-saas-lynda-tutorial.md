<properties 
    pageTitle="Samouczek: Integracja Azure Active Directory z witryny Lynda.com | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą witryny Lynda.com usługi Azure Active Directory można włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Samouczek: Integracja Azure Active Directory z witryny Lynda.com
  
Celem tego samouczka jest pokazanie integracji Azure i witryny Lynda.com.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy witryny Lynda.com
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do witryny Lynda.com będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny Lynda.com witryny (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla witryny Lynda.com
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Scenariusz")
##<a name="enabling-the-application-integration-for-lyndacom"></a>Włączanie integracji aplikacji dla witryny Lynda.com
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla witryny Lynda.com.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla witryny Lynda.com, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **witryny Lynda.com**.

    ![Galeria aplikacji] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Galeria aplikacji")

7.  W okienku wyników wybierz **witryny Lynda.com**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Witryny lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Witryny lynda.com")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom służą do uwierzytelniania do witryny Lynda.com przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

>[AZURE.IMPORTANT]Aby móc skonfigurować rejestracji jednokrotnej na dzierżawcy usługi witryny Lynda.com, należy skontaktować się najpierw pomocy technicznej witryny Lynda.com, aby uzyskać ta funkcja jest włączona.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **witryny Lynda.com** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do witryny Lynda.com** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Witryny Lynda.com adres URL logowania** wpisz adres URL swojej witryny Lynda.com dzierżawy (np.: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-lynda-tutorial/IC781047.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w witryny Lynda.com** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Konfigurowanie logowania jednokrotnego")

5.  Wysyłanie pliku pobranego metadanych do zespołu pomocy technicznej witryny Lynda.com. Zespół pomocy technicznej witryny Lynda.com czy konfiguracji rejestracji jednokrotnej dla Ciebie.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Nie ma żadnych czynności do wykonania do konfiguracji przypisywania do witryny Lynda.com użytkowników.  
Jeśli przypisany użytkownik próbuje zalogować się do witryny Lynda.com przy użyciu panelu programu access, witryny Lynda.com sprawdza, czy użytkownik istnieje.  
Jeśli nie konto użytkownika dostępnych jest jeszcze, są tworzone przez witryny Lynda.com.

>[AZURE.NOTE]Można użyć dowolnego innego witryny Lynda.com użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez witryny Lynda.com zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Aby przypisać użytkowników do witryny Lynda.com, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **witryny Lynda.com **integracja aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).