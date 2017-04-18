<properties
    pageTitle="Testowanie ustawień Menedżer ruchu | Microsoft Azure"
    description="W tym artykule pomoże Ci Przetestuj ustawienia Menedżer ruchu"
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

# <a name="test-your-traffic-manager-settings"></a>Przeprowadź test ustawień Menedżer ruchu

Aby sprawdzić ustawienia Menedżer ruchu, należy następująco skonfigurować wielu klientów w różnych lokalizacjach, z których można uruchamiać testy. Następnie zabrać ze sobą punkty końcowe w swoim profilu Menedżer ruch w dół pojedynczo.

* Ustaw wartość DNS TTL niskim tak, aby zmiany propagowanie szybko (na przykład 30 sekund).
* Znane adresy IP usług w chmurze Azure i witryny sieci Web w profil, który testujesz.
* Za pomocą narzędzia umożliwiające rozpoznawanie nazw DNS na adres IP i wyświetlania tego adresu.

Są sprawdzanie, czy nazwy DNS rozpoznawać adresów IP punkty końcowe w swoim profilu. Nazwy należy rozwiązać w sposób zgodny z metody routingu ruchu zdefiniowanej w profilu Menedżer ruchu. Za pomocą narzędzi, takich jak **nslookup** lub **Drąż** do rozpoznawania nazw DNS.

Poniższe przykłady ułatwiają Testowanie profilu Menedżer ruchu.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Zaznacz profil Menedżera ruch przy użyciu narzędzia nslookup i ipconfig w systemie Windows

1. Otwórz polecenia lub monit programu Windows PowerShell jako administrator.
2. Typ `ipconfig /flushdns` opróżnić pamięć podręczną rozpoznawania nazw DNS.
3. Typ `nslookup <your Traffic Manager domain name>`. Na przykład następujące polecenie sprawdza nazwy domeny z prefiksem *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Typowe wynik zawiera następujące informacje:

    * Nazwa DNS i adres IP serwera DNS wykorzystywane rozwiązania tej nazwy domeny Menedżer ruchu.
    * Nazwa domeny Menedżer ruchu została wpisana w wierszu polecenia po "nslookup" i adres IP, do którego jest rozpoznawana jako Menedżer ruchu domeny. Drugi adres IP jest to ważne, aby sprawdzić. Powinien być zgodny publiczny adres IP (VIP) wirtualnej dla jednej z usług w chmurze lub witryny sieci Web w profilu Menedżer ruchu, które testujesz.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Jak przetestować metody routingu ruchu pracy awaryjnej

1. Pozostaw wszystkie punkty końcowe w górę.
2. Za pomocą jednego klienta, żądanie rozpoznawania nazw DNS dla nazwy domeny firmy przy użyciu narzędzia nslookup lub podobne narzędzia.
3. Upewnij się, czy rozwiązany adres IP pasuje podstawowego punktu końcowego.
4. Przesuń w dół punkt końcowy podstawowego lub usunąć plik monitorowania, tak aby Menedżer ruch uznaje, że aplikacja jest wyłączony.
5. Poczekaj, aż DNS czasu to-Live (TTL) profilu Menedżer ruchu oraz dodatkowe dwie minuty. Na przykład jeśli TTL usługi DNS jest 300 sekund (5 minut), należy poczekać 7 minut.
6. Opróżnianie DNS klienta pamięci podręcznej i żądania DNS rozdzielczość przy użyciu narzędzia nslookup. W systemie Windows można opróżnić pamięć podręczną DNS przy użyciu polecenia ipconfig/flushdns.
7. Upewnij się, że rozpoznany adres IP odpowiada punkt końcowy pomocniczej.
8. Powtórz proces wyłączania każdego punktu końcowego z kolei. Upewnij się, że DNS zwraca adres IP następnego punktu końcowego na liście. Gdy wszystkie punkty końcowe w dół, należy ponownie uzyskać adres IP podstawowego punktu końcowego.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Jak przetestować metody routingu ruchu ważona

1. Pozostaw wszystkie punkty końcowe w górę.
2. Za pomocą jednego klienta, żądanie rozpoznawania nazw DNS dla nazwy domeny firmy przy użyciu narzędzia nslookup lub podobne narzędzia.
3. Upewnij się, że adres IP rozwiązany zgodny z jednym z punktów końcowych.
4. Opróżnij pamięć podręczną DNS klienta, a następnie powtórz kroki 2 i 3 dla każdego punktu końcowego. Powinien zostać wyświetlony różnymi adresami IP zwracane dla każdego z punktów końcowych.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Jak przetestować metody routingu ruchu wydajności

Aby przetestować skuteczne metody routingu ruchu wydajności, musisz mieć klientów znajdujące się w różnych częściach świata. Klientów można tworzyć w różnych Azure regionów, których można używać do testowania usług. Jeśli masz globalnej sieci, można zdalnie Zaloguj się do klientów w innych częściach świata i uruchom testy z niego.

Również są bezpłatne wyszukiwanie DNS oparte na sieci web i wyświetlić dostępne usługi. Niektóre z tych narzędzi pozwalają sprawdzać rozpoznawania nazw DNS w różnych lokalizacjach na całym świecie. Do wyszukania "Wyszukiwanie DNS" przykłady. Usługi innych firm, takie jak Gomez lub głównych może służyć do potwierdzenia profili są rozpowszechniania ruch zgodnie z oczekiwaniami.

## <a name="next-steps"></a>Następne kroki

* [Informacje o metody routingu ruchu Menedżer ruchu](traffic-manager-routing-methods.md)
* [Zagadnienia dotyczące wydajności Menedżer ruchu](traffic-manager-performance-considerations.md)
* [Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)




