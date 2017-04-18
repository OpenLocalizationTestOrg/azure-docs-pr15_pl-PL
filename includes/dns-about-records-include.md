## <a name="about-records"></a>Informacje o rekordach

Każdy rekord DNS ma nazwę i typ. Rekordy są zorganizowane na różne typy zgodnie z danymi, które zawierają. Najpopularniejszym typem jest "" rekordu, który map nazwę na adres IP protokołu IPv4. Rekord "MX", który mapy nazwę na serwerze poczty jest innego typu.

Azure DNS obsługuje wszystkich typowych typów rekordów DNS, łącznie z odpowiedzi, AAAA, CNAME, MX, NS, PTR, SOA, SRV i TXT. Należy zauważyć, że:
- Zestawy rekordów SOA są tworzone automatycznie w każdej strefie, nie można utworzyć oddzielnie.
- Czy można utworzyć rekordy SPF przy użyciu tego typu rekordu TXT. Aby uzyskać więcej informacji zobacz [tę stronę](http://tools.ietf.org/html/rfc7208#section-3.1).

W systemie DNS Azure określono rekordów przy użyciu nazw względnych. "Pełna" nazwa domeny (FQDN) zawiera nazwę strefy, dlatego nie ma nazwę "względnym". Na przykład względną nazwą rekord "www", "contoso.com" w strefie daje www.contoso.com w pełni kwalifikowana nazwa rekordu.

## <a name="about-record-sets"></a>Informacje o zestawach rekordów

Czasem trzeba utworzyć więcej niż jeden rekord DNS z danej nazwy i typu. Załóżmy na przykład, że witryna sieci web "www.contoso.com" jest obsługiwana na dwóch różnych adresów IP. Witryna sieci Web wymaga dwóch różnych rekordów, jedną dla każdego adresu IP. Oto przykład zestaw rekordów:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS zarządza rekordami DNS za pomocą zestawy rekordów. Zestaw rekordów jest zbiór rekordów DNS w strefie, które mają taką samą nazwę i tego samego typu. Większość zestawów rekordów zawierających jeden rekord, ale przykłady podobny do tego, w której zestaw rekordów zawiera więcej niż jeden rekord, nie są rzadko.

Zestawy rekordów CNAME i SOA są wyjątki. Standardy DNS nie zezwala na wielu rekordów z tą samą nazwą dla tych typów.

Czas wygaśnięcia lub TTL Określa, jak długo każdy rekord jest buforowane przez klientów przed poszukiwanych ponownie. W tym przykładzie wartość TTL jest 3600 sekund lub 1 godzina. Wartość TTL jest określona dla zestawu rekordów, nie dla każdego rekordu, tę samą wartość jest używana dla wszystkich rekordów w ramach tego zestawu rekordów.

#### <a name="wildcard-record-sets"></a>Zestawy rekordów symboli wieloznacznych

Azure DNS obsługuje [rekordy symboli wieloznacznych](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Zwracane dla dowolnej kwerendy przy użyciu pasujące nazwy (o ile nie znajduje się bliżej dopasowanie z wartością symboli zestawu rekordów). Symbol wieloznaczny zestawy rekordów są obsługiwane dla wszystkich typów rekordów, z wyjątkiem NS i SOA.  

Aby utworzyć zestaw rekordów symboli wieloznacznych, należy użyć nazwy zestawu rekordów "\*". Możesz także użyć nazwy z etykietą "\*", na przykład"\*foo".

#### <a name="cname-record-sets"></a>Zestawy rekordów CNAME

Zestawy rekordów CNAME nie współistnienie inne zestawy rekordów o takiej samej nazwie. Na przykład, nie można utworzyć rekord CNAME ustaw o nazwie względne "www", a rekord A nazwą względne "www" w tym samym czasie. Ponieważ wierzchołka strefy (nazwa = ‘@’) zawsze zawiera zestawy, które zostały utworzone podczas tworzenia strefy rekordów NS i SOA, nie można utworzyć rekord CNAME na wierzchołka strefy. Te ograniczenia wynikać z standardy DNS i nie są ograniczenia dotyczące Azure DNS.
