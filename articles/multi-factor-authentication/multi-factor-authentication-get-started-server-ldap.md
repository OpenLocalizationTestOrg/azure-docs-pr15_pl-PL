<properties 
    pageTitle="Serwer uwierzytelnianie wieloskładnikowe uwierzytelnianie LDAP i Azure"
    description="Jest to strona uwierzytelnianie wieloskładnikowe Azure, który pomoże wdrażanie serwera uwierzytelnianie wieloskładnikowe Azure i uwierzytelnianie LDAP."
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
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>Serwer uwierzytelnianie wieloskładnikowe uwierzytelnianie LDAP i Azure


Domyślnie serwer uwierzytelniania wieloskładnikowego Azure jest skonfigurowany do importowania lub zsynchronizować użytkowników z usługi Active Directory. Jednak można skonfigurować powiązać z różnych katalogach LDAP, takich jak katalogu ADAM lub określonego kontrolera domeny usługi Active Directory. Podczas konfigurowania, aby połączyć się z katalogiem za pośrednictwem protokołu LDAP, można skonfigurować serwer uwierzytelniania wieloskładnikowego Azure ma pełnić rolę serwera proxy LDAP przeprowadzić uwierzytelnienia. Umożliwia także stosowania powiązania LDAP jako element docelowy RADIUS przed uwierzytelniania użytkowników podczas korzystania z usług IIS uwierzytelniania lub uwierzytelniania podstawowego w portalu użytkownika uwierzytelnianie wieloskładnikowe Azure.

Jeśli używane uwierzytelnianie wieloskładnikowe Azure jako serwer proxy LDAP, serwer uwierzytelniania wieloskładnikowego Azure zostanie wstawiony między klientem LDAP (VPN urządzenia, aplikacji) i serwer katalogu LDAP aby można było dodać uwierzytelnianie wieloskładnikowe. W przypadku uwierzytelniania wieloskładnikowego Azure funkcji serwer uwierzytelniania wieloskładnikowego Azure musi być skonfigurowany do komunikowania się z serwerami klienta i katalogu LDAP. W tej konfiguracji serwera uwierzytelnianie wieloskładnikowe Azure odbiera LDAP z serwerów klienta i aplikacji i przekazuje je do serwer katalogu LDAP docelowej do sprawdzania poprawności poświadczeń podstawowego. Jeśli odpowiedź z katalogu LDAP znajduje się one podstawowego poświadczenia są prawidłowe, uwierzytelnianie wieloskładnikowe Azure wykonuje drugiego uwierzytelniania i wysyła odpowiedź do klienta LDAP. Uwierzytelnianie całego powiedzie się tylko wtedy, gdy zarówno uwierzytelniania na serwerze LDAP i uwierzytelnianie wieloskładnikowe powiodła się.





## <a name="ldap-authentication-configuration"></a>Konfiguracja uwierzytelniania protokołu LDAP


Aby skonfigurować uwierzytelnianie LDAP, należy zainstalować serwer uwierzytelniania wieloskładnikowego Azure w systemie Windows server. Wykonaj poniższe kroki:

