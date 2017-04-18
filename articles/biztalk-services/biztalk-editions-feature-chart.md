<properties
    pageTitle="Informacje o funkcjach w wersjach usług BizTalk | Microsoft Azure"
    description="Porównanie możliwości wersje usług BizTalk: wolny, Deweloper, Basic, Standard i Premium. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>Usługi BizTalk: Wykres wersji

Usług Azure BizTalk oferuje różne wersje. W tym artykule umożliwia określenie, która wersja jest odpowiednio do potrzeb scenariusz i małych firm.


## <a name="compare-the-editions"></a>Porównanie wersji

**Bezpłatne (wersja Preview)**

Można tworzyć i zarządzanie połączeniami hybrydowych. Połączenie hybrydowego jest łatwy sposób nawiązywania połączenia z lokalnym systemem, takich jak program SQL Server Azure witrynę sieci Web.

**Deweloper**

Zawiera połączeń hybrydowe, EAI i Edytuj przetwarzanie wiadomości z portalu zarządzania partnerów handlowych łatwe w obsłudze i obsługę typowe schematy grupy użytkowników i sformatowanego przetwarzanie Edytuj nad X12 i AS2. Można utworzyć typowe scenariusze EAI połączenia usług w chmurze z protokołów HTTP/S, REST, FTP, WCF i SFTP do odczytu i zapisu wiadomości.  Połączenie lokalne systemy LOB korzystać z gotowych do użycia karty SAP, zmierzają Oracle, bazy danych programu Oracle, Siebel i SQL Server. Za pomocą środowisku wyśrodkowany na Deweloper narzędzia Visual Studio łatwość programowania i wdrażania. Ograniczone do celów projektowania i testowania tylko z nie usługi umową dotyczącą poziomu (SLA).

**Podstawowe**

Zawiera większości funkcji Deweloper z podwyżki w połączeniach połączeń hybrydowe, EAI mosty, Edytuj umów i BizTalk karty dodatkiem Service Pack. Oferuje wysoką dostępność i możliwość skalowania z umowy poziomu usług (SLA).

**Standardowe**

Zawiera wszystkie podstawowe funkcje z podwyżki w połączeniach połączeń hybrydowe, EAI mosty, Edytuj umów i BizTalk karty dodatkiem Service Pack. Oferuje wysoką dostępność i możliwość skalowania z umowy poziomu usług (SLA).

**Premium**

Zawiera standardowych funkcji z podwyżki w połączeniach połączeń hybrydowe, EAI mosty, Edytuj umów i BizTalk karty dodatkiem Service Pack. Zawiera również archiwizowania, wysokiej dostępności i opcja Skaluj z umowy poziomu usług (SLA).


## <a name="editions-chart"></a>Wersje wykresu
W poniższej tabeli wymieniono różnice.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Bezpłatne (wersja Preview)</th>
        <th>Deweloper</th>
        <th>Podstawowe</th>
        <th>Standardowe</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Uruchamianie cena</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Cennik usługi Azure BizTalk</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure ceny kalkulatora</a></td>
