<properties
    pageTitle="Konfigurowanie niestandardowej nazwy domeny w usług w chmurze | Microsoft Azure"
    description="Dowiedz się, jak udostępnić Azure aplikacji lub danych w Internecie z własną domeną konfigurowania ustawień DNS.  W tych przykładach użyto Azure portal."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurowanie niestandardowej nazwy domeny dla usługi w chmurze Azure

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-custom-domain-name-portal.md)
- [Portal Azure klasyczny](cloud-services-custom-domain-name.md)

Po utworzeniu usługi w chmurze Azure przypisuje go do poddomeny **cloudapp.net**. Na przykład jeśli usługa w chmurze ma nazwę "contoso", użytkownicy będą mogli uzyskać dostęp do aplikacji na adres URL podobny http://contoso.cloudapp.net. Azure przypisuje wirtualny adres IP.

Można jednak także uwidaczniają aplikacji na własną nazwę domeny, na przykład **contoso.com**. W tym artykule wyjaśniono, jak można zarezerwować lub skonfigurować niestandardowej nazwy domeny dla usługi w chmurze role w sieci web.

Jeśli nie ma już undestand, jakie rekordy CNAME i A są? [Skok poza explaination](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Procedury przedstawione w tym zadaniu mają zastosowanie Azure usługami w chmurze. W przypadku aplikacji usług zobacz [ten](../app-service-web/web-sites-custom-domain-name.md). W przypadku kont miejsca do magazynowania zobacz [ten](../storage/storage-custom-domain-name.md).

<p/>

> [AZURE.TIP]
> Pracuj szybciej — za pomocą nowego Azure [Przewodnik ze wskazówkami dotyczącymi](http://support.microsoft.com/kb/2990804)!  Z usług w chmurze Azure lub witrynach Azure przyciągania ułatwia kojarzenie niestandardowej nazwy domeny i zabezpieczanie komunikacji (SSL).

## <a name="understand-cname-and-a-records"></a>Opis rekordów CNAME i A

CNAME (alias rekordów lub) i rekordów obu umożliwiają kojarzenie nazwy domeny z określonego serwera (lub obsługi w tym przypadku) jednak ich działają inaczej. Istnieją również zagadnienia określonym, podczas korzystania z usługami w chmurze Azure, które należy rozważyć przed mapowanych rekordów.

### <a name="cname-or-alias-record"></a>Rekord CNAME lub Alias

Rekord CNAME mapy *określonej* domeny, na przykład **contoso.com** lub **www.contoso.com**z nazwą domeny kanonicznych. W tym przypadku nazwa kanoniczna domeny jest **.cloudapp [MojaApl] .net** nazwy domeny usługi Azure hostowanej aplikacji. Po utworzeniu CNAME tworzy alias dla **.cloudapp [MojaApl] .net**. Wpis CNAME rozwiąże adres IP swojego **.cloudapp [MojaApl] .net** usługi automatycznie, więc zmiana adresu IP usługi w chmurze, nie musisz podejmować żadnych działań.

> [AZURE.NOTE]
> Niektóre rejestratorów domen Zezwalaj tylko do zamapować poddomen, używając rekordu CNAME, na przykład www.contoso.com, a nie głównej nazwy, na przykład contoso.com. Aby uzyskać więcej informacji dotyczących rekordów CNAME zobacz dokumentację dostarczoną przez rejestratora, [Zapis Wikipedia rekord CNAME](http://en.wikipedia.org/wiki/CNAME_record)lub dokument [IETF domen — Implementacja i specyfikacja](http://tools.ietf.org/html/rfc1035) .

### <a name="a-record"></a>Rekord

Rekord *A* map domenę, na przykład **contoso.com** lub **www.contoso.com**, *lub domena symboli* takich jak ** \*. contoso.com**, adres IP. W przypadku Azure usługi w chmurze, wirtualnych adresów IP usługi. Dlatego głównych korzyści, jakie rekord A nad rekord CNAME ma mieć jeden wpis, która korzysta z symboli wieloznacznych, takich jak \* **. contoso.com**, które chcesz obsługiwać żądań wiele domen podrzędnych, takich jak **mail.contoso.com**, **login.contoso.com**lub **www.contso.com**.

> [AZURE.NOTE]
> Ponieważ rekord A jest mapowane statyczny adres IP, nie można automatycznie rozpoznać zmiany adresu IP usługi w chmurze. Adres IP używane przez usługi w chmurze przydzielonego po raz pierwszy Wdroż pustego przedział (produkcji lub tymczasowego.) Po usunięciu wdrożenia dla niej adres IP został wydany przez Azure i wszystkie przyszłe wdrożenia do przedziału należy podać adres IP.
>
> Wygodne adres IP przedział danego wdrożenia (produkcji lub tymczasowego) jest zachowywane podczas wymiany między tymczasowego i wdrożeń produkcji lub w przypadku uaktualniania w miejscu istniejącego wdrożenia. Aby uzyskać więcej informacji o wykonaniu powyższych czynności zobacz [jak zarządzać usługami w chmurze](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Dodawanie rekordu CNAME dla domeny niestandardowej

Do utworzenia rekordu CNAME, użytkownik musi Dodaj nowy wpis w tabeli DNS dla domeny niestandardowej za pomocą narzędzi dostarczony przez rejestratora. Każdy rejestrator zawiera podobne, ale nieco inną metodę określania rekordu CNAME, ale pojęcia są takie same.

1. Użyj jednej z następujących metod znajdowanie **. cloudapp.net** nazwy domeny przypisane do usługi w chmurze.

    * Zaloguj się do [portalu Azure], zaznacz do usługi w chmurze, spójrz na sekcji **Essentials** , a następnie znajdź pozycję **Adres URL witryny** .

        ![Sekcja Szybkie rzut przedstawiające adres URL witryny][csurl]
            
        **LUB**
  
    * Instalowanie i konfigurowanie [Programu Powershell Azure](../powershell-install-configure.md), a następnie użyj następującego polecenia:

        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Zapisz nazwę domeny używaną w adresie URL zwracane przez wybranej metody, jak będą potrzebne podczas tworzenia rekordu CNAME.

1.  Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Poszukaj łącza lub obszarów witryny oznaczony jako **Nazwę domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

2.  Teraz znaleźć miejsce, w którym można wybrać lub wprowadzić w CNAME. Być może trzeba wybrać typ rekordu, z listy w dół lub przejdź do strony ustawień zaawansowanych. Należy szukać wyrazy **CNAME** **Alias**lub **poddomeny**.

3.  Również należy podać alias domeny lub poddomeny rekordu CNAME, takie jak **"www"** , jeśli chcesz utworzyć aliasu dla **www.customdomain.com**. Jeśli chcesz utworzyć aliasu dla domeny głównej, może być dostępna jako "**@**" symbol w narzędziach DNS rejestratora.

4. Następnie należy podać nazwę hosta kanonicznych, w tym przypadku jest domeną **cloudapp.net** aplikacji.

Na przykład następujący rekord CNAME przekazuje cały ruch z **www.contoso.com** do **contoso.cloudapp.net**, nazwę domeny niestandardowej wdrożonej aplikacji:

| Alias-hosta Nazwa poddomeny | Kanoniczny domeny     |
| ------------------------- | -------------------- |
| "www"                       | contoso.cloudapp.NET |

> [AZURE.NOTE]
PRAWDA hosta (contoso.cloudapp.net), nie będzie widoczna w odwiedzającego **www.contoso.com** , więc procesu przesyłania dalej jest niewidoczne dla użytkownika końcowego.

> Powyższym przykładzie dotyczy tylko ruchu w poddomeny **ciągu "www"** . Ponieważ nie można używać symboli wieloznacznych rekordów CNAME, należy utworzyć jeden CNAME dla każdej domeny i poddomeny. Jeśli chcesz przekierowanie ruchu z poddomen, takich jak *. contoso.com, aby adres cloudapp.net można skonfigurować * *Przekierowania adresu URL* * lub * *Adres URL dalej** wpisowi ustawień DNS, lub Utwórz rekord.


## <a name="add-an-a-record-for-your-custom-domain"></a>Dodawanie rekordu A dla domeny niestandardowej

Aby utworzyć rekord, możesz znaleźć wirtualny adres IP usługi w chmurze. Następnie dodaj nowy wpis w tabeli DNS dla domeny niestandardowej za pomocą narzędzi dostarczony przez rejestratora. Każdy rejestrator zawiera podobne, ale nieco inną metodę określania rekordu A, ale pojęcia są takie same.

1. Użyj jednej z następujących metod uzyskiwania adresu IP usługi w chmurze.

    * Zaloguj się do [Azure portal]wybierz usługi w chmurze, spójrz na sekcji **Essentials** , a następnie znajdź wpis **publiczne adresy IP** .

        ![Sekcja Szybkie rzut przedstawiający VIP][vip]

        **LUB**

    * Instalowanie i konfigurowanie [Programu Powershell Azure](../powershell-install-configure.md), a następnie użyj następującego polecenia:

        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Zapisz adres IP, jak będą potrzebne podczas tworzenia rekordu A.

1.  Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Poszukaj łącza lub obszarów witryny oznaczony jako **Nazwę domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

2.  Teraz znaleźć miejsce, w którym można wybrać lub wprowadzić rekordu. Być może trzeba wybrać typ rekordu, z listy w dół lub przejdź do strony ustawień zaawansowanych.

3. Wybierz lub wprowadź domena lub poddomena, który będzie używany ten rekord. Na przykład zaznacz **ciągu "www"** , jeśli chcesz utworzyć aliasu dla **www.customdomain.com**. Jeśli chcesz utworzyć wpis symboli wieloznacznych dla wszystkich poddomen, wprowadź "__*__". To obejmuje wszystkie domeny podrzędne, takie jak **mail.customdomain.com**, **login.customdomain.com**i **www.customdomain.com**.

    Jeśli chcesz utworzyć rekord A dla domeny głównej, może być dostępna jako "**@**" symbol w narzędziach DNS rejestratora.

4. Wprowadź adres IP usługi w chmurze w polu dostarczonych. Skojarzenie wpisu domain używane w rekordzie o adresie IP wdrożenia usługi cloud.

Na przykład po rekordu przekazuje cały ruch z **contoso.com** do **137.135.70.239**, adres IP usługi aplikacji:

| Nazwa hosta/poddomeny | Adres IP     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |


W tym przykładzie przedstawiono tworzenia rekordu A dla domeny głównej. Jeśli chcesz utworzyć wpis symbol wieloznaczny dotyczyć wszystkich poddomen, należy wpisać "__*__" jako poddomena.

>[AZURE.WARNING]
>Adresy IP platformy Azure są dynamiczne domyślnie. Prawdopodobnie można użyć [zastrzeżonego adresu IP](../virtual-network/virtual-networks-reserved-public-ip.md) , aby upewnić się, że adres IP nie zmienia się.

## <a name="next-steps"></a>Następne kroki

* [Jak zarządzać usługami w chmurze](cloud-services-how-to-manage.md)
* [Jak do zawartości sieci CDN mapy niestandardowej domeny](../cdn/cdn-map-content-to-custom-domain.md)
* [Ogólne Konfiguracja usługi w chmurze](cloud-services-how-to-configure-portal.md).
* Dowiedz się, jak [wdrożyć usługi w chmurze](cloud-services-how-to-create-deploy-portal.md).
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
 