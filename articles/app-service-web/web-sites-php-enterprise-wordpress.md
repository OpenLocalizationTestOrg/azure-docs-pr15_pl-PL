<properties
    pageTitle="WordPress klasy korporacyjnej na Azure aplikacji usługi | Microsoft Azure"
    description="Dowiedz się, jak udostępnić witrynę WordPress klasy korporacyjnej Azure aplikacji usługi"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>WordPress klasy korporacyjnej na Azure aplikacji usługi

Azure aplikacji usługi oferuje skalowalna, bezpieczny i łatwy w użyciu środowisko misji krytyczne, dużej skali [WordPress] [ wordpress] witryn. Witrynach klasy korporacyjnej, takich jak pakiet [Office] jest uruchamiany samej firmie Microsoft[ officeblog] i [Bing] [ bingblog] blogów. Ten dokument zawiera korzystaniem Azure aplikacji usługi sieci Web do ustanawiania i zarządzania klasy korporacyjnej, oparte na chmurze witryny WordPress obsługujący dużej liczby odwiedzających.

## <a name="architecture-and-planning"></a>Architektura i planowanie

Podstawowe instalacji WordPress ma tylko dwa wymagania.

* **Bazy danych MySQL** - dostępne za pośrednictwem [ClearDB w Azure Marketplace][cdbnstore], lub możesz zarządzać własną instalacji MySQL na Azure maszyn wirtualnych przy użyciu [systemu Windows] [ mysqlwindows] i [Linux oraz][mysqllinux].

  > [AZURE.NOTE] ClearDB udostępnia kilka konfiguracji MySQL, z różnych parametrów dla każdej konfiguracji. Zobacz [Sklepu Azure] [ cdbnstore] uzyskać informacji na temat ofert w sklepie Azure lub bezpośrednio w [witrynie sieci Web ClearDB](http://www.cleardb.com/pricing.view).

* **PHP 5.2.4 lub większa** — usługa aplikacji Azure obecnie zapewnia [PHP w wersji 5.4, 5,5 oraz 5.6][phpwebsite].

    > [AZURE.NOTE] Zalecamy zawsze uruchamiania w najnowszej wersji PHP, aby upewnić się, że masz najnowsze poprawki zabezpieczeń.

### <a name="basic-deployment"></a>Podstawy wdrażania

Podstawowe wymagania można utworzyć rozwiązanie podstawowe w regionie Azure.

![Azure web app i bazy danych MySQL obsługiwany w jednym regionie Azure][basic-diagram]

Podczas pozwoli na skali w nowym oknie aplikacji, tworząc wielu wystąpień aplikacji sieci Web witryny, wszystko jest obsługiwana w centrach danych w określonym regionu geograficznego. Odwiedzający witrynę z zewnątrz tego regionu może zostać wyświetlony czasu wolnego odpowiedzi podczas korzystania z witryny, a centrach danych, w tym regionie Przejdź w dół, co powoduje aplikacji.


### <a name="multi-region-deployment"></a>W przypadku wdrożenia

Za pomocą Azure [Menedżer ruchu][trafficmanager], istnieje możliwość skalowania WordPress witryny w wielu regionach geograficznych zapewniając tylko jeden adres URL dla odwiedzających. Wszystkie osoby odwiedzające okazać się za pomocą Menedżera ruch, a następnie są kierowane do regionu, w zależności od konfiguracji równoważenia obciążenia.

![Azure aplikacji sieci web programu, obsługiwany w wielu regionów, za pomocą szybkiej CDBR router do kierowania do MySQL między różnymi regionami][multi-region-diagram]

W każdym obszarze witryny WordPress nadal będzie można skalować u wielu wystąpień aplikacji sieci Web, ale tym skalowanie jest region określonym; duży ruch regionów można skalować inaczej niż niskiego ruchu z nich.

Można przeprowadzić replikacji i routingu wiele baz danych MySQL korzystanie przez ClearDB [CDBR wysoka dostępność routera] [ cleardbscale] (jak pokazano po lewej stronie) lub [MySQL klaster CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>W przypadku wdrożenia z pamięci podręcznej i magazynowania multimediów
Jeśli witryna akceptuje przekazywania lub plików multimedialnych hosta, za pomocą magazyn obiektów Blob platformy Azure. Jeśli potrzebujesz pamięci podręcznej, należy rozważyć, czy [Redis pamięci podręcznej][rediscache], [Chmury Memcache](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)lub jeden z pamięci podręcznej ofercie w [Magazynie Azure](http://azure.microsoft.com/gallery/store/).

![Azure aplikacji sieci web programu, obsługiwany w wielu regionów, za pomocą szybkiej CDBR router dla MySQL, z pamięci podręcznej zarządzane, magazyn obiektów Blob i sieci CDN][performance-diagram]

Magazyn obiektów blob jest rozkładany regionów domyślnie geo, więc nie musisz się martwić, replikacji plików we wszystkich witrynach. Możesz również włączyć Azure [Sieci dystrybucji zawartości (CDN)] [ cdn] dla magazyn obiektów Blob, które rozdziela pliki, aby zakończyć węzły bliżej odwiedzających.

### <a name="planning"></a>Planowanie

#### <a name="additional-requirements"></a>Wymagania dodatkowe

W tym celu... | Użyj tego ustawienia...
------------------------|-----------
**Przekazywanie lub przechowywanie dużych plików** | [Dodatek WordPress dotyczące korzystania z magazynem obiektów Blob][storageplugin]
**Wysyłanie wiadomości e-mail** | [SendGrid] [ storesendgrid] i [dodatek WordPress dotyczące korzystania z SendGrid][sendgridplugin]
**Niestandardowe nazwy domen** | [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji][customdomain]
**PROTOKÓŁ HTTPS** | [Włącz protokół HTTPS dla aplikacji sieci web w usłudze Azure aplikacji][httpscustomdomain]
**Faza przygotowań sprawdzania poprawności** | [Konfigurowanie przemieszczenia środowiska dla aplikacji sieci web w usłudze Azure aplikacji][staging] <p>Przenoszenie aplikacji sieci web z tymczasowej produkcji również przenosi konfiguracji WordPress. Należy upewnić się, że wszystkie ustawienia są aktualizowane wymagania dotyczące aplikacji produkcji, przed przełączeniem etapowej aplikacji do produkcji.</p>
**Monitorowania i rozwiązywania problemów** | [Włącz rejestrowanie diagnostyczne dla aplikacji sieci web w usłudze Azure aplikacji] [ log] i [Monitorowanie aplikacji sieci Web w usłudze Azure aplikacji][monitor]
**Wdrażanie witryny** | [Wdrażanie aplikacji sieci web w usłudze Azure aplikacji][deploy]

#### <a name="availability-and-disaster-recovery"></a>Dostępność i odtwarzania po awarii

W tym celu... | Użyj tego ustawienia...
------------------------|-----------
**Załaduj salda witryn** lub **geo-rozpowszechnianie witryny** | [Trasy ruchu z menedżerem ruch Azure][trafficmanager]
**Wykonywanie kopii zapasowych i przywracanie** | [Wykonywanie kopii zapasowej aplikacji sieci web w usłudze Azure aplikacji] [ backup] i [Przywracanie aplikacji sieci web w usłudze Azure aplikacji][restore]

#### <a name="performance"></a>Wydajność

Wydajność w chmurze zapewnia przede wszystkim pamięci podręcznej i poza skalowanie; jednak należy rozważyć pamięci, przepustowości i inne atrybuty hostingu aplikacji sieci Web.

W tym celu... | Użyj tego ustawienia...
------------------------|-----------
**Opis możliwości wystąpienia aplikacji usługi** | [Szczegółowe informacje o cenach, włącznie z funkcjami warstwy aplikacji usługi][websitepricing]
**Zasoby pamięci podręcznej** | [Redis pamięci podręcznej][rediscache], [Chmury Memcache](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)lub jeden z pamięci podręcznej ofercie w [Magazynie Azure](/gallery/store/)
**Skalowanie aplikacji** | [Zmienianie aplikacji sieci web w usłudze Azure aplikacji] [ websitescale] [ClearDB wysoka dostępność Routing]i[cleardbscale]. Jeśli wybierzesz do zarządzania instalacji MySQL, należy rozważyć [MySQL klaster CGE] [ cge] dla skali w nowym oknie

#### <a name="migration"></a>Migracji

Istnieją dwie metody Migrowanie istniejącej witryny WordPress do Azure aplikacji usługi.

* ** [Eksportowanie WordPress] [ export] ** -eksportowane zawartość blogu, który można zaimportować do nowej witryny WordPress na usługę aplikacji Azure za pomocą [wtyczki importera WordPress][import].

    > [AZURE.NOTE] Podczas tego procesu umożliwia migracja zawartości, nie są migrowane dowolnego dodatki plug-in, motywy i inne dostosowania. Te musi być zainstalowany ponownie ręcznie.

* **Ręczna migracja** - [kopię zapasową witryny] [ wordpressbackup] i [Baza danych][wordpressdbbackup], a następnie ręcznie go przywrócić do aplikacji sieci web w usłudze Azure aplikacji i skojarzone bazy danych MySQL do migracji wysoce dostosowanych witryn i uniknąć ręcznego zainstalowania wtyczki, motywów i inne dostosowania konieczność powtarzania.

## <a name="step-by-step-instructions"></a>Instrukcje krok po kroku

### <a name="create-a-wordpress-site"></a>Tworzenie witryny WordPress

1. Za pomocą [Usługi Azure Marketplace] [ cdbnstore] do utworzenia bazy danych MySQL rozmiaru określonymi w sekcji [Architektura i planowanie](#planning) , regionu, że będzie przechowywana witryny.

2. Postępuj zgodnie z instrukcjami w sekcji [Tworzenie WordPress aplikacji web app w usłudze Azure aplikacji] [ createwordpress] tworzenie aplikacji sieci web WordPress. Podczas tworzenia aplikacji sieci web, zaznacz pole wyboru **Użyj istniejącej bazy danych MySQL** , a następnie wybierz bazę danych utworzony w kroku 1.

Jeśli są migrowane istniejącej witryny WordPress, zobacz [Migrowanie istniejącej witryny WordPress Azure](#Migrate-an-existing-WordPress-site-to-Azure) po utworzeniu nowej aplikacji sieci web.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migrowanie istniejącej witryny WordPress Azure

Jak wskazano w sekcji [Architektura i planowania](#planning) , istnieją dwa sposoby migrowania witryn WordPress.

* **Eksportowanie oraz importowanie** - witryn bez dużo dostosowania lub po prostu miejsce, w którym ma zostać przeniesiona zawartość.

* **Kopia zapasowa i przywracanie** - witryn z dużą ilością dostosowywania miejsce, w którym chcesz przenieść wszystkie elementy.

Migrowanie witryny, użyj jednej z następujących sekcji.

#### <a name="the-export-and-import-method"></a>Metoda eksportu i importu

1. [Eksportowanie WordPress] za pomocą[ export] do wyeksportowania istniejącej witryny.

2. Tworzenie aplikacji sieci web przy użyciu kroki opisane w sekcji [Tworzenie witryny WordPress](#Create-a-new-WordPress-site) .

3. Zaloguj się do witryny WordPress w aplikacjach sieci Web i kliknij pozycję **dodatki plug-in** -> **Dodaj nowy**. Wyszukaj i zainstaluj wtyczkę **Importera WordPress** .

4. Po zakończeniu instalacji wtyczka importera, kliknij pozycję **Narzędzia** -> **importu**, a następnie wybierz pozycję **WordPress** wtyczki importera WordPress.

5. Na stronie **WordPress importu** kliknij przycisk **Wybierz plik**. Przejdź do pliku WXR wyeksportowanego z istniejącej witryny WordPress, a następnie wybierz **Przekazywanie pliku i zaimportować**.

6. Kliknij przycisk **Prześlij**. Zostanie wyświetlony monit, że importowanie zakończyło się pomyślnie.

8. Po wykonaniu wszystkich tych kroków uruchom ponownie witryny od jej karta aplikacji sieci web w [Azure portal][mgmtportal].

Po zaimportowaniu witryny, może być konieczne wykonaj następujące czynności, aby włączyć ustawienia nie są zawarte w pliku importu.

W przypadku używania to... | Czynność
------------------ | ----------
**Stałych** | Pulpit nawigacyjny WordPress nową witrynę, kliknij kolejno pozycje **Ustawienia** -> **stałych** , a następnie Aktualizuj strukturę stałych
**łącza obrazów i multimediów** | Aby zaktualizować łącza do nowej lokalizacji, za pomocą [wtyczki Mietlica niebieskie zaktualizować adresy URL][velvet], narzędzia wyszukiwania i zamienianie, lub ręcznie w bazie danych
**Motywy** | Przejdź do pozycji **Wygląd** -> aktualizacja motywu witryny zgodnie z potrzebami i**motywu**
**Menu** | Jeśli motywu obsługuje menu, łącza do strony głównej może być nadal katalogu podrzędnego stare osadzony w nich. Przejdź do pozycji **Wygląd** -> **menu** i zaktualizuj je

#### <a name="the-backup-and-restore-method"></a>Metoda i przywracania kopii zapasowych

1. Wykonywanie kopii zapasowej usługi WordPress istniejącej witryny, korzystając z informacji w [kopii zapasowych WordPress][wordpressbackup].

2. Kopia zapasowa istniejącej bazy danych przy użyciu informacji w [kopii zapasowej bazy danych][wordpressdbbackup].

3. Tworzenie bazy danych i przywracanie kopii zapasowej.

    1. Kup nową bazę danych z [Usługi Azure Marketplace][cdbnstore], lub ustawień bazy danych MySQL w [systemie Windows] [ mysqlwindows] i [Linux oraz] [ mysqllinux] maszyn wirtualnych.

    2. Przy użyciu klienta MySQL, takich jak [MySQL Workbench][workbench], nawiązywanie połączenia z bazą danych i zaimportować WordPress bazy danych.

    3. Aktualizowanie bazy danych zmieniania wpisów domenę do nowej domeny Azure aplikacji usługi. Na przykład mywordpress.azurewebsites.net. Użyj [wyszukiwać i zamieniać WordPress baz danych skryptu] [ searchandreplace] bezpiecznie zmienić wszystkie wystąpienia.

4. Tworzenie aplikacji sieci web w portalu Azure i publikowanie kopii zapasowej WordPress.

    1. Tworzenie aplikacji sieci web w [Azure portal] [ mgmtportal] z bazą danych przy użyciu **Nowy** -> **Web + Mobile** -> **Azure Marketplace** -> **Aplikacji Web Apps** -> **aplikacji Web app + SQL** (lub **aplikacji sieci Web + MySQL**) -> **Utwórz**. Konfigurowanie wszystkie wymagane ustawienia, aby utworzyć aplikację pusta sieć web.

    2. W kopii zapasowej WordPress zlokalizuj plik **wp config.php** , a następnie otwórz go w edytorze. Zamień następujące pozycje informacje dla nowej bazy danych MySQL.

        * **DB_NAME** - nazwa użytkownika bazy danych

        * **DB_USER** - nazwa użytkownika umożliwia dostęp do bazy danych

        * **DB_PASSWORD** - hasła użytkownika

        Po zmianie te wpisy, Zapisz i zamknij plik **wp config.php** .

    3. [Wdrażanie aplikacji sieci web w usłudze Azure aplikacji] za pomocą[ deploy] informacje, aby umożliwić metody wdrażania chcesz użyć, a następnie wdrożenie kopii zapasowej WordPress do aplikacji sieci web w usłudze Azure aplikacji.

5. Po wdrożeniu witryny WordPress, powinno być możliwe uzyskać dostęp do nowej witryny (jako aplikacji usługi aplikacji sieci web) za pomocą *. azurewebsite.net adres URL witryny.

### <a name="configure-your-site"></a>Konfigurowanie witryny

Po witrynie WordPress został utworzony lub migracji, aby zwiększyć wydajność lub włączyć dodatkowe funkcje za pomocą następujące informacje.

W tym celu... | Użyj tego ustawienia...
------------- | -----------
**Ustawianie trybu plan aplikacji usługi, rozmiar i Włącz skalowania** | [Zmienianie aplikacji sieci web w usłudze Azure aplikacji][websitescale]
**Włączanie połączenia z bazą danych trwałych** | Domyślnie WordPress nie korzysta z połączeń trwałych bazy danych, które mogą spowodować połączenie bazy danych są zamieniane na ograniczenie po wiele połączeń. Aby włączyć połączenia stałe, zainstaluj (wtyczkę karty połączenia stałe) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Zwiększanie wydajności** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Wyłączanie plików cookie ARR</a> — można poprawić wydajność, podczas uruchamiania WordPress na wielu wystąpień aplikacji sieci Web</p></li><li><p>Włącz buforowanie. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis pamięci podręcznej</a> (wersja preview) można używać z <a href="https://wordpress.org/plugins/redis-object-cache/">Redis dodatek WordPress pamięci podręcznej obiektów</a>lub użyj jednej z pamięci podręcznej ofert ze <a href="/gallery/store/">Sklepu Azure</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Jak przyspieszyć WordPress z Wincache</a> - Wincache jest domyślnie włączona w przypadku aplikacji sieci Web</p></li><li><p><a href="../web-sites-scale/">Skala aplikacji sieci web w usłudze Azure aplikacji</a> i używanie <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB wysoka dostępność routingu</a> lub <a href="http://www.mysql.com/products/cluster/">CGE klaster MySQL</a></p></li></ul>
**Używanie obiektów blob dla miejsca do magazynowania** | <ol><li><p><a href="../storage-create-storage-account/">Tworzenie konta magazynu platformy Azure</a></p></li><li><p>Dowiedz się, jak do <a href="../cdn-how-to-use/">użycia sieci dystrybucyjnej zawartości (CDN)</a> geo-rozpowszechnianie danych przechowywanych w obiektów blob.</p></li><li><p>Instalowanie i konfigurowanie <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure miejsca do magazynowania dla WordPress wtyczki</a>.</p><p>Szczegółowe ustawienia i informacje o wtyczkę konfiguracji zobacz <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">Podręcznik użytkownika</a>.</p> </li></ol>
**Włączanie poczty e-mail** | Włącz <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> przy użyciu magazynu Azure. Zainstaluj <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">wtyczkę SendGrid</a> dla WordPress.
**Konfigurowanie niestandardowej nazwy domeny** | [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji][customdomain]
**Włącz protokół HTTPS dla niestandardowej nazwy domeny** | [Włącz protokół HTTPS dla aplikacji sieci web w usłudze Azure aplikacji][httpscustomdomain]
**Ładowanie Saldo lub geo-rozpowszechnianie witryny** | [Skierować ruch przy użyciu Menedżera ruch Azure][trafficmanager]. Jeśli korzystasz z niestandardowej domeny, zobacz [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji] [ customdomain] informacji na temat korzystania z nazwami domen niestandardowych Menedżer ruchu
**Włączanie automatycznego wykonywania kopii zapasowych** | [Wykonywanie kopii zapasowej aplikacji sieci web w usłudze Azure aplikacji][backup]
**Włącz rejestrowanie diagnostyczne** | [Włącz rejestrowanie diagnostyczne dla aplikacji sieci web w usłudze Azure aplikacji][log]

## <a name="next-steps"></a>Następne kroki

* [Optymalizacja WordPress](http://codex.wordpress.org/WordPress_Optimization)

* [Konwertowanie WordPress wielooddziałowość w usłudze Azure aplikacji](web-sites-php-convert-wordpress-multisite.md)

* [Kreator dla Azure uaktualniania ClearDB](http://www.cleardb.com/store/azure/upgrade)

* [Hostingu WordPress w podfolderze aplikacji sieci web w usłudze Azure aplikacji](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Procedura: Tworzenie witryny WordPress przy użyciu platformy Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Host blogu WordPress istniejących Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Włączanie łatwa stałych w WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Jak przeprowadzić migrację i przeprowadzić blogu WordPress Azure aplikacji usługi](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Jak uruchomić WordPress bezpłatnie na Azure aplikacji usługi](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress Azure w dwóch minut lub szybciej](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Przenoszenie blogu WordPress Azure — część 1: tworzenie blogu WordPress Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Przenoszenie blogu WordPress Azure — część 2: przenoszenia zawartości](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Przenoszenie blogu WordPress Azure — część 3: Konfigurowanie domeny niestandardowej](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Przenoszenie blogu WordPress Azure - część 4: praktycznie stałych i adres URL edycji reguły](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Przenoszenie blogu WordPress Azure - część 5: przechodzenie z podfolderu w katalogu głównym](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Jak skonfigurować aplikację sieci web WordPress na koncie Azure](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping w górę WordPress Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Porady dotyczące WordPress Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
