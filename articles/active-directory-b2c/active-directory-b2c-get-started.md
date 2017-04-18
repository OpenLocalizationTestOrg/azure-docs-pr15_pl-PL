<properties
    pageTitle="Azure Active Directory B2C: Tworzenie dzierżawy usługi Azure Active Directory B2C | Microsoft Azure"
    description="Temat na temat tworzenia dzierżawy usługi Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Tworzenie dzierżawy usługi Azure AD B2C

Aby rozpocząć korzystanie z usługi Microsoft Azure Active Directory (Azure AD) B2C, wykonaj trzy kroki opisane w tym artykule.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Krok 1: Konta w usłudze Azure subskrypcji

Jeśli masz już subskrypcji usługi Azure, Pomiń ten krok. W przeciwnym razie konta w usłudze [Azure subskrypcji](../active-directory/sign-up-organization.md) i uzyskać dostęp do Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Krok 2: Tworzenie dzierżawy usługi Azure AD B2C

Wykonaj następujące czynności, aby utworzyć nowej dzierżawy Azure AD B2C. Obecnie funkcje B2C nie można włączyć w do istniejących dzierżaw.

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) jako Administrator subskrypcji. Jest to taką samą pracę lub konta służbowego lub tego samego konta Microsoft, którego użyto do konta w usłudze Azure.
2. Kliknij przycisk **Nowy** > **aplikacji usług** > **usługi Active Directory** > **katalogu** > **utworzyć niestandardowe**.

    ![Zrzut ekranu: rozpoczynanie tworzenie dzierżawy](./media/active-directory-b2c-get-started/new-directory.png)

3. Wybierz **nazwę**, **Nazwę domeny** i **kraju lub regionie** dla Twojej dzierżawy.
4. Zaznacz opcję informacją, że **jest to katalog B2C**.
5. Kliknij pole wyboru do wykonania akcji.

    ![Zrzut ekranu przedstawiający znacznik wyboru, aby utworzyć katalog B2C](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Dzierżawy usługi teraz zostanie utworzona i pojawi się w wewnętrzny usługi Active Directory. Są również administratorem globalnym dzierżawy. Możesz dodać innych administratorów globalnego, zależnie od potrzeb.

    > [AZURE.IMPORTANT]
    Jeśli planujesz korzystanie z dzierżawą B2C dla aplikacji produkcji, przeczytaj artykuł skali [produkcji a Podgląd B2C dzierżaw](active-directory-b2c-reference-tenant-type.md). Należy zauważyć, że są znane problemy, usunięcie istniejącej dzierżawy B2C i utwórz je ponownie z tą samą nazwą domeny. Musisz utworzyć dzierżawy B2C z inną nazwę domeny.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Krok 3: Przejdź do B2C karta funkcje na Azure portal

1. Przejdź do rozszerzenie usługi Active Directory na pasku nawigacyjnym po lewej stronie.
2. Znajdowanie dzierżawy usługi na karcie **katalogu** , a następnie kliknij go.
3. Kliknij kartę **Konfiguruj** .
4. Kliknij łącze **Zarządzaj B2C ustawienia** w sekcji **Administracja B2C** .

    ![Zrzut ekranu przedstawiający katalogu konfiguracji B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Portal Azure z karta funkcje B2C przedstawiający zostanie otwarty w nowej karcie w przeglądarce lub w oknie.

    > [AZURE.IMPORTANT]
    Może potrwać do 2 – 3 minuty dla dzierżawy była dostępna w portalu Azure. Ponawianie te kroki po trochę czasu będą rozwiązać ten problem. Jeśli nie, skontaktuj się z obsługą.

6. Przypinanie ta karta do swojego Startboard w celu ułatwienia dostępu. (Narzędzie numer Pin jest oznaczony w kolorze czerwonym w prawym górnym rogu karta funkcji).

    ![Zrzut ekranu: karta funkcje B2C](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Możesz zarządzać użytkownicy i grupy, konfiguracji resetowania hasła samodzielne i funkcje znakowania firmy dzierżawy usługi w [portalu klasyczny Azure](https://manage.windowsazure.com/).

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Łatwy dostęp do karta funkcje B2C portalu Azure

Aby usprawnić wyszukiwanie, dodaliśmy skrót do karta funkcje B2C portalu Azure.

1. Zaloguj się do portalu Azure jako Administrator globalny dzierżawy usługi B2C. Jeśli jesteś użytkownikiem zalogowanym do innej dzierżawy, przełączanie dzierżaw (w prawym górnym rogu).
2. W obszarze nawigacji po lewej kliknij przycisk **Przeglądaj** .
3. Kliknij pozycję **Azure AD B2C** , aby uzyskać dostęp do karta funkcje B2C.

    ![Zrzut ekranu: Przejdź do B2C karta funkcje](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak rejestrowania aplikacji Azure AD B2C, a także do tworzenia aplikacji Szybki Start, czytając [Azure Active Directory B2C: zarejestrować aplikację](active-directory-b2c-app-registration.md).
