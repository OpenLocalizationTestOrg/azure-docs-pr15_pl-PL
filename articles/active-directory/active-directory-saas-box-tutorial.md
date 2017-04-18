<properties 
    pageTitle="Samouczek: Azure Active Directory pole integrację z | Microsoft Azure" 
    description="Dowiedz się, jak użyć pola z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Samouczek: Azure Active Directory pole integrację z


  
Celem tego samouczka jest pokazanie integracji Azure i pole.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Test dzierżawy w polu
  
Ten samouczek Azure AD użytkowników, dla których zostały przypisane do pola będą mogli rejestracji jednokrotnej do aplikacji w polu firmowej witryny (znak inicjowanych przez dostawcę usługi na) lub przy użyciu [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla pola
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników i grupy inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-box-tutorial/IC769537.png "Scenariusz")



##<a name="enabling-the-application-integration-for-box"></a>Włączanie integracji aplikacji dla pola
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla pola.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla pola, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-box-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-box-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-box-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  **W **polu wyszukiwania**wpisz tekst.**

    ![Galeria aplikacji] (./media/active-directory-saas-box-tutorial/IC701023.png "Galeria aplikacji")

7.  W okienku wyników zaznacz **pole**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Pole] (./media/active-directory-saas-box-tutorial/IC701024.png "Pole")



##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelnienia do pola przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML. W ramach tej procedury musisz przekazać metadanych do Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji aplikacji **pola** kliknij pozycję **Konfiguruj logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-box-tutorial/IC769538.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkownikom logowanie do pola** wybierz pozycję **Microsoft Azure AD logowania jednokrotnego**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-box-tutorial/IC769539.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Skonfigurować adres URL aplikacji** w polu tekstowym **Adres URL dzierżawy polu** wpisz adres URL dzierżawy pola (np.: https://<mydomainname>. box.com), a następnie kliknij przycisk **Dalej**.

    ![Skonfigurować adres URL aplikacji] (./media/active-directory-saas-box-tutorial/IC669826.png "Skonfigurować adres URL aplikacji")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w polu** , aby pobrać metadanych, kliknij **pobierania metadanych**, a następnie polecenie Plik danych lokalnie na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-box-tutorial/IC669824.png "Konfigurowanie logowania jednokrotnego")

5.  Przekazywanie tego pliku metadanych do zespołu pomocy technicznej. Na potrzeby zespołu pomocy technicznej konfiguruje rejestracji jednokrotnej dla Ciebie.

6.  Zaznacz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-box-tutorial/IC769540.png "Konfigurowanie logowania jednokrotnego")
##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Celem w tej sekcji jest konspektu, jak włączyć inicjowania obsługi administracyjnej usługi Active Directory kont użytkowników do pola.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1. W portalu klasyczny Azure, na stronie **okna** aplikacji integracji kliknij pozycję **Konfiguruj przypisywanie użytkowników** , aby otworzyć okno dialogowe **Konfigurowanie przypisywanie użytkowników** . 

    ![Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.] (./media/active-directory-saas-box-tutorial/IC769541.png "Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.")

2. Na stronie okno dialogowe **umożliwić użytkownikowi inicjowania obsługi administracyjnej do pola** kliknij opcję **Włącz, przypisywanie użytkowników**. 

    ![Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.] (./media/active-directory-saas-box-tutorial/IC769544.png "Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.")

3. Na stronie **Logowanie się do udzielania dostępu do pola** wymagane poświadczenia, a następnie kliknij **temat autoryzowanie**. 

    ![Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.] (./media/active-directory-saas-box-tutorial/IC769546.png "Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.")


4. Kliknij pozycję **Udzielanie dostępu do pola** , aby zezwolić tej operacji i powrócić do portalu klasyczny Azure. 

    ![Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.] (./media/active-directory-saas-box-tutorial/IC769549.png "Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.")


5. Na stronie **Opcje rozbudowy** pola wyboru **typów obiektów udzielenia** umożliwiają zaznacz, czy obiekty grupy zainicjowano obsługę administracyjną do pola oprócz obiektów użytkowników.  Aby uzyskać więcej informacji, zobacz Przypisywanie użytkowników i grup, zobacz sekcję"poniżej.


