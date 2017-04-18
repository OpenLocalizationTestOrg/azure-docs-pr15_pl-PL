<properties
   pageTitle="Konfigurowanie bramy istniejącej aplikacji do obsługi wielu witryn w portalu Azure | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące konfigurowania bramy istniejących Azure aplikacji do obsługi wielu aplikacji sieci web na tej samej bramie Portal Azure."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurowanie bramy istniejącej aplikacji do obsługi wielu aplikacji sieci web

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-multisite-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Wiele hostingu witryny umożliwia wdrażanie więcej niż jedna aplikacja sieci web na tej samej bramie aplikacji. Zależy obecności nagłówek hosta w przychodzące żądanie HTTP, aby określić, które odbiornik może odbierać danych. Odbiornik następnie kieruje ruch do puli odpowiednie wewnętrznej bazy danych, zgodnie z konfiguracją w definicji reguł bramy. W aplikacjach sieci web SSL włączone bramy aplikacji zależy od rozszerzenia wskazanie nazwa serwera (SNI) wybierz poprawny detektor ruchu w sieci web. Powszechnie używane do obsługi wielu witryn jest załadowanie bilans żądań domeny innej witryny sieci web na innym serwerze wewnętrznej pul. Podobnie wiele poddomen tej samej domeny głównej może być również obsługiwane na tej samej bramie aplikacji.

## <a name="scenario"></a>Scenariusz

W poniższym przykładzie, brama aplikacji służy ruch contoso.com i fabrikam.com z dwóch pul wewnętrznej serwera: contoso puli server i fabrikam puli serwera. Podobne konfiguracji może służyć do hosta poddomen, takich jak app.contoso.com i blog.contoso.com.

![Scenariusz wielooddziałowości][multisite]

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym scenariuszu dodaje do istniejącego bramy aplikacji obsługę wielu witrynach. Aby wykonać w tym scenariuszu Scenariusz bramy istniejącej aplikacji musi być można konfigurować. Odwiedź stronę [Tworzenie bramy aplikacji za pomocą portalu](./application-gateway-create-gateway-portal.md) , aby dowiedzieć się, jak utworzyć bramę podstawowe zastosowanie w portalu.

## <a name="requirements"></a>Wymagania dotyczące

- **Wewnętrznej puli server:** Lista adresów IP serwerów wewnętrznej. Adresy IP wymienione albo powinna należeć do podsieci, wirtualną sieć lub powinny być publicznej IP-VIP. Można także FQDN.
- **Ustawienia puli serwera wewnętrznej:** Co Pula ma ustawienia, takie jak portu, Protocol (protokół) i koligacji systemem plików cookie. Te ustawienia są powiązane z puli i zostaną zastosowane do wszystkich serwerów w puli.
- **Zewnętrzną portu:** Ten port jest publicznej otwarcia na bramie aplikacji. Ruch trafienia tego portu, a następnie przekierowaniem do jednego z serwerów wewnętrznej.
- **Odbiornika:** Odbiornik ma port zewnętrzną, protokół (Http lub Https, wartości te są uwzględniania wielkości liter) oraz nazwę certyfikatu SSL (jeśli Konfigurowanie protokołu SSL offload). Dla bramy aplikacji obsługującej wiele witryn nazwa hosta i SNI wskaźniki są także dodawane.
- **Reguły:** Reguła powiąże odbiornika puli serwera wewnętrznej i określa, które puli serwera wewnętrznej dane powinny być kierowane do, gdy trafienia w szczególności odbiornika.
- **Certyfikatów:** Każdy odbiornik wymaga unikatowe certyfikatu, w tym przykładzie detektory 2 są tworzone dla wielu witryn. Dwa certyfikaty PFX i hasła dla nich muszą zostać utworzone.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Poniżej przedstawiono czynności, aby zaktualizować bramy aplikacji:

1. Tworzenie pul wewnętrznej dla każdej witryny.
2. Utwórz nowy odbiornik dla każdej witryny aplikacji bramy będzie obsługiwać.
3. Tworzenie reguły do zamapować każdego detektora odpowiednie wewnętrzną.

## <a name="create-back-end-pools-for-each-site"></a>Tworzenie puli wewnętrznej dla każdej witryny

