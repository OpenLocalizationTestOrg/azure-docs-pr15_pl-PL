<properties
   pageTitle="Konfigurowanie bramy aplikacji dla odciążania SSL za pomocą portalu | Microsoft Azure"
   description="Ta strona zawiera instrukcje, aby utworzyć bramy aplikacji za pomocą protokołu SSL offload za pomocą portalu"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurowanie bramy aplikacji dla odciążania SSL za pomocą portalu

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure Menedżera zasobów programu PowerShell](application-gateway-ssl-arm.md)
-[Azure klasycznego programu PowerShell](application-gateway-ssl.md)

Aby zakończyć sesję Secure Sockets Layer (SSL) u bramy, aby uniknąć kosztów zadań odszyfrowywania SSL działania na farmie sieci web można skonfigurować Azure bramy aplikacji. Odciążanie SSL upraszcza zarządzanie aplikacji sieci web i konfiguracji serwera frontonu.

## <a name="scenario"></a>Scenariusz

Następującym scenariuszu przechodzi przez skonfigurowanie SSL offload na bramy istniejącej aplikacji. Scenariusz zakłada już wykonane kroki opisane tworzenie [Bramy aplikacji](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby skonfigurować odciążanie protokołu SSL z bramy aplikacji, wymagany jest certyfikat. Ten certyfikat jest ładowana na bramie aplikacji i używane do szyfrowania i odszyfrowywania ruch wysyłane za pośrednictwem protokołu SSL. Certyfikat musi mieć format wymiana informacji osobistych (pfx). W tym formacie plików pozwala na klucz prywatny do wyeksportowania, które są wymagane przez bramę aplikacji do wykonania do szyfrowania i odszyfrowywania ruchu.

## <a name="add-an-https-listener"></a>Dodać detektor HTTPS

Odbiornika HTTP wyszukuje ruch na podstawie konfiguracji i ułatwia skierować ruch do pul wewnętrznej bazy danych.

### <a name="step-1"></a>Krok 1

Przejdź do portalu Azure i wybierz pozycję bramy istniejącej aplikacji

### <a name="step-2"></a>Krok 2

Kliknij detektory i kliknij przycisk Dodaj, aby dodać detektor.

![Karta Przegląd bramy aplikacji][1]

### <a name="step-3"></a>Krok 3

Wypełnij wymagane informacje dotyczące odbiornika i przekazywanie certyfikatu PFX, po zakończeniu kliknij przycisk OK.

**Nazwa** - wartość ta jest przyjazną nazwę, która nasłuchującego.

**Konfiguracja IP serwera sieci Web** — ta wartość jest konfiguracji IP serwera sieci Web, która jest używana odbiornika.

**Port serwera sieci Web (nazwa/Port)** — przyjazną nazwę port używany na końcu przedniego bramy aplikacji i rzeczywisty port używany.

**Protokół** - przełącznika ustalenie, jeśli https lub http jest używany do zewnętrznej.

**Certyfikat (nazwy i hasła)** — Jeśli SSL odciążania jest używana, certyfikat PFX jest wymagane dla tego ustawienia i przyjazną nazwę i hasło są wymagane.

![Dodawanie karta odbiornika][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Utwórz regułę i skojarzenia ich odbiornika

Utworzono odbiornik. Nadszedł czas, aby utworzyć regułę do obsługi ruchu z odbiornika.

### <a name="step-1"></a>Krok 1

Kliknij pozycję **reguły** bramy aplikacji, a następnie kliknij przycisk Dodaj.

![Karta reguły bramy aplikacji][3]

### <a name="step-2"></a>Krok 2

Na karta **Dodaj regułę podstawowe** wpisz przyjazną nazwę dla reguły i wybierz odbiornika utworzony w poprzednim kroku. Wybierz pozycję puli odpowiednie wewnętrznej bazy danych i ustawienia protokołu http, a następnie kliknij **przycisk OK**

![Protokół HTTPS w oknie Ustawienia][4]

Ustawienia są teraz zapisywane do bramy aplikacji. Proces zapisywania dla tych ustawień może trochę potrwać, zanim staną się dostępne do wyświetlania za pośrednictwem portalu lub przy użyciu programu PowerShell. Po zapisaniu bramy aplikacji obsługuje szyfrowanie i odszyfrowywanie ruchu. Cały ruch między brama aplikacji i serwery wewnętrznej bazy danych sieci web będą obsługiwane za pomocą protokołu http. Każde powiadomienie do klienta, gdy zainicjowana za pomocą protokołu https zostaną zwrócone do klienta szyfrowane.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak konfigurować sondy niestandardowych kondycji bramy aplikacji Azure, zobacz [Tworzenie sondy kondycji niestandardowych](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png