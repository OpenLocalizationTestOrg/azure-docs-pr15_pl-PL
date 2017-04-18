<properties
    pageTitle="Zabezpieczenia dla koncentratorów powiadomień"
    description="W tym temacie wyjaśniono zabezpieczeń dla koncentratorów Azure powiadomienie."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Zabezpieczenia

##<a name="overview"></a>Omówienie

W tym temacie opisano model zabezpieczeń koncentratorów powiadomienie Azure. Powiadomienie o koncentratory są podmiot Bus usługi, ich zaimplementować ten sam model zabezpieczeń, co Bus usługi. Aby uzyskać więcej informacji zobacz tematy [Uwierzytelniania Bus usługi](https://msdn.microsoft.com/library/azure/dn155925.aspx) .

##<a name="shared-access-signature-security-sas"></a>Udostępnienia podpisu zabezpieczeń (SA) 

Powiadomienie o koncentratory wykonuje schematu zabezpieczeń na poziomie jednostki nazywane skojarzeń zabezpieczeń (udostępnione podpis programu Access). Ten program umożliwia podmioty wiadomości zadeklarować do 12 reguły autoryzacji w ich opis, które prawa dotyczące tego obiektu.

Każda reguła zawiera nazwę, wartość klucza (tajnego) i zestaw praw, zgodnie z opisem w sekcji "Zabezpieczeń roszczeń." Podczas tworzenia Centrum powiadomienie, dwie reguły są tworzone automatycznie: jedna z słuchać prawa dostępu (korzysta z aplikacji klienckiej), a druga wszystkie uprawnienia (używane do wewnętrznej bazy danych aplikacji).

Podczas wykonywania zarządzania rejestracji w aplikacjach klienckich, jeśli informacje zostały wysłane za pośrednictwem powiadomienia nie jest wielkość liter (na przykład aktualizacji pogody), typowe sposobem uzyskania dostępu do koncentratora powiadomienie aby nadać wartość klucza reguły dostępu tylko do słuchać aplikację klienta i udzielić wartość klucza reguły pełny dostęp do wewnętrznej bazy danych aplikacji.

Osadzanie wartość klucza w aplikacjach klienckich ze Sklepu Windows nie jest zalecane. Sposobem uniknięcia osadzania wartość klucza ma pobranie ich z wewnętrznej bazy danych aplikacji podczas uruchamiania aplikacji klienta.

Aby dowiedzieć się, że klawisz z dostępem słuchać umożliwia aplikacji klienckiej zarejestrować żadnego znacznika ważne jest. Jeśli aplikacji należy ograniczyć rejestracji do określonych znaczników do określonych klientów (na przykład gdy znaczniki odpowiadają identyfikatorach użytkowników), do wewnętrznej bazy danych aplikacji należy wykonać rejestracji. Aby uzyskać więcej informacji zobacz Zarządzanie rejestracji. Należy zauważyć, że w ten sposób aplikację klienta nie będą mieć bezpośredni dostęp do koncentratorów powiadomienie.

##<a name="security-claims"></a>Bezpieczeństwa

Podobnie jak z innymi obiektami, powiadomień Centrum operacje są dozwolone dla trzy zastrzeżenia zabezpieczeń: odsłuchać, wysyłanie i zarządzanie nimi.

| Roszczeń | Opis | Dozwolone operacje |
|-------|-------------|--------------------|
| Odsłuchać | Tworzenie i aktualizowanie, przeczytaj i usuwanie pojedynczej rejestracji | Tworzenie i aktualizowanie rejestracji<br><br>Przeczytaj rejestracji<br><br>Czytanie wszystkich rejestracji dla uchwyt<br><br>Usuwanie rejestracji |
| Wyślij | Wysyłanie wiadomości do Centrum powiadomień | Wysyłanie wiadomości |
| Zarządzanie | CRUDs na koncentratory powiadomienia (w tym aktualizowanie poświadczeń obwodowym układzie Nerwowym i kluczy zabezpieczeń) i przeczytaj rejestracji na podstawie znaczników | Tworzenie/aktualizacja/przeczytane/usuwanie powiadomień koncentratory<br><br>Przeczytaj rejestracji według znaczników |


Powiadomienie koncentratory Zaakceptuj oświadczeniach udzielił tokeny kontrola dostępu Microsoft Azure i tokeny podpisu wygenerowany przez usługę kluczy udostępnionych skonfigurowano bezpośrednio w Centrum powiadomienie.