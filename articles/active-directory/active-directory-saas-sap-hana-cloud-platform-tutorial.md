<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z platformy chmury HANA SAP | Microsoft Azure" 
    description="Dowiedz się, jak użyć platformy chmury HANA SAP z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Samouczek: Azure Active Directory Integracja z platformy chmury HANA SAP
  
Celem tego samouczka jest pokazanie integracji Azure i platformy chmury HANA SAP.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Konto platformy chmury HANA SAP
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do SAP HANA chmury platformy będą mogli pojedynczy Zaloguj się do aplikacji przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).

>[AZURE.IMPORTANT]Należy wdrożyć aplikację własne lub subskrybowanie aplikacji na Twoim koncie SAP HANA chmury platformy do testowania rejestracji jednokrotnej na. W tym samouczku aplikacji zostanie wdrożony w oknie konta.
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji platformy chmury HANA SAP
2.  Konfigurowanie logowania jednokrotnego
3.  Przypisywanie roli do użytkownika
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenariusz")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Włączanie integracji aplikacji platformy chmury HANA SAP
  
Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji platformy chmury HANA SAP.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Aby włączyć integracji aplikacji platformy chmury HANA SAP, wykonaj następujące czynności:

1.  W portalu zarządzania Azure w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Platformy chmury HANA SAP**.

    ![Galeria aplikacji] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Galeria aplikacji")

