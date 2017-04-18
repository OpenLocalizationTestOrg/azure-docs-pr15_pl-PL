<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z TOPdesk - publicznej | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą TOPdesk - publicznej z usługi Azure Active Directory, aby włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych!" 
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

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Samouczek: Integracja z katalogiem Azure z TOPdesk - publicznej

Celem tego samouczka jest pokazanie integracji Azure i TOPdesk - publicznej.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   A TOPdesk - publicznej rejestracji jednokrotnej włączone subskrypcji
  
Ten samouczek użytkowników Azure AD zostały przypisane do TOPdesk — publicznej będą mogli rejestracji jednokrotnej do aplikacji u TOPdesk - witryny publicznej firmy (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla TOPdesk - publicznej
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Scenariusz")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>Włączanie integracji aplikacji dla TOPdesk - publicznej
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla TOPdesk - publicznej.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla TOPdesk - publicznego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **TOPdesk - publicznej**.

    ![Galeria aplikacji] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Galeria aplikacji")

7.  W okienku wyników wybierz **TOPdesk - publiczna**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![TOPdesk publicznej] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk publicznej")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania TOPdesk - publicznej przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla TOPdesk - publicznej wymaga przekazywanie pliku ikona logo. Aby pobrać plik ikony, skontaktuj się z zespołem pomocy technicznej TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy **TOPdesk - publicznej** jako administrator.

2.  W menu **TOPdesk** kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Ustawienia")

3.  Kliknij pozycję **Ustawienia logowania**.

    ![Ustawienia logowania] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Ustawienia logowania")

4.  Rozwiń menu **Ustawienia logowania** , a następnie kliknij pozycję **Ogólne**.

    ![Ogólne] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Ogólne")

5.  W sekcji **publicznej** sekcji konfiguracji **SAML logowania** wykonaj następujące czynności:

    ![Ustawienia techniczne] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Ustawienia techniczne")

    1.  Kliknij przycisk **Pobierz** o pobranie pliku metadanych publicznej, a następnie zapisz ją lokalnie na komputerze.
    2.  Otwórz plik metadanych, a następnie odszukaj węzeł **AssertionConsumerService** .
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Skopiuj wartość **AssertionConsumerService** .  

        >[AZURE.NOTE] Wartości w sekcji **Konfigurowanie adresu URL aplikacji** jest potrzebna w dalszej części tego samouczka.

6.  W oknie przeglądarki innej witryny sieci web Zaloguj się do sieci **portal Azure klasyczny** jako administrator.

7.  Na stronie integracji aplikacji **TOPdesk - publicznej** kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Konfigurowanie logowania jednokrotnego")

8.  Na stronie **jak chcesz użytkownikom logowanie do TOPdesk - publicznej** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Konfigurowanie logowania jednokrotnego")

9.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **TOPdesk - publiczny logowania na adres URL** wpisz adres URL używane przez użytkowników, aby zalogować się do swojego TOPdesk - publicznej aplikacji (np.: "*https://qssolutions.topdesk.net*").
    2.  W polu tekstowym **TOPdesk — publicznego adresu URL Odpowiedz** Wklej **TOPdesk - publicznego adresu URL AssertionConsumerService** (np.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Kliknij przycisk **Dalej**.

10. Na stronie **Konfigurowanie logowania jednokrotnego w TOPdesk - publicznej** Aby pobrać plik metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Konfigurowanie logowania jednokrotnego")

11. Aby utworzyć plik certyfikatu, wykonaj następujące czynności:

    ![Certyfikat] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Certyfikat")

    1.  Otwórz plik pobrany metadane.
    2.  Rozwiń węzeł **RoleDescriptor** **xsi: type** z **rana: ApplicationServiceType**.
    3.  Skopiuj wartość węzła **certyfikacie x 509** .
    4.  Zapisz wartość **certyfikacie x 509** skopiowanych lokalnie na komputerze w pliku.

12. Na swojej TOPdesk - publicznej firmowej witryny, w **TOPdesk** menu, kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Ustawienia")

13. Kliknij pozycję **Ustawienia logowania**.

    ![Ustawienia logowania] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Ustawienia logowania")

14. Rozwiń menu **Ustawienia logowania** , a następnie kliknij pozycję **Ogólne**.

    ![Ogólne] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Ogólne")

15. W sekcji **publicznej** kliknij przycisk **Dodaj**.

    ![SAML logowania] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML logowania")

16. Na stronie okno dialogowe **Asystent konfiguracji SAML** wykonaj następujące czynności:

    ![Asystent konfiguracji SAML] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "Asystent konfiguracji SAML")

    1.  Przekazywanie pliku pobranego metadanych w obszarze **Metadane Federacji**kliknij przycisk **Przeglądaj**.
    2.  Aby przekazać plik certyfikatu, w obszarze **Certyfikatu (RSA)**, kliknij przycisk **Przeglądaj**.
    3.  Aby przekazać plik logo, które masz od zespołu pomocy technicznej TOPdesk, w obszarze **ikona Logo**, kliknij przycisk **Przeglądaj**.
    4.  W polu tekstowym **atrybut nazwy użytkownika** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  W polu tekstowym **Nazwa wyświetlana** wpisz nazwę swojej konfiguracji.
    6.  Kliknij przycisk **Zapisz**.

17. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do TOPdesk - publicznego, musi być przygotowana do TOPdesk - publicznej.  
W przypadku TOPdesk - publicznego, inicjowania obsługi administracyjnej jest zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy **TOPdesk - publicznej** jako administrator.

2.  W menu u góry kliknij **TOPdesk \> nowy \> plików pomocy technicznej \> osoby**.

    ![Osoby] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Osoby")

3.  W oknie dialogowym nowej osoby wykonaj następujące czynności:

    ![Nową osobę] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Nową osobę")

    1.  Kliknij kartę Ogólne.
    2.  W polu tekstowym nazwisko wpisz nazwisko działające konto usługi Azure Active Directory, potrzebne do świadczenia.
    3.  Wybierz **witrynę** dla konta.
    4.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Możesz użyć dowolnego innego TOPdesk — narzędzia do tworzenia konta użytkownika publicznej lub interfejsów API dostarczony przez TOPdesk - publiczna, aby zabezpieczanie AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Aby przypisać użytkowników do TOPdesk - publicznego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **TOPdesk - publicznej **kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).