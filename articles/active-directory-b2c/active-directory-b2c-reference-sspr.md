<properties
    pageTitle="Azure Active Directory B2C: Samodzielne Resetowanie hasła | Microsoft Azure"
    description="Jak skonfigurować samodzielnego resetowania hasła dla osób korzystających ze Azure Active Directory B2C pokazujące tematu"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Konfigurowanie samodzielnego resetowania hasła dla osób korzystających ze

Za pomocą funkcji resetowania hasła Samoobsługowe usługi konsumentów, (którzy zalogowali dla kont lokalnych) mogą resetować haseł samodzielnie. Zmniejsza to znacznie obciążenia działu pomocy technicznej, zwłaszcza jeśli aplikacja ma miliony konsumentów jej używać na bieżąco. Obecnie obsługiwany jest tylko przy użyciu adresu e-mail zweryfikowanym jako metoda odzyskiwania. Metody odzyskiwania dodatkowe (numer telefonu zweryfikowane, pytania dotyczące zabezpieczeń itp.) będą dodawane w przyszłości.

> [AZURE.NOTE]
Ten artykuł dotyczy hasło Samodzielne resetowanie używane w kontekście zasady logowania. W razie potrzeby w pełni dostosować zasady wywołane z aplikacji do resetowania hasła, zobacz [Ten artykuł](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Domyślnie katalogu nie będą mieć hasło Samodzielne resetowanie włączone. Aby włączyć tę funkcję, wykonaj następujące czynności:

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) jako Administrator subskrypcji. Jest to taką samą pracę lub konta służbowego lub tego samego konta Microsoft, który został użyty do utworzenia katalogu.
2. Przejdź do rozszerzenie usługi Active Directory na pasku nawigacyjnym po lewej stronie.
3. Znajdowanie katalogu na karcie **katalogu** , a następnie kliknij go.
4. Kliknij kartę **Konfiguruj** .
5. Przewiń w dół do sekcji **zasady resetowania hasła użytkownika** i Przełącz **Resetowanie haseł użytkowników** opcja **Tak**. Zwróć uwagę, że jest zaznaczona opcja **Alternatywny adres E-mail** ; pozostaw ją.

    ![Resetowanie hasła Sklep internetowy](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Kliknij pozycję **Zapisz** u dołu strony. Gotowe!

Aby przetestować, należy użyć funkcji "Uruchom teraz" w dowolnym logowania zasady konta lokalne jako dostawcy tożsamości. Podczas logowania lokalnego konta się strony (miejsce, w którym możesz wprowadź adres e-mail i hasło, lub nazwę użytkownika i hasło), kliknij przycisk **nie może uzyskać dostępu do konta?** do weryfikacji obsługi dla klientów indywidualnych.

> [AZURE.NOTE]
Strony resetowania hasła samoobsługowego można dostosować za pomocą [funkcji znakowania firmy](../active-directory/active-directory-add-company-branding.md).
