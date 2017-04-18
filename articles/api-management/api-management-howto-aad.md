<properties 
    pageTitle="Jak autoryzować konta Deweloper za pomocą usługi Azure Active Directory w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak autoryzować użytkowników przy użyciu usługi Azure Active Directory w zarządzaniu interfejsu API." 
    services="api-management" 
    documentationCenter="API Management" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Jak autoryzować konta Deweloper za pomocą usługi Azure Active Directory w zarządzaniu interfejsu API platformy Azure


## <a name="overview"></a>Omówienie
Ten przewodnik pokazano, jak włączyć dostęp do portalu Deweloper dla wszystkich użytkowników w jeden lub więcej katalogów Active Azure. Ten przewodnik zawiera również sposobu zarządzania grupami użytkowników usługi Azure Active Directory, dodając zewnętrznych grup, które zawierają użytkowników usługi Azure Active Directory.

>Do wykonania kroków w tym przewodniku musisz najpierw włączyć usługi Azure Active Directory w której chcesz utworzyć aplikację.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Jak zezwolić Deweloper konta za pomocą usługi Azure Active Directory

Aby rozpocząć pracę, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

W menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **Zabezpieczenia** , a następnie kliknij pozycję **Tożsamości zewnętrznych**.

![Zewnętrzne tożsamości][api-management-security-external-identities]

Kliknij pozycję **Azure Active Directory**. Zanotuj **Adres URL przekierowania** i Przełącz swojej usługi Azure Active Directory w portalu klasyczny Azure.

![Zewnętrzne tożsamości][api-management-security-aad-new]

Kliknij przycisk **Dodaj** , aby utworzyć nową aplikację usługi Azure Active Directory, a następnie wybierz pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**.

![Dodawanie nowej aplikacji usługi Azure Active Directory][api-management-new-aad-application-menu]

Wprowadź nazwę aplikacji, wybierz **aplikację sieci Web i/lub interfejs API sieci Web**i kliknij przycisk Dalej.

![Nowa aplikacja usługi Azure Active Directory][api-management-new-aad-application-1]

**Logowania jednokrotnego adres URL**wprowadź adres URL logowania jednokrotnego portalem Deweloper. W tym przykładzie jest adres **URL logowania jednokrotnego** `https://aad03.portal.current.int-azure-api.net/signin`. 

Adres **URL identyfikator aplikacji**wpisz domeny domyślnej lub niestandardowej domeny dla usługi Azure Active Directory i dołączyć ciąg unikatowy. W tym przykładzie domyślnej domeny **https://contoso5api.onmicrosoft.com** jest używana z sufiksem **/api** określone.

![Nowe właściwości aplikacji usługi Azure Active Directory][api-management-new-aad-application-2]

Kliknij przycisk Sprawdź, aby zapisać i tworzenie nowej aplikacji, a następnie przejdź na kartę **Konfiguruj** , aby skonfigurować nową aplikację.

![Nowej aplikacji usługi Azure Active Directory][api-management-new-aad-app-created]

Jeśli wiele katalogów Active Azure trafiają może być używany dla tej aplikacji, kliknij przycisk **Tak** dla **aplikacji jest wiele dzierżawy**. Wartość domyślna to **Brak**.

![Aplikacja jest wiele dzierżawy][api-management-aad-app-multi-tenant]

Skopiuj **Adres URL przekierowania** z sekcji **Usługi Azure Active Directory** karty **Tożsamości zewnętrznych** w portalu programu publisher i wkleić go w polu tekstowym **Adres URL odpowiedź** . 

![Adres URL odpowiedzi][api-management-aad-reply-url]

Przewiń w dół na karcie Konfigurowanie, wybierz listę rozwijaną **Uprawnienia aplikacji** i zaznacz pole wyboru **odczytu katalogu dane**.

![Uprawnienia aplikacji][api-management-aad-app-permissions]

Wybierz listę rozwijaną **Uprawnienia pełnomocnika** i sprawdź **włączyć logowania jednokrotnego i przeczytaj profilów użytkowników**.

![Delegowane uprawnienia][api-management-aad-delegated-permissions]

>Aby uzyskać więcej informacji dotyczących aplikacji i delegowanych uprawnień zobacz [Uzyskiwanie dostępu do interfejsu API wykresu][].

Skopiuj **Identyfikator klienta** do Schowka.

![Identyfikator klienta][api-management-aad-app-client-id]

Powróć do portalu programu publisher i Wklej w polu **Identyfikator klienta** , kopiowane z konfiguracji aplikacji usługi Azure Active Directory.

![Identyfikator klienta][api-management-client-id]

Powróć do konfigurowania usługi Azure Active Directory i kliknij pozycję **Wybierz czas trwania** listy rozwijanej w sekcji **klawiszy** i określanie interwału. W tym przykładzie jest używany **1 rok** .

![Klawisz][api-management-aad-key-before-save]

Kliknij przycisk **Zapisz** , aby zapisać konfigurację i wyświetlić klucz. Kopiowanie klucz do Schowka.

>Zanotuj tego klucza. Po zamknięciu okna Konfiguracja usługi Azure Active Directory klucz nie można wyświetlić ponownie.

![Klawisz][api-management-aad-key-after-save]

Powróć do portalu programu publisher i Wklej klucz w polu tekstowym **Hasło klienta** .

![Tajny klienta][api-management-client-secret]

