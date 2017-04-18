<properties 
    pageTitle="Uwierzytelnianie wieloskładnikowe Azure — jak to działa"
    description="Azure uwierzytelnianie wieloskładnikowe ułatwia ochronę informacji dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania. Zapewnia dodatkowe zabezpieczenia przez wymaganie drugi formularz uwierzytelniania, a zapewnia silne uwierzytelnianie za pomocą zakresu opcje łatwe weryfikacji."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Jak działa uwierzytelnianie wieloskładnikowe Azure

Bezpieczeństwo uwierzytelnianie wieloskładnikowe znajduje się w jej ułożone warstwowo podejście. Naruszenie wiele czynników uwierzytelniania przedstawia stanowiło problem dla intruzów. Nawet jeśli atakującej uda się informacje hasło użytkownika, jest bezużyteczny również bez posiadania urządzenie zaufane. Należy użytkownik utraci urządzenia, osoba, która umożliwia znalezienie go będzie można używać tej funkcji, chyba że dana osoba również zna hasło użytkownika.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure uwierzytelnianie wieloskładnikowe ułatwia ochronę informacji dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania.  Zapewnia dodatkowe zabezpieczenia przez wymaganie drugą formę uwierzytelniania, a zapewnia silne uwierzytelnianie za pośrednictwem zakresu opcje weryfikacji proste:

- Rozmowa telefoniczna
- wiadomość tekstowa
- powiadomienie o aplikacji dla urządzeń przenośnych — dzięki czemu użytkownicy mogą wybrać metodę preferują
- kod weryfikacyjny aplikacji dla urządzeń przenośnych
- tokeny PRZYSIĘGĄ strona 3

Aby uzyskać dodatkowe informacje Niestety jak to działa Zobacz poniższym klipie wideo.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Metody uwierzytelniania wieloskładnikowego
Gdy użytkownik zaloguje się, dodatkowej weryfikacji jest wysyłany do użytkownika.  Poniżej przedstawiono listę metod, które mogą być używane dla drugiego weryfikacji.

Metodę weryfikacji  | Opis
------------- | ------------- |
Rozmowa telefoniczna | Połączenie jest umieszczana smartfonu użytkownika prosząc o zweryfikować, że są one logowanie się, naciskając klawisz znak #.  Proces weryfikacji zostanie zakończony.  Ta opcja jest konfigurowany i może zostać zmieniony kod, który można określić.
Wiadomość tekstowa | Wiadomość tekstowa będą wysyłane do użytkownika smartfonu z kodem 6 cyfr.  Wprowadź kod w, aby ukończyć proces weryfikacji.
Powiadomienie o aplikacji dla urządzeń przenośnych | Żądanie weryfikacji zostanie wysłane do użytkownika telefon inteligentny prosząc wykonania weryfikacji, wybierając pozycję Weryfikuj z aplikacji dla urządzeń przenośnych. Taka sytuacja występuje w przypadku wybrania powiadomienie aplikacji jako metodę weryfikacji podstawowego.  Jeśli ta osoba otrzyma to po ich nie logowanie, ich można wybrać opcję to zgłosić jako oszustwa.
Kod weryfikacyjny przy użyciu aplikacji Mobile | Kod weryfikacyjny będą wysyłane do aplikacji dla urządzeń przenośnych uruchomionym na telefon inteligentny użytkownika.  Taka sytuacja występuje w przypadku wybrania kod weryfikacyjny jako metodę weryfikacji podstawowego.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Dostępne wersje uwierzytelnianie wieloskładnikowe Azure
Uwierzytelnianie wieloskładnikowe Azure jest dostępna w trzech różnych wersji.  W poniższej tabeli opisano każdego z nich bardziej szczegółowo.

