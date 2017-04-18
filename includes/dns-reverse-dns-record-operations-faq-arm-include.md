<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Często zadawane pytania — odwrotne DNS dotyczące adresu IP przypisany Azure

### <a name="how-much-do-reverse-dns-records-cost"></a>Ile odwrócić koszt rekordy DNS?
Są one bezpłatne!  Nie ma żadnych dodatkowych kosztów dla odwrotnej rekordy DNS lub kwerend.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Odwrotnej rekordy DNS dla mojej przypisane Azure publiczny adres IP rozwiąże z Internetu?
Wartość Tak. Po ustawieniu odwrotnej właściwość DNS dla swojej publiczny adres IP, Azure zarządza delegowania DNS i strefy DNS wymagane w celu zapewnienia, że dla wszystkich użytkowników internet jest rozpoznawana jako odwrotnej rekordu DNS.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Rekord DNS odwrotnej domyślne będą tworzone dla mojej publicznych adresów IP?
Wartość nie. Odwrotnej DNS jest funkcją opcjonalnych. Nie domyślnego odwrotnej rekordy DNS są tworzone, jeśli nie zostanie skonfigurowane.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Co to jest format dla w pełni kwalifikowaną nazwę domeny (FQDN)?
Nazwy FQDN są określane w kolejności do przodu i musi być zamykane przez kropki (np "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Co się stanie, jeśli sprawdzanie poprawności odwrotnej DNS I określone kończą się niepowodzeniem?
Miejsce, w którym sprawdzanie poprawności dla odwrotne DNS kończą się niepowodzeniem, operacji usługi zarządzania zakończy się niepowodzeniem. Popraw odwrotnej wartość DNS zgodnie z wymaganiami i spróbuj ponownie.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Mojej witryny sieci Web Azure mogą zarządzać odwrotne DNS?
Odwrotne DNS nie jest obsługiwane Azure witryn sieci Web. Odwrotne DNS jest obsługiwane dla maszyn wirtualnych Azure.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Moje publiczny adres IP można skonfigurować wiele odwrotne rekordów DNS?
Wartość nie. Azure obsługuje jeden rekord DNS odwrotne dla każdego publiczny adres IP. Jednak każdy publiczny adres IP może mieć własne odwrotne rekordu DNS.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Dla publiczny adres IP protokołu IPv6 można skonfigurować odwrotne rekordy DNS?
Wartość nie.  W tej chwili odwrotnej rekordy DNS są tylko obsługiwane przez publiczne adresy IP protokołu IPv4.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Można skonfigurować odwrotnej rekord DNS dla mojej publiczny adres IP bez konieczności DomainNameLabel określonej?
Wartość nie. Aby wykorzystać odwrotnej rekordy DNS dla usługi publiczne adresy IP, musisz określić właściwość DomainNameLabel.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Można wysyłać wiadomości e-mail do domen zewnętrznych z moimi usługami obliczyć Azure?
Wartość nie. [Obliczenia azure usług nie obsługują wysyłanie wiadomości e-mail do domen zewnętrznych](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
