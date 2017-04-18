<properties 
    pageTitle="Samouczek: Azure Active Directory Integracja z Citrix GoToMeeting | Microsoft Azure" 
    description="Dowiedz się, jak użyć Citrix GoToMeeting z usługą Azure Active Directory w celu włączenia rejestracji jednokrotnej, automatycznego inicjowania obsługi administracyjnej i nie tylko!." 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Samouczek: Azure Active Directory Integracja z Citrix GoToMeeting  
Dotyczy: Azure

>[AZURE.TIP]W przypadku opinię, kliknij [tutaj](http://go.microsoft.com/fwlink/?LinkId=522412).

Celem tego samouczka jest pokazanie integracji Azure i Citrix GoToMeeting. Scenariusz przedstawionych w tym samouczku założono, że już następujące elementy:

-   Ważna subskrypcja Azure
-   Dzierżawy w sekcji Citrix GoToMeeting

Scenariusz przedstawionych w tym samouczku składa się z następujących bloków konstrukcyjnych:

1.  Włączanie integracji aplikacji dla Citrix GoToMeeting
2.  Konfigurowanie logowania jednokrotnego
3.  Konfigurowanie użytkowników inicjowania obsługi administracyjnej.
4.  Przypisywanie użytkowników

![Konfiguracja] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfiguracja")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Włączanie integracji aplikacji dla Citrix GoToMeeting

Celem w tej sekcji jest konspektu, jak włączyć integrację aplikacji dla Citrix GoToMeeting.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Aby włączyć integrację aplikacji dla Citrix GoToMeeting, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Usługi Active Directory")

2.  Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3.  Aby otworzyć widok aplikacji w widoku katalogu, w górnym menu kliknij pozycję **aplikacje** .

    ![Aplikacje] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Aplikacje")

4.  Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Dodawanie aplikacji] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Dodawanie aplikacji")

5.  W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację z galerii**.

    ![Dodawanie aplikacji z galerii] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Dodawanie aplikacji z galerii")

6.  W **polu wyszukiwania**wpisz **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  W okienku wyników wybierz **Citrix GoToMeeting**, a następnie kliknij **Wykonano** w celu dodania aplikacji.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Konfigurowanie logowania jednokrotnego

Celem w tej sekcji jest konspektu, jak umożliwić użytkownikom uwierzytelniania Citrix GoToMeeting przy użyciu swojego konta w Azure AD przy użyciu Federacji na podstawie protokołu SAML.  
W ramach tej procedury jest wymagane przekazywanie certyfikatu zakodowany base 64 do Twojej dzierżawy Citrix GoToMeeting.  
Jeśli nie znasz tej procedury, zobacz [jak przekonwertować certyfikat binarny do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować logowania jednokrotnego, wykonaj następujące czynności:

1.  Na stronie **Citrix GoToMeeting** aplikacji integracji kliknij **Konfigurowanie logowania jednokrotnego** aby otworzyć okno dialogowe **Konfigurowanie na pojedynczy znak** .

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Włączanie rejestracji jednokrotnej")

2.  Na stronie **jak chcesz użytkownikom logowanie do Citrix GoToMeeting** wybierz **Microsoft Azure AD rejestracji jednokrotnej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Konfigurowanie logowania jednokrotnego")


3. Na stronie **Konfigurowanie ustawień aplikacji** kliknij przycisk **Dalej**. 

    ![Włączanie rejestracji jednokrotnej] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Włączanie rejestracji jednokrotnej")

4.  Na stronie **Konfigurowanie logowania jednokrotnego w Citrix GoToMeeting** kliknij pozycję **Pobierz certyfikat**, a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Konfigurowanie logowania jednokrotnego")

5.  W oknie przeglądarki różnych Zaloguj się do Centrum organizacji Citrix ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Kliknij kartę **Dostawcy tożsamości** , a następnie wykonaj następujące czynności:  

    ![Ustawienia SAML] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "Ustawienia SAML")

    . Wybierz pozycję **ręcznie**

    
    b. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Citrix GoToMeeting** skopiuj wartość **Adres URL strony — logowania** , a następnie wklej go w polu tekstowym **adres URL strony — logowania** . 

    
    c. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Citrix GoToMeeting** skopiuj wartość **Adres URL strony Sign-Out** , a następnie wklej go w polu tekstowym **adres URL strony wyrejestrowywania** .

    
    d. W portalu klasyczny Azure, na stronie okno dialogowe **Konfigurowanie logowania jednokrotnego w Citrix GoToMeeting** skopiuj wartość **Identyfikatora obiektu** , a następnie wklej go w polu tekstowym **Identyfikator jednostki dostawcy tożsamości** .

   
    e. Aby przekazać pobranego certyfikatu, kliknij pozycję **Przekaż certyfikat**.

    
    f. Kliknij przycisk **Zapisz**.