1. W obrębie serwera uwierzytelnianie wieloskładnikowe Azure kliknij ikonę uwierzytelnianie LDAP w menu po lewej stronie.
2. Zaznacz pole wyboru Włącz uwierzytelnianie LDAP.![Uwierzytelnianie LDAP](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Na karcie klienci zmienić TCP port i SSL port usługi LDAP uwierzytelnianie wieloskładnikowe Azure należy powiązania z niestandardowych porty, aby odsłuchać dla żądań LDAP z klientami, które zostaną skonfigurowane.
4. Jeśli planujesz używać LDAPS z klienta do serwera uwierzytelnianie wieloskładnikowe Azure, certyfikat SSL musi być zainstalowany na serwerze z systemem na serwerze. Kliknij przycisk Przeglądaj. przycisk obok pola certyfikat SSL i wybierz zainstalowany certyfikat, który będzie używany do bezpiecznego połączenia.
5. Kliknij przycisk Dodaj. przycisk.
6. W oknie dialogowym Dodawanie klienta LDAP wprowadź adres IP urządzenia serwera lub aplikacji, która będzie służą do uwierzytelniania serwera i nazwy aplikacji (opcjonalnie). Nazwa aplikacji jest wyświetlana w raportach uwierzytelnianie wieloskładnikowe Azure i mogą być wyświetlane w wiadomości SMS lub aplikacji Mobile wiadomości uwierzytelniania.
7. Zaznacz pole Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego Azure wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i uwierzytelnianiem czynniki mutli. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będą wykluczone z uwierzytelniania czynniki mutli, nie zaznaczaj pola wyboru. Zobacz plik pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
8. Może być Powtórz kroki od 5 do 7, aby dodać dodatkowe klienci LDAP.
9. Po skonfigurowaniu uwierzytelnianie wieloskładnikowe Azure do odbierania uwierzytelnienia LDAP musi serwera proxy tych uwierzytelnienia do katalogu LDAP. W związku z tym wyświetla tylko karty docelowej na pojedyncze, wyszarzona opcję, aby użyć obiektu docelowego LDAP. Aby skonfigurować połączenie katalogu LDAP, kliknij ikonę integracji katalogów.
10. Na karcie Ustawienia wybierz Użyj określonych LDAP konfiguracji przycisk opcji.
11. Kliknij przycisk Edytuj. przycisk.
12. W oknie dialogowym Edytowanie konfiguracji LDAP Wypełnij pola informacje wymagane do połączenia katalogu LDAP. W poniższej tabeli znajdują się opisy pól. Uwaga: Te informacje również znajduje się w pliku pomocy serwera uwierzytelniania wieloskładnikowego Azure.![Integracja katalogu](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Testowanie połączenia LDAP, klikając przycisk Test.
14. Jeśli LDAP testowanie połączenia zakończyło się pomyślnie, kliknij przycisk OK.
15. Kliknij kartę filtry. Serwer jest wstępnie skonfigurowana, aby załadować kontenery, grupami zabezpieczeń i użytkowników z usługi Active Directory. Jeśli powiązanie do innego katalogu LDAP, prawdopodobnie będzie konieczne edytowanie filtrów wyświetlane. Kliknij łącze Pomoc, aby uzyskać więcej informacji o filtrach.
16. Kliknij kartę atrybuty. Serwer jest wstępnie skonfigurowana do zamapować atrybuty z usługi Active Directory.
17. Jeśli powiązanie do innego katalogu LDAP lub zmienić mapowania atrybutów wstępnie skonfigurowane, kliknij przycisk Edytuj. przycisk.
18. W oknie dialogowym Edytowanie atrybutów zmodyfikować mapowania atrybutów katalogu LDAP. Nazwy atrybutów można wpisane lub przez kliknięcie przycisku... przycisk obok każdego pola.
19. Kliknij łącze Pomoc, aby uzyskać więcej informacji na temat atrybutów.
20. Kliknij przycisk OK.
21. Kliknij ikonę Ustawienia firmy, a następnie wybierz kartę rozpoznawanie nazwy użytkownika.
22. Jeśli połączenie z usługą Active Directory z serwera domeny, należy pozostawić identyfikatory zabezpieczeń Użyj systemu Windows (identyfikatorów zabezpieczeń) dla pasujących zaznaczony przycisk radiowy użytkowników. W przeciwnym razie wybierz atrybut Unikatowy identyfikator LDAP Użyj odpowiadającego mu przycisku radiowego użytkowników. Po zaznaczeniu serwer uwierzytelniania wieloskładnikowego Azure spróbuje rozpoznawać poszczególnych unikatowym identyfikatorem w katalogu LDAP. Wyszukiwanie LDAP zostaną wykonane na Nazwa użytkownika atrybuty zdefiniowane w integracji katalogów -> karta atrybuty. Jeśli użytkownik uwierzytelnia, nazwa użytkownika będzie można rozpoznać unikatowym identyfikatorem w katalogu LDAP i unikatowy identyfikator będzie używana dla pasujących użytkownika w pliku danych uwierzytelnianie wieloskładnikowe Azure. Dzięki temu w porównaniach bez uwzględniania wielkości liter, jako nazwy użytkownika oraz jak długie i krótkie format to wykonuje konfiguracji serwera uwierzytelnianie wieloskładnikowe Azure. Serwer oczekuje się teraz na skonfigurowane porty na żądania dostępu LDAP skonfigurowany klientów i Ustaw serwer proxy te żądania do katalogu LDAP uwierzytelniania.


## <a name="ldap-client-configuration"></a>Konfiguracja klienta LDAP

Aby skonfigurować klienta LDAP, użyj wytycznych:

- Konfigurowanie urządzenia serwera lub aplikacji do uwierzytelniania za pomocą protokołu LDAP na serwerze uwierzytelnianie wieloskładnikowe Azure, tak jakby była ona katalogu LDAP. Należy używać tych samych ustawień, które zwykle służących do połączenia bezpośrednio z katalogu LDAP z wyjątkiem nazwa serwera lub adres IP, który będzie serwera uwierzytelnianie wieloskładnikowe Azure.
- Limit czasu LDAP do 30 – 60 sekund tak skonfigurować program jest czas do sprawdzania poprawności poświadczeń użytkowników z katalogu LDAP, uwierzytelniania drugiego czynniki, odbieranie swoją odpowiedź, a następnie odpowiedzieć na żądania dostępu LDAP.
- Jeśli używasz LDAPS, na urządzeniu lub na serwerze wykonywania kwerend LDAP musi ufać certyfikat SSL zainstalowane na serwerze uwierzytelnianie wieloskładnikowe Azure.