6. Aby zakończyć konfigurację, kliknij przycisk zakończenie. 

    ![Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.] (./media/active-directory-saas-box-tutorial/IC769551.png "Włącz automatyczne użytkownika inicjowania obsługi administracyjnej.")



##<a name="assigning-a-test-user"></a>Przypisywanie użytkowników testowych
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Aby przypisać użytkowników do pola, należy wykonać następujące czynności:

1. W portalu klasyczny Azure Utwórz konto testowe.

2. Na stronie **okna **aplikacji integracji kliknij polecenie **Przypisz użytkowników**. 

    ![Przypisywanie użytkowników] (./media/active-directory-saas-box-tutorial/IC769552.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania. 

    ![Tak] (./media/active-directory-saas-box-tutorial/IC767830.png "Tak")
  
Należy teraz Odczekaj 10 minut i sprawdź, czy konta zostały zsynchronizowane do pola.

Pierwszym krokiem weryfikacji możesz sprawdzić stan obsługi administracyjnej, klikając pozycję pulpitu nawigacyjnego w D na stronie pola aplikacji integracji w portalu klasyczny Azure.

![Pulpit nawigacyjny] (./media/active-directory-saas-box-tutorial/IC769553.png "Pulpit nawigacyjny")

Ukończony użytkownika inicjowania obsługi administracyjnej cyklu jest wskazywana przez powiązanych stanu:

![Stan integracji] (./media/active-directory-saas-box-tutorial/IC769555.png "Stan integracji")


W dzierżawie usługi pole synchronizowani użytkownicy są wymienione w obszarze **Zarządzania użytkownikami** w **Konsoli administracyjnej**.

![Stan integracji] (./media/active-directory-saas-box-tutorial/IC769556.png "Stan integracji")


##<a name="assigning-users-and-groups"></a>Przypisywanie użytkowników i grup

**Okno > Użytkownicy i grupy** kartę w portalu klasyczny Azure pozwala określić, którzy użytkownicy i grupy powinny mieć przyznany dostęp do pola. Przypisywanie użytkownika lub grupy powoduje następujących elementów ma być wykonywana:

* Azure AD pozwala przypisany użytkownik (albo przez bezpośrednie przypisanie lub członkostwa w grupach) do uwierzytelnienia do pola. Jeśli użytkownik nie jest przypisany, Azure AD nie pozwoli na ich tak, aby zalogować się do pola i zwróci błąd na stronie logowania Azure AD.

* Kafelka aplikacji dla pola jest dodawany do użytkownika, [Uruchom aplikację](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

* Po włączeniu automatycznego inicjowania obsługi administracyjnej następnie przypisane użytkowników lub grup zostaną dodane do obsługi administracyjnej kolejki ma zostać automatycznie zastrzeżona.

    * Jeśli tylko obiektów użytkowników zostały skonfigurowane ma zostać zastrzeżona, następnie wszystkich użytkowników bezpośrednio przypisane są umieszczane w kolejce obsługi administracyjnej, a wszyscy użytkownicy, którzy należą do żadnych grup przypisane zostaną umieszczone w kolejce obsługi administracyjnej. 
    
    * Jeśli grupa obiekty zostały skonfigurowane ma zostać zastrzeżona, wszystkich przydzielonych Grupuj obiekty są obsługi administracyjnej do pola, a także wszystkich użytkowników, które należą do tych grup. Członkostwo w grupie i każdemu użytkownikowi są zachowywane podczas zapisywania do pola.
    
Możesz użyć **atrybuty > logowania jednokrotnego** kartę, aby skonfigurować, które atrybuty użytkownika (lub oświadczeń) są dostarczane do pola przy uwierzytelniania opartego na protokole SAML oraz **atrybuty > obsługi** kartę, aby skonfigurować sposób atrybuty użytkowników i grup przepływ od Azure AD do pola podczas inicjowania obsługi administracyjnej operacji. Zobacz poniżej zasoby, aby uzyskać więcej informacji.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dostosowywanie oświadczeniach wydawane w SAML token](active-directory-saml-claims-customization.md)
* [Obsługa administracyjna: Dostosowywanie mapowania atrybutów](active-directory-saas-customizing-attribute-mappings.md)
* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
