<properties
    pageTitle="Słownik ochrony tożsamości Azure Active Directory | Microsoft Azure"
    description="Słownik ochrony tożsamości Azure Active Directory"
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory chmury aplikacji odnajdowanie, zarządzanie aplikacji, ryzyka, poziom ryzyka, luka w zabezpieczeniach, zasad zabezpieczeń, zabezpieczenia — słownik"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Słownik ochrony tożsamości Azure Active Directory 


### <a name="at-risk-user"></a>Na ryzyko (użytkownika)  
Użytkownik z jednego lub większej liczby zdarzeń aktywnych czynników ryzyka. 

### <a name="atypical-sign-in-location"></a>Nietypowe lokalizacji logowania   
Logowanie z lokalizacji geograficznej, która nie jest typowe dla określonego użytkownika, podobnych użytkowników lub dzierżawy.

### <a name="azure-ad-identity-protection"></a>Ochrona tożsamości Azure AD    
Zabezpieczenia moduł usługi Azure Active Directory, zawierającego skonsolidowany widok do zdarzenia ryzyka i słabych wpływu tożsamości w organizacji.

### <a name="conditional-access"></a>Dostępu warunkowego  
Zasady dotyczące zabezpieczania dostępu do zasobów. Reguły warunkowego dostępu są przechowywane w usłudze Azure Active Directory i będą sprawdzane przy Azure AD przed przyznaniem dostępu do tego zasobu.  Przykładowe reguły obejmują ograniczanie dostępu na podstawie lokalizacji użytkownika metody uwierzytelniania zdrowia lub użytkownika urządzenia.

### <a name="credentials"></a>Poświadczenia     
Informacje, które obejmują identyfikator i potwierdzenie identyfikacji używane do uzyskania dostępu do lokalnych i zasobów. Przykładami poświadczeń są nazwy użytkownika i hasła, certyfikaty i kartach inteligentnych.

### <a name="event"></a>Zdarzenia   
Rekord działania w usłudze Azure Active Directory.

### <a name="false-positive-risk-event"></a>Wynik dodatni na FAŁSZ (zdarzenie ryzyka) 
Stan zdarzenia ryzyka Ręczne ustawianie użytkownik ochronę tożsamości, informujący, że zdarzenia ryzyka firmy i niepoprawnie został oznaczony jako zdarzenie ryzyka.

### <a name="identity"></a>Tożsamości    
Osoba lub jednostki, który musi zostać zweryfikowane, za pomocą uwierzytelniania na podstawie kryteriów, takich jak hasło lub certyfikat.

### <a name="identity-risk-event"></a>Zdarzenia ryzyka tożsamości     
Zdarzenie AAD został oznaczony jako anomalous przez ochronę tożsamości, która może oznaczać, że jest już bezpieczne tożsamości.

### <a name="ignored-risk-event"></a>Ignorowane (zdarzenie ryzyka)    
Stan zdarzenia ryzyka Ręczne ustawianie użytkownik ochronę tożsamości, wskazująca, że zdarzenia ryzyka jest zamknięte bez wniesienia naprawy.

### <a name="impossible-travel-from-atypical-locations"></a>Możliwe podróży z nietypowe lokalizacji   
Zdarzenie ryzyka, wyzwalane, gdy dwie rejestrowaniu dla tego samego użytkownika zostaną wykryte, gdzie co najmniej jeden z nich jest z nietypowe lokalizacji logowania, a czasu między dodatki logowania jest mniejsza niż minimalny czas, który chcesz wykonać, aby fizycznie podróży między te miejsca.  

###<a name="investigation"></a>Badanie    
Opis procesu recenzowania działań, dzienników i innych istotnych informacji związanych z wydarzeniem ryzyka zdecydować, czy naprawy lub łagodzenia czynności są konieczne, jeśli i jak zostało naruszone tożsamości i poznać zasady używania złamany tożsamości.

### <a name="leaked-credentials"></a>Przecieku poświadczeń  

Zdarzenie ryzyka, wyzwalane, gdy bieżących poświadczeń użytkownika (nazwa użytkownika i hasło) znajdują się opublikowany publicznie w sieci web, ciemny przez naszych pracowników badawczych.

### <a name="mitigation"></a>Ograniczenia  
Akcja, aby ograniczyć lub wyeliminować możliwość atakującej wykorzystać tożsamości złamany lub urządzenia bez przywrócenia tożsamości lub urządzenia to bezpieczne. Łagodzenia nie rozpoznaje poprzedniego zdarzenia ryzyka skojarzone z tożsamości lub urządzenia.

