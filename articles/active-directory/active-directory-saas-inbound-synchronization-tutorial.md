<properties 
    pageTitle="Samouczek: Konfigurowanie Workday ruchu przychodzącego synchronizacji | Microsoft Azure" 
    description="Dowiedz się, jak użyć ruchu przychodzącego synchronizacji z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i innych!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Samouczek: Konfigurowanie Workday ruchu przychodzącego synchronizacji
>[AZURE.NOTE]Premium Azure Active Directory (AD) jest dostępna dla klientów w Chinach za pomocą na całym świecie wystąpienia Azure AD.    
Azure AD Premium nie jest obecnie obsługiwana w usłudze Microsoft Azure obsługiwana przez firmę 21Vianet w Chinach.    

Celem tego samouczka jest wyświetlane są kroki, które należy wykonać w pracy i Microsoft Azure AD, aby zaimportować kontakty z pracy do firmy Microsoft Azure AD.    
 Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:  

-   Ważna subskrypcja Azure  
-   Dzierżawy w pracy  

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:  

1.  Włączanie integracji aplikacji do pracy  
2.  Tworzenie użytkownika systemu integracji  
3.  Tworzenie grupy zabezpieczeń  
4.  Przypisywanie użytkownika systemu integracji do grupy zabezpieczeń  
5.  Konfigurowanie opcji grupy zabezpieczeń  
6.  Aktywowanie zmiany zasad zabezpieczeń  
7.  Konfigurowanie importowania użytkowników w programie Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>Włączanie integracji aplikacji do pracy

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla usługi Salesforce.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla dnia roboczego, wykonaj następujące czynności:

