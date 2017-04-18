<properties
    pageTitle="Historia wersji diagnostyki Azure"
    description="Wyjaśnienie zmian w różnych wersjach Azure diagnostyki jako wysłany z różnymi wersjami programu Microsoft Azure SDK."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Historia wersji diagnostyki platformy Microsoft Azure

Jesteś nowym użytkownikiem diagnostyki Azure? Zobacz [Omówienie diagnostyki Azure](azure-diagnostics.md).

W każdej wersji Azure SDK zwykle dostarczany z nowej wersji diagnostyki Azure. W poniższej tabeli opisano wersje Azure SDK i diagnostyce Azure skojarzone z wersji SDK.



Azure wersji SDK | Azure wersji narzędzia diagnostyczne | Model
--- | --- | ---
1.x      | 1.0 | wtyczki
2.0 do 2,4| 1.0 | "
2,5      | 1.2 | rozszerzenie
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1,5 | "


Najnowsza wersja jest 1,5, której dołączono 2,8 SDK Azure. Wersja rozszerzenia diagnostyki Azure, dostarczony wraz z zestawu SDK służy tylko do lokalnych emulatora scenariusze. Podczas pracy w Azure, niezależnie od tego, która wersja pakietu aplikacji wbudowana w dowolnej aplikacji automatycznie używa najnowszą wersję. Dopóki nie zostanie zainstalowana najnowsza SDK Azure, być może nie masz wszystkie narzędzia, które ułatwiają korzystanie z nowych funkcji.

Różne funkcje i zmiany opisane poniżej.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure 2,8 SDK dodane możliwość wysyłania danych diagnostyki [Wniosków aplikacji](./application-insights/app-insights-cloudservices.md) , co ułatwia diagnozowanie problemów w aplikacji, a także poziom system i infrastruktury.

## <a name="azure-26-diagnostics-changes"></a>Zmiany diagnostyki Azure 2.6

W przypadku usługi w chmurze 2.6 SDK Azure projektów w programie Visual Studio wprowadzono następujące zmiany. (Te zmiany także dotyczą nowszych wersjach systemu Azure SDK.)

- Lokalne emulatora obsługuje teraz diagnostyki. Oznacza to, czy zbieranie danych diagnostyki i upewnij się, że aplikacja tworzy prawo śledzenia podczas już opracowywania i testowania w programie Visual Studio. Parametry połączenia `UseDevelopmentStorage=true` umożliwia zbieranie danych diagnostyki, gdy korzystasz z chmury usługi projektu w programie Visual Studio przy użyciu emulatora Azure miejsca do magazynowania. Wszystkie dane diagnostyki są zbierane na koncie miejsca do magazynowania (opracowywania miejsca do magazynowania).

- Parametry połączenia konta diagnostyki przestrzeni dyskowej (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) znajduje się ponownie w pliku konfiguracji (.cscfg) usługi. W Azure SDK 2.5 konta miejsca do magazynowania diagnostyki określono w pliku diagnostics.wadcfgx.

Istnieje kilka godne uwagi różnic między jak parametry połączenia pracy w 2,4 SDK Azure lub nowszy i sposobu jej działania w Azure SDK 2.6 lub nowszym.

- W 2,4 SDK Azure i starszych parametry połączenia użyto jako środowisko uruchomieniowe przez wtyczkę diagnostyki Aby uzyskać informacje o koncie miejsca do magazynowania przesyłania dzienniki diagnostyczne.

- W Azure SDK 2.6 lub nowszym parametry połączenia diagnostyki jest używany przez program Visual Studio do konfigurowania rozszerzenia diagnostyki magazynowania odpowiednie informacje o koncie podczas publikowania. Parametry połączenia pozwala na zdefiniowanie magazynu innego konta dla konfiguracji innej usługi, używające programu Visual Studio podczas publikowania. Jednak ponieważ dodatek diagnostyki nie jest już dostępna (po Azure SDK 2.5), plik .cscfg przez siebie nie można włączyć rozszerzenia diagnostyki. Musisz włączyć rozszerzenie oddzielnie za pomocą narzędzi, takich jak Visual Studio lub programu PowerShell.

- Aby uprościć proces konfigurowania rozszerzenia diagnostyki przy użyciu programu PowerShell, dane wyjściowe pakietu Visual Studio zawiera publicznej konfiguracji XML rozszerzenia diagnostyki dla poszczególnych ról. Programu Visual Studio umożliwia parametry połączenia diagnostyki Wypełnij informacje o koncie miejsca do magazynowania, które są zawarte w konfiguracji publicznej. Pliki konfiguracji publicznej są tworzone w folderze rozszerzenia i postępuj zgodnie ze wzorcem PaaSDiagnostics. <RoleName>. PubConfig.xml. Wszelkie wdrożeń programu PowerShell podstawie można używać tego wzorca do zamapować każdej konfiguracji do roli.

