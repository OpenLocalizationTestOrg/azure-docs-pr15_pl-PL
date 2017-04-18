<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z NetDocuments | Microsoft Azure" 
    description="Dowiedz się, jak użyć NetDocuments z usługi Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Samouczek: Azure Active Directory Integracja z NetDocuments
  
Celem tego samouczka jest pokazanie integracji Azure i NetDocuments.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy NetDocuments
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do NetDocuments będą mogli rejestracji jednokrotnej do aplikacji w witrynie firmy NetDocuments (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla NetDocuments
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenariusz")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Włączanie integracji aplikacji dla NetDocuments
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla NetDocuments.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla NetDocuments, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **NetDocuments**.

    ![Galeria aplikacji] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Galeria aplikacji")

7.  W okienku wyników wybierz **NetDocuments**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania NetDocuments przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla NetDocuments wymaga pobierania wartości odcisku z certyfikatu.  
Jeśli nie znasz tej procedury, zobacz [sposób pobierania wartości odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **NetDocuments** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do NetDocuments** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników w celu Logowanie do aplikacji NetDocuments (np.: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  W polu tekstowym **Adres URL odpowiedź NetDocuments** wpisz tę samą wartość zostały wpisane w polu tekstowym **Logowania na adres URL** .  

        >[AZURE.NOTE]Można znaleźć prawidłową wartość na koniec okna dialogowego **Do federacji tożsamości** (zobacz zrzut ekranu w kroku 9).

    3.  Kliknij przycisk **Dalej**

4.  Na stronie **Konfigurowanie logowania jednokrotnego w NetDocuments** Aby pobrać certyfikatu, kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy NetDocuments jako administrator.

6.  Przejdź do pozycji **Administrator**.

7.  Kliknij przycisk **Dodaj i Usuń użytkowników i grup**.

    ![Repozytorium] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repozytorium")

8.  Kliknij przycisk **Konfiguruj zaawansowane opcje uwierzytelniania**.

    ![Konfigurowanie zaawansowanych opcji uwierzytelniania] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Konfigurowanie zaawansowanych opcji uwierzytelniania")

9.  W oknie dialogowym **Federacji tożsamości** wykonaj następujące czynności:

    ![Federacji Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federacji Identitty")

    1.  **Typ serwera federacji tożsamości**zaznacz **Active Directory Federation Services**.
    2.  Kliknij pozycję **Wybierz plik**, aby przekazać plik pobrany metadanych.
    3.  Kliknij **przycisk OK**.

10. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do NetDocuments, musi być przygotowana do NetDocuments.  
W przypadku NetDocuments inicjowania obsługi administracyjnej to zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  SING do witryny firmy **NetDocuments** jako administrator.

2.  W menu u góry kliknij pozycję **Administrator**.

    ![Administrator] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Administrator")

3.  Kliknij przycisk **Dodaj i Usuń użytkowników i grup**.

    ![Repozytorium] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repozytorium")

4.  W polu tekstowym **Adres E-mail** wpisz adres e-mail działające konto usługi Azure Active Directory udzielenia, a następnie kliknij przycisk **Dodaj użytkownika**.

    ![Adres e-mail] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Adres e-mail")

    >[AZURE.NOTE]Właściciel konta usługi Azure Active Directory otrzymają wiadomości e-mail, która zawiera łącze o potwierdzenie konto, zanim będzie aktywna.

>[AZURE.NOTE]Można użyć dowolnego innego NetDocuments użytkownika konta narzędzia do tworzenia lub interfejsów API dostarczane przez NetDocuments zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Aby przypisać użytkowników do NetDocuments, należy wykonać następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **NetDocuments **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).