<properties
    pageTitle="Uzyskiwanie pracę dostawcy uwierzytelnianie wieloskładnikowe Azure | Microsoft Azure"
    description="Dowiedz się, jak utworzyć dostawcę uwierzytelnianie wieloskładnikowe Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Wprowadzenie do dostawcy uwierzytelnianie wieloskładnikowe Azure
Weryfikacja dwuetapowa jest domyślnie dostępne dla administratorów globalnych usługi Azure Active Directory i użytkowników usługi Office 365. Jednak jeśli chcesz korzystać z [zaawansowanych funkcji](multi-factor-authentication-whats-next.md) następnie możesz należy zakupić pełną wersję programu Azure uwierzytelnianie wieloskładnikowe (MFA).

> [AZURE.NOTE]  Dostawca uwierzytelniania wieloskładnikowego Azure służy do wykorzystać funkcje oferowane przez pełną wersję programu Azure MFA. Jest dla użytkowników, którzy **nie ma licencji przez Azure MFA, Azure AD Premium lub EMS**.  Azure MFA, Azure AD Premium i EMS zawierać pełną wersję programu Azure MFA domyślnie.  Jeśli masz licencji nie muszą z dostawcą uwierzytelnianie wieloskładnikowe Azure.

Dostawca uwierzytelniania wieloskładnikowego Azure jest wymagane do pobrania zestawu SDK.

> [AZURE.IMPORTANT]  Aby pobrać zestawu SDK, należy utworzyć z dostawcą uwierzytelnianie wieloskładnikowe Azure, nawet jeśli masz Azure MFA, AAD Premium lub EMS licencji.  Jeśli w tym celu utwórz dostawcą uwierzytelnianie wieloskładnikowe Azure i już licencji, pamiętaj utworzyć dostawcę z modelem **Na użytkownika z włączoną** . Następnie połączyć dostawcę katalog zawierający Azure MFA, Azure AD Premium lub EMS licencji.  Dzięki temu, że tylko konta Jeśli masz więcej unikatowych użytkowników przy użyciu zestawu SDK niż liczba licencji użytkownika.


## <a name="to-create-a-multi-factor-auth-provider"></a>Aby utworzyć dostawcę uwierzytelniania wieloskładnikowego

Wykonaj następujące czynności, aby utworzyć dostawcę uwierzytelnianie wieloskładnikowe Azure.

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com) jako administrator.
2. Po lewej stronie wybierz pozycję **Usługi Active Directory**.
3. Na stronie usługi Active Directory u góry wybierz **Dostawców uwierzytelniania wieloskładnikowego**.
![Tworzenie dostawcy MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. U dołu kliknij przycisk **Nowy**.
![Tworzenie dostawcy MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. W obszarze usługi aplikacji wybierz **Dostawcę uwierzytelnianie wieloskładnikowe**
![tworzenia z dostawcą MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Wybierz pozycję **Szybkie tworzenie**.
![Tworzenie dostawcy MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Wypełnij następujące pola i wybierz pozycję **Utwórz**.
    1. **Nazwa** — Nazwa dostawcy uwierzytelnianie wieloskładnikowe.
    2. **Model zastosowania** — czy chcesz umożliwić użytkownikom lub zapłacić na weryfikacji. Wybierz jedną z dwóch opcji:
        - Na uwierzytelnianie — zakupów modelu, który pobiera na uwierzytelniania. Zazwyczaj używany dla scenariuszy, które korzystają uwierzytelnianie wieloskładnikowe Azure w aplikacji dla klientów indywidualnych skierowaną.
        - Dla każdego użytkownika włączone — zakupów modelu, który pobiera na włączone użytkownika. Zazwyczaj używany dostępu pracownika do aplikacji, takich jak usługi Office 365. Wybierz tę opcję, jeśli masz niektórych użytkowników, które są już licencjonowana dla Azure MFA.
    2. **Katalog** — dzierżawy usługi Azure Active Directory, którą jest skojarzony dostawcy uwierzytelnianie wieloskładnikowe. Należy pamiętać o następujących czynności:
        - Katalog Azure AD, aby utworzyć dostawcę uwierzytelnianie wieloskładnikowe nie ma potrzeby. Pozostaw pole puste, jeśli planujesz tylko za pomocą serwera uwierzytelniania wieloskładnikowego Azure lub SDK.
        - Dostawca uwierzytelniania wieloskładnikowego muszą być skojarzone z katalogu Azure AD, aby można było korzystać z zaawansowanych funkcji.
        - Azure AD Connect, AAD Sync lub DirSync są wymagane tylko, jeśli synchronizujesz środowiska usługi Active Directory w lokalnej z katalogu Azure AD.  Jeśli używasz wyłącznie katalogu Azure AD, który nie jest zsynchronizowany, a następnie nie jest to wymagane.
![Tworzenie dostawcy MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Po kliknięciu przycisku Tworzenie, tworzony jest dostawca uwierzytelnianie wieloskładnikowe i powinien zostać wyświetlony komunikat z informacją: **pomyślnie utworzony dostawca uwierzytelniania wieloskładnikowego**. Kliknij przycisk **Ok**.
![Tworzenie dostawcy MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
