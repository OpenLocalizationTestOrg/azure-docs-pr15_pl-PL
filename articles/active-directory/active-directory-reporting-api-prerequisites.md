<properties
    pageTitle="Wymagania wstępne dotyczące uzyskiwania dostępu Azure AD raportowania interfejsu API. | Microsoft Azure"
    description="Dowiedz się więcej o wymagania wstępne dotyczące dostępu Azure AD raportowania interfejsu API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Wymagania wstępne dotyczące dostępu Azure AD raportowania interfejsu API 

[Azure AD raportowania API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) umożliwiają programowy dostęp do danych za pomocą zestaw opartych na pozostałych interfejsów API. Możesz nawiązać połączenie te interfejsy API różnych programowania języków i narzędzi.

Raportowania interfejsu API używa [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) , aby zezwolić na dostęp do interfejsów API w sieci web. 

Aby przygotować dostępu do raportowania interfejsu API, należy:

1. Tworzenie aplikacji w dzierżawie usługi Azure AD 

2. Udzielić odpowiednich uprawnień dostępu do danych Azure AD aplikacji

3. Gromadzenie ustawienia konfiguracji z katalogu

Pytania, problemów lub opinii skontaktuj się z [AAD raportowania pomocy](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Tworzenie aplikacji Azure AD

Aby skonfigurować katalogu, aby uzyskać dostęp do Azure AD raportowania interfejsu API, musisz się zalogować do portalu klasyczny Azure przy użyciu konta administratora Azure subskrypcji, który jest członkiem roli administratora globalnego katalogów w dzierżawie usługi Azure AD.

> [AZURE.IMPORTANT] Aplikacje w obszarze poświadczenia z uprawnieniami "Administrator" w następujący sposób mogą być bardzo zaawansowanym, więc należy upewnić się, że bezpieczeństwo poświadczeń ID i hasło aplikacji.


1. W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na liście **usługi active directory** zaznacz katalogu.

3. W menu u góry kliknij pozycję **aplikacje**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na dolnym pasku kliknij przycisk **Dodaj**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Na **co chcesz zrobić?** okno dialogowe, kliknij pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**. 

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/04.png) 


6. W oknie dialogowym **Przekaż nam informacje o aplikacji** wykonaj następujące czynności: 

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/05.png) 

    . W polu tekstowym **Nazwa** wpisz nazwę (np.: aplikację raportowanie interfejsu API).

    b. Wybierz **aplikację sieci Web i/lub interfejs API sieci web**.

    c. Kliknij przycisk **Dalej**.


7. W oknie dialogowym **Właściwości aplikacji** wykonaj następujące czynności: 

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/06.png) 

    . W polu tekstowym **adres URL logowania jednokrotnego** wpisz `https://localhost`.

    b. W polu tekstowym **Identyfikator URI identyfikator aplikacji** wpisz ```https://localhost/ReportingApiApp```.

    c. Kliknij pozycję **pełne**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Zezwolić aplikacji za pomocą interfejsu API

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na liście **usługi active directory** zaznacz katalogu.

3. W menu u góry kliknij pozycję **aplikacje**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/02.png)


3. Na liście aplikacji zaznacz nowo utworzonej aplikacji.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/07.png)

4. W menu u góry kliknij przycisk **Konfiguruj**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/08.png)


5. W sekcji **uprawnienia do innych aplikacji** dla zasobu **Usługi Azure Active Directory** kliknij na liście rozwijanej **Uprawnienia aplikacji** , a następnie wybierz polecenie **odczytu katalogu danych**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/09.png)


5. Na dolnym pasku kliknij przycisk **Zapisz**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Gromadzenie ustawienia konfiguracji z katalogu

W tej sekcji przedstawiono sposób uzyskiwania następujące ustawienia z katalogu:

- Nazwa domeny
- Identyfikator klienta
- Tajny klienta

Potrzebujesz tych wartości podczas konfigurowania połączenia do raportowania interfejsu API. 


### <a name="get-your-domain-name"></a>Uzyskaj nazwę domeny

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na liście **usługi active directory** zaznacz katalogu.

3. W menu u góry kliknij pozycję **domeny**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/11.png) 

4. W kolumnie **Nazwa domeny** skopiuj nazwy domeny.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Pobieranie aplikacji identyfikator klienta

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na liście **usługi active directory** zaznacz katalogu.

3. W menu u góry kliknij pozycję **aplikacje**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na liście aplikacji zaznacz nowo utworzonej aplikacji.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/07.png)

4. W menu u góry kliknij przycisk **Konfiguruj**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/08.png)

4. Kopiowanie swój **identyfikator klienta**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Pobieranie aplikacji klienckich hasła

Aby uzyskać klucz tajny klienta aplikacji, należy utworzyć nowy klucz i zapisywanie jej wartość na zapisywanie nowego klucza, ponieważ nie można pobrać później już tej wartości.

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), w okienku nawigacji po lewej stronie kliknij pozycję **Usługi Active Directory**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na liście **usługi active directory** zaznacz katalogu.

3. W menu u góry kliknij pozycję **aplikacje**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na liście aplikacji zaznacz nowo utworzonej aplikacji.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/07.png)

4. W menu u góry kliknij przycisk **Konfiguruj**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/08.png)

5. W sekcji **klawiszy** należy wykonać następujące czynności: 

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/14.png)

    . Na liście czas trwania wybierz czas trwania

    b. Na dolnym pasku kliknij przycisk **Zapisz**.

    ![Zarejestruj się w aplikacji](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Skopiuj wartość klucza.

## <a name="next-steps"></a>Następne kroki

- Czy chcesz uzyskać Azure AD raportowania interfejsu API w sposób programowy dostęp do danych? Zapoznaj się z [Azure Active Directory raportowania API — wprowadzenie](active-directory-reporting-api-getting-started.md).

- Jeśli chcesz dowiedzieć się więcej o raportowaniu usługi Azure Active Directory, zobacz [Azure Active Directory raportowania przewodnika](active-directory-reporting-guide.md).  
