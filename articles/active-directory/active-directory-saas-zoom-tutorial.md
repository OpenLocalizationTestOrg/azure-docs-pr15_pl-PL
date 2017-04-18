<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z powiększenia | Microsoft Azure" 
    description="Dowiedz się, jak użyć powiększenia z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Samouczek: Azure Active Directory Integracja z powiększenia
  
Celem tego samouczka jest pokazanie integracja Azure i powiększenie.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy powiększenia
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do powiększenia będą mogli rejestracji jednokrotnej do aplikacji firmowej witryny powiększenia (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md)
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla powiększenia
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenariusz")

##<a name="enabling-the-application-integration-for-zoom"></a>Włączanie integracji aplikacji dla powiększenia
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla powiększenia.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla powiększenia, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **powiększenia**.

    ![Galeria aplikacji] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Galeria aplikacji")

7.  W okienku wyników zaznacz opcję **Powiększenie**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Powiększenie] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Powiększenie")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania w Azure AD przy użyciu Federacji na podstawie protokołu SAML powiększenia przy użyciu swojego konta.  
W ramach tej procedury jest wymagane do utworzenia pliku base 64 zakodowany certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **powiększenia** integracji aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak czy chcesz użytkowników do zalogowania się celu powiększenia** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** w polu tekstowym **Powiększenia adres URL logowania** wpisz swój adres URL przy użyciu następującego wzorca "*http://company.zoom.us*", a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w powiększenie** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy powiększenia jako administrator.

6.  Kliknij kartę **Rejestracji jednokrotnej** .

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Rejestracji jednokrotnej")

7.  Kliknij kartę **Kontrola zabezpieczeń** , a następnie przejdź do pozycji ustawienia **Rejestracji jednokrotnej** .

8.  W sekcji rejestracji jednokrotnej wykonaj następujące czynności:

    ![Rejestracji jednokrotnej] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Rejestracji jednokrotnej")

    1.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w powiększenia** skopiuj wartość **Pojedynczy znak na adres URL usługi** , a następnie wklej go w polu tekstowym **adres URL strony — logowania** .
    2.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w powiększenia** skopiuj wartość **Adres URL usługi Sign-Out pojedynczej** , a następnie wklej go w polu tekstowym **adres URL strony wyrejestrowywania** .
    3.  Tworzenie pliku **base 64 zakodowany** z pobranego certyfikatu.  

        >[AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otwórz base-64 certyfikatu zakodowany w Notatniku, skopiuj zawartość go do Schowka i następnie wkleić go w polu tekstowym **certyfikatu dostawcy tożsamości**
    5.  W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w powiększenia** skopiuj wartość **Adres URL wystawcy** , a następnie wklej go w polu tekstowym **wystawcy** .
    6.  Kliknij przycisk **Zapisz**.

9.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do powiększenia, musi być przygotowana do powiększenia.  
W przypadku powiększenie inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Do obsługi administracyjnej konta użytkowników, wykonaj następujące czynności:

1.  Zaloguj się do witryny firmy **powiększenia** jako administrator.

2.  Kliknij kartę **Zarządzanie kontem** , a następnie kliknij pozycję **Zarządzanie użytkownikami**.

3.  W sekcji Zarządzanie użytkownikami kliknij przycisk **Dodaj użytkowników**.

    ![Zarządzanie użytkownikami] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Zarządzanie użytkownikami")

4.  Na stronie **Dodawanie użytkowników** wykonaj następujące czynności:

    ![Dodawanie użytkowników] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Dodawanie użytkowników")

    1.  **Typ użytkownika**zaznacz **podstawowe**.
    2.  W polu tekstowym **wiadomości E-mail** wpisz adres e-mail działające konto AAD potrzebne do świadczenia.
    3.  Kliknij przycisk **Dodaj**.

>[AZURE.NOTE] Można użyć dowolnego innego powiększenia użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczony przez powiększenie udzielenia AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Aby przypisać użytkowników do powiększenia, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie **powiększenia **integracji aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).