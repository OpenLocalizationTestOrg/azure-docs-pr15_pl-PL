<properties 
    pageTitle="Serwer uwierzytelnianie wieloskładnikowe uwierzytelniania RADIUS i Azure"
    description="Jest to strona uwierzytelnianie wieloskładnikowe Azure, który pomoże wdrażanie serwera uwierzytelnianie wieloskładnikowe Azure i uwierzytelniania RADIUS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>Serwer uwierzytelnianie wieloskładnikowe uwierzytelniania RADIUS i Azure

W sekcji uwierzytelnianie RADIUS umożliwia włączanie i konfigurowanie uwierzytelniania RADIUS na serwerze uwierzytelnianie wieloskładnikowe Azure. RADIUS to standardowy protokół do akceptowania żądań uwierzytelniania i przetwarzanie tych żądań. Serwer uwierzytelniania wieloskładnikowego Azure działa jako serwer RADIUS i dodaje się między klientem RADIUS (np. urządzenia VPN) i docelowego uwierzytelniania, który może być Active Directory (AD), katalogu LDAP lub innego serwera RADIUS, aby można było dodać uwierzytelnianie wieloskładnikowe Azure. W przypadku uwierzytelniania wieloskładnikowego Azure funkcji należy skonfigurować serwer uwierzytelniania wieloskładnikowego Azure, aby go można komunikować się z serwerami klienta i element docelowy uwierzytelniania. Serwer uwierzytelniania wieloskładnikowego Azure akceptuje żądania od klienta RADIUS, sprawdza poświadczenia element docelowy uwierzytelniania, dodaje uwierzytelnianie wieloskładnikowe Azure i wysyła odpowiedź do klienta RADIUS. Uwierzytelnianie całego powiedzie się tylko wtedy, gdy zarówno podstawowego uwierzytelnianie i uwierzytelnianie wieloskładnikowe Azure powiodła się.

>[AZURE.NOTE]
>Serwer MFA obsługuje tylko PAP (Protokół uwierzytelniania haseł) i MSCHAPv2 (protokół uwierzytelniania wyzwania uzgadniania firmy Microsoft) RADIUS protokoły działając jako serwer RADIUS.  Inne protokoły, takie jak EAP (extensible authentication protocol) można używać podczas serwer MFA działa jako serwer proxy RADIUS do innego serwera RADIUS, który obsługuje tego protokołu, takich jak Microsoft NPS.
></br>
>Podczas używania innych protokołów w tej konfiguracji, jednokierunkowa tokeny SMS i PRZYSIĘGĄ nie będzie działać, ponieważ serwer MFA nie będzie mógł zainicjować pomyślną odpowiedź wezwanie RADIUS przy użyciu protokołu.


