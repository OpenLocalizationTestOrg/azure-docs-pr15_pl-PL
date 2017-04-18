Systemu nazw domen (DNS) jest używane do znajdowania elementów w Internecie. Na przykład podczas wprowadzania adresu w przeglądarce lub kliknij łącze na stronie sieci web używa DNS, można tłumaczyć domeny na adres IP. Adres IP jest sort z takich jak adres zamieszkania, ale nie są często bardzo ludzi. Na przykład jest łatwiej zapamiętać nazwę DNS, na przykład **contoso.com** , niż do zapamiętania adresu IP, takich jak 192.168.1.88 lub 2001:0:4137:1f67:24a2:3888:9cce:fea3.

DNS system jest oparty na *rekordów*. Rekordy skojarzyć określone *nazwy*, na przykład **contoso.com**, adres IP lub inną nazwę usługi DNS. Gdy aplikacji, takich jak przeglądarka sieci web, wyszukuje nazwy w systemie DNS, wyszukuje rekord i używa, niezależnie od wskazuje jako adres. Jeśli wartość, która wskazuje jest adres IP, wartości używać przeglądarki. Wskazuje na inną nazwę usługi DNS, aplikacja zawiera wykonaj ponownie rozdzielczość. Ostatecznie rozpoznawanie nazw zakończy się w polu adres IP.

Po utworzeniu witryny sieci Web Azure nazwę DNS jest automatycznie przypisywany do witryny. Ta nazwa ma postać ** &lt;yoursitename&gt;. azurewebsites.net**. Po dodaniu swojej witryny sieci Web jako punkt końcowy Azure ruch menedżera witryny sieci Web będzie dostępne za pośrednictwem ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domeny.

> [AZURE.NOTE] Po skonfigurowaniu witryny sieci Web jako punkt końcowy Menedżer ruchu, którego użyjesz **. trafficmanager.net** adresu podczas tworzenia rekordów DNS.

> Można używać tylko rekordy CNAME z menedżerem ruchu

Są także wiele typów rekordów, każda z ich funkcje i ograniczenia, ale witryn sieci Web skonfigurowane jako punkty końcowe Menedżer ruchu, firma Microsoft tylko zadbać o jeden; Rekordy *CNAME* .

###<a name="cname-or-alias-record"></a>Rekord CNAME lub Alias

Rekord CNAME mapy *określone* nazwy DNS, takich jak **mail.contoso.com** lub **www.contoso.com**na inną nazwę domeny (kanonicznego). W przypadku Azure witryny sieci Web przy użyciu Menedżera ruch nazwa kanoniczna domeny jest ** &lt;MojaApl >. trafficmanager.net** nazwy domeny profilu Menedżer ruchu. Po utworzeniu CNAME tworzy alias dla ** &lt;MojaApl >. trafficmanager.net** nazwy domeny. Wpis CNAME będzie rozpoznawać adresu IP usługi ** &lt;MojaApl >. trafficmanager.net** nazwy domeny na automatycznie, dzięki czemu zmiana adresu IP witryny sieci Web nie musi podejmować żadnych działań.

Gdy ruch odbierana na Menedżer ruchu, następnie kieruje ruch do witryny sieci Web przy użyciu metody, który nie jest skonfigurowana do równoważenia obciążenia. To jest zupełnie przezroczysty odwiedzającym do witryny sieci Web. Zostanie wyświetlony tylko nazwę domeny niestandardowej w przeglądarce.

> [AZURE.NOTE] Niektóre rejestratorów domen Zezwalaj tylko do zamapować poddomen, używając rekordu CNAME, na przykład **www.contoso.com**, a nie głównej nazwy, na przykład **contoso.com**. Aby uzyskać więcej informacji dotyczących rekordów CNAME zapoznaj się z dokumentacją, dostarczony przez rejestratora, <a href="http://en.wikipedia.org/wiki/CNAME_record">Zapis Wikipedia rekord CNAME</a>lub dokumentu <a href="http://tools.ietf.org/html/rfc1035">IETF domen — Implementacja i specyfikacja</a> .