Będzie tej bramy aplikacji puli wewnętrznej dla każdej witryny pomocy technicznej jest potrzebna, w tym przypadku 2 zostanie utworzony, jedną dla contoso11.com i jedną dla fabrikam11.com.

### <a name="step-1"></a>Krok 1

Przejdź do istniejącego bramy aplikacji w portalu Azure (https://portal.azure.com). Wybierz pozycję **Pule wewnętrznej bazy danych** , a następnie kliknij przycisk **Dodaj**

![Dodawanie pul wewnętrznej bazy danych][7]

### <a name="step-2"></a>Krok 2

Wypełnij informacje dotyczące wewnętrznej puli **pool1**, dodając adresów ip lub nazwy FQDN dla serwerów wewnętrznych i kliknij **przycisk OK**

![Ustawienia pool1 puli wewnętrznej bazy danych][8]

### <a name="step-3"></a>Krok 3

Na karta pul wewnętrznej bazy danych kliknij przycisk **Dodaj** , aby dodać dodatkowe puli wewnętrznej **pool2**, dodając adresów ip lub nazwy FQDN dla serwerów wewnętrznych i kliknij **przycisk OK**

![Ustawienia pool2 puli wewnętrznej bazy danych][9]

### <a name="create-listeners-for-each-back-end"></a>Tworzenie detektorów dla każdej wewnętrznej

Brama aplikacji zależy od nagłówków hosta HTTP 1.1 do obsługi więcej niż jednej witryny sieci Web, w tym samym publiczny adres IP i port. Podstawowe odbiornika utworzony w portalu nie zawiera tej właściwości.

### <a name="step-1"></a>Krok 1

Kliknij **detektory** istniejącej bramy aplikacji, a następnie kliknij pozycję **wiele witryn** , aby dodać pierwszy odbiornika.

![Karta Przegląd detektory][1]

### <a name="step-2"></a>Krok 2

Wypełnij informacje dotyczące odbiornika, w tym przykładzie zakończenie SSL są skonfigurowane tak, Utwórz nowy port serwera sieci Web. Przekazywanie certyfikatu PFX ma być używany dla zakończenia SSL. Różnica tylko na ta karta w porównaniu z karta standardowy odbiornik podstawowe jest to nazwa hosta.

![Karta właściwości odbiornika][2]

### <a name="step-3"></a>Krok 3

Kliknij **wiele witryn** i utwórz inny odbiornika zgodnie z opisem w poprzednim kroku dla drugiej witryny. Upewnij się zastosować inny certyfikat dla drugiego odbiornika. Różnica tylko na ta karta w porównaniu z karta standardowy odbiornik podstawowe jest to nazwa hosta. Wypełnij informacje dotyczące odbiornika, a następnie kliknij **przycisk OK**.

![Karta właściwości odbiornika][3]

> [AZURE.NOTE] Tworzenie detektorów w portalu Azure bramy aplikacji jest długotrwałych zadania, może upłynąć trochę czasu, aby utworzyć dwa detektory w tym scenariuszu. Po ukończeniu Pokaż detektory w portalu, jak pokazano na poniższej ilustracji.

![Omówienie odbiornika][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Tworzenie reguły do zamapować detektory pul wewnętrznej bazy danych

### <a name="step-1"></a>Krok 1

Przejdź do istniejącego bramy aplikacji w portalu Azure (https://portal.azure.com). Wybierz pozycję **reguły** , a następnie wybierz istniejące reguły domyślne **rule1** i kliknij przycisk **Edytuj**.

### <a name="step-2"></a>Krok 2

Wypełnij karta reguły, jak pokazano na poniższej ilustracji. Wybranie pierwszego odbiornika i pierwszy puli i kliknięcie przycisku **Zapisz** po zakończeniu.

![Edytowanie istniejącej reguły][6]

### <a name="step-3"></a>Krok 3

Kliknij przycisk **podstawowe reguły** , aby utworzyć regułę 2. Wypełnij formularz z drugim odbiornika i drugi puli wewnętrznej bazy danych, a następnie kliknij **przycisk OK** , aby zapisać.

![Dodawanie karta podstawowe reguły][10]

Na tym kończy się konfigurowanie bramy istniejącej aplikacji z obsługą wielu witryn portalu Azure.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak chronić witrynach sieci Web przy użyciu [Aplikacji bramy - zapory aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png