6.  W portalu klasyczny Azure wybierz potwierdzenia konfiguracji rejestracji jednokrotnej firmy, a następnie kliknij **Dalej**.

    ![Konfigurowanie logowania jednokrotnego] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Konfigurowanie logowania jednokrotnego")


7. Na stronie **potwierdzenia rejestracji jednokrotnej** kliknij przycisk **Zakończ**.

    ![Ustawienia SAML] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "Ustawienia SAML")





##<a name="configuring-user-provisioning"></a>Konfigurowanie użytkowników inicjowania obsługi administracyjnej.

Celem w tej sekcji jest konspektu, jak włączyć inicjowania obsługi administracyjnej konta użytkowników usługi Active Directory w celu Citrix GoToMeeting.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, przypisywanie użytkowników, wykonaj następujące czynności:

1.  W portalu klasyczny Azure, na stronie **Citrix GoToMeeting** aplikacji integracji kliknij pozycję **Konfiguruj przypisywanie użytkowników** , aby otworzyć okno dialogowe **Konfigurowanie przypisywanie użytkowników** .

    ![Konfigurowanie użytkowników inicjowania obsługi administracyjnej.] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Konfigurowanie użytkowników inicjowania obsługi administracyjnej.")

2.  Na stronie **Ustawienia i poświadczenia administratora** wykonaj następujące czynności:

    ![Konfigurowanie użytkowników inicjowania obsługi administracyjnej.] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Konfigurowanie użytkowników inicjowania obsługi administracyjnej.")

    . W polu tekstowym **Nazwa użytkownika administratora GoToMeeting Citrix** wpisz nazwę użytkownika administratora.

    
    b. W polu tekstowym **Hasło administratora GoToMeeting Citrix** hasło administratora.

    
    c. Kliknij przycisk **Dalej**.

3.  Na stronie **potwierdzenia** kliknij znacznik wyboru, aby zapisać konfigurację.

4.  Kliknij przycisk **Sprawdzanie poprawności** , aby Sprawdź konfigurację.


##<a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz zezwolić użytkownikom Azure AD, że chcesz zezwolić przez przypisywanie ich za pomocą aplikacji dostęp do niego.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Aby przypisać użytkowników do Citrix GoToMeeting, wykonaj następujące czynności:

1.  W portalu klasyczny Azure Utwórz konto testowe.

2.  Na stronie integracji **Citrix GoToMeeting** aplikacji kliknij polecenie **Przypisz użytkowników**.

    ![Przypisywanie użytkowników] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Przypisywanie użytkowników")

3.  Wybierz użytkownika test, kliknij przycisk **Przypisz**, a następnie kliknij **Tak,** aby potwierdzić przypisania.

    ![Tak] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Tak")

Należy teraz Odczekaj 10 minut i sprawdź, czy konto zsynchronizowaniu Dropbox dla firm.

Pierwszym krokiem weryfikacji możesz sprawdzić stan obsługi administracyjnej, klikając pozycję pulpitu nawigacyjnego w D **Citrix GoToMeeting** aplikacji integracji na stronie portalu klasyczny Azure.

![Pulpit nawigacyjny] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Pulpit nawigacyjny")

Ukończony użytkownika inicjowania obsługi administracyjnej cyklu jest wskazywana przez powiązanych stanu:

![Stan integracji] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Stan integracji")

Jeśli chcesz sprawdzić ustawienia pojedynczego logowania jednokrotnego, otwórz Panel dostępu.

Aby uzyskać więcej informacji na temat Panel dostępu Zobacz [Wprowadzenie do Panel dostępu](https://msdn.microsoft.com/library/dn308586).
