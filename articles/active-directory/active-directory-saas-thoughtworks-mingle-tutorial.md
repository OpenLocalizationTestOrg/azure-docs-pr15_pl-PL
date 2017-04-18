<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Thoughtworks Mingle | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą Thoughtworks Mingle usługi Azure Active Directory można włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Samouczek: Azure Active Directory Integracja z Thoughtworks Mingle
  
Celem tego samouczka jest pokazanie integracji Azure i Thoughtworks Mingle.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Thoughtworks Mingle dzierżawy
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Thoughtworks Mingle
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scenariusz")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Włączanie integracji aplikacji dla Thoughtworks Mingle
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Thoughtworks Mingle.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Thoughtworks Mingle, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **thoughtworks mingle**.

    ![Galeria aplikacji] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Thoughtworks Mingle**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Thoughtworks Mingle] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Thoughtworks Mingle przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury musisz przekazać certyfikatu do Thoughtworks Mingle.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Thoughtworks Mingle **integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do Thoughtworks Mingle** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Adres URL dzierżawy Mingle Thoughtworks** wpisz adres URL przy użyciu następującego wzorca "*http://company.mingle.thoughtworks.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Thoughtworks Mingle** kliknij przycisk Pobierz metadane, a następnie zapisz go na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Konfigurowanie logowania jednokrotnego")

5.  Zaloguj się do witryny firmy **Thoughtworks Mingle** jako administrator.

6.  Kliknij kartę **Administrator** , a następnie kliknij przycisk **Logowania jednokrotnego konfiguracji**.

    ![Konfiguracja logowania jednokrotnego] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "Konfiguracja logowania jednokrotnego")

7.  W sekcji **Konfiguracji logowania jednokrotnego** wykonaj następujące czynności:

    ![Konfiguracja logowania jednokrotnego] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "Konfiguracja logowania jednokrotnego")

    1.  Aby przekazać plik metadanych, kliknij pozycję **Wybierz plik**.
    2.  Kliknij przycisk **Zapisz zmiany**.

8.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
AAD użytkownikom będzie można się zalogować musi być przygotowana do Thoughtworks Mingle aplikacji przy użyciu nazwy użytkowników usługi Azure Active Directory.  
W przypadku Thoughtworks Mingle inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy Thoughtworks Mingle jako administrator.

2.  Kliknij **profil**.

    ![Pierwszy projektu] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Pierwszy projektu")

3.  Kliknij kartę **Administrator** , a następnie kliknij pozycję **Użytkownicy**.

    ![Użytkownicy] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Użytkownicy")

4.  Kliknij pozycję **Nowy użytkownik**.

    ![Nowy użytkownik] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Nowy użytkownik")

5.  Na stronie okno dialogowe **Nowy użytkownik** wykonaj następujące czynności:

    ![Nowy użytkownik] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Nowy użytkownik")

    1.  Wpisz **nazwę logowania**, **nazwę wyświetlaną**, **Wybierz hasło**, **Potwierdź hasło** prawidłowe konta AAD, które chcesz należy do powiązanych pól tekstowych.
    2.  Jako **Typ użytkownika**wybierz pozycję **pełne użytkownika**.
    3.  Kliknij przycisk **Utwórz tego profilu**.

>[AZURE.NOTE] Można użyć dowolnego innego Thoughtworks Mingle użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez Thoughtworks Mingle zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Aby przypisać użytkowników do Thoughtworks Mingle, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **Thoughtworks Mingle** integracji aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
