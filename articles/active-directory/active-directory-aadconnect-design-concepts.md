<properties
   pageTitle="Narzędzie Azure AD Connect: Projektu | Microsoft Azure"
   description="W tym temacie opisano pewne obszary projektu implementacji"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Narzędzie Azure AD Connect: Projektu
Celem w tym temacie jest do opisu obszarów, które można traktować za pośrednictwem podczas planowania wdrożenia Azure AD Connect. W tym temacie jest głębokości dive w niektórych obszarach i te pojęcia pokrótce opisano w także inne tematy.

## <a name="sourceanchor"></a>sourceAnchor
Atrybut sourceAnchor jest definiowana jako *atrybut niezmienne czasie trwania obiektu*. Musi jednoznacznie identyfikować obiektu jako samego obiektu lokalnego i Azure AD. Atrybut jest nazywany **immutableId** i dwóch nazw są używane wymiennym.

Niezmienne, wyraz, który jest "nie można zmienić", należy w tym temacie. Ponieważ nie można zmienić wartości ten atrybut, gdy została ustawiona, należy wybierz projekt, który obsługuje rozwiązania.

Atrybut jest używany w następujących przypadkach:

- Gdy nowy serwer aparat synchronizacji jest wbudowany lub odbudowany po scenariuszu odzyskiwania po awarii, ten atrybut łączy istniejących obiektów w Azure AD przy użyciu obiektów lokalnego.
- Po przeniesieniu z tożsamości tylko do chmury do modelu zsynchronizowane tożsamości następnie ten atrybut pozwala obiektów "słabo dopasowania" istniejących obiektów w Azure AD z obiektami lokalnej.
- Jeśli używasz Federacji, ten atrybut razem z **userPrincipalName** jest używany w w roszczenia jednoznacznie zidentyfikować użytkownika.

Ten temat zawiera tylko o sourceAnchor w odniesieniu do użytkowników. Te same reguły mają zastosowanie do wszystkich typów obiektów, ale jest tylko dla użytkowników, że ten problem zazwyczaj jest.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Wybieranie atrybut dobre sourceAnchor
Wartość atrybutu należy wykonać następujące reguły:

- Być mniejsza niż 60 znaków
    - Znaki, które nie są w z, A-Z lub 0-9 kodowania i liczona jako 3 znaków
- Nie zawiera znaki specjalne: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ (< >) "; : , [ ] " @ _
- Musi być unikatowa globalnie
- Musi być ciąg, liczba całkowita lub binarny
- Nie powinny być oparte na nazwę użytkownika, te zmiany
- Nie powinny być uwzględniana wielkość liter i uniknąć wartości, które mogą się zmieniać w przypadku
- Mają być przydzielane po utworzeniu obiektu

Zaznaczonego sourceAnchor nie jest typu ciąg, następnie Azure AD Base64Encode łączenie wartość atrybutu zapewnienie znaków specjalnych nie są wyświetlane. Jeśli używasz innego serwer federacyjny niż ADFS, upewnij się, serwer można także Base64Encode atrybutu.

Atrybut sourceAnchor jest uwzględniana wielkość liter. Wartość "JanNowak" nie jest taki sam, jak "JanNowak". Ale nie ma dwa różne obiekty z różnicą tylko w przypadku.

Jeśli masz jeden las lokalnego, następnie atrybut, który należy używać jest **objectGUID**. Jest to także atrybutu używanego przy użyciu ustawienia ekspresowe w narzędzie Azure AD Connect, a także atrybutu używane przez usługę DirSync.

Jeśli masz wiele lasów i nie przenoś użytkowników między lasami i domen, **objectGUID** jest dobrym atrybut korzystać nawet w tym przypadku.

Po przeniesieniu użytkowników między lasami i domeny, a następnie należy znaleźć atrybut, który nie zmienia się lub mogą być przenoszone z użytkownikami podczas przenoszenia. Zalecane podejście jest wprowadzenie syntetycznych atrybut. Odpowiednia może być atrybut, który można umieścić coś wyglądającego identyfikator GUID. Podczas tworzenia obiektu nowy identyfikator GUID jest tworzony i dołączana do użytkownika. Reguły niestandardowe synchronizacji mogą być tworzone na serwerze aparatu synchronizacji do tworzenia tej wartości według **objectGUID** i aktualizowania zaznaczony atrybut w DODAJE. Podczas przenoszenia obiektu, należy również skopiować zawartość tej wartości.

Innym rozwiązaniem jest, aby wybrać istniejący atrybut, które znasz, nie zmienia się. Najczęściej używanych atrybutów zawiera **pole Identyfikator pracownika**. Jeśli uważasz atrybut, który zawiera litery, upewnij się, że bez możliwości wielkości liter (wielkie i małe litery) można zmienić wartość atrybutu jest. Nieprawidłowe atrybuty, które nie powinny być używane zawierać te atrybuty z nazwą użytkownika. Małżeństwa lub rozwodu nazwę oczekuje się zmienić, co nie jest dozwolone dla tego atrybutu. Jest to także powodem dlaczego atrybuty, takie jak **userPrincipalName**, **Poczta**i **targetAddress** nie są jeszcze można wybrać w Kreatorze instalacji narzędzie Azure AD Connect. Te atrybuty również zawierać @-character, co nie jest dozwolone w sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Zmienianie atrybutu sourceAnchor
Wartość atrybutu sourceAnchor nie można zmienić po obiekt został utworzony w Azure AD i tożsamości są synchronizowane.

