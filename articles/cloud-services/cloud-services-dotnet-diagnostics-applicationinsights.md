<properties
   pageTitle="Rozwiązywanie problemów z usługami w chmurze za pomocą aplikacji wniosków | Microsoft Azure"
   description="Dowiedz się, jak rozwiązywać problemy usługi cloud przy użyciu aplikacji wniosków przetworzyć danych od diagnostyki Azure."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Rozwiązywanie problemów z usługami w chmurze za pomocą aplikacji wniosków

Z rozszerzeniem diagnostyki [2,8 SDK Azure](https://azure.microsoft.com/downloads/) i Azure 1,5 możesz teraz wysłać danych diagnostyki Azure dla usługi w chmurze bezpośrednio do wniosków aplikacji. Różnych typów dzienników zgromadzone przez diagnostyki Azure Dzienniki aplikacji, dzienniki zdarzeń systemu windows, w tym dzienniki zdarzeń systemu Windows i liczników wydajności można teraz wysyłane do wniosków aplikacji i zwizualizować w portalu wniosków aplikacji interfejsu użytkownika. Stosowany razem z SDK wniosków aplikacji teraz można uzyskać spostrzeżeń metryki i dzienniki pochodzące z aplikacji, a także system i infrastruktury poziomu dane pochodzące z diagnostyki Azure.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Konfigurowanie diagnostyki Azure wysyłanie danych do aplikacji wniosków

Wykonaj poniższe czynności Konfigurowanie projektu usługi cloud wysyłanie diagnostyki Azure danych do aplikacji wnioski.

1) W oknie Eksplorator rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy rolę, a następnie wybierz pozycję **Właściwości** , aby otworzyć projektanta roli

![Właściwości roli Eksploratora rozwiązanie][1]

2) W Projektancie ról w sekcji narzędzia diagnostyczne zaznacz pole wyboru, aby **wysyłać dane Diagnostyka analizy aplikacji**

![Projektant roli Wyślij dane Diagnostyka analizy aplikacji][2]

3) W oknie dialogowym, które pojawia się wybierz zasób wniosków aplikacji, którą chcesz wysyłać dane diagnostyki Azure. Okno dialogowe umożliwia zaznacz istniejący zasób aplikacji wniosków z subskrypcji lub ręcznie określić klucz oprzyrządowania zasobu wniosków aplikacji. Jeśli nie masz istniejący zasób aplikacji wniosków następnie możesz utworzyć na klikając łącze **Utwórz nowy zasób** , które zostanie otwarte okno przeglądarki do portalu klasyczny Azure miejsce, w którym można utworzyć zasób wniosków aplikacji. Aby uzyskać więcej informacji dotyczących tworzenia zasobu wniosków aplikacji, zobacz [Tworzenie nowego zasobu wniosków aplikacji](../application-insights/app-insights-create-new-resource.md)

![Wybierz zasób wniosków aplikacji][3]

4) Po dodaniu zasobu wniosków aplikacji klucz oprzyrządowania dla tego zasobu jest przechowywana jako ustawienie konfiguracji usługi o nazwie **APPINSIGHTS_INSTRUMENTATIONKEY**. Taka konfiguracja dla każdego Konfiguracja usługi lub środowiska można zmienić, wybierając inną konfigurację z listy Konfiguracja usługi w dół i określanie nowy klucz oprzyrządowania dla tej konfiguracji.

![Wybierz pozycję Konfiguracja usługi][4]

