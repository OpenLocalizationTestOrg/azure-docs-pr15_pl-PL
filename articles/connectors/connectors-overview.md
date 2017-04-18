<properties
    pageTitle="Omówienie łączników aplikacje logiczny | Microsoft Azure"
    description="Omówienie łączników, których można używać w aplikacji dla warunków logicznych"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Za pomocą łączników w aplikacji dla warunków logicznych

Łączniki zapewnienia szybkiego dostępu do zdarzenia, danych i akcje w usługach, protokoły i platformach.  Pełna lista łączników obsługuje aplikacje logika można [można znaleźć tutaj](apis-list.md).  Łączniki mogą być używane jako wyzwalacza lub akcji w aplikacji dla logiki i mogą wymagać skonfigurowane *połączenie* (na przykład: autoryzowanie konto Twitter, aby uzyskać dostęp do lub Opublikuj w Twoim imieniu).

## <a name="basics"></a>Podstawowe informacje

Łączniki są obsługiwane usługi, których masz dostęp jako część aplikacji logiczny, aby zintegrować z innymi usługami, takimi jak Dynamics Azure, usług Salesforce, [i więcej](apis-list.md).  Są one wdrożony i zarządzane przez firmę Microsoft, można je tworzyć integracji przepływów pracy przy użyciu skali, przepustowość i zabezpieczeń wykonywane obsługę.  Możesz dodać łącznik do aplikacji logiczny, wyszukiwanie i wybierając pozycję Akcja łącznik lub wyzwalacza w obszarze **Pokaż Microsoft zarządzane interfejsów API**.

![Menu akcji do zaznaczania wyzwalacza][1]

Każdej akcji łącznik lub wyzwalacza ma swój zestaw właściwości, aby skonfigurować.  Kliknij przyciski informacje, aby dowiedzieć się więcej na temat akcji lub odniesienie jego dokumentacji [Aby dowiedzieć się więcej](apis-list.md).

Jeśli chcesz zintegrować z usługą lub interfejsu API, która nie jest jeszcze łącznika, można także rozszerzanie aplikacji logika za pośrednictwem [łącznika niestandardowego](../app-service-logic/app-service-logic-create-api-app.md) lub po prostu połączenia bezpośrednio do usługi przez protokół, takich jak HTTP.

## <a name="triggers"></a>Wyzwalaczy

Niektóre łączniki mają wyzwalacza, co oznacza wydarzenia z tego łącznika spowoduje uruchomienie aplikacji logiki i przenieść wszystkie dane jako część wyzwalacza.  Wyzwalacz jest zawsze pierwszy krok w aplikacji logicznych.  Popularne wyzwalaczy dołączyć operacje, takie jak:
 
 * Cykl - uruchamianie co godzinę
 * Po odebraniu żądania HTTP
 * Po dodaniu elementu do kolejki
 * Po odebraniu wiadomości e-mail
 
Niektóre wyzwalaczy zostanie zastosowana błyskawicznych, które zdarzenie za pośrednictwem powiadomienie do aplikacji logiczny, a inne osoby muszą interwał cyklu skonfigurowane na częstotliwość aplikacji logika sprawdza, czy usługa zdarzenie (maksymalnie co 15 sekund).  

Po otrzymaniu zdarzenia, uruchom aplikację logiczny zostanie zastosowana, a akcje w ramach przepływu pracy zostanie uruchomiony.  Można również uzyskać dostęp do danych z wyzwalacza całej przepływu pracy (na przykład wyzwalacza "na nowy tweet" przejdzie tweet w procesie).

## <a name="actions"></a>Akcje

Większość łączniki mają jednego lub wielu akcji, które mogą być wykonywane w ramach przepływu pracy.  Akcje są wszystkie kroki wystąpić po Uruchom zostało uruchomione z wyzwalacza.  Aby dodać działanie, kliknij przycisk **Nowy krok** i wyszukaj łącznik chcesz użyć.  Kiedy zaznaczony (i po skonfigurowaniu żadnych [połączeń](#connections) , które mogą być wymagane) zostanie wyświetlona karta akcji, które można skonfigurować.  Wybieranie danych z powyższych czynności, klikając na wszystkich tokenów dla obrazów lub innych konfiguracji należy wprowadzić stosownie do potrzeb.

![Konfigurowanie akcji łącznika][2]

## <a name="connections"></a>Połączenia

Większość łączników trzeba skonfigurować *połączenie* , zanim będzie można używać łącznik.  *Połączenie* jest żadna konfiguracja połączenia lub logowania, aby uzyskać dostęp do łącznika.  Dla łączników, w których jest używany OAuth, tworzenie połączenia oznacza, że logowanie się do usługi (na przykład usługi Office 365, usługi Salesforce lub GitHub) miejsce, w którym można szyfrowane i bezpiecznie przechowywane w magazynie tajne Azure token dostępu.  Inne łączników (na przykład FTP i SQL) wymaga połączenia zawierający konfigurację, takie jak adres serwera, nazwę użytkownika i hasło.  Te informacje dotyczące konfiguracji połączenia są również szyfrowane i przechowywane bezpiecznie.  Połączenia będą mieli dostęp do obsługi dla, jak długo usługa umożliwia.  W przypadku połączeń Azure Active Directory OAuth (na przykład usługi Office 365 i Dynamics) firma Microsoft w dalszym ciągu czas nieokreślony odświeżanie token dostępu.  Innych usług mogą wprowadzone ograniczenia na jak długo firma Microsoft korzysta tokenu nie są odświeżane.  Ogólnie określonych akcji, takich jak zmienianie hasła będzie powodować wszystkich tokenów dostępu.  

Połączenia można wyświetlać i zarządzania nimi Azure, kliknąć przycisk **Przeglądaj** i wybierając **Interfejsu API połączenia**.  Z zasobu połączenia interfejsu API można wyświetlanie, edytowanie, aktualizowanie lub ponownej autoryzacji wszystkie połączenia, które zostały utworzone.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie pierwszej aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Dowiedz się, typowe zastosowania i przykłady dotyczące aplikacji warunków logicznych](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Rozpoczynanie pracy z wyzwalaczami integracji przedsiębiorstwa i akcje](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png