Z tego powodu narzędzie Azure AD Connect dotyczą następujące ograniczenia:

- Atrybut sourceAnchor można ustawić tylko podczas początkowej instalacji. Jeśli Uruchom Kreatora instalacji, ta opcja jest tylko do odczytu. Jeśli chcesz zmienić to ustawienie, należy odinstalować i zainstalować ponownie.
- Po zainstalowaniu inny serwer Azure AD Connect, należy wybrać tę samą atrybutu sourceAnchor, jak wcześniej używana. Jeśli wcześniej została przy użyciu DirSync i przechodzenie do Azure AD Connect, to należy użyć **objectGUID** ponieważ jest atrybut używane przez usługę DirSync.
- Jeśli wartość w polu sourceAnchor zostanie zmieniony po obiekt wyeksportowanej Azure AD, następnie Azure AD Connect synchronizacja zgłasza błąd i nie zezwala na zmiany w obiekt przed problem został rozwiązany i sourceAnchor zostanie zmieniony w katalogu źródłowym.

## <a name="azure-ad-sign-in"></a>Azure AD logowania
Podczas integracji katalogu lokalnego z usługą Azure Active Directory, ważne jest, aby dowiedzieć się, jak ustawienia synchronizacji mogą wpływać na sposób użytkownika uwierzytelnia. Azure AD używa userPrincipalName (UPN) do uwierzytelnienia użytkownika. Jednak podczas synchronizowania użytkowników możesz wybrać atrybut, który ma być używany dla wartości userPrincipalName starannie.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Wybieranie atrybut userPrincipalName
Podczas wybierania atrybutu umożliwiające powinno wartość głównej nazwy użytkownika może być używany w jednym Azure

- Wartości atrybutów odpowiadają UPN składnię (RFC 822), że należy go formatuusername@domain
- Sufiks wartości pasuje do jednej z zweryfikowanych domen niestandardowych w Azure AD

W obszarze Ustawienia ekspresowe założonej wybór atrybutu jest userPrincipalName. Jeśli ten atrybut userPrincipalName nie zawiera wartości chcesz, aby użytkownicy się zalogować do Azure, a następnie należy wybrać **Instalację niestandardową**.

### <a name="custom-domain-state-and-upn"></a>Stan domeny niestandardowej i głównej nazwy użytkownika
Należy się upewnić, że jest zweryfikowaną domeną sufiksu głównej nazwy użytkownika.

Jan jest użytkownika w contoso.com. Chcesz Jan używać lokalnej głównej john@contoso.com się zalogować do Azure po zostały zsynchronizowane użytkowników do usługi Azure AD katalogu contoso.onmicrosoft.com. W tym celu należy dodać i zweryfikować contoso.com jako domenę niestandardową w Azure AD przed rozpoczęciem synchronizowania użytkowników. Jeśli w Azure AD sufiksu głównej nazwy użytkownika z Jan, na przykład contoso.com, jest niezgodny z zweryfikowaną domeną, wówczas Azure AD zamienia sufiksu głównej nazwy użytkownika contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Domeny lokalnej obsługi routingu i UPN dla Azure AD
Niektóre organizacje są obsługi routingu domeny, takiej jak contoso.local lub prostych pojedyncza etykieta domen, takich jak contoso. Nie jest możliwe zweryfikować domenę obsługi routingu w Azure AD. Narzędzie Azure AD Connect do zweryfikowaną domeną w synchronizować Azure AD. Po utworzeniu katalogu Azure AD tworzy domenę routingu, która staje się domyślną domenę dla usługi Azure AD, na przykład contoso.onmicrosoft.com. Dlatego konieczne jest Sprawdź innej domeny routingu w takiej sytuacji, w przypadku, gdy nie chcesz, aby zsynchronizować domyślną domenę onmicrosoft.com.

Aby uzyskać dodatkowe informacje dotyczące dodawania i weryfikowanie domeny, przeczytaj [Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory](active-directory-add-domain.md) .

Narzędzie Azure AD Connect wykrywa Jeśli pracujesz w środowisku domeny obsługi routingu i odpowiednio czy ostrzeżenie z wyprzedzeniem wdrożyć usługę Ustawienia ekspresowe. Jeśli pracujesz w domenie obsługi routingu, następnie prawdopodobnie UPN użytkowników, że obsługi routingu sufiksów zbyt. Na przykład, jeśli są uruchomione w obszarze contoso.local, narzędzie Azure AD Connect sugeruje na używanie ustawień niestandardowych, a nie przy użyciu ustawień express. Przy użyciu ustawień niestandardowych, jest możliwe określić atrybut, który powinien służyć jako głównej nazwy użytkownika do zalogowania się do Azure, gdy użytkownicy są synchronizowane z Azure AD.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
