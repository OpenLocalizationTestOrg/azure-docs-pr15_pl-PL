<properties
    pageTitle="Azure Active Directory B2C: Rejestrowanie aplikacji | Microsoft Azure"
    description="Jak zarejestrować aplikacji usługi Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Zarejestrować aplikację

## <a name="prerequisite"></a>Wymagania wstępne

Aby utworzyć aplikację, która akceptuje konsumenta zapisów i logowania, należy najpierw zarejestrować tę aplikację z dzierżawą usługi Azure Active Directory B2C. Uzyskaj własne dzierżawy, wykonując czynności opisane w sekcji [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md). Po wykonaniu wszystkich kroków w tym artykule, konieczne będzie karta funkcje B2C przypięta do swojego Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Przejdź do B2C karta funkcje

Jeśli masz karta funkcje B2C przypięta do Twojej Startboard, zobaczysz karta zaraz po zalogowaniu się do [portalu Azure](https://portal.azure.com/) jako Administrator globalny dzierżawy B2C.

Karta może też uzyskiwać dostęp przez kliknięcie przycisku **Przeglądaj** , a następnie **Azure AD B2C** w okienku nawigacji po lewej stronie [Azure portal](https://portal.azure.com/).

> [AZURE.IMPORTANT] Musisz być administratorem globalnym dzierżawy B2C, aby można było uzyskać dostęp do karta funkcje B2C. Administrator globalny z innych dzierżawy lub użytkownika z dowolnej dzierżawy nie może uzyskać do niego dostęp.  Możesz przełączać się do Twojej dzierżawy B2C za pomocą przełącznika dzierżawy w prawym górnym rogu Azure Portal.

## <a name="register-an-application"></a>Zarejestruj się w aplikacji

1. Na karta funkcje B2C portalu Azure kliknij pozycję **aplikacje**.
2. Kliknij przycisk **+ Dodaj** w górnej części karta.
3. Wprowadź **nazwę** aplikacji, opisując aplikacji dla konsumentów. Na przykład można wprowadzić "Contoso B2C aplikację".
4. Jeśli piszesz aplikacji sieci web, Przełącz przełącznik **obejmują aplikacji sieci web / sieci web interfejsu API** **Tak**. **Adresy URL odpowiedzi** są punkty końcowe, gdzie Azure AD B2C zwróci wszelkie tokeny żądania aplikacji. Na przykład wprowadź `https://localhost:44321/`. Jeśli aplikacji sieci web będzie także dzwoniąc niektórych interfejsu API zabezpieczone Azure AD B2C w sieci web, można także utworzyć **Hasło aplikacji** , klikając przycisk **Generowanie klucza** .

    > [AZURE.NOTE] **Tajny aplikacji** jest ważne poświadczenie i powinny być odpowiednio zabezpieczone.

5. Jeśli piszesz aplikacji mobilnej Przełącz przełącznik **klientami Dołącz** na wartość **Tak**. Skopiuj domyślne **Przekierowywać identyfikatora URI** , który jest automatycznie tworzona w dół.
6. Kliknij przycisk **Utwórz** , aby zarejestrować aplikację.
7. Kliknij aplikację, która została właśnie utworzona, a następnie skopiuj dół globalnie unikatowy **Identyfikator klienta aplikacji** , które będą używane w dalszej części kodu.

> [AZURE.IMPORTANT] Aplikacje utworzone w karta funkcje B2C trzeba zarządzanych w tej samej lokalizacji. Jeśli edytujesz B2C aplikacji przy użyciu programu PowerShell lub innego portalu one stają się nieobsługiwane i prawdopodobnie nie działa w systemie Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Tworzenie aplikacji Szybki Start

Teraz, gdy masz aplikację zarejestrowane Azure AD B2C, można wykonać jedną z naszych samouczki szybki start, aby rozpocząć pracę. Poniżej przedstawiono kilka zaleceń:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