1.  W portalu zarządzania Azure w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.    

    ![Usługi Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Usługi Active Directory")  

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.    

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .    

    ![Aplikacje] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Aplikacje")  

4.  Aby otworzyć **Galerię aplikacji**, kliknij pozycję **Dodaj aplikację**, a następnie kliknij **Dodawanie aplikacji dla swojej organizacji użyć**.    

    ![Co chcesz zrobić?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Co chcesz zrobić?")  

5.  W **polu wyszukiwania**wpisz **pracy**.    

    ![Dzień roboczy] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Dzień roboczy")  

6.  W okienku wyników wybierz **pracy**, a następnie kliknij **Wykonano** w celu dodania aplikacji.    

    ![Dzień roboczy] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Dzień roboczy")  

##<a name="creating-an-integration-system-user"></a>Tworzenie użytkownika systemu integracji

1.  W **Pracy Workbench**wprowadź **Utwórz użytkownika** w polu wyszukiwania, a następnie kliknij łącze **Utwórz użytkownika systemu integracji**.     

    ![Tworzenie użytkownika] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "Tworzenie użytkownika")  

2.  Wykonywanie zadania Utwórz użytkownika systemu integracji, podając nazwę użytkownika i hasło dla nowego użytkownika systemu integracji.  Pozostaw wymagają nowe hasło w następnej Zaloguj się opcja nie jest zaznaczona, ponieważ tego użytkownika będą logować się programowo.    
    Pozostaw wartość domyślną 0, co uniemożliwi sesji użytkownika limit czasu przed czasem minut limit czasu sesji.    

    ![Utwórz użytkownika systemu integracji] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Utwórz użytkownika systemu integracji")  

##<a name="creating-a-security-group"></a>Tworzenie grupy zabezpieczeń

Scenariusz przedstawionych w tym samouczku należy utworzyć grupę zabezpieczeń systemu nieograniczony integracji i przypisać użytkownikowi.    

1.  Wprowadź Tworzenie grupy zabezpieczeń w polu wyszukiwania, a następnie kliknij łącze Utwórz grupę zabezpieczeń.     

    ![Grupa CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "Grupa CreateSecurity")  

2.  Wykonywanie zadania Tworzenie grupy zabezpieczeń.  Zaznacz grupę zabezpieczeń systemu integracji — nieograniczone z listy rozwijanej Typ grupy zabezpieczeń dzierżawcza, aby utworzyć grupę zabezpieczeń, do którego członkowie zostaną jawnie dodane.     

    ![Grupa CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "Grupa CreateSecurity")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>Przypisywanie użytkownika systemu integracji do grupy zabezpieczeń

1.  W polu wyszukiwania wprowadź edytowanie grupy zabezpieczeń, a następnie kliknij łącze **Edytuj grupy zabezpieczeń**.     

    ![Edytowanie grupy zabezpieczeń] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Edytowanie grupy zabezpieczeń")  

2.  Wyszukaj, a następnie wybierz pozycję Nowa grupa zabezpieczeń integracji według nazwy    

    ![Edytowanie grupy zabezpieczeń] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Edytowanie grupy zabezpieczeń")  

3.  Dodawanie nowego użytkownika systemu integracji do nowej grupy zabezpieczeń.       

    ![Grupy zabezpieczeń systemu] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Grupy zabezpieczeń systemu")  

##<a name="configuring-security-group-options"></a>Konfigurowanie opcji grupy zabezpieczeń

W tym kroku udzielanie do nowego uprawnienia grupy zabezpieczeń dla zakresu operacji wysyłania i obiektom zabezpieczonych przez następujące zasady zabezpieczeń domeny:  

-   Konta zewnętrznego inicjowania obsługi administracyjnej.  
-   Dane pracownika: Raporty Pracownik publicznej  
-   Dane pracownika: Wszystkie pozycje  
-   Dane pracownika: Bieżące personelu informacji  
-   Dane pracownika: Tytuł firm na profil pracownika  

&nbsp;  

1.  W polu wyszukiwania wprowadź zasady zabezpieczeń domeny, a następnie kliknij łącze zasady zabezpieczeń domeny dla obszarów funkcjonalnych.     

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Zasady zabezpieczeń domeny")  

2.  Wyszukaj systemu i zaznacz obszar funkcjonalności systemu.  Kliknij przycisk oznakowane, przycisk OK.     

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Zasady zabezpieczeń domeny")  

3.  Na liście zasad zabezpieczeń dla obszaru funkcjonalności systemu rozwiń Administracja ubezpieczenia i wybierz zasady zabezpieczeń domeny zewnętrznej obsługi administracyjnej konta.     

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Zasady zabezpieczeń domeny")  

4.  Kliknij przycisk Edytuj uprawnienia, a następnie na ekranie Edytuj uprawnienia Dodaj nową grupę zabezpieczeń na liście grup zabezpieczeń z uprawnieniami integracji Odbierz i Wyślij.     

    ![Uprawnienia do edycji] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Uprawnienia do edycji")  

5.  Powtórz krok 1 powyżej, aby powrócić do ekranu do zaznaczania obszarów funkcjonalnych i tym razem wyszukiwanie personel, zaznacz obszar funkcjonalny Staffing i kliknij przycisk oznakowane OK.    

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Zasady zabezpieczeń domeny")  

6.  Na liście zasad zabezpieczeń dla obszaru funkcjonalności Staffing Rozwiń dane pracownika: Staffing i powtórz krok 4 powyżej dla każdej z tych pozostałych zasad zabezpieczeń:    

    -   Dane pracownika: Raporty Pracownik publicznej  
    -   Dane pracownika: Wszystkie pozycje  
    -   Dane pracownika: Bieżące personelu informacji  
    -   Dane pracownika: Tytuł firm na profil pracownika    

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Zasady zabezpieczeń domeny")  

##<a name="activating-security-policy-changes"></a>Aktywowanie zmiany zasad zabezpieczeń

1.  Wprowadź w polu wyszukiwania, a następnie kliknij łącze Aktywuj oczekujące zmiany zasad zabezpieczeń.    

    ![Aktywowanie] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktywowanie")  

2.   Rozpocznij zadania aktywować oczekujące zmiany zasad zabezpieczeń komentarz na potrzeby inspekcji, a następnie klikając przycisk oznakowane, przycisk OK.      

    ![Aktywowanie oczekujących zabezpieczeń] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Aktywowanie oczekujących zabezpieczeń")  

3.  Wykonywanie zadania na następnym ekranie, zaznaczając pole wyboru oznakowane Potwierdź i kliknij przycisk oznakowane, przycisk OK.     

    ![Aktywowanie oczekujących zabezpieczeń] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Aktywowanie oczekujących zabezpieczeń")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Konfigurowanie importowania użytkowników w programie Microsoft Azure AD

Celem w tej sekcji jest konspektu, jak skonfigurować program Microsoft Azure AD, aby zaimportować kontakty z pracy.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Aby skonfigurować importowania użytkowników w programie Microsoft Azure AD, wykonaj następujące czynności:

1.  Na stronie integracji **Workday** aplikacji kliknij pozycję **Importowanie konfiguracji użytkownika** , aby otworzyć okno dialogowe **Konfigurowanie inicjowania obsługi administracyjnej** .    

2.  Na stronie **Ustawienia i poświadczenia administratora** należy wykonać następujące czynności, a następnie kliknij przycisk Dalej:    

    ![Ustawienia i poświadczenia administratora] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Ustawienia i poświadczenia administratora")    

    1.  W polu tekstowym **Nazwa użytkownika administratora Workday** wpisz nazwę użytkownika, którego został utworzony w sekcji [Tworzenie integracji systemu użytkownika](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
    2.  W polu tekstowym **hasło administratora Workday** wpisz hasło użytkownika, którego został utworzony w sekcji [Tworzenie integracji systemu użytkownika](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
    3.  W polu tekstowym **adres URL dzierżawy Workday** wpisz adres URL lub dzierżawy swojego dnia pracy.    

3.  Na stronie **Połączenie testowe** kliknij przycisk **Rozpocznij test** , aby potwierdzić połączenie, a następnie kliknij przycisk **Dalej**.    

    ![Testuj połączenie] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Testuj połączenie")  

4.  Na stronie **Opcje obsługi** kliknij przycisk **Dalej**.    

    ![Opcje rozbudowy] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Opcje rozbudowy")  

5.  W oknie dialogowym **Rozpoczynanie inicjowania obsługi administracyjnej** kliknij przycisk **Zakończ**.    

    ![Rozpoczynanie inicjowania obsługi administracyjnej] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Rozpoczynanie inicjowania obsługi administracyjnej")  

Możesz teraz przejdź do sekcji **Użytkownicy** i sprawdź, czy użytkownik dzień roboczy został zaimportowany.    