Wersja  | Opis
------------- | ------------- |
Uwierzytelnianie wieloskładnikowe dla usługi Office 365 | Ta wersja działa wyłącznie z aplikacji usługi Office 365 i odbywa się w portalu usługi Office 365. Dlatego administratorzy mogą teraz zabezpieczyć ich zasobami usługi Office 365 przy użyciu uwierzytelnianie wieloskładnikowe.  Ta wersja jest zawarty w subskrypcji usługi Office 365.
Uwierzytelnianie wieloskładnikowe dla administratorów Azure | Tym samym podzestaw możliwości uwierzytelnianie wieloskładnikowe usługi Office 365 będą dostępne bezpłatnie do wszystkich administratorów Azure. Co konta administracyjnego Azure subskrypcji teraz można uzyskać dodatkową ochronę, włączając tę funkcję uwierzytelnianie wieloskładnikowe core. Aby administrator chce uzyskać dostęp do portal Azure, aby utworzyć Głosowa, witryny sieci web, zarządzanie magazynem, usług mobilnych lub innej usługi Azure można dodać uwierzytelnianie wieloskładnikowe do swojego konta administratora.
Uwierzytelnianie wieloskładnikowe Azure | Uwierzytelnianie wieloskładnikowe Azure oferuje najszerszym zestaw funkcji. <br><br>Zapewnia dodatkowe opcje konfiguracji za pomocą portalu zarządzania Azure, zaawansowane raportowanie i pomocy technicznej dla zakresu lokalnego i aplikacje w chmurze. Uwierzytelnianie wieloskładnikowe Azure można kupić jako autonomicznego licencji i jest powiązane Azure Active Directory Premium i pakiecie mobilności przedsiębiorstwa. <br><br>Można również go kupić na podstawie zużycie, tworząc dostawca uwierzytelniania wieloskładnikowego Azure Azure subskrypcji.
##<a name="feature-comparison-of-versions"></a>Porównanie funkcji wersji
Poniższa tabela poniżej zawiera listę funkcji, które są dostępne w różnych wersjach uwierzytelnianie wieloskładnikowe Azure.


Funkcja  | Uwierzytelnianie wieloskładnikowe usługi Office 365 (dołączone do usługi Office 365 w wersji produktu)|Uwierzytelnianie wieloskładnikowe dla administratorów Azure (dostępny w subskrypcji Azure) | Azure uwierzytelnianie wieloskładnikowe (zawarte w Azure AD Premium i Enterprise mobilności Suite)
------------- | :-------------: |:-------------: |:-------------: |
Administratorzy mogą chronić kont z MFA| * | * (Dostępne tylko w przypadku kont administratora Azure)|*
Jako drugi czynnik aplikacji dla urządzeń przenośnych|* | * | *
Rozmowa telefoniczna jako drugi czynnik|* | * | *
Wiadomości SMS jako drugi czynnik|* | * | *
Hasła aplikacji dla klientów, które nie obsługują MFA|* | * | *
Administrator kontrolę nad metod uwierzytelniania| *| *| *
Tryb numeru PIN| | | *
Alert oszustwa| | | *
Raporty MFA| | | *
Obejście jednorazowego| | | *
Niestandardowe powitania dla połączenia telefoniczne| | | *
Dostosowywanie identyfikator rozmówcy dla połączenia telefoniczne| | | *
Potwierdzenie zdarzenia| | | *
Adresy IP zaufanych| | | *
Zawieszania MFA urządzeń do zapamiętania (publiczna wersja Preview)| | | *
ZESTAW SDK MFA| | | *
MFA dla aplikacji lokalnej za pomocą serwera MFA| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Jak uzyskać uwierzytelnianie wieloskładnikowe Azure

Jeśli chcesz pełny zestaw funkcji oferowanych przez uwierzytelnianie wieloskładnikowe Azure zamiast tylko określone dla użytkowników usługi Office 365 i administratorów Azure istnieją go na kilka sposobów:

1.  Zakup licencji uwierzytelnianie wieloskładnikowe Azure i przypisanie ich do użytkowników.
2.  Zakup licencji, które zostały połączone w nich, takich jak Azure Active Directory Premium lub Enterprise Suite mobilności uwierzytelnianie wieloskładnikowe Azure i przypisywanie ich do użytkowników.
3.  Tworzenie dostawcy uwierzytelnianie wieloskładnikowe Azure w ramach subskrypcji usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można Załóż Azure subskrypcji wersji próbnej. Subskrypcje próbne muszą są konwertowane na zwykłą subskrypcje przed wygaśnięciem wersji próbnej.

Podczas korzystania z dostawcą uwierzytelnianie wieloskładnikowe Azure są dostępne dwa modele zastosowania, które są wystawiona przez subskrypcji Azure:


- **Dla każdego użytkownika**. Ogólnie dla przedsiębiorstw którego chcesz włączyć uwierzytelnianie wieloskładnikowe dla stałej liczby pracowników, którzy muszą regularnie uwierzytelniania.
- **Na uwierzytelniania**. Ogólnie dla przedsiębiorstw którego chcesz włączyć uwierzytelnianie wieloskładnikowe dla dużej grupy użytkowników zewnętrznych, którzy potrzebują rzadko uwierzytelniania.

Cennik szczegóły dla [ceny MFA Azure.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Wybierz na stanowisko lub oparte na zużycie modelu, który najlepiej dla Twojej organizacji.   Następnie, aby uzyskać wprowadzenie zobacz [Wprowadzenie do aplikacji](multi-factor-authentication-get-started.md)