</tr>
<tr>
<td><strong>Minimalna konfiguracja domyślne</strong></td>
<td>1 jednostka bezpłatne</td>
<td>Jednostka 1 — Deweloper</td>
<td>Podstawowa jednostka 1</td>
<td>1 jednostka standardowy</td>
<td>Jednostki premii 1</td>
</tr>
<tr>
<td><strong>Skala</strong></td>
<td>Bez skalowania</td>
<td>Bez skalowania</td>
<td>Tak, w przyrostach 1 podstawowa jednostka</td>
<td>Tak, w przyrostach 1 jednostka standardowy</td>
<td>Tak, w przyrostach 1 jednostka Premium</td>
</tr>
<tr>
<td><strong>Maksymalna dozwolona wartość skali się</strong></td>
<td>Bez skalowania</td>
<td>Bez skalowania</td>
<td>Do 8 jednostek</td>
<td>Do 8 jednostek</td>
<td>Do 8 jednostek</td>
</tr>
<tr>
<td><strong>Mosty EAI na jednostkę</strong></td>
<td>Niedostępna</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDYTUJ, AS2</strong>
<br/><br/>
Zawiera umów TPM</td>
<td>Niedostępna</td>
<td>Uwzględniane. 10 Umowy na jednostkę.</td>
<td>Uwzględniane. 50 umowy na jednostkę.</td>
<td>Uwzględniane. 250 umowy na jednostkę.</td>
<td>Uwzględniane. 1000 umowy na jednostkę.</td>
</tr>
<tr>
<td><strong>Połączenia hybrydowych na jednostkę</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Transfer danych połączenia hybrydowych (GB) na jednostkę</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>Usługa karty BizTalk połączeń z systemami LOB lokalnego</strong></td>
<td>Niedostępna</td>
<td>Liczba połączeń: 1</td>
<td>połączenia 2</td>
<td>5 połączenia</td>
<td>25 połączeń</td>
</tr>
<tr>
<td align="left"><strong>Obsługiwane protokoły i systemy:</strong>
<ul>
<li>HTTP</li>
<li>PROTOKÓŁ HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Usługa Bus (SB)</li>
<li>Obiektów Blob platformy Azure</li>
<li>Interfejsy API REST</li>
</ul>
</td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
</tr>
<tr>
<td><strong>Wysoka dostępność</strong>
<br/><br/>
Umowa dotycząca poziomu usług (SLA) zobacz <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk usług ceny</a>.
</td>
<td>Niedostępna</td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
</tr>
<tr>
<td><strong>Kopia zapasowa i przywracanie</strong></td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
</tr>
<tr>
<td><strong>Śledzenie</strong></td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
</tr>
<tr>
<td><strong>Archiwizowanie</strong><br/><br/>
Zawiera Niemożność wyparcia się potwierdzenia (NRR) i pobieranie śledzone wiadomości</td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Niedostępna</td>
<td>Niedostępna</td>
<td>Dostępny</td>
</tr>
<tr>
<td><strong>Użycia kodu niestandardowego</strong></td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
</tr>
<tr>
<td><strong>Używanie transformacji, w tym niestandardowe XSLT</strong></td>
<td>Niedostępna</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
<td>Dostępny</td>
</tr>
</table>

> [AZURE.NOTE] Dla ochrony przed awariami sprzętu szybkiej oznacza o wielu maszyny wirtualne w całość BizTalk.


## <a name="faqs"></a>Często zadawane pytania

#### <a name="what-is-a-biztalk-unit"></a>Co to jest jednostka BizTalk?
"Jednostka" jest poziomem Atomowej wdrożenia usługi Azure BizTalk. Każdej wersji zawiera jednostkę, która ma inny wydajności i pamięci. Na przykład podstawowa jednostka ma obliczeń więcej niż Deweloper, standardowe ma więcej niż podstawowe do uruchamiania i tak dalej. Skalowanie usługą BizTalk skalowania pod względem jednostki.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Jaka jest różnica między usługami BizTalk i maszyn wirtualnych systemu BizTalk Azure?
Usług BizTalk przewiduje wartość PRAWDA architektura platformy jako z usługi (PaaS) tworzenie rozwiązań integracji w chmurze. Z modelem PaaS całkowicie skoncentrować się na logiki aplikacji i pozostaw wszystkie zarządzania infrastrukturą do firmy Microsoft, w tym:

- Bez konieczności lub poprawka maszyn wirtualnych i zarządzać nimi.
- Microsoft zapewnia dostępność.
- Sterowanie skali na żądanie, po prostu zażądanie więcej lub mniej pojemności za pośrednictwem portalu Azure.

Serwer BizTalk na maszyn wirtualnych Azure udostępnia architekturę infrastruktury jako z usługi (IaaS). Tworzenie maszyn wirtualnych i skonfigurować je dokładnie tak, jak środowiska lokalnego, co ułatwia uruchamianie aplikacji w chmurze bez żadnych zmian kodu. Z IaaS możesz nadal są odpowiedzialne za konfigurowania maszyn wirtualnych, zarządzanie maszyn wirtualnych (na przykład, instalowanie oprogramowania i poprawek systemu operacyjnego) i Projektując wniosku o wysokiej dostępności.

