<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Często zadawane pytania — hostingu swoją strefę ARPA w systemie DNS Azure

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Czy można zawierają stref ARPA w mojej bloków IP przypisany usługodawcy internetowego na Azure DNS?
Wartość Tak. Obsługują strefy ARPA dla własnego zakresów adresów IP w systemie DNS Azure jest w pełni obsługiwany.

Po prostu [utworzyć strefy Azure DNS](dns-getstarted-create-dnszone.md), a następnie współdziałają z usługodawcy internetowego do [delegowania strefy](dns-domain-delegation.md).  Rekordy PTR dla każdej odwrotnej wyszukiwania można zarządzać w taki sam sposób jak inne typy rekordów.

Możesz też [zaimportować istniejącej strefie wyszukiwania odwrotnego za pomocą interfejsu wiersza polecenia Azure](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Ile kosztuje usługa hostingu moich kosztów strefy ARPA?
Obsługa strefie ARPA swój adres IP przypisany usługodawcy internetowego bloku w systemie DNS Azure jest rozliczana według [stawki standardowej Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Czy można udostępniać stref ARPA dla adresów IPv4 i IPv6 w systemie DNS Azure?
Wartość Tak.