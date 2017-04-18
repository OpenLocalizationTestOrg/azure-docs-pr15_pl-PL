<properties 
    pageTitle="Zdalny pulpit bramy i serwer uwierzytelniania wieloskładnikowego Azure za pomocą RADIUS"
    description="Jest to strona uwierzytelnianie wieloskładnikowe Azure, który pomoże wdrażanie pulpitu zdalnego (RD) bramy i serwer uwierzytelniania wieloskładnikowego Azure za pomocą RADIUS."
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

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Zdalny pulpit bramy i serwer uwierzytelniania wieloskładnikowego Azure za pomocą RADIUS

W większości przypadków bramy usług pulpitu zdalnego używa lokalnych zasad Sieciowych do uwierzytelniania użytkowników. Tym dokumencie opisano sposób do kierowania żądania RADIUS się z bramy usług pulpitu zdalnego (za pośrednictwem lokalnych zasad Sieciowych) na serwerze uwierzytelnianie wieloskładnikowe.

Serwer uwierzytelniania wieloskładnikowego powinna zostać zainstalowana na serwerze osobnych, który zostanie następnie serwera proxy żądanie RADIUS powrót do zasad Sieciowych na serwerze bramy usług pulpitu zdalnego. Po zasad Sieciowych sprawdza nazwy użytkownika i hasła, zwróci odpowiedź do serwera uwierzytelnianie wieloskładnikowe, który wykonuje współczynnik drugiego uwierzytelniania cmd. wyniku bramy.





## <a name="configure-the-rd-gateway"></a>Konfigurowanie bramy usług pulpitu zdalnego

Brama usług pulpitu zdalnego musi być skonfigurowany do wysyłania uwierzytelniania RADIUS do serwera uwierzytelnianie wieloskładnikowe Azure. Po zainstalowaniu bramy usług pulpitu zdalnego skonfigurowany i czy działa, przejdź do właściwości bramy usług pulpitu zdalnego. Przejdź na kartę magazyn zasad RD i zmień ją na wartość Użyj serwera centralnego sieciowych zamiast lokalnego serwera sieciowych. Dodawanie jednego lub kilku serwerów uwierzytelnianie wieloskładnikowe Azure jako serwery RADIUS i określić wspólne hasło dla każdego serwera.





## <a name="configure-nps"></a>Konfigurowanie zasad Sieciowych

Brama RD używa zasad Sieciowych, aby wysłać żądanie RADIUS do uwierzytelnianie wieloskładnikowe Azure. Aby zapobiec bramy usług pulpitu zdalnego z Przekroczono limit czasu przed ukończeniem uwierzytelnianie wieloskładnikowe należy zmienić przekroczenia limitu czasu. Poniższa procedura umożliwia skonfigurowanie zasad Sieciowych.

1. Na serwerze zasad Sieciowych rozwiń menu klientów RADIUS i serwera w kolumnie po lewej stronie i kliknij pozycję grupy serwerów zdalnych RADIUS. Przejdź do właściwości grupy serwerów TS bramy. Edytuj serwery RADIUS wyświetlane i przejdź do karty usługi równoważenia obciążenia. Zmienianie "inicjały liczbę sekund bez odpowiedzi, zanim żądanie zostanie uznane za" i "Liczbę sekund między żądaniami, gdy serwer jest identyfikowany jako niedostępny" na 30 – 60 sekund. Kliknij kartę uwierzytelniania i konto i upewnij się, że porty RADIUS określone zgodne portów, których serwer uwierzytelniania wieloskładnikowego będzie nasłuchują na.
2. Zasad Sieciowych musi być również skonfigurowany do odbierania uwierzytelnienia RADIUS z serwera uwierzytelnianie wieloskładnikowe Azure. Kliknij klientów RADIUS w menu po lewej stronie. Dodaj serwer uwierzytelnianie wieloskładnikowe Azure jako klienta RADIUS. Wybierz przyjazną nazwę, a następnie określić wspólne hasło.
3. Rozwiń sekcję zasady w lewym okienku nawigacji i kliknij pozycję zasady żądań połączeń. Powinien zawierać do zasady żądań połączeń o nazwie TS bramy autoryzacji zasad, które zostały utworzone podczas konfigurowania bramy usług pulpitu zdalnego. Tych zasad przesyłania dalej żądań RADIUS na serwerze uwierzytelnianie wieloskładnikowe.
4. Skopiuj te zasady, aby utworzyć nową. W nowych zasad Dodaj warunek, który odpowiada przyjazna nazwa klienta z przyjazną nazwę zestawu w kroku 2 powyżej klienta RADIUS serwera uwierzytelnianie wieloskładnikowe Azure. Zmienianie dostawcy uwierzytelniania na komputerze lokalnym. Tych zasad gwarantuje, że po otrzymaniu żądania RADIUS z serwera uwierzytelnianie wieloskładnikowe Azure uwierzytelnianie odbywa się lokalnie, zamiast wysyłania żądania RADIUS do serwera uwierzytelniania wieloskładnikowego Azure, która doprowadziłaby warunku pętli. Aby zapobiec warunek pętli, należy zamówić nowe zasady powyżej przesyłania dalej na serwerze uwierzytelnianie wieloskładnikowe zasad oryginalnych.

## <a name="configure-azure-multi-factor-authentication"></a>Konfigurowanie uwierzytelniania wieloskładnikowego Azure


--------------------------------------------------------------------------------



Serwer uwierzytelnianie wieloskładnikowe Azure jest skonfigurowany jako serwer proxy RADIUS między bramy usług pulpitu zdalnego i zasad Sieciowych.  Powinna zostać zainstalowana na serwerze domeny, który jest używany na serwerze bramy usług pulpitu zdalnego. Poniższa procedura umożliwia skonfigurowanie serwera uwierzytelnianie wieloskładnikowe Azure.

1. Otwórz serwer uwierzytelniania wieloskładnikowego Azure i kliknij ikonę uwierzytelniania RADIUS. Zaznacz pole wyboru Włącz RADIUS uwierzytelniania.
2. Na karcie klienci upewnij się, że porty zgodne, co to jest skonfigurowany na serwerze zasad Sieciowych, a następnie kliknij pozycję Dodaj. przycisk. Dodaj adres IP serwera bramy usług pulpitu zdalnego, nazwa aplikacji (opcjonalnie) i ustaw wspólne hasło. Wspólne hasło muszą być taka sama na serwerze uwierzytelnianie wieloskładnikowe Azure i bramy usług pulpitu zdalnego.
3. Kliknij docelową kartę i wybierz przycisk radiowy serwery RADIUS.
4. Kliknij przycisk Dodaj. przycisk. Wprowadź adres IP, tajnego i portów serwera zasad Sieciowych. Jeśli przy użyciu centralnej zasad Sieciowych, klienta RADIUS i docelowej RADIUS będą takie same. Wspólne hasło musi odpowiadać jeden ustawienia w sekcji klienta RADIUS serwera zasad Sieciowych.

![Uwierzytelnianie RADIUS](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