Jeśli szukasz na które minimalizowanie wszelką zarządzania infrastrukturą rozwiązań integracji, za pomocą usług BizTalk. Jeśli chcesz szybko przeprowadzić migrację z istniejących rozwiązań BizTalk, czy szukasz środowisku na żądanie do projektowania i testowania aplikacji serwera BizTalk, Użyj serwera BizTalk na maszyn wirtualnych Azure.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Jaka jest różnica między usługą karty BizTalk i hybrydowego połączenia?
Usługa karty BizTalk jest używany przez usługę BizTalk Azure. Usługa karty BizTalk używa pakiet karty BizTalk nawiązywania połączenia z lokalnym systemem linii biznesowych (LOB). Połączenie hybrydowych stanowi łatwy i wygodny sposób łączenia Azure aplikacji, takich jak funkcja aplikacji sieci Web w usłudze Azure aplikacji i usług Mobile Azure, do zasobu lokalnego.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Co oznacza "Hybrydowych połączenia Transfer danych (GB) na jednostkę"? Jest to na minutę/godzina/dnia/tygodnia/miesiąca? Co się dzieje po osiągnięciu limitu?

Połączenie hybrydowych koszt jednostkowy zależy od wersji usługi BizTalk. Krótko mówiąc, zależą koszty na ilości danych można transferować. Na przykład przesyłania 10 GB danych codziennie koszty mniejszy niż codziennie przenoszenia 100 GB. Za pomocą [Kalkulatora cennik](https://azure.microsoft.com/pricing/calculator/?scenario=full) usługi BizTalk określisz określonych kosztów. Zazwyczaj limity są wymuszane codziennie. W przypadku przekroczenia limitu, wszelkie nadmiar jest rozliczana według stawki $ 1 za GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Gdy tworzę Umowę w usługach BizTalk, dlaczego liczbę mosty przejść w górę przez dwa zamiast tylko jednej?

Każda umowa składa się z dwóch różnych mosty: mostka komunikacji stronie Wyślij i Odbierz mostka komunikacji po stronie.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Co się dzieje po naciśnięciu I limit przydziału na liczbę mosty lub umów?

Nie jest możliwe w przypadku wdrażania wszelkie nowe mosty lub utwórz wszelkie nowe umowy. Aby wdrożyć więcej, musisz rozbudowy więcej jednostek usługi BizTalk lub uaktualnianie do nowszej wersji.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Jak migrację z jednej warstwy usług BizTalk do innego?

Bezpłatna wersja nie może być migrowane lub "skalowana w górę" do innej warstwy, a nie mogą być kopii zapasowej i przywrócone do innej warstwy. Jeśli potrzebujesz innej warstwie, tworzenie nowej usługi BizTalk przy użyciu nowego poziomu. Wszelkie artefakty utworzony przy użyciu bezpłatna wersja, łącznie z połączeniami hybrydowych konieczne zostanie odtworzony w nowej usługi BizTalk. 

W przypadku pozostałych wersji za pomocą kopii zapasowej i przywracanie do migracji programu artefakty z jednej warstwy do innego. Na przykład, aby utworzyć kopię zapasową usługi artefaktów w warstwie standardowy, a następnie je przywrócić w warstwie Premium. [Usług BizTalk: wykonywanie kopii zapasowych i przywracanie](biztalk-backup-restore.md) opisano obsługiwane ścieżki migracji i list, jakie artefakty kopię zapasową. Zauważ, że hybrydowych połączeń nie są kopii zapasowej. Po kopii zapasowych i przywracanie do nowego poziomu, możesz ponownie utworzyć połączenia hybrydowych.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Jest usługa karty BizTalk zawarte w usłudze? Jak otrzymać oprogramowanie?

Tak, Usługa karty BizTalk pakietem karty BizTalk są dołączone Azure BizTalk Services SDK [Pobieranie](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Następne kroki

Aby utworzyć usługi Azure BizTalk w Azure portal, przejdź do [usług BizTalk: inicjowania obsługi administracyjnej przy użyciu Azure portal](biztalk-provision-services.md). Aby rozpocząć tworzenie aplikacji, przejdź do [Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Dodatkowe zasoby
- [Usługi BizTalk: Inicjowania obsługi administracyjnej za pomocą portalu Azure](biztalk-provision-services.md)<br/>
- [Usługi BizTalk: Wykres stan inicjowania obsługi administracyjnej](biztalk-service-state-chart.md)<br/>
- [BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk usługi: Kopia zapasowa i przywracanie](biztalk-backup-restore.md)<br/>
- [Usługi BizTalk: ograniczanie](biztalk-throttling-thresholds.md)<br/>
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](biztalk-issuer-name-issuer-key.md)<br/>
- [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
