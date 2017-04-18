<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z TOPdesk - bezpiecznego | Microsoft Azure"
    description="Dowiedz się, jak używać TOPdesk - bezpiecznego z usługi Azure Active Directory, aby włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych!." 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Samouczek: Azure Active Directory Integracja z TOPdesk - bezpieczny
  
Celem tego samouczka jest pokazanie integracji Azure i TOPdesk - bezpiecznego.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   A TOPdesk - bezpiecznego logowania jednokrotnego włączone subskrypcji
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do TOPdesk - bezpiecznego będą mogli rejestracji jednokrotnej do aplikacji u TOPdesk - bezpiecznego firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla TOPdesk - bezpieczny
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenariusz")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Włączanie integracji aplikacji dla TOPdesk - bezpieczny
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla TOPdesk - bezpiecznego.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla TOPdesk - bezpiecznego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **TOPdesk - bezpiecznego**.

    ![Galeria aplikacji] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Galeria aplikacji")

7.  W okienku wyników wybierz **TOPdesk - zabezpieczeń**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![TOPdesk - bezpieczny] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - bezpieczny")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania TOPdesk - bezpiecznego przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla TOPdesk - bezpiecznego wymaga przekazywanie pliku ikona logo. Aby pobrać plik ikony, skontaktuj się z zespołem pomocy technicznej TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy **TOPdesk - bezpiecznego** jako administrator.

2.  W menu **TOPdesk** kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Ustawienia")

3.  Kliknij pozycję **Ustawienia logowania**.

    ![Ustawienia logowania] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Ustawienia logowania")

4.  Rozwiń menu **Ustawienia logowania** , a następnie kliknij pozycję **Ogólne**.

    ![Ogólne] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Ogólne")

5.  W sekcji **bezpiecznego** **logowania SAML** sekcji konfiguracji należy wykonać następujące czynności:

    ![Ustawienia techniczne] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Ustawienia techniczne")

    1.  Kliknij przycisk **Pobierz** o pobranie pliku metadanych publicznej, a następnie zapisz ją lokalnie na komputerze.
    2.  Otwórz plik metadanych, a następnie odszukaj węzeł **AssertionConsumerService** .
        ![Usługi dla klientów indywidualnych potwierdzenia] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Usługi dla klientów indywidualnych potwierdzenia")
    3.  Skopiuj wartość **AssertionConsumerService** .  

        >[AZURE.NOTE] Wartości w sekcji **Konfigurowanie adresu URL aplikacji** jest potrzebna w dalszej części tego samouczka.

6.  W oknie przeglądarki innej witryny sieci web Zaloguj się do sieci **portal Azure klasyczny** jako administrator.

7.  Na stronie integracji **TOPdesk - bezpiecznego** aplikacji kliknij pozycję **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Konfigurowanie logowania jednokrotnego")

8.  Na stronie **jak chcesz użytkownikom logowanie do TOPdesk - bezpiecznego** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Konfigurowanie logowania jednokrotnego")

9.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Skonfigurować adres URL aplikacji")

    1.  W polu tekstowym **TOPdesk - bezpiecznego logowania na adres URL** wpisz adres URL używane przez użytkowników, aby zalogować się do swojego TOPdesk - bezpiecznego aplikacji (np.: "*https://qssolutions.topdesk.net*").
    2.  W polu tekstowym **TOPdesk — publicznego adresu URL Odpowiedz** Wklej **TOPdesk - AssertionConsumerService adres URL** (np.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Kliknij przycisk **Dalej**.

10. Na stronie **Konfigurowanie logowania jednokrotnego w TOPdesk - bezpieczne** Aby pobrać plik metadanych, kliknij przycisk **Pobierz metadanych**, a następnie zapisz plik lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Konfigurowanie logowania jednokrotnego")

11. Aby utworzyć plik certyfikatu, wykonaj następujące czynności:

    ![Certyfikat] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certyfikat")

    1.  Otwórz plik pobrany metadane.
    2.  Rozwiń węzeł **RoleDescriptor** **xsi: type** z **rana: ApplicationServiceType**.
    3.  Skopiuj wartość węzła **certyfikacie x 509** .
    4.  Zapisz wartość **certyfikacie x 509** skopiowanych lokalnie na komputerze w pliku.

12. Na swojej TOPdesk - bezpiecznego firmowej witryny, w **TOPdesk** menu, kliknij przycisk **Ustawienia**.

    ![Ustawienia] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Ustawienia")

13. Kliknij pozycję **Ustawienia logowania**.

    ![Ustawienia logowania] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Ustawienia logowania")

14. Rozwiń menu **Ustawienia logowania** , a następnie kliknij pozycję **Ogólne**.

    ![Ogólne] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Ogólne")

15. W sekcji **publicznej** kliknij przycisk **Dodaj**.

    ![Dodawanie] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Dodawanie")

16. Na stronie okno dialogowe **Asystent konfiguracji SAML** wykonaj następujące czynności:

    ![Asystent konfiguracji SAML] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "Asystent konfiguracji SAML")

    1.  Przekazywanie pliku pobranego metadanych w obszarze **Metadane Federacji**kliknij przycisk **Przeglądaj**.
    2.  Aby przekazać plik certyfikatu, w obszarze **Certyfikatu (RSA)**, kliknij przycisk **Przeglądaj**.
    3.  Aby przekazać plik logo, które masz od zespołu pomocy technicznej TOPdesk, w obszarze **ikona Logo**, kliknij przycisk **Przeglądaj**.
    4.  W polu tekstowym **atrybut nazwy użytkownika** wpisz **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  W polu tekstowym **Nazwa wyświetlana** wpisz nazwę swojej konfiguracji.
    6.  Kliknij przycisk **Zapisz**.

17. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Konfigurowanie logowania jednokrotnego")

##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Aby umożliwić użytkownikom Azure AD zalogować się do TOPdesk - bezpieczne, ich musi być przygotowana do TOPdesk - bezpiecznego.  
W przypadku TOPdesk - bezpieczne, inicjowania obsługi administracyjnej jest zadanie ręczne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  Logowanie się do witryny firmy **TOPdesk - bezpiecznego** jako administrator.

2.  W menu u góry kliknij **TOPdesk \> nowy \> plików pomocy technicznej \> Operator**.

    ![Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3.  W oknie dialogowym **Nowy Operator** wykonaj następujące czynności:

    ![Nowy Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Nowy Operator")

    1.  Kliknij kartę Ogólne.
    2.  W polu tekstowym **nazwisko** sekcji **Ogólne** wpisz nazwisko działające konto usługi Azure Active Directory, potrzebne do świadczenia.
    3.  Wybierz **witrynę** dla konta w sekcji **Lokalizacja** .
    4.  W polu tekstowym **Nazwa logowania** sekcji **TOPdesk logowania** wpisz nazwę logowania użytkownika.
    5.  Kliknij przycisk **Zapisz**.

>[AZURE.NOTE] Możesz użyć dowolnego innego TOPdesk — narzędzia do tworzenia konta użytkownika bezpiecznego lub interfejsów API dostarczony przez TOPdesk - bezpiecznego do zapewniania obsługi AAD kont użytkowników.

##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Aby przypisać użytkowników do TOPdesk - bezpiecznego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **TOPdesk - bezpiecznego **aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Tak")
  
Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).