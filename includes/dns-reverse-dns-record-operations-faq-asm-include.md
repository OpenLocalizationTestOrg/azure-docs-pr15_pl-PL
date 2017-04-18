<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Często zadawane pytania — odwrotnej DNS dotyczące adresu IP przypisany Azure

### <a name="how-much-do-reverse-dns-records-cost"></a>Ile odwrócić koszt rekordy DNS?
Są one bezpłatne!  Nie ma żadnych dodatkowych kosztów dla odwrotnej rekordy DNS lub kwerend.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Moje odwrotnej rekordy DNS rozwiąże z Internetu?
Wartość Tak. Po ustawieniu odwrotnej właściwość DNS dla usługi w chmurze, Azure zarządza delegowania DNS i strefy DNS wymagane w celu zapewnienia, że dla wszystkich użytkowników internet jest rozpoznawana jako odwrotnej rekordu DNS.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Zostaną utworzone domyślnego odwrotnej rekordu DNS dla usług w chmurze?
Wartość nie. Odwrotnej DNS jest funkcją opcjonalnych. Nie domyślnego odwrotnej rekordy DNS są tworzone, jeśli nie zostanie skonfigurowane.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Co to jest format dla w pełni kwalifikowaną nazwę domeny (FQDN)?
Nazwy FQDN są określane w kolejności do przodu i musi być zamykane przez kropki (np "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Co się stanie, jeśli sprawdzanie poprawności odwrotnej DNS I określone kończą się niepowodzeniem?
Miejsce, w którym sprawdzanie poprawności dla odwrotnej DNS kończą się niepowodzeniem, operacji usługi zarządzania zakończy się niepowodzeniem. Popraw odwrotnej wartość DNS zgodnie z wymaganiami i spróbuj ponownie.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Mojej witryny sieci Web Azure mogą zarządzać odwrotnej DNS?
Odwrotnej DNS nie jest obsługiwane Azure witryn sieci Web. Odwrotnej DNS jest obsługiwane dla ról Azure PaaS i maszyn wirtualnych IaaS.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Można skonfigurować wiele odwrotnej rekordów DNS dla usługi w chmurze?
Wartość nie. Azure obsługuje jeden rekord DNS odwrotnej dla każdej usługi w chmurze Azure. Jednak każdej usługi w chmurze Azure może mieć własne odwrotnej rekordu DNS.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Można wysyłać wiadomości e-mail do domen zewnętrznych z moimi usługami obliczyć Azure?
Wartość nie. [Obliczenia azure usług nie obsługują wysyłanie wiadomości e-mail do domen zewnętrznych](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
