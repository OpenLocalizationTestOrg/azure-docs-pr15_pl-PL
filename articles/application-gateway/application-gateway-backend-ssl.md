<properties
   pageTitle="Włączanie zasad protokołu SSL i kompleksowego SSL na bramie aplikacji | Microsoft Azure"
   description="Ta strona zawiera omówienie bramy aplikacji SSL kompleksowego pomocy technicznej."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Włączanie zasad protokołu SSL i kompleksowego SSL na bramie aplikacji

## <a name="overview"></a>Omówienie

Brama aplikacji obsługuje zakończenia SSL na bramę, po którym ruch zwykle przepływał bez szyfrowania się z serwerami wewnętrznej bazy danych. Dzięki temu serwery web mogą być unburdened z góry kosztów Szyfrowanie/odszyfrowywanie. Jednak w przypadku niektórych klientów niezaszyfrowanym komunikacja z serwerami wewnętrznej bazy danych nie jest dopuszczalne opcję. Może to być spowodowane zabezpieczeń i spełnianie wymagań dotyczących lub aplikacja może je przyjąć bezpiecznego połączenia. Takie aplikacje bramy aplikacji obsługuje teraz kompleksowego szyfrowania SSL.

Zakończenia w celu SSL umożliwia bezpieczne przesyłanie poufnych danych do robienia nadal zaszyfrowane wewnętrznej bazy danych, korzystając z zalet funkcji równoważenia obciążenia 7 warstwy które bramy aplikacji przewiduje, takich jak koligacji plików cookie oparte na adres URL, rozsyłania routingu na podstawie witryn lub zdolności do dodania X-przesłanych dalej-* nagłówków.

Gdy skonfigurowano tryb komunikacji SSL kompleksowego, bramy aplikacji kończy sesji protokołu SSL użytkownika na bramy i odszyfrowuje ruch użytkownika. Następnie stosowane skonfigurowanych reguł zaznacz wystąpienia puli odpowiednie wewnętrznej bazy danych w celu rozsyłania ruchu. Następnie bramy aplikacji inicjuje nowe połączenie SSL na serwerze wewnętrznej bazy danych i ponownie szyfrowanie danych przy użyciu certyfikatu klucza publicznego serwera wewnętrznej bazy danych przed przekazaniem żądania do wewnętrznej bazy danych. End, aby zakończyć SSL jest włączona, ustawiając ustawienie protokołu w BackendHTTPSetting na Https, która następnie jest stosowana do puli wewnętrznej bazy danych. Każdy serwera wewnętrznej bazy danych w puli wewnętrznej bazy danych za pomocą protokołu SSL kompleksowego włączone musi być skonfigurowany za pomocą certyfikatu, aby umożliwić bezpieczna komunikacja.

![scenariusz kompleksowego ssl][1]

W tym przykładzie żądania przy użyciu TLS1.2 są kierowane do serwerów wewnętrznej bazy danych w Pool1 przy użyciu protokołu SSL kompleksowego.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End, aby zakończyć SSL i whitelisting certyfikatów

Brama aplikacji komunikuje się tylko z wystąpień znane wewnętrznej bazy danych, które mają whitelisted swój certyfikat bramy aplikacji. Aby włączyć whitelisting certyfikaty, możesz przekazać klucz publiczny certyfikaty serwera wewnętrznej bazy danych do aplikacji bramy (nie certyfikat główny). Następnie są dozwolone tylko połączenia z pomocą znanych i whitelisted. Pozostała pomocą powoduje błąd bramy. Są certyfikaty z podpisem własnym do celów testowych tylko i nie zalecane dla obciążenia produkcji. Takie certyfikaty musi być whitelisted z bramą aplikacji zgodnie z opisem w poprzednich krokach, zanim będzie można ich używać.

## <a name="application-gateway-ssl-policy"></a>Zasady SSL bramy aplikacji

Brama aplikacji obsługuje użytkownika można konfigurować SSL wymiany informacji zasady, które użytkownicy większą kontrolę nad połączenia SSL na bramy aplikacji.

1. SSL 2.0 i 3.0 domyślnie wyłączone dla wszystkich bram aplikacji. Nie są one można konfigurować w ogóle.
2. Zapewnia definicji zasad protokołu SSL opcja wyłączenia dowolną z następujących protokołów 3 - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Jeśli nie SSL zdefiniowano zasad wszystkie trzy (TLSv1\_0, TLSv1\_1, TLSv1_2) są włączone.

## <a name="next-steps"></a>Następne kroki

Po informacje na temat zakończenia w celu SSL oraz zasady SSL, przejdź do pozycji [Włącz kompleksowego SSL na bramie aplikacji](application-gateway-end-to-end-ssl-powershell.md) na tworzenie bramy aplikacji możliwość wysyłać ruch do pomocą w zaszyfrowanej.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png