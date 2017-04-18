<properties 
    pageTitle="Samouczek: Konfigurowanie Workday ruchu przychodzącego synchronizacji | Microsoft Azure" 
    description="Dowiedz się, jak użyć Workday jako źródła danych tożsamości dla usługi Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Samouczek: Konfigurowanie Workday ruchu przychodzącego synchronizacji


Celem tego samouczka jest wyświetlane są kroki, które należy wykonać w pracy i Azure AD, aby zaimportować kontakty z pracy do Azure AD. 

Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure AD Premium
-   Dzierżawy w pracy
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1. Włączanie integracji aplikacji do pracy 


2. Tworzenie użytkownika systemu integracji 


3. Tworzenie grupy zabezpieczeń 


4. Przypisywanie użytkownika systemu integracji do grupy zabezpieczeń 


5. Konfigurowanie opcji grupy zabezpieczeń 


6. Aktywowanie zmiany zasad zabezpieczeń 


7. Konfigurowanie importowania użytkowników w Azure AD 



##<a name="enabling-the-application-integration-for-workday"></a>Włączanie integracji aplikacji do pracy
  
Celem w tej sekcji jest konspektu, jak włączyć integracji aplikacji do pracy.

### <a name="steps"></a>Kroki:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Dodawanie aplikacji")

  
5. W polu wyszukiwania wpisz **pracy**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Dodawanie aplikacji z gallerry")

6. W okienku wyników wybierz pracy, a następnie kliknij Zakończ, aby dodać aplikację.

    ![Galeria aplikacji] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Galeria aplikacji")





## <a name="creating-an-integration-system-user"></a>Tworzenie użytkownika systemu integracji

### <a name="steps"></a>Kroki:


1. W **Pracy Workbench**wprowadź Utwórz użytkownika w polu wyszukiwania, a następnie kliknij **Utwórz użytkownika systemu integracji**. 

    ![Tworzenie użytkownika] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Tworzenie użytkownika")



2. Wykonywanie zadania **Utwórz użytkownika systemu integracji** , podając nazwę użytkownika i hasło dla nowego użytkownika systemu integracji.  Pozostaw wymagają nowe hasło w następnej Zaloguj się opcja nie jest zaznaczona, ponieważ tego użytkownika będą logować się programowo. Pozostaw wartość domyślną 0, co uniemożliwi sesji użytkownika limit czasu przed czasem minut limit czasu sesji. 

    ![Utwórz użytkownika systemu integracji] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Utwórz użytkownika systemu integracji")
 




## <a name="creating-a-security-group"></a>Tworzenie grupy zabezpieczeń

Scenariusz przedstawionych w tym samouczku należy utworzyć grupę zabezpieczeń systemu nieograniczony integracji i przypisać użytkownikowi.

### <a name="steps"></a>Kroki:

1. Wprowadź Tworzenie grupy zabezpieczeń w polu wyszukiwania, a następnie kliknij **Utwórz grupę zabezpieczeń**. 

    ![Grupa CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "Grupa CreateSecurity")
 

2. Wykonywanie zadania Tworzenie grupy zabezpieczeń.  Zaznacz grupę zabezpieczeń systemu integracji — nieograniczone z listy rozwijanej Typ grupy zabezpieczeń dzierżawcza, aby utworzyć grupę zabezpieczeń, do którego członkowie zostaną jawnie dodane. 

    ![Grupa CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "Grupa CreateSecurity")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Przypisywanie użytkownika systemu integracji do grupy zabezpieczeń

### <a name="steps"></a>Kroki:


1. W polu wyszukiwania wprowadź edytowanie grupy zabezpieczeń, a następnie kliknij **Edytowanie grupy zabezpieczeń**. 

    ![Edytowanie grupy zabezpieczeń] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Edytowanie grupy zabezpieczeń")
 
 

2. Wyszukaj i wybierz pozycję Nowa grupa zabezpieczeń integracji według nazwy. 

    ![Edytowanie grupy zabezpieczeń] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Edytowanie grupy zabezpieczeń")

 

3. Dodawanie nowego użytkownika systemu integracji do nowej grupy zabezpieczeń. 

    ![Grupy zabezpieczeń systemu] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Grupy zabezpieczeń systemu")  




## <a name="configuring-security-group-options"></a>Konfigurowanie opcji grupy zabezpieczeń

W tym kroku udzielić na nowe uprawnienia grupy zabezpieczeń dla operacji **Get** i **umieszczenie** w obiektów zabezpieczonych przez następujące zasady zabezpieczeń domeny:

- Konta zewnętrznego inicjowania obsługi administracyjnej.

- Dane pracownika: Raporty Pracownik publicznej

- Dane pracownika: Wszystkie pozycje

- Dane pracownika: Bieżące personelu informacji

- Dane pracownika: Tytuł firm na profil pracownika

 
### <a name="steps"></a>Kroki:

