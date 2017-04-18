<properties 
    pageTitle="Samouczek: Integracja usługi Azure Active Directory z ArcGIS | Microsoft Azure" 
    description="Dowiedz się, jak użyć ArcGIS z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Samouczek: Integracja usługi Azure Active Directory z ArcGIS

Celem tego samouczka jest pokazanie integracji Azure i ArcGIS. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   ArcGIS logowania jednokrotnego włączone subskrypcji

Ten samouczek Azure AD użytkowników, dla których zostały przypisane do ArcGIS będą mogli pojedynczej Zaloguj się do aplikacji w witrynie firmy ArcGIS (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla ArcGIS
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Scenariusz")
##<a name="enabling-the-application-integration-for-arcgis"></a>Włączanie integracji aplikacji dla ArcGIS

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla ArcGIS.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla ArcGIS, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **ArcGIS**.

    ![Galeria Applcation] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Galeria Applcation")

7.  W okienku wyników wybierz **ArcGIS**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania ArcGIS przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **ArcGIS** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do ArcGIS** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **ArcGIS adres URL logowania** wpisz adres URL używane przez użytkowników, aby zalogować się przy użyciu następującego wzorca "*https://company.maps.arcgis.com*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w ArcGIS** kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy ArcGIS jako administrator.

6.  Kliknij przycisk **Edytuj ustawienia**.

    ![Edytowanie ustawień] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Edytowanie ustawień")

7.  Kliknij pozycję **Zabezpieczenia**.

    ![Zabezpieczenia] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Zabezpieczenia")

8.  W obszarze **Logowania przedsiębiorstwa**kliknij **Ustaw dostawcy tożsamości**.

    ![Przedsiębiorstwo logowania] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Przedsiębiorstwo logowania")

9.  Na stronie Konfiguracja **Dostawcy tożsamości zestawu** wykonaj następujące czynności:

    ![Ustawianie dostawcy tożsamości] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Ustawianie dostawcy tożsamości")

    1.  W polu tekstowym Nazwa wpisz nazwę organizacji.
    2.  Wybierz **Plik** **metadanych dla dostawcy tożsamości przedsiębiorstwa będą stosowane przy użyciu**.
    3.  Aby przekazać metadanych pobrany plik, kliknij pozycję **Wybierz plik**.
    4.  Kliknij przycisk **Ustaw dostawcy tożsamości**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Aby umożliwić użytkownikom Azure AD zalogować się do ArcGIS, musi być przygotowana do ArcGIS.  
W przypadku ArcGIS inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do Twojej dzierżawy **ArcGIS** .

2.  Kliknij polecenie **Zaproś członków**.

    ![Zapraszanie członków] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Zapraszanie członków")

3.  Wybierz pozycję **Dodaj członków automatycznie bez wysyłania wiadomości e-mail**, a następnie kliknij przycisk **Dalej**.

    ![Automatyczne dodawanie członków] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Automatyczne dodawanie członków")

4.  Na stronie okno dialogowe **członków** wykonaj następujące czynności:

    ![Dodawanie i recenzowanie] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Dodawanie i recenzowanie")

    1.  Wprowadź **Imię**, **Nazwisko** i **poczty E-mail** działające konto AAD, które mają dostarczaniem.
    2.  Kliknij przycisk **Dodaj i przejrzyj**.

5.  Przejrzyj dane, które zostały wprowadzone, a następnie kliknij pozycję **Dodaj członków**.

    ![Dodawanie członka] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Dodawanie członka")

>[AZURE.NOTE] Można użyć dowolnego innego ArcGIS użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez ArcGIS zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Aby przypisać użytkowników do ArcGIS, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **ArcGIS **aplikacji kliknij pozycję **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Tak")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
