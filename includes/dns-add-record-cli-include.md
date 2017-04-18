#### <a name="create-an-a-record-set-with-single-record"></a>Tworzenie rekordu A zestaw z pojedynczego rekordu

Aby utworzyć zestaw rekordów, należy użyć `azure network dns record-set create`. Określanie grupy zasobów, nazwa strefy rekordu ustawić względną nazwę, typ rekordu i czas wygaśnięcia (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Po utworzeniu A rekordu zestawu, Dodaj adres IP protokołu IPv4 do rekordu zestaw z `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Tworzenie rekordu AAAA zestaw z pojedynczego rekordu

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Utwórz rekord CNAME z pojedynczego rekordu

Rekordy CNAME Zezwalaj tylko jedną wartość jeden ciąg znaków.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Utwórz rekord MX, ustaw z jednym rekordem

W tym przykładzie używamy nazwę zestawu rekordów "@" do utworzenia rekordu MX u wierzchołka strefy (w tym przypadku "contoso.com"). Jest to typowe w rekordach MX.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Tworzenie rekordu NS zestaw z pojedynczego rekordu

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Utwórz rekord PTR zestaw z pojedynczego rekordu  
W tym przypadku "Moje-arpa-zone.com" oznacza strefę ARPA reprezentującą zakres adresów IP.  Każdy rekord PTR w tej strefie odpowiada adres IP z tego zakresu adresów IP.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Utworzenie rekordu SRV z pojedynczego rekordu

Jeśli tworzysz rekord SRV w katalogu głównym strefy, można określić "_service" i "_protocol" w polu Nazwa rekordu. Trzeba uwzględnić "@" w polu Nazwa rekordu.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Utwórz rekord TXT został za jeden rekord

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
