<properties
   pageTitle="Tworzenie reguły opartej na ścieżkę dla bramy aplikacji za pomocą portalu | Microsoft Azure"
   description="Dowiedz się, jak tworzyć reguły oparte na ścieżkę dla bramy aplikacji za pomocą portalu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Tworzenie reguły opartej na ścieżkę dla bramy aplikacji za pomocą portalu

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-url-route-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-url-route-arm-ps.md)

Routingu opartego na protokole ścieżkę adresu URL umożliwia kojarzenie trasy w oparciu o ścieżki adresu URL żądania Http. Sprawdza, czy istnieje trasa do puli wewnętrznej skonfigurowane dla list adres URL w polu Brama aplikacji, a wysyłać ruch sieciowy do zdefiniowanej puli wewnętrznej. Powszechnie używane dla routingu opartego na adres URL jest załadowanie żądań saldo różne typy zawartości na innym serwerze wewnętrznej pul.

Adresy URL routingu wprowadza nowy typ reguły do bramy aplikacji. Brama aplikacji występują dwa typy reguł: reguły podstawowe i ścieżkę. Typ reguły podstawowe oferuje usługę okrężny puli wewnętrznej podczas reguły oparte na ścieżkę, oprócz rozkładu okrężnego, uwzględnia również ścieżka wzorzec adresu URL żądania podczas wybierania puli wewnętrznej bazy danych.

## <a name="scenario"></a>Scenariusz

Następującym scenariuszu przechodzi przez utworzenie reguły oparte na ścieżkę w istniejącej bramy aplikacji.
Scenariusz zakłada już wykonane kroki opisane tworzenie [Bramy aplikacji](application-gateway-create-gateway-portal.md).

![trasa adresu URL][scenario]

## <a name="createrule"></a>Tworzenie reguły opartej na ścieżki

Reguły oparte na ścieżkę wymaga własnej odbiornika, przed tworzenia reguły należy upewnić się masz detektor dostępne korzystać.

### <a name="step-1"></a>Krok 1

Przejdź do http://portal.azure.com i wybierz istniejący bramy aplikacji. Kliknij pozycję **reguły**

![Omówienie aplikacji bramy][1]

### <a name="step-2"></a>Krok 2

Kliknij przycisk **oparte na ścieżkę** Dodawanie nowej reguły oparte na ścieżkę.

### <a name="step-3"></a>Krok 3

Karta **reguły oparte na ścieżkę Dodaj** występują dwie sekcje. Pierwsza sekcja jest miejsce, w którym określone odbiornika, nazwy reguły i domyślne ustawienia ścieżki. Domyślne ustawienia ścieżki są marszrut, które nie są objęte niestandardowe trasę oparte na ścieżkę. Druga sekcja karta **reguły oparte na ścieżkę Dodaj** to miejsce, w którym definiowania reguły oparte na ścieżkę, się.

**Ustawienia podstawowe**

- **Nazwa** - przyjazna nazwa reguły, która jest dostępna w portalu.
- **Odbiornik** — jest to odbiornika używaną dla reguły.
- **Domyślna pula wewnętrznej bazy danych** — to ustawienie jest to ustawienie, która definiuje wewnętrzną może być używany dla reguły domyślnej
- **Ustawienia domyślne HTTP** — to ustawienie jest to ustawienie, która definiuje ustawienia HTTP może być używany dla reguły domyślnej.

**Reguły oparte na ścieżki**

- **Nazwa** - przyjazna nazwa reguły oparte na ścieżkę.
- **Ścieżki** — to ustawienie określa ścieżkę, które będą wyglądały dla podczas przesyłania ruchu
- **Pula wewnętrznej bazy danych** — to ustawienie jest to ustawienie, definiujące wewnętrznej może być używany dla reguły
- **Ustawienie HTTP** — to ustawienie jest to ustawienie, który definiuje ustawienia protokołu HTTP ma być używany dla reguły.

>[AZURE.IMPORTANT] Ścieżki: Lista wzorców ścieżka zgodnie z. Każdy musi rozpoczynać się- i umieszczanie tylko "\*" jest dozwolona znajduje się na końcu. Przykłady prawidłowych są /xyz /xyz* lub /xyz/*.  

![Dodawanie reguły oparte na ścieżkę karta z informacjami o wypełniania][2]

Dodawanie reguły oparte na ścieżkę do istniejącego bramy aplikacji jest łatwy za pośrednictwem portalu. Po utworzeniu reguły oparte na ścieżka może być edytowany łatwe dodawanie dodatkowych reguł. 

![Dodawanie dodatkowe reguły ścieżki][3]

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak skonfigurować odciążanie protokołu SSL Azure aplikacji bramy zobacz [Konfigurowanie Offload SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png