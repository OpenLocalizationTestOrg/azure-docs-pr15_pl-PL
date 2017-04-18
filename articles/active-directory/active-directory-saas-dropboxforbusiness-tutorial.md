<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z usługi Dropbox dla firm | Microsoft Azure" 
    description="Dowiedz się, jak włączyć logowania jednokrotnego, automatycznego inicjowania obsługi administracyjnej i innych elementów za pomocą skrzynki dla firm z usługą Azure Active Directory!" 
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

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Samouczek: Azure Active Directory Integracja z usługi Dropbox dla firm
  
Celem tego samouczka jest pokazanie integracji Azure i Dropbox dla firm.  
Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Test dzierżawy w usłudze Dropbox dla firm
  
Ten samouczek użytkowników Azure AD zostały przypisane do usługi Dropbox dla firm będą mogli rejestracji jednokrotnej do aplikację z usługi Dropbox dla firm firmy witryny (znak inicjowanych przez dostawcę usługi na) lub za pomocą [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).
  
Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla usługi Dropbox dla firm
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Scenariusz] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scenariusz")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Włączanie integracji aplikacji dla usługi Dropbox dla firm
  
Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla usługi Dropbox dla firm.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla usługi Dropbox dla firm, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z gallerry] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Dodawanie aplikacji z gallerry")

6.  W **polu wyszukiwania**wpisz **Dropbox dla firm**.

    ![Galeria aplikacji] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Galeria aplikacji")

7.  W okienku wyników wybierz pozycję **Dropbox dla firm**, a następnie kliknij przycisk **Zakończ** , aby dodać aplikację.

    ![Dropbox dla firm] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox dla firm")

##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego
  
Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Dropbox dla firm przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.

W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do usługi Dropbox dla firm dzierżawy. Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie integracji aplikacji **Dropbox dla firm** kliknij pozycję **Konfiguruj rejestracji jednokrotnej** aby otworzyć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** .

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Konfigurowanie logowania jednokrotnego")

2.  Na stronie **jak chcesz użytkowników do zalogowania się usłudze Dropbox dla firm** wybierz pozycję **Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Konfigurowanie logowania jednokrotnego")

3.  Na stronie **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:

    . Logowania jednokrotnego do usługi Dropbox dla firm dzierżawy. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Konfigurowanie logowania jednokrotnego")

    b. W okienku nawigacji po lewej stronie kliknij pozycję **Konsoli administracyjnej**. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Konfigurowanie logowania jednokrotnego")

    c. **Konsola administracyjna**w okienku nawigacji po lewej stronie kliknij pozycję **Uwierzytelnianie** . 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Konfigurowanie logowania jednokrotnego")

    d. W sekcji **rejestracji jednokrotnej** zaznacz pole wyboru **Włącz rejestracji jednokrotnej**, a następnie kliknij **więcej** aby rozwinąć tę sekcję.  

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Konfigurowanie logowania jednokrotnego")

    e. Skopiuj adres URL obok **użytkownicy mogli logować się, podając adres e-mail lub ich można przejść bezpośrednio do**. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Konfigurowanie logowania jednokrotnego")

    f. W portalu klasyczny Azure w polu tekstowym **DropBox dla firm Zaloguj** adres URL Wklej adres URL. 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Konfigurowanie logowania jednokrotnego")  



4. Na stronie **Konfigurowanie logowania jednokrotnego w usłudze Dropbox dla firm** kliknij przycisk **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.  

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Konfigurowanie logowania jednokrotnego")


5. Na usługi Dropbox dla dzierżawy firm, w sekcji **rejestracji jednokrotnej** strony **uwierzytelniania** wykonaj następujące czynności: 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfigurowanie logowania jednokrotnego")

    . Kliknij opcję **wymagane**.

    b. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w usłudze Dropbox dla firm** skopiuj wartość **adres URL strony — logowania** , a następnie wklej go w polu tekstowym **Zaloguj się w adresie URL** .


    c. Tworzenie pliku **zakodowany Base-64** z pobranego certyfikatu. 

    > [AZURE.TIP] Aby uzyskać więcej informacji zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).


    d. Kliknij przycisk **"Wybierz certyfikat"** , a następnie przejdź do swojej **base-64 zakodowany plik certyfikatu**.


    e. Kliknij przycisk **"Zapisz zmiany"** , aby ukończyć konfigurację na usługi DropBox dla firm dzierżawy.


6. W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Konfigurowanie rejestracji jednokrotnej** . 

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Konfigurowanie logowania jednokrotnego")



##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
  
Celem w tej sekcji jest konspektu, jak włączyć użytkownika inicjowania obsługi administracyjnej usługi Active Directory kont użytkowników do usługi Dropbox dla firm.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1. W portalu klasyczny Azure, na stronie integracji aplikacji **Dropbox dla firm** kliknij pozycję **Konfiguruj przypisywanie użytkowników** , aby otworzyć okno dialogowe **Konfigurowanie przypisywanie użytkowników** .

2. Włącz przypisywanie użytkowników do usługi DropBox dla firm, wybierz polecenie Włącz przypisywanie użytkowników, aby otworzyć Zaloguj się do skrzynki, aby połączyć przy użyciu okna dialogowego Azure AD.  

    ![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Przypisywanie użytkowników")

3. W oknie dialogowym **Zaloguj się do usługi Dropbox, aby połączyć się z usługą Azure Active Directory** Zaloguj się do usługi Dropbox dla firm dzierżawy. 

    ![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Przypisywanie użytkowników")



4. Kliknij przycisk **Zezwalaj** , aby udzielić Azure AD dostępu do skrzynki. 

    ![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Przypisywanie użytkowników")



5. Aby zakończyć konfigurację, kliknij przycisk **Zakończ** .  

    ![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Przypisywanie użytkowników")




##<a name="assigning-users"></a>Przypisywanie użytkowników
  
Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Aby przypisać użytkowników do usługi Dropbox dla firm, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji aplikacji **Dropbox dla firm **kliknij przycisk **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Tak")
  


Należy teraz Odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu Dropbox dla firm.

Pierwszym krokiem weryfikacji możesz sprawdzić stan obsługi administracyjnej, klikając pozycję **pulpit nawigacyjny** na stronie integracji aplikacji **Dropbox dla firm** w portalu klasyczny Azure.

![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Przypisywanie użytkowników")


Ukończony użytkownika inicjowania obsługi administracyjnej cyklu jest oznaczany powiązanych stanu.

![Przypisywanie użytkowników] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Przypisywanie użytkowników")


Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu.
Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczki dotyczące integracji władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)