- Parametry połączenia w pliku .cscfg jest również używane przez Azure portal uzyskiwać dostęp do danych diagnostyki, może być wyświetlany na karcie **monitorowania** . Parametry połączenia jest potrzebne do skonfigurowania usługi umożliwia wyświetlanie pełnej monitorowania danych w portalu.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrowanie projektów Azure SDK 2.6 i nowszych

Podczas migracji z Azure SDK 2,5 Azure SDK 2.6 lub nowszym, jeśli masz konto miejsca do magazynowania diagnostyki określone w pliku .wadcfgx, następnie pozostanie on. Aby skorzystać z elastyczności przy użyciu magazynu innego konta dla konfiguracji innego miejsca do magazynowania, musisz ręcznie dodać parametry połączenia do projektu. Jeśli przeprowadzasz migrację projektu z 2,4 SDK Azure lub starszym do Azure SDK 2.6, parametry połączenia diagnostyki są zachowywane. Pamiętaj, jednak zmiany w jak parametry połączenia są traktowane w Azure SDK 2.6 określone w poprzedniej sekcji.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Jak Visual Studio Określa konto miejsca do magazynowania diagnostyki

- Jeśli określono diagnostyki parametry połączenia w pliku .cscfg, Visual Studio używa go skonfigurować rozszerzenia diagnostyki podczas publikowania i generowanie plików xml publicznej konfiguracji podczas pakowania.

- Jeśli określono parametry połączenia nie diagnostyczne w pliku .cscfg, następnie Visual Studio przechodzi do przy użyciu konta miejsca do magazynowania, określonego w pliku .wadcfgx skonfigurować rozszerzenia diagnostyki podczas publikowania i generowanie pliki xml publicznej konfiguracji podczas pakowania.

- Narzędzia diagnostyczne parametry połączenia w pliku .cscfg ma pierwszeństwo przed konto miejsca do magazynowania w pliku .wadcfgx. Jeśli określono diagnostyki parametry połączenia w pliku .cscfg, Visual Studio korzysta z którego i ignoruje konta miejsca do magazynowania w .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Co oznacza "Aktualizacja opracowywania miejsca do magazynowania parametry połączenia..." pole wyboru zrobić?

Pole wyboru **Parametry połączenia aktualizacji opracowywania miejsca do magazynowania dla narzędzia diagnostyczne i buforowanie przy użyciu poświadczeń konta magazynu platformy Microsoft Azure podczas publikowania w programie Microsoft Azure** zapewnia wygodny sposób, aby zaktualizować wszystkie ciągi połączenia opracowywania miejsca do magazynowania konta za pomocą konta magazynu platformy Azure określony podczas publikowania.

Załóżmy na przykład, zaznacz to pole wyboru i określić parametry połączenia diagnostyki `UseDevelopmentStorage=true`. Podczas publikowania projektu Azure Visual Studio zostanie automatycznie zaktualizowany diagnostyki parametry połączenia z kontem miejsca do magazynowania w określonej w Kreatorze publikowania. Jednak jeśli konto rzeczywistą magazynowania określono jako parametry połączenia diagnostyki, następnie konta zamiast niej zostanie użyta.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Narzędzia diagnostyczne funkcji różnice między 2,4 SDK Azure i wcześniej i Azure SDK 2.5 lub nowszy

W przypadku uaktualniania projektu Azure SDK 2,4 2,5 SDK Azure lub nowszy, możesz należy pamiętać następujące różnice funkcji Diagnostyka.

- **Interfejsy API konfiguracji są przestarzałe** — programowych konfiguracji diagnostyki jest dostępny w 2,4 SDK Azure i starszych wersjach, ale jest przestarzałe w 2,5 SDK Azure lub nowszym. Jeśli konfiguracji diagnostyki jest aktualnie zdefiniowana w kodzie, musisz skonfigurować te ustawienia od podstaw migracją projektu w kolejności diagnostyki pakietu, aby kontynuować pracę. Plik konfiguracji diagnostyki Azure SDK 2,4 jest diagnostics.wadcfg i diagnostics.wadcfgx dla 2,5 SDK Azure lub w nowszej wersji.

- **Informacje diagnostyczne dla aplikacji usług w chmurze można skonfigurować tylko na poziomie roli, nie na poziomie obiektu.**

- **Za każdym razem, wdrażanie aplikacji, konfiguracji diagnostyki jest aktualizowana** — to może powodować problemy z spójności, jeśli zmienić konfigurację diagnostyki Eksploratora serwera, a następnie ponowne wdrożenie aplikacji.

- **W 2,5 SDK Azure i nowszym, awaria zrzuty są skonfigurowane w pliku konfiguracji diagnostyki nie w kodzie** — Jeśli masz skonfigurowane w kodzie zrzutów awaryjnych, musisz ręcznie przenieść konfiguracji od kodu do pliku konfiguracji, ponieważ zrzuty awaryjne nie są transferowane podczas migracji do Azure SDK 2.6.