![Uwierzytelnianie RADIUS](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>Konfiguracja uwierzytelniania RADIUS

Aby skonfigurować uwierzytelnianie RADIUS, należy zainstalować serwer uwierzytelniania wieloskładnikowego Azure w systemie Windows server. Jeśli masz środowiska usługi Active Directory, serwer powinny zostać połączone z domeny w sieci. Poniższa procedura umożliwia skonfigurowanie serwera uwierzytelnianie wieloskładnikowe Azure:

1. W obrębie serwera uwierzytelnianie wieloskładnikowe Azure kliknij ikonę uwierzytelniania RADIUS w menu po lewej stronie.
2. Zaznacz pole wyboru Włącz RADIUS uwierzytelniania.
3. Na karcie klienci zmienić porty uwierzytelniania i księgowe porty usługi RADIUS uwierzytelnianie wieloskładnikowe Azure należy powiązać niestandardowe porty Aby odsłuchać dla żądań RADIUS z klientami, które zostaną skonfigurowane.
4. Kliknij przycisk Dodaj. przycisk.
5. W oknie dialogowym Dodawanie klienta RADIUS wprowadź adres IP urządzenia i serwera, który będzie uwierzytelniania serwera uwierzytelnianie wieloskładnikowe Azure, nazwa aplikacji (opcjonalnie) i wspólne hasło. Wspólne hasło muszą być taka sama na serwerze uwierzytelnianie wieloskładnikowe Azure i urządzenia/serwer. Nazwa aplikacji jest wyświetlana w raportach uwierzytelnianie wieloskładnikowe Azure i mogą być wyświetlane w wiadomości SMS lub aplikacji Mobile wiadomości uwierzytelniania.
6. Zaznacz pole Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte uwierzytelnianie wieloskładnikowe. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z uwierzytelnianie wieloskładnikowe, nie zaznaczaj pola wyboru. Zobacz plik pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
7. Włącz alternatywnych PRZYSIĘGĄ token zaznacz pole wyboru, jeśli użytkownicy będą używać uwierzytelniania aplikacji dla urządzeń przenośnych uwierzytelnianie wieloskładnikowe Azure i chcesz używać haseł PRZYSIĘGĄ jako alternatywnych uwierzytelniania powiadomienie połączenia, SMS lub wypychanych telefonu w nowym oknie grupy.
8. Kliknij przycisk OK.
9. Może być Powtórz kroki od 4 do 8, aby dodać dodatkowe klientów RADIUS.
10. Kliknij docelową kartę.
11. Jeśli serwer uwierzytelnianie wieloskładnikowe Azure jest zainstalowany na serwerze domeny w środowisku usługi Active Directory, wybierz pozycję domeny systemu Windows.
12. Jeśli przed katalogu LDAP uwierzytelniania użytkowników, wybierz pozycję powiązania LDAP. Używając powiązania LDAP, musi kliknij ikonę integracja katalogów i edytować konfiguracji LDAP na karcie Ustawienia serwera można powiązać z katalogu. Instrukcje dotyczące konfigurowania LDAP można znaleźć w podręczniku konfiguracji serwera Proxy LDAP.
13. Jeśli na innym serwerze RADIUS uwierzytelniania użytkowników, wybierz pozycję serwery RADIUS.
14. Konfigurowanie serwera, że serwer będzie serwera proxy żądania RADIUS, klikając pozycję Dodaj... przycisk.
15. W oknie dialogowym Dodawanie serwera usługi RADIUS wprowadź adres IP serwera RADIUS i wspólne hasło. Wspólne hasło muszą być taka sama na serwerze serwer uwierzytelniania wieloskładnikowego Azure i RADIUS. Zmienianie port uwierzytelniania i księgowe, jeśli różne porty są używane przez serwer RADIUS.
16. Kliknij przycisk OK.
17. Serwer uwierzytelniania wieloskładnikowego Azure należy dodać jako klienta RADIUS na serwerze RADIUS, dzięki czemu będzie przetwarzać żądań dostępu wysłanych do niego z serwera uwierzytelnianie wieloskładnikowe Azure. Należy użyć tego samego wspólnego hasła jest skonfigurowana na serwerze uwierzytelnianie wieloskładnikowe Azure.
18. Może być Powtórz ten krok, aby dodać dodatkowe serwery RADIUS i skonfiguruj kolejność, w której serwer należy połączenie za pomocą przycisków Przenieś w górę i Przenieś w dół. Na tym kończy konfiguracji serwera uwierzytelnianie wieloskładnikowe Azure. Serwer oczekuje się teraz na skonfigurowane porty na żądania dostępu RADIUS skonfigurowanych klientów.   


## <a name="radius-client-configuration"></a>Konfiguracja klienta RADIUS

Aby skonfigurować klienta RADIUS, użyj wytycznych:

- Konfigurowanie urządzenia i serwer do uwierzytelniania za pośrednictwem RADIUS adres IP serwera uwierzytelnianie wieloskładnikowe Azure, które będą działać jako serwer RADIUS.
- Użyj tego samego wspólnego hasła, który został skonfigurowany powyżej.
- Przekroczono limit czasu RADIUS do 30 – 60 sekund tak skonfigurować program jest czas do sprawdzania poprawności poświadczeń użytkowników, przeprowadzać uwierzytelnianie wieloskładnikowe, odbieranie swoją odpowiedź, a następnie odpowiedzieć na żądania dostępu RADIUS.