### <a name="multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe 
Metody uwierzytelniania, która wymaga dwóch lub więcej metod uwierzytelniania, co może obejmować coś użytkownik ma certyfikat; coś zna, takich jak nazwy użytkowników, hasła lub wyrażenia przebiegu; fizyczny atrybutów, takich jak odcisku palca; i osobiste atrybutów, takich jak osobisty podpis.

### <a name="offline-detection"></a>Wykrywanie trybu offline   
Wykrywanie różnic w odniesieniu i ocena ryzyka zdarzenie na przykład próba logowania po zakończeniu, na zdarzenie, które już wystąpiło zdarzenie.

### <a name="policy-condition"></a>Warunek zasad    
Część zasad zabezpieczeń, który definiuje jednostek (grup, użytkowników, aplikacje, platformy urządzeń, Stany urządzeń, zakresów adresów IP, typy klienta) zasady włączone lub wyłączone z niego.

### <a name="policy-rule"></a>Reguła     
Część zasad zabezpieczeń, który opisuje okoliczności, które spowoduje zasady, a inne czynności wyzwolenia zasady.

### <a name="prevention"></a>Zapobieganie  
AKCJA Aby zapobiec uszkodzeniu organizacji za pomocą abuse tożsamości lub urządzenie wiadomości wydające się być lub wiedzieć, aby zostać złamane. Akcja zapobiegania nie chroni urządzenie lub tożsamości, a nie rozwiąże poprzedniego zdarzenia ryzyka.

### <a name="privileged-user"></a>Uprawnieniach (użytkownika)
Użytkownik, który w momencie zdarzenie ryzyka w wyniosła mniej niż uprawnienia administratora trwałe lub tymczasowe do co najmniej jeden zasób usługi Azure Active Directory, takich jak Administrator globalny, Administrator rozliczeń, Administrator usługi, administrator użytkowników i hasła administratora. 

###<a name="real-time"></a>W czasie rzeczywistym    
Zobacz wykrywanie w czasie rzeczywistym.

###<a name="real-time-detection"></a>Wykrywanie w czasie rzeczywistym  
Wykrywanie różnic w odniesieniu i ocena ryzyka zdarzenie na przykład próba logowania przed zdarzeniem może kontynuować.

### <a name="remediated-risk-event"></a>Wyeliminować (zdarzenie ryzyka)     
Stan zdarzenia ryzyka ustawiana automatycznie przez ochronę tożsamości, wskazująca zdarzenia ryzyka została wyeliminować, przy użyciu akcji naprawy standardowego dla tego typu zdarzenia ryzyka. Na przykład po zresetowaniu hasła użytkownika są automatycznie wyeliminować wiele zdarzenia ryzyka, które wskazują, że poprzednie hasło zostało złamane.

### <a name="remediation"></a>Rozwiązywanie problemu     
Akcja zapewnienie tożsamości lub urządzenie, które zostały wcześniej wiadomości wydające się być lub wiadomo, że zostać złamane. Akcja naprawy przywraca tożsamości lub urządzenia to bezpieczne i usuwa poprzednie zdarzenia ryzyka skojarzone z tożsamości lub urządzenia.

### <a name="resolved-risk-event"></a>Rozpoznawania (zdarzenie ryzyka)   
Ręczne ustawianie przez użytkownika ochronę tożsamości stanu zdarzenia ryzyka, wskazująca, że użytkownik miał akcji odpowiednie korygowania poza ochronę tożsamości i należy uwzględnić zdarzenia ryzyka zamknięty.

###<a name="risk-event-status"></a>Stan zdarzenia ryzyka    
Właściwość zdarzenia ryzyka, wskazująca, czy zdarzenie jest aktywna, a jeśli zamknięte, przyczynę zamknięcia go.

###<a name="risk-event-type"></a>Typ zdarzenia ryzyka  
Kategoria zdarzenia ryzyka, wskazującą typ anomalii, które spowodowało zdarzenia, które mają być traktowane jako ryzykowna.

###<a name="risk-level-risk-event"></a>Poziom ryzyka (ryzyka event)  
Wskazanie (wysoki, średni lub niski) ważności zdarzenia ryzyka ułatwiających ochronę tożsamości Wyznaczanie priorytetów akcje przyjmować zmniejszenie ryzyka do swojej organizacji. 