7.  W okienku wyników wybierz **Platformę chmury HANA SAP**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![SAP Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania platformy chmury HANA SAP przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do dzierżawy usługi SAP HANA chmury platformy.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji **SAP HANA chmury platformy** aplikacji kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do SAP HANA chmury platformy** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurowanie logowania jednokrotnego")

3.  W oknie przeglądarki sieci web innej logowanie do panelu sterowania platformy systemu SAP HANA chmury u https://account. \<hosta pozioma\>.ondemand.com/cockpit (przykład: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Kliknij kartę **zaufania** .

    ![Ufaj] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Ufaj")

5.  W sekcji Zarządzanie zaufania należy wykonać następujące czynności:

    ![Uzyskiwanie metadane] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Uzyskiwanie metadane")

    1.  Kliknij kartę **Lokalny dostawca usług** .
    2.  Aby pobrać plik metadanych SAP HANA chmury platformę, kliknij **Uzyskiwanie metadanych**.

6.  W portalu klasyczny Azure Active, na stronie **Konfigurowanie adresu URL aplikacji** należy wykonać następujące czynności, a następnie kliknij **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **Logowania na adres URL** wpisz adres URL używane przez użytkowników, aby zalogować się do aplikacji **Platformy chmury HANA SAP** . To jest adres URL charakterystyczne dla kont chronionego zasobu w aplikacji SAP HANA chmury platformy. Adres URL jest oparty na następującego wzorca: *https://\<applicationName\>\<nazwa konta\>.\< Orientacja pozioma hosta\>.ondemand.com/\<ścieżki\_do\_chroniony\_zasobów\> * (np.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Jest to adres URL w aplikacji SAP HANA chmury platformy wymagającego uwierzytelnienia użytkownika.

    2.  Otwórz pobrany plik platformy chmury HANA SAP w metadanych, a następnie zlokalizuj znacznik **ns3:AssertionConsumerService** .
    3.  Skopiuj wartość atrybutu **lokalizacji** , a następnie wklej go w polu tekstowym **Adres URL SAP HANA chmury platformy odpowiedź** .

7.  Na stronie **Konfigurowanie logowania jednokrotnego w SAP HANA chmury platformy** Aby pobrać metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurowanie logowania jednokrotnego")

8.  Na SAP HANA chmury platformy Panelu sterowania w sekcji **Lokalny dostawca usług** , wykonaj następujące czynności:

    ![Zaufanie dla zarządzania] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Zaufanie dla zarządzania")

    1.  Kliknij przycisk **Edytuj**.
    2.  Jako **Typ konfiguracji**wybierz pozycję **niestandardowe**.
    3.  Jak **Lokalne Nazwa dostawcy**pozostaw wartość domyślna.
    4.  Aby wygenerować **Klucza podpisywania** i parę kluczy **Certyfikatu podpisywania** , kliknij pozycję **Generuj para klucz**.
    5.  **Propagowanie kapitału**zaznaczyć **wyłączony**.
    6.  **Wymuszanie uwierzytelniania**wybierz pozycję **wyłączone**.
    7.  Kliknij przycisk **Zapisz**.

9.  Kliknij kartę **Zaufanego dostawcy tożsamości** , a następnie kliknij przycisk **Dodawanie zaufanego dostawcy tożsamości**.

    ![Zaufanie dla zarządzania] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Zaufanie dla zarządzania")

    >[AZURE.NOTE]Aby zarządzać listą zaufanych dostawców tożsamości, musisz wybrany typ konfiguracji niestandardowe w sekcji lokalny dostawca usług. Domyślny typ konfiguracji masz nie można edytować i niejawnych zaufania Identyfikatora usługi SAP. Dla Brak nie jest dowolne ustawienia Centrum zaufania.

10. Kliknij kartę **Ogólne** , a następnie kliknij przycisk **Przeglądaj,** Aby przekazać plik pobrany metadanych.

    ![Zaufanie dla zarządzania] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Zaufanie dla zarządzania")

    >[AZURE.NOTE] Po przekazaniu pliku metadanych, wartości **URL rejestracji jednokrotnej**, **Pojedynczy Wyloguj adresu URL** i **Certyfikatu podpisywania** zostaną wypełnione automatycznie.

11. Kliknij kartę **atrybuty** .

12. Na karcie **atrybuty** wykonaj następujące czynności:

    ![Atrybuty] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Atrybuty")

    1.  Klikając **Atrybut Add Assertion-Based**, Dodaj następujące atrybuty oparte na potwierdzenia:

        |Atrybut potwierdzenia| Atrybut kapitału|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName|   Imię|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname|        nazwisko|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress|Adres e-mail|

    >[AZURE.NOTE]Konfiguracja atrybutów zależy od tego, jak aplikacje na HCP są rozwinięte, to znaczy atrybutów, które spodziewają w odpowiedzi SAML i w obszarze które name (atrybut kapitału) będą uzyskiwać dostęp do tego atrybutu w kodzie.
    >  
    >.  **Domyślny atrybut** ekranu jest tylko do celów ilustracji. Wprowadź scenariusz pracy nie jest wymagane.  
    >
    >b.  Nazwy i wartości dla **Atrybutu kapitału** w zrzut ekranu są zależy od tego, jak opracowanych aplikacji. Istnieje możliwość, że aplikacja wymaga innego mapowania.

13. W portalu klasyczny Azure, na stronie dialog **Konfigurowanie logowania jednokrotnego w SAP HANA chmury platformy** wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **wykonane**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurowanie logowania jednokrotnego")
  
Jako krok opcjonalny możesz skonfigurować grupy oparte na potwierdzenia dla swojego Azure Active Directory tożsamości dostawcy

>[AZURE.NOTE]Korzystanie z grup na platformie chmury HANA SAP pozwala na dynamiczne przypisywanie jednego lub kilku użytkowników do jednej lub więcej ról w aplikacjach SAP HANA chmury platformy uzależnione od wartości atrybutów w potwierdzenia SAML 2.0. Na przykład potwierdzenia zawiera atrybut "*Umowy = tymczasowe*", może być wszystkich odpowiednich użytkowników, aby dodać do grupy "*tymczasowy*". Grupa "*tymczasowy*" może zawierać jedną lub więcej ról z jedną lub więcej aplikacji wdrożone na koncie platformy chmury HANA SAP.
>  
>Użyj grupy oparte na potwierdzenia, jeśli chcesz masy przypisywanie wielu użytkowników do ról jeden lub więcej aplikacji na swoim koncie platformy chmury HANA SAP. Jeśli chcesz przypisać jednej lub mała liczba użytkowników () określone role zaleca się przypisywanie ich bezpośrednio na karcie "**zezwolenia**" w Panelu sterowania systemu SAP HANA chmury platformy.

##<a name="assigning-a-role-to-a-user"></a>Przypisywanie roli do użytkownika
  
W celu umożliwienia użytkownikom zalogować się do systemu SAP HANA chmury platformy Azure AD, możesz przypisywać role platformy chmury HANA SAP do nich.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Aby przypisać rolę do użytkownika, wykonaj następujące czynności:

1.  Zaloguj się do panelu sterowania usługi **SAP HANA chmury platformy** .

2.  Wykonaj następujące czynności:

    ![Zezwolenia] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Zezwolenia")

    1.  Kliknij pozycję **autoryzacji**.
    2.  Kliknij kartę **Użytkownicy** .
    3.  W polu tekstowym **użytkownika** wpisz adres e-mail użytkownika.
    4.  Kliknij przycisk **Przypisz** przypisać użytkowników do roli.
    5.  Kliknij przycisk **Zapisz**.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Aby przypisać użytkowników do SAP HANA chmury platformy, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **SAP HANA chmury platformy** kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).