<properties 
    pageTitle="Zabezpieczania usług wewnętrznej przy użyciu klienta certyfikatu uwierzytelniania w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak zabezpieczanie usług wewnętrznej przy użyciu funkcji uwierzytelniania certyfikat klienta w zarządzaniu interfejsu API Azure." 
    services="api-management" 
    documentationCenter="" 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Zabezpieczania usług wewnętrznej przy użyciu klienta certyfikatu uwierzytelniania w zarządzaniu interfejsu API platformy Azure

Interfejs API zarządzania umożliwia bezpieczny dostęp do usług wewnętrznej interfejsu API przy użyciu certyfikatów klienta. Ten przewodnik pokazano, jak zarządzać certyfikatów w portalu interfejsu API programu publisher i sposobu konfigurowania interfejsu API można uzyskać dostęp do jego usług wewnętrznej za pomocą certyfikatu.

Informacji o zarządzaniu certyfikatów za pomocą interfejsu API usługi REST zarządzania interfejsu API zobacz [jednostki Azure interfejsu API zarządzania pozostałych interfejsu API certyfikatu][].

## <a name="prerequisites"> </a>Wymagania wstępne

Ten przewodnik zawiera sposobu konfigurowania usługi zarządzania interfejsu API wystąpienia umożliwia dostęp do usługi wewnętrznej interfejs API uwierzytelnianie klienta na podstawie certyfikatu. Przed wykonując czynności opisane w tym temacie, należy mieć usługi wewnętrznej skonfigurowany do uwierzytelniania certyfikatu klienta (,[Aby skonfigurować certyfikatu uwierzytelniania w witrynach Azure odwołują się do tego artykułu][]) i mieć dostęp do certyfikatu i hasło dla certyfikatu dla przekazywania w portalu zarządzania interfejsu API programu publisher.

## <a name="step1"> </a>Przekazać certyfikat klienta

Aby rozpocząć pracę, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal interfejsu API programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

W menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **Zabezpieczenia** , a następnie kliknij przycisk **Certyfikaty klienta**.

![Certyfikaty klienta][api-management-security-client-certificates]

Aby przekazać nowego certyfikatu, kliknij przycisk **Przekaż certyfikatu**.

![Przekazywanie certyfikatu][api-management-upload-certificate]

Przejdź do certyfikatu, a następnie wprowadź hasło dla tego certyfikatu.

>Certyfikat musi być w formacie **pfx** . Certyfikaty z podpisem własnym jest dozwolone.

![Przekazywanie certyfikatu][api-management-upload-certificate-form]

Kliknij przycisk **Przekaż** , aby przekazać certyfikatu.

>Hasło certyfikatu sprawdzania poprawności w tej chwili. Jeśli jest nieprawidłowa jest wyświetlany komunikat o błędzie.

![Certyfikat przekazane][api-management-certificate-uploaded]

Po przesłaniu certyfikat pojawi się na karcie **Certyfikaty klienta** . Jeśli masz wiele certyfikatów, upewnij się, notatki tematu lub ostatnie cztery znaki odcisku palca, które są używane do wybierz certyfikat, podczas konfigurowania interfejsu API do korzystania z certyfikatów, jak to opisano w sekcji [Konfigurowanie interfejsu API do certyfikat klienta w celu uwierzytelnienia bramy za pomocą][] następujących.

>Aby wyłączyć sprawdzanie poprawności łańcucha certyfikatu używając, na przykład certyfikat z podpisem własnym, wykonaj kroki opisane w tym często zadawane pytania dotyczące [elementu](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Usunąć certyfikat klienta

Aby usunąć certyfikat, kliknij przycisk **Usuń** obok odpowiedniej certyfikatu.

![Usuwanie certyfikatu][api-management-certificate-delete]

Kliknij przycisk **Tak, usuń ją** , aby potwierdzić.

![Potwierdzanie usunięcia][api-management-confirm-delete]

Jeśli certyfikat jest używany przez interfejs API, zostanie wyświetlona na ekranie ostrzeżenie. Aby usunąć certyfikat musi najpierw usunąć certyfikat z dowolnego interfejsów API, które są skonfigurowane do używania go.

![Potwierdzanie usunięcia][api-management-confirm-delete-policy]

## <a name="step2"> </a>Konfigurowanie interfejsu API do uwierzytelniania bramy za pomocą certyfikatu klienta

Kliknij pozycję **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, kliknij nazwę odpowiedniej interfejsu API i kliknij kartę **Zabezpieczenia** .

![Interfejs API zabezpieczeń][api-management-api-security]

**Certyfikaty klienta** wybierz z listy rozwijanej **przy użyciu poświadczeń** .

![Certyfikaty klienta][api-management-mutual-certificates]

Wybierz żądany certyfikat z listy rozwijanej **certyfikat klienta** . Jeśli istnieje wiele certyfikatów może przeglądać temat lub ostatnie cztery znaki odcisku palca, które zostały wymienione w poprzedniej sekcji, aby określić poprawny certyfikat.

![Wybierz certyfikat][api-management-select-certificate]

Kliknij przycisk **Zapisz** , aby zapisać zmiany konfiguracji API.

>Ta zmiana obowiązuje od razu, a połączenia do operacji tego interfejsu API zostaną użyte certyfikatu do uwierzytelnienia na serwerze wewnętrznym.

![Zapisz zmiany w interfejsie API][api-management-save-api]

>Po określeniu certyfikatu uwierzytelniania bramy w usłudze wewnętrznej interfejsu API staje się częścią zasad dla tego interfejsu API i mogą być wyświetlane w edytorze zasad.

![Zasady certyfikatów][api-management-certificate-policy]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat innych sposobów zabezpieczenia usługi wewnętrznej bazy danych, takich jak podstawowe lub udostępnionych tajne uwierzytelniania HTTP Zobacz poniższym klipie wideo.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance

[Azure jednostki interfejsu API zarządzania pozostałych interfejsu API certyfikatu]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Konfigurowanie certyfikatu uwierzytelniania w witrynach Azure odwołują się do tego artykułu]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Konfigurowanie interfejsu API do uwierzytelniania bramy za pomocą certyfikatu klienta]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