###<a name="risk-level-sign-in"></a>Poziom ryzyka (Zaloguj) 
Wskazanie (wysoki, średni lub niski) prawdopodobieństwo, że dla określonych logowania, ktoś próbuje użyć tożsamość użytkownika.

###<a name="risk-level-user-compromise"></a>Poziom ryzyka (złamania użytkownika) 
Wskazanie (wysoki, średni lub niski) prawdopodobieństwo, że jest już bezpieczne tożsamości.

### <a name="risk-level-vulnerability"></a>Poziom ryzyka (luka)  
Wskazanie (wysoki, średni lub niski) ważności luka ułatwiających ochronę tożsamości Wyznaczanie priorytetów akcje przyjmować zmniejszenie ryzyka do swojej organizacji.

### <a name="secure-identity"></a>Zabezpieczanie (tożsamość)   
Wykonywanie akcji rozwiązywanie problemu, takie jak zmiana hasła lub machine reimaging przywrócić stan Niezrównana potencjalnie złamany tożsamości.

### <a name="security-policy"></a>Zasady zabezpieczeń
Kolekcja reguł i warunków. Zasady można stosować do obiektów, takich jak użytkownicy, grupy, aplikacje, urządzenia, platformy urządzeń, Stany urządzeń, zakresów adresów IP i typami klienta Auth2.0. Gdy jest włączona, zostanie ona potraktowana zawsze, gdy jednostka objęte zasady jest wydawany token dla zasobu.

### <a name="sign-in-v"></a>Zaloguj się (v) 
Do uwierzytelniania tożsamości w usłudze Azure Active Directory.

### <a name="sign-in-n"></a>Logowania (n) 
Proces lub akcji uwierzytelniania tożsamości usługi Azure Active Directory i zdarzenia, które znajdują się tej operacji.

###<a name="sign-in-from-anonymous-ip-address"></a>Logowanie z anonimowych adresu IP    
Zdarzenia ryzyka uruchomienie po pomyślnego logowania z adresu IP, który wykryto za pomocą adresu IP serwera proxy anonimowe.

###<a name="sign-in-from-infected-device"></a>Logowanie z zainfekowanego urządzenia 
Zdarzenie ryzyka, wyzwalane, gdy logowania pochodzi z adresu IP, w których jest używane przez jeden lub więcej urządzeń złamany, które aktywnie spróbujesz komunikować się z serwerem robot.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Logowanie z adresu IP z podejrzane działania 
Zdarzenia ryzyka uruchomienie po pomyślnym logowanie z adresów IP adresowa z duża liczba prób logowania nie powiodło się przez wiele kont użytkowników przez krótki czas.

###<a name="sign-in-from-unfamiliar-location"></a>Logowanie z nieznanych lokalizacji 
Zdarzenie ryzyka, wyzwalane, gdy użytkownik pomyślnie rejestruje w nowej lokalizacji (IP i szerokość/długość geograficzna ASN).

###<a name="sign-in-risk"></a>Logowania ryzyka     
Zobacz ryzyka poziom (Zaloguj)

###<a name="sign-in-risk-policy"></a>Zasady logowania ryzyka  
Zasady dostępu warunkowego, które oblicza czynnik ryzyka określonych logowania i dotyczy czynniki na podstawie wstępnie zdefiniowanego warunki i reguły.

###<a name="user-compromise-risk"></a>Ryzyko złamania użytkownika     
Zobacz ryzyka poziom (złamania użytkownika)

###<a name="user-risk"></a>Użytkownik ryzyka    
Zobacz ryzyka poziom (złamania użytkownika).

###<a name="user-risk-policy"></a>Zasady użytkownika ryzyka     
Zasady dostępu warunkowego, które są uwzględnione logowania i dotyczy czynniki na podstawie wstępnie zdefiniowanego warunki i reguły.

###<a name="users-flagged-for-risk"></a>Użytkownicy flagą ryzyka   
Użytkownicy, którzy mają zdarzenia ryzyka, które są aktywne lub wyeliminować

###<a name="vulnerability"></a>Luka w zabezpieczeniach    
Konfiguracja lub warunku w usługi Azure Active Directory, dzięki czemu katalogu podatne na zagrożenia lub zagrożeń.


## <a name="see-also"></a>Zobacz też

- [Ochrona tożsamości Azure Active Directory](active-directory-identityprotection.md)
