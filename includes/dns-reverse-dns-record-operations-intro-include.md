## <a name="what-is-reverse-dns"></a>Co to jest DNS odwrotnej?

Konwencjonalny rekordy DNS włączyć mapowanie nazwy DNS (na przykład "www.contoso.com") na adres IP (na przykład 64.4.6.100).  System DNS odwrotnej umożliwia tłumaczenia adresu IP (64.4.6.100) do nazwy (www.contoso.com).

Odwrotnej rekordy DNS są używane w różnych sytuacjach. Na przykład odwrotnej rekordy DNS są powszechnie używane w zwalczania wiadomości-śmieci wiadomości e-mail od zweryfikowania nadawcy wiadomości e-mail.  Odbierania serwera poczty pobrać odwrotnej rekord DNS adres IP serwera wysyłającego i sprawdź, jeśli host jest autoryzowany do wysyłania wiadomości e-mail z domeny źródłowym. (Uwaga jednak tej [usługi Azure obliczyć nie obsługują wysyłanie wiadomości e-mail z domenami zewnętrznymi](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Jak odwrotnej działa DNS

Odwrotnej rekordy DNS są obsługiwane w specjalne strefy DNS, nazywane "ARPA" strefy.  Te strefy tworzą hierarchię DNS osobnych równolegle z hierarchią normalny hostingu domen, takich jak "contoso.com".

Na przykład rekordów DNS "www.contoso.com" jest zaimplementowana przy użyciu rekordu DNS "A" o nazwie "www" w strefie contoso.com.  Ten rekord wskazuje odpowiedni adres IP, w tym przypadku 64.4.6.100.  Odwrotnej wyszukiwania jest zaimplementowana oddzielnie, przy użyciu rekord "PTR" o nazwie "100" w strefie "6.4.64.in-addr.arpa" (należy zauważyć, że adresy IP są odwrotne w strefach ARPA.)  Ten rekord PTR, jeśli został on skonfigurowany poprawnie, wskazuje nazwy www.contoso.com.

Gdy organizacja przypisano bloku adresu IP, również uzyskać prawa do zarządzania odpowiednią strefą ARPA. Stref ARPA odpowiadających bloki adresów IP używanych przez Azure są obsługiwane i zarządzane przez firmę Microsoft. Usługodawcy internetowego może obsługiwać strefę ARPA dla adresów IP dla Ciebie, lub może pomóc w utrzymywania strefy ARPA usługi DNS wybranych przez użytkownika, takie jak Azure DNS.

>[AZURE.NOTE] W osobnych, równolegle hierarchii DNS są stosowane wyszukiwania DNS do przodu i odwrotne wyszukiwanie DNS. Jest odwrotne wyszukiwanie "www.contoso.com" **nie** obsługiwany w strefie "contoso.com", ogół jest ona hostowana w strefie ARPA dla odpowiednich blok adresu IP.

Aby uzyskać więcej informacji o odwrotnej DNS Zobacz [Odwrotne wyszukiwanie DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure obsługę odwrotnej DNS

Azure obsługuje dwa scenariusze osobnych odnoszących się do odwrócić DNS:

1. Obsługujący strefę ARPA odpowiadające bloku adresu IP.
2. Umożliwia konfigurowanie odwrotnej rekordu DNS dotyczące adresu IP przypisane usłudze Azure.

Do obsługi DNS poprzedniego, Azure służy do przechowywania stref ARPA i zarządzanie nimi rekordów PTR dla każdego odwrotne wyszukiwanie DNS.  Proces tworzenia strefy ARPA, konfigurowanie przedstawicielstwa i konfigurowanie rekordów PTR jest taka sama jak zwykłe strefy DNS.  Tylko różnice są że musi być skonfigurowane delegowanie za pośrednictwem usługodawcy internetowego zamiast rejestratora DNS i powinien być używany tylko typ rekordu PTR.

Do obsługi jego, Azure umożliwia konfigurowanie odwrotnej wyszukiwanie adresów IP, przydzielonych do tej usługi.  Tego odnośnika odwrotnej jest konfigurowany przez Azure jako rekord PTR w odpowiedniej strefie ARPA.  Te strefy ARPA odpowiadające z zakresów adresów IP używanych przez Azure, są obsługiwane przez firmę Microsoft. **W dalszej części tego artykułu opisano w tym scenariuszu szczegółowo.**
