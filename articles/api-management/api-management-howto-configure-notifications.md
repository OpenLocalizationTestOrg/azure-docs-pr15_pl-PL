<properties 
    pageTitle="Jak skonfigurować powiadomienia i szablony wiadomości e-mail w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak skonfigurować powiadomienia i szablonów Azure interfejsu API zarządzania w wiadomości e-mail." 
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

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Jak skonfigurować powiadomienia i szablony wiadomości e-mail w zarządzaniu interfejsu API platformy Azure

Interfejs API zarządzania umożliwia konfigurowanie powiadomień dla określonych zdarzeń i skonfigurować szablony wiadomości e-mail, które są używane do komunikowania się z Administratorzy i deweloperzy wystąpienia interfejsu API zarządzania. W tym temacie pokazano, jak skonfigurować powiadomienia o zdarzeniach dostępne i omówiono konfigurowanie szablonów wiadomości e-mail na potrzeby te zdarzenia.

## <a name="publisher-notifications"> </a>Konfigurowanie powiadomień dla programu publisher

Aby skonfigurować powiadomienia, kliknij przycisk **Zarządzaj** w portalu klasycznym Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

Kliknij pozycję **powiadomienia** w menu **Zarządzania interfejsu API** po lewej stronie, aby wyświetlić dostępne powiadomienia.

![Powiadomienia programu Publisher][api-management-publisher-notifications]

Poniższa lista zdarzeń można skonfigurować powiadomienia.

-   **Żądania subskrypcji (wymaganie zatwierdzenia)** - adresatów określonej wiadomości e-mail i użytkowników, otrzymają powiadomień e-mail na temat subskrypcji żądania związane z produktami interfejsu API wymaganie zatwierdzenia.
-   **Nowe subskrypcje** - adresatów określonej wiadomości e-mail i użytkowników, otrzymają powiadomień e-mail na temat nowej subskrypcji produktu interfejsu API.
-   **Galeria wniosków** - adresatów określonej wiadomości e-mail i użytkowników otrzymają powiadomienia e-mail, gdy nowe aplikacje są przesyłane do galerii aplikacji.
-   **UDW** - adresatów określonej wiadomości e-mail i użytkowników otrzymają wiadomość e-mail kopie ukryte wszystkie wiadomości e-mail wysyłane do deweloperów.
-   **Nowy problem lub komentarz** - adresatów określonej wiadomości e-mail i użytkowników będzie otrzymywać powiadomienia e-mail, gdy nowy problem lub komentarz zostaje przesłane w portalu Deweloper.
-   **Zamknij konto wiadomości** — adresatów określonej wiadomości e-mail i użytkowników otrzymają powiadomienia e-mail, gdy konto jest zamknięty.
-   **Limit przydziału subskrypcji Approaching** — następujących adresatów wiadomości e-mail i użytkowników otrzymają powiadomienia e-mail, gdy subskrypcja zastosowania otrzymuje zbliżony przydział użycia.

Dla każdego zdarzenia możesz określić adresatów wiadomości e-mail w polu tekstowym adres e-mail lub użytkowników można wybrać z listy.

Aby określić adresy e-mail, aby otrzymywać powiadomienia, wprowadź je w polu tekstowym adres e-mail. Jeśli masz wiele adresów e-mail, oddzielając je przecinkami.

![Powiadomienie o adresatów][api-management-email-addresses]

Aby określić użytkowników, aby otrzymywać powiadomienia, kliknij pozycję **Dodaj adresatów**, zaznacz pole obok powiadomienia użytkowników, a następnie kliknij **przycisk OK**.

>Należy zauważyć, że tylko administratorzy są wyświetlane na liście.

Po skonfigurowaniu adresatów powiadomienia, kliknij przycisk **Zapisz** , aby zastosować adresatów powiadomienia zaktualizowane.

>Jeśli przejdziesz na karcie **Powiadomienia programu Publisher** portalu programu publisher alertów, gdy istnieją niezapisane zmiany.

## <a name="email-templates"> </a>Konfiguruj szablony wiadomości e-mail

Interfejs API zarządzania zawiera szablony wiadomości e-mail dotyczące wiadomości e-mail, które są wysyłane w trakcie administrowanie i korzystania z usługi. Dostępne są następujące szablony wiadomości e-mail.

-   Zatwierdzone przesyłania galerii aplikacji
-   Litera farewell Deweloper
-   Limit przydziału Deweloper zbliża powiadomień
-   Zapraszanie użytkownika
-   Nowy komentarz dodane do problemu
-   Nowy problem odebrana
-   Nowej subskrypcji aktywowany
-   Potwierdzenie odnowienia subskrypcji
-   Żądanie subskrypcji odmowie
-   Otrzymano żądanie subskrypcji

Te szablony można modyfikować pożądane.

Aby wyświetlić i skonfigurować szablony wiadomości e-mail dla wystąpienia interfejsu API zarządzania, kliknij **powiadomienia** z **Interfejsu API zarządzania** menu po lewej stronie, a wybierz kartę **Szablony wiadomości E-mail** .

![Szablony wiadomości e-mail][api-management-email-templates]

Aby wyświetlić lub zmodyfikować określonego szablonu, wybierz go z listy rozwijanej **Szablony** .

![Lista szablonów wiadomości e-mail][api-management-email-templates-list]

Każdy szablon wiadomości e-mail ma tematu w formacie zwykłego tekstu i definicji treści w formacie HTML. Każdy element można dostosować tak, jak odpowiednie.

![Edytor szablonu wiadomości e-mail][api-management-email-template]

Lista **parametrów** zawiera listę parametrów służący wstawione w temacie lub w treści, zostanie zastąpiony wyznaczone wartości po wysłaniu wiadomości e-mail. Aby wstawić parametr, umieść kursor w miejsce, w którym chcesz wstawić parametr, aby przejść, a następnie kliknij strzałkę znajdującą się po lewej stronie nazwy parametru.

Kliknij przycisk **Podgląd** lub **Wyślij test** , aby zobaczyć, jak wiadomości e-mail będzie wyglądać lub wysłać wiadomość e-mail z testu.

>Należy zauważyć, że parametry nie zostaną zastąpione wartościami rzeczywisty do wyświetlania podglądu lub wysyłania odpowiedzi w teście.
>
>Aby zapisać zmiany szablonu wiadomości e-mail, kliknij przycisk **Zapisz**lub, aby anulować zmiany kliknij przycisk **Anuluj**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance