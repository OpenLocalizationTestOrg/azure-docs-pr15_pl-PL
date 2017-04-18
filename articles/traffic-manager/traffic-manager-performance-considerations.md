<properties
    pageTitle="Zagadnienia dotyczące wydajności Menedżer ruchu Azure | Microsoft Azure"
    description="Opis wydajności na temat testowania wydajności witryny sieci Web, korzystając z Menedżera ruchu i Menedżer ruchu"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Zagadnienia dotyczące wydajności dla Menedżera ruchu

Ta strona omówiono zagadnienia dotyczące wydajności za pomocą Menedżera ruch. Scenariusz:

Masz wystąpienia witryny sieci Web w regionach WestUS i EastAsia. Jednego wystąpienia kończy się niepowodzeniem sprawdzanie kondycji sondy Menedżer ruchu. Ruch aplikacji są kierowane do prawidłowy region. Oczekuje tej pracy awaryjnej, ale wydajność może być problem związany z opóźnienie ruchu do obszaru odległości teraz podróż.

## <a name="how-traffic-manager-works"></a>Jak działa Menedżer ruchu

Tylko wpływ na wydajność zawierających przez Menedżer ruchu w witrynie sieci Web jest początkowej wyszukiwanie DNS. Żądanie DNS dla nazwy profilu Menedżera ruch jest obsługiwany przez serwer główny DNS firmy Microsoft, który obsługuje strefę trafficmanager.net. Menedżer ruchu wypełnia i regularnie aktualizuje, główny serwery DNS firmy Microsoft, na podstawie zasad Menedżer ruchu i wyniki sondy. Dzięki temu nawet podczas początkowej wyszukiwania serwera DNS Brak kwerendy DNS są wysyłane do Menedżera ruch.

Menedżer ruchu składa się z kilku części: serwerów, usługi interfejsu API Warstwa przechowywania i punktu końcowego monitorowanie usługi nazw DNS. Jeśli składnik usługi Menedżer ruchu nie powiedzie się, jest nie wpływa na nazwę DNS skojarzone z profilu Menedżer ruchu. Rekordy serwerów DNS firmy Microsoft pozostaną niezmienione. Jednak nie nawiązały monitorowania punktu końcowego i aktualizowanie DNS. W związku z tym Menedżer ruchu nie będzie mógł zaktualizować system DNS, aby wskazywały witryny pracy awaryjnej po witrynie podstawowego awarii.

Rozpoznawanie nazw DNS jest szybkie i wyniki w pamięci podręcznej. Szybkość początkowej wyszukiwanie DNS zależy od używanego przez klienta do rozpoznawania nazw serwerów DNS. Zazwyczaj klient może wykonać wyszukiwanie DNS w ms ~ 50. Wyniki wyszukiwania w pamięci podręcznej na czas trwania DNS Time-to-live (TTL). Domyślny czas TTL dla Menedżera ruch to 300 sekund.

Ruch nie przepływu za pomocą Menedżera ruch. Po zakończeniu wyszukiwania DNS klienta ma adres IP w przypadku wystąpienia witryny sieci web. Klient nawiązuje połączenie bezpośrednio do tego adresu i nie przechodzić za pomocą Menedżera ruch. Zasady ruch Menedżera wybrany nie ma wpływu na wydajność serwera DNS. Jednak wydajności routingu metody może mieć negatywny wpływ na obsługi aplikacji. Na przykład jeśli zasad przekierowuje ruchu z Ameryki Północnej do wystąpienia obsługiwany w Azji, opóźnień sieci dla tych sesji może być problem z wydajnością.

## <a name="measuring-traffic-manager-performance"></a>Pomiaru wydajności Menedżer ruchu

Istnieje kilka witryn sieci Web, używanego do zrozumienia wydajność i działanie profilu Menedżer ruchu. Wiele z tych witryn jest bezpłatna, ale może być ograniczenia. Niektóre witryny oferuje ulepszone monitorowania i raportowania za opłatą.

Narzędzia w tych witrynach miary DNS opóźnienia i wyświetlanie adresów IP rozpoznawania dla lokalizacji klienta na całym świecie. Większość z tych narzędzi nie Buforuj wyniki DNS. Dlatego narzędzia Pokaż pełny wyszukiwanie DNS każdym uruchomieniu test. Podczas testowania z własnych klienta tylko występują pełną wydajność wyszukiwania DNS raz podczas trwania TTL.

## <a name="sample-tools-to-measure-dns-performance"></a>Przykładowe narzędzia do pomiaru wydajności DNS

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS oferuje wiele narzędzi wydajności. Narzędzie do porównywania DNS możesz wyświetlić, jak długo trwa rozwiązywać nazwę DNS i jak który różni się od innych dostawców usług DNS.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Jest jednym z narzędzi najłatwiejszym WebSitePulse. Wprowadź adres URL, aby wyświetlić czas rozwiązania DNS, pierwszy bajt ostatni bajt i innych statystyk wydajności. Możesz wybrać z trzech różnych wzorcach lokalizacji. W tym przykładzie pojawi się, że przy pierwszym wykonaniu wskazuje, że wyszukiwanie DNS ma 0.204 s.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Ponieważ wyniki w pamięci podręcznej, drugi test dla samego punktu końcowego Menedżer ruchu wyszukiwanie DNS trwa 0,002 s.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [Monitorowanie aplikacji syntetycznych urzędu certyfikacji](https://asm.ca.com/en/checkit.php)

    Wcześniej znana pod nazwą narzędzie Watchmouse wyboru witryny sieci Web, ta witryna wyświetlanych czasu rozwiązania DNS z wielu regionach geograficznych jednocześnie. Wprowadź adres URL czasu rozwiązania DNS, czas połączenia i szybkości z kilku lokalizacji geograficznej. Ten test umożliwia wyświetlanie, które hostowana usługa jest zwracany dla różnych miejsc na całym świecie.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    To narzędzie oferuje statystyki dotyczące wydajności dla każdego elementu strony sieci web. Na karcie analiza strony pokazuje procent czasu poświęconego na wyszukiwanie DNS.

- [Co to jest DNS?](http://www.whatsmydns.net/)

    Ta witryna nie wyszukiwanie DNS z 20 różnych lokalizacji i wyniki są wyświetlane na mapie.

- [Wyświetlić interfejs sieci Web](http://www.digwebinterface.com)

    Ta witryna zawiera szczegółowe informacje DNS, łącznie z rekordów CNAME i rekordów. Upewnij się, należy sprawdzić "Koloruj wynik" oraz "Statystykę" w obszarze Opcje, a w obszarze serwery nazw, zaznacz "Wszystkie".

## <a name="next-steps"></a>Następne kroki

[Informacje o metody routingu ruchu Menedżer ruchu](traffic-manager-routing-methods.md)

[Przeprowadź test ustawień Menedżer ruchu](traffic-manager-testing-settings.md)

[Operacje na Menedżer ruchu (REST interfejsu API informacje)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure poleceń cmdlet Menedżera ruchu](http://go.microsoft.com/fwlink/p/?LinkId=400769)
