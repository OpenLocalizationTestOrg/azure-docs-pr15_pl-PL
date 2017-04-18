<properties 
    pageTitle="Jak zarządzać kontami użytkowników w zarządzaniu interfejsu API Azure | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć lub zapraszania użytkowników w zarządzaniu interfejsu API platformy Azure" 
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

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Jak zarządzać kontami użytkowników w zarządzania interfejsu API platformy Azure

W obszarze Zarządzanie interfejsu API Deweloperzy są użytkowników interfejsów API, które uwidaczniają, za pomocą interfejsu API zarządzania. Ten przewodnik przedstawia się, jak tworzyć i Zaproś deweloperów używania interfejsów API i produktów dokonaj dostępne z wystąpienia interfejsu API zarządzania. Dla informacji na temat zarządzania kontami użytkowników programistycznie zapoznaj się z dokumentacją [jednostki użytkownika](https://msdn.microsoft.com/library/azure/dn776330.aspx) w odwołaniu [Interfejsu API zarządzania pozostałych](https://msdn.microsoft.com/library/azure/dn776326.aspx) .

## <a name="create-developer"> </a>Tworzenie nowych Deweloper

Aby utworzyć nowy Deweloper, kliknij pozycję **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher. Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

![Portal programu Publisher][api-management-management-console]

W menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **Użytkownicy** , a następnie kliknij pozycję **Dodaj użytkownika**.

![Tworzenie Deweloper][api-management-create-developer]

Wprowadzanie **wiadomości E-mail**, **hasło**i **nazwę** dla nowego Deweloper, a następnie kliknij przycisk **Zapisz**.

![Tworzenie Deweloper][api-management-add-new-user]

Domyślnie nowo utworzony Deweloper konta są **aktywne**i skojarzonych z grupą **programista** .

![Nowy — Deweloper][api-management-new-developer]

Deweloper konta, które są w stanie **aktywnym** można uzyskiwać dostęp do wszystkich interfejsów API, dla którego mają subskrypcji. Aby skojarzyć nowo utworzonego Deweloper z dodatkowych grup, zobacz, [jak skojarzyć grup zawierających programista][].

## <a name="invite-developer"> </a>Zaprosić Deweloper

Aby zaprosić dewelopera, w menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **Użytkownicy** , a następnie kliknij **Zaprosić użytkownika**.

![Zapraszanie Deweloper][api-management-invite-developer]

Wprowadź adres e-mail i nazwę dewelopera, a następnie kliknij polecenie **Zaproś**.

![Zapraszanie Deweloper][api-management-invite-developer-window]

Zostanie wyświetlony komunikat z potwierdzeniem, ale nowo zaproszonych deweloper nie jest wyświetlana na liście, poczekaj, aż po jego zaakceptowaniu. 

![Zapraszanie potwierdzenia][api-management-invite-developer-confirmation]

Deweloper jest zapraszany, wiadomości e-mail są wysyłane do projektanta. Tej wiadomości e-mail, jest generowany przy użyciu szablonu i można dostosować. Aby uzyskać więcej informacji zobacz [Konfigurowanie szablonów wiadomości e-mail][].

Po zaakceptowaniu zaproszenia konta staje się aktywny.

## <a name="block-developer"></a> Dezaktywuj lub Aktywuj ponownie konta dewelopera

Domyślnie nowo utworzony lub zaproszonych Deweloper konta są **aktywne**. Aby dezaktywować konto Deweloper, kliknij przycisk **Zablokuj**. Aby ponownie aktywować konto zablokowanymi Deweloper, kliknij pozycję **Aktywuj**. Konta zablokowanych dewelopera nie można uzyskać dostęp do portalu deweloper lub połączeń dowolnego interfejsy API. Aby usunąć konto użytkownika, kliknij przycisk **Usuń**.

![Blokowanie Deweloper][api-management-new-developer]

## <a name="reset-a-user-password"></a>Resetowanie hasła użytkownika

Aby zresetować hasło dla konta użytkownika, kliknij nazwę konta.

![Resetowanie hasła][api-management-view-developer]

Kliknij przycisk **Resetuj hasło** , aby wysłać łącze do użytkownika, aby zresetować swoje hasło.

![Resetowanie hasła][api-management-reset-password]

Aby programowo pracować z kont użytkowników, zapoznaj się z dokumentacją [jednostki użytkownika](https://msdn.microsoft.com/library/azure/dn776330.aspx) w odwołaniu [Interfejsu API zarządzania pozostałych](https://msdn.microsoft.com/library/azure/dn776326.aspx) . Aby zresetować hasło do konta użytkownika do określonej wartości, można użyć operacji [Aktualizowanie użytkownika](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) i określ odpowiednie hasło.

## <a name="pending-verification"></a>Oczekiwanie na sprawdzenie

![Oczekiwanie na sprawdzenie][api-management-pending-verification]

## <a name="next-steps"> </a>Następne kroki

Po utworzeniu konta dewelopera, możesz skojarzyć ról i zasubskrybuj produktów oraz interfejsy API. Aby uzyskać więcej informacji zobacz [jak tworzenie i używanie grup][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Jak utworzyć i używanie grup]: api-management-howto-create-groups.md
[Jak chcesz skojarzyć deweloperów grup]: api-management-howto-create-groups.md#associate-group-developer

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Konfigurowanie szablonów wiadomości e-mail]: api-management-howto-configure-notifications.md#email-templates