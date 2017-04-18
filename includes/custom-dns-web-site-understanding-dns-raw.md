System nazw domen (DNS) jest używany do lokalizowania zasobów w Internecie. Na przykład kiedy wprowadzić adres aplikacji sieci web w przeglądarce lub kliknij łącze na stronie sieci web, używa DNS, można tłumaczyć domeny na adres IP. Adres IP jest sort z takich jak adres zamieszkania, ale nie jest bardzo ludzi przyjaznej. Na przykład jest łatwiej zapamiętać nazwę DNS, na przykład **contoso.com** , niż do zapamiętania adresu IP, takich jak 192.168.1.88 lub 2001:0:4137:1f67:24a2:3888:9cce:fea3.

DNS system jest oparty na *rekordów*. Rekordy skojarzyć określone *nazwy*, na przykład **contoso.com**, adres IP lub inną nazwę usługi DNS. Gdy aplikacji, takich jak przeglądarka sieci web, wyszukuje nazwy w systemie DNS, wyszukuje rekord i używa, niezależnie od wskazuje jako adres. Jeśli wartość, która wskazuje jest adres IP, wartości używać przeglądarki. Wskazuje na inną nazwę usługi DNS, aplikacja zawiera wykonaj ponownie rozdzielczość. Ostatecznie rozpoznawanie nazw zakończy się w polu adres IP.

Po utworzeniu aplikacji sieci web w aplikacji usługi nazwę DNS jest automatycznie przypisywany do aplikacji sieci web. Ta nazwa ma postać ** &lt;yourwebappname&gt;. azurewebsites.net**. Istnieje także wirtualny adres IP dostępne do użytku podczas tworzenia DNS rekordy, więc możesz utworzyć rekordy, które wskazują **. azurewebsites.net**, lub możesz wskazać adres IP.

> [AZURE.NOTE] Adres IP aplikacji sieci web ulegnie zmianie, usuwanie i ponowne tworzenie aplikacji sieci web, czy zmienić tryb plan usługi aplikacji **wolny** po ustawiono na **podstawowe**, **udostępnione**lub **Standardowy**.

Są także wiele typów rekordów, każda z ich funkcje i ograniczenia, ale w przypadku aplikacji sieci web możemy tylko zwracać uwagę na dwa rekordy *A* i *CNAME* .

###<a name="address-record-a-record"></a>Rekord adresu (rekordu)

Rekord A map domenę, na przykład **contoso.com** lub **www.contoso.com**, *lub domena symboli* takich jak ** \*. contoso.com**, adres IP. W przypadku aplikacji sieci web w aplikacji usługi wirtualnych adresów IP usługi lub określony adres IP adres istnieje zakupiona dla aplikacji sieci web.

Podstawowe zalety rekordu A rekordów CNAME są:

* Mapowanie domeny głównej, takiej jak **contoso.com** adresu IP. wielu rejestratorów Zezwalaj tylko na tego rekordów przy użyciu

* Możesz mieć jeden wpis, która korzysta z symboli wieloznacznych, takich jak ** \*. contoso.com**, które chcesz obsługiwać żądań wiele domen podrzędnych, takich jak **mail.contoso.com**, **blogs.contoso.com**lub **www.contso.com**.

> [AZURE.NOTE] Ponieważ rekord A jest mapowane statyczny adres IP, nie można automatycznie rozpoznać zmiany adresu IP aplikacji sieci web. Adres IP do użytku z rekordami znajduje się po skonfigurowaniu ustawień nazwy domeny niestandardowej dla aplikacji sieci web; Jednak tę wartość mogą zmieniać usuwanie i ponowne tworzenie aplikacji sieci web lub zmień tryb plan aplikacji usługi, aby z powrotem na **wolny**.

###<a name="alias-record-cname-record"></a>Alias rekord (rekord CNAME)

Rekord CNAME mapy *określone* nazwy DNS, takich jak **mail.contoso.com** lub **www.contoso.com**na inną nazwę domeny (kanonicznego). W przypadku aplikacji usługi sieci Web, nazwa kanoniczna domeny jest ** &lt;yourwebappname >. azurewebsites.net** nazwy domeny aplikacji sieci web. Po utworzeniu CNAME tworzy alias dla ** &lt;yourwebappname >. azurewebsites.net** nazwy domeny. Wpis CNAME rozwiąże adres IP swojego ** &lt;yourwebappname >. azurewebsites.net** nazwy domeny na automatycznie, dzięki czemu zmiana adresu IP aplikacji sieci web nie musi podejmować żadnych działań.

> [AZURE.NOTE] Niektóre rejestratorów domen Zezwalaj tylko do zamapować poddomen, używając rekordu CNAME, na przykład **www.contoso.com**, a nie głównej nazwy, na przykład **contoso.com**. Aby uzyskać więcej informacji dotyczących rekordów CNAME zobacz dokumentację dostarczoną przez rejestratora, <a href="http://en.wikipedia.org/wiki/CNAME_record">Zapis Wikipedia rekord CNAME</a>lub dokument <a href="http://tools.ietf.org/html/rfc1035">IETF domen — Implementacja i specyfikacja</a> .

###<a name="web-app-dns-specifics"></a>Szczegóły DNS aplikacji sieci Web

Rekord za pomocą aplikacji sieci Web należy najpierw utworzyć jeden z poniższych rekordów TXT:

* **Dla domeny głównej** - rekord TXT DNS **@** do ** &lt;yourwebappname&gt;. azurewebsites.net**.

* **Dla określonej domeny podrzędne** — nazwy DNS ** &lt;poddomenę >** do ** &lt;yourwebappname&gt;. azurewebsites.net**. Na przykład **blogów** Jeśli rekord jest **blogs.contoso.com**.

* **Dla symbolu wieloznacznego sub-dodmains** - rekordu DNS TXT z *** do ** &lt;yourwebappname&gt;. azurewebsites.net**.

Ten rekord TXT jest używany do weryfikowania, że jesteś właścicielem domeny, którą próbujesz użyć. Jest to oprócz tworzenia rekordu A wskazujący na adres IP wirtualnego aplikacji sieci web.

Możesz znaleźć adres IP i **. azurewebsites.net** nazw dla aplikacji sieci web, wykonując następujące czynności:

1. Otwórz [Azure Portal](https://portal.azure.com)w przeglądarce.

2. W karta **Aplikacji sieci Web** kliknij nazwę aplikacji sieci web, a następnie wybierz **domen niestandardowych** z dołu strony.

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. W karta **domen niestandardowych** będą widoczne wirtualny adres IP. Zapisz te informacje, jak będzie używany podczas tworzenia rekordów DNS

    ![](./media/custom-dns-web-site/virtual-ip-address.png)

    > [AZURE.NOTE] Nie można używać nazwy domeny niestandardowej aplikacji sieci web **wolnego** i musisz uaktualnić plan usług aplikacji do **udostępnione**, **podstawowe**, **Standardowy**lub warstwa **Premium** . Więcej informacji na temat plan usług aplikacji ceny poziomów, w tym zmienianie cennik warstwy aplikacji sieci web, zobacz [jak przeskalować aplikacji sieci web](../articles/web-sites-scale.md).