1. W polu wyszukiwania wprowadź zasady zabezpieczeń domeny, a następnie kliknij łącze zasady zabezpieczeń domeny dla obszarów funkcjonalnych.  

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Zasady zabezpieczeń domeny")  
 

2. Wyszukaj systemu i zaznacz obszar funkcjonalności **systemu** .  Kliknij **przycisk OK**.  

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Zasady zabezpieczeń domeny")  


3. Na liście zasad zabezpieczeń dla obszaru funkcjonalności systemu rozwiń Administracja ubezpieczenia i wybierz zasady zabezpieczeń domeny zewnętrznej obsługi administracyjnej konta.  

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Zasady zabezpieczeń domeny")  


4. Kliknij przycisk **Edytuj uprawnienia**, a następnie, na stronie okno dialogowe **Edytowanie uprawnień**Dodaj nową grupę zabezpieczeń na liście grup zabezpieczeń z uprawnieniami integracji **Uzyskiwanie** i **umieszczenie** . 

    ![Uprawnienia do edycji] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Uprawnienia do edycji")  

 

5. Powtórz krok 1 powyżej, aby powrócić do ekranu do zaznaczania obszarów funkcjonalnych i tym razem wyszukiwania dla personelu, zaznacz obszar funkcjonalny Staffing, a następnie kliknij przycisk **OK**.

    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Zasady zabezpieczeń domeny")  
 

6. Na liście zasad zabezpieczeń dla obszaru funkcjonalności Staffing Rozwiń dane pracownika: Staffing i powtórz krok 4 powyżej dla każdej z tych pozostałych zasad zabezpieczeń:

     - Dane pracownika: Raporty Pracownik publicznej

     - Dane pracownika: Wszystkie pozycje

     - Dane pracownika: Bieżące personelu informacji

     - Dane pracownika: Tytuł firm na profil pracownika


    ![Zasady zabezpieczeń domeny] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Zasady zabezpieczeń domeny")  







## <a name="activating-security-policy-changes"></a>Aktywowanie zmiany zasad zabezpieczeń

### <a name="steps"></a>Kroki:

1. Wprowadź w polu wyszukiwania, a następnie kliknij łącze Aktywuj oczekujące zmiany zasad zabezpieczeń. 

    ![Aktywowanie] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktywowanie") 
 

2. Rozpoczęcia zadania aktywować oczekujące zmiany zasad zabezpieczeń, wprowadzając komentarz na potrzeby inspekcji, a następnie kliknij **przycisk OK**. 

    ![Aktywowanie oczekujących zabezpieczeń] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Aktywowanie oczekujących zabezpieczeń")   
 

3. Wykonywanie zadania na następnym ekranie, zaznaczając pole wyboru oznakowane Potwierdź, a następnie kliknij **przycisk OK**. 

    ![Aktywowanie oczekujących zabezpieczeń] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Aktywowanie oczekujących zabezpieczeń")  





## <a name="configuring-user-import-in-azure-ad"></a>Konfigurowanie importowania użytkowników w Azure AD

Celem w tej sekcji jest konspektu, jak skonfigurować Azure AD, aby zaimportować kontakty z pracy.


### <a name="steps"></a>Kroki:


1. Na stronie integracji **Workday** aplikacji kliknij pozycję **Importowanie konfiguracji użytkownika** , aby otworzyć okno dialogowe **Konfigurowanie inicjowania obsługi administracyjnej** .


2. Na stronie **Ustawienia i poświadczenia administratora** należy wykonać następujące czynności, a następnie kliknij przycisk **Dalej**: 

    ![Ustawienia i poświadczenia administratora] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Ustawienia i poświadczenia administratora")  
 
    . W tekstowym Nazwa użytkownika administratora Workday, wpisz nazwę użytkownika, został utworzony w tworzenie integracji systemu użytkownika sekcji.

    b. W polu tekstowym hasło administratora Workday wpisz hasło użytkownika został utworzony w tworzenie sekcją integracji systemu użytkownika.

    c. W polu tekstowym adres URL Workday dzierżawy wpisz adres URL lub dzierżawy swojego dnia pracy.


3. Na stronie **Połączenie testowe** kliknij przycisk **Rozpocznij test** , aby potwierdzić połączenie, a następnie kliknij przycisk **Dalej**. 

    ![Testuj połączenie] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Testuj połączenie")  
 

4. Na stronie opcje **obsługi** kliknij przycisk **Dalej**. 

    ![Opcje rozbudowy] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Opcje rozbudowy")


5. W oknie dialogowym **Rozpoczynanie inicjowania obsługi administracyjnej** kliknij przycisk **Zakończ**. 

    ![Rozpoczynanie inicjowania obsługi administracyjnej] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Rozpoczynanie inicjowania obsługi administracyjnej")
 

Możesz teraz przejdź do sekcji **Użytkownicy** i sprawdź, czy użytkownik dzień roboczy został zaimportowany.



## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