**Dozwolone dzierżaw** Określa, które katalogi mają dostęp do interfejsów API wystąpienia usługi zarządzania interfejsu API. Określ domen wystąpienia usługi Azure Active Directory, do których chcesz udzielić dostępu. Możesz oddzielić wiele domen newlines, spacje lub przecinki.

![Dozwolone dzierżaw][api-management-client-allowed-tenants]

W sekcji **Dozwolone dzierżaw** można określić wiele domen. Przed dowolny użytkownik może logować się w domenie innej niż oryginalny domena, w której aplikacja została zarejestrowana, administrator globalny innej domeny musi udzielić uprawnień dla aplikacji dostęp do katalogu danych. Aby udzielić uprawnień, administrator globalny musisz zalogować się do aplikacji, a następnie kliknij przycisk **Zaakceptuj**. W poniższym przykładzie `miaoaad.onmicrosoft.com` został dodany do **Dozwolonych dzierżaw** i globalny administratora z tej domeny jest logować się po raz pierwszy.

![Uprawnienia][api-management-permissions-form]

>Jeśli administrator globalny nie próbuje zalogować się przed uprawnienia przez administratora globalnego, próba logowania kończy się niepowodzeniem i jest wyświetlany komunikat o błędzie.

Gdy określono Konfiguracja docelowa, kliknij przycisk **Zapisz**.

![Zapisywanie][api-management-client-allowed-tenants-save]

Gdy te zmiany zostaną zapisane, użytkownicy w określonej usługi Azure Active Directory zalogować się do portalu Deweloper, wykonując czynności opisane w [zalogować się do portalu Deweloper przy użyciu konta usługi Azure Active Directory][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Jak dodać zewnętrznych Azure grupy usługi Active Directory

Po włączeniu dostępu dla użytkowników w usłudze Azure Active Directory, możesz dodać grup usługi Azure Active Directory do zarządzania interfejsu API łatwiej zarządzać skojarzenia deweloperów w grupie z odpowiednim produktów.

> Aby skonfigurować zewnętrzne grupy usługi Azure Active Directory, usługi Azure Active Directory najpierw należy skonfigurować na karcie tożsamości, wykonując procedurę opisaną w poprzedniej sekcji. 

Zewnętrzne grupy usługi Azure Active Directory zostaną dodane na karcie **widoczność** produktu, dla którego chcesz udzielić dostępu do grupy. Kliknij opcję **produkty**, a następnie kliknij nazwę odpowiedniej produktu.

![Konfigurowanie produktu][api-management-configure-product]

Przejdź na kartę **widoczność** , a następnie kliknij pozycję **Dodaj grupy z usługi Azure Active Directory**.

![Dodawanie grup][api-management-add-groups]

Z listy rozwijanej wybierz pozycję **Azure Active Directory dzierżawy** , a następnie wpisz nazwę odpowiedniej grupy w **grupach** do dodania pola tekstowego.

![Wybierz grupę][api-management-select-group]

Ta nazwa grupy znajdują się na liście **grupy** dla usługi Azure Active Directory, jak pokazano w poniższym przykładzie.

![Lista grup usługi Azure Active Directory][api-management-aad-groups-list]

Kliknij przycisk **Dodaj** , aby sprawdzić poprawność nazwę grupy i Dodaj grupę. W tym przykładzie **Deweloperów 5 Contoso** zewnętrznych grupa jest dodawana. 

![Grupa dodane][api-management-aad-group-added]

Kliknij przycisk **Zapisz** , aby zapisać nowe zaznaczenie grupy.

Po skonfigurowaniu grupy usługi Azure usługi Azure Active Directory z jednego produktu jest dostępna do sprawdzenia na karcie **widoczności** dla innych produktów w wystąpieniu usług zarządzania interfejsu API.

Aby przejrzeć i skonfigurować właściwości grupy zewnętrznej po ich dodaniu, kliknij nazwę grupy na karcie **grupy** .

![Zarządzanie grupami][api-management-groups]

W tym miejscu możesz edytować **nazwę** i **Opis** grupy.

![Edytowanie grupy][api-management-edit-group]

Użytkownikom skonfigurowane usługi Azure Active Directory można zalogować się do portalu Deweloper i wyświetlanie i subskrybowanie żadnych grup, do których mają widoczność, postępując zgodnie z instrukcjami w następnej sekcji.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Jak zalogować się do portalu Deweloper przy użyciu konta usługi Azure Active Directory

Aby zalogować się do portalu Deweloper przy użyciu konta usługi Azure Active Directory skonfigurowane w poprzednich sekcjach, otwieranie nowego okna przeglądarki przy użyciu **adresu URL logowania jednokrotnego** od konfiguracji aplikacji usługi Active Directory, a następnie kliknij **Usługi Azure Active Directory**.

![Dzięki portalowi deweloperów][api-management-dev-portal-signin]

Wprowadź poświadczenia jednego z użytkowników w usłudze Active Directory platformy Azure, a następnie kliknij przycisk **Zaloguj**.

![Rejestrowanie][api-management-aad-signin]

Może pojawić się monit o formularz rejestracji Jeśli wymagane jest dodatkowe informacje. Wypełnij formularz rejestracji, a następnie kliknij przycisk **Zarejestruj**.

![Rejestracja][api-management-complete-registration]

Użytkownika jest obecnie zalogowany do portalu Deweloper wystąpienia usługi zarządzania interfejsu API.

![Rejestracja zakończona][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Uzyskiwanie dostępu do wykresu interfejsu API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Zaloguj się do portalu Deweloper przy użyciu konta usługi Azure Active Directory]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