Ustawienie konfiguracji **APPINSIGHTS_INSTRUMENTATIONKEY** jest używana przez program Visual Studio Aby skonfigurować rozszerzenia diagnostyki z odpowiednich aplikacji wniosków informacje o zasobie podczas publikowania. Ustawienie konfiguracji jest to wygodny sposób określania klawiszy oprzyrządowania różnych konfiguracji innej usługi. Program Visual Studio będzie tłumaczenie to ustawienie i wstawić go do konfiguracji publicznej rozszerzenia diagnostyki podczas publikowania. Aby uprościć proces konfigurowania rozszerzenia diagnostyki przy użyciu programu PowerShell, pakiet wyjście z programu Visual Studio również zawiera publicznej konfiguracji XML przy użyciu odpowiedniego oprzyrządowania wniosków aplikacji klucza dostępny. Pliki konfiguracji publicznej są tworzone w folderze rozszerzenia i postępuj zgodnie ze wzorcem PaaSDiagnostics. <RoleName>. PubConfig.xml. Wszelkie wdrożeń programu PowerShell podstawie można używać tego wzorca do zamapować każdej konfiguracji do roli.

5) Włączanie **Wyślij dane Diagnostyka analizy aplikacji** automatycznie skonfiguruje Azure Diagnostyka, aby wysłać wszystkie liczniki wydajności i poziomu dzienników błędów, które są zbierane przez agenta diagnostyki Azure analizy aplikacji. Jeśli chcesz kontynuować konfigurowanie, jakie dane są wysyłane do wniosków aplikacji, a następnie należy ręcznie edytować plik *diagnostics.wadcfgx* dla poszczególnych ról. Zobacz [Konfigurowanie diagnostyki Azure wysyłanie danych do wniosków aplikacji](../azure-diagnostics-configure-applicationinsights.md) , aby dowiedzieć się więcej o ręczne aktualizowanie konfiguracji.

Po skonfigurowaniu usługi w chmurze wysyłanie Azure diagnostyki danych do aplikacji wniosków, że można ją wdrożyć Azure tak jak zwykle upewnić się, że rozszerzenie Azure diagnostyki jest włączona. Zobacz [Publikowanie usługi w chmurze przy użyciu programu Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Wyświetlanie diagnostyki Azure danych w aplikacji wniosków
Azure telemetrycznego diagnostyczne będą widoczne w zasobie wniosków aplikacji skonfigurowane dla usługi w chmurze.

Poniżej przedstawiono, jak różne diagnostyki Azure logowania mapy typy pojęcia wniosków aplikacji:  

-  Liczniki wydajności są wyświetlane jako metryki niestandardowe w aplikacji wniosków
-  Dzienniki zdarzeń systemu Windows są wyświetlane jako śledzenia i zdarzeń niestandardowych w aplikacji wniosków
-  Dzienniki aplikacji, dzienniki zdarzeń systemu Windows i wszelkie dzienniki diagnostyczne infrastruktury są wyświetlane jako śledzenia w aplikacji wnioski.

Aby wyświetlić dane Azure diagnostyki w wniosków aplikacji:

- Za pomocą [Eksploratora metryki](../application-insights/app-insights-metrics-explorer.md) wizualizowanie liczniki wydajności niestandardowych ani liczby różnych rodzajów zdarzeń dziennika zdarzeń systemu windows.

![Niestandardowe metryki w Eksploratorze metryki][5]

- Aby przeszukać różnych dzienników wysyłane przez diagnostyki Azure za pomocą [wyszukiwania](../application-insights/app-insights-diagnostic-search.md) . Na przykład gdyby powodu nieobsługiwanego wyjątku w roli, które spowodowało roli, aby ulec awarii i Odtwórz te informacje będą widoczne w kanale *aplikacji* *dziennika zdarzeń systemu Windows*. Funkcje wyszukiwania umożliwia przeglądanie błędu dziennika zdarzeń systemu Windows i uzyskiwanie śledzenia pełnym stosie dla wyjątku, co umożliwia znajdowanie głównej przyczyny problemu.

![Wyszukiwanie śledzenia][6]

## <a name="next-steps"></a>Następne kroki

- [Dodaj SDK wniosków aplikacji do usługi w chmurze](../application-insights/app-insights-cloudservices.md) , aby wysyłać dane dotyczące żądania, wyjątki zależności i wszelkie niestandardowe telemetrycznego z aplikacji. Połączone z diagnostyki Azure danych zostanie wyświetlony kompletny obraz systemu i aplikacji w ten sam zasób wglądu aplikacji.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
