<properties
   pageTitle="Wykres Azure Active Directory interfejsu API | Microsoft Azure"
   description="Omówienie i szybki start przewodnik interfejsu API wykres, który umożliwia programowy dostęp do Azure AD za pośrednictwem interfejsu API usługi REST punktów końcowych."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Wykres Azure Active Directory interfejsu API

> [AZURE.IMPORTANT] Funkcje Azure AD wykresu interfejsu API jest również dostępne za pośrednictwem [Programu Microsoft Graph](https://graph.microsoft.io/), ujednolicony interfejs API, który zawiera interfejsy API z innych usług firmy Microsoft, takich jak Outlook, usługi OneDrive programu OneNote, terminarz i funkcji Office Graph, dostępne za pośrednictwem jeden punkt końcowy i przy użyciu tokenu pojedynczy dostęp.

Z Azure Active Directory wykresu interfejsu API zapewnia programowy dostęp do Azure AD za pośrednictwem interfejsu API usługi REST punktów końcowych. Aplikacje za pomocą interfejsu API wykresu można wykonywać tworzenie, odczytywanie, aktualizowanie i usuwanie operacji (OBSŁUGIWAŁ) w katalogu danych i obiektów. Na przykład wykres interfejsu API obsługuje następujące operacje typowe obiekt użytkownika:

- Tworzenie nowego użytkownika w katalogu

- Uzyskaj szczegółowe właściwości użytkownika, takie jak ich grup

- Aktualizowanie właściwości użytkownika, takie jak ich lokalizacji i numer telefonu lub zmieniać swoje hasła

- Zaznacz użytkownika członkostwo w grupach dostępu oparta na rolach

- Wyłączanie konta użytkownika lub całkowicie usunąć

Oprócz obiektów użytkownika można wykonywać operacje podobne do innych obiektów, takich jak grupy i aplikacje. Aby nawiązać połączenie z interfejsu API wykres w katalogu, aplikacja musi być zarejestrowany z usługą Azure Active Directory i jest skonfigurowany do zezwalania na dostęp do katalogu. Zazwyczaj jest to realizowane przez użytkownika lub administratora przepływu zgody.

Aby rozpocząć korzystanie z platformy Azure Active Directory wykresu interfejsu API, zobacz [Przewodnik Szybki Start interfejsu API wykresu](active-directory-graph-api-quickstart.md)lub zapoznaj się z [interakcyjnym dokumentacji interfejsu API wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Funkcje

Interfejs API wykresu oferuje następujące funkcje:

- **Punkty końcowe interfejsu API pozostałych**: API wykres jest usługą RESTful obejmuje punkty końcowe, które są dostępne za pomocą standardowej żądania HTTP. Interfejs API wykresu obsługiwane typy zawartości XML lub notacji obiektu Javascript (JSON) dla zaproszenia i odpowiedzi. Aby uzyskać więcej informacji zobacz [odwołanie interfejsu API pozostałych Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Uwierzytelnianie za pomocą Azure AD**: każdego żądania API wykresu muszą zostać uwierzytelnione przez dołączenie JSON Web Token (JWT) w nagłówku autoryzacji żądania. Token ten zostanie pobrana żądanie Azure AD token punktu końcowego i ich dostarczania prawidłowe poświadczenia. Można użyć przepływu poświadczeń klienta OAuth 2.0 lub kodu autoryzacji udzielić przepływu umożliwia uzyskanie tokenu połączenie na wykresie. Aby uzyskać więcej informacji, [OAuth 2.0 w Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Autoryzacja oparta na rolach (RBAC)**: grupy zabezpieczeń są używane do wykonywania RBAC w interfejsie API wykresu. Na przykład jeśli chcesz określić, czy użytkownik ma dostęp do określonego zasobu, aplikacja może wywołać operacji [Sprawdzanie członkostwo w grupach (przechodnie)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) , która zwraca wartość PRAWDA lub FAŁSZ.

- **Kwerendy różnicy**: Jeśli chcesz wyszukania zmian w katalogu między dwoma okresami czasu bez konieczności tworzenia kwerend częste API wykres, można sprawić, żądanie różnicy kwerendy. Ten typ żądania może zwrócić tylko zmiany dokonane od poprzedniego żądania różnicy kwerendy do bieżącego żądania. Aby uzyskać więcej informacji zobacz [kwerendy różnicy interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Rozszerzenia katalogu**: Jeśli tworzysz aplikację, która wymaga odczytu lub zapisu unikatowe właściwości dla obiektów katalogu może rejestrować i użyj rozszerzenia wartości przy użyciu interfejsu API wykresu. Na przykład jeśli aplikacja wymaga Właściwość Identyfikator Skype dla każdego użytkownika, Nowa właściwość można zarejestrować w katalogu i będzie on dostępny dla każdego obiektu użytkownika. Aby uzyskać więcej informacji zobacz [rozszerzenia schematu katalogu interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Bezpieczne przez zakresy uprawnień**: interfejs API wykresu AAD udostępnia zakresów uprawnień, które umożliwiają bezpiecznego zgodę dostęp do danych AAD i obsługa techniczna różnych typów aplikacji klienta, w tym:
 - z interfejsu użytkownika, które podano delegowana dostęp do danych za pośrednictwem autoryzacji z zalogowany użytkownik (delegowanych)
  - tych, które korzystają aplikacji zdefiniować kontrola dostępu oparta na rolach, takich jak klienci usługi demon (aplikacji role)

    Zarówno delegowanej i zakresów uprawnień roli aplikacji reprezentują uprawnienia ujawnionego przez interfejs API wykres i mogą być żądane w aplikacjach klienckich za pośrednictwem aplikacji rejestracji uprawnienia [funkcje w portalu klasyczny Azure](https://manage.windowsazure.com). Klienci, można sprawdzić zakresów uprawnień przyznanych do nich sprawdzając roszczeń zakresu ("połączenia") otrzymanych w token dostępu delegowane uprawnienia i role ("role") roszczeń dla uprawnienia roli aplikacji. Dowiedz się więcej o [zakresach uprawnień interfejsu API Azure AD wykres](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Scenariusze

Interfejs API wykres umożliwia wielu scenariuszy aplikacji. Najczęściej używane są następujące scenariusze:

- **Aplikacja linii biznesowych (pojedynczy dzierżawy)**: W tym scenariuszu Deweloper przedsiębiorstwa działa w przypadku organizacji, która ma subskrypcję usługi Office 365. Projektanta tworzenie aplikacji sieci web, który współdziała z usługą Azure Active Directory do wykonywania zadań takich przypisywania licencji do użytkownika. To zadanie wymaga dostępu do API wykres, aby rejestruje Deweloper pojedynczy dzierżawa aplikacji Azure AD i konfiguruje odczytu i zapisu dla interfejsu API wykres. Następnie aplikacja jest skonfigurowana do używać własnej poświadczenia lub tych obecnie logowania użytkownika w celu pobrania tokenu nawiązać połączenie z interfejsu API wykresu.

- **Oprogramowanie jako aplikacja usługi (wielu dzierżawy)**: W tym scenariuszu niezależny dostawca oprogramowania (Model) jest opracowywania aplikacji hostowanej dzierżawy wielu sieci web, która zapewnia funkcje zarządzania użytkowników innymi organizacjami używającymi Azure AD. Te funkcje wymagają dostępu do obiektów katalogu, a więc aplikacja musi nawiązać połączenie z interfejsu API wykresu. Deweloper rejestruje aplikacji w Azure AD, konfiguruje ją do wymagają odczytu i zapisu interfejsu API wykresu, a potem umożliwią dostępu zewnętrznego, tak aby innych organizacjach mogą przyznać w katalogu ich za pomocą aplikacji. Uwierzytelnianie użytkownika w innej organizacji do aplikacji po raz pierwszy, są wyświetlane okno dialogowe zgody z uprawnieniami, który żąda aplikacja.  Udzielanie, że zgody zostanie następnie nadaj aplikacji te wymagane uprawnienia, aby interfejs API wykres w katalogu użytkownika. Aby uzyskać więcej informacji w ramach zgody zobacz [Omówienie Framework zgodę](active-directory-integrating-applications.md).

## <a name="see-also"></a>Zobacz też

[Przewodnik Szybki Start Azure AD wykresu interfejsu API](active-directory-graph-api-quickstart.md)

[Dokumentacja Umieść wykres AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Przewodnik programisty usługi Azure Active Directory](active-directory-developers-guide.